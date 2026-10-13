## Exercise 1.1 Generate partitioning recommendations with Partition Advisor

After completing these steps you will have successfully generated five partitioning recommendations for your HANA Cloud instance in **Recommendation App**.

1. **Open SAP BTP Cockpit** (Skip this step if you already have it opened.)
- Navigate to [SAP BTP Cockpit](https://emea.cockpit.btp.cloud.sap/cockpit/?idp=tdct3ched2.accounts.ondemand.com).
- Login with your **_User Name_** and **_Password_**.
    - User Name: **_DA163-0xx@education.cloud.sap_** (xx: your index)
    - Password: **Acce$$teched26**

2. **Access HANA Cloud Central** (Skip this step if you already have it opened.)
- Click **Instances and Subscriptions** in the leftmost navigation panel.
- Click **SAP HANA Cloud** in the Application list to open HANA Cloud Central.
<br>![](/exercises/images_general/01_00.png)
- In HANA Cloud Central, you can see your HANA Cloud instance. Click the instance and open **Overview** page.
<br>![](/exercises/images_general/01_01.png)


3. **Switch On the Partition Advisor**
- Choose **Partition** from **Recommendations** card.
<br>![](/exercises/ex1/images/2026/01_01.png)

- You need a dedicated **Partition Advisor User** to use the Partition Advisor. You can find more details about the **Partition Advisor User** in the [Getting Started](/exercises/ex0/) section. The **Partition Advisor User** has been already created for this exercise. Add the following credential and click **Save**.

  - Username: _**PAUSER**_
  - Password: _**Partitionadvisor1**_
<br>![](/exercises/ex1/images/2026/01_02.png)


4. **Generate Recommendations**
- The Partition Advisor is **On** and you can view the **Username**. Click **Generate Recommendations**.
<br>![](/exercises/ex1/images/2026/01_04.png)

- Click **Add** to select target object(s) that you want the advisor to analyze against. 
<br>![](/exercises/ex1/images/2026/01_05.png)

- You can choose **Object Type** between **Schema** or **Table**. Choose **Table** here and click **Schema** field to show a list of schema that the **Partition Advisor User** can view.
<br>![](/exercises/ex1/images/02_06.png)

- Select the schema, named **PA_DEMO**. Then you can view the 5 tables in the schema. Click **Add**.
<br>![](/exercises/ex1/images/2026/01_07.png)
<br>![](/exercises/ex1/images/2026/01_08.png)

- The target objects are added in the list. Click **Generate Recommendation**.
<br>![](/exercises/ex1/images/2026/01_09.png)

- The advisor is generating recommendations for the selected objects.
<br>![](/exercises/ex1/images/2026/01_10.png)

- Click **Refresh** icon on the top right side to see the progress updated.
<br>![](/exercises/ex1/images/2026/01_11.png)

- The recommendations have been generated and the **Status** has changed to **Executed**. Go to **Recommendations** tab to view the available recommendations.
<br>![](/exercises/ex1/images/2026/01_12.png)

- Here you can find the list of available recommendations for all 5 scenarios.
<br>![](/exercises/ex1/images/2026/01_13.png)

Continue to - [Exercise 1.2 - Apply partitioning recommendations from Recommendation App](../ex1.2/README.md)
