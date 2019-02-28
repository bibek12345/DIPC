# Lab 800 - Data Lake Builder Elevated Task 
![](images/800/image800_0.png)

## Before You Begin

### Introduction
This lab will explain how to create a data lake and deliver data into it

### Objectives
- Create connection to Oracle Object Storage
- Create a Data Lake 
- Execute a Data Lake Builder elevated task

### Time to Complete 
Approximately 15 minutes

### What Do You Need?
Your will need:
- DIPC Instance URL
- DIPC User and Password
- Connection information for an Oracle Object Storage container: domain, URL, user/password and container name
- Flat file "webclicks.txt"


## Log into DIPC Server

### Login into DIPC using Oracle Cloud Services Dashboard

1. In your web browser, navigate to cloud.oracle.com, then click "Sign in".
2. Provide the cloud account; for example,oscnas001 then **\<Enter\>**.
![](images/Common/Login/imageCommL_01.png)
3. Provide your user name and password, then click "Sign In" button. You will land in your Home screen. ![](images/Common/Login/imageCommL_02.png)
4. Scroll in your home screen until you locate "Data Integration Platform" service and click on it.  ![](images/Common/Login/imageCommL_03.png)
5. Click on the hamburger menu of the DIPC server assigned to you, then click "Data Integration Platform Console". ![](images/Common/Login/imageCommL_04.png)

You will be navigated to your DIPC server Home page. ![](images/Common/Login/imageCommL_05.png)

### Login into DIPC using direct URL

1. Open a browser window an provide your DIPC server URL. The URL will be provided by the instructor and will look like this one "https://osc132657dipc-oscnas001.uscom-central-1.oraclecloud.com/dicloud"
2. Provide your user name and password, then click "Sign In" button. ![](images/Common/Login/imageCommL_02.png)
You will be navigated to your DIPC server Home page.


## Data Lake Builder Elevated Task

### Create Connection to File (Source)
1. You should be logged into DIPC, if that is NOT the case, log in and navigate to the Catalog.
2.	If you have NOT created connection "FILE_SRC", click “Create" button on the "Connection” box from top section.
![](images/Common/General/imageCommG_04.png)
3. 	Enter the following information:
	- Name: FILE_SRC
	- Description: Read Files
	- Agent: **\<REMOTE_AGENT\>**
	- Type: File
	- Directory: /home/oracle
```
where:
    <REMOTE_AGENT> - Select the DIPC agent you created
```
4. Click "Test Connection" button and when the test is successful click "Save" button.
![](images/Common/Connections/imageCommC_03.png)	


### Create Connection to Object Storage (Target)
1. From the "Create" drop down menu on the top right corner select "Connection" 
![](images/Common/General/imageCommG_04.png)
2.	Enter the following information:
    - Name: objectstorageC 
    - Description: Connection to Object Storage Classic
    - Agent: **\<REMOTE_AGENT\>**
    - Type: Oracle Object Storage Classic.
    - Domain: **\<OBJECT_STORAGE_DOMAIN\>**
	- Service URL: **\<OBJECT_STORAGE_URL\>**
	- Container: DIPC Workshop
    - Username: **<YOUR_USER\>**
    - Password: **<YOUR_PASSWORD\>**
	```
	where:
		<REMOTE_AGENT> - Select the DIPC agent you created 
		<OBJECT_STORAGE_DOMAIN> - This have been provided in your environment page; look for entry OBJECT_STORAGE_DOMAIN
		<OBJECT_STORAGE_URL> - This have been provided in your environment page; look for entry OBJECT_STORAGE_URL
		<YOUR_USERNAME> - This is the login you use to log into Oracle Cloud/DIPC server. This have been provided in your environment page; look for entry YOUR_USER.
		<YOUR_PASSWORD> - This is the password of the login you use to log into Oracle Cloud/DIPC server. This have been provided in your environment page; look for entry YOUR_PASSWORD.
	```
3. Click "Test Connection" button and when the test is successful click "Save" button. 
![](images/800/image800_9.1.png)


### Create Data Lake (Target)
1. From the "Create" drop down menu on the top right corner select "Data Lake" 
![](images/800/image800_10.png)
2.	Enter the following information:
    - Name: DATA_LAKE
    - Description: Creating a data lake
    - Connection: forAdw
    - Type: Parquet

3. Click "Save" button and when the test is successful click "Save" button. 
![](images/800/image800_11.1.png)


### Data Lake Builder task
1.	From the "Create" drop down menu on the top right corner select "Add Data to Data Lake" 
![](images/800/image800_12.png)
2.	Provide the following information:
	- Name:  DATA_LAKE
	- Description: Create Datalake pipeline
	- Connection: FILE_SRC
	![](images/800/image800_13.png)
	- Directory: /home/oracle 
		- If you would like to select a different directory, click on the "Select" button on the right of the field 
		![](images/800/image800_14.png)
		- Navigate to the directory then click on "Select" button 
	- File: webclicks.txt
		- Click on the "Select" button on the right of the field
		![](images/800/image800_15.png)
		- Click on  "webclicks.txt" file and then click on "Select" button 
	- Connection: DATA_LAKE
	- Data Entity: Web_Clicks
	- Type: Parquet
	- File Path: **\<YOUR_USERNAME\>**
	```
	where:
		<YOUR_USER> - This is the login you use to log into Oracle Cloud/DIPC server. This have been provided in your environment page; look for entry YOUR_USER. Please use your user name to assure the file has a unique name and there is no problems with other workshop participants.
	```
		
	![](images/800/image800_16.png)

3. Click on "Save & Execute" button located on the top right corner of the screen to execute the task 
![](images/800/image800_17.png)
4.	A meesage  will appear in the notification bar to inform that the test has been created and you will be navigated to the “Monitor” screen. 
5.	The job will appear in the "Monitor" page. This may take up to 1 minute


### Review Job in DIPC
1.	You should be in the “Monitor” screen. Click on the job to see details. The DATA_LAKE job will show "Successful" after a little while 
![](images/800/image800_18.png)
2.	Review the details provided (start and end time, duration, processed rows, etc). Once finished, click on the "Home" hyperlink located on the left panel 
![](images/800/image800_19.png)


## Summary
In this lab, we have seen how to create a data pipeline to object storage; this data can then be used by other application (Big Data, etc.)
