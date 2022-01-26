# AWS CLI

- **By default, the AWS CLI uses a page size of 1000** and retrieves all available items.
- You can use the `--page-size` option to specify that the AWS CLI request a smaller number of items from each call to the AWS service. The CLI still retrieves the full list but performs a larger number of service API calls in the background and retrieves a smaller number of items with each call. This gives the individual calls a better chance of succeeding without a timeout.

## AWS CLI Questions

- What is the default page size in the AWS CLI?
- What option can you use to modify the number of items from each call to the AWS service?

---

# ECS

> ECS is a highly scalable and fast container management service that makes it easy to run, stop, and manage containers on a **Cluster**.

### Container Concepts

#### Containers

A **container** is a standardized unit of software development that contains everything that your software application needs to run, including **relevant code, runtime, system tools, and system libraries**. Containers are created from a read-only template called an **image**.

#### Images

Images are typically built from a Dockerfile, which is a **plaintext file** that specifies all of the components that are included in the container.

#### Registry

After being built, these images are stored in a registry where they then can be downloaded and run on your cluster.

## ECS Concepts

ECS is composed of several core pieces, they include:

- **Clusters**

  > When you run tasks using ECS, you place them in a cluster, which is a logical grouping of resources.

- **Tasks**
  > A task is the instantiation of a task definition within a cluster. After you have created a task definition for your application, you can specify the number of tasks that will run on your cluster.
  - **Task Definition**
    > Task definitions specify various parameters for your application. It is a text file, in JSON format, that describes one or more containers, up to a maximum of ten, that form your application.
  - **Task Scheduler**
    > The task scheduler is responsible for placing tasks within your cluster.
- **Containers**
  > A container is a standardized unit of software development that contains everything that your software application needs to run, including relevant code, runtime, system tools, and system libraries. Containers are created from a read-only template called an image.
  - **Container Agent**
    > The container agent runs on each container instance within an Amazon ECS cluster. The agent sends information about the resource's current running tasks and resource utilization to Amazon ECS.

### Application Architecture

#### Fargate

**The Fargate launch type is good for the following workloads:**

- Large workloads that need to be optimized for low overhead
- Small workloads that have occasional burst
- Tiny workloads
- Batch workloads

When architecting your application to run on Amazon ECS using AWS Fargate, the main question is when should you put multiple containers into the same task definition versus deploying containers separately in multiple task definitions.

**When the following conditions are required, we recommend that you deploy your containers in a single task definition:**

- Your containers share a common lifecycle (that is, they are launched and terminated together).
  Your containers must run on the same underlying host (that is, one container references the other on a localhost port).
- You require that your containers share resources.
- Your containers share data volumes.

  Otherwise, you should define your containers in separate tasks definitions so that you can scale, provision, and deprovision them separately.

#### EC2 Launch Type

The EC2 launch type is good for large workloads that need to be optimized for price.

When you’re considering how to model task definitions and services using the EC2 launch type, it helps to think about what processes need to run together and how to scale each component.

**As an example, imagine an application that consists of the following components:**

- A frontend service that displays information on a webpage
- A backend service that provides APIs for the frontend service
- A data store

---

## Clusters

> When you run tasks using ECS, you place them in a cluster, which is a logical grouping of resources.

You can register one or more Amazon EC2 instances (also referred to as **container instances**) with your cluster to run tasks on them. Or, you can use the serverless infrastructure that Fargate provides to run tasks. When your tasks are run on Fargate, your cluster resources are also managed by Fargate.

- Infrastructure capacity can be provided by AWS Fargate (Serverless/Managed), EC2, on-prem, or even VMs that you manage remotely.
- A cluster may contain a mix of tasks hosted on AWS Fargate, Amazon EC2 instances, or external instances.
- Clusters are **region-specific**.
- **Before you can delete a cluster, you must delete the services and deregister the container instances inside that cluster.**
- Enabling managed Amazon ECS cluster auto scaling allows ECS to manage the scale-in and scale-out actions of the Auto Scaling group. On your behalf, Amazon ECS creates an AWS Auto Scaling scaling plan with a target tracking scaling policy based on the target capacity value that you specify.

## Task Components

### Tasks

> A **task** is the instantiation of a task definition within a cluster. After you have created a task definition for your application, you can specify the number of tasks that will run on your cluster.

After you have created a task definition for your application within Amazon ECS, you can specify the number of tasks to run on your cluster.

### Tasks Definitions

> Task definitions specify various parameters for your application. It is a text file, in JSON format, that describes one or more containers, up to a maximum of ten, that form your application.

- **Your entire application stack does not need to be on a single task definition, and in most cases it should not.** Your application can span multiple task definitions. You can do this by combining related containers into their own task definitions, each representing a single component.

**Task definitions are split into separate parts:**

- **Task family** – the name of the task, and each family can have multiple revisions.
- **IAM task role** – specifies the permissions that containers in the task should have.
- **Network mode** – determines how the networking is configured for your containers.
- **Container definitions** – specify which image to use, how much CPU and memory the container are allocated, and many more options.
- **Volumes** – allow you to share data between containers and even persist the data on the container instance when the containers are no longer running.
- **Task placement constraints** – lets you customize how your tasks are placed within the infrastructure.
- **Launch types** – determines which infrastructure your tasks use.

### Task Scheduler

> The task scheduler is responsible for placing tasks within your cluster.

- **REPLICA** — places and maintains the desired number of tasks across your cluster. By default, the service scheduler spreads tasks across Availability Zones. You can use task placement strategies and constraints to customize task placement decisions.
- **DAEMON** — deploys exactly one task on each active container instance that meets all of the task placement constraints that you specify in your cluster. When using this strategy, there is no need to specify a desired number of tasks, a task placement strategy, or use Service Auto Scaling policies.

- You can upload a new version of your application task definition, **and the ECS scheduler automatically starts new containers using the updated image and stop containers running the previous version.**

- **Amazon ECS tasks running on both Amazon EC2 and AWS Fargate can mount Amazon Elastic File System (EFS) file systems.**

## Container Agent

> The container agent runs on each container instance within an Amazon ECS cluster. The agent sends information about the resource's current running tasks and resource utilization to Amazon ECS.

- The Container Agent starts and stops tasks whenever it receives a request from Amazon ECS.

- Port mappings allow containers to access ports on the host container instance to send or receive traffic. Port mappings are specified as part of the container definition which can be configured in the task definition.
- **Service scheduler** this only provides you the ability to run tasks _manually_ (for batch jobs or single run tasks), with Amazon ECS placing tasks on your cluster for you. The service scheduler is ideally suited for long running stateless services and applications but not to configure port mappings.
- **Container instance** this is just an Amazon EC2 instance that is running the Amazon ECS container agent and has been registered into a cluster. When you run tasks with Amazon ECS, your tasks using the EC2 launch type are placed on your active container instances. However, you can’t manually configure the port mappings directly on your container instances but through task definitions.
- **Container Agent** this only allows container instances to connect to your cluster. The Amazon ECS container agent is included in the Amazon ECS-optimized AMIs, but you can also install it on any Amazon EC2 instance that supports the Amazon ECS specification. Same as the other incorrect options, you can’t configure port mappings with this component.

## Service Load Balancing

- Amazon ECS services support the Application Load Balancer, Network Load Balancer, and Classic Load Balancer ELBs. Application Load Balancers are used to route HTTP/HTTPS (or layer 7) traffic. Network Load Balancers are used to route TCP or UDP (or layer 4) traffic. Classic Load Balancers are used to route TCP traffic.

- The Classic Load Balancer doesn’t allow you to run multiple copies of a task on the same instance. You must statically map port numbers on a container instance. However, an Application Load Balancer uses **dynamic port mapping**, so you can run multiple tasks from a single service on the same container instance.

- Services with tasks that use the `awsvpc` network mode, such as those with the Fargate launch type, do not support Classic Load Balancers. **You must use NLB instead for TCP.**

## Deployment

### Task Placement Strategy

> **Task placement strategy** is an algorithm for selecting instances for task placement or tasks for termination.

- Amazon ECS supports the following task placement strategies:
  - **binpack**: tasks are placed on container instances so as to leave the least amount of unused CPU or memory. This strategy minimizes the number of container instances in use. When this strategy is used and a scale-in action is taken, Amazon ECS terminates tasks. It does this based on the amount of resources that are left on the container instance after the task is terminated. The container instance that has the most available resources left after task termination has that task terminated.
  - **random**: this will just place tasks on instances randomly.
  - **spread**: The spread strategy, contrary to the `binpack` strategy, tries to put your tasks on as many different instances as possible. It is typically used to achieve high availability and mitigate risks, by making sure that you don’t put all your task-eggs in the same instance-baskets. Spread across Availability Zones, therefore, is the default placement strategy used for services. **When using the spread strategy, you must also indicate a field parameter.** It is used to indicate the bins that you are considering. The accepted values are `instanceID`, `host`, or a custom attribute key:value pairs such as `attribute:ecs.availability-zone` to balance tasks across zones. There are several AWS attributes that start with the ecs prefix, but you can be creative and create your own attributes.

## ECS Questions

- What is a Cluster?
- Are Clusters region specific?
- What must you do before you delete a Cluster?
- What is a Task?
- What is a Task Definition?
- How many containers can be defined by a single class definition?
- Is it a good idea, or normal, to have an entire application task based on one task definition?
- What does a Task Scheduler do?
- Can Tasks running on EC2 and Fargate mount EFS?
- What are ECS port mappings used for?
- What is the Service Scheduler?
- What is the Container Instance?
- What is the Container Agent?
- What is an ECS task placement strategy?
- What are the three ECS task placement strategies?
- Describe the binpack task placement strategy.
- Describe the random task placement strategy.
- Describe the spread task placement strategy.

---

# S3

- Amazon S3 Transfer Acceleration enables fast, easy, and secure transfers of files over long distances between your client and your Amazon S3 bucket. **Transfer Acceleration leverages Amazon CloudFront’s globally distributed AWS Edge Locations. As data arrives at an AWS Edge Location, data is routed to your Amazon S3 bucket over an optimized network path.**

---

# Elastic Beanstalk

### Deployment

- Depending on the platform version you’d like to update to, Elastic Beanstalk recommends one of two methods for performing platform updates.

  - **Update your Environment’s Platform Version** – This is the recommended method when you’re updating to the latest platform version, _without a change in runtime, web server, or application server versions, and without a change in the major platform version._ This is the most common and routine platform update.
  - **Perform a Blue/Green Deployment** – This is the recommended method when you’re updating to a different runtime, web server, or application server versions, or to a different major platform version. _This is a good approach when you want to take advantage of new runtime capabilities_ or the latest Elastic Beanstalk functionality.

- This service is not suitable for deploying _serverless applications_. In addition, it doesn’t have the capability to locally build, test, and debug your serverless applications as effectively as what AWS SAM can do.

- **Blue/green deployments require that your environment runs independently of your production database if your application uses one.** If your environment has an Amazon RDS DB instance attached to it, the data will not transfer over to your second environment and **will be lost** if you terminate the original environment.

### Deployment Options

> In ElasticBeanstalk, you can choose from a variety of deployment methods:

- **All at once** – Deploy the new version to all instances simultaneously. All instances in your environment are out of service for a short time while the deployment occurs. It is possible that there would be a degradation of the service since some instances would be unavailable during the deployment process. _All at once has the shortest deployment time of all the methods._

- **Rolling** – Deploy the new version in batches. Each batch is taken out of service during the deployment phase, reducing your environment’s capacity by the number of instances in a batch. This method will deploy the new version in batches only to existing instances, without provisioning new resources. The compute capacity of the environment would still be compromised in this method.

- **Rolling with additional batch** – Starts by deploying your application code to a single batch of newly created EC2 instances. Once the deployment succeeds on the first batch of instances, the application code is deployed to the remaining instances in batches until the last batch of instances remains. At this point, the last batch of instances is terminated. This deployment policy ensures that the impact of a failed deployment is limited to a single batch of instances and enables your application to serve traffic at full capacity during an ongoing deployment.

- **Immutable** – Deploy the new version to a fresh group of instances by performing an immutable update. To perform an immutable environment update, Elastic Beanstalk creates a second, temporary Auto Scaling group behind your environment’s load balancer to contain the new instances. Immutable deployments can prevent issues caused by partially completed rolling deployments. If the new instances don’t pass health checks, Elastic Beanstalk terminates them, leaving the original instances untouched. If an immutable environment update fails, the rollback process requires only terminating an Auto Scaling group. A failed rolling update, on the other hand, requires performing an additional rolling update to roll back the changes.

- **Traffic splitting** – Deploy the new version to a fresh group of instances and temporarily split incoming client traffic between the existing application version and the new one.

## Elastic Beanstalk Questions

- What is the recommended method when you’re updating to a different runtime, web server, or application server versions, or to a different major platform version?
- Is Elastic Beanstalk suitable for deploying serverless applications?
- Can Elastic Beanstalk build, test, and debug, serverless applications as well as AWS SAM can?
- What are the **five** deployment options for Elastic Beanstalk?
- Describe the **All at once** deployment option.
- Describe the **Rolling** deployment option.
- Describe the **Rolling with additional batch** deployment option.
- Describe the **Immutable** deployment option.
- Describe the **Traffic Splitting** deployment option.

---

# CodeDeploy

- CodeDeploy can deploy application content that runs on a server and is stored in Amazon S3 buckets, GitHub repositories, or Bitbucket repositories.

### Deployment

#### CodeDeploy Agent

- The CodeDeploy agent is a software package that, when installed and configured on an instance, makes it possible for that instance to be used in CodeDeploy deployments. **The CodeDeploy agent communicates outbound using HTTPS over port 443.**

- It is also important to note that the CodeDeploy agent is _required only if you deploy to an EC2/On-Premises compute platform._ **It is not required for ECS or Lambda deployments.**

#### In-Place Deployment

- **Only deployments that use the EC2/On-Premises compute platform can use in-place deployments.**
- The application on each instance in the _deployment group_ is stopped, the latest application revision is installed, and the new version of the application is started and validated.
- AWS Lambda compute platform deployments _cannot_ use an in-place deployment type.
- ECS deployments _cannot_ use an in-place deployment type.
- **For Lambda and ECS, you can only do a blue/green deployment in CodeDeploy.**

#### Blue/Green Deployment

- The behavior of your deployment depends on which compute platform you use:

  – **Blue/green on an EC2/On-Premises compute platform:** The instances in a _deployment group_ (the original environment) are replaced by a different set of instances (the replacement environment). _If you use an EC2/On-Premises compute platform, be aware that blue/green deployments work with Amazon **EC2 instances only.**_

  - **Blue/green on an AWS Lambda compute platform:** Traffic is shifted from your current serverless environment to one with your updated Lambda function versions. You can specify Lambda functions that perform validation tests and choose the way in which the traffic shift occurs. **All AWS Lambda compute platform deployments are blue/green deployments. For this reason, you do not need to specify a deployment type.**

  - **Blue/green on an Amazon ECS compute platform:** Traffic is shifted from the task set with the original version of a containerized application in an Amazon ECS service to a replacement task set in the same service. The protocol and port of a specified load balancer listener are used to reroute production traffic. _During deployment, a test listener can be used to serve traffic to the replacement task set while validation tests are run._

## CodeDeploy Questions

- What is a CodeDeploy agent?
- What protocol does the CodeDeploy agent use for outbound communication.
- What compute platforms is the CodeDeploy agent **required** for?
- Describe the in-place deployment process.
- What are the only compute types that can use in-place deployment?
- Can AWS Lambda use in-place deployments?
- Describe the Blue/Green deployment process for EC2/On-Prem.
- Describe the Blue/Green deployment process for Lambda.
- Describe the Blue/Green deployment process for ECS.
- Can Blue/Green deployments work with both EC2 and On-prem compute platforms?
- Do you need to specify a deployment type for Lambda functions?
- For ECS deployments, what purpose can using a **test listener** provide?

---

# Lambda

- To create a Lambda function, **you need a deployment package and an execution role.**

  - **deployment package** contains your function code.
  - The **execution role** grants the function permission to use AWS services, such as Amazon CloudWatch Logs for log streaming and AWS X-Ray for request tracing.

- A function has an **unpublished version**, and can have **published versions** and **aliases**.

  - The **unpublished version** changes when you update your function’s code and configuration.
  - A **published version** is a snapshot of your function code and configuration that can’t be changed.
  - An **alias** is a named resource that maps to a version, _and can be changed to map to a different version._

- You can use the CreateFunction API _via the AWS CLI or the AWS SDK_ of your choice.
- **Lambda Alias** is just like a pointer to a specific Lambda function version.
- **Execution Context** is a temporary runtime environment that initializes any external dependencies of your Lambda function code, such as database connections or HTTP endpoints.

- To create a Lambda function, you first create a Lambda function deployment package, **a `.zip` or `.jar` file** consisting of your code and any dependencies. **When creating the zip, include only the code and its dependencies, not the containing folder.** You will then need to set the appropriate security permissions for the zip package.

- In **Lambda non-proxy (or custom) integration**, you can specify how the incoming request data is mapped to the integration request and how the resulting integration response data is mapped to the method response.

#### Errors

- The `InvalidParameterValueException` will be returned if one of the parameters in the request is invalid. For example, if you provided an IAM role in the CreateFunction API which AWS Lambda is unable to assume.
- If you have exceeded your maximum total code size per account, the `CodeStorageExceededException` will be returned.
- If the resource already exists, the `ResourceConflictException` will be returned.
- If the AWS Lambda service encountered an internal error, the `ServiceException` will be returned.

#### Layers

- You can configure your Lambda function to pull in additional code and content in the form of layers.
- A layer is a ZIP archive that contains libraries, a custom runtime, or other dependencies. With layers, you can use libraries in your function without needing to include them in your **deployment package**.
- Layers help you keep your deployment package small, which makes development easier. You can avoid errors that can occur when you install and package dependencies with your function code.
- For _Node.js, Python, and Ruby functions_, you can develop your function code in the Lambda console **as long as you keep your deployment package under 3 MB**.
- **A function can use up to 5 layers at a time.** The total unzipped size of the function and **all layers can’t exceed the unzipped deployment package size limit of 250 MB.**
- You can create layers, or use layers published by AWS and other AWS customers.
- Layers are extracted to the `/opt` directory in the function execution environment. Each runtime looks for libraries in a different location under `/opt`, depending on the language. Structure your layer so that function code can access libraries without additional configuration.

### Deployment

- **Canary**: In a Canary deployment configuration, the traffic is shifted in two increments. You can choose from predefined canary options that specify the percentage of traffic shifted to your updated Lambda function version in the first increment and the interval, in minutes, before the remaining traffic is shifted in the second increment.
- **Linear**: This will cause the traffic to be shifted in equal increments with an equal number of minutes between each increment. You can choose from predefined linear options that specify the percentage of traffic shifted in each increment and the number of minutes between each increment.
- **All-at-once** With this deployment configuration, the traffic is shifted from the original Lambda function to the updated Lambda function version all at once.

## Lambda Questions

- What two file types are suitable for the lambda function deployment package?
- What _two_ things are required when you create a lambda functions?
- what does the **deployment package** provide?
- What does the **execution role** provide?
- A lambda can come in two versions, what are they?
- Describe a **published version** of a lambda function.
- Describe a **unpublished version** of a lambda function.
- Describe a lambda function **alias**.
- What is the Execution Context in a lambda function and what use cases can it provide?
- Describe the `InvalidParameterValueException` error.
- Describe the `CodeStorageExceededException` error.
- Describe the `ResourceConflictException` error.
- Describe the `ServiceException` error.
- What are lambda layers?
- Layers help keep supporting code out of what?
- What languages can you develop your function code in within the lambda console, what is max size for the deployment package?
- How many layers can a lambda function have at a time?
- What is the max unzipped deployment package size for a lambda function and all of its layers?
- Are you able to create layers and also use layers published by AWS and AWS Customers?
- What directory are layers extracted from in the function execution environment?
- What three types of lambda deployments are there?
- Describe lambda **canary** deployments.
- Describe lambda **linear** deployments.
- Describe lambda **all-at-once** deployments.

---

# EBS

- You can detach an Amazon EBS volume from an instance explicitly or by terminating the instance. _However, if the instance is running, you must first unmount the volume from the instance._
- If an EBS volume is the root device of an instance, you must stop the instance before you can detach the volume.
- After you attach an Amazon EBS volume to your instance, it is exposed as a block device. **You can format the volume with any file system and then mount it.** After you make the EBS volume available for use, you can access it in the same ways that you access any other volume. Any data written to this file system is written to the EBS volume and is transparent to applications using the device.
- **New volumes are raw block devices and do not contain any partition or file system.** You need to login to the instance and then format the EBS volume with a file system, and then mount the volume for it to be usable.
- Volumes that have been restored from snapshots likely have a file system on them already; if you create a new file system on top of an existing file system, the operation overwrites your data.
- Use the `sudo file -s device` command to list down the information about your volume, such as file system type.
- **There is _no way_ on the AWS console to assign a file system on an EBS volume.**
- By default, there is no pre-configured file system in EBS. AWS does not do this for you.

### EBS Questions

- What must you do to an EBS volume if it's being used as the root device and you want to detach it?
- When you attach an EBS volume to your instance as a block device, does it come with the file system already setup?
- Do new volumes contain a partition or file system?
- What command can you run to see the information of your volume?
- Is there a way to use the AWS file system to assign a file system to your EBS volume?

---

# CloudFormation

- A Cloudformation template is a JSON- or YAML-formatted text file that describes your AWS infrastructure.

- When you specify a transform, **you can use AWS SAM syntax to declare resources in your template.** The model defines the syntax that you can use and how it is processed. More specifically, the `AWS::Serverless` transform, **which is a macro hosted by AWS CloudFormation**, takes an entire template written in the AWS Serverless Application Model (AWS SAM) syntax and transforms and expands it into a compliant AWS CloudFormation template.

- `Transform` section specifies the version of the AWS Serverless Application Model (AWS SAM) to use. This is used for serverless applications (also referred to as Lambda-based applications).
- `Mappings` section just lists a mapping of keys and associated values that you can use to specify conditional parameter values, similar to a lookup table.
- `Resources` section is primarily used to specify the stack resources and their properties.
- `Parameter` section is just the part of the template that contains values to pass to your template at runtime when you create or update a stack.

### Deployment

#### Lambda

- For **NodeJS** and **Python** functions, you can specify the function code inline in the template. The `ZipFile` parameter will allow the developer to place the python/nodeJS code inline in the template.
- If you are using a CloudFormation template, you can configure the `AWS::Lambda::Function` resource which creates a Lambda function. To create a function, you need a deployment package and an execution role.
- **Uploading the code in S3 then specifying the `S3Key` and `S3Bucket` parameters under the `AWS::Lambda::Function` resource in the CloudFormation template is a valid deployment step**, though you still have to upload the code in S3 instead of just including the function source inline in the ZipFile parameter.
- **Changes to a deployment package in Amazon S3 are not detected automatically during stack updates. To update the function code, change the object key or version in the template.**
- Uploading the code in S3 as a _ZIP file_ then specifying the _S3 path_ in the `Code: ZipFile` parameter of the `AWS::Lambda::Function` resource in the CloudFormation template is incorrect because contrary to its name, **the `ZipFile` parameter directly accepts the source code of your Lambda function and not an actual zip file.** If you include your function source inline with this parameter, AWS CloudFormation places it in a file named index and zips it to create a deployment package.

#### StackSets

- StackSets extend the functionality of stacks, using a single AWS CloudFormation template, by enabling you to create, update, or delete stacks **across multiple accounts and regions with a single operation.**
- Remember that **a stack set is a regional resource** so if you create a stack set in one region, you cannot see it or change it in other regions.
- _Using an administrator account_, you define and manage an AWS CloudFormation template, and use the template as the basis for provisioning stacks into selected target accounts across specified regions.
- CloudFormation doesn’t have the capability to locally build, test, and debug your application like what AWS SAM has.

#### Change Sets

- Change Sets only allow you to _preview_ how proposed changes to a stack might impact your running resources.

#### Stack Instance

- A stack instance is simply a reference to a stack in a target account within a region. Remember that a stack instance is associated with one stack set which is why this is just one of the components of CloudFormation StackSets.

## CloudFormation Questions

- What language templates does the CloudFormation script use?
- What CloudFormation section allows you to use AWS SAM syntax to declare resources in your template?
- The AWS SAM `Transform` macro in CloudFormation does what with the SAM syntax?
- Describe the usage of the CloudFormation `Transform` section.
- Describe the usage of the CloudFormation `Mappings` section.
- Describe the usage of the CloudFormation `Resources` section.
- Describe the usage of the CloudFormation `Parameter` section.
- For Lambda deployments using CloudFormation, you can specify the function code inline in the template with what two languages?
- What Cloudformation parameter do you use to paste lambda code directly into a CloudFormation script?
- The `ZipFile` parameter is within what CloudFormation section?
- Can you store your code in S3 and reference it through CloudFormation?
- Are changes to a lambda deployment package, stored in S3, automatically detected during CloudFormation stack updates?
- Is creating a zip file, storing it in S3, and then providing that S3 file path back to the CloudFormation template's `ZipFile` parameter a suitable approach for the `ZipFile` parameter?
- What is a StackSet?
- Do StackSets enable you to create, update, and delete stacks across multiple accounts and regions.
- Are stack sets regional?
- Does CloudFormation have the ability to locally build, test, and debug your application?
- What is a **Change Set**?
- What is a **Stack Instance**?

---

# AWS SAM (Serverless Application Model)

- **AWS SAM is an open-source framework that you can use to build serverless applications on AWS.** It consists of the AWS SAM template specification that you use to define your serverless applications, and the AWS SAM command line interface (AWS SAM CLI) that you use to build, test, and deploy your serverless applications.
- Because **AWS SAM is an extension of AWS CloudFormation**, you get the reliable deployment capabilities of AWS CloudFormation.
- You can use AWS SAM with a suite of AWS tools for building serverless applications. **The AWS SAM CLI lets you locally build, test, and debug serverless applications that are defined by AWS SAM templates.**
- **The CLI provides a Lambda-like execution environment _locally_.** It helps you catch issues upfront by providing parity with the actual Lambda execution environment.
- To step through and debug your code to understand what the code is doing, you can use AWS SAM with AWS toolkits like the AWS Toolkit for JetBrains, AWS Toolkit for PyCharm, AWS Toolkit for IntelliJ, and AWS Toolkit for Visual Studio Code. This tightens the feedback loop by making it possible for you to find and troubleshoot issues that you might run into in the cloud.
- After you develop and test your serverless application locally, you can deploy your application by using the `sam package` and `sam deploy` commands.
  - The `sam package` command zips your code artifacts, uploads them to Amazon S3, and produces a packaged AWS SAM template file that’s ready to be used.
  - The `sam deploy` command uses this file to deploy your application.
  - To deploy an application that contains one or more nested applications, you must include the `CAPABILITY_AUTO_EXPAND` capability in the `sam deploy` command.
  - `sam publish` publishes an AWS SAM application to the AWS Serverless Application Repository and does not generate the template file.

## AWS SAM Questions

- What does SAM stand for?
- What is AWS SAM?
- What service is AWS SAM an extension of?
- Does the AWS SAM CLI let you locally build, test, and debug serverless applications that are defined by AWS SAM templates?
- What does the AWS SAM CLI provide locally?
- What does the `sam package` command do?
- What does the `sam deploy` command do?
- What does the `sam publish` command do?
- If you're trying to deploy an application that contains one or more nested applications, what capability do you need to include in the `sam deploy` command?

---

# X-Ray

- **AWS X-Ray** helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture. With X-Ray, you can understand how your application and its underlying services are performing to identify and troubleshoot the root cause of performance issues and errors. _X-Ray provides an end-to-end view of requests as they travel through your application_, and shows a map of your application’s underlying components.
- You can use X-Ray to analyze both applications in development and in production, from simple three-tier applications to complex microservices applications consisting of thousands of services.
- AWS X-Ray works with Amazon EC2, Amazon EC2 Container Service (Amazon ECS), AWS Lambda, and AWS Elastic Beanstalk.

- A segment can break down the data about the work done into **subsegments**. Subsegments provide more granular timing information and details about downstream calls that your application made to fulfill the original request. A subsegment can contain additional details about a call to an AWS service, an external HTTP API, or an SQL database. You can even define arbitrary subsegments to instrument specific functions or lines of code in your application.

- For services that don’t send their own segments like Amazon DynamoDB, X-Ray uses subsegments to generate **_inferred_ segments** and downstream nodes on the service map. This lets you see all of your downstream dependencies, even if they don’t support tracing, or are external.

- Subsegments represent your application’s view of a downstream call as a client. If the downstream service is also instrumented, the segment that it sends replaces the inferred segment generated from the upstream client’s subsegment. The node on the service graph always uses information from the service’s segment, if it’s available, while the edge between the two nodes uses the upstream service’s subsegment.

---

# DynamoDB

#### Read Units (RCU)

- One read request unit represents **one strongly consistent read request**, or **two eventually consistent read requests**, for an item **up to 4 KB** in size.
- **Transactional read requests require 2 read request units to perform one read** for items up to 4 KB.
- If you need to read an item that is larger than 4 KB, DynamoDB needs additional read request units.
- **Item sizes for reads are rounded up to the next 4 KB multiple.** For example, reading a 3,500-byte item consumes the same throughput as reading a 4 KB item.
- RCU Breakdowns:
  - eventually consistent requests: .5 RCU per request. 150 x .5 = 75.
  - strongly consistent requests: 1 RCU per request. 150 x 1 = 150.
  - transactional read requests: 2 RCU per request. 150 x 2 = 300.

## DynamoDB questions

- One read request unit represents how many _strongly consistent_ read requests?
- One read request unit represents how many _eventually consistent_ read requests?
- How many reads does a _transactional read request_ require to perform one read?
- How large can a **read request** unit be in size?
- If an item is larger than 4 KB in size results in what from DynamoDB?

---

# EC2

## Security

---

# Security

- If you have resources that are running inside AWS, that need programmatic access to various AWS services,then the best practice is to always use IAM roles. **However, for applications running outside of an AWS environment, these will need access keys for programmatic access to AWS resources.** For example, monitoring tools running on-premises and third-party automation tools will need access keys.
- In order to use the AWS SDK for your application, you have to create your credentials file first at` ~/.aws/credentials` for Linux servers or at `C:\Users\USER_NAME\.aws\credentials` for Windows users and then save your access keys. Go to the AWS Console and create a new IAM user with programmatic access. In the application server, create the credentials file at `~/.aws/credentials` with the access keys of the IAM user.
- **You cannot directly assign an IAM Role to a server on your on-premises data center.** Although it may be possible to use a combination of STS and IAM Role, the use of access keys for AWS SDK is still preferred especially if the application server is on-premises.

## Security Questions

- If applications are running outside of an AWS environment, what will they need to access AWS resources?
- Where are your access keys saved on a Linux machine?
- Where are your access keys saved on a Windows machine?
- Can you assign an IAM role to a server on your on-prem data center?
- What is the preferred access solution for on-prem application servers?

---

## AWS KMS | Encryption

- A**WS KMS uses customer master keys (CMKs) to encrypt your Amazon S3 objects.**
- The first time you add an SSE-KMS–encrypted object to a bucket in a region, a default CMK is created for you automatically. This key is used for SSE-KMS encryption unless you select a CMK that you created separately using AWS Key Management Service.
- To upload an object to the S3 bucket which uses SSE-KMS, you have to send a request with an `x-amz-server-side-encryption` header with the value of `aws:kms`. There’s also an optional `x-amz-server-side-encryption-aws-kms-key-id` header which specifies the ID of the AWS KMS master encryption key that was used for the object. The Amazon S3 API also supports encryption context, with the `x-amz-server-side-encryption-context` header.
- **When you upload an object, Amazon S3 uses the encryption key you provide to apply `AES-256` encryption to your data** and removes the encryption key from memory.
- **S3 does not store the encryption key you provide.** Instead, it is stored in a randomly salted `HMAC` value of the encryption key in order to validate future requests. **The salted `HMAC` value cannot be used to derive the value of the encryption key or to decrypt the contents of the encrypted object. That means, if you lose the encryption key, you lose the object.**
- **The salted `HMAC` is just used to validate future encryption requests.** It cannot be used to derive the value of the encryption key or to decrypt the contents of the encrypted object. You cannot use the salted `HMAC` value to decrypt the object.
- **An S3 bucket that is configured to use Server-Side Encryption with Customer-Provided Encryption Keys (SSE-C). In this configuration, Amazon S3 does not store the encryption key you provide but instead, stores a randomly salted _hash-based message authentication code_ (`HMAC`) value of the encryption key in order to validate future requests.**

## AWS KMS | Encryption Questions

- What does AWS KMS use to encrypt your S3 objects?
- What encryption solution is used by AWS to encrypt your data?
- Where does AWS store the provided encryption key?
- What does HMAC stand for?
- Can the salted `HMAC` value be used to derive the value of the encryption key or decrypt the contents of the encrypted object?
- What is the salted `HMAC` value used for?
- An S3 bucket using SSE-C stores what value to validate future requests?

# API Gateway

### API integration request

- An integration request is an HTTP request that API Gateway submits to the backend, passing along the client-submitted request data, and transforming the data, if necessary. The HTTP method (or verb) and URI of the integration request are dictated by the backend (that is, the integration endpoint). They can be the same as or different from the method request's HTTP method and URI, respectively.

For example, when a Lambda function returns a file that is fetched from Amazon S3, you can expose this operation intuitively as a GET method request to the client even though the corresponding integration request requires that a POST request be used to invoke the Lambda function. For an HTTP endpoint, it is likely that the method request and the corresponding integration request both use the same HTTP verb. However, this is not required. You can integrate the following method request:

---

# SWF

- **Markers** are used to record events in the workflow execution history for application specific purposes. Markers are useful when you want to record custom information to help implement decider logic. For example, you could use a marker to count the number of loops in a recursive workflow.
- **Signals** enable you to inject information into a running workflow execution.
- **Timers** enable you to notify your decider when a certain amount of time has elapsed.
- **Tags** enable you to filter the listing of the executions when you use the visibility operations.

---
