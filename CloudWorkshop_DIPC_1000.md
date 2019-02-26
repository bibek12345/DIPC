# Lab 1000 - Replicate Data Elevated Task For Autonomous Data Warehouse
![](images/1000/image1000_0.png)

## Before You Begin

### Objectives
- Review how to create connections
- Review how to execute a Replicate Data elevated task for Autonomous Data Warehouse

### Time to Complete 
Approximately 60 minutes.

### What Do You Need?
Your will need:
- Oracle Managed DIPC Instance URL
- DIPC User and Password
- DB information for source system: server name, user/password and service name
- DB information for target system: client credentials file, admin user password and service names
- SQL Developer
- General understanding of RDBMS and data integration concepts

## Configure Oracle Autonomous Data Warehouse Cloud for Replication
In the Oracle Autonomous Data Warehouse Cloud database, complete the following tasks:

1. Oracle Autonomous Data Warehouse Cloud has a pre-existing database user created for Oracle GoldenGate called ggadmin. The ggadmin user has been granted the right set of privileges for Oracle GoldenGate Replicat to work. By default, this user is locked. To unlock the ggadmin user, connect to your Oracle Autonomous Data Warehouse Cloud database as the ADMIN user using any SQL client tool.

For example, in SQL Developer 18.3 and higher in the Connection Type field select the value Cloud Wallet that allows you to enter a credentials file. SQL Developer then presents a list of the available connections in the Service field (the connections are included in the credentials files).

If your application provides support for wallets or provides specific support for an Autonomous Data Warehouse connection,for example, Oracle SQL Developer, Oracle recommends that you use that type of connection.

![](images/1000/image1000_1.png)

2. Run the alter user command to unlock the ggadmin user and set the password for it.
    ```
    alter user ggadmin identified by <password> account unlock;
    ```
3. Create the Target Schema for the data replication.
    ```
    create user sales_tgt identified by password;
    grant create session, resource, create view, create table to sales_tgt;
    ```
4. Connect to Oracle Autonomous Data Warehouse Cloud database as the sales_tgt user and create target tables for which DDL replication is not enabled

## Obtain Oracle Autonomous Data Warehouse Cloud client credentials file.
To establish connection to your Oracle Autonomous Data Warehouse Cloud database, you can download the client credentials file from the Oracle Autonomous Data Warehouse Cloud service console.

    ```
    Note: If you do not have administrator access to Oracle Autonomous Data Warehouse Cloud, you should ask your 
    service administrator to download and provide the client credentials file to you.
    ```
1. Log into your Oracle Autonomous Data Warehouse Cloud account.
2. From the Instance page, click the menu option for the Oracle Autonomous Data Warehouse Cloud instance and select Service Console.
3. Log into the service console using the admin username and its associated password.
4. In the service console, click the Administration tab.
5. Click Download Client Credentials.
6. Enter a password to secure your client credentials file and click Download.
7. Save the client credentials file to your local system.

The client credentials file contains the following files:
- cwallet.sso
- ewallet.p12
- keystore.jks
- ojdbc.properties
- sqlnet.ora
- tnsnames.ora
- truststore.jks

You refer to the sqlnet.ora and tnsnames.ora files while configuring Oracle Data Integration Platform Cloud to work with Oracle Autonomous Data Warehouse Cloud.

## Configure Oracle Data Integration Platform Cloud for replication.
Complete the following tasks in the system where ADIPC agent which will be used to connect to ADWC in configured, In this Lab we will be using a compute instance COMPUTE_DIPC01 to configure the DIPC agent:

1. Transfer the client credentials file that you downloaded from Oracle Autonomous Data Warehouse Cloud to your compute instance

![](images/1000/image1000_2.png)

2. In the compute instance, unzip the client credentials file into a new directory. For example, /home/oracle/adwc_credentials. This will be your key directory

![](images/1000/image1000_3.png)

3. To configure the connection details, We need to create the tns files in the DIPC agent. Copy sqlnet.ora and tnsnames.ora files from the client credentials to /home/oracle/dicloud/oci/ location

![](images/1000/image1000_4.png)

4. Edit the tnsnames.ora file in the /home/oracle/dicloud/oci/ location to include the connection details that is available in the tnsnames.ora file in your key directory (the directory where you unzipped the client credentials file downloaded from Oracle Autonomous Data Warehouse Cloud).

```
Note: The tnsnames.ora file provided with the client credentials file contains three database service names 
identifiable as:
ADWC_Database_Name_low
ADWC_Database_Name_medium
ADWC_Database_Name_high

For Oracle GoldenGate replication, use ADWC_Database_Name_low
```

![](images/1000/image1000_5.png)

5. Edit this sqlnet.ora file to include your key directory.

![](images/1000/image1000_6.png)

## Log into DIPC Server

### Login into DIPC using Oracle Cloud Services Dashboard

1. In your web browser, navigate to cloud.oracle.com, then click Sign in.

2. Provide the cloud account: orasenatdpltintegration02 then **\<Enter\>**

3. Provide your user name and password, then click "Sign In" button Or You can log in with Oracle SSO. You will land in the Dasboard screen.

![](images/1000/image1000_7.png)

4. In the Dasboard search for  "Autonomous Data Integration Platform Cloud" and click on the service.
 
 ![](images/1000/image1000_8.png)

5. Click on the hamburger menu of the DIPC server assigned to you, then click "Data Integration Platform Console" ![]

 ![](images/1000/image1000_9.png)

You will be navigated to your DIPC server Home page. ![](images/1000/image1000_10.png)

### Login into DIPC using direct URL

1. Open a browser window an provide your DIPC server URL. The URL will be provided by the instructor and will look like this one "https://dipc01-orasenatdpltintegration02.adipc.ocp.oraclecloud.com/dicloud".

2. Provide your user name and password, then click "Sign In" button ![](images/1000/image1000_7.png)
You will be navigated to your DIPC server Home page.


## Create Connections and Review Catalog

1. Log into your Workshop DIPC Server.

2. For synchronization jobs we will need a CDB (Container DB) connection to our DB. In the Home Page click the “Create" button in the "Connection” box from top section. 

![](images/1000/image1000_11.png)

3.	Enter the following information
    - Name: SRC_CDB
    - Description: CDB User for Source DB
    - Agent: **\<LOCAL_AGENT\>**
    - Type: Oracle CDB
    - Hostname: **\<SOURCE_DB_NAME\>**
    - Port: 1521
    - Username: C##GGSRC
    - Password: Wel_Come#123
    - Service Name: **\<CDB_SOURCE_SERVICE_NAME\>**

![](images/1000/image1000_12.png) 

    ```
    where:
        <LOCAL_AGENT> - Select the local DIPC agent 
        <SOURCE_DB_NAME> - Name of the source database server. This have been provided in your environment page; look for entry SOURCE_DB_NAME
        <CDB_SOURCE_SERVICE_NAME> - CDB Service name string for the source database server. This have been provided in your environment page; look for entry CDB_SOURCE_SERVICE_NAME
    ```
4. Click "Test Connection" button and when the test is successful click "Save" button.

5. Open the drop-down menu from the top far right corner and then select “Connection”. 

![](images/1000/image1000_13.png)

6. Enter the following information:
    - Name: SALES_SRC
    - Description: Sales OLTP Source Data
    - Agent: **\<LOCAL_AGENT\>**
    - Type Oracle: selecting Oracle will expand the Connection Settings ![](images/1000/image1000_14.png)
    - Hostname: **\<SOURCE_DB_NAME\>**
    - Port: 1521
    - Username: SALES_SRC
    - Password: Wel_Come#123
    - Service Name: **\<SOURCE_DB_SERVICE_NAME\>**
    - Schema Name: SALES_SRC (Default) – When you try to select the schema, you are testing the connection at the same time
    - CDB Connection: SRC_CDB 
    ![](images/1000/image1000_15.png)
    ```
    where:
        <LOCAL_AGENT> - Select the local DIPC agent 
        <SOURCE_DB_NAME> - Name of the source database server. This have been provided in your environment page; look for entry SOURCE_DB_NAME
        <SOURCE_DB_SERVICE_NAME> - Service name string for the source database server. This have been provided in your environment page; look for entry SOURCE_DB_SERVICE_NAME
    ```
7. Click "Test Connection" button and when the test is successful click "Save" button. DIPC will create the connection and will harvest the entities in the schema. You will be navigated to the Catalog and you will see, after some time, the connection you just created and the entities in that schema
    
    **Note: Data Entities are harvested and profiled at the time the connection is created, their popularity is also calculated by reviewing the DB query logs. This process may take some time (5 minutes or so), the Catalog will show a message when new updates are available**

    ![](images/1000/image1000_16.png)

8.	Now, we are going to create the target connection for Autonomous Data Warehouse. Open the drop-down menu from the top far right corner and then select “Connection”  ![](images/1000/image1000_13.png)

9.	Enter the following information:
    - Name: ADWC_TGT 
    - Description: Connection for ADWC Target
    - Agent: **\<LOCAL_AGENT\>**
    - Type : Oracle Autonomous Data Warehouse Cloud
    - Username: ggadmin 
    - Password: Wel_Come#123
    - Credential File : **\<Upload the creadential file downloaded\>**
    - Connection URL : **\<Select from drop down\>**
    - Service Name: dipcadw_low
    - Schema Name: SALES_TGT  (Default)
    ```
    where:
        <LOCAL_AGENT> - Select the local DIPC agent 
              
    ```
    ![](images/1000/image1000_17.png)

    ![](images/1000/image1000_18.png)

10. Click "Test Connection" button and when the test is successful click "Save" button. DIPC will create the connection and will harvest the entities in the schema. You will be navigated to the Catalog and you will see, after some time, the new connection you just created and the entities in that schema (if any)


## Create Replicate Data Elevated Task
1.	Connections have been defined. We are ready to create and execute our “Replicate Data” elevated task. From the top bar, open the drop-down menu from the top far right corner and then select “Replicate Data” 

![](images/1000/image1000_13.png) 

2.	Provide the following information and Click Design:
    - Name: Replicate Data to ADWC
    - Identifier: Replicate Data to ADWC  
    - Description: Replicate Data to ADWC 

    **Note: If you run into any issues when trying to select a Connection refresh the page manually. The Schemas may take some time to appear as well, this is expected.**

    ![](images/1000/image1000_19.png) 
    
3. Now provide the Source Schema connection in the Connection drop down box

    ![](images/1000/image1000_20.png) 

4. Next in the schemas tab select the source schema : SALES_SRC

    ![](images/1000/image1000_21.png) 

5. Same way now select the target and target ADWC connection in the Connection drop down box

    ![](images/1000/image1000_22.png) 

6. Now in the Schemas/Topics tab select the target schema : SALES_TGT and click Save & Run

    ![](images/1000/image1000_23.png) 

7. After this you should be able to see the Job created in the Monitor Section in DIPC console
    
    ![](images/1000/image1000_24.png) 

    ![](images/1000/image1000_25.png) 

## Testing the Data Replication from Source to ADWC Target

1. If you log in to the compute instance where the DIPC agent is configured. You can find the Goldengate Processes created for
   Replicate data task.

    ![](images/1000/image1000_26.png) 


2. Now lets log in to the source DB and create a table and see it getting replicated. 

    ![](images/1000/image1000_27.png) 

    ![](images/1000/image1000_28.png) 

3. These changes are captured by the extract

    ![](images/1000/image1000_29.png) 

4. The should appear in the ADWC target :

    ![](images/1000/image1000_30.png) 

5. I will insert one record to the new table at the source Database and then update it.

    ![](images/1000/image1000_31.png) 

6. Changes will appear in the target ADWC 

    ![](images/1000/image1000_32.png) 

7. We can go to the Monitor section in the DIPC Console and go to the job and see the statistics in the cosole

    ![](images/1000/image1000_33.png) 


```
Using DIPC for replicating to ADWC below link as the reference :
https://docs.oracle.com/en/cloud/paas/data-integration-platform-cloud/using/replicate-data-oracle-autonomous-data-warehouse-cloud.html
```
