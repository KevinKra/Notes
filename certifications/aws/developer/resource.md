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

h. [Cognito](#Cognito)

i. [X-Ray](#X-Ray)

j. [CloudFront](#CloudFront)

k. [AWS Systems Manager](#AWS-Systems-Manager)

# DynamoDB <a name="DynamoDB"></a>

## Core Components

In DynamoDB, tables, items, and attributes are the core components that you work with. A table is a collection of items, and each item is a collection of attributes. DynamoDB uses primary keys to uniquely identify each item in a table and secondary indexes to provide more querying flexibility. You can use DynamoDB Streams to capture data modification events in DynamoDB tables.

### Optimizations

While using the DynamoDB Accelerator (DAX) will improve the scalability and read performance of the database, it adds a significant cost in maintaining your application. Using `Query` operations and reducing the page size of your query are more cost-effective.

#### Scans

In general, Scan operations are less efficient than other operations in DynamoDB. **A Scan operation always scans the entire table or secondary index. It then filters out values to provide the result you want, essentially adding the extra step of removing data from the result set.**

- As a table or index grows, the `Scan` operation slows. **The `Scan` operation examines every item for the requested values and can use up the provisioned throughput for a large table or index in a single operation.**
- For faster response times, design your tables and indexes so that your applications can use `Query` instead of `Scan`.

#### Reduce page size

- Because a `Scan` operation reads an entire page (by default, 1 MB), you can reduce the impact of the scan operation by setting a smaller page size.
- The `Scan` operation provides a **_Limit_** parameter that you can use to set the page size for your request.

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

### Expressions

- `Projection Expression`: A `projection expression` is a string that identifies the attributes you want. To retrieve a single attribute, specify its name. For multiple attributes, the names must be comma-separated.

```
aws dynamodb get-item \
    --table-name ProductCatalog \
    --key '{"Id":{"N":"1"}}' \
    --projection-expression "Description, RelatedItems[0], ProductReviews.FiveStar"
```

- `Condition Expression`: this is primarily used to determine which items should be modified for data manipulation operations such as `PutItem`, `UpdateItem`, and `DeleteItem` calls.
- `Filter expressions`: simply determines which **items** within the Query results should be returned to you.

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

- The document types are `list` and `map`. **These data types can be nested within each other**, to represent complex data structures **up to 32 levels deep.**

- There is no limit on the number of values in a list or a map, as long as the item containing the **values fits within the DynamoDB item size limit (400 KB).**

#### List

- A list type attribute can store an ordered collection of values. Lists are enclosed in square brackets: `[ ... ]` A list is similar to a JSON array.

- `FavoriteThings: ["Cookies", "Coffee", 3.14159]`

#### Map

- A map type attribute can store an unordered collection of name-value pairs. Maps are enclosed in curly braces: `{ ... }` A map is similar to a JSON object.

- Maps are ideal for storing JSON documents in DynamoDB.

#### Sets

- DynamoDB supports types that represent sets of **number, string, or binary values**. **All the elements within a set must be of the same type.**

- There is no limit on the number of values in a set, as long as the item containing the values fits within the DynamoDB item size limit (400 KB).

- **Each value within a set must be unique.** **The order of the values within a set is not preserved.** Therefore, your applications must not rely on any particular order of elements within the set. DynamoDB does not support empty sets, however, empty string and binary values are allowed within a set.

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

Global secondary indexes in DynamoDB are also composed of partitions. **The data in a global secondary index is stored separately from the data in its base table,** but index partitions behave in much the same way as table partitions.

### Data Distribution: Partition Key

To write an item to the table, DynamoDB uses the value of the partition key as input to an internal hash function. The output value from the hash function determines the partition in which the item will be stored.

> DynamoDB is optimized for uniform distribution of items across a table's partitions, no matter how many partitions there may be. We recommend that you choose a partition key that can have a large number of distinct values relative to the number of items in the table.

### Data Distribution: Partition Key and Sort Key

If the table has a composite primary key (partition key and sort key), DynamoDB calculates the hash value of the partition key in the same way as described in Data Distribution: Partition Key. However, it stores all the items with the same partition key value physically close together, ordered by sort key value.

To write an item to the table, DynamoDB calculates the hash value of the partition key to determine which partition should contain the item. **In that partition, several items could have the same partition key value.** So DynamoDB stores the item among the others with the same partition key, in ascending order by sort key.

You can read multiple items from the table in a single operation (`Query`) if the items you want have the same partition key value. DynamoDB returns all of the items with that partition key value. **Optionally, you can apply a condition to the sort key so that it returns only the items within a certain range of values.**

To read all of the items with an `AnimalType` of _Dog_, you can issue a Query operation without specifying a sort key condition. By default, the items are returned in the order that they are stored (that is, in ascending order by sort key). Optionally, you can request descending order instead.

## DynamoDB Questions

- What are two cost-effective alternatives to DAX for improving read performance on a table or index?
- How does the `Scan` operation work?
- What parameter can you use on `Scan` operations to limit page size?
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
- Can you create a _local_ secondary index for your table after the table has been already created?
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
- For a table with a stream, what happens when an item is deleted from the table?
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
- With the `UpdateItem` command, are you able to add _and_ remove attributes from the selected item?
- In the DataPlane, what two commands can you use for deleting data?
- What are the four stream commands?
- Can `ListStreams` list streams from one or multiple tables?
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
- What kind of fee and usage levels do you need to define when you use DynamoDB reserved capacity?
- DynamoDB allocates additional partitions in what two situations?
- What two table classes does DynamoDB offer?
- What is reserved capacity, what capacity mode is it related to?
- Is reserved capacity available for tables using DynamoDB standard-IA or on-demand capacity mode?
- Which DynamoDB table class is the default class and recommended for most workloads?
- Is the choice of the table class permanent?
- Is the data in a global secondary index stored separately from the data in its base table?
- Can several items exist in the same partition? What is used to differentiate them?
- For your `Query` operations on a table, can you provide an optional condition to the sort key so it only returns the items within a certain range in the partitioned space?

---

# ECS <a name="ECS"></a>

Elastic Container Service (ECS) is a highly scalable and fast container management service that makes it easy to run, stop, and manage containers on a **Cluster**.

## ECS Overview

ECS is composed of several core pieces, they include:

- **Clusters**
  - When you run tasks using ECS, you place them in a cluster, which is a logical grouping of resources.
- **Task Definition**
  - Task definitions specify various parameters for your application. It is a text file, _in JSON format_, that describes one or more containers, **up to a maximum of ten**, that form your application.
    - **Tasks**
      - A task is the instantiation of a task definition _within a cluster_. After you have created a task definition for your application, you can specify the number of tasks that will run on your cluster.
    - **Task Scheduler**
      - The task scheduler is responsible for placing tasks within your cluster.
- **Containers**
  - A container is a standardized unit of software development that contains everything that your software application needs to run, including relevant code, runtime, system tools, and system libraries. Containers are created from a read-only template called an image.
    - **Container Agent**
      - _The container agent runs on each container instance within an Amazon ECS cluster._ The agent sends information about the resource's current running tasks and resource utilization to Amazon ECS.

## Application Architecture

### Fargate Launch Type

**The Fargate launch type is good for the following workloads:**

- Large workloads that need to be optimized for low overhead.
- Small workloads that have occasional burst.
- Tiny workloads.
- Batch workloads.

When architecting your application to run on Amazon ECS using AWS Fargate, the main question is when should you put multiple containers into the _same Task Definition_ versus deploying containers _separately in multiple Task Definitions_.

> **Batch Workloads:** Jobs that can run without end user interaction, or can be scheduled to run as resources permit, are called batch jobs. Batch processing is for those frequently used programs that can be executed with **minimal human interaction**.

#### When the following conditions are required, we recommend that you deploy your containers in a single Task Definition:

- Your containers **share a common lifecycle** (that is, they are launched and terminated together).
- Your containers **must run on the same underlying host** (that is, one container references the other on a localhost port).
- You require that your **containers share resources**.
- Your containers **share data volumes**.

Otherwise, you should define your containers in separate tasks definitions so that you can scale, provision, and deprovision them separately.

### EC2 Launch Type

The EC2 launch type is good for **large workloads that need to be optimized for price**.

When you’re considering how to model task definitions and services using the EC2 launch type, it helps to think about what processes need to run together and how to scale each component.

**As an example, imagine an application that consists of the following components:**

- A frontend service that displays information on a webpage.
- A backend service that provides APIs for the frontend service.
- A data store.

---

## Clusters

- When you run tasks using ECS, you place them in a cluster, **which is a logical grouping of resources.**

You can register one or more Amazon EC2 instances (also referred to as **container instances**) with your cluster to run tasks on them. Or, you can use the serverless infrastructure that Fargate provides to run tasks. When your tasks are run on Fargate, your cluster resources are also managed by Fargate.

- A cluster may contain a _mix of tasks_ hosted on AWS Fargate, Amazon EC2 instances, or external instances.
- Clusters are **region-specific**.
- **Before you can delete a cluster, you must delete the services and deregister the container instances inside that cluster.**

## Task Components

### Tasks

- A **task** is the instantiation of a task definition within a cluster.
- After you have created a task definition for your application within Amazon ECS, you can specify the number of tasks to run on your cluster.
- Amazon ECS tasks running on both Amazon EC2 and AWS Fargate can mount EFS file systems.

### Tasks Definitions

- Task definitions specify various parameters for your application. It is a text file, in JSON format, that describes one or more containers, up to a maximum of ten, that form your application.
- **Your entire application stack does not need to be on a single task definition, and in most cases it should not.** Your application can span multiple task definitions. You can do this by combining related containers into their own task definitions, each representing a single component.

### Task Scheduler

- The task scheduler is responsible for placing tasks within your cluster.

## Container Agent

- **The container agent runs on each container instance within an Amazon ECS Cluster.**
- The agent sends information about the resource's **current running tasks and resource utilization to Amazon ECS.**
- The Container Agent starts and stops tasks whenever it receives a request from Amazon ECS.

## Deployment

### Task Placement Strategy

- **Task placement strategy** is an algorithm for selecting instances for task placement or tasks for termination.

- **Amazon ECS supports the following task placement strategies:**
  - **binpack**: tasks are placed on container instances so as to leave the least amount of unused CPU or memory. This strategy minimizes the number of container instances in use. When this strategy is used and a scale-in action is taken, Amazon ECS terminates tasks. It does this based on the amount of resources that are left on the container instance after the task is terminated. The container instance that has the most available resources left after task termination has that task terminated.
  - **random**: this will just place tasks on instances randomly.
  - **spread**: The spread strategy, contrary to the `binpack` strategy, tries to put your tasks on as many different instances as possible. It is typically used to achieve high availability and mitigate risks, by making sure that you don’t put all your task-eggs in the same instance-baskets.

## ECS Questions

- A Cluster is a logical grouping of what?
- What is a Task Definition?
- What language are Task Definitions written in?
- What is the maximum number of containers that a Task Definition can describe?
- What is a Task?
- Where is a Task Definition, as a Task, instantiated?
- What does the Task Scheduler do?
- Can EC2 and Fargate Tasks mount EFS?
- What is an Container?
- What is the name of the _read-only_ template that Containers are created from?
- What workloads is Fargate suitable for?
- What four conditions are suitable for deploying multiple containers in the _same_ Task Definition?
- If your Task Definition's underlying containers will share the same resources and/or volumes, should they exist in the same Task Definition?
- What is a Batch Workload?
- The EC2 Launch type is suitable for what type of workloads?
- EC2 instances being used in a Cluster are also called what?
- Can a Cluster contain tasks hosted on Fargate, EC2 instances, and external instances?
- What is another name for EC2 instances that are attached to Clusters?
- Are Clusters region specific?
- Before you delete a cluster, do you need to delete the services?
- Before you delete a cluster, do you need to deregister the container instances running inside that Cluster?
- What runs on each container instance within an ECS cluster?
- What information does the Container Agent send to ECS regarding the container it's on?
- Is the Container Agent responsible for starting and stopping tasks when it receives requests from ECS?
- What is an ECS task placement strategy?
- What are the three ECS task placement strategies?
- Describe the `binpack` task placement strategy.
- What is the scale-in approach for the `binpack` task placement strategy?
- Describe the `random` task placement strategy.
- Describe the `spread` task placement strategy.
- The objective of the `spread` strategy is to provide what?

---

# Elastic Beanstalk <a name="Elastic-Beanstalk"></a>

## Elastic Beanstalk concepts

### Application

- An Elastic Beanstalk **_application_** is a logical collection of Elastic Beanstalk components, including environments, versions, and environment configurations.

### Application Version

- In Elastic Beanstalk, an **_application version_** refers to a **specific, labeled iteration of deployable code for a web application.** An application version points to an **S3 object** that contains the deployable code.

- Applications can have many versions and **each application version is unique.**

### Environment

- An **_environment_** is a collection of AWS resources running an application version.

- **Each environment runs only one application version at a time**, however, you can run the same application version or different application versions in many environments simultaneously.

## Environment Tier

- The environment is the heart of the application. The Environment Tier designates the **_type of application_** that the environment runs, and determines what resources Elastic Beanstalk provisions to support it.

- When you launch an Elastic Beanstalk environment, you first choose an environment tier.

- There are two environment tiers in Elastic Beanstalk: **Web Server Tier** and **Worker Environment Tier**.

### _Web Server Tier_

> An application that serves HTTP requests runs in a **web server environment tier**.

- When you create an environment, **Elastic Beanstalk provisions the resources required to run your application.** AWS resources created for an environment include: one ELB, an Auto Scaling group, and one or more EC2 instances.
- Every environment has a CNAME (URL) that points to a load balancer.
- **The environment has a URL**, such as `myapp.us-west-2.elasticbeanstalk.com`. **This URL is aliased in Amazon Route 53 to an Elastic Load Balancing URL**—something like `abcdef-123456.us-west-2.elb.amazonaws.com` -—by using a CNAME record.
- The software stack running on the EC2 instances is dependent on the **_Container Type_**. A container type defines the infrastructure topology and software stack to be used for that environment.
  > For example, an Elastic Beanstalk environment with an Apache Tomcat container uses the Amazon Linux operating system, Apache web server, and Apache Tomcat software.
- A software component called the **Host Manager (HM) runs on each Amazon EC2 instance**. The Host Manager reports metrics, errors, events, and server instance status, which are available via the Elastic Beanstalk console, APIs, and CLIs.

### _Worker Environment Tier_

> A backend environment that pulls tasks from an SQS queue runs in a **worker environment tier.**

- AWS resources created for a worker environment tier include: an Auto Scaling group, one or more Amazon EC2 instances, and an IAM role.
- For the worker environment tier, **Elastic Beanstalk also creates and provisions an Amazon SQS queue if you don’t already have one.**
- When you launch a worker environment, **Elastic Beanstalk installs the necessary support files for your programming language of choice** _and_ **a daemon on each EC2 instance in the Auto Scaling group.**
  - The daemon reads messages from an Amazon SQS queue.
  - The daemon sends data from each message that it reads to the web application running in the worker environment for processing.
  - If you have multiple instances in your worker environment, each instance has its own daemon, but they all read from the same Amazon SQS queue.
- You can define periodic tasks in a file named `cron.yaml` in your source bundle to add jobs to your worker environment's queue automatically at a regular interval.

#### Dead-Letter Queues

- Elastic Beanstalk worker environments support Amazon Simple Queue Service (Amazon SQS) dead-letter queues. A **dead-letter queue** is a queue where other (source) queues can send messages that for some reason could not be successfully processed.
- A primary benefit of using a dead-letter queue is **the ability to sideline and isolate the unsuccessfully processed messages.** You can then analyze any messages sent to the dead-letter queue to try to determine why they were not successfully processed.

### Environment Configuration

- An **_environment configuration_** identifies a collection of parameters and settings that define how an environment and its associated resources behave.

#### Docker Configuration

- There are **two** generic configurations for Docker with EB:
  - **Single Container:** Used to deploy a Docker image and source code inside a **single container per instance.**
  - **Multicontainer:** Used to deploy _multiple_ containers per instance. Uses ECS to deploy a cluster in the EB environment.

### Saved Configuration

- A **_saved configuration_** is a template that you can use as a starting point for creating unique environment configurations.

- The API and the AWS CLI refer to saved configurations as **_configuration templates_**.

### Platform

- A **_platform_** is a combination of an operating system, programming language runtime, web server, application server, and Elastic Beanstalk components.

- You design and target your web application to a platform. Elastic Beanstalk provides a variety of platforms on which you can build your applications.

## Deployment

- This service is not suitable for deploying _serverless applications_. In addition, it doesn’t have the capability to locally build, test, and debug your serverless applications as effectively as what AWS SAM can do.

- Because AWS Elastic Beanstalk performs an **in-place** update when you update your application versions, your application might become unavailable to users for a short period of time. **To avoid this, perform a blue/green deployment.** To do this, deploy the new version to a separate environment, and then swap the CNAMEs of the two environments to redirect traffic to the new version instantly.

### Deployment Options

> In ElasticBeanstalk, you can choose from a variety of deployment methods:

- **All at once** – Deploy the new version to all instances simultaneously. All instances in your environment are out of service for a short time while the deployment occurs. It is possible that there would be a degradation of the service since some instances would be unavailable during the deployment process. _All at once has the shortest deployment time of all the methods._

- **Rolling** – Deploy the new version in batches. Each batch is taken out of service during the deployment phase, reducing your environment’s capacity by the number of instances in a batch. This method will deploy the new version in batches only to existing instances, without provisioning new resources. The compute capacity of the environment would still be compromised in this method.

- **Rolling with additional batch** – Starts by deploying your application code to a single batch of newly created EC2 instances. **Once the deployment succeeds on the first batch of instances**, the application code is deployed to the remaining instances in batches until the last batch of instances remains. **At this point, the last batch of instances is terminated.** This deployment policy ensures that the impact of a failed deployment is limited to a single batch of instances and enables your application to serve traffic at full capacity during an ongoing deployment.

- **Immutable** – Deploy the new version to a fresh group of instances by performing an immutable update. To perform an immutable environment update, Elastic Beanstalk creates a second, temporary Auto Scaling group behind your environment’s load balancer to contain the new instances. Immutable deployments can prevent issues caused by partially completed rolling deployments. **If the new instances don’t pass their health checks, Elastic Beanstalk terminates them, leaving the original instances untouched.** If an immutable environment update fails, the **rollback process requires only terminating an Auto Scaling group**. **A failed rolling update, on the other hand, requires performing an additional rolling update to roll back the changes.**

- **Traffic splitting** – Deploy the new version to a fresh group of instances and temporarily split incoming client traffic between the existing application version and the new one.

## Elastic Beanstalk Questions

- Describe the concept of an Elastic Beanstalk **Application**.
- Describe the concept of an Elastic Beanstalk **Application Version**.
- Does the Application Version point to an S3 Object to find the deployable code for that version?
- Describe the concept of an Elastic Beanstalk **Environment**.
- Can an Elastic Beanstalk environment run _more than_ one application version at a time?
- Can you run the same application version, or different versions, in many environments simultaneously?
- The Environment Tier designates what?
- Does Elastic Beanstalk use the environment tier to determine what resources to provision to support it?
- What are the two types of Elastic Beanstalk environment tiers?
- An application that serves HTTP requests runs in which Elastic Beanstalk environment tier?
- A backend application that pulls tasks from an SQS queue runs in which Elastic Beanstalk environment tier?
- Does your _Web Server Application Tier_ environment have a URL?
- Is your web server environment aliased in Amazon Route 53 to an Elastic Load Balancing URL?
- For the **Elastic Beanstalk Web Server Tier** does AWS setup the following resources for the environment: an ELB, Auto Scaling Group, and one or more EC2 instances?
- The software stack running on the Elastic Beanstalk EC2 instances is dependent on what?
- Does a **container type** define the infrastructure topology and software stack to be used for an EC2 instance's environment?
- Does a **host manager** run on each Elastic Beanstalk EC2 instance?
- Does the Host Manager report metrics, errors and events, and server instance status?
- Where are Host Manager reports found?
- What three resources are created for a worker environment tier?
- For the worker environment tier, does Elastic Beanstalk create and provision an Amazon SQS queue if you don’t already have one?
- When you launch a worker environment, does Elastic Beanstalk install the necessary support files for your programming language of choice and a daemon on each EC2 instance in the Auto Scaling group?
- Does the installed daemon read messages from the SQS queue?
- Does the daemon send data from each SQS message to the worker environment for processing?
- If you have multiple instances in your worker environment, does each instance have _its own_ daemon?
- Do the daemon, on each EC2 instance, read from the same SQS queue?
- What is an **Environment Configuration**?
- What is a **Saved Configuration**?
- What does the API and AWS CLI refer to saved configurations as?
- What is a **Platform?**
- Is Elastic Beanstalk suitable for deploying serverless applications?
- Can Elastic Beanstalk build, test, and dug, serverless applications as well as AWS SAM can?
- Elastic Beanstalk performs in-place updates when you update your application versions, in order to avoid downtime what type of deployment can you use?
- What are the _five_ deployment options for Elastic Beanstalk?
- Describe the `All at once` deployment option.
- Describe the `Rolling` deployment option.
- Describe the `Rolling with additional batch` deployment option.
- Describe the `Immutable` deployment option.
- Describe the `Traffic Splitting` deployment option.
- What is a dead-letter queue?
- What is the primary benefit of dead-letter queues?
- What are the two generic configurations for Docker with Elastic Beanstalk?
- Describe the Docker single container EB configuration.
- Describe the Docker multicontainer EB configuration.

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
  - The **execution role** grants the function permission to use AWS services, such as CloudWatch Logs for log streaming and AWS X-Ray for request tracing.

- A function has an **unpublished version**, and can have **published versions** and **aliases**.

  - The **unpublished version** changes when you update your function’s code and configuration.
  - A **published version** is a snapshot of your function code and configuration that can’t be changed.
  - An **alias** is a named resource that maps to a version, _and can be changed to map to a different version._

- You can use the CreateFunction API _via the AWS CLI or the AWS SDK_ of your choice.
- **Lambda Alias** is just like a pointer to a specific Lambda function version.
- **Execution Context** is a temporary runtime environment that initializes any external dependencies of your Lambda function code, such as database connections or HTTP endpoints.

- To create a Lambda function, you first create a Lambda function deployment package, **a `.zip` or `.jar` file** consisting of your code and any dependencies. **When creating the zip, include only the code and its dependencies, not the containing folder.** You will then need to set the appropriate security permissions for the zip package.

- In **Lambda non-proxy (or custom) integration**, you can specify how the incoming request data is mapped to the integration request and how the resulting integration response data is mapped to the method response.

### Lambda VPC configurations

- AWS Lambda uses the VPC information you provide **to set up ENIs** that allow your Lambda function to access VPC resources. Each **ENI is assigned a private IP address from the IP address range within the subnets you specify** but is not assigned any public IP addresses.
- If your Lambda function requires Internet access (for example, to access AWS services that don't have VPC endpoints ), **you can configure a NAT instance inside your VPC or you can use the Amazon VPC NAT gateway.**
- You cannot use an Internet gateway attached to your VPC to communicate with your Lambda, since that requires the ENI to have public IP addresses.
- You should also ensure that the associated security group of the Lambda function allows outbound connections.

### Sync & Async lambda functions

- AWS Lambda supports synchronous and asynchronous invocation of a Lambda function. **You can control the invocation type only when you invoke a Lambda function (referred to as on-demand invocation).** The following examples illustrate on-demand invocations:

  - Your custom application invokes a Lambda function.
  - You manually invoke a Lambda function (for example, using the AWS CLI) for testing purposes.

- When you use AWS services as a trigger, **the invocation type is predetermined for each service.** **You have no control over the invocation type that these event sources use** when they invoke your Lambda function.

- In the `Invoke` API, you have 3 options to choose from for the `InvocationType`:
  - `RequestResponse` (default) - Invoke the function synchronously. Keep the connection open until the function returns a response or times out. The API response includes the function response and additional data.
  - `Event` - Invoke the function asynchronously. Send events that fail multiple times to the function's dead-letter queue (if it's configured). The API response only includes a status code.
  - `DryRun` - Validate parameter values and verify that the user or role has permission to invoke the function.

### Deployment Package

- If your deployment package is larger than **50 MB**, we recommend uploading your function code and dependencies to an Amazon S3 bucket.
- There is a **hard limit of 50MB for compressed deployment package** with AWS Lambda and an **uncompressed AWS Lambda hard limit of 250MB.**
- If we put these large binaries into an AWS Lambda Layer and attach it to our AWS Lambda function. **We still can’t escape the 250mb hard limit.**

### Errors

- The `InvalidParameterValueException` will be returned if one of the parameters in the request is invalid. For example, if you provided an IAM role in the CreateFunction API which AWS Lambda is unable to assume.
- If you have exceeded your maximum total code size per account, the `CodeStorageExceededException` will be returned.
- If the resource already exists, the `ResourceConflictException` will be returned.
- If the AWS Lambda service encountered an internal error, the `ServiceException` will be returned.

### Layers

- You can configure your Lambda function to pull in additional code and content in the form of layers.
- A layer is a ZIP archive that contains libraries, a custom runtime, or other dependencies. With layers, you can use libraries in your function without needing to include them in your **deployment package**.
- Layers help you keep your deployment package small, which makes development easier. You can avoid errors that can occur when you install and package dependencies with your function code.
- For _Node.js, Python, and Ruby functions_, you can develop your function code in the Lambda console **as long as you keep your deployment package under 3 MB**.
- **A function can use up to 5 layers at a time.** The total unzipped size of the function and all layers can’t exceed the unzipped deployment package size **limit of 250 MB.**
- You can create layers, or use layers published by AWS and other AWS customers.
- Layers are extracted to the `/opt` directory in the function execution environment. Each runtime looks for libraries in a different location under `/opt`, depending on the language. Structure your layer so that function code can access libraries without additional configuration.

### Aliases & Versioning

- Aliases allow you to point to two different versions of the Lambda function and dictate what percentage of incoming traffic is sent to each version.
- Each Lambda function has a unique ARN.
- Each alias has a unique ARN.
- The latest version of a function is tagged as `$LATEST`.
- After you _publish_ a version, it cannot be changed.
- Aliases _can_ be modified.
- You can update an alias to point do a different version.

### Concurrency

- If you create a Lambda function to process events from event sources that aren't poll-based (for example, Lambda can process every event from other sources, like Amazon S3 or API Gateway), each published event is a unit of work, in parallel, up to your account limits.

- Formula: `concurrent executions = (invocations per second) x (average execution duration in seconds)`

  - For example, consider a Lambda function that processes Amazon S3 events. Suppose that the Lambda function takes on average three seconds and Amazon S3 publishes 10 events per second. Then, you will have 30 concurrent executions of your Lambda function.
  - `(10 events per second) x (3 seconds average execution duration)` = 30 concurrent executions

- AWS Lambda dynamically scales function execution in response to increased traffic, up to your concurrency limit.
- AWS Lambda limits **the total concurrent executions across all functions within a given region to 1000**. This limit can be raised by requesting for AWS to increase the limit of the concurrent executions of your account.

### Deployment

- **Canary**: In a Canary deployment configuration, the traffic is shifted in two increments. You can choose from predefined canary options that specify the percentage of traffic shifted to your updated Lambda function version in the first increment and the interval, in minutes, before the remaining traffic is shifted in the second increment.
- **Linear**: This will cause the traffic to be shifted in equal increments with an equal number of minutes between each increment. You can choose from predefined linear options that specify the percentage of traffic shifted in each increment and the number of minutes between each increment.
- **All-at-once** With this deployment configuration, the traffic is shifted from the original Lambda function to the updated Lambda function version all at once.

## Execution Environment

- You can now configure your AWS Lambda functions to run up to **15 minutes per execution**. Previously, the maximum execution time (timeout) for a Lambda function was 5 minutes.
- Lambda invokes your function in an execution environment, **which provides a secure and isolated runtime environment.**
- The execution environment manages the resources required to run your function. The execution environment also provides **lifecycle support** for the function's runtime and any external extensions associated with your function.
- When you create your Lambda function, you specify configuration information, **such as the amount of memory available and the maximum execution time allowed for your function.** Lambda uses this information to set up the execution environment.
- The function's **runtime and each external extension** are processes that run within the execution environment.
- **Permissions, resources, credentials, and environment variables** are shared between the function and the extensions.

### Lambda Execution Environment Lifecycle

- The lifecycle of the execution environment includes the following phases:

  - **Init:** In this phase, **Lambda creates or unfreezes an execution environment** with the configured resources, downloads the code for the function and all layers, initializes any extensions, initializes the runtime, and then runs the function’s initialization code (the code outside the main handler).
  - **Invoke:** In this phase, Lambda invokes the function handler. After the function runs to completion, **Lambda prepares to handle another function invocation**.
  - **Shutdown:** This phase is triggered if the Lambda function **_does not receive any invocations_** for a period of time. In the Shutdown phase, Lambda shuts down the runtime, alerts the extensions to let them stop cleanly, and then removes the environment. Lambda sends a Shutdown event to each extension, which tells the extension that the environment is about to be shut down.

- Each phase starts with an **event** that Lambda sends _to the runtime and to all registered extensions._

- The runtime and each extension indicate completion by **sending a `Next` API request**. **Lambda freezes the execution environment when the runtime and each extension have completed and there are no pending events.**

#### Init Phase

- In the Init phase, Lambda performs **three tasks**:

  - Start all extensions (Extension init)
  - Bootstrap the runtime (Runtime init)
  - Run the function's static code (Function init)

- The Init phase ends when **the runtime and all extensions signal that they are ready** by sending a `Next` API request.
- **The Init phase is limited to 10 seconds.**
- If all three tasks do not complete within 10 seconds, **Lambda retries the Init phase at the time of the first function invocation.**

#### Invoke Phase

- When a Lambda function is invoked in response to a Next API request, Lambda sends an Invoke event to the runtime and to each extension.
- **The function's timeout setting limits the duration of the entire Invoke phase.** For example, if you set the function timeout as 360 seconds, the function and all extensions need to complete within 360 seconds.
- Note that **there is no independent post-invoke phase.** The duration is the sum of all invocation time (runtime + extensions) and is not calculated until the function and all extensions have _finished_ executing.
- The invoke phase ends after the runtime and all extensions signal that they are done by sending a Next API request.
- If the Lambda function **crashes or times out** during the Invoke phase, **Lambda resets the execution environment.**
- The reset behaves like a Shutdown event. First, Lambda shuts down the runtime. Then Lambda sends a Shutdown event to each registered external extension. **The event includes the reason for the shutdown.** If another Invoke event results in this execution environment being reused, Lambda initializes the runtime and extensions as part of the _next_ invocation.
  - The Lambda reset **does not** clear the `/tmp` directory content prior to the next init phase. This behavior is consistent with the regular shutdown phase.

#### Shutdown Phase

- When Lambda is about to shut down the runtime, it sends a Shutdown event to the runtime and to each external extension. Extensions can use this time for final cleanup tasks.
- The Shutdown event is a response to a `Next` API request.
- **The entire Shutdown phase is capped at 2 seconds.** If the runtime or any extension does not respond, **Lambda terminates it via a signal (`SIGKILL`).**
- After the function and all extensions have completed, Lambda maintains the execution environment for some time in anticipation of another function invocation. **In effect, Lambda freezes the execution environment.**
- When the function is invoked again, Lambda thaws the environment for reuse.

- Reusing the execution environment has the following implications:
  - **Objects declared _outside_ of the function's handler method remain initialized**, providing additional optimization when the function is invoked again. For example, if your Lambda function establishes a database connection, instead of reestablishing the connection, the original connection is used in subsequent invocations. **We recommend adding logic in your code to check if a connection exists before creating a new one.**
  - **Each execution environment provides 512 MB of disk space in the `/tmp` directory.** The directory content _remains_ when the execution environment is frozen, **providing a transient cache that can be used for multiple invocations.** You can add extra code to check if the cache has the data that you stored.
  - **Background processes or callbacks that were initiated by your Lambda function and did not complete when the function ended resume if Lambda reuses the execution environment.** Make sure that any background processes or callbacks in your code are complete before the code exits.

When you write your function code, **do not assume that Lambda automatically reuses the execution environment for subsequent function invocations.** Other factors may dictate a need for Lambda to create a new execution environment, which can lead to unexpected results, such as database connection failures.

## Lambda Questions

- Lambda uses the VPC information you provide to setup what?
- The ENI used by lambda is assigned a public or private IP address?
- If your Lambda function within your VPC requires internet access what can you use?
- What Lambda support both synchronous and asynchronous invocations?
- And what circumstance can _you_ determine the invocation type?
- When you use AWS services as the trigger, do you have any control over the lambda invocation type?
- In the Lambda `Invoke` API, you have 3 options to choose for `InvocationType`. What are they?
- Which lambda invocation type is the default variant?
- Which lambda invocation type is synchronous?
- Which lambda invocation type is asynchronous?
- What two elements are _required_ when you create a lambda function?
- what does the **deployment package** provide?
- What does the **execution role** provide?
- What two file types are suitable for the lambda function deployment package?
- A lambda can come in two "versions", what are they?
- Describe a **unpublished version** of a lambda function.
- Describe a **published version** of a lambda function.
- Describe a lambda function **alias**.
- What is the Execution Context in a lambda function and what use cases can it provide?
- What is the hard limit for a _compressed_ deployment package?
- What is the hard limit for an _uncompressed_ deployment package?
- Can layers be used to get around the hard limit of 250MB?
- Describe the `InvalidParameterValueException` error.
- Describe the `CodeStorageExceededException` error.
- Describe the `ResourceConflictException` error.
- Describe the `ServiceException` error.
- What are lambda layers?
- Layers help keep supporting code out of what?
- What languages can you develop your function code in within the lambda console, what is max size for the deployment package?
- How many layers can a lambda function have at a time?
- What is the max size for deployment package when you develop your code in the lambda console?
- What is the max unzipped deployment package size for a lambda function and all of its layers?
- Are you able to create layers and also use layers published by AWS and AWS Customers?
- What directory are layers extracted from in the function execution environment?
- With aliases, can you distribute incoming traffic to different versions of a function?
- What is the concurrency formula?
- If you had a function that takes 3 seconds and S3 publishes 10 events per second, how many concurrent executions will you have?
- What is the **default** concurrency limit for all functions within a given region?
- What three types of lambda deployments are there?
- Describe lambda **canary** deployments.
- Describe lambda **linear** deployments.
- Describe lambda **all-at-once** deployments.
- How long can your lambda function's execution run for?
- Is an lambda execution environment a secure and isolated environment?
- When you setup a Lambda function, do you provide configuration info such as available memory and maximum execution time for your function?
- What two processes run within a functions execution environment?
- Does the function's runtime _and_ extensions share permissions, resources, credentials, and environment variables between them?
- The lifecycle of an execution environment consists of what three phases?
- Is the **Init** phase able to unfreeze previous execution environments?
- What triggers the **Shutdown** phase?
- What does each phase start with and does lambda send it to the runtime and all registered events?
- What api request does the run time, and each extension, use to indicate completion?
- Does Lambda freeze the execution environment when the runtime and each extension have completed and there are no pending events?
- What three tasks does the **Init phase** perform?
- The Init phase ends when what happens?
- The Init phase is limited to how much time?
- What happens if all three tasks don't complete within 10 seconds?
- Does the function and all it's extension need to complete _within_ the timeout phase you set?
- What does the runtime and all extensions use to signal they are done?
- If the lambda function crashes, or times out, does it reset the execution environment?
- Explain the process in which the Invoke phase handles a shutdown.
- Does the lambda **Invoke phase** reset clear the `/tmp` directory prior to the next init phase? Is this behavior consistent with the regular shutdown phase?
- The entire Shutdown phase is capped at how many seconds?
- What happens if the runtime, or any extension, does not respond to the shutdown phase?
- Does lambda temporarily maintain the execution environment after the function, and all extensions, have completed?
- The process of preserving your runtime environment is called what?
- What is an example of objects declared outside of a functions handler remaining initialized?
- Should you include logic in your code to check if a connection exists before creating a new one?
- Each execution environment provides how many MB of disk space in the `/tmp` directory?
- Does the `/tmp` directory content remain when the execution environment freezes, thus serving as a transient cache between multiple invocations?
- Do background processes or callbacks that were initiated by your Lambda function and did not complete when the function ended resume if Lambda reuses the execution environment?
- Should you make sure that any background processes or callbacks in your code are **complete** before your code exits?
- Reusing an execution environment has what (3) implications?
- Does each Lambda function have a unique ARN?
- Does each Lambda alias have a unique ARN?
- The latest lambda version has what tag?
- Can aliases be modified, ie updating an alias to point to a different version?

---

# CloudFormation <a name="CloudFormation"></a>

> AWS CloudFormation is a service that helps you model and set up your AWS resources so that you can spend less time managing those resources and more time focusing on your applications that run in AWS. You create a template that describes all the AWS resources that you want (like Amazon EC2 instances or Amazon RDS DB instances), and CloudFormation takes care of provisioning and configuring those resources for you. You don't need to individually create and configure AWS resources and figure out what's dependent on what; CloudFormation handles that.

## Core Benefits of IAAS

- **Simplify Infrastructure Management**

  - **Problem:** For a scalable web application that also includes a backend database, you might use an Auto Scaling group, an Elastic Load Balancing load balancer, and an Amazon Relational Database Service database instance. You might use each individual service to provision these resources and after you create the resources, you would have to configure them to work together. All these tasks can add complexity and time before you even get your application up and running.
  - **Solution:** You can create a **CloudFormation _template_** or modify an existing one. _A template describes all your resources and their properties._ When you use that template to create a CloudFormation stack, CloudFormation provisions the Auto Scaling group, load balancer, and database for you. After the stack has been successfully created, your AWS resources are up and running. You can delete the stack just as easily, which deletes all the resources in the stack. By using CloudFormation, you easily manage a collection of resources as a single unit.

- **Quickly Replicate your Infrastructure**

  - **Problem:** If your application requires additional availability, you might replicate it in multiple regions so that if one region becomes unavailable, your users can still use your application in other regions. The challenge in replicating your application is that it also requires you to replicate your resources. Not only do you need to record all the resources that your application requires, but you must also provision and configure those resources in each region.
  - **Solution:** Reuse your CloudFormation template to create your resources in a consistent and repeatable manner. To reuse your template, describe your resources once and then provision the same resources over and over in multiple regions.

- **Easily Control and Track Changes**

  - **Problem:** In some cases, you might have underlying resources that you want to upgrade incrementally. For example, you might change to a higher performing instance type in your Auto Scaling launch configuration so that you can reduce the maximum number of instances in your Auto Scaling group. If problems occur after you complete the update, you might need to roll back your infrastructure to the original settings. To do this manually, you not only have to remember which resources were changed, you also have to know what the original settings were.
  - **Solution:** When you provision your infrastructure with CloudFormation, the CloudFormation template describes exactly what resources are provisioned and their settings. Because these templates are text files, you simply track differences in your templates to track changes to your infrastructure, similar to the way developers control revisions to source code. For example, you can use a version control system with your templates so that you know exactly what changes were made, who made them, and when. If at any point you need to reverse changes to your infrastructure, you can use a previous version of your template.

### Drift in CloudFormation

When you start to use CloudFormation for establishing your infrastructure, you may start to experience an issue called **drift**. As an application grows and transforms, changes are gradually introduced through CloudFormation templates. These templates assume no changes have occurred to your AWS infrastructure outside of them. In the event that a user starts directly interfacing with the AWS console or CLI and modifies resources, _drift_ will start to appear between the actual true state of your infrastructure and what is being maintained through your CloudFormation templates. Consequently, it's very important to _only_ make changes to your infrastructure through templates once CloudFormation is established.

## CloudFormation Concepts

### Templates

- A Cloudformation template is a **JSON- or YAML-formatted text file** that describes your AWS infrastructure.
- You can save template files with any extension, such as `.json`, `.yaml`, `.template`, or `.txt`.
- CloudFormation uses templates as blueprints for building your AWS resources. For example, in a template, you can describe an EC2 instance, such as the instance type, the AMI ID, block device mappings, and its Amazon EC2 key pair name. **Whenever you create a stack, you also specify a template that CloudFormation uses** to create whatever you described in the template.
- CloudFormation templates have additional capabilities that you can use to build complex sets of resources and reuse those templates in multiple contexts. For example, **you can add input parameters whose values are specified when you create a CloudFormation stack.** In other words, _you can specify a value like the instance type when you create a stack instead of when you create the template_, making the template easier to reuse in different situations.

### Stacks

- When you use CloudFormation, you manage related resources as a single unit called a stack.
- You create, update, and delete a collection of resources by creating, updating, and deleting stacks. **All the resources in a stack are defined by the stack's CloudFormation template.**
- Suppose you created a template that includes an Auto Scaling group, Elastic Load Balancing load balancer, and an Amazon Relational Database Service (Amazon RDS) database instance. **To create those resources, you create a stack by submitting the template that you created, and CloudFormation provisions all those resources for you.** You can work with stacks by using the CloudFormation console, API, or AWS CLI.

### Change Sets

- If you need to make changes to the running resources in a stack, you update the stack. Before making changes to your resources, you can generate a change set, **which is a summary of your proposed changes.** Change sets allow you to see how your changes might impact your running resources, especially for critical resources, before implementing them.
- Change Sets are similar to a `git diff`. It shows the _before_ and _after_ changes of the set it's referencing.
- For example, **if you change the name of an Amazon RDS database instance, CloudFormation will create a new database and delete the old one. You will lose the data in the old database unless you've already backed it up.** If you generate a change set, you will see that your change will cause your database to be replaced, and you will be able to plan accordingly before you update your stack.
- When you specify a transform, **you can use AWS SAM syntax to declare resources in your template.** The model defines the syntax that you can use and how it is processed. More specifically, the `AWS::Serverless` transform, **which is a macro hosted by AWS CloudFormation**, takes an entire template written in the AWS Serverless Application Model (AWS SAM) syntax and transforms and expands it into a compliant AWS CloudFormation template.
- `Transform` section specifies the version of the AWS SAM to use. This is used for serverless applications (also referred to as Lambda-based applications).
- `Mappings` section lists a mapping of keys and associated values that you can use to specify conditional parameter values, similar to a lookup table.
- `Resources` section is primarily used to specify the stack resources and their properties.
- `Parameter` section is the part of the template that contains values to pass to your template at runtime when you create or update a stack.

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

- What are three core benefits of using CloudFormation and IAAS?
- Describe how CloudFormation helps with simplifying infrastructure management.
- Describe how CloudFormation helps with infrastructure replication.
- Describe how CloudFormation helps with controlling and tracking changes.
- What is drift?
- What languages are a CloudFormation template be written in?
- What do you need to provide when creating a stack?
- What feature on templates can you leverage that allows you to define values when a stack is being initialized?
- By using parameters on templates, are developers able to create more adaptable and reusable stacks?
- All resources in a stack are defined by what?
- What use case does a ChangeSet provide?
- A ChangeSet can be consider similar to what `git` feature?
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
- Are changes to a lambda deployment package, that is stored in S3, automatically detected during CloudFormation stack updates?
- When updating a stack, how can you detect a change to your function code stored in S3 within a CloudFormation template?
- Does the `Zipfile` parameter in the `AWS:Lambda:Function` want or accept your code from S3 as a zip file?
- What is a StackSet?
- Do StackSets enable you to create, update, and delete stacks across multiple accounts and regions.
- Are StackSets regional?
- Does CloudFormation have the ability to locally build, test, and debug your application?
- What is a **Change Set**?

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
- A serverless application can include one or more **nested applications**. You can deploy a nested application as a stand-alone artifact or as a component of a larger application.
  - As serverless architectures grow, common patterns emerge in which the same components are defined in multiple application templates. You can now separate out common patterns as dedicated applications, and then nest them as part of new or existing application templates. With nested applications, you can stay more focused on the business logic that's unique to your application.
  - To define a nested application in your serverless application, use the `AWS::Serverless::Application` resource type.

### Resource Types

- `AWS::Serverless::Function` resource type describes configuration information for **creating a Lambda function.** You can describe any event source that you want to attach to the Lambda function—such as Amazon S3, Amazon DynamoDB Streams, and Amazon Kinesis Data Streams.
- `AWS::Serverless::LayerVersion` resource type creates a Lambda layer version (LayerVersion) that contains library or runtime code that's needed by a Lambda function. When a serverless layer version is transformed, AWS SAM also transforms the logical ID of the resource so that old layer versions aren't automatically deleted by AWS CloudFormation when the resource is updated.
- `AWS::Serverless::Api` is incorrect because **s** It's useful for advanced use cases where you want full control and flexibility when you configure your APIs. For most scenarios, it is recommended that you create APIs by specifying this resource type as an event source of your `AWS::Serverless::Function` resource.

### CodeDeploy Integration

If you use AWS SAM to create your serverless application, **it comes built-in with CodeDeploy to help ensure safe Lambda deployments.**

- `CodeDeployDefault.LambdaCanary10Percent5Minutes`: 10 percent of your customer traffic is immediately shifted to your new version. After 5 minutes, all traffic is shifted to the new version. This means that the entire deployment time will only take 5 minutes

## AWS SAM Questions

- `AWS::Serverless::Function` is used for what?
- `AWS::Serverless::Api` is used for what?
- What does SAM stand for?
- What is AWS SAM?
- What service is AWS SAM an extension of?
- Does the AWS SAM CLI let you locally build, test, and debug serverless applications that are defined by AWS SAM templates?
- What does the AWS SAM CLI provide locally?
- What does the `sam package` command do?
- What does the `sam deploy` command do?
- What does the `sam publish` command do?
- If you're trying to deploy an application that contains one or more nested applications, what capability do you need to include in the `sam deploy` command?
- Can a serverless application include one, or more, nested applications?

---

# Cognito <a name="Cognito"></a>

Amazon Cognito provides **authentication, authorization, and user management** for your web and mobile apps.

Your users can sign in directly with a user **name and password**, or through a **third party idP** such as Facebook, Amazon, Google or Apple.

The two main components of Amazon Cognito are **user pools** and **identity pools**. User pools are user directories that provide sign-up and sign-in options for your app users. Identity pools enable you to grant your users access to other AWS services.

### An Amazon Cognito user pool and identity pool used together:

- Example: The goal is to authenticate your user, and then grant your user access to another AWS service.
  - In the first step **your app user signs in through a user pool and receives user pool tokens** after a successful authentication.
  - Next, **your app exchanges the user pool tokens for AWS credentials** through an identity pool.
  - Finally, **your app user can then use those AWS credentials to access other AWS services** such as Amazon S3 or DynamoDB.

## User Pools

- **A user pool is a _user directory_** in Amazon Cognito.
- With a user pool, your users can sign in to your web or mobile app through Amazon Cognito, or federate through a third-party identity provider (IdP).
- Whether your users sign in directly or through a third party, **all members of the user pool have a directory profile that you can access through an SDK.**

### User Pools Provide:

- Sign-up and sign-in services.
- A built-in, customizable web UI to sign in users.
- Social sign-in with Facebook, Google, Login with Amazon, and Sign in with Apple, and through SAML and OIDC identity providers from your user pool.
- User directory management and user profiles.
- Security features such as multi-factor authentication (MFA), checks for compromised credentials, account takeover protection, and phone and email verification.
- Customized workflows and user migration through AWS Lambda triggers.

## Identity Pools

- With an identity pool, **your users can obtain temporary AWS credentials to access AWS services**, such as Amazon S3 and DynamoDB.
- Identity pools support anonymous guest users.
- To save user profile information, your identity pool needs to be integrated with a user pool.

### IdPs supported by Identity Pools

- Amazon Cognito user pools.
- Social sign-in with Facebook, Google, Login with Amazon, and Sign in with Apple.
- OpenID Connect (OIDC) providers.
- SAML identity providers.
- Developer authenticated identities.

## Cognito Questions

- What three features does Cognito provide to your web and mobile apps?
- Does Cognito support usernames and passwords _and_ federated idPs?
- What are the two main components of Cognito?
- In the example provided, what is the flow to authenticate a user?
- Can a user pool be considered as a _user directory_?
- Do all members of the user pool have a directory profile that can be accessed through an SDK regardless of how they sign in?
- Do user pools provide MFA?
- Do user pools provide checks for compromised credentials?
- Do user pools provide account takeover protection?
- Do user pools provide phone verification?
- Do user pools provide email verification?
- Do your users gain _temporary or permanent_ AWS credentials through Identity pools?
- Do Identity pools support anonymous guest users?
- To save user profile information, does your identity pool need to be integrated with a user pool?

---

# X-Ray <a name="X-Ray"></a>

> X-Ray provides an end-to-end view of requests as they travel through your application, and shows a map of your application’s underlying components.

**AWS X-Ray is a service that collects data about requests that your application serves**, and provides tools that you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization.

For any _traced request_ to your application, you can see detailed information not only about the request and response, but also about **calls that your application makes to downstream AWS resources, microservices, databases, and web APIs.**

**AWS X-Ray receives traces from your application, in addition to AWS services your application uses that are already integrated with X-Ray.**

**_Instrumenting_ your application involves _sending trace data_ for incoming and outbound requests and other events within your application, along with metadata about each request.**

### X-Ray Daemon

AWS services that are integrated with X-Ray **can add tracing headers to incoming requests, send trace data to X-Ray, or run the X-Ray daemon.** For example, AWS Lambda can send trace data about requests to your Lambda functions, and run the X-Ray daemon on workers to make it simpler to use the X-Ray SDK.

Instead of sending trace data _directly to X-Ray_, **each client SDK sends JSON segment documents to a daemon process** listening for UDP traffic. The X-Ray daemon buffers segments in a queue and uploads them to X-Ray in batches. The daemon is available for **Linux**, **Windows**, and **macOS**, and is included on **AWS Elastic Beanstalk and AWS Lambda platforms**.

### X-Ray Service Graph

X-Ray uses trace data from the AWS resources that power your cloud applications to generate a detailed **service graph**.

The service graph shows the client, your frontend service, and backend services that your frontend service calls to process requests and persist data.

Use the service graph to **identify bottlenecks, latency spikes, and other issues** to solve to improve the performance of your applications.

AWS X-Ray _receives data from services_ as **segments**. X-Ray then _groups segments that have a common request_ into **traces**. X-Ray processes the traces to generate a **service graph** that provides a visual representation of your application.

### Segments

- The compute resources running your application logic send data about their work as **segments**.
- A segment provides the **resource's name**, details about the **request**, and **details about the work done**.

- For example, when an HTTP request reaches your application, it can record the following data about:
  - The **host** – hostname, alias or IP address
  - The **request** – method, client address, path, user agent
  - The **response** – status, content
  - The **work done** – start and end times, subsegments
  - **Issues** that occur – errors, faults and exceptions, including automatic capture of exception stacks.

The X-Ray SDK gathers information from **request and response headers, the code in your application, and metadata about the AWS resources on which it runs**. _You choose the data to collect_ by modifying your application configuration or code to instrument incoming requests, downstream requests, and AWS SDK clients.

_If a load balancer or other intermediary forwards a request to your application_, X-Ray takes the **client IP** from the `X-Forwarded-For` header in the request instead of from the source IP in the IP packet. **The client IP that is recorded for a forwarded request can be forged, so it should not be trusted.**

- A segment field **_cannot_** be used as a filter expression
- **Segment documents can be up to 64 kB in size.**

### Subsegments

- A segment can break down the data about the work done into **subsegments**.
- Subsegments provide more granular **timing information** and **details about downstream calls** that your application made to fulfill the original request.
- A subsegment can contain additional details about a **call to an AWS service, an external HTTP API, or a SQL database**. You can even define arbitrary subsegments to instrument specific functions or lines of code in your application.

#### Inferred Segments

- For services that don't send their own segments, _like DynamoDB_, **X-Ray uses _subsegments_ to generate _inferred segments_** and downstream nodes on the service map. **This lets you see all of your downstream dependencies, even if they don't support tracing, or are external.**
- Subsegments represent your application's view of a downstream call as a client.
- If the downstream service _is also instrumented_, **the segment that it sends replaces the inferred segment generated from the upstream client's subsegment.**
- The node on the service graph always uses information from the **service's segment, if it's available**, while _the edge between the two nodes_ **uses the upstream service's subsegment**.
  - For example, when you call DynamoDB with an instrumented AWS SDK client, the X-Ray SDK records a subsegment for that call. DynamoDB doesn't send a segment, so the inferred segment in the trace, the DynamoDB node on the service graph, and the edge between your service and DynamoDB all contain information from the subsegment.
  - When you call another instrumented service with an instrumented application, the downstream service sends its own segment to record its view of the same call that the upstream service recorded in a subsegment. In the service graph, both services' nodes contain timing and error information from those services' segments, while the edge between them contains information from the upstream service's subsegment.
  - Both viewpoints are useful, as the downstream service records precisely when it started and ended work on the request, and the upstream service records the round trip latency, including time that the request spent traveling between the two services.

### Service Graph

- X-Ray uses the data that your application sends to generate a service graph.
- Each AWS resource that sends data to X-Ray appears as a _service_ in the graph.
- **Edges** connect the services that work together to serve requests. **Edges connect clients to your application, and your application to the downstream services and resources that it uses.**
- **A service graph is a `JSON` document** that contains information about the services and resources that make up your application. The X-Ray console uses the service graph to **generate a visualization or _service map_**.
- For a distributed application, _X-Ray combines nodes from all services that process requests with **the same trace ID** into a single service graph._ The first service that the request hits adds a **tracing header** that is propagated between the frontend and services that it calls.

  - For example, Scorekeep runs a web API that calls a microservice (an AWS Lambda function) to generate a random name by using a Node.js library. The X-Ray SDK for Java generates the trace ID and includes it in calls to Lambda. **Lambda sends tracing data and passes the trace ID to the function.** The X-Ray SDK for Node.js also uses the trace ID to send data. As a result, nodes for the API, the Lambda service, and the Lambda function all appear as separate, but connected, nodes on the service map.

- **Service graph data is retained for 30 days.**

### Traces

- A **trace ID** tracks the path of a request through your application.
- **A trace collects all the segments generated by a single request.** That request is typically an HTTP `GET` or `POST` request that travels through a load balancer, hits your application code, and generates downstream calls to other AWS services or external web APIs.
- **The first supported service that the HTTP request interacts with adds a trace ID header to the request**, and propagates it downstream to track the latency, disposition, and other request data.

### Sampling

- To ensure efficient tracing and provide a representative sample of the requests that your application serves, the **X-Ray SDK applies a sampling algorithm to determine which requests get traced.**
- To avoid incurring service charges when you are getting started, the default sampling rate is **conservative**. You can configure X-Ray to modify the default sampling rule and configure additional rules that apply sampling based on properties of the service or request.
  - For example, you might want to disable sampling and trace all requests for calls that modify state or handle user accounts or transactions. For high-volume read-only calls, like background polling, health checks, or connection maintenance, you can sample at a low rate and still get enough data to see any issues that arise.

### Tracing Header

- **All requests are traced, up to a configurable minimum.**
- After reaching that minimum, **a percentage of requests are traced** to avoid unnecessary cost.
- The **sampling decision** and **trace ID** are added to `HTTP` requests in tracing headers named `X-Amzn-Trace-Id`. The **first X-Ray-integrated service that the request hits adds a tracing header**, which is read by the X-Ray SDK and included in the response.

Example Tracing header with root trace ID and sampling decision:

- `X-Amzn-Trace-Id: Root=1-5759e988-bd862e3fe1be46a994272793;Sampled=1`

- The tracing header can also contain a **parent segment ID** if the request originated from an instrumented application.
  - For example, if your application calls a downstream HTTP web API with an instrumented HTTP client, the X-Ray SDK adds the segment ID for the original request to the tracing header of the downstream request. An instrumented application that serves the downstream request can record the parent segment ID to connect the two requests.

Example Tracing header with root trace ID, parent segment ID and sampling decision

- `X-Amzn-Trace-Id: Root=1-5759e988-bd862e3fe1be46a994272793;Parent=53995c3f42cd8ad8;Sampled=1`

### Filter Expressions

- Even with sampling, a complex application generates a lot of data. The AWS X-Ray console provides an easy-to-navigate view of the service graph. It shows health and performance information that helps you identify issues and opportunities for optimization in your application.
- For advanced tracing, you can drill down to traces for individual requests, or use **filter expressions** to **find traces related to specific paths or users.**
  - example: `http.url CONTAINS "/api/user"`

### Groups

- Extending filter expressions, X-Ray also supports the group feature. **Using a filter expression, you can define criteria by which to accept traces into the group.**
- You can call the group by name or by Amazon Resource Name (ARN) to generate its own service graph, trace summaries, and Amazon CloudWatch metrics.
- Once a group is created, **_incoming traces are checked against the group’s filter expression_ as they are stored in the X-Ray service.**
- Metrics for the number of traces matching each criteria are published to CloudWatch **every minute.**
- Updating a group's filter expression doesn't change data that's already recorded. **The update applies only to subsequent traces.** This can result in a merged graph of the new and old expressions. **To avoid this, delete the current group and create a fresh one.**

### Annotations and Metadata

- When you _instrument your application_, **the X-Ray SDK records information about incoming and outgoing requests, the AWS resources used, and the application itself.**
- You can add other information to the segment document as **annotations and metadata.**
- Annotations and metadata are **aggregated at the trace level**, and can be added to any **segment or subsegment.**
- You can view annotations and metadata in the segment or subsegment details in the X-Ray console.

#### Annotations

- **Annotations are _simple key-value pairs_ that are indexed** for use with **filter expressions.**
- Use annotations to record data that you want to use to group traces in the console, or when calling the `GetTraceSummaries` API.
- X-Ray indexes up to **50 annotations per trace.**

#### Metadata

- Metadata is **key-value pairs with values of any type, _including objects and lists_**, but that are **not indexed.**
- Use metadata to record data you want to _store in the trace_ but **don't need to use for searching traces.**

### Errors, faults, and exceptions

- X-Ray tracks errors that occur in your **application code**, and **errors that are returned by downstream services**. Errors are categorized as follows.
  - **Error** – Client errors (400 series errors)
  - **Fault** – Server faults (500 series errors)
  - **Throttle** – Throttling errors (429 Too Many Requests)
- When an exception occurs while your application is serving _an instrumented request_, the X-Ray SDK records details about the exception, including the stack trace, if available. You can view exceptions under segment details in the X-Ray console.

## X-Ray Questions

- What does Amazon X-Ray collect data on?
- What utility can X-Ray provide to your application?
- **For any traced request**, you can see detailed information not only about the request and response, but what else?
- _Instrumenting_ your application involves what?
- Can AWS services integrated with X-Ray add tracing headers to incoming requests?
- Can AWS services integrated with X-Ray send trace data to the X-Ray service?
- Can AWS services integrated with X-Ray run the X-Ray daemon?
- Can AWS Lambda send trace data about your requests to your Lambda functions thus allowing the X-Ray daemon on workers have simpler X-Ray SDK integrations?
- When daemons are being used, is trace data sent directly to X-Ray?
- Do daemon processes listen for UDP traffic?
- How do daemons handle the segments they receive before sending them to X-Ray?
- Daemons sending the segments to X-Ray in what?
- What Operating Systems is the X-Ray daemon available for?
- Is the X-Ray daemon included on Elastic Beanstalk and Lambda platforms?
- Does the service graph show your frontend services?
- Does the service graph show backend services that your frontend calls to process requests and persist data?
- What usage does the service graph provide towards improving performance in your applications?
- What is a **service graph**?
- The data X-Ray receives from services is called what?
- X-Ray groups the incoming segments that have a _common request_ into what?
- The traces that X-Ray processes generate a what?
- Compute resources, running your application logic, send data about their work as what?
- What three pieces of information does a segment provide?
- When an HTTP request reaches your application, what information (5) can you capture from it?
- Can the X-Ray SDK gather information from request and response headers?
- Can the X-Ray SDK gather information about the code in your application?
- Can the X-Ray SDK gather information regarding metadata on the AWS resources on which it runs?
- Can you, the developer, choose what data the X-Ray SDK collects?
- Can a **segment field** be used as a filter expression?
- Segment documents can be up to how many kB in size?
- When a segment breaks work down into smaller sizes, the unit is called a what?
- Subsegments provide more granular what?
- What are some examples of what a subsegment can provide additional details on?
- For services that don't send their own segments, what does X-Ray use to send _inferred segments_ to downstream nodes on the service map?
- What is an example of an AWS service that doesn't send its own segments?
- Do Subsegments represent your application's view of a downstream call as a client?
- If the downstream service is instrumented, does it replace the upstream client's inferred segment with _its_ segment?
- Does the node on the service graph always use the information from the service segment if it's available?
- Does the edge _between two nodes_ use the upstream service's subsegment?
- Describe the example of the upstream and downstream relationships for the service graph in the notes.
- Every AWS resource that sends data to X-Ray appears as a _what_ in the graph?
- What do **Edges** connect?
- What type of document is a service graph?
- The X-Ray console uses the service graph to generate a visualization, what is this visualization called?
- In a distributed application, X-Ray combines all nodes _from all services the process requests_, with what same attribute, into a **single** service graph?
- The first service that a request hits gets what type of header?
- Does the tracing header get propagated between the frontend and the services that it calls? What is an example of this?
- How long is service graph data retained for?
- What does a **trace ID** track?
- What does a **trace** collect? What is this request typically?
- The first _supported_ service that the HTTP request interacts with adds what to the request?
- The trace ID header added to the request is propagated downstream to track what?
- The X-Ray SDK applies a sampling algorithm to determine which requests get traced in order to do what?
- Can you configure X-Ray to modify your default sampling rule and provide additional rules based on properties of a service or request? Describe an example of this.
- Are all requests traced, up to a configurable minimum (maximum???)?
- After reaching the tracing minimum, are a percentage of requests traced to avoid unnecessary cost?
- What **two** attributes are added to `HTTP` requests in tracing headers named `X-Amzn-Trace-Id`?
- Within the request flow, which X-Ray-instrumented service adds a tracing header?
- What does a tracing header with root trace ID and sampling decision look like?
- How can **filter expressions** be used to help with navigating through data?
- Can filter expressions be used to find traces related to specific paths?
- Can filter expressions be used to find traces related to specific users?
- What is an example of a filter expression used to find traces related to specific paths or users?
- Describe the X-Ray **group** feature.
- Can groups be called by name?
- Can groups be called by their ARN?
- Do groups have their own service graph, trace summaries, and Amazon CloudWatch metrics?
- As they are being stored in the X-Ray service, incoming traces are checked against a groups what?
- Does updating a groups filter expression change the data thats already been recorded?
- What is a **merged graph?**
- Should you delete the old group and create a new one to avoid a _merged graph_?
- You can add additional information to a segmented document as what?
- Annotations and metadata can be aggregated at what level?
- Can annotations and metadata be added to _any_ segment or subsegment?
- What are **annotations**?
- Are annotations indexed and useable with filter expressions?
- What use cases do annotations have?
- Up to how many annotations does X-ray index per trace?
- What types can metadata be?
- Can metadata be indexed?
- What use case is metadata useful for?
- Can metadata be used for searching traces?
- X-Ray tracks errors that occur where?
- What three categories are errors categorized as?

---

# CloudFront <a name="CloudFront"></a>

### Security

- For web distributions, you can configure CloudFront to require that viewers **use HTTPS to request your objects**, so connections are encrypted when CloudFront communicates with viewers.
- You can also configure CloudFront to **use HTTPS to get objects from your origin** so connections are encrypted when CloudFront communicates with your origin.
- If you configure CloudFront to require HTTPS both to communicate with viewers and to communicate with your origin, here's what happens when CloudFront receives a request for an object. The process works basically the same way whether your origin is an Amazon S3 bucket or a custom origin such as an HTTP/S server:
  - A viewer submits an HTTPS request to CloudFront. There's some SSL/TLS negotiation here between the viewer and CloudFront. In the end, the viewer submits the request in an encrypted format.
  - _If the object is in the CloudFront edge cache_, CloudFront encrypts the response and returns it to the viewer, and the viewer decrypts it.
  - If the object is not in the CloudFront cache, CloudFront performs SSL/TLS negotiation with your origin and, when the negotiation is complete, forwards the request to your origin in an encrypted format.
  - Your origin decrypts the request, encrypts the requested object, and returns the object to CloudFront.
  - CloudFront decrypts the response, re-encrypts it, and forwards the object to the viewer. CloudFront also saves the object in the edge cache so that the object is available the next time it's requested.
  - The viewer decrypts the response.
- You can configure one or more cache behaviors to **allow both HTTP and HTTPS**, so that CloudFront requires HTTPS for some objects but not for others.

---

# AWS Systems Manager <a name="AWS-Systems-Manager"></a>

---

# CloudWatch <a name="CloudWatch"></a>

## CloudWatch Alarms

- When you create an alarm, you **specify three settings** to enable CloudWatch to evaluate when to change the alarm state:
  - **Period** is the length of time to evaluate the metric or expression to create each individual data point for an alarm. It is expressed in seconds. If you choose one minute as the period, there is one datapoint every minute.
  - **Evaluation Period** is the number of the most recent periods, or data points, to evaluate when determining alarm state.
  - **Datapoints to Alarm** is the number of data points within the evaluation period that must be breaching to cause the alarm to go to the `ALARM` state. The breaching data points do not have to be consecutive, they just must all be within the last number of data points equal to **Evaluation Period**.

**High-Resolution metrics** provide _**sub-minute**_ monitoring. You can specify an alarm of 10 seconds or 30 seconds.

---

# API Gateway <a name="API-Gateway"></a>

- **Stage variables** are name-value pairs that you can define as configuration attributes associated with a deployment stage of a REST API. They act like environment variables and can be used in your API setup and mapping templates.

### Lambda Proxy Integration

Amazon API Gateway Lambda proxy integration is a simple, powerful, and nimble mechanism to build an API with a setup of a single API method. The Lambda proxy integration allows the client to call a single Lambda function in the backend. The function accesses many resources or features of other AWS services, including calling other Lambda functions.

In Lambda proxy integration, when a client submits an API request, API Gateway passes the raw request as-is to the integrated Lambda function, except that the order of the request parameters is not preserved. This request data includes the request headers, query string parameters, URL path variables, payload, and API configuration data. The configuration data can include current deployment stage name, stage variables, user identity, or authorization context (if any). The backend Lambda function parses the incoming request data to determine the response that it returns.

- For API Gateway to pass the Lambda output as the API response to the client, the Lambda function must return the result in JSON (not XML) format.
- If a Lambda function returns the result in `XML` format, it will cause `502 Bad Gateway` errors in the API Gateway.

### Caching

- A client of your API can invalidate an existing cache entry and reload it from the integration endpoint for individual requests. The client must send a request that contains the `Cache-Control: max-age=0` header.
- Ticking the `Require authorization` checkbox ensures that not every client can invalidate the API cache. If most or all of the clients invalidate the API cache, this could significantly increase the latency of your API.

---

# SQS <a name="SQS"></a>
