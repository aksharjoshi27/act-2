# Analytics in Microsoft Intelligent Data Platform (MIDP) DREAM Demo DREAM Demo in a Box Setup Guide

## What is it?
DREAM Demos in a Box (DDiB) are packaged Industry Scenario DREAM Demos with ARM templates (with a demo web app, Power BI reports, Synapse resources, AML Notebooks etc.) that can be deployed in a customer’s subscription using the CAPE tool in a few hours.  Partners can also deploy DREAM Demos in their own subscriptions using DDiB.

 ## Objective & Intent
Partners can deploy DREAM Demos in their own Azure subscriptions and show live demos to customers. 
In partnership with Microsoft sellers, partners can deploy the Industry scenario DREAM demos into customer subscriptions. 
Customers can play,  get hands-on experience navigating through the demo environment in their own subscription and show to their own stakeholders.

**Before You Begin**

## :exclamation:IMPORTANT NOTES:  

  1. **Please read the [license agreement](https://github.com/microsoft/Azure-Analytics-and-AI-Engagement/blob/main/CDP-Retail/license.md) and [disclaimer](https://github.com/microsoft/Azure-Analytics-and-AI-Engagement/blob/main/CDP-Retail/disclaimer.md) before proceeding, as your access to and use of the code made available hereunder is subject to the terms and conditions made available therein.** Any third party tool used in the demo is not owned by us and you will have to buy the licenses for extended use.
  2. Without limiting the terms of the [license](https://github.com/microsoft/Azure-Analytics-and-AI-Engagement/blob/main/CDP-Retail/license.md) , any Partner distribution of the Software (whether directly or indirectly) may only be made through Microsoft’s Customer Acceleration Portal for Engagements (“CAPE”). CAPE is accessible by Microsoft employees. For more information about the CAPE process, please connect with your local Data & AI specialist or CSA/GBB.
  3. Please note that **Azure hosting costs** are involved when DREAM Demos in a Box are implemented in customer or partner Azure subscriptions. **Microsoft will not cover** DDiB hosting costs for partners or customers.
  4. Since this is a DDiB, there are certain resources open to the public. **Please ensure proper security practices are followed before you add any sensitive data into the environment.** To strengthen the security posture of the environment, **leverage Azure Security Centre.** 
  5.  For any questions or comments please email **[dreamdemos@microsoft.com](mailto:dreamdemos@microsoft.com).**
  
   > **Note**: Set up your demo environment at least two hours before your scheduled demo to make sure everything is working.
   
# Copyright - 2023

© 2021 Microsoft Corporation. All rights reserved.   

By using this demo/lab, you agree to the following terms: 

The technology/functionality described in this demo/lab is provided by Microsoft Corporation for purposes of obtaining your feedback and to provide you with a learning experience. You may only use the demo/lab to evaluate such technology features and functionality and provide feedback to Microsoft.  You may not use it for any other purpose. You may not modify, copy, distribute, transmit, display, perform, reproduce, publish, license, create derivative works from, transfer, or sell this demo/lab or any portion thereof. 

COPYING OR REPRODUCTION OF THE DEMO/LAB (OR ANY PORTION OF IT) TO ANY OTHER SERVER OR LOCATION FOR FURTHER REPRODUCTION OR REDISTRIBUTION IS EXPRESSLY PROHIBITED. 

THIS DEMO/LAB PROVIDES CERTAIN SOFTWARE TECHNOLOGY/PRODUCT FEATURES AND FUNCTIONALITY, INCLUDING POTENTIAL NEW FEATURES AND CONCEPTS, IN A SIMULATED ENVIRONMENT WITHOUT COMPLEX SET-UP OR INSTALLATION FOR THE PURPOSE DESCRIBED ABOVE. THE TECHNOLOGY/CONCEPTS REPRESENTED IN THIS DEMO/LAB MAY NOT REPRESENT FULL FEATURE FUNCTIONALITY AND MAY NOT WORK THE WAY A FINAL VERSION MAY WORK. WE ALSO MAY NOT RELEASE A FINAL VERSION OF SUCH FEATURES OR CONCEPTS.  YOUR EXPERIENCE WITH USING SUCH FEATURES AND FUNCITONALITY IN A PHYSICAL ENVIRONMENT MAY ALSO BE DIFFERENT.

## Contents

<!-- TOC -->

- [Requirements](#requirements)
- [Before Starting](#before-starting)
  - [Task 1: Create a resource group in Azure](#task-1-create-a-resource-group-in-azure)
  - [Task 2: Power BI Workspace creation](#task-2-power-bi-workspace-creation)
  - [Task 3: Deploy the ARM Template](#task-3-deploy-the-arm-template)
  - [Task 4: Run the Cloud Shell to provision the demo resources](#task-4-run-the-cloud-shell-to-provision-the-demo-resources)
  - [Task 5: Data Explorer Setup](#task-5-data-explorer-setup)
  - [Task 6: Azure Purview Setup](#task-6-azure-purview-setup)
  - [Task 7: Power BI reports and dashboard creation](#task-7-power-bi-reports-and-dashboard-creation)
  	- [Steps to validate the credentials for reports](#steps-to-validate-the-credentials-for-reports)
  	- [Steps to create realtime reports](#steps-to-create-realtime-reports)
  	- [Follow these steps to create the Power BI dashboard](#follow-these-steps-to-create-the-power-bi-dashboard)
  	- [Updating Dashboard and Report Ids in Web app](#updating-dashboard-and-report-ids-in-web-app)
  - [Task 8: Pause or Resume script](#task-8-pause-or-resume-script)
  - [Task 9: Clean up resources](#task-9-clean-up-resources)

<!-- /TOC -->

## Requirements

* An Azure Account with the ability to create an Azure Synapse Workspace.
* A Power BI Pro or Premium account to host Power BI reports.
* Make sure you are the Power BI administrator for your account and service principal access is enabled on your Power BI tenant.
* Make sure the following resource providers are registered with your Azure Subscription.
   - Microsoft.Sql 
   - Microsoft.Synapse 
   - Microsoft.StreamAnalytics 
   - Microsoft.EventHub 
   - Microsoft.Media.MediaServices
   - Microsoft.Databricks
   - Microsoft.Kusto
* You can run only one deployment at any point in time and need to wait for its completion. You should not run multiple deployments in parallel as that will cause deployment failures.
* Select a region where the desired Azure Services are available. If certain services are not available, deployment may fail. See [Azure Services Global Availability](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=all) for understanding target service availability. (consider the region availability for Synapse workspace, Iot Central and cognitive services while choosing a location)
* Do not use any special characters or uppercase letters in the environment code. Also, do not re-use your environment code.
* In this Accelerator we have converted Real-time reports into static reports for the ease of users but have covered entire process to configure Realtime dataset. Using those Realtime dataset you can create Realtime reports.
* Please ensure that you select the correct resource group name. We have given a sample name which may need to be changed should any resource group with the same name already exist in your subscription.
* The audience for this document is CSAs and GBBs.
* Please log in to Azure and Power BI using the same credentials.
* Once the resources have been setup, please ensure that your AD user and synapse workspace have “Storage Blob Data Owner” role assigned on storage account name starting with “stretail”. You need to contact AD admin to get this done.
* Please review the [License Agreement](https://github.com/microsoft/Azure-Analytics-and-AI-Engagement/blob/main/CDP-Retail/license.md) before proceeding.

## Before starting

### Task 1: Create a resource group in Azure

1. **Log into** the [Azure Portal](https://portal.azure.com) using your Azure credentials.

2. On the Azure Portal home screen, **select** the '+ Create a resource' tile.

	![A portion of the Azure Portal home screen is displayed with the + Create a resource tile highlighted.](media/create-a-resource.png)

3. In the Search the Marketplace text box, **type** "Resource Group" and **press** the Enter key.

	![On the new resource screen Resource group is entered as a search term.](media/resource-group.png)

4. **Select** the 'Create' button on the 'Resource Group' overview page.

	![A portion of the Azure Portal home screen is displayed with Create Resource Group tile](media/resource-group-2.png)
	
5. On the 'Create a resource group' screen, **select** your desired Subscription. For Resource group, **type** 'DDiB-Lab'. 

6. **Select** your desired region.

	> **Note:** Some services behave differently in different regions and may break some part of the setup. Choosing one of the following regions is preferable: 		westus2, eastus2, northcentralus, northeurope, southeastasia, australliaeast, centralindia, uksouth, japaneast.

7. **Click** the 'Review + Create' button.

	![The Create a resource group form is displayed populated with Synapse-MCW as the resource group name.](media/resource-group-3.png)

8. **Click** the 'Create' button once all entries have been validated.

	![Create Resource Group with the final validation passed.](media/resource-group-4.png)

### Task 2: Power BI Workspace creation

1. **Open** Power BI in a new tab using the following link:  [https://app.powerbi.com/](https://app.powerbi.com/)

2. **Sign in**, to Power BI using your Power BI Pro account.

	![Sign in to Power BI.](media/power-bi.png)

	> **Note:** Use the same credentials for Power BI which you will be using for the Azure account.

3. In Power BI service **Click** on 'Workspaces'.

4. Then **click** on the 'Create a workspace' tab.

	![Create Power BI Workspace.](media/power-bi-2.png)

	> **Note:** Please create a Workspace by the name "DDiB-MIDP".

5. **Copy** the Workspace GUID or ID. You can get this by browsing to [https://app.powerbi.com/](https://app.powerbi.com/), selecting the workspace, and then copying the GUID 	from the address URL.

6. **Paste** the GUID in a notepad for future reference.

	![Give the name and description for the new workspace.](media/power-bi-3.png)

	> **Note:** This workspace ID will be used during the ARM template deployment.

7. Go to your Power BI **workspace** and **click** on New button.

8. Then **click** on **Streaming Dataset** option from the dropdown. 

	![Select new and then steaming dataset.](media/power-bi-4.png)

9. **Select API** from the list of options and **click** next. 

10. **Enable** the ‘Historic data analysis’ 

	![Select API then next.](media/power-bi-5.png)

	![Switch Historical data analysis on.](media/power-bi-6.png)
	
11. **Enter** ‘Real-time ADX Miami and San Diego’ as dataset name and **enter** the column names in “values from stream” option from list below  and **click** on create button: 

	| Field Name                        	| Type     |
	|---------------------------------------|----------|
	| City 				| text |
	| target_avg_dur_in_store				| number |
	| target_checkout_time				| number |
	| target_dwell_time				| number |
	| aisle_dwell_time_may			| number |
	| aisle_dwell_time_sep			| number |
	| aisle_dwell_time_oct				| number |
	| aisle_dwell_time_nov				| number |
	| aisle_dwell_time_dec					| number |
	| avg_checkout_time_may			| number |
	| avg_checkout_time_sep				| number |
	| avg_checkout_time_oct				| number |
	| avg_checkout_time_nov			| number |
	| avg_checkout_time_dec			| number |
	| avg_dur_in_store_may				| number |
	| avg_dur_in_store_sep				| number |
	| avg_dur_in_store_oct					| number |
	| avg_dur_in_store_nov			| number |
	| avg_dur_in_store_dec				| number |
	| EnqueuedTimeUTC			| datetime |
	| RecordedonUTC				| datetime |

	
	![Create new streaming dataset.](media/power-bi-7.png)

12. **Copy** the push url of dataset ‘Real-time ADX Miami and San Diego’ and **paste** it in a notepad for later use.

	![Provide the Push Url.](media/power-bi-8.png)
	
13. Similarly add one more dataset, "CCO Realtime", copy the push url and **paste** it in a notepad.

	| Field Name                        	| Type     |
	|---------------------------------------|----------|
	| NPS 			| number |
	| TargetNPS				| number |
	| AccountOpeningTime				| number |
	| TargetAccountOpeningTime				| number |
	| RequestsWithinSLA					| number |
	| TargetRequestsWithinSLA					| number |
	| SocialSentiment				| text |
	| RecordedOn 				| datetime |
	| SocialSentimentAfter			| text |
	| TargetRequestsWithinSLAAfter				| number |
	| RequestsWithinSLAAfter			| number |
	| TargetAccountOpeningTimeAfter			| number |
	| AccountOpeningTimeAfter				| number |
	| TargetNPSAfter 			| number |
	| NPSAfter				| number |
	| ChurnBeforeTarget				| number |
	| ChurnMid				| number |
	| ChurnMidTarget					| number |
	| ChurnAfter					| number |
	| ChurnAfterTarget				| number |
	| ChurnNov 				| number |
	| ChurnNovTarget			| number |
	| LoyalCustomerBefore				| number |
	| LoyalCustomerMid			| number |
	| LoyalCustomerAfter			| number |
	| NumberofVisitorsBefore				| number |
	| NumberofVisitorsMid					| number |
	| NumberofVisitorsAfter					| number |
	| NumberofVisitorsTarget				| number |
	| LoyalCustomerTargetBefore 				| number |
	| LoyalCustomerTargetMid			| number |
	| LoyalCustomerTargetAfter				| number |
	| ChurnBefore			| number |
	| Sensor_Per_Store_Sep			| number |
	| Sensor_Per_Store_Oct				| number |
	| Sensor_Per_Store_Nov			| number |
	| Sensor_Per_Store_Dec				| number |
	| Sensor_Per_Store_Bef_Tar			| number |
	| Sensor_Per_Store_Sep_Tar			| number |
	| Sensor_Per_Store_Oct_Tar				| number |
	| Sensor_Per_Store_Nov_Tar			| number |
	| Sensor_Per_Store_Dec_Tar			| number |
	| Sensor_Per_Store_Bef				| number |

	

### Task 3: Deploy the ARM Template

1. **Open** this link in a new tab of the same browser that you are currently in: 
	
	<a href='https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmicrosoft%2FAzure-Analytics-and-AI-Engagement%2Fmidp-with-cosmos%2FmidpCosmos%2FmainTemplate.json' target='_blank'><img src='http://azuredeploy.net/deploybutton.png' /></a>

2. On the Custom deployment form, **select** your desired Subscription.

3. **Select** the resource group name **DDiB-Lab** which you created in [Task 1](#task-1-create-a-resource-group-in-azure).

4. **Provide/Type** an environment code which is unique to your environment. This code is a suffix to your environment and should not have any special characters or uppercase letters and should not be more than 6 characters. 

5. **Provide** a strong SQL Administrator login password and set this aside for later use.

6. **Enter** the Power BI workspace ID copied in step 6 of [Task 2](#task-2-power-bi-workspace-creation).

7. **Enter** the power BI streaming dataset url for **CCO Realtime** you copied in step 13 of [Task 2](#task-2-power-bi-workspace-creation).

8. **Enter** the power BI streaming dataset url for **Store Telemetry Realtime** you copied in step 12 of [Task 2](#task-2-power-bi-workspace-creation).

9. **Click** ‘Review + Create’ button.

	![The Custom deployment form is displayed with example data populated.](media/powerbi-deployment-1.png)

10. **Click** the **Create** button once the template has been validated.

	![Creating the template after validation.](media/powerbi-deployment-3.png)
	
	> **NOTE:** The provisioning of your deployment resources will take approximately 10 minutes.
	
11. **Stay** on the same page and wait for the deployment to complete.
    
	![A portion of the Azure Portal to confirm that the deployment is in progress.](media/microsoft-template.png)
    
12. **Select** the **Go to resource group** button once your deployment is complete.

	![A portion of the Azure Portal to confirm that the deployment is in progress.](media/microsoft-template-2.png)

### Task 4: Run the Cloud Shell to provision the demo resources

1. **Open** the Azure Portal. In the Resource group section, **open** the Azure Cloud Shell by selecting its icon from the top toolbar.

	![A portion of the Azure Portal taskbar is displayed with the Azure Cloud Shell icon highlighted.](media/cloud-shell.png)

2. **Click** on 'Show advanced settings'.

	![Mount a Storage for running the Cloud Shell.](media/cloud-shell-2.png)

	> **Note:** If you already have a storage mounted for Cloud Shell, you will not get this prompt. In that case, skip step 2 and 3.

3. **Select** your 'Resource Group' and **enter** the 'Storage account' and 'File share' name.

	![Mount a storage for running the Cloud Shell and Enter the Details.](media/cloud-shell-3.png)

	> **Note:** If you are creating a new storage account, give it a unique name with no special characters or uppercase letters.

4. In the Azure Cloud Shell window, ensure the PowerShell environment is selected and **enter** the following command to clone the repository files.

```
git clone -b midp-with-cosmos --depth 1 --single-branch https://github.com/microsoft/Azure-Analytics-and-AI-Engagement.git midpCosmos
```

![Git Clone Command to Pull Down the demo Repository.](media/cloud-shell-4.png)
	
> **Note:** If you get File already exist error, please execute the following command: rm midpCosmos -r -f to delete existing clone.

> **Note**: When executing scripts, it is important to let them run to completion. Some tasks may take longer than others to run. When a script completes execution, you will be returned to a command prompt. 

5. **Execute** the midpComosSetup.ps1 script by executing the following command:

```
cd ./midpCosmos/midpCosmos
```

6. Then **run** the PowerShell: 
```
./midpComosSetup.ps1
```
    
![Commands to run the PowerShell Script.](media/cloud-shell-5.png)

7. You will see the below screen, **enter** 'Y' and **press** the enter key.

	![Commands to run the PowerShell Script.](media/cloud-shell-18.png)
      
8. From the Azure Cloud Shell, **copy** the authentication code

9. Click on the link [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) and a new browser window will launch.

	![Authentication link and Device Code.](media/cloud-shell-6.png)
     
10. **Paste** the authentication code and **click** on Next.

	![New Browser Window to provide the Authentication Code.](media/cloud-shell-7.png)

11. **Select** the same user that you used for signing in to the Azure Portal in [Task 1](#task-1-create-a-resource-group-in-azure).

	![Select the User Account which you want to Authenticate.](media/cloud-shell-8.png)
	
12. In the below screen **click** on continue.

	![Authentication done.](media/cloud-shell-20.png)

13. **Close** the browser tab once you see the message window at right and **go back** to your Azure Cloud Shell execution window.

	![Authentication done.](media/cloud-shell-9.png)
	
14. You will see the below screen and perform step #9 to step #13 again.

	![Authentication done.](media/cloud-shell-21.png)

15. Now you will be prompted to select subscription if you have multiple subscription assigned to the user you used for device login.

    ![Close the browser tab.](media/select-sub.png)
	
	> **Notes:**
	> - The user with single subscription won't be prompted to select subscription.
	> - The subscription highlighted in yellow will be selected by default if you do not enter any disired subscription. Please select the subscription carefully, as it may break the execution further.
	> - While you are waiting for processes to get completed in the Azure Cloud Shell window, you'll be asked to enter the code three times. This is necessary for performing installation of various Azure Services and preloading content in the Azure Synapse Analytics SQL Pool tables.

16. You will be asked to confirm for the subscription, **enter** 'Y' and **press** the enter key.

	![Commands to run the PowerShell Script.](media/cloud-shell-19.png)

17. You will now be prompted to **enter** the resource group name in the Azure Cloud Shell. **Type** the same resource group name that you created in [Task 1](#task-1-create-a-resource-group-in-azure). – 'DDiB-Lab'.

	![Enter Resource Group name.](media/cloud-shell-14.png)

18. After the complete script has been executed, you get to see the message "--Execution Complete--", now **go to** the Azure Portal and **search** for "app service" and **click** on the simulator app by the name "app-realtime-kpi-midp-.....".

	![Enter Resource Group name.](media/cloud-shell-16.png)
	
19. **Click** on the browse button of the app service.

	![Enter Resource Group name.](media/cloud-shell-17.png)

20. A new window will appear as shown in the below screenshot. Wait for the page to load and **close** the tab.

	![Enter Resource Group name.](media/cloud-shell-17.1.png)

### Task 5: Data Explorer Setup

1. In the Azure Portal **search** for data explorer and **click** on the data explorer resource.

	![Adx.](media/adx-1.png)
	
2. In the data explorer resource **copy** the URI.

	![Adx.](media/adx-2.png)
	
3. **Open** a new tab in the browser and **paste** the URI in the search bar, the data explorer studio opens.
	
	![Adx.](media/adx-3.png)
	
4. In the Data Explorer Studio under Data section **click** on Ingest data.

	![Adx.](media/adx-4.png)
	
5. In the Ingest data, under destination tab, **select** appropriate values in the respective fields, in Cluster **select** the kusto pool name as "midpcosmoskust....", in the Database select "MidpCosmosKustoDB" database, in the Table field **enter** the table name i.e. Occupancy and then **click** on Next.

	![Adx.](media/adx-5.png)
	
6. Under the source tab, **select** Source type as "Event Hub", in subscription **select** your subscription, in Event Hub Namespace **select** you eventhub namespace i.e. "adx-thermostat-occupancy-...", in Event Hub **enter** "occupancy", in Data connection name **select** "MidpCosmosKustoDB-occupancy", in Consumer group **select** $Default. **Expand** More Parameters section and in compression **select** None and then **click** on Next.

	![Adx.](media/adx-6.png)
	
7. Wait for some time for data preview to load, **select** JSON in the "Data format" section and then **click** on Next: Start Ingestion.

	![Adx.](media/adx-7.png)
	
8. Once the Continuous ingestion from Event Hub has been established, **click** on Close.

	![Adx.](media/adx-8.png)
	
9. Repeat the above step from 4 to 8, replacing few values, i.e. in step 5, this time **enter** the table name as "thermostat", in step 6 **enter** Event Hub as "thermostat" and Data connection name as "MidpCosmosKustoDB-thermostat".
	
### Task 6: Azure Purview Setup

> **Note:** Firstly you should assign Reader permission to the Azure Purview account starting with name "purviewmidp..." for Cosmos Account, Synapse Workspace and Storage Account starting with name "stmidp...". Once the permission has been granted, proceed with the following steps.

1. From Azure Portal, **search** for Azure purview resource in the resource group and **click** on the resource.

	![Select Purview Resource.](media/purview-1.png)
	
2. The Azure Purview resource window will open, **click** on Open Azure Purview Studio and the Azure Purview Studio will open in a new window.

	![Select Purview Resource.](media/purview-2.png)
	
3. In the Azure Purview Studio **click** on Data map **goto** source and **select** Map view. Now expand the parent collection by **clicking** on the "+" sign.

	![Select Purview Resource.](media/purview-8.png)

4. All the sub collections will be visible, **click** on the "+" sign under AzureDataLakeStorage, AzureSynapse, CosmosDB-midp and PowerBI-midp.

	![Select Purview Resource.](media/purview-9.png)

5. Under the datasource AzureDataLakeStorage **click** on View Details.

	![Select Purview Resource.](media/purview-10.png)

6. **Click** on New Scan, a window appears, here verify the default values in the different fields, **select** your collection name and finally **click** on Continue.

	![Select Purview Resource.](media/purview-11.png)
	
7. In the new window verify that the check box is **checked** for all the required containers, and then **click** on continue.

	![Select Purview Resource.](media/purview-12.png)
	
8. Again **click** on continue.

	![Select Purview Resource.](media/purview-13.png)
	
9. In the new window **check** the Once radio button and **click** on continue.

	![Select Purview Resource.](media/purview-14.png)
	
10. In the new window **click** on Save and run.

	![Select Purview Resource.](media/purview-30.png)
	
11. **Repeat** steps #5 to #10 for AzureSynapseAnalytics, AzureSqlDatabase and PowerBI. 
	
### Task 7: Power BI reports and dashboard creation

### Steps to validate the credentials for reports

1. **Open** Power BI and **Select** the Workspace, which is created in [Task 2](#task-2-power-bi-workspace-creation).
	
	![Select Workspace.](media/power-bi-report-3.png)
	
Once [Task 4](#task-4-run-the-cloud-shell-to-provision-the-demo-resources) has been completed successfully and the template has been deployed, you will be able to see a set of reports in the Reports tab of Power BI, and real-time datasets in the Dataset tab. 

The image on the below shows the Reports tab in Power BI.  We can create a Power BI dashboard by pinning visuals from these reports.

![Reports Tab.](media/power-bi-report-4.png)
	
> **Note:** If you do not see this list in your workspace after script execution, it may indicate that something went wrong during execution. You may use the subscript to patch it or manually upload the reports from this location and changing their parameters appropriately before authentication.

To give permissions for the Power BI reports to access the data sources:

2. **Click** the ellipses or settings icon on top right-side corner.

3. **Click** the settings dropdown.

4. **Click** on settings.

	![Permission.](media/power-bi-report-5.png)

5. **Click** on ‘Datasets’ tab.
	
	![Dataset.](media/power-bi-report-6.png)
	
6. **Click** on the dataset "6 ADX Website Bounce Rate Analysis".

7. **Expand** Data source credentials.

8. **Click** Edit credentials and a dialogue box will pop up.

	![Data Source Creds.](media/power-bi-report-7.png)

> **Note:** Verify the server name has been updated to your current sql pool name for all the datasets. If not, update the same under parameters section and click apply.

9. **Enter** Username as ‘labsqladmin’.

10. **Enter** the same SQL Administrator login password that was created for [Task 3](#task-3-deploy-the-arm-template) Step #5.

11. **Click** on Sign in.

	![Validate Creds.](media/power-bi-report-8.png)
	
12. **Click** on the dataset "ADX Thermostat and Occupancy".

13. **Expand** Data source credentials.

14. **Click** Edit credentials in front of Synapse and a dialogue box will pop up.

	![Data Source Creds.](media/power-bi-report-01.png)

15. **Select** privacy level as Organisational and **Click** on Sign in, a new window will pop-up.

	![Validate Creds.](media/power-bi-report-02.png)

16. In the new window, **select** the appropriate user.

	![Validate Creds.](media/power-bi-report-03.png)
		
17. **Click** on the dataset "Campaign Analytics".

18. **Expand** Data source credentials.

19. **Click** Edit credentials in front of AzureDataExplorer and a dialogue box will pop up.

	![Data Source Creds.](media/power-bi-report-04.png)

20. **Enter** Username as ‘labsqladmin’. **Enter** the same SQL Administrator login password that was created for [Task 3](#task-3-deploy-the-arm-template) Step #5. **Click** on Sign in.

	![Validate Creds.](media/power-bi-report-8.png)
	
### Steps to create realtime reports

1.	**Click** on the three dots in front of the “CCO Realtime” dataset and **click** on Create report, a new report will be created.

	![Validate Creds.](media/power-bi-report-014.png)

**CCO Realtime Visualizations:**

**Available Inventory**

2. **Select** the stacked KPI visual from “Visualizations”.

	![Validate Creds.](media/power-bi-report-015.png)

3. **Select** the field from the visual from the field panel.

4. **Drag /Select** the column name into the fields which is below the visualization panel.
	
5. **Select** Visual level or Page Level Filter in Filter pane as per the requirement.

6. **Drag** the “RecordedOn”, column to “Filters on this visual” in filter pane and **Select** filter Type as “Relative Time”.

7. In “Show items when the value” options, select “Is in the last”, “1” & “minute” **Select** Apply Filter.

	![Validate Creds.](media/power-bi-report-016.png)
	
	![Validate Creds.](media/power-bi-report-062.png)

	![Validate Creds.](media/power-bi-report-017.png)
	
	![Validate Creds.](media/power-bi-report-018.png)

**Total Visitors and Loyal Customers by RecordedOn**

8. **Select** the Stacked area line chart for the next visual.

	![Validate Creds.](media/power-bi-report-019.png)

9. **Select** the required field column for the visual.

10. **Drag /Select** the column name into the fields which is below the visualization panel.

	![Validate Creds.](media/power-bi-report-020.png)
	
	![Validate Creds.](media/power-bi-report-021.png)
	
	![Validate Creds.](media/power-bi-report-022.png)

11. After creating all the required visuals **click** on “Save” button and save the report in workspace.

	![Validate Creds.](media/power-bi-report-022.1.png)

12. Similarly one can create the required KPIs and Graphs for both the realtime reports.
		
### Follow these steps to create the Power BI dashboard

1. **Select** the workspace created in [Task 2](#task-2-power-bi-workspace-creation).

	![Select Workspace.](media/power-bi-report-9.png)
	
2. **Click** on ‘+ New’ button on the top-right navigation bar.

3. **Click** the ‘Dashboard’ option from the drop-down menu.

      ![New Dashboard.](media/power-bi-report-10.png)

4. **Name** the dashboard 'CEO Dashboard May' and **click** 'create'.

	![Create Dashboard further steps.](media/power-bi-report-11.png)

5. This new dashboard will appear in the 'Dashboard' section of the Power BI workspace.

**Follow the below steps to change the dashboard theme:**

6. **Open** the URL in a new browser tab to get JSON code for a custom theme:
[https://raw.githubusercontent.com/microsoft/Azure-Analytics-and-AI-Engagement/retail/retail/CustomTheme.json](https://github.com/microsoft/Azure-Analytics-and-AI-Engagement/blob/midp-with-cosmos/midpCosmos/CustomTheme.json)

7. **Right click** anywhere in browser and **click** 'Save as...'.

8. **Save** the file to your desired location on your computer, leaving the name unchanged.

	![Save the File.](media/theme1.png)

9. **Go back** to the Power BI dashboard you just created.

10. **Click** on the “Edit” at the top right-side corner.

11. **Click** on “Dashboard theme”.

	![Click on Dashboard Theme.](media/theme2.png)

12. **Click** ‘Upload the JSON theme’.

13. **Navigate** to the location where you saved the JSON theme file in the steps above and **select** open.

14. **Click** Save.

	![Navigate Select and Click Save.](media/theme3.png)

**Follow these steps to pin the report to the Power BI dashboard:**

15. **Search** the report 'CDP Vision Report' and then **click** on the report to open it.

	![Create Dashboard further steps.](media/power-bi-report-12.png)

16. Inside the report 'CDP Vision Report' **click** on 'Edit' at the top of the right corner.

	![Select Pillar 1 before.](media/power-bi-report-13.png)

17. **Click** over the tile and **click** on the icon to 'Pin to dashboard'.

	![Select Pillar 1 before.](media/power-bi-report-14.png)	

18. 'Pin to dashboard' window will appear.

19. **Select** the 'Existing Dashboard' radio button.

20. **Select** the existing dashboard 'CEO Dashboard May' and **click** on the 'Pin' button.

	![Select Pin to dashboard.](media/power-bi-report-15.png)

21. Similarly, **pin** the others tiles to the Dashboard

	![Pin to dashboard further steps.](media/power-bi-report-16.png)
	
22. Similarly, **pin** the tiles for the 'CEO Dashboard May' to the Dashboard

23. **Select** workpace created in [Task 2](#task-2-power-bi-workspace-creation) in the left pane.

	![Select Workspace.](media/power-bi-report-18.png)
	
24. **Open** ‘Dashboard-Images’ report.

	![Select Workspace.](media/power-bi-report-19.png)
	
25. **Click** on 'Page 3' page.

26. **Click** on Edit.

	![Click on edit.](media/power-bi-report-20.png)
	
27. **Hover** on Deep Dive chicklet and **click** pin button.

	![Hover and Click.](media/power-bi-report-21.png)
	
28. Select the ‘CEO Dashboard May’ from existing dashboard list and **click** on pin.
	
	![Hover and Click.](media/power-bi-report-22.png)

29. Similarly pin rest of the images from different tabs of the ‘Dashboard-Images’ report.
	
30. **Go back** to the ‘CEO Dashboard May’ dashboard.

	![Go back to Dashboard.](media/power-bi-report-24.png)
	
To hide title and subtitle for all the **images** that you have pined above. Please do the following:

31. Hover on the chiclet and **Click** on ellipsis ‘More Options’ of the image you selected.

32. **Click** on ‘Edit details’.

	![Click on Edit Details.](media/power-bi-report-25.png)
	
33. **Uncheck** ‘Display title and subtitle’.

34. **Click** on ‘Apply’.

35. **Repeat** Step 4 to 22 for all image tiles.

	![Click apply and repeat.](media/power-bi-report-26.png)
	
36. After disabling ‘Display title and subtitle’ for all images, **resize** and **rearrange** the top images tiles as shown in the screenshot. 
	
	![Resize and Rearrange.](media/power-bi-report-27.png)
	
37. Similarly pin left image tiles from ‘Dashboard-Images’ of chicklets report to the "CEO Dashboard May" Dashboard.

38. **Resize** and **rearrange** the left images tiles as shown in the screenshot. Resize the KPI tile to 1x2. Resize the Deep Dive to 1x4. Resize the logo to 1x1 size; resize other vertical tiles to 2x1 size.  

	![Resize and Rearrange again.](media/power-bi-report-28.png)

39. The Dashboard **CEO Dashboard May** should finally look like this. Table in following row indicates which KPI’s need to be pinned from which report to achieve this final look.
	
	![Final Look.](media/power-bi-report-38.png)

40. **Refer** to this table while pinning rest of the tiles to the dashboard.

	![Table.](media/power-bi-table-6.png)

41. Here is the list of Dashboards you have to create for Retail and the report to migrate to prod environment. You will see the necessary details for the same below. You must refer to the [Excel](https://github.com/microsoft/Azure-Analytics-and-AI-Engagement/blob/midp-with-cosmos/midpCosmos/Dashboard%20Mapping.xlsx) file for pinning the tiles to the dashboard.

	![Final Look.](media/power-bi-report-33.png)

42. **CEO Dashboard - Dec** should look like this. Following are the details of tiles for the same.

	![Final Look.](media/power-bi-report-32.png)
	
43. **Refer** to this table while pinning rest of the tiles to the dashboard.

	![Table.](media/power-bi-table-1.png)

### Updating Dashboard and Report Ids in Web app

By default, the web app will be provisioned with Gif placeholders for web app screens with dashboards. Once you have completed the steps listed above in this section, you can update the dashboard id’s generated into the main web app if you choose. Here are the steps for it.

1. **Navigate** to your Power BI workspace.

2. **Click** on one of the dashboards you created. Eg. CEO Dashboard May.

	![Navigate and Click.](media/power-bi-report-24.png)

3. **Copy** the dashboard id from the url bar at the top.
	
	![Copy the dashboard id.](media/updating-powerbi-2.png)

4. **Navigate** to Azure Portal.

5. **Open** the Azure Cloud Shell by selecting its icon from the top toolbar.

	![Navigate and Open.](media/updating-powerbi-3.png)

6. **Click** on upload/download button.

7. **Click** download.

8. **Enter** the following path:  
	
	```
	midpCosmos/midpCosmos/midpcosmos-demo-app/wwwroot/config-poc.js
	```

9. **Click** Download button.

	![Enter path and Click download button.](media/updating-powerbi-4.png)
	
10. At the right bottom of the cloudshell screen you get a hyperlink, **click** on it.

	![Enter path and Click download button.](media/updating-powerbi-12.png)

11. **Edit** the downloaded file in notepad.

12. **Paste** the dashboard id you copied earlier between the double quotes of key ‘CEO Dashboard - May’.

13. Similarly repeat step #12 according to the following mapping:

	| Field Name                        	| Type     |
	|---------------------------------------|----------|
	| CEODashboardMayDashboardID 			| CEO Dashboard May |
	| CEODashboardAfterDashboardID				| CEO Dashboard Dec |
	| RealTimeInStoreAnalyticsReportID				| Realtime In Store Analytics |

14. **Save** the changes to the file.

	![Edit paste and save.](media/updating-powerbi-5.png)

15. **Navigate** to Azure Portal.

16. **Open** the Azure Cloud Shell by selecting its icon from the top toolbar.

	![Navigate and Open.](media/updating-powerbi-6.png)

17. **Click** upload/download button.

18. **Click** upload.

19. **Select** the config-poc.js file you just updated.

20. **Click** open.

	![Select and Click open.](media/updating-powerbi-7.png)

21. **Execute** the following command in cloudshell:  
	
	```
	cp config-poc.js ./midpcosmos/midpcosmos/midpcosmos-demo-app/wwwroot
	```
	
	![Execute the command.](media/updating-powerbi-8.png)

22.	**Execute** the following command in cloudshell: 
	
	```
	cd midpcosmos/midpcosmos/subscripts 
	./updateWebAppSubScript.ps1
	```
	
	![Execute the command.](media/updating-powerbi-9.png)

23. From the Azure Cloud Shell, **copy** the authentication code. 

24. **Click** on the link [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) and a new browser window will launch.

	![Copy and Click on Link.](media/updating-powerbi-10.png)

25. **Paste** the authentication code.

26. **Select** appropriate username when prompted.

27. Wait for the script execution to complete.

	![Paste select and wait.](media/updating-powerbi-11.png)

> **Note:** You may be prompted to select your subscription if you have multiple subscriptions.
	
> **Note:** The setup for your Dream Demo in a Box is done here and now you can follow the demo script for testing/demoing your environment.
	
### Task 8: Pause or Resume script

> **Note:** Please perform these steps after your demo is done and you do not need the environment anymore. Also ensure you Resume the environment before demo if you paused it once. 
 
1. **Open** the Azure Portal 

2. **Click** on the Azure Cloud Shell icon from the top toolbar. 

	![Open and Click on Azure Cloud Shell.](media/authentication-5.png)

**Execute** the Pause_Resume_script.ps1 script by executing the following command: 
3. **Run** Command: 
	```
	cd "midpCosmos\midpCosmos"
	```

4. Then **run** the PowerShell script: 
	```
	./pause_resume_script.ps1 
	```
	
	![Run the Powershell Script.](media/powershell.png)
	
5. From the Azure Cloud Shell, **copy** the authentication code
	
	![Copy the Authentication Code.](media/powershell-2.png)
	
6. Click on the link [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) and a new browser window will launch.
	
7. **Paste** the authentication code.
	
	![Paste the authentication code.](media/authentication.png)
	
8. **Select** the same user that you used for signing into the Azure Portal in [Task 1](#task-1-create-a-resource-group-in-azure). 

9. **Close** this window after it displays successful authentication message.

	![Select the same user and Close.](media/authentication-2.png)

10. When prompted, **enter** the resource group name to be paused/resumed in the Azure Cloud Shell. Type the same resource group name that you created. 
	
	![Enter the Resource Group Name.](media/authentication-3.png)

11. **Enter** your choice when prompted. Enter ‘P’ for **pausing** the environment or ‘R’ for **resuming** a paused environment. 

11. Wait for the script to finish execution. 

	![Enter your choice.](media/authentication-4.png)

### Task 9: Clean up resources

> **Note: Perform these steps after your demo is done and you do not need the resources anymore**

**Open** the Azure Portal.

1. Open the Azure Cloud Shell by **clicking** its icon from the top toolbar.

	![Open the Azure Portal.](media/authentication-5.png)

**Execute** the resourceCleanup.ps1 script by executing the following:

2. **Run** Command: 
	```
	cd "midpCosmos\midpCosmos"
	```

3. Then **run** the PowerShell script: 
	```
	./resource_cleanup.ps1
	```

	![Run the Powershell Script.](media/authentication-6.png)

4. You will now be prompted to **enter** the resource group name to be deleted in the Azure Cloud Shell. Type the same resource group name that you created in [Task 1](#task-1-create-a-resource-group-in-azure) - 'DDiB-Lab'.

5. You may be prompted to select a subscription in case your account has multiple subscriptions.

	![Close the browser tab.](media/select-sub.png)
	
	> **Notes:**
	> - The user with single subscription won't be prompted to select subscription.
	> - The subscription highlighted in yellow will be selected by default if you do not enter any disired subscription. Please select the subscription carefully, as it may break the execution further.
	> - While you are waiting for processes to get completed in the Azure Cloud Shell window, you'll be asked to enter the code three times. This is necessary for performing installation of various Azure Services and preloading content in the Azure Synapse Analytics SQL Pool tables.

6. You will now be prompted to **enter** the resource group name in the Azure Cloud Shell. Type the same resource group name that you created in [Task 1](#task-1-create-a-resource-group-in-azure). – 'DDiB-Lab'.

	![Enter Resource Group name.](media/cloud-shell-14.png)
