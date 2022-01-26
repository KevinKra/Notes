# AWS Developer Associate

> Guide for the Developer Associate Exam.

## Index

a. [DynamoDB](#DynamoDB)

b. [ECS](#ECS)

c. [Elastic Beanstalk](#Elastic-Beanstalk)

d. [CodeDeploy](#CodeDeploy)

e. [Lambda](#Lambda)

f. [CloudFormation](#CloudFormation)

g. [AWS-SAM](#AWS-SAM)

# DynamoDB <a name="DynamoDB"></a>

## Core Components

In DynamoDB, tables, items, and attributes are the core components that you work with. A table is a collection of items, and each item is a collection of attributes. DynamoDB uses primary keys to uniquely identify each item in a table and secondary indexes to provide more querying flexibility. You can use DynamoDB Streams to capture data modification events in DynamoDB tables.

### Tables, Items, and Attributes

- **Tables** – Similar to other database systems, DynamoDB stores data in tables. A table is a collection of data.

- **Items** – Each table contains zero or more items. An item is a group of attributes that is uniquely identifiable among all of the other items.

- **Attributes** – Each item is composed of one or more attributes. An attribute is a fundamental data element, something that does not need to be broken down any further.

Each item in the table has a unique identifier, or **primary key**, that distinguishes the item from all of the others in the table.

Other than the primary key, the `People` table is schemaless, which means that neither the attributes nor their data types need to be defined beforehand. Each item can have its own distinct attributes.

DynamoDB supports nested attributes up to **32 levels deep**.

The primary key for Music consists of two attributes (Artist and SongTitle). **Each item in the table must have these two attributes.** The combination of Artist and SongTitle distinguishes each item in the table from all of the others.

## Primary Key

When you create a table, in addition to the table name, you must specify the primary key of the table. The primary key uniquely identifies each item in the table, so that no two items can have the same key.

### Partition Key

- **Partition key** – A simple primary key, composed of one attribute known as the partition key.

DynamoDB uses the partition key's value as input to an internal hash function. The output from the hash function determines the partition (physical storage internal to DynamoDB) in which the item will be stored.

### Partition Key and Sort Key

- **Partition key and sort key** – Referred to as a composite primary key, this type of key is composed of two attributes. The first attribute is the partition key, and the second attribute is the sort key.

DynamoDB uses the partition key value as input to an internal hash function. The output from the hash function determines the partition (physical storage internal to DynamoDB) in which the item will be stored. All items with the same partition key value are stored together, in sorted order by sort key value.

In a table that has a partition key and a sort key, it's possible for multiple items to have the same partition key value. However, those items must have different sort key values.

A composite primary key gives you additional flexibility when querying data. For example, if you provide only the value for `Artist`, DynamoDB retrieves all of the songs by that artist. To retrieve only a subset of songs by a particular artist, you can provide a value for `Artist` along with a range of values for `SongTitle`.

> The partition key of an item is also known as its **hash attribute**. The term hash attribute derives from the use of an internal hash function in DynamoDB that evenly distributes data items across partitions, based on their partition key values.

> The sort key of an item is also known as its **range attribute**. The term range attribute derives from the way DynamoDB stores items with the same partition key physically close together, in sorted order by the sort key value.

Each primary key attribute must be a scalar (meaning that it can hold only a single value). The only data types allowed for primary key attributes are string, number, or binary. There are no such restrictions for other, non-key attributes.

## Secondary Indexes

You can create one or more secondary indexes on a table.

A secondary index lets you query the data in the table using an alternate key, in addition to queries against the primary key.

DynamoDB doesn't require that you use indexes, but they give your applications more flexibility when querying your data.

When you create a secondary index, DynamoDB actually creates a _new table_ that is sorted with your partition key and the new sort key, serving as the composite/primary key for the table.

Secondary Indexes are created at table creation time. You cannot create a secondary index after the table has been created.

#### DynamoDB supports two kinds of indexes:

- **Global secondary index** – An index with a partition key and sort key that can be different from those on the table.
- **Local secondary index** – An index that has the same partition key as the table, but a different sort key.

Each table in DynamoDB has a quota of **20 global secondary indexes (default quota) and 5 local secondary indexes.**

In the example `Music` table shown previously, you can query data items by `Artist` (partition key) or by `Artist` and `SongTitle` (partition key and sort key). What if you also wanted to query the data by `Genre` and `AlbumTitle`? To do this, you could create an index on `Genre` and `AlbumTitle`, and then query the index in much the same way as you'd query the `Music` table.

- Every index belongs to a table, which is called the _base table_ for the index. In the preceding example, `Music` is the base table for the GenreAlbumTitle index.

- DynamoDB maintains indexes automatically. When you add, update, or delete an item in the base table, DynamoDB adds, updates, or deletes the corresponding item in any indexes that belong to that table.

- When you create an index, you specify which attributes will be copied, or projected, from the base table to the index. At a minimum, DynamoDB projects the key attributes from the base table into the index. This is the case with `GenreAlbumTitle`, where only the key attributes from the `Music` table are projected into the index.

## DynamoDB Streams

DynamoDB Streams is an **_optional_** feature that captures data modification events in DynamoDB tables. **The data about these events appear in the stream in near-real time, and in the order that the events occurred.**

Each event is represented by a **_stream record_**. If you enable a stream on a table, DynamoDB Streams writes a stream record whenever one of the following events occurs:

- **A new item is added to the table:** The stream captures an image of the entire item, including all of its attributes.
- **An item is updated:** The stream captures the _"before"_ and _"after"_ image of any attributes that were modified in the item.
- **An item is deleted from the table:** The stream captures an image of the entire item before it was deleted.

Each stream record also contains the name of the table, the event timestamp, and other metadata. **Stream records have a lifetime of 24 hours;** after that, they are automatically removed from the stream.

**You can use DynamoDB Streams together with AWS Lambda to create a trigger—code that runs automatically whenever an event of interest appears in a stream.**

In addition to triggers, DynamoDB Streams enables powerful solutions such as data replication within and across AWS Regions, materialized views of data in DynamoDB tables, data analysis using Kinesis materialized views, and much more.

## DynamoDB API

### Control Plane

_Control plane_ operations let you create and manage DynamoDB tables. They also let you work with indexes, streams, and other objects that are dependent on tables.

- `CreateTable`
- `DescribeTable`
- `ListTables`
- `UpdateTable`
- `DeleteTable`

### Data Plane

Data plane operations let you perform create, read, update, and delete (also called CRUD) actions on data in a table. Some of the data plane operations also let you read data from a secondary index.

You can use **PartiQL** - A SQL-Compatible Query Language for Amazon DynamoDB, to perform these CRUD operations or you can use DynamoDB’s classic CRUD APIs that separates each operation into a distinct API call.

#### Creating Data

- `PutItem`: Writes a single item from a table.
- `BatchWriteItem`: **Writes up to 25 items to a table.** This is more efficient than calling `PutItem` multiple times because your application only needs a single network round trip to write the items. _You can also use BatchWriteItem for deleting multiple items from one or more tables._

#### Reading Data

- `GetItem`: Retrieves a single item from a table.
- `BatchGetItem`: **Retrieves up to 100 items from one or more tables.** This is more efficient than calling GetItem multiple times because your application only needs a single network round trip to read the items.
- `Query`: Retrieves all items that have a specific partition key.
- `Scan`: Retrieves all items in the specified table or index.

#### Updating Data

- `UpdateItem`: Modifies one or more attributes in an item. You must specify the primary key for the item that you want to modify. You can add new attributes and modify or remove existing attributes.

#### Deleting Data

- `DeleteItem`: Deletes a single item from a table.
- `BatchWriteItem`: **Deletes up to 25 items from one or more tables.** This is more efficient than calling DeleteItem multiple times because your application only needs a single network round trip to delete the items. _You can also use BatchWriteItem for adding multiple items to one or more tables._

### DynamoDB Streams

DynamoDB Streams operations let you enable or disable a stream on a table, and allow access to the data modification records contained in a stream.

- `ListStreams`: Returns a list of all your streams, _or_ just the stream for a specific table.
- `DescribeStreams`: Returns information about a stream, such as its Amazon Resource Name (ARN) and where your application can begin reading the first few stream records.
- `GetShardIterator`: Returns a _shard iterator_, which is a data structure that your application uses to retrieve the records from the stream.
- `GetRecords`: Retrieves one or more stream records, using a given shard iterator.

## Data Types

### Scalar Types

- `Number`
- `String`
- `Binary`: Binary type attributes can store any binary data, such as compressed text, encrypted data, or images. Whenever DynamoDB compares binary values, it treats each byte of the binary data as unsigned.
- `Boolean`
- `Null`

### Document Types

The document types are `list` and `map`. **These data types can be nested within each other**, to represent complex data structures **up to 32 levels deep.**

There is no limit on the number of values in a list or a map, as long as the item containing the **values fits within the DynamoDB item size limit (400 KB).**

#### List

A list type attribute can store an ordered collection of values. Lists are enclosed in square brackets: `[ ... ]` A list is similar to a JSON array.

- `FavoriteThings: ["Cookies", "Coffee", 3.14159]`

#### Map

A map type attribute can store an unordered collection of name-value pairs. Maps are enclosed in curly braces: `{ ... }` A map is similar to a JSON object.

- Maps are ideal for storing JSON documents in DynamoDB.

#### Sets

DynamoDB supports types that represent sets of **number, string, or binary values**. **All the elements within a set must be of the same type.**

There is no limit on the number of values in a set, as long as the item containing the values fits within the DynamoDB item size limit (400 KB).

**Each value within a set must be unique.** **The order of the values within a set is not preserved.** Therefore, your applications must not rely on any particular order of elements within the set. DynamoDB does not support empty sets, however, empty string and binary values are allowed within a set.

- `["Black", "Green", "Red"]`

- `[42.2, -19, 7.5, 3.14]`

- `["U3Vubnk=", "UmFpbnk=", "U25vd3k="]`

## Read Consistency

Amazon DynamoDB is available in multiple AWS Regions around the world. **Tables in each Region are independent and isolated from tables other AWS Regions.** For example, if you have a table called `People` in the `us-east-2` Region and another table named `People` in the `us-west-2` Region, these are considered two entirely separate tables.

When your application writes data to a DynamoDB table and receives an **HTTP 200 response (OK)**, the write has occurred and is durable. The data is eventually consistent across all storage locations, usually within one second or less.

### Eventually Consistent Read

When you read data from a DynamoDB table, the response might not reflect the results of a recently completed write operation. The response might include some stale data. If you repeat your read request after a short time, the response should return the latest data.

### Strongly Consistent Read

When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful. However, this consistency comes with some disadvantages:

- A strongly consistent read might not be available if there is a network delay or outage. In this case, DynamoDB may return a server error (HTTP 500).
- Strongly consistent reads may have higher latency than eventually consistent reads.
- **Strongly consistent reads are not supported on global secondary indexes.**
- Strongly consistent reads use more throughput capacity than eventually consistent reads.

## Read/Write Capacity Mode

Amazon DynamoDB has two read/write capacity modes for processing reads and writes on your tables:

- **On-demand**
- **Provisioned (default, free-tier eligible)**

The read/write capacity mode controls how you are charged for read and write throughput and how you manage capacity. You can set the read/write capacity mode when creating a table or you can change it later.

**Secondary indexes inherit the read/write capacity mode from the base table.**

### On-Demand Mode

Amazon DynamoDB on-demand is a flexible billing option capable of serving thousands of requests per second without capacity planning. DynamoDB on-demand offers pay-per-request pricing for read and write requests so that you pay only for what you use.

**When you choose on-demand mode, DynamoDB instantly accommodates your workloads as they ramp up or down to any previously reached traffic level.** If a workload’s traffic level hits a new peak, DynamoDB adapts rapidly to accommodate the workload. Tables that use on-demand mode deliver the same single-digit millisecond latency, service-level agreement (SLA) commitment, and security that DynamoDB already offers.

**On-demand mode is a good option if any of the following are true:**

- You create new tables with unknown workloads.
- You have unpredictable application traffic.
- You prefer the ease of paying for only what you use.

You can switch between read/write capacity modes **once every 24 hours.**

#### Read Request Units

- One read request unit represents one strongly consistent read request, or two eventually consistent read requests, for an item up to 4 KB in size.
- Two read request units represent one transactional read for items up to 4 KB.
- If you need to read an item that is larger than 4 KB, DynamoDB needs additional read request units.

> The total number of read request units required depends on the item size, and whether you want an eventually consistent or strongly consistent read. For example, if your item size is 8 KB, you require 2 read request units to sustain one strongly consistent read, 1 read request unit if you choose eventually consistent reads, or 4 read request units for a transactional read request.

#### Write Request Units

- One write request unit represents one write for an item up to 1 KB in size.
- If you need to write an item that is larger than 1 KB, DynamoDB needs to consume additional write request units.
- Transactional write requests require 2 write request units to perform one write for items up to 1 KB.

> The total number of write request units required depends on the item size. For example, if your item size is 2 KB, you require 2 write request units to sustain one write request or 4 write request units for a transactional write request.

### Peak Traffic and Scaling Properties

DynamoDB tables using on-demand capacity mode automatically adapt to your application’s traffic volume. **On-demand capacity mode instantly accommodates up to double the previous peak traffic on a table.** For example, if your application’s traffic pattern varies between 25,000 and 50,000 strongly consistent reads per second where 50,000 reads per second is the previous traffic peak, on-demand capacity mode instantly accommodates sustained traffic of up to 100,000 reads per second.

If you need more than double your previous peak on table, DynamoDB automatically allocates more capacity as your traffic volume increases to help ensure that your workload does not experience throttling. **However, throttling can occur if you exceed double your previous peak within 30 minutes.**

If you recently switched an existing table to on-demand capacity mode for the first time, or if you created a new table with on-demand capacity mode enabled, the table has the following previous peak settings, even though the table has not served traffic previously using on-demand capacity mode.

When you switch a table from provisioned capacity mode to on-demand capacity mode, DynamoDB makes several changes to the structure of your table and partitions. This process can take several minutes. **During the switching period, your table delivers throughput that is consistent with the previously provisioned write capacity unit and read capacity unit amounts.**

### Provisioned Mode

If you choose provisioned mode, you specify the number of reads and writes per second that you require for your application. You can use auto-scaling to adjust your table’s provisioned capacity automatically in response to traffic changes. This helps you govern your DynamoDB use to stay at or below a defined request rate in order to obtain cost predictability.

**Provisioned mode is a good option if any of the following are true:**

- You have predictable application traffic.
- You run applications whose traffic is consistent or ramps gradually.
- You can forecast capacity requirements to control costs.

#### RCU and WCU

If your application reads or writes larger items (up to the DynamoDB _maximum item size_ of 400 KB), it will consume more capacity units.

For example, suppose that you create a provisioned table with 6 read capacity units and 6 write capacity units. With these settings, your application could do the following:

- Perform strongly consistent reads of up to 24 KB per second (4 KB × 6 read capacity units).
- Perform eventually consistent reads of up to 48 KB per second (twice as much read throughput).
- Perform transactional read requests of up to 12 KB per second.
- Write up to 6 KB per second (1 KB × 6 write capacity units).
- Perform transactional write requests of up to 3 KB per second.

### Throttling

_Provisioned throughput_ is the maximum amount of capacity that an application can consume from a table or index. If your application exceeds your provisioned throughput capacity on a table or index, it is subject to request throttling.

**Throttling prevents your application from consuming too many capacity units.** When a request is throttled, **it fails with an HTTP 400 code (Bad Request)** and a `ProvisionedThroughputExceededException`. **The AWS SDKs have built-in support for retrying throttled requests (see Error Retries and Exponential Backoff)**, so you do not need to write this logic yourself.

### DynamoDB Auto Scaling

**DynamoDB auto scaling actively manages throughput capacity for tables and global secondary indexes.** With auto-scaling, you define a range (upper and lower limits) for read and write capacity units. **You also define a target utilization percentage within that range.**

**With DynamoDB auto-scaling, a table or a global secondary index can increase its provisioned read and write capacity to handle sudden increases in traffic, without request throttling.** When the workload decreases, DynamoDB auto scaling can decrease the throughput so that you don't pay for unused provisioned capacity.

### Reserved Capacity

As a DynamoDB customer, you can purchase reserved capacity in advance for tables that use the DynamoDB Standard table class, as described at Amazon DynamoDB Pricing.

**With reserved capacity, you pay a one-time upfront fee and commit to a minimum provisioned usage level over a period of time.**

**Your reserved capacity is billed at the hourly reserved capacity rate.** By reserving your read and write capacity units ahead of time, you realize significant cost savings on your provisioned capacity costs. Any capacity that you provision in excess of your reserved capacity is billed at standard provisioned capacity rates.

Reserved capacity is not available for replicated write capacity units. **Reserved capacity is also not available for tables using the DynamoDB Standard-IA table class or on-demand capacity mode.**

## Table Classes

DynamoDB offers two table classes designed to help you optimize for cost.

- **The DynamoDB Standard table class is the default, and is recommended for the vast majority of workloads.**

- **The DynamoDB Standard-Infrequent Access (DynamoDB Standard-IA) table class is optimized for tables where storage is the dominant cost.** For example, tables that store infrequently accessed data, such as application logs, old social media posts, e-commerce order history, and past gaming achievements, are good candidates for the Standard-IA table class.

Every DynamoDB table is associated with a table class (DynamoDB Standard by default).

**The choice of a table class is not permanent** -—you can change this setting using the AWS Management Console, AWS CLI, or AWS SDK. DynamoDB also supports managing your table class using AWS CloudFormation for single-Region tables (tables that are not global tables).

## Partitions and Data Distributions

**Amazon DynamoDB stores data in partitions.** **A partition is an allocation of storage for a table, backed by solid state drives (SSDs) and automatically replicated across multiple Availability Zones within an AWS Region.** Partition management is handled entirely by DynamoDB —-you never have to manage partitions yourself.

When you create a table, the initial status of the table is `CREATING`. During this phase, DynamoDB allocates sufficient partitions to the table so that it can handle your provisioned throughput requirements. You can begin writing and reading table data after the table status changes to `ACTIVE`.

**DynamoDB allocates additional partitions to a table in the following situations:**

- If you increase the table's provisioned throughput settings beyond what the existing partitions can support.
- If an existing partition fills to capacity and more storage space is required.

Global secondary indexes in DynamoDB are also composed of partitions. The data in a global secondary index is stored separately from the data in its base table, but index partitions behave in much the same way as table partitions.

### Data Distribution: Partition Key

To write an item to the table, DynamoDB uses the value of the partition key as input to an internal hash function. The output value from the hash function determines the partition in which the item will be stored.

> DynamoDB is optimized for uniform distribution of items across a table's partitions, no matter how many partitions there may be. We recommend that you choose a partition key that can have a large number of distinct values relative to the number of items in the table.

### Data Distribution: Partition Key and Sort Key

If the table has a composite primary key (partition key and sort key), DynamoDB calculates the hash value of the partition key in the same way as described in Data Distribution: Partition Key. However, it stores all the items with the same partition key value physically close together, ordered by sort key value.

To write an item to the table, DynamoDB calculates the hash value of the partition key to determine which partition should contain the item. In that partition, several items could have the same partition key value. So DynamoDB stores the item among the others with the same partition key, in ascending order by sort key.

You can read multiple items from the table in a single operation (`Query`) if the items you want have the same partition key value. DynamoDB returns all of the items with that partition key value. **Optionally, you can apply a condition to the sort key so that it returns only the items within a certain range of values.**

To read all of the items with an `AnimalType` of _Dog_, you can issue a Query operation without specifying a sort key condition. By default, the items are returned in the order that they are stored (that is, in ascending order by sort key). Optionally, you can request descending order instead.

## DynamoDB Questions

- DynamoDB's data structures are divided into three what levels?
- A row in DynamoDB is called what?
- A column in DynamoDB is called what?
- How many layers nested can an attribute be?
- What is a primary key?
- What is a sort key?
- What is a partition key?
- What is a composite key?
- How does DynamoDB use the partition key?
- Beyond the primary key, do other table attributes need to be defined beforehand and share the same attribute types?
- In a table that has a partition key _and_ sort key, can multiple items have the same partition key value?
- An item's partition key is also called it's _what_ attribute.
- An item's sort key is also called it's _what_ attribute.
- Each primary key must be comprised of what types of value?
- What happens when you declare a secondary index for your table?
- Can you create a secondary index for your table after the table has been already created?
- What is the difference between a global secondary index and a local secondary index?
- What is the default quota limit for **global secondary indexes** on a table?
- What is the default quota limit for **local secondary indexes** on a table?
- Every index belongs to a table, what is this table called?
- When creating an index, which table elements need to be projected from the base table to the index?
- At a minimum, DynamoDB projects which attributes from the base table to the index?
- Are DynamoDB streams required or optional?
- Do DynamoDB stream events appear in the order they occurred?
- Each event on a stream is represented by what?
- For a table with a stream, what happens when a new item is added to the table?
- For a table with a stream, what happens when an item is updated?
- For a table with a stream, what happens when a new item is deleted from the table?
- Each stream record contains what core pieces of data?
- What is the lifespan of a stream record?
- Streams can be used together with what service to build event-driven architecture when events of interest appear in a stream?
- Can streams help enable data replication in, and across, AWS regions?
- Can streams be used for data analysis and producing materialized views with tools like Kineses?
- What is the ControlPlane?
- What is the DataPlane?
- In the DataPlane, what two commands can you use for writing data?
- `BatchWriteItem` writes up to how many items in a table?
- `BatchWriteItem` is also useful/used for what other table operation?
- In the DataPlane, what four commands can you use for reading data?
- `BatchGetItem` returns how many items?
- Can `BatchGetItem` get items from multiple tables?
- Which read command retrieves all items that have a specific partition key?
- Which read command retrieves all items in a specified table or index?
- In the DataPlane, what two commands can you use for writing data?
- With the `UpdateItem` command, are you able to add _and_ remove attributes from the selected item?
- In the DataPlane, what two commands can you use for deleting data?
- What are the four stream commands?
- `GetShardIterator` is used to retrieve what data structure? What is this structure used to retrieve?
- `GetRecords` uses what data structure to review one or more stream records?
- What _five_ Scalar types does DynamoDB support?
- Describe the `Binary` scalar type.
- What are the two **document types** in DynamoDB?
- How deeply nested can a map / list be?
- An Item can have up to how much memory allocated to it?
- Describe a `List`.
- Describe a `Map`.
- Describe a `Set`.
- Does each item in a set need to be unique?
- What three types do sets support?
- If two tables, in two different regions, share the same table namespace are they the same table?
- What HTTP response do you get when a write to a DynamoDB table has occurred and is durable.
- With eventually consistent reads, is it possible to get a stale response that doesn't reflect the current state of the table?
- Do strongly consistent reads have a higher latency than eventually consistent reads?
- Are strongly consistent reads supported on the global secondary index?
- Do strongly consistent reads require more throughput than eventually consistent reads?
- What are the two read/write capacity modes that DynamoDB provides for your tables?
- Which capacity mode is the default mode?
- Which capacity mode has free-tier eligible options?
- Do secondary indexes inherit the read/write capacity from the base table?
- DynamoDB on-demand mode instantly accommodate your workloads to support what previously reached metric?
- What situations are DynamoDB on-demand instances suitable for?
- How frequently can you switch a table between on-demand and provisioned?
- What is the RCU expense for a strongly consistent read?
- What is the RCU expense for an eventually consistent read?
- What is the RCU expense for a transactional read?
- RCU's support up to what data size before an additional RCU is needed?
- What is the WCU expense for writing an item?
- What is the WCU expense for a transactional write?
- WCU's support up to what data size before an additional WCU is needed?
- Explain how DynamoDB on-demand capacity mode scales for your peak volumes.
- What happens if you need more capacity than double your previous peak, will you experience throttling?
- Can throttling occur if you exceed double your previous peak in the last 30 minutes?
- If you switch capacity types from provisioned to on-demand, what throughput will your table have until the allocations have been configured?
- What situations is the DynamoDB provisioned capacity mode suitable for?
- If you set a provisioned table with 6 RCU, how many **strong reads** in KB/s can the table support?
- If you set a provisioned table with 6 RCU, how many **eventual reads** in KB/s can the table support?
- If you set a provisioned table with 6 RCU, how many **transactional reads** in KB/s can the table support?
- If you set a provisioned table with 6 WCU, how many **writes** in KB/s can the table support?
- If you set a provisioned table with 6 WCU, how many **transactional writes** in KB/s can the table support?
- If you exceed your provisioned throughput, what is your throughput subject to?
- If a request is throttled, what failure message does it return?
- If a request is throttled, what exception is returned?
- Does DynamoDB automatically manage the throughput for your index tables?
- With auto scaling enabled, you define a RCU/WCU range and what else?
- With auto scaling, can a table or global secondary index adjust its provisioned read/write capacity to handle sudden spikes in traffic without causing throttling?
- What kind of fee and usage levels do you need to define when you used DynamoDB reserved capacity?
- Is reserved capacity available for tables using DynamoDB standard-IA or on-demand capacity mode?
- What two table classes does DynamoDB offer?
- Which DynamoDB table class is the default class and recommended for most workloads?
- Is the choice of the table class permanent?
- DynamoDB allocates additional partitions in what two situations?
- Is the data in a global secondary index stored separately from the data in its base table?
- For your `Query` operations on a table, can you provide an optional condition to the sort key so it only returns the items within a certain range in the partitioned space?

---

# ECS <a name="ECS"></a>

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

# Elastic Beanstalk <a name="Elastic-Beanstalk"></a>

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

# CodeDeploy <a name="CodeDeploy"></a>

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

# Lambda <a name="Lambda"></a>

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

# CloudFormation <a name="CloudFormation"></a>

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

# AWS SAM (Serverless Application Model) <a name="AWS-SAM"></a>

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

# CloudWatch

## CloudWatch + API Gateway

---

# CodeCommit

---

# Lambda

---
