---
title: "DAT329 - DynamoDB Schema Design"
date: 2023-11-27T08:30:00-07:00
draft: false
---

## Schema Design

* Primary keys are a Primary Key and an optional Sort Key.
* Global Secondary Index is just another index consisting of a Primary Key and an optional Sort Key.
* Always include primary index attributes in GSIs so you can do a reverse lookup with your primary key.

## ECommerce Platform Example

In this example, we seem to be designing a Dynamo schema for a particular application.

1. Define access patterns to inform schema design. e.g. Customer adds products to their shopping cart. e.g. Company needs to see unfulfilled orders (orthogonal requirement to customer requirement).
2. Consider the cardinality of each attribute. Customers=high, products=medium
3. How should I organize the data to meet the specific needs of the application? i.e. How are the data retrievers going to query the data? Need to know this _ahead of time!_. "It's like the foundation of the house."
4. Multi-table design. Joining work must happen on the client. Not atomic because you make separate query to each table. Benefit is targeted access -- you aren't accessing data you don't need (unless you are).
5. Single-table design. This should be the default, probably. Monitoring is easier, queries are faster (only one table). 

## Partition Keys

1. Higher cardinality the better (high level of uniqueness).
2. Think about who "owns" the data. Who cares about it? e.g. for a customer's shopping cart, the customer ID is a good partition key for their shopping cart, even though it's in a different table.
3. Make sure your DDB record doesn't grow over time.
4. One-to-many relationship: built into a single table. e.g. in the Orders table, each order is PK'd with the Customer ID and ordered on order time.
5. Composite sort keys can be lexicographically queried using `SK_BEGINS_WITH("prefix")`


## Cost Optimization

* 1KB = 1 Write Capacity Unit (WCU)
* A few hundred KB is the point at which you will start to see performance issues with querying. e.g. DDB throttles above 1000WCUs, so writing 3 x 400KB items causes throttling (1200 WCUs).
* Updating a single attribute of a large item requires WCUs sufficient to write the entire new item size.
* Break content into static/dynamic. Static content can live in bigger objects. Dynamic content (frequently updated) should live in small records to save WCUs. This is known as vertical partitioning.

## Filtering Data

Three ways:
1. Use partition/sort key
2. Use keys on a secondary index (local or global)
3. Use a scan with filter expression (more computationally expensive). Basically never do this.

### Global Secondary Indexes

* GSIs are basically duplicates of your table, partitioned differently.
* Downsides of GSIs:
    * additional storage cost (essentially a full replica per GSI)
    * Downside: Write contention (aka Hot Keys) results in throttling
    * Downside: Consistency -- GSIs are _eventually_ consistent.
* Sparse Secondary Indexes limit updates to only when the sort key is present in the item. If the item is missing either the partition or sort key, the index is "sparse". [See documentation here](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-indexes-general-sparse-indexes.html)

### Local Secondary Indexes

From the AWS docs:

    Every local secondary index must meet the following conditions:

    - The partition key is the same as that of its base table.
    - The sort key consists of exactly one scalar attribute.
    - The sort key of the base table is projected into the index, where it acts as a non-key attribute.


