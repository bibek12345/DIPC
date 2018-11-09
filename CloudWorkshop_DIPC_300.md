# Lab 300 - Remote Agent Install and On-prem to On-prem DB Synchronization
![](images/300/image300_0.png)

## Before You Begin

### Introduction

This lab covers installation and configuration of DIPC remote agent along with synchronization of two on-prem database schemas. Agents allow synchronization of data from sources outside Oracle Cloud. The target and source schemas will reside in the same database.

This lab supports the following use cases:
-   Configure Remote DIPC Agent
-   Synchronize two On-Premise Databases

### Objectives
-	Review downloading process, installation and configuration of DIPC remote agent
-   Synchronize two On-Premise Databases
   
### Time to complete
Approximately 30 minutes.

### What Do You Need?
Your will need:
- DIPC Instance URL
- DIPC User and Password
- DB information for on-prem source system: server name/ip address, user/password and service name
- DB information for on-prem target system: server name/ip address, user/password and service name
- Private keys in OpenSSH format for all instances 
- OnPremiseVM public IP address
- Putty for SSH connection to instances
- VNC viewer
- SQL developer


## Remote Agent

### Install Agent
1.	Open an SSH session into your compute server (we will simulate on-prem with a compute instance); please refer to Appendix 1 to learn how to establish a SSH session
3.	Move to the directory where the remote agent was downloaded, execute: cd /home/oracle/dipcagent/dicloud
7.	Execute command to install agent: 
./dicloudConfigureAgent.sh -recreate -debug -dipchost=**{DIPC SERVER HostName eg osc######DIPC##-oscnas001.uscom-central-1.oraclecloud.com}** -dipcport=443 -user=**{YOUR_CLOUD_ACCOUNT_USERNAME}** -password=**{YOUR_CLOUD_ACCOUNT_PASSWORD}** -authType=OAUTH2 -idcsServerUrl=https://idcs-bfb16122271a47fc91ada73842325e52.identity.oraclecloud.com -agentIdcsScope=**{YOUR_DIPC_SECAPP}** -agentClientId=4b8201b85cb946eab6f0006c37093f26 -agentClientSecret=c5e45679-aa81-4d98-a574-01c0484b37b6
	```
	where:
		{DIPC SERVER} - This is the name of your DIPC Server. This have been provided in your environment page; look for entry DIPC SERVER.
		{YOUR_USER} - This is the login you use to log into Oracle Cloud/DIPC server. This have been provided in your environment page; look for entry YOUR_USER
		{YOUR_PASSWORD} - This is the password of the login you use to log into Oracle Cloud/DIPC server. This have been provided in your environment page; look for entry YOUR_PASSWORD
		{YOUR_DIPC_SECAPP} - This is the scope of the security application defined in the Identity Server. This have been provided in your environment page; look for entry YOUR_DIPC_SECAPP
	```
8.	New directories will be created, to look at them execute: ls
9.	We will take a look at the configuration file (agent.properties) and we will change the port in which this agent will talk to DIPC. Move to the configuration directory, execute: cd /home/oracle/dipcagent/dicloud/agent/dipcagent001/conf
10.	Open the file: nano agent.properties 
![](images/300/image300_9.png)
11.	Locate parameter "agentPort"
![](images/300/image300_10.png)
12.	Change the port from "7005" to "7010"
![](images/300/image300_11.png)
13.	Save your changes, press ctrl-x, "Y", then press the ENTER key
![](images/300/image300_12.png)


### Start the Agent
1.	We will move to the directory with the necessary commands to start the agent; execute: cd /home/oracle/dipcagent/dicloud/agent/dipcagent001/bin
2.	We will start the agent by executing:
nohup ./startAgentInstance.sh &
3.	Your agent is now running. If your not logged, log into your DIPC server and navigate to the "Agents" screen
![](images/300/image300_14.png)
4. You will see the new remote agent
![](images/300/image300_14a.png)


## On-Prem to On-Prem synchronization

### Execute Data Synch Elevated Task
1. You should be logged into DIPC, if that is NOT the case, log in.
2. From the left side panel, SELECT "Home" 
![](images/300/image300_30.png)
3. For synchronization jobs we will need a CDB (Container DB) connection to our DB. In the Home Page click the “Create" button in the "Connection” box from top section 
![](images/300/image300_31.png)
4. Enter the following information:
    - Name: ONPREM_SRC_CDB
    - Description:  CDB User for on-prem source DB
	- Agent: **{REMOTE_AGENT}**
	- Type: Oracle
  	- Hostname: **{SERVER_IP_ADDRESS}**
	- Port: 1521
	- Username: C##GGSRC
	- Password: Welcome#123
	- Service Name: onprem
	```
	where:
		{REMOTE_AGENT} - Select the remote DIPC agent you just created
		{SERVER_IP_ADDRESS} - IP Address of the compute instance (simulated OnPrem environment). This have been provided in your environment page; look for entry SERVER _IP_ADDRESS
	```
5. Click "Test Connection" button and when the test is successful click "Save" button.
6. Open the drop-down menu from the top far right corner and then select “Connection”.
![](images/300/image300_37.png)
7. Enter the following information:
    - Name: ONPREM_SRC
    - Description: Connection to on-prem database schema with source tables. AMER
	- Agent: {REMOTE_AGENT}
	- Type: Oracle
  	- Hostname: {SERVER_IP_ADDRESS}
	- Port: 1521
	- Username: AMER_SRC
	- Password: Welcome#123
	- Service Name: PDB1
    - Schema Name: AMER_SRC (Default)
	```
	where:
		{REMOTE_AGENT} - Select the remote DIPC agent you just created
		{SERVER_IP_ADDRESS} - IP Address of the compute instance (simulated OnPrem environment). This have been provided in your environment page; look for entry SERVER _IP_ADDRESS
	```
8. Click on "Test Connection" button at the bottom. a green message should appear on top when everything is in order
9. Click on "Save" 
![](images/300/image300_32.png)
10. From the top bar, open the drop-down menu and the select "Connection" 
![](images/300/image300_34.png)
11. Enter the following information:
    - Name: ONPREM_TRG
    - Description: Connection to target schema onprem_trg EMEA
	- Agent: {REMOTE_AGENT}
	- Type: Oracle
	- Hostname: {SERVER_INSTANCE_IP}
	- Port: 1521
	- Username: EMEA_TRG
	- Password: Welcome#123
	- Service Name: PDB1
    - Schema Name: EMEA_TRG (Default)	
	```
	where:
		{REMOTE_AGENT} - Select the remote DIPC agent you just created
		{SERVER_IP_ADDRESS} - IP Address of the compute instance (simulated OnPrem environment). This have been provided in your environment page; look for entry SERVER _IP_ADDRESS
	```	
12. Click on "Test Connection" button at the bottom. a green message should appear on top when everything is in order
13. Click on "Save" 
![](images/300/image300_35.png)
14. From the top bar, open the drop-down menu and the select "Synchronize Data" 
![](images/300/image300_37.png)
15. Enter the following information:
	- Name: Sync OnPrem Schemas
	- Description: Sync on-prem schemas AMER to EMEA
	- Connection: ONPREM_SRC
	- Schema: AMER
	- Connection: ONPREM_TRG
	- Schema: EMEA
	- Advanced - Include Initial Load: SELECTED
	- Advanced - Include Replication: SELECTED
![](images/300/image300_38.png)
16. Click on "Save & Run" button on the top right of the screen to execute the task
17. A message will appear in the notification bar and you will be navigated to the "Monitor" screen. 
18. The job will automatically appear within the "Monitor" page
![](images/300/image300_39.png)
19. Click job to review details 
![](images/300/image300_40.png)


### Verify Data in Source and Target DBs (Optional)
Up until this point, we have monitored the job within DIPC but it would nice to see the data in both source and target to verify that they are the same. For such task, we will use SQL Developer; please refer to Appendix 3 to learn how to create connections against the workshop databases.

1. Start SQL Developer. On the connections panel, select your source database WS - AMER_SRC) and click on the plus (+) sign to open the connection 
![](images/300/image300_41.png)
 
2.	Once opened, copy and paste the following statements in the panel on the right:
	```
		SELECT COUNT(*)CATEGORIES FROM CATEGORIES;
		SELECT COUNT(*)CUSTOMERS FROM CUSTOMERS;
		SELECT COUNT(*)CUSTOMERS_INFO FROM CUSTOMERS_INFO;
		SELECT COUNT(*)ORDERS FROM ORDERS;
		SELECT COUNT(*)ORDERS_TOTAL FROM ORDERS_TOTAL;
		SELECT COUNT(*)PRODUCTS FROM PRODUCTS;
		SELECT COUNT(*)PRODUCTS_DESCRIPTION FROM PRODUCTS_DESCRIPTION;
	```
3.	Execute the statements by clicking on the “Run script” icon (second icon from left to right on the icon bar; right-ponting green arrow head on top of a page) 
![](images/300/image300_42.png)
4.	This will show all entities count on the results panel (lower section) 
![](images/300/image300_43.png)
5.	Repeat steps 1 through 4 for connection “WS - EMEA_TRG” 
![](images/300/image300_44.png)

This will show that the count in both data bases is exactly the same.


## Summary
You have now successfully completed the Hands-on Lab, and have successfully installed the remote agent and synchronized two on-prem databases.
