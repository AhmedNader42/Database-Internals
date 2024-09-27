# Introduction

DBMS can serve different purposes. Some are used for temporary hot data, others are used for long-lived cold storage. Some allow complex analytical queries, while others allow accessing values by their keys. Some allow storing time-series data, while other store large blobs efficiently.

- Online Transactional Processing Databases (OLTP)

  - Handling large number of user facing requests and transactions. Queries are often pre-defined and short-lives.

- Online Analytical Processing Databases (OLAP)

  - Handling complex aggregations. Used for analytics and data warehousing. Capable of handling complex long-running ad hoc queries.

- Hybrid transactional and analytical processing databases (HTAP)

  - Combine properties of OLAP and OLTP

### Database Architecture
