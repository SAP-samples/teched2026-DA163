## Exercise 1.2 Apply partitioning recommendations from Recommendation App

After completing these steps, you will have successfully applied all partitioning recommendations from the Recommendation App and validated that the partitioning actions have been implemented as suggested.

### Scenario 1: Full unpartitioned table with primary key

This scenario has set aggressive partitioning parameters for the table:
- MIN_ROWS_FOR PARTITIONING: 100
- REPARTITIONING_THRESHOLD: 100 (very low threshold)
- INITIAL_PARTITIONS: 3

1. **Review recommendation details for the table T_SCENARIO_1**
<br>![](/exercises/ex1/images/2026/01_14a.png)
<br>![](/exercises/ex1/images/2026/01_14b.png)
<br>![](/exercises/ex1/images/2026/01_14c.png)
The advisor recommends **hash partitioning** on the primary key, **ID**, to resolve the full table issue.

2. **Check current partitioning status** 
- Before applying the recommendation, verify the current partitioning state of table 'T_SCENARIO_1'.

- Open the **SQL Console** for your instance.
<br>![](/exercises/ex1/images/2026/01_15.png)

- Execute the following SQL script to check the current partitioning.
```SQL
SELECT TABLE_NAME, PARTITION_DEFINITION FROM PARTITIONED_TABLES WHERE TABLE_NAME='T_SCENARIO_1';
```
- The query returns no results, confirming that the table is currently unpartitioned.
<br>![](/exercises/ex1/images/2026/01_16.png)

3. **Apply the recommendation**
- Click **Apply** in the recommendation detail page.
<br>![](/exercises/ex1/images/2026/01_17.png)
- You can monitor the advisor applying the recommendation in the **Processes** tab.
<br>![](/exercises/ex1/images/2026/01_18.png)

4. **Validate partitioning implementation**
- Re-run the same SQL script to confirm the partitioning has been applied.
<br>![](/exercises/ex1/images/2026/01_19.png)
The table has been successfully hash partitioned into 12 partitions on the primary key to distribute data evenly across multiple partitions and improve query performance by avoiding full table scans.


### Scenario 2: Unpartitioned table with high data usage and complex query patterns

This scenario has simulated heavy query load by executing the same query 100 times, creating a usage pattern where a specific column is frequently queried.

1. **Review recommendation details for the table T_SCENARIO_2**
<br>![](/exercises/ex1/images/2026/01_20.png)
The advisor identifies the 'EVENT_TYPE' column as the most frequently queried column and recommends hash partitioning on this column for optimal performance.

2. **Check current partitioning status** 
- Verify the current partitioning state using the same approach as Scenario 1.

- Make sure to be connected with DBADMIN.

- Run the following SQL script to check that the table is unpartitioned.
```SQL
SELECT TABLE_NAME, PARTITION_DEFINITION FROM PARTITIONED_TABLES WHERE TABLE_NAME='T_SCENARIO_2';
```

3. **Apply the recommendation**
- Click **Apply** in the recommendation detail page, in the same way you did for scenari 1.
<br>![](/exercises/ex1/images/2026/01_21.png)

4. **Validate partitioning implementation**
- Re-run the same SQL script to confirm the partitioning has been applied.
<br>![](/exercises/ex1/images/2026/01_22.png)
The table has been successfully hash-partitioned into 3 partitions to improve query performance by distributing data based on the most-accessed column.

### Scenario 3: Unpartitioned time-based table with high access patterns

This scenario has simulated a time-based table with data spanning from 2019 to 2025, creating a historical dataset where recent "hot" data (2023 onwards) is frequently accessed, while older data (2019-2022) remains relatively unused.

1. **Review recommendation details for the table T_SCENARIO_3**
<br>![](/exercises/ex1/images/2026/01_23.png)
The advisor suggests range partitioning to isolate the hot data (2023 onwards) in a single partition.

2. **Check current partitioning status** 
- Verify the current partitioning state using the same approach as Scenario 1.

- Run the following SQL script to check that the table is unpartitioned.
```SQL
SELECT TABLE_NAME, PARTITION_DEFINITION FROM PARTITIONED_TABLES WHERE TABLE_NAME='T_SCENARIO_3';
```

3. **Apply the recommendation**
- Click **Apply** in the recommendation detail page.

4. **Validate partitioning implementation**
- Re-run the same SQL script to confirm the partitioning has been applied.
<br>![](/exercises/ex1/images/2026/01_24.png)
The table has been successfully range-partitioned into 3 partitions, isolating frequently accessed recent data in a dedicated partition and improving query performance by avoiding scans of older, unused data.


### Scenario 4: Range partitioned table with full 'OTHERS' partition 

This scenario has created a range-partitioned table with a problematic 'OTHERS' partition.

1. **Review recommendation details for the table T_SCENARIO_4**
<br>![](/exercises/ex1/images/2026/01_25.png)
The advisor recommends splitting the partition into more proper range-based partitions.

2. **Check current partitioning status** 
- To verify the current partitioning state of table 'T_SCENARIO_4', repeat the same steps from Scenario 1.

- Run the following SQL script to check the current partitioning state.
```SQL
SELECT TABLE_NAME, PARTITION_DEFINITION FROM PARTITIONED_TABLES WHERE TABLE_NAME='T_SCENARIO_4';
```
- The table is divided into partitions based on ranges of values in the 'O_ORDERKEY' column, but there are no explicit ranges defined, so all rows with any 'O_ORDERKEY' value are stored in the 'OTHERS' partition.
<br>![](/exercises/ex1/images/2026/01_26.png)

- Check the performance by running the following query on a specific range of order keys before applying the recommendation.
```SQL
SET SCHEMA PA_DEMO;
SELECT * FROM T_SCENARIO_4 WHERE O_ORDERKEY > 1000000 AND O_ORDERKEY < 1200000;
```

- Check **Execution Elapsed Time** and **Execution CPU Time** in the **Messages** tab.
<br>![](/exercises/ex1/images/2026/01_26_a.png)

3. **Apply the recommendation**
- Click **Apply** in the recommendation detail page.

- Check the applied recommendation by adding **Applied** in the **Status** filter.
<br>![](/exercises/ex1/images/2026/01_27.png)

4. **Validate partitioning implementation**
- Re-run the same SQL script to confirm the partitioning has been applied. The oversized 'OTHERS' partition has been split into proper range-based partitions to improve query performance for range-based queries.
<br>![](/exercises/ex1/images/2026/01_26_b.png)

- Check the performance with the same SQL script and confirm the performance improvement compared to before applying the recommendation.
```SQL
SET SCHEMA PA_DEMO;
SELECT * FROM T_SCENARIO_4 WHERE O_ORDERKEY > 1000000 AND O_ORDERKEY < 1200000;
```

- You can see that the **Statement execute time** have been reduced significantly.
<br>![](/exercises/ex1/images/2026/01_26_c.png)

### Scenario 5: Hash-hash partitioned table with some full partitions

This scenario has created an imbalanced multi-level hash partitioned table. The hash  partitioning on 'REGION' doesn't distribute data evenly, causing some partitions to become full while others remain relatively empty.

1. **Review recommendation details for the table T_SCENARIO_5**
<br>![](/exercises/ex1/images/2026/01_28_a.png)
<br>![](/exercises/ex1/images/2026/01_28_b.png)
The advisor proposes to increase the number of leaf partitions while keeping the partitioning hierarchy balanced.

2. **Check current partitioning status** 
- Check the current partitioning state of table 'T_SCENARIO_5' by repeating the same steps from Scenario 1.
```SQL
SELECT TABLE_NAME, PARTITION_DEFINITION FROM PARTITIONED_TABLES WHERE TABLE_NAME='T_SCENARIO_5';
```
- The table currently has a two-level partitioning scheme: the first level is hash partitioned with the 'REGION' value into 2 partitions, and the second level is hash subpartitioned with the 'CUSTOMER_ID' into 2 partitions.
<br>![](/exercises/ex1/images/2026/01_29.png)

3. **Apply the recommendation**
- Click **Apply** in the recommendation detail page.

4. **Validate partitioning implementation**
- Re-run the same SQL script to confirm the partitioning has been applied.
<br>![](/exercises/ex1/images/2026/01_30.png)
The table now has 16 leaf partitions to achieve better data distribution while maintaining partitioning hierarchy balance.

## Clean-up

**Switch off Partition Advisor**

- Go to **Advisors** tab in the Recommendation App.

- Click the three dots on the top right side and select **Switch Off**.

- Choose **Remove data immediately** and click **Switch Off**.

The Partition Advisor is off and all recommendation data has been deleted.
You can go back to **Instance Overview** page.

## Summary

You've successfully generated partitioning recommendations for your selected schema(s) and table(s) and applied them easily with a simple click of **Apply** from the **Recommendation App**. 

You can find an asset about Partition Advisor below.
- [Partition Advisor Help Documentation](https://help.sap.com/docs/hana-cloud-database/sap-hana-cloud-sap-hana-database-administration-guide/about-partition-advisor?locale=en-US)


Continue to - [Exercise 2 - Index Advisor](../../ex2/README.md)

