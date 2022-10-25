# SWF

Amazon **_Simple Workflow Service_** (AWS SWF) makes it easy to **build applications that coordinate work across distributed components.** In SWF, **a _task_ represents a logical unit of work that is performed by a component of your application.**

Coordinating tasks across the application involves managing intertask dependencies, scheduling, and concurrency in accordance with the logical flow of the application. **Amazon SWF gives you full control over implementing tasks and coordinating them without worrying about underlying complexities such as tracking their progress and maintaining their state.**

When using Amazon SWF, **you implement workers to perform tasks.** These **workers can run either on cloud infrastructure**, such as Amazon Elastic Compute Cloud (Amazon EC2), or on your own premises.

**Amazon SWF stores tasks and assigns them to workers when they are ready, tracks their progress, and maintains their state, including details on their completion.**

**To coordinate tasks, you write a program that gets the latest state of each task from Amazon SWF and uses it to initiate subsequent tasks.** **Amazon SWF maintains an application's execution state durably** so that the application is resilient to failures in individual components. With Amazon SWF, you can implement, deploy, scale, and modify these application components independently.

Amazon SWF offers capabilities to support a variety of application requirements. **It is suitable for a range of use cases that require coordination of tasks, including media processing, web application back-ends, business process workflows, and analytics pipelines.**

### Questions

- What does SWF stand for?
- What goal / purpose does SWF serve?
- What is a task in SWF?
- Coordinating tasks across distributed applications introduces what types of challenges?
- SWF simplifies what distributed application challenges?
- What are SWF workers, can they run on-prem or on the cloud?
- AWS manages what parts of the SWF task flow?
- How do you coordinate tasks with SWF?
- Does AWS maintain an application's execution state durably?
- What sort of use cases is AWS SWF suitable for?

---

_A growing number of applications are relying on asynchronous and distributed processing._ The scalability of such applications is the primary motivation for using this approach. By designing autonomous distributed components, developers have the flexibility to deploy and scale out parts of the application independently if the load on the application increases. Another motivation is the availability of cloud services. As application developers start taking advantage of cloud computing, they have a need to combine their existing on-premises assets with additional cloud-based assets. Yet another motivation for the asynchronous and distributed approach is the inherent distributed nature of the process being modeled by the application; for example, the automation of an order-fulfillment business process may span several systems and human tasks.

Developing such applications can be complicated. It requires that you coordinate the execution of multiple distributed components and deal with the increased latencies and unreliability inherent in remote communication. To accomplish this, you would typically need to write complicated infrastructure involving message queues and databases, along with the complex logic to synchronize them.

The Amazon Simple Workflow Service (Amazon SWF) makes it easier to develop asynchronous and distributed applications by providing a programming model and infrastructure for coordinating distributed components and maintaining their execution state in a reliable way. By relying on Amazon SWF, you are freed to focus on building the aspects of your application that differentiate it.

- The fundamental concept in Amazon SWF is the **_workflow_**. **A workflow is a set of activities that carry out some objective, together with logic that coordinates the activities.** For example, a workflow could receive a customer order and take whatever actions are necessary to fulfill it.

- **Each workflow runs in an AWS resource called a _domain_**, which controls the workflow's scope. **An AWS account can have multiple domains, each of which can contain multiple workflows, but workflows in different domains can't interact.**

- **When designing an Amazon SWF workflow, you precisely define each of the required _activities_. You then register each activity with Amazon SWF as an activity type.** **When you register the activity, you provide information such as a name and version, and some timeout values based on how long you expect the activity to take.** For example, a customer may have an expectation that an order will ship within 24 hours. Such expectations would inform the timeout values that you specify when registering your activities.

- **In the process of carrying out the workflow, some activities may need to be performed more than once, perhaps with varying inputs.** For example, in a customer-order workflow, you might have an **activity** that handles purchased items. _If the customer purchases multiple items, then this activity would have to run multiple times_. Amazon SWF has the concept of an _**activity task**_ that represents one invocation of an activity. In our example, **the processing of each item would be represented by a single activity task.**

- **An _activity worker_ is a program that receives activity tasks, performs them, and provides results back.** Note that the task itself might actually be performed by a person, in which case the person would use the activity worker software for the receipt and disposition of the task.

- **Activity tasks —-and the activity workers that perform them -- can run synchronously or asynchronously**. **They can be distributed across multiple computers, potentially in different geographic regions, or they can all run on the same computer. Different activity workers can be written in different programming languages and run on different operating systems.**

**The coordination logic in a workflow is contained in a software program called a _decider_.** **The decider schedules activity tasks, provides input data to the activity workers, processes events that arrive while the workflow is in progress, and ultimately ends (or closes) the workflow when the objective has been completed.**

**The role of the Amazon SWF service is to function as a reliable central hub through which data is exchanged between the decider, the activity workers, and other relevant entities** such as the person administering the workflow. **Amazon SWF also maintains the state of each workflow execution, which saves your application from having to store the state in a durable way.**

**The _decider_ directs the workflow by receiving _decision tasks_ from SWF and responding back to SWF with decisions.** **A _decision_ represents an action or set of actions which are the next steps in the workflow.** A typical decision would be to schedule an activity task. Decisions can also be used to set timers to delay the execution of an activity task, to request cancellation of activity tasks already in progress, and to complete or close the workflow.

The mechanism by which both the _activity workers_ and the _decider_ receive their tasks _(activity tasks and decision tasks respectively)_ **is by polling the Amazon SWF service.**

**Amazon SWF informs the decider of the state of the workflow by including, with each decision task, a copy of the current workflow execution history.** **The workflow execution history is composed of events, where an _event_ represents a significant change in the state of the workflow execution.**

Examples of events would be the: completion of a task, notification that a task has timed out, or the expiration of a timer that was set earlier in the workflow execution. The history is a complete, consistent, and authoritative record of the workflow's progress.

**Amazon SWF access control uses AWS Identity and Access Management (IAM)**, which allows you to provide access to AWS resources in a controlled and limited way that doesn't expose your access keys. **For example, you can allow a user to access your account, but only to run certain workflows in a particular domain.**

#### hierarchy

```
// the decider and SWF determine the decisions -- or action or set of actions which are the next steps in the workflow.
SWF -decision tasks-> decider
_then_
decider -decisions-> SWF

( (activity tasks >+ activity worker) >+ activities >+ workflows >+ domain )
```

### Questions

1. What is a _workflow_?
1. What is a _domain_?
1. Can an AWS account have multiple domains?
1. Can domains have multiple workflows?
1. Can workflows communicate across domains?
1. What do you define in a workflow?
1. With what do you register activities?
1. What information do you need to provide for registering activities?
1. What is an _activity task_?
1. What is an _activity worker_?
1. Do activity tasks, and the workers that run them, support synchronous and asynchronous workloads?
1. Do activity workers _need_ to be written in the same language?
1. Can activity tasks be distributed across multiple computers, potentially in different regions?
1. What is a _decider_ and what does it do?
1. What is the role of the SWF service throughout the process?
1. Where is the state of each workflow execution stored?
1. Which role(?) _directs the workflow_ after receiving decisions tasks from SWF?
1. What is a _decision_?
1. What are some examples of a _decision_?
1. What does SWF include, with each decision task, to inform the decider of the state of the workflow?
1. What is the _workflow execution history_ composed of?
1. What is an _event_?
1. What are examples of _events_?
1. Does SWF use IAM? What's an example of IAM usage within SWF?

### Answers

1. A **workflow** is a set of activities that carry out some objective, together with logic that coordinates the activities.
1. Each workflow runs in an AWS resource called a **domain**, which controls the workflow's scope. Think of a domain as a wrapper that encapsulates a collection of workflows.
1. Yes, An AWS account can have multiple domains.
1. A domain _can_ contain multiple workflows.
1. Workflows _cannot_ communicate across domains. Domains encapsulate and isolate their children workflows.
1. When designing an SWF workflow, **you need to define each _activity_ therein.** Then you register each activity with SWF as an _activity type_.
1. When registering an activity, you need to provide the **name and version, and timeout values** for how long you expect the activity to take.
1. If a customer purchases multiple items, then this activity would have to run multiple times. Amazon SWF has the concept of an _activity task_ **that represents one invocation of an activity.**
1. An _activity worker_ is a program that receives _activity tasks_, performs them, and provides results back.
1. Activity tasks, and their workers respectively, can be synchronous or asynchronous.
1. Activity workers **do not** need to be in the same language.
1. Activity tasks _can_ be distributed across multiple computers in multiple regions.
1. The _decider_ directs the workflow by receiving _decision tasks_ from SWF and responding back to SWF with decisions.
1. The role of the Amazon SWF service is to function as a reliable central hub through which data is exchanged between the decider, the activity workers, and other relevant entities.
1. The state of each workflow is stored in SWF for easier management.
1. The decider directs the workflow after receiving decision tasks from SWF.
1. A _decision_ represents an action or set of actions which are the next steps in the workflow.
1. A typical decision would be to schedule an activity task. Decisions can also be used to set timers to delay the execution of an activity task, to request cancellation of activity tasks already in progress, and to complete or close the workflow.
1. Amazon SWF informs the decider of the state of the workflow by including, with each decision task, a copy of the current workflow execution history.
1. The workflow execution history is composed of events, where an _event_ represents a significant change in the state of the workflow execution.
1. An _event_ represents a significant change in the state of the workflow execution.
1. Examples of events would be the: completion of a task, notification that a task has timed out, or the expiration of a timer that was set earlier in the workflow execution.
1. Amazon SWF access control uses AWS Identity and Access Management (IAM), which allows you to provide access to AWS resources in a controlled and limited way that doesn't expose your access keys. For example, you can allow a user to access your account, but only to run certain workflows in a particular domain.

## Workflow Execution

> Bringing together the ideas discussed in the preceding sections, here is an overview of the steps to develop and run a workflow in Amazon SWF:

1. Write activity workers that implement the processing steps in your workflow.

2. Write a decider to implement the coordination logic of your workflow.

3. Register your activities and workflow with Amazon SWF.

> You can do this step programmatically or by using the AWS Management Console.

4. Start your activity workers and decider.

> These actors can run on any computing device that can access an Amazon SWF endpoint. For example, you could use compute instances in the cloud, such as Amazon Elastic Compute Cloud (Amazon EC2); servers in your data center; or even a mobile device, to host a decider or activity worker. Once started, the decider and activity workers should start polling Amazon SWF for tasks.

5. Start one or more executions of your workflow.

> Executions can be initiated either programmatically or via the AWS Management Console.

> Each execution runs independently and you can provide each with its own set of input data. When an execution is started, Amazon SWF schedules the initial decision task. In response, your decider begins generating decisions which initiate activity tasks. Execution continues until your decider makes a decision to close the execution.

6. View workflow executions using the AWS Management Console.

> You can filter and view complete details of running as well as completed executions. For example, you can select an open execution to see which tasks have completed and what their results were.
