# Introduction

The primary job of a DBMS is reliably storing data and making it available for users. Databases are primary sources of data, helping us share with with different parts of our applications. Instead of finding a way to store and retrieve information and inventing new ways to organize data every time we create a new app. This way we can concentrate on application logic instead of infrastructure.

### Database Modules

- <b>Transport layer</b> accepting requests
- <b>Query processor</b> to determine the most efficient ways to run queries.
- <b>Execution engine</b> to carry out the operations
- <b>Storage engine</b>

### Storage Engine

The database engine software is responsible for storing, retrieving and managing data in-memory and on disk, designed to capture a persistent long-term memory of each node.

While databases can respond to complex queries, storage engines look at data more granularly and offer simple CRUD API.

DBMS are like applications built on top of storage engines, using their APIs to offer schemas, query language, indexing, transactions and many other features.

### Comparing Databases

The choice of database has long-term consequences. Database may not be a good fit because of performance problems, consistency issues or operational challenges and it is better to find out about them earlier in the development cycle than do a costly migration later on.

Trying to compare databases based on their components(modules above), their online ranking(arbitrary) or their implementation language can lead to invalid and premature conclusions.

Simulating real-world workloads helps you understand how the databases performs and how to operate and debug. Performance is not always the most important aspect. Better to a database that slowly saves the date, rather than one that quickly loses it.

Understand the following variables:

- Schema and record sizes
- Number of clients
- Types of queries
- Rates of the read and write
- Changes for these variables

To help you answer the questions:

- Does the database support the required types of queries?
- Is this database able to handle the amount of data to be stored?
- How many read and write operations can a single node handle?
- How many nodes should the system have?
- How to expand the cluster given the expected growth rate?
- What is the maintenance process?

You can use the database stress tools.

The ideal scenario is to be able to use the database as a black-box without having to look inside, but in practice sooner or later a bug, an outage, a performance regression or other problems will show up. At that time it's better to be prepared for it by knowing database internals, so that you can reduce business risk and improve chances for quick recovery

### Tradeoffs

Designing a storage engine is more complicated than implementing a text-book data structure. There are many details like designing physical data layout, organizing pointers, deciding on the serialization formats, understanding how data is going to be garbage collected and figure out how to make it concurrent.

tradeoff example: If we save records in the order they were inserted into the database it will store them quicker, but if we retrieve them in their lexicographical order we will have to re-sort them.
