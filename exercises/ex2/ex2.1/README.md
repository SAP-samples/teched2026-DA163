## Exercise 2.1 Generate index recommendations with Index Advisor

After completing these steps you will have successfully generated four index recommendations for your HANA Cloud instance with Index Advisor in **Recommendation App**.

1. **Open SAP BTP Cockpit** (Skip this step if you already have it opened.)
- Navigate to [SAP BTP Cockpit](https://emea.cockpit.btp.cloud.sap/cockpit/?idp=tdct3ched2.accounts.ondemand.com).
- Login with your **_User Name_** and **_Password_**.
    - User Name: **_da163_0xx@education.cloud.sap_** (xx: your index)
    - Password: Provided in the PPT slide

2. **Access HANA Cloud Central** (Skip this step if you already have it opened.)
- Click **Instances and Subscriptions** in the leftmost navigation panel.
- Click **SAP HANA Cloud** in the Application list to open HANA Cloud Central.
<br>![](/exercises/images_general/01_00.png)
- In HANA Cloud Central, you can see your HANA Cloud instance. Click the instance and open **Overview** page.
<br>![](/exercises/images_general/01_01.png)

3. **Switch On the Index Advisor**
- Choose **Index** from **Recommendations** card.
<br>![](/exercises/ex2/images/2026/02_00.png)

- Click **Switch On** and then click **Save**.
<br>![](/exercises/ex2/images/2026/02_01.png)
<br>![](/exercises/ex2/images/2026/02_01_a.png)

4. **Generate Recommendations**
- Now the advisor is switched on. Click **Generate Recommendations**.
<br>![](/exercises/ex2/images/2026/02_02.png)

- You can see **Index** area is already selected. Click **Generate Recommendations** at the bottom.
<br>![](/exercises/ex2/images/2026/02_03.png)
There is no specific configuration you need to do to switch on the Index Advisor unlike ECN Advisor or Partition Advisor. As soon as you request a recommendation generation, the advisor will start analyzing all the schemas and tables in your instance.

- The advisor is analyzing the tables and columns to generate recommendations to create or drop index. It takes around 1-2 minutes for this exercise.
<br>![](/exercises/ex2/images/2026/02_04.png)

- Once the analysis is done and the advisor has identified tables that would benefit from creating or dropping index, there will be available recommendations generated. For this exercise, there are four recommendations generated. Two of them are create index recommendations and the other two are drop index recommendations.
<br>![](/exercises/ex2/images/2026/02_05.png)

Continue to - [Exercise 2.2 - Apply index recommendations via SQL Console](ex2.2/README.md)





