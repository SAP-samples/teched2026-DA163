## Exercise 3.2 Provision and deprovision an ECN with SAP Automation Pilot

You can provision and deprovision the ECN and route the workload class to the ECN within HANA Cloud Central. However, to make this process more easily repeatable, in this practice, you will learn how to make use of **SAP Automation Pilot**.

1. **Configure the HANA Cloud instance to allow connections for Automation Pilot**
- In the Instance Overview page at HANA Cloud Central, click **Manage Configuration** on top.

- Go to **Connections**, choose **Allow specific IP addresses and IP ranges** option and then add the following 6 IP addresses if they are not added already.
```
18.192.167.80,
18.158.182.81,
18.192.94.153,
18.159.0.107,
18.159.145.225,
3.122.189.245
```
You can click need to add the IP address in the table view one by one by clicking **Add** button as you see in the screenshot below.
<br>![](/exercises/ex3/images/2026/03_ap00.png)

Or, in a simpler way, you can switch to the syntax view and copy and paste the 6 IP addresses like below.
<br>![](/exercises/ex3/images/2026/03_ap01.png)

- Click **Review and Save**.

- Choose **Apply changes without a restart** option for the sake of time and click **Save Changes**.


2. **Open Automation Pilot from SAP BTP Cockpit**
- Go back to SAP BTP Cockpit.
- Click **Automation Pilot** from Application list from **Subscriptions**.
<br>![](/exercises/ex3/images/2026/03_ap02.png)


3.	**Import an ECN HANA Cloud Other Environment Catalog**

With the **Automation Pilot**, you can run predefined commands or create custom commands. For ECN provisioning and deprovisioning in HANA Cloud Other Environment instance, we have published a catalog with predefined commands to make it easy to use. This catalog is not included in **Provided Catalogs** of the service, but you will import the prepared catalog through the following steps.

- Go to **Catalogs** and click **Import** button on the top right side.
<br>![](/exercises/ex3/images/2026/03_ap03.png)

- Click [here](https://github.com/SAP-samples/automation-pilot-examples/blob/main/hana-ecn-other/ecncatalog_otherenv.json) and copy raw file. This repository contains a curated collection of example Automation Pilot commands and what you see now is for ECN scenario.
<br>![](/exercises/ex3/images/2026/03_ap04.png)

- Paste the copied raw file into the bottom field and click import. This is a json file that includes all the necessary parameters for ECN provisioning and deprovisioning.
<br>![](/exercises/ex3/images/2026/03_ap05.png)

- You can now see the imported ECN catalog in your catalog list filtering by name "ECN".
<br>![](/exercises/ex3/images/2026/03_ap06.png)


4. **Edit Inputs of the Catalog**
- Click **Inputs** in the catalog.
<br>![](/exercises/ex3/images/2026/03_ap07.png)

- Click **HanaCloudOtherEnvironments**.
<br>![](/exercises/ex3/images/2026/03_ap08.png)

- You can see the list of **Keys** in **Content** field. Edit each key by clicking a Pencil icon.
<br>![](/exercises/ex3/images/2026/03_ap09.png)

- For the first key, you need to add **connectionUrl**. Access **HANA Cloud Central** to **Copy SQL Endpoint** of your instance.
<br>![](/exercises/ex3/images/2026/03_ap10.png)

- Add the copied SQL Endpoint in the value after <ins>**jdbc:sap://**</ins> and save it.
<br>![](/exercises/ex3/images/2026/03_ap11.png)

- Update **dbPassword** to your instance password.
<br>![](/exercises/ex3/images/2026/03_ap12.png)

Your DB Password: **_Welcome1_**

- Update **instanceID** to your instance ID. You can copy it from **HANA Cloud Central** again.
<br>![](/exercises/ex3/images/2026/03_ap13.png)
<br>![](/exercises/ex3/images/2026/03_ap14.png)
<br>![](/exercises/ex3/images/2026/03_ap15.png)


- Update **serviceKey**. Copy JSON from **BTP Cockpit** and paste it as below.
<br>![](/exercises/ex3/images/2026/03_ap16.png)
<br>![](/exercises/ex3/images/2026/03_ap17.png)
<br>![](/exercises/ex3/images/2026/03_ap18.png)
<br>![](/exercises/ex3/images/2026/03_ap19.png)

- Now you go back to **Inputs** list and click **HanaCloudServiceKey**.
<br>![](/exercises/ex3/images/2026/03_ap20.png)

- Update **serviceKey** <ins>again</ins> by copying and pasting JSON from **BTP Cockpit**.
<br>![](/exercises/ex3/images/2026/03_ap21.png)
<br>![](/exercises/ex3/images/2026/03_ap22.png)


5. **Provision ECN with Commands**
- Click **Commands** from the imported ECN HANA Cloud Other Environment catalog.
<br>![](/exercises/ex3/images/2026/03_ap23.png)

- Click **ProvisionHanaCloudElasticComputeNode** from the list of commands.
<br>![](/exercises/ex3/images/2026/03_ap24.png)

- Scroll down and select **EnableWorkloadClass** in the **Executors** and edit the statement and the name of the workload class which needs to be routed to the ECN (the workload class is reported in the recommendation). <ins>You will find which statement to put in the field in the following steps.</ins> 
<br>![](/exercises/ex3/images/2026/03_ap25.png)

Come back to **Automation Pilot** and edit the statement to route the selected workload class to a new ECN that will be provisioned by setting the routing location hint.
You can copy and paste the following statement.
```sql
ALTER WORKLOAD CLASS "WLC1" SET 'ROUTING LOCATION HINT'='ecn1';
```
<br>![](/exercises/ex3/images/2026/03_ap26.png)


- And now you are ready to **Trigger** the command. Click **Trigger** on the top right side.
<br>![](/exercises/ex3/images/2026/03_ap27.png)

- Select **HanaCloudOtherEnvironments** for **Inputs**.
<br>![](/exercises/ex3/images/2026/03_ap28.png)

- For the rest of the **Parameters**, you can take the recommended configuration that the advisor provided.
Add **cpu**, **memory**, **storage** as recommended, and add a **nodeName**. Make sure you are using the same node name as you put in the **EnableWorkloadClass** executor earlier. And click **Trigger**.

**Note:** for this particular very simple case we have observed the advisor generating ECN recommendations with less capacity than the ones appearing in the previous exercise in some cases, if that happens please fill in the properties for the node with at least memory 32 GB, 4 vCUPs and 120 GV space.

<br>![](/exercises/ex3/images/2026/03_ap29.png)
<br>![](/exercises/ex3/images/2026/03_ap30.png)

- Now the command is being executed and you can check the progress. It will take around 3-4 minutes.
<br>![](/exercises/ex3/images/2026/03_ap31.png)

- You will see **Finished** status when it's completed. It means the ECN is successfully provisioned and the selected workload class is set to be routed to the provisioned ECN.
<br>![](/exercises/ex3/images/2026/03_ap32.png)

- To check, you can go to **HANA Cloud Central** and in **Elastic Compute Node** page, hit the refresh button and then you see the <u>**ecn1**</u> created in the **Current Running ECNs** list. You can also click **1 workload classes** and see now the **WLC1** has a routing location set as **ecn1**.
<br>![](/exercises/ex3/images/2026/03_ap33.png)
<br>![](/exercises/ex3/images/2026/03_ap34.png)

6. **Run the workload with ECN**

- Open the SQL Console.
<br>![](/exercises/ex3/images/2026/03_ap35.png)

- Connect to **ECN User** by running the following script.
```SQL
CONNECT ECNUSER PASSWORD Ecnadvisor1;
CALL CALL_SPIKE();
```
<br>![](/exercises/ex3/images/2026/03_ap36.png)

- Once it's executed, go back to **Instance Overview** page and click **Compute** card. You can verify that the workload was run with the **ecn1** from the spike observed in the Compute graph.
<br>![](/exercises/ex3/images/2026/03_ap37.png)

7. **Deprovision ECN with Commands**
- Choose **DeprovisionHanaCloudElasticComputeNode** from the **Commands** list.
<br>![](/exercises/ex3/images/2026/03_ap38.png)

- Edit **statement** parameter to update the workload class name from **DisableWorkloadClass** Executor. This executor will disable the workload class but instead the commad we use is to redirect to coordinaror.
<br>![](/exercises/ex3/images/2026/03_ap39.png)
<br>![](/exercises/ex3/images/2026/03_ap40.png)

- Trigger the command. Select **HanaCloudOtherEnvironments** for **Inputs** and add the ECN name, <u>**ecn1**</u> in the **nodeName** field. And click **Trigger**.
<br>![](/exercises/ex3/images/2026/03_ap41.png)

- The **Automation Pilot** is redirecting the workload class and deprovisioning the ECN.
<br>![](/exercises/ex3/images/2026/03_ap42.png)

- Now the evn1 ECN is successfully deprovisioned.

- In **HANA Cloud Central**, you can check there is no running ECN called "ecn1" anymore.
<br>![](/exercises/ex3/images/2026/03_ap43.png)


8. **Easily Repeat Commands with Automation Pilot**
- To simplify repeating the provisioning and deprovisioning of ECN with the same configuration in a regular basis, you can go to **Executions** list and select the command that you have executed before and simply **Retrigger** the command.
<br>![](/exercises/ex3/images/2026/03_ap44.png)


## Clean-up

**Switch off ECN Advisor**

- Go to **Advisors** tab in the Recommendation App.

- Click the three dots on the top right side and select **Switch Off**.

- Choose **Remove data immediately** and click **Switch Off**.

The ECN Advisor is off now and all recommendation data has been deleted.
You can go back to **Instance Overview** page.


## Summary

You've learned how to configure ECN Advisor to generate recommendations for your workload, and how to provision and deprovision ECNs more simply and automatically with Automation Pilot.

You can find more assets about ECN and ECN Advisor below.
- Blog: [Harnessing Dynamic Elasticity (Elastic Compute Node) for Smarter Scaling in SAP HANA Cloud](https://community.sap.com/t5/technology-blog-posts-by-sap/harnessing-dynamic-elasticity-elastic-compute-node-for-smarter-scaling-in/ba-p/14016836)
- Tutorial: [Use an Elastic Compute Node (ECN) for Scheduled Workloads](https://developers.sap.com/tutorials/hana-cloud-ecn..html#dab4c6d2-91f5-4b55-be9f-16a0ee7a50e2)


You've now completed all the hands-on exercise for **DA163: SAP HANA Cloud Advisor and Automation**.

Thank you very much for your participation.