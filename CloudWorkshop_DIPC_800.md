# Lab 800 - Data Lake Builder Elevated Task 
![](images/800/image800_0.png)

## Before You Begin

### Objectives
-   Review how to create connections to Oracle Object Storage
-   Review how to execute a Data Lake Builder task

### Time to Complete 
Approximately 15 minutes.

### What Do You Need?
Your will need:
- DIPC Instance URL
- DIPC User and Password
- Connection information for an Oracle Object Storage container: domain, URL, user/password and container name
- Flat file "webclicks.txt"


## Data Lake Builder Elevated Task

### Navigate to Oracle Object Storage container
Before we execute the task, gather the necessary Object Storage container information to connect within DIPC, as well as to verify that the container is empty. First of all we will log into you Oracle cloud instance:
1. In your web browser, navigate to cloud.oracle.com, then click Sign in.
2. Provide the cloud account: orasenatdpltintegration02 then <Enter>
3. Provide your user name and password, then click Sign In. 
![](images/800/image800_1.png)
4. Get the below details for the object storage container from the instructor.
	4.1 Domain
	4.2 Service URL
	4.3 Container

	Example:
	Domain: Storage-natdburlnthubtrainingenv1
	Service URL: https://uscom-central-1b.storage.oraclecloud.com
	Container: sensorwio
8. Store it in a notepad or other text editor, you will need this URL later


### Create Connection to File (Source)
1. Log into your Workshop DIPC Server
2.	In the Home Page click “Create" button on the "Connection” box from top section 
![](images/800/image800_6.png)
3. 	Enter the following information:
	- Name: FILE_SRC
	- Description: Read Files
	- Agent: **{LOCAL_AGENT}**
	- Type: File
	- Directory: /home/DIPC
	```
	where:
		{LOCAL_AGENT} - Select the local DIPC agent 
	```
 4. Click "Test Connection" button and when the test is successful click "Save" button 
 ![](images/800/image800_7.png)


### Create Connection to Object Storage (Target)
1. From the "Create" drop down menu on the top right corner select "Connection" 
![](images/800/image800_8.png)
2.	Enter the following information:
    - Name: DATA_LAKE
    - Description: Connection to Object Storage to create a Data Lake
    - Agent: **{LOCAL_AGENT}**
    - Type: Oracle Object Storage Classic.
    - Domain: **{OBJECT_STORAGE_DOMAIN}**
	- Service URL: **{OBJECT_STORAGE_URL}**
	- Container: DIPC Workshop
    - Username: **{USERNAME_TO_CONNECT_OBJECT_STORAGE}**
    - Password: **{PASSWORD}**
	```
	where:
		{LOCAL_AGENT} - Select the local DIPC agent 
		{OBJECT_STORAGE_DOMAIN} - From the Rest EndPoint URL you saved previously copy and paste the last part. That is from "https://uscom-east-1.storage.oraclecloud.com/v1/Storage-oscnas100" copy "Storage-oscnas001. Or this have been provided in your environment page; look for entry OBJECT_STORAGE_DOMAIN
		{OBJECT_STORAGE_URL} - From the Rest EndPoint URL you saved previously copy and paste the server URL. That is from "https://uscom-east-1.storage.oraclecloud.com/v1/Storage-oscnas100" copy "https://uscom-east-1.storage.oraclecloud.com". Or this have been provided in your environment page; look for entry OBJECT_STORAGE_URL
		{USERNAME_TO_CONNECT_OBJECT_STORAGE} - This is the login you use to connect to object storage classic.
		{YOUR_PASSWORD} - This is the password of the login you use to log into Oracle Cloud/DIPC server.
	```
3. Click "Test Connection" button and when the test is successful click "Save" button. 
![](images/800/image800_9.png)


### Create Data Lake (Target)
1. From the "Create" drop down menu on the top right corner select "Connection" 
![](images/800/image800_10.png)
2.	Enter the following information:
    - Name: DATA_LAKE
    - Description: Creating a data lake
    - Connection: **{OBJECT_STORAGE_CLASSIC_CONNECTION}**
    - Type: Parquet
	```
	where:
		{OBJECT_STORAGE_CLASSIC_CONNECTION} - This is the object storage classic connection name created above. 
		{Parquet} - This is the default storage format in object storage classic.
	```
3. Click "Save" button and when the test is successful click "Save" button. 
![](images/800/image800_11.png)


### Data Lake Builder task
1.	From the "Create" drop down menu on the top right corner select "Add Data to Data Lake" 
![](images/800/image800_12.png)
2.	Provide the following information:
	- Name:  DATA_LAKE
	- Description: Create Datalake pipeline
	- Connection: FILE_SRC
	- Directory: /home/oracle 
		- Click on the "Select" button on the right of the field 
		![](images/800/image800_13.png)
		![](images/800/image800_14.png)
		- Click on "Files" directory and then click on "Select" button 
	- File: webclicks.txt
		- Click on the "Select" button on the right of the field 
		![](images/800/image800_15.png)
		- Click on  "webclicks.txt" file and then click on "Select" button 
		![](images/800/image800_16.png)
	- Connection: DATA_LAKE
	- Data Entity: Web_Clicks
	- Type: Parquet
	- File Path: **{YOUR_USERNAME}**
	```
	where:
		{YOUR_USER} - This is the login you use to log into Oracle Cloud/DIPC server. This have been provided in your environment page; look for entry YOUR_USER. Please use your user name to assure the file has a unique name and there is no problems with other workshop participants.
	```
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
In this lab, we have seen how it is create a data pipeline to object storage; this data can then be used by other application (Big Data, etc.)
