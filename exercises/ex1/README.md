# Exercise 1 - Partition Advisor

In this exercise, you will generate partitioning recommendations for a specific schema and apply them using **Recommendation App** in **HANA Cloud Central**. You will then verify that all recommendations have been applied correctly.

## What is Partition Advisor?

Partitioning divides a table so that each partition stores a distinct subset of rows. Beyond overcoming the 2-billion-record limit per partition, well-designed partitions enable several performance benefits: load balancing across hosts, parallel query execution, partition pruning (both static and dynamic) to skip irrelevant data scans, faster and more targeted delta merges, and improved compression ratios.

However, excessive fragmentation into small partitions introduces overhead. To address this, the **Partition Advisor** analyzes table sizes, data distribution patterns, and workload characteristics to identify tables or partitions exceeding configurable row-count thresholds. It then recommends optimized partitioning specifications that maximize performance while minimizing maintenance costs and ensuring each partition remains within size limits. The partitioning decisions focus on the following points: 
- choosing an effective partition key based on access patterns and cardinality
- choosing the right partitioning method (HASH or RANGE, or multilevel partitioning)

The Partition Advisor also offers an **apply function** that allows users to easily implement the partitioning recommendations.

## Dataset Information and Preparation

For this hands-on exercise, **five exemplifying scenarios** have been designed. You can find the table information for these five scenarios below.
- **T_SCENARIO_1**: Full unpartitioned table with primary key
- **T_SCENARIO_2**: Unpartitioned table with high data usage and complex query patterns
- **T_SCENARIO_3**: Unpartitioned time-based table with high access patterns
- **T_SCENARIO_4**: Range partitioned table with a full 'OTHERS' partition
- **T_SCENARIO_5**: Hash-hash partitioned table with some full partitions

The minimum rows for repartitioning, repartitioning threshold, and initial partition values were set to artificially low levels to facilitate demonstration scenarios with minimal data and reduced workload. Refer to the [table placement rule](https://help.sap.com/docs/hana-cloud-database/sap-hana-cloud-sap-hana-database-administration-guide/table-placement-rules?locale=en-US) for detailed parameter guide.

**T_SCENARIO_1**, **T_SCENARIO_4**, and **T_SCENARIO_5** have been already set up in your instance. You need to create the other two tables, **T_SCENARIO_2** and **T_SCENARIO_3** before starting the Exercise 1.1.

Please copy the following script and run it in the **SQL Console** in HANA Cloud Central as instructed at the beginning of the workshop. You will learn more about each table in the Exercise 1.2.

**SQL Script**
```SQL

------- T_SCENARIO_2 script -------

CONNECT DBADMIN PASSWORD Welcome1;
SET SCHEMA PA_DEMO;

ALTER SYSTEM ALTER TABLE PLACEMENT (SCHEMA_NAME=>'PA_DEMO', TABLE_NAME=>'T_SCENARIO_2')
SET (MIN_ROWS_FOR_PARTITIONING=>100, REPARTITIONING_THRESHOLD=>10000, INITIAL_PARTITIONS=>3);

CREATE COLUMN TABLE T_SCENARIO_2(
        EVENT_ID INTEGER,
        CUSTOMER_ID INTEGER NOT NULL,
        EVENT_TYPE VARCHAR(20) NOT NULL,
        EVENT_TIME DATE);

INSERT INTO T_SCENARIO_2
        SELECT
            s.ELEMENT_NUMBER AS EVENT_ID,
            MOD(s.ELEMENT_NUMBER, 200) AS CUSTOMER_ID,
            CASE
                WHEN MOD(s.ELEMENT_NUMBER, 10) < 5 THEN 'LOGIN'
                WHEN MOD(s.ELEMENT_NUMBER, 10) < 7 THEN 'PURCHASE'
                WHEN MOD(s.ELEMENT_NUMBER, 10) < 8 THEN 'SUBSCRIBE'
                WHEN MOD(s.ELEMENT_NUMBER, 10) = 8 THEN 'LOGOUT'
                ELSE 'DELETE'
            END AS EVENT_TYPE,
            ADD_DAYS(DATE'2023-01-01', MOD(s.ELEMENT_NUMBER, 180)) AS EVENT_TIME
        FROM SERIES_GENERATE_INTEGER(1, 1, 300) s;

-- Simulate frequent access pattern
DO
BEGIN
  DECLARE i INT = 0;
  WHILE :i < 100 DO
    EXECUTE IMMEDIATE '
      SELECT * FROM T_SCENARIO_2
      WHERE EVENT_ID > 9 OR EVENT_TYPE = ''LOGIN''
    ';
    i = :i + 1;
  END WHILE;
END;

------- T_SCENARIO_3 script -------

ALTER SYSTEM ALTER TABLE PLACEMENT (SCHEMA_NAME=>'PA_DEMO', TABLE_NAME=>'T_SCENARIO_3')
SET (MIN_ROWS_FOR_PARTITIONING=>100, REPARTITIONING_THRESHOLD=>10000, INITIAL_PARTITIONS=>3);

CREATE COLUMN TABLE T_SCENARIO_3 (
    EVENT_ID INTEGER NOT NULL,
    EVENT_DATE DATE NOT NULL,
    DESCRIPTION VARCHAR(50) NOT NULL
);

INSERT INTO T_SCENARIO_3
SELECT
    ELEMENT_NUMBER,
    ADD_DAYS(DATE'2019-01-01', MOD(ELEMENT_NUMBER, 2190)),
    'Event ' || ELEMENT_NUMBER
FROM SERIES_GENERATE_INTEGER(1, 1, 3600);

-- Simulate frequent access pattern
DO
BEGIN
  DECLARE i INT = 0;
  WHILE :i < 100 DO
    EXECUTE IMMEDIATE '
      SELECT * FROM T_SCENARIO_3
      WHERE EVENT_DATE > DATE''2023-01-01''
    ';
    i = :i + 1;
  END WHILE;
END;
```

Now you are ready with all the datasets. 

Continue to - [Exercise 1.1 - Generate partitioning recommendations with Partition Advisor](ex1.1/README.md)

