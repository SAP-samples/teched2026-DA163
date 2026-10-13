# Exercise 3 - Elastic Compute Node Advisor and SAP Automation Pilot

In this exercise, we will configure **Elastic Compute Node Advisor (ECN Advisor)** and generate an ECN recommendation for a specific timeframe in **Recommendation App** from **HANA Cloud Central**. In addition, we will use **SAP Automation Pilot** to simplify and automate the provisioning and deprovisioning processes for ECNs.

## What are Elastic Compute Node (ECN) and ECN Advisor?

An **Elastic Compute Node (ECN)** is a read-only compute node where the compute-intensive workloads can be distributed. It's basically equivalent to the indexserver but no persistency to store data permanently, and can be easily added and dropped when required depending on the current workload state. It is most suitable for compute-intensive OLAP or read-only peak workloads. It can effectively and flexibly handle upcoming workload peaks while keeping your coordinator (indexserver) unchanged. For example, an Intelligent Data Application running on SAP HANA Cloud can leverage ECNs to effectively address compute-intentive workload peaks, such as quarter-end jobs. You can find more information about the ECN in [this guide](https://help.sap.com/docs/hana-cloud-database/sap-hana-cloud-sap-hana-database-administration-guide/sap-hana-cloud-elastic-compute-node?locale=en-US).

The **ECN Advisor** helps you identify workloads that drive peak CPU or memory within a specified time window and recommend when to provision and deprovision ECNs. The selection criteria are as below:
- Significant CPU or memory consumption
- Predictable patterns (daily, weekly, or monthly)
- Read-only (no DML, no updatable SQLScript)

The ECN Advisor will be continuously enhanced with more advanced automation and usability features.

## What is SAP Automation Pilot?

**SAP Automation Pilot** is a cloud-based automation service within the SAP Business Technology Platform (BTP) DevOps portfolio. As a low-code/no-code engine, it is designed to simplify and automate complex manual processes and tasks, enhancing efficiency and reducing operational overhead by designing and running Ops automation flows. You can find further information about Automation Pilot [here](https://help.sap.com/docs/automation-pilot/automation-pilot/what-is-sap-automation-pilot?locale=en-US), and please be noted that this service is <ins>**not yet**</ins> available in all regions.

Continue to - [Exercise 3.1 - Configure ECN Advisor and generate an ECN recommendation](ex3.1/README.md)