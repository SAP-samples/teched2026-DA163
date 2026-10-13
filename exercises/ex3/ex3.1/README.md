## Exercise 3.1 Configure ECN Advisor and generate an ECN recommendation

After completing these steps you will have successfully generated an ECN recommendation for your HANA Cloud instance from HANA Cloud Central.

1. **Open SAP BTP Cockpit**
- Navigate to [SAP BTP Cockpit](https://emea.cockpit.btp.cloud.sap/cockpit/?idp=tdct3ched2.accounts.ondemand.com).
- Login with your **_User Name_** and **_Password_**.
    - User Name: **_da263-0xx@education.cloud.sap_** (xx: your index)
    - Password: **_Provided in the PPT slide_**


2. **Access HANA Cloud Central**
- Click **Instances and Subscriptions** in the leftmost navigation panel.
- Click **SAP HANA Cloud** in the Application list to open HANA Cloud Central.
<br>![](/exercises/images_general/01_00.png)
- In HANA Cloud Central, you can see your HANA Cloud instance.
<br>![](/exercises/images_general/01_01.png)


3. **Check Resource Consumption**
- When you click your HANA Cloud instance, you see an **Overview** page with many different tiles.
<br>![](/exercises/ex3/images/01_01_01.png)

- Click **Compute** card.

- When you choose **24 Hours** on top, you will see some compute spikes occurred.
<br>![](/exercises/ex3/images/2026/03_17.png)


4. **Switch on ECN Advisor**
- Go back to **Overview** page by clicking your instance name on top next to Usage Monitor.

- Find a **Recommendations** card and click **Elastic Compute Node**.
<br>![](/exercises/ex3/images/2026/03_02.png)  


- Click **Switch On**. You can see the resource usage over the past few days in the chart.
<br>![](/exercises/ex3/images/2026/03_03.png)  


- Add **Thresholds** for Memory and Compute. The default value is **80%**, and you can add the thresholds that are suitable for your use case. In this exercise, we will set it as **_70%_** for both **Memory** and **Compute**.
<br>![](/exercises/ex3/images/2026/03_04.png)
<br> Now the ECN Advisor is **switched on**.  


5. **Generate Recommendations**
- Click **Generate Recommendations**.
<br>![](/exercises/ex3/images/2026/03_05.png)  


- You can see a recommendation topic selected as **Elastic Compute Node**. Define the **Analysis Timeframe** that the advisor will analyze your resource usage to generate a recommendation. The time frame must be <u>*between 30 minutes and 24 hours*</u> in length. It must end <u>*at least 20 minutes before*</u> the recommendation request, and must be <u>*within the last 14 days*</u>. 
In this exercise, you keep it as 1 day as it has been already selected by default. Click **Generate Recommendation** at the bottom.
<br>![](/exercises/ex3/images/2026/03_06.png)


- The ECN Advisor is **Generating** a recommendation. You can click Refresh icon at the top right side to see if the recommendation has been generated.
<br>![](/exercises/ex3/images/2026/03_07.png)


- Once the recommendation has been generated, you see **Available** in the **Status** column.
<br>![](/exercises/ex3/images/2026/03_08.png)


6. **Review Recommendation Details** 
- Click the available recommendation in the list. There is a **Recommended Configuration** and an **Explanation** field. In the **Recommended Configuration**, you can see the recommended schedule and configuration for the ECN as well as expected capacity units that will be consumed with the ECN. You can click **Workload Class** to see which workload class is recommended to be routed to the ECN.
<br>![](/exercises/ex3/images/2026/03_09.png)
<br>![](/exercises/ex3/images/2026/03_10.png)


- (Optional) On the top right side of the recommendation, there is a **Provide Feedback** button to share your feedback on this recommendation. You can provide your evaluation on if and how this specific recommendation was helpful for you. This information will be kept confidential and used only for analysis by SAP internal teams to improve the advisors.
<br>![](/exercises/ex3/images/2026/03_12_fb.png)

<br>Now you have successfully generated the ECN recommendation. You can move on to the next step to provision and deprovision an ECN as recommended by the advisor.


Continue to - [Exercise 3.2 - Provision and deprovision an ECN with SAP Automation Pilot](../ex3.2/README.md)