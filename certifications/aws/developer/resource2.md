# DynamoDB

## Core Components

In DynamoDB, tables, items, and attributes are the core components that you work with. A table is a collection of items, and each item is a collection of attributes. DynamoDB uses primary keys to uniquely identify each item in a table and secondary indexes to provide more querying flexibility. You can use DynamoDB Streams to capture data modification events in DynamoDB tables.

### Tables, Items, and Attributes

- **Tables** – Similar to other database systems, DynamoDB stores data in tables. A table is a collection of data.

- **Items** – Each table contains zero or more items. An item is a group of attributes that is uniquely identifiable among all of the other items.

- **Attributes** – Each item is composed of one or more attributes. An attribute is a fundamental data element, something that does not need to be broken down any further.

Each item in the table has a unique identifier, or primary key, that distinguishes the item from all of the others in the table.

Other than the primary key, the People table is schemaless, which means that neither the attributes nor their data types need to be defined beforehand. Each item can have its own distinct attributes.

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

A composite primary key gives you additional flexibility when querying data. For example, if you provide only the value for Artist, DynamoDB retrieves all of the songs by that artist. To retrieve only a subset of songs by a particular artist, you can provide a value for Artist along with a range of values for SongTitle.

> The partition key of an item is also known as its **hash attribute**. The term hash attribute derives from the use of an internal hash function in DynamoDB that evenly distributes data items across partitions, based on their partition key values.

> The sort key of an item is also known as its **range attribute**. The term range attribute derives from the way DynamoDB stores items with the same partition key physically close together, in sorted order by the sort key value.

Each primary key attribute must be a scalar (meaning that it can hold only a single value). The only data types allowed for primary key attributes are string, number, or binary. There are no such restrictions for other, non-key attributes.

## Secondary Indexes

You can create one or more secondary indexes on a table.

A secondary index lets you query the data in the table using an alternate key, in addition to queries against the primary key.

DynamoDB doesn't require that you use indexes, but they give your applications more flexibility when querying your data.

#### **DynamoDB supports two kinds of indexes:**

- **Global secondary index** – An index with a partition key and sort key that can be different from those on the table.
- **Local secondary index** – An index that has the same partition key as the table, but a different sort key.

Each table in DynamoDB has a quota of **20 global secondary indexes (default quota) and 5 local secondary indexes.**

In the example `Music` table shown previously, you can query data items by `Artist` (partition key) or by `Artist` and `SongTitle` (partition key and sort key). What if you also wanted to query the data by Genre and `AlbumTitle`? To do this, you could create an index on `Genre` and `AlbumTitle`, and then query the index in much the same way as you'd query the `Music` table.

- Every index belongs to a table, which is called the _base table_ for the index. In the preceding example, Music is the base table for the GenreAlbumTitle index.

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

You can use PartiQL - A SQL-Compatible Query Language for Amazon DynamoDB, to perform these CRUD operations or you can use DynamoDB’s classic CRUD APIs that separates each operation into a distinct API call.

#### Creating Data

- `PutItem`: Retrieves a single item from a table.
- `BatchWriteItem`: **Writes up to 25 items to a table.** This is more efficient than calling PutItem multiple times because your application only needs a single network round trip to write the items. _You can also use BatchWriteItem for deleting multiple items from one or more tables._

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

The document types are list and map. **These data types can be nested within each other**, to represent complex data structures **up to 32 levels deep.**

There is no limit on the number of values in a list or a map, as long as the item containing the **values fits within the DynamoDB item size limit (400 KB).**

#### List

A list type attribute can store an ordered collection of values. Lists are enclosed in square brackets: `[ ... ]` A list is similar to a JSON array.

- `FavoriteThings: ["Cookies", "Coffee", 3.14159]`

#### Map

A map type attribute can store an unordered collection of name-value pairs. Maps are enclosed in curly braces: `{ ... }` A map is similar to a JSON object.

- Maps are ideal for storing JSON documents in DynamoDB.

#### Sets

DynamoDB supports types that represent sets of number, string, or binary values. **All the elements within a set must be of the same type.**

There is no limit on the number of values in a set, as long as the item containing the values fits within the DynamoDB item size limit (400 KB).

**Each value within a set must be unique.** **The order of the values within a set is not preserved. **Therefore, your applications must not rely on any particular order of elements within the set. DynamoDB does not support empty sets, however, empty string and binary values are allowed within a set.

- `["Black", "Green", "Red"]`

- `[42.2, -19, 7.5, 3.14]`

- `["U3Vubnk=", "UmFpbnk=", "U25vd3k="]`

## Read Consistency

Amazon DynamoDB is available in multiple AWS Regions around the world. **Each Region is independent and isolated from other AWS Regions.** For example, if you have a table called `People` in the `us-east-2` Region and another table named `People` in the `us-west-2` Region, these are considered two entirely separate tables.

When your application writes data to a DynamoDB table and receives an **HTTP 200 response (OK)**, the write has occurred and is durable. The data is eventually consistent across all storage locations, usually within one second or less.

### Eventually Consistent Read

When you read data from a DynamoDB table, the response might not reflect the results of a recently completed write operation. The response might include some stale data. If you repeat your read request after a short time, the response should return the latest data.

### Strongly Consistent Read

When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful. However, this consistency comes with some disadvantages:

- A strongly consistent read might not be available if there is a network delay or outage. In this case, DynamoDB may return a server error (HTTP 500).
- Strongly consistent reads may have higher latency than eventually consistent reads.
- **Strongly consistent reads are not supported on global secondary indexes.**
- Strongly consistent reads use more throughput capacity than eventually consistent reads. For details, see Read/Write Capacity Mode.

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

When you switch a table from provisioned capacity mode to on-demand capacity mode, DynamoDB makes several changes to the structure of your table and partitions. This process can take several minutes. During the switching period, your table delivers throughput that is consistent with the previously provisioned write capacity unit and read capacity unit amounts.

### Provisioned Mode

If you choose provisioned mode, you specify the number of reads and writes per second that you require for your application. You can use auto scaling to adjust your table’s provisioned capacity automatically in response to traffic changes. This helps you govern your DynamoDB use to stay at or below a defined request rate in order to obtain cost predictability.

**Provisioned mode is a good option if any of the following are true:**

- You have predictable application traffic.
- You run applications whose traffic is consistent or ramps gradually.
- You can forecast capacity requirements to control costs.

#### RCU and WCU

If your application reads or writes larger items (up to the DynamoDB maximum item size of 400 KB), it will consume more capacity units.

For example, suppose that you create a provisioned table with 6 read capacity units and 6 write capacity units. With these settings, your application could do the following:

- Perform strongly consistent reads of up to 24 KB per second (4 KB × 6 read capacity units).
- Perform eventually consistent reads of up to 48 KB per second (twice as much read throughput).
- Perform transactional read requests of up to 12 KB per second.
- Write up to 6 KB per second (1 KB × 6 write capacity units).
- Perform transactional write requests of up to 3 KB per second.

#### Throttling

_Provisioned throughput_ is the maximum amount of capacity that an application can consume from a table or index. If your application exceeds your provisioned throughput capacity on a table or index, it is subject to request throttling.

**Throttling prevents your application from consuming too many capacity units.** When a request is throttled, **it fails with an HTTP 400 code (Bad Request)** and a `ProvisionedThroughputExceededException`. **The AWS SDKs have built-in support for retrying throttled requests (see Error Retries and Exponential Backoff)**, so you do not need to write this logic yourself.

#### DynamoDB Auto Scaling

**DynamoDB auto scaling actively manages throughput capacity for tables and global secondary indexes.** With auto scaling, you define a range (upper and lower limits) for read and write capacity units. You also define a target utilization percentage within that range.

With DynamoDB auto scaling, a table or a global secondary index can increase its provisioned read and write capacity to handle sudden increases in traffic, without request throttling. When the workload decreases, DynamoDB auto scaling can decrease the throughput so that you don't pay for unused provisioned capacity.

#### Reserved Capacity

As a DynamoDB customer, you can purchase reserved capacity in advance for tables that use the DynamoDB Standard table class, as described at Amazon DynamoDB Pricing.

With reserved capacity, you pay a one-time upfront fee and commit to a minimum provisioned usage level over a period of time.

Your reserved capacity is billed at the hourly reserved capacity rate. By reserving your read and write capacity units ahead of time, you realize significant cost savings on your provisioned capacity costs. Any capacity that you provision in excess of your reserved capacity is billed at standard provisioned capacity rates.

> Reserved capacity is not available for replicated write capacity units. Reserved capacity is also not available for tables using the DynamoDB Standard-IA table class or on-demand capacity mode.

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

You can read multiple items from the table in a single operation (`Query`) if the items you want have the same partition key value. DynamoDB returns all of the items with that partition key value. Optionally, you can apply a condition to the sort key so that it returns only the items within a certain range of values.

To read all of the items with an `AnimalType` of _Dog_, you can issue a Query operation without specifying a sort key condition. By default, the items are returned in the order that they are stored (that is, in ascending order by sort key). Optionally, you can request descending order instead.
