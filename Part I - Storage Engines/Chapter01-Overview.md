# Introduction

DBMS can serve different purposes. Some are used for temporary hot data, others are used for long-lived cold storage. Some allow complex analytical queries, while others allow accessing values by their keys. Some allow storing time-series data, while other store large blobs efficiently.

- Online Transactional Processing Databases (OLTP)

  - Handling large number of user facing requests and transactions. Queries are often pre-defined and short-lives.

- Online Analytical Processing Databases (OLAP)

  - Handling complex aggregations. Used for analytics and data warehousing. Capable of handling complex long-running ad hoc queries.

- Hybrid transactional and analytical processing databases (HTAP)

  - Combine properties of OLAP and OLTP

### Database Architecture

DBMS use a client/server model, where database system instances (nodes) are like servers and applications are like clients

Client requests arrive through the transport subsystem, Requests come in the form of queries written in a specified query language. The transport subsystem is also responsible for communication with other nodes.

After receiving the request, the transport subsystem hands the query to the query processor. which parses, interprets and validates it. Access control checks are performed later

The parsed query is then passed to the query optimizer, which first eliminates impossible and redundant parts of the query. Then attempts to find the most efficient way to execute it based on internal statistics (index cardinality, approximate intersection size) and data placement (which nodes hold the data and what is the cost of transfer)

The query is usually presented in the form of an execution plan. A sequence of operations that have to be carried out for the results to be considered complete

The execution plan is executed by the execution engine, which collects the results of the execution of local and remote operations. Remote execution can involve writing and reading data to and from other nodes in the cluster and replication.

### Storage engine

Local queries(coming directly from clients or from other nodes) are executed by the storage engine. The following are the components:

- Transaction manager: Schedules transactions and ensures they can't leave the database in a logically inconsistent state.
- Lock manager: Locks the database objects for the running transactions, ensuring concurrent operations do not violate physical data integrity
- Access methods (storage structures): Manage access and organizing data on disk, access methods include heap files and storage structures such as B-Trees
- Buffer manager: Caches data page in memory
- Recovery manager: Maintains the operations log and restoring the system state in case of failure

### Memory Vs Disk based DBMS

- In-memory DBMS: Store data primarily in memory and use disk for recovery and logging
- Disk-based DBMS: Holds most of the data on disk and uses memory for caching disk contents or as temp storage

### Column Vs Row oriented

- Row-oriented: Store the values of the same row together

  - Better for spatial locality (If a memory location is accessed then its nearby memory locations will be accessed soon)
  - This is great when we need to access an entire record's data, but more expensive when fetching a single column

- Column-orients: Store the values of the same column together

  - Partition the data vertically, Meaning values for the same column are stored contiguously on disk
  - Great fit for analytical workloads that compute aggregates, such as finding trends, computing averages

  ### Distinctions and optimizations

  Distinction between row and column oriented is not just in the way data is stored. Columnar stores are targeting more than just choosing the data layout, as reading multiple values for the same column in one run significantly improves cache utilization and computational efficiency. Vectorized instructions on modern CPUs can be used to process multiple data points with a single instruction.

  Compression is also an improvement when storing values that have the same data type together.

  Access patterns. If the read data is consumed in records and the workload is point queries and range scans, then go with row oriented. If scans span many rows or compute aggregate over a subset of columns, then go with column oriented.

  ### Wide Column Stores

  Wide column stores are different from column oriented databases. In wide column stores, data is represented as a multidimensional map. Columns are grouped into column families and inside each column family data is stored row-wise. This layout is best for storing data by key

  ### Data files and Index files

  The primary goal of a DBMS is to store data and allow quick access to it. Databases do use files for storing the data, but instead of relying on filesystem hierarchies of directories and files for locating records, they compose files using implementation specific formats. Here are the main reasons:

  - Storage efficiency: Files are organized in a way that minimizes storage overhead per record
  - Access efficiency: Records can be located in the least number of steps
  - Update efficiency: Record updates are performed in a way to minimize the changes on disk

  Database systems store data records, consisting of multiple fields in tables. Where each table is usually represented in a separate file. Each record in a table can be looked up using a search key. To locate records, DBMS uses indexes, which are auxiliary data structures that allow it to locate records efficiently without having to do a full scan.

  Files are partitioned into pages, which typically have the size of a single or be a multiple of disk block size. Pages can be organized as sequences of records or as slotted pages.

  New records and updates to existing records are represented by key/value pairs. Modern storage systems don't delete data from pages, but instead use deletion markers (tombstones). Which contain deletion metadata
