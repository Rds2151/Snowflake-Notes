# Snowflake Notes

- [Snowflake Notes](#snowflake-notes)
  - [Virtual warehouses](#virtual-warehouses)
  - [Overview of warehouses](#overview-of-warehouses)
  - [Warehouse size](#warehouse-size)
  - [Snowpark-optimized warehouses](#snowpark-optimized-warehouses)
    - [When to use a Snowpark-optimized warehouse](#when-to-use-a-snowpark-optimized-warehouse)

## Virtual warehouses
A virtual warehouse in Snowflake is a **cluster of compute resources** that you can use to execute queries and process data. It's called a "virtual" warehouse because you don't manage the underlying hardware or infrastructure. Instead, you create a virtual warehouse and then use it to process your data. A virtual warehouse is available in two types:

- Standard

- [Snowpark-optimized](#snowpark-optimized-warehouses)


A warehouse provides the required resources, such as CPU, memory, and temporary storage, to perform the following operations in a Snowflake session:

- Executing SQL [SELECT](https://docs.snowflake.com/en/sql-reference/sql/select) statements that require compute resources (e.g. retrieving rows from tables and views).

- Performing DML (Data Manipulation Language) operations, such as:

  - Updating rows in tables ([DELETE](https://docs.snowflake.com/en/sql-reference/sql/delete) , [INSERT](https://docs.snowflake.com/en/sql-reference/sql/insert) , [UPDATE](https://docs.snowflake.com/en/sql-reference/sql/update)).
  - Loading data into tables ([COPY INTO \<table>](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table)).
  - Unloading data from tables ([COPY INTO \<location>](https://docs.snowflake.com/en/sql-reference/sql/copy-into-location)).


## Overview of warehouses

Warehouses can be started and stopped at any time. They can also be resized at any time, even while running, to accommodate the need for more or less compute resources, based on the type of operations being performed by the warehouse.

## Warehouse size

| Warehouse Size | Credits / Hour | Credits / Second | Notes |
| :--------------: | :--------------: | :----------------: | ----- |
| X-Small | 1 | 0.0003 | Default size for warehouses created in Snowsight and using CREATE WAREHOUSE. |
| Small | 2 | 0.0006 
| Medium | 4 | 0.0011
| Large | 8 | 0.0022
| X-Large | 16 | 0.0044 |Default size for warehouses created using the Classic Console.
| 2X-Large | 32 | 0.0089
| 3X-Large | 64 | 0.0178
| 4X-Large | 128 | 0.0356
| 5X-Large | 256 | 0.0711 | Generally available in Amazon Web Services (AWS) and Microsoft Azure regions, and in preview in US Government regions.
| 6X-Large | 512 | 0.1422 | Generally available in Amazon Web Services (AWS) and Microsoft Azure regions, and in preview in US Government regions.


> Note
> 
> For a [multi-cluster warehouse](https://docs.snowflake.com/en/user-guide/warehouses-multicluster), the number of credits billed is calculated based on the warehouse size and the number of clusters that run within the time period.
> 
> For example, if a 3X-Large multi-cluster warehouse runs 1 cluster for one full hour and then runs 2 clusters for the next full hour, the total number of credits billed would be 192 (i.e. 64 + 128).
> 
> Multi-cluster warehouses are an Enterprise Edition feature


## Snowpark-optimized warehouses
Snowpark-optimized warehouses which provide **16x memory per node** compared to a standard Snowflake virtual warehouse.

### When to use a Snowpark-optimized warehouse
[Snowpark](# "Snowpark is a new feature offered by Snowflake that allows developers to use their preferred programming languages to build, optimize, and execute data workloads within Snowflake. Snowpark provides a unified API that abstracts away the complexities of SQL, allowing you to write code that is more readable, maintainable, and reusable.") workloads can be run on both Standard and Snowpark-optimized warehouses. Snowpark-optimized warehouses are recommended for workloads that have large memory requirements such as ML training use cases using a stored procedure on a single virtual warehouse node. Initial creation and resumption of a Snowpark-optimized virtual warehouse may take longer than standard warehouses. Additionally, Snowpark workloads, utilizing [UDF](# "You can write user-defined functions (UDFs) to extend the system to perform operations that are not available through the built-in system-defined functions provided by Snowflake. Once you create a UDF, you can reuse it multiple times.") or [UDTF](# "A UDTF is a user-defined function (UDF) that returns tabular results."), may also benefit from Snowpark-optimized warehouses.

Creating a Snowpark-optimized warehouse
Use the `warehouse_type` property in the [CREATE WAREHOUSE](https://docs.snowflake.com/en/sql-reference/sql/create-warehouse) command to create a new Snowpark-optimized warehouse.

- Create a new Snowpark-optimized warehouse snowpark_opt_wh:
  ```sql
  CREATE OR REPLACE WAREHOUSE snowpark_opt_wh WITH
    WAREHOUSE_SIZE = 'MEDIUM'
    WAREHOUSE_TYPE = 'SNOWPARK-OPTIMIZED';
  ```
---

![data warehouse](https://www.snowflake.com/wp-content/uploads/2018/07/architecture-imageg4.png)

