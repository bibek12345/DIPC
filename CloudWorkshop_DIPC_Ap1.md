# Appendix 1: SSH Session Configuration

## Windows OS

### Create SSH Configuration

1.	Start PuTTY

![](images/Ap1/imageAp1_10.png)
 

2.	In the screen provide the following information:
    -  Host: **{SERVER_IP_ADDRESS}**
    - Saved Session: Provide a name 

where:

{SERVER_IP_ADDRESS} - is teh IP address to the server you want to connect (DIPC server, OnPrem server)

![](images/Ap1/imageAp1_20.png)
 
3.	Click on “Save” button
4.	From the hierarchical panel on the left, select “Connection > Data” and provide the following information:
    - Auto-login username: opc

![](images/Ap1/imageAp1_30.png)

5.	From the hierarchical panel on the left, select “Connection > SSH > Auth”. Click on “Browse”  button to select the private key (PPK) file

 ![](images/Ap1/imageAp1_40.png)

6.	Navigate to the directory in which you copied the files provided for the labs and select “dipcdemoSSH.ppk”. Click on “Open” button

 ![](images/Ap1/imageAp1_50.png)

7.	From the hierarchical panel on the left, select “Connection > SSH > Tunnels” and provide the following information:
    - Source port: 6905
    - Destination: **{SERVER_IP_ADDRESS}**:5901

where:

{SERVER_IP_ADDRESS} - is teh IP address to the server you want to connect (DIPC server, OnPrem server)

8.	Click on “Add” button

 ![](images/Ap1/imageAp1_60.png)

9.	From the hierarchical panel on the left, select “Sessions” (on top) and then click on “Save” button. This will save this configuration so you can re-use it.

 ![](images/Ap1/imageAp1_70.png)

10.	Click on “Open” button to start your SSH session. 

 ![](images/Ap1/imageAp1_80.png)



## Load Existing Configuration
If you have saved the SSH configuration previously.

1.	Start PuTTY

![](images/Ap1/imageAp1_10.png)

2.	Select your configuration in the “Saved Sessions” section then click on “Load” button

 ![](images/Ap1/imageAp1_90.png)

3.	Click on “Open” button to start your SSH session. 
 
![](images/Ap1/imageAp1_100.png) 



## Mac OS
### Prerequisite

1.	If Homebrew is NOT installed on your machine, execute:

/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

2. Install PuTTY via Homebrew, execute:

brew install putty

3. Convert the PPK to PEM, execute:

puttygen **{dipc_demo_keys.ppk}** -O private-openssh -o **{nameOfNewPemFile.pem}**

where:

{dipc_demo_keys.ppk} - is the private key (PPK) file provided by the instructor

{nameOfNewPemFile.pem} - is the name you want to assign to the key file. Respect the "pem" extension.

4. Verify that the file you just created with the key have permissions of 600. If this is NOT the case, execute:

chmod 600 **{nameOfNewPemFile.pem}**

where:

{nameOfNewPemFile.pem} - is the name you assigned to the key file.

### Start the SSH session

1. To start the SSH session, execute:

ssh -i **{pathtokeyfile}**/**{nameOfNewPemFile.pem}** -L 6905:localhost:5901 opc@**{SERVER_IP_ADDRESS}**

where:

{pathtokeyfile} - is path to the file you created in the previous section step 3.

{nameOfNewPemFile.pem} - is the name you assigned to the key file.

{SERVER_IP_ADDRESS} - is the IP address of the server you want to connect (DIPC server, OnPrem server)
