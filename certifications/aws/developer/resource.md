# ECS

### Task Placement Strategy

> **Task placement strategy** is an algorithm for selecting instances for task placement or tasks for termination.

- Amazon ECS supports the following task placement strategies:
  - **binpack**: place tasks based on the _least_ available amount of CPU or memory. This minimizes the number of instances in use.
  - **random**: place tasks randomly.
  - **spread**: place tasks evenly based on the specified value. _Accepted values are attribute key-value pairs, instanceId, or host._

## ECS Questions

- What is an ECS task placement strategy?
- What are the three ECS task placement strategies?
- Describe the binpack task placement strategy.
- Describe the random task placement strategy.
- Describe the spread task placement strategy.

---

# Elastic Beanstalk

### Deployment

- This service is not suitable for deploying _serverless applications_. In addition, it doesn’t have the capability to locally build, test, and debug your serverless applications as effectively as what AWS SAM can do.

### Deployment Options

> In ElasticBeanstalk, you can choose from a variety of deployment methods:

- **All at once** – Deploy the new version to all instances simultaneously. All instances in your environment are out of service for a short time while the deployment occurs. It is possible that there would be a degradation of the service since some instances would be unavailable during the deployment process. _All at once has the shortest deployment time of all the methods._

- **Rolling** – Deploy the new version in batches. Each batch is taken out of service during the deployment phase, reducing your environment’s capacity by the number of instances in a batch. This method will deploy the new version in batches only to existing instances, without provisioning new resources. The compute capacity of the environment would still be compromised in this method.

- **Rolling with additional batch** – Deploy the new version in batches, but first launch a new batch of instances to ensure full capacity during the deployment process.

- **Immutable** – Deploy the new version to a fresh group of instances by performing an immutable update. To perform an immutable environment update, Elastic Beanstalk creates a second, temporary Auto Scaling group behind your environment’s load balancer to contain the new instances. First, Elastic Beanstalk launches a single instance with the new configuration in the new group. This instance serves traffic alongside all of the instances in the original Auto Scaling group that are running the previous configuration.

- **Blue/Green** – Deploy the new version to a separate environment, and then swap CNAMEs of the two environments to redirect traffic to the new version instantly.

## Elastic Beanstalk Questions

- Is Elastic Beanstalk suitable for deploying serverless applications?
- Can Elastic Beanstalk build, test, and debug, serverless applications as well as AWS SAM can?
- What are the **five** deployment options for Elastic Beanstalk?
- Describe the **All at once** deployment option.
- Describe the **Rolling** deployment option.
- Describe the **Rolling with additional batch** deployment option.
- Describe the **Immutable** deployment option.
- Describe the **Blue/Green** deployment option.

---

# CodeDeploy

- CodeDeploy can deploy application content that runs on a server and is stored in Amazon S3 buckets, GitHub repositories, or Bitbucket repositories.

### Deployment

#### CodeDeploy Agent

- The CodeDeploy agent is a software package that, when installed and configured on an instance, makes it possible for that instance to be used in CodeDeploy deployments. **The CodeDeploy agent communicates outbound using HTTPS over port 443.**

- It is also important to note that the CodeDeploy agent is _required only if you deploy to an EC2/On-Premises compute platform._ **It is not required for ECS or Lambda deployments.**

#### In-Place Deployment

- The application on each instance in the deployment group is stopped, the latest application revision is installed, and the new version of the application is started and validated.
- **Only deployments that use the EC2/On-Premises compute platform can use in-place deployments.**
- AWS Lambda compute platform deployments _cannot_ use an in-place deployment type.

#### Blue/Green Deployment

- The behavior of your deployment depends on which compute platform you use:

  – **Blue/green on an EC2/On-Premises compute platform:** The instances in a deployment group (the original environment) are replaced by a different set of instances (the replacement environment). _If you use an EC2/On-Premises compute platform, be aware that blue/green deployments work with Amazon **EC2 instances only.**_

  - **Blue/green on an AWS Lambda compute platform:** Traffic is shifted from your current serverless environment to one with your updated Lambda function versions. You can specify Lambda functions that perform validation tests and choose the way in which the traffic shift occurs. _All AWS Lambda compute platform deployments are blue/green deployments. For this reason, you do not need to specify a deployment type._

  - **Blue/green on an Amazon ECS compute platform:** Traffic is shifted from the task set with the original version of a containerized application in an Amazon ECS service to a replacement task set in the same service. The protocol and port of a specified load balancer listener are used to reroute production traffic. _During deployment, a test listener can be used to serve traffic to the replacement task set while validation tests are run._

## CodeDeploy Questions

- What is a CodeDeploy agent?
- What protocol does the CodeDeploy agent use for outbound communicate.
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

---

# CloudFormation

### Deployment

#### StackSets

- Stack sets extend the functionality of stacks, using a single AWS CloudFormation template, by enabling you to create, update, or delete stacks _across multiple accounts and regions with a single operation._
- Remember that **a stack set is a regional resource** so if you create a stack set in one region, you cannot see it or change it in other regions.
- _Using an administrator account_, you define and manage an AWS CloudFormation template, and use the template as the basis for provisioning stacks into selected target accounts across specified regions.
- CloudFormation doesn’t have the capability to locally build, test, and debug your application like what AWS SAM has.

#### Change Sets

- Change Sets only allow you to _preview_ how proposed changes to a stack might impact your running resources.

#### Stack Instance

- A stack instance is simply a reference to a stack in a target account within a region. Remember that a stack instance is associated with one stack set which is why this is just one of the components of CloudFormation StackSets.

## CloudFormation Questions

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

## AWS SAM Questions

- What does SAM stand for?
- What is AWS SAM?
- What service is AWS SAM an extension of?
- Does the AWS SAM CLI let you locally build, test, and debug serverless applications that are defined by AWS SAM templates?
- What does the AWS SAM CLI provide locally?
