## Exercise 2.2 Apply index recommendations via SQL Console

As you have successfully generated index recommendations, you can apply the recommendations manually via SQL Console.

1.	**Open the SQL Console and connect to your instance**
- Go to the SQL Console by clicking the command line icon ![](/exercises/ex2/images/2026/02_06.png) in the left navigation panel.

- Unless it's already connected to your instance with DBADMIN, select your instance **DA163__0XX** and click **Connect with Stored Credentials**.
<br>![](/exercises/ex2/images/2026/02_06_a.png)
<br>![](/exercises/ex2/images/2026/02_06_b.png)

2. **Check the performance before applying the recommendations**
- Delete any existing script in the console if there's any. Run the following SQL command to call the procedure and check the **Execution Elapsed Time** and **Execution CPU Time**. 
```SQL
CALL GENERATE_SELECT_WORKLOAD(10000);
```
<br>![](/exercises/ex2/images/2026/02_07.png)
You will compare the performance after applying all recommendations.

3. **Review and apply the recommendations**
- Click the instances icon ![](/exercises/ex2/images/2026/02_8.png) on the top of the navigation panel to go back to the instance list.

- Select your instance.
<br>![](/exercises/ex2/images/2026/02_09.png)

- Go to the index recommendation list and click the first recommendation to check the details.
<br>![](/exercises/ex2/images/2026/02_10.png)

- Review the details of the recommendation such as index information and copy the index SQL statement. This recommends to create an index on STATUS column in MOVIES table. Creating an index on a frequently accessed column improves query performance by allowing the database to quickly find and retrieve data without scanning the entire table. <ins>Please keep in mind that the sequence of the recommendations generated with your instance can be different from this guide.</ins>
<br>![](/exercises/ex2/images/2026/02_11.png)

- Go back to the SQL Console and paste the copied SQL statement to create index.
<br>![](/exercises/ex2/images/2026/02_12.png)

- Repeat the same task for the second recommendation which proposes to create index on GENRE column in MOVIES table.
<br>![](/exercises/ex2/images/2026/02_13.png)

- Repeat the same task for the third recommendation to drop the index on GENRE column in MOVIES2 table. It's recommended as this index has never been used. Dropping the unused index improves database performance by reducing storage overhead and speeding up write operations.
<br>![](/exercises/ex2/images/2026/02_14.png)

- Repeat it for the last recommendation to drop the index on STATUS column in MOVIES2 table.
<br>![](/exercises/ex2/images/2026/02_15.png)

- After applying all four recommendations, you can see it from the statement history as below.
<br>![](/exercises/ex2/images/2026/02_16.png)

4. **Validate the performance after applying the recommendations**

- Re-run the same SQL statement to call the procedure to check how much the performance has improved. 
```SQL
CALL GENERATE_SELECT_WORKLOAD(10000);
```

- You can see that the **Execution Elapsed Time** and **Execution CPU Time** have been significantly reduced, executed elapsed time was near two minutes before:
<br>![](/exercises/ex2/images/2026/02_17.png)


5. **Delete the existing recommendations**
- As the current Index Advisor doesn't support Apply feature, it cannot track the status of each recommendation. After you have applied the recommendations manually, the recommendations will stay as Available in the list. If you would like to clean up these recommendations, you can switch off the advisor instead.

- In the **Advisors** tab, click **Index Advisor** from the list.
<br>![](/exercises/ex2/images/2026/02_18.png)

- Click **Switch Off** on the top right side.
<br>![](/exercises/ex2/images/2026/02_19.png)

- Recommend retaining data for two weeks for your real instances, but for this exercise, choose **Remove data immediately** and click **Switch Off**.
<br>![](/exercises/ex2/images/2026/02_20.png)

- The recommendations are deleted from the list.
<br>![](/exercises/ex2/images/2026/02_21.png)

## Clean-up

**Stop your instance.**

- Go back to your Instance list.

- Click three dots icon in the Actions column and choose **Stop** from the list.

- Click **Stop Instance** in the pop-up window.

- Refresh the page to see if the instance has been stopped.

- Once it's stopped, close the HANA Cloud Central.

## Summary

You've successfully generated index recommendations for your instance and applied them manually via SQL Console.

You can find an asset about Index Advisor below.
- [Index Advisor Help Documentation](https://help.sap.com/docs/hana-cloud-database/sap-hana-cloud-sap-hana-database-administration-guide/about-index-advisor?locale=en-US)


Continue to - [Exercise 3 - Elastic Compute Node Advisor and SAP Automation Pilot](../../ex3/README.md)





