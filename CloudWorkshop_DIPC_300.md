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
Approximately 45 minutes.

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

### Download Agent
1.	Open an SSH session into your compute server (we will simulate on-prem with a compute instance); please refer to Appendix 1 to learn how to establish a SSH session
2.	Open a VNC viewer; please refer to Appendix 2 to learn how to establish a VNC session
3.	Open a terminal; select “Applications > Favorites > Firefox” from the top left corner of the screen

![](images/300/image300_1.png)
 
4.	Navigate to your DIPC server, provide the URL (it should look like this): https://osc132657dipc-oscnas001.uscom-central-1.oraclecloud.com/dicloud
5.	Provide your user name and password, then click "Sign In" button

![](images/300/image300_2.png)
 
6.	This will bring you to your DIPC server’s  home page
7.	Click on “Agents” (left part of your screen)

![](images/300/image300_3.png)

8.	Click on "Download Installer" button
![](images/300/image300_4.png)

9. Open the drop down menu and select the Operating System of your choice; in our case “Linux”
![](images/300/image300_4a.png)

10.	Once you have selected the operating system, you can choose the components that you would need in your remote agent. Select "Oracle 12c" and "Data Integrator (ODI)" then click on "Download" button.

![](images/300/image300_5.png)
 
11.	Select “Save File” and the click “OK”, to download the file to your “on-premise” machine

![](images/300/image300_6.png)


### Download DIPC Certificate
1. If you are NOT already logged into your DIPC server, log into it. 
2. Once in your DIPC server, click on the green padlock icon on the left of the address field of your browser
![](images/300/image300_6a.png)
 
3.	Click on the arrow on the right side of the listed secure connection
![](images/300/image300_6b.png)

4.	Click on “More Information” hyperlink of the Site Security pop-up
![](images/300/image300_6c.png)

5.	In the “Website Identity” section of the “Security” tab click “View Certificate” button 
![](images/300/image300_6d.png)
 
6.	In the “certificate Viewer” applet in the “Details” tab, click “Export …” button
![](images/300/image300_6e.png)
 
7.	On the left panel click on "oracle", then click button "Create Folder", provide name "Certificate"
![](images/300/image300_6f.png)

8. Now you are in the directory you just created, be sure to get rid of the character ("*" or “-“) located at the beginning of the default name. Click “Save’ button
![](images/300/image300_6g.png)

9.	Close all applets


### Add Certificate to Keystore
1.	In your on-prem machine, open a terminal, select “Applications > Favorites > Terminal” from the top left corner of the screen
![](images/300/image300_7.png)

2.	Import the client side certificate into the cacerts in your system. Execute:
$JAVA_HOME/bin/keytool -import -alias uscom-central-1oraclecloudcom  -keystore $JAVA_HOME/jre/lib/security/cacerts -file /home/oracle/Certificate/uscom-central-1oraclecloudcom.crt
 
3.	Provide password: “changeit”
4.	Answer “yes” to question “trust this certificate?”


### Install Agent
1.	In the terminal you opened in your on-prem machine, create a directory for the agent, execute: mkdir dipcagent
3.	Move the downloaded file to that new directory, execute: mv Downloads/* dipcagent
4.	Move to the new directory, execute: cd dipcagent
5.	Unzip the file, execute: unzip agent-linux.64.bit.zip
6.	Move to the agent directory, execute: cd dicloud
7.	Execute command to install agent: 

./dicloudConfigureAgent.sh -recreate -debug -dipchost=**<YOUR_DIPC SERVER>** -dipcport=443 -user=**<YOUR_USER>** -password=**<YOUR_PASSWORD>** -authType=OAUTH2 -idcsServerUrl=https://idcs-bfb16122271a47fc91ada73842325e52.identity.oraclecloud.com -agentIdcsScope=https://919181A11AC6494CBCFA146EBC9A2A48.uscom-central-1.oraclecloud.com:443external -agentClientId=ec9958f548e7414f872cc480c0bf3455 -agentClientSecret=77321de1-6f4a-4997-945e-8acb271b0473

	where:

	<YOUR_DIPC SERVER> - This have been provided in your environment page. Look for entry DIPC SERVER.

	<YOUR_USER> - This is the login you use to log into Oracle Cloud/DIPC server

	<YOUR_PASSWORD> - This is the password of the login you use to log into Oracle Cloud/DIPC server

8.	New directories will be created, to look at then execute: ls
9.	We will take a look at the configuration file (agent.properties) and we will change the port in which this agent will talk to DIPC
10.	Open the editor, select “Applications > Accessories > Editor” from the top left corner of the screen
![](images/300/image300_9.png)
 
11.	Click on “Open” then “Other Documents”
![](images/300/image300_10.png)

12.	Browse to “/home/oracle/dipcagent/dicloud/agent/dipcagent001/conf/agent.properties”
![](images/300/image300_11.png)
 
13.	Modify agent port "agentPort" in parameter file "agent.properties" to 7010
![](images/300/image300_12.png)
 
14.	Save and close. Click on “Save” button then on "Close" icon (top right corner)


### Execute the Agent
1.	We will move to the directory with the necessary commands to start the agent; execute: cd agent/dipcagent001/bin
2.	We will start the agent by executing:
nohup ./startAgentInstance.sh &
3.	Your agent is now running. Go back to Firefox and look the “Agent” screen in DIPC
 
![](images/300/image300_14.png)


## On-Prem to On-Prem synchronization

### Verify Data in Source and Target DBs (Optional)
** MISSING STEPS
** Saves images 15 to 29 for this section


### Execute Data Synch Elevated Task
1. You should be logged into DIPC, if that is NOT the case, log in.
2. From the left side panel, SELECT "Home"

![](images/300/image300_30.png)

3. In the Home Page click "Create Connections" from the top section

![](images/300/image300_31.png)

4. Enter the following information:
    - Name: ONPREM_SRC
    - Description: Connection to on-prem database schema with source tables. AMER
	- Agent: <REMOTE_AGENT>
	- Type: Oracle
  	- Hostname: <COMPUTE_INSTANCE_IP>
	- Port: 1521
	- Username: AMER_SRC
	- Password: Welcome#123
	- Service Name: orcl
    - Schema Name: AMER_SRC (Default)	
5. Click on "Test Connection" button at the bottom. a green message should appear on top when everything is in order
6. Click on "Save"

![](images/300/image300_32.png)

7. From the top bar, open the drop-down menu and the select "Connection"

![](images/300/image300_34.png)

8. Enter the following information:
    - Name: ONPREM_TRG
    - Description: Connection to target schema onprem_trg EMEA
	- Agent: <REMOTE_AGENT>
	- Type: Oracle
	- Hostname: <COMPUTE_INSTANCE_IP>
	- Port: 1521
	- Username: EMEA_TRG
	- Password: Welcome#123
	- Service Name: orcl
    - Schema Name: EMEA_TRG (Default)	
5. Click on "Test Connection" button at the bottom. a green message should appear on top when everything is in order
6. Click on "Save"

![](images/300/image300_35.png)

7. From the top bar, open the drop-down menu and the select "Synchronize Data"

![](images/300/image300_37.png)

8. Enter the following information:
	- Name: Sync OnPrem Schemas
	- Description: Sync on-prem schemas AMER to EMEA
	- Connection: ONPREM_SRC
	- Schema: AMER
	- Connection: ONPREM_TRG
	- Schema: EMEA
	- Advanced - Include Initial Load: SELECTED
	- Advanced - Include Replication: SELECTED

** MISSING ![](images/300/image300_38.png)

9. Click on "Save & Run" button on the top right of the screen to execute the task
10. You will be navigated to teh "Jobs"screen. After some time, a message will appear in the notification bar

** MISSING ![](images/300/image300_39.png)

11. The job will automatically appear within the "Jobs" page

** MISSING ![](images/300/image300_40.png)

12. Click job to review details

** MISSING ![](images/300/image300_41.png)


### Verify Data in Target DB (Optional)
** MISSING STEPS
** MISSING IMAGES

## Summary
You have now successfully completed the Hands-on Lab, and have successfully installed the remote agent and synchronized two on-prem databases.