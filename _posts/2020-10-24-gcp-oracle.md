---
title: "How to connect to an Oracle database from GCP VM instance "
tags: 
  - GCP
  - Google Cloud Platform
  - Oracle
  - ODBC
  - JDBC
  - Python
---


# 	Accessing Oracle db from GCP instance



Google's cloud platform is great for scaling infrastructure and it makes computing and building models so much easier. Data can be easily loaded onto compute instances from Google's BigQuery using python libraries. But what happens when you don't have the data stored in GBQ, but in another server? In this post, I am gonna walk through the steps to install the drivers necessary to be able to directly pull data from an Oracle database by running python code.

# VM Instances set-up

When a notebook instance is created in GCP, it automatically creates a Virtual Machine (VM) instance in which the notebook will be located. The VM instance has Debian OS installed. So the steps to install the instant clients to access the drivers would involve command line entries.

## Debian OS

It's a Linux based OS very similar to Ubuntu. There might be slight changes to commands required like replacing 'yum' (in redhat based OS like fedora) with apt-get for installation. This tutorial makes installation on GCP VM instance straight-forward, as it is specifically created for Debian OS.

# Python requirements

To run the query on a database, we need to be able to establish a connection to the database. This is done through Database connectivity libraries. If the driver is Java based, then JDBC library like cx_Oracle should be installed. The other way is to install an ODBC library like pyodbc.

# Oracle Database structure

# What is an instant client?

Oracle is a proprietary database does not offer open source API abilities and restricts Oracle cloud license: http://www.oracle.com/us/corporate/pricing/cloud-licensing-070579.pdf. The licensing document by Oracle does not list GCP as its authorized vendor, which means we won't be able to access the Oracle database with the click of a few buttons. Although GCP does not support Oracle DB, it is still possible to access the Oracle database from a VM instance directly. 

We therefore, cannot access an Oracle database directly without from third party applications. It requires installation of an instant client which will act as an intermediary between  communicate to the Oracle driver and access the data.

Cue Instant clients. Instant client acts as the bridge between the third party platform (aka GCP instance) and the Oracle database. 

# Using JDBC instant client

Installing instant client from: https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html

Oracle provides rpm and zip files for installing the clients on a linux system. Debian needs files to be in .deb format to install. There are three ways to tackle this:

1. Install by converting rpm files to deb file

2. Install rpm directly using rpm file converter

3. Unzip file and work around the driver settings.

Detailed instructions for each option above is given in step 3.

# Detailed installation steps


## Step1: Download the clients from https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html

### a. Download zip / rpm files 

   ![image-20201026164019036](C:\Users\harini.palanisamy\AppData\Roaming\Typora\typora-user-images\image-20201026164019036.png)

   ![img/image-20201026164019036.png](/img/image-20201026164019036.png)

### b. (optional) Download ODBC package

   ![image-20201026164207873](C:\Users\harini.palanisamy\AppData\Roaming\Typora\typora-user-images\image-20201026164207873.png)

The rpm file needn't be downloaded. It is also possible to directly get the file from url (refer step 3). 

## Step 2: launch shell
-	Go to your Jupyter notebook instance -> upload zip/rpm files to the JCP instance home directory 
-	Click the '+' button (new laucher) -> terminal

## Step 3: Shell commands
There are 3 methods with few variations. My preferred method of installing the clients is Method 2.

In the terminal, enter the following in the same order:
### Method 1. Install by converting rpm files to deb file
```shell
bash
sudo mkdir -p /opt/oracle
```
Either enter the commands in Method 1a or 1b, as per your preference:
#### Method 1a. Direct url

```shell
cd /opt/oracle
sudo wget https://download.oracle.com/otn_software/linux/instantclient/199000/oracle-instantclient19.9-basic-19.9.0.0.0-1.x86_64.rpm
```

#### Method 1b. Uploaded file
For this step, remember to upload the rpm file to the home directory. The default home directory path for notebook instances is: `/home/jupyter/`
```shell
sudo mv *.rpm /opt/oracle
cd /opt/oracle
```
#### common steps for both methods
Install alien to convert rpm files to deb files
```
sudo apt-get update
sudo apt-get install alien
```
Convert to deb file and install:
```
sudo alien -k oracle-instantclient19.9-basic-19.9.0.0.0-1.x86_64.rpm --scripts
sudo dpkg -i oracle-instantclient19.9-basic_19.9.0.0.0-1_amd64.deb
```
Make sure the file names match with the actual files located in the folder. You can look at the file list inside a folder by using `ls` command. 

### Method 2. Install rpm directly using rpm file converter
In the terminal, enter the following, in order:
```shell
bash
sudo mkdir -p /opt/oracle
```
Again, two ways possible. Either enter the commands in Method 2a or 2b, as per your preference:
#### Method 2a. Direct url

```shell
cd /opt/oracle
sudo wget https://download.oracle.com/otn_software/linux/instantclient/199000/oracle-instantclient19.9-basic-19.9.0.0.0-1.x86_64.rpm
```
#### Method 2b. Uploaded file
For this step, remember to upload the rpm file to the home directory. The default home directory path for notebook instances is: `/home/jupyter/`
```shell
sudo mv *.rpm /opt/oracle
cd /opt/oracle
```
#### common steps for both methods
This is where there is slight difference between method 1 and method 2. Instead of converting rpm file to deb file and then installing the package, we can directly install using alien.

Install alien to install rpm files
```
sudo apt-get update
sudo apt-get install alien
```
Install:
```
sudo alien -i oracle-instantclient19.9-basic-19.9.0.0.0-1.x86_64.rpm --scripts
```
Make sure the file names match with the actual files located in the folder. You can look at the file list inside a folder by using `ls` command. 

### Method 3. Unzip file and work around the driver settings.
Enter the commands in the same order:
```shell
bash
sudo mkdir -p /opt/oracle
```
```shell
sudo mv *.zip /opt/oracle
cd /opt/oracle
```
```shell
sudo unzip instantclient-basic-linux.x64-19.9.0.0.0dbru.zip
```

## Step 4: Install dependencies and set environment variables

If you followed method 3 in step 3, enter the below line, else, skip.
```shell
sudo apt-get update
```
install libaio1:
```shell
sudo apt-get install libaio1
```
set path:
```shell
cd /home/jupyter/
sudo nano .bashrc
export LD_LIBRARY_PATH=/opt/oracle/instantclient_19_9:$LD_LIBRARY_PATH
source .bashrc
```
Well, that's it, if you want to use a JDBC library like cx_Oracle. When you want to establish a connection, enter the following:

```python
import cx_Oracle
cx_Oracle.connect(username/password@DataBasePath:Port/DataSourceName")
```
And you are all set to go.

## Step 5: Additional steps for ODBC based library




   https://www.howtoforge.com/converting_rpm_to_deb_with_alien

  
      sudo apt-get install alien
      sudo wget https://download.oracle.com/otn_software/linux/instantclient/199000/oracle-instantclient19.9-basic-19.9.0.0.0-1.x86_64.rpm
      
      sudo wget https://download.oracle.com/otn_software/linux/instantclient/199000/oracle-instantclient19.9-odbc-19.9.0.0.0-1.x86_64.rpm
      ```

   5. ```
      sudo alien -k oracle-instantclient19.9-basic-19.9.0.0.0-1.x86_64.rpm --scripts
      
      sudo alien -k oracle-instantclient19.9-odbc-19.9.0.0.0-1.x86_64.rpm --scripts
      
      sudo dpkg -i oracle-instantclient19.9-basic_19.9.0.0.0-1_amd64.deb
      sudo dpkg -i oracle-instantclient19.9-odbc_19.9.0.0.0-1_amd64.deb 
      ```

   6. 

   ```
   sudo alien -i oracle-instantclient19.9-basic-19.9.0.0.0-1.x86_64.rpm --scripts
   
   sudo alien -i oracle-instantclient19.9-odbc-19.9.0.0.0-1.x86_64.rpm --scripts
   ```

   1. 

   2. 

   3. no need to install. just unzip packages

   4. download zip files from link (directly unzipping didn't work for me, may be some url tweaking is required)

   5. upload the zipfiles to home dir

   6. move them to /opt/oracle

   7. ```
      sudo apt-get install libaio1
      ```

   8. ```
      export LD_LIBRARY_PATH=/opt/oracle/instantclient_19_9
      ```

   9. 

   10. odbc config

   11. https://blog.devart.com/installing-and-configuring-odbc-driver-on-linux.html

   /etc/odbcinst.ini might be the key

   -install fresh. cx and odbc separately

   - odbc - j
   - /etc/odbcinst.ini for odbc

   -cx - just follow instructions

   ## 10/24 mrng

   inst basic, .bash file for env var, odbc

   1. upload zip file(s)
   2. sudo mkdir -p /opt/oracle
   3. sudo mv *.zip /opt/oracle/
   4. cd /opt/oracle
   5. sudo unzip instantclient-basic-linux.x64-19.9.0.0.0dbru.zip
   6. sudo apt-get install libaio1

   Instead of the following:

   1. ```
      vim ~/.bashrc
      
      export LD_LIBRARY_PATH=/opt/oracle/instantclient_19_8:$LD_LIBRARY_PATH 
      
      :wq!
      source ~/.bashrc
      ```

   2. do this instead:

   3. ```
      sudo sh -c "echo /opt/oracle/instantclient_19_9 > \
            /etc/ld.so.conf.d/oracle-instantclient.conf"
        sudo ldconfig
      ```

   good to go for cx_Oracle.

   I still get time-out error. which shd be sorted with devops

   for odbc: odbc_update_ini.sh <ODBCDM_Home> [<Install_Location> <Driver_Name> ]

   ```
   odbc_update_ini.sh /etc /opt/oracle/ oracleodbc dssprod /home/jupyter/.odbc.ini
   ```

# Resources

Useful resources based on which this tutorial is constructed:

Oracle's instant client download links based on OS: https://www.oracle.com/database/technologies/instant-client/downloads.html

Extra steps for ODBC client: https://www.oracle.com/database/technologies/releasenote-odbc-ic.html

Oracle's detailed call interface documentation: https://docs.oracle.com/en/database/oracle/oracle-database/19/lnoci/instant-client.html#GUID-AAB0378F-2C7B-41EB-ACAC-18DD5D052B01

# Steps Overview

1. Install JDBC client
2. Insall ODBC client
3. Check if drivers are installed and connected
4. Update odbcinst.ini and odbc.ini (in case of ODBC drivers)

## what I remember

1. I installed drivers, set env vars with export, ldd ,etc. I uploaded dns config files (tnsnames.ora,sql, oracle.sh)
2. 

Next step: trying first with pyodbc library..

### pyodbc

```
Error: ('01000', "[01000] [unixODBC][Driver Manager]Can't open lib 'driver_name' : file not found (0) (SQLDriverConnect)")
```

1. driver not found - should either install driver or install instant client to access driver
2. odbcinst -j #to get the odbc driver installed details
3. nano /path/to/file #to open/view files

<details class="details-reset details-overlay details-overlay-dark" id="jumpto-line-details-dialog" style="box-sizing: border-box; display: block;"><summary data-hotkey="l" aria-label="Jump to line" role="button" style="box-sizing: border-box; display: list-item; cursor: pointer; list-style: none;"></summary></details>



 

# Steps

1. follow instructions
2. odbcisnt -j
3. update odbcinst.ini and odbc.ini 



## what I remember

1. I installed drivers, set env vars with export, ldd ,etc. I uploaded dns config files (tnsnames.ora,sql, oracle.sh)
2. 

# commands I might have used

1. ldconfig
2. 





## Starting fresh

1. create a VM/notebook instance - skip if you already have one
2. edit vm settings - networking -> select 'Networks shared with me (from bnile-ops-vpn-dev)' - its available for bnile-analysts project
3. select dev-net-us-west1-1 in shared subnetwork
4. click save/create

Next step: trying first with pyodbc library..

### pyodbc





```python
---------------------------------------------------------------------------
Error                                     Traceback (most recent call last)
<ipython-input-9-c3b163270826> in <module>
      1 dssprod_con = pyodbc.connect(Driver='{Oracle in OraClient11g_home1}', 
----> 2                              DBQ='dssprod.BLUENILE.COM', UID='harini.palanisamy', PWD='9Bluenile9')

Error: ('01000', "[01000] [unixODBC][Driver Manager]Can't open lib 'Oracle in OraClient11g_home1' : file not found (0) (SQLDriverConnect)")
```

1. driver not found - should either install driver or install instant client to access driver
2. odbcinst -j  #to get the odbc driver installed details
3. nano /path/to/file #to open/view files

## mysql connector/python



1. https://docs.oracle.com/cd/E17952_01/connector-python-en/index.html

2. installed: mysql server using pip

3. ```python
   DatabaseError: 2003 (HY000): Can't connect to MySQL server on 'dssprod.bluenile.com' (110)
   ```

   Maybe mysql is not oracle sql. smh back to cx_Oracle

   

   ## cx_Oracle

   holy grail: https://cx-oracle.readthedocs.io/en/latest/user_guide/installation.html

   - Install driver from : https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html

   1. but only rpm format is available. we need .deb files for debian os

      1. https://www.howtoforge.com/converting_rpm_to_deb_with_alien 
      2. or just unzip a zip file and install. smh

   2. ```shell
      sudo mkdir -p /opt/oracle
      ```

   3. ```shell
      cd /opt/oracle
      ```

   4. ```
      sudo apt-get install alien
      sudo wget https://download.oracle.com/otn_software/linux/instantclient/199000/oracle-instantclient19.9-basic-19.9.0.0.0-1.x86_64.rpm
      
      sudo wget https://download.oracle.com/otn_software/linux/instantclient/199000/oracle-instantclient19.9-odbc-19.9.0.0.0-1.x86_64.rpm
      ```

   5. ```
      sudo alien -k oracle-instantclient19.9-basic-19.9.0.0.0-1.x86_64.rpm --scripts
      
      sudo alien -k oracle-instantclient19.9-odbc-19.9.0.0.0-1.x86_64.rpm --scripts
      
      sudo dpkg -i oracle-instantclient19.9-basic_19.9.0.0.0-1_amd64.deb
      sudo dpkg -i oracle-instantclient19.9-odbc_19.9.0.0.0-1_amd64.deb 
      ```

   6. 

      ```
      sudo alien -i oracle-instantclient19.9-basic-19.9.0.0.0-1.x86_64.rpm --scripts
      
      sudo alien -i oracle-instantclient19.9-odbc-19.9.0.0.0-1.x86_64.rpm --scripts
      ```

   7. ```
      sudo apt-get update
      sudo apt-get install libaio1
      ```

      

   8. 

   9. no need to install. just unzip packages

   10. download zip files from link (directly unzipping didn't work for me, may be some url tweaking is required)

   11. upload the zipfiles to home dir

   12. move them to /opt/oracle

   13. ```
       sudo apt-get install libaio1
       ```

   14. ```
       export LD_LIBRARY_PATH=/opt/oracle/instantclient_19_9
       ```

   15. 

   16. odbc config

   17. https://blog.devart.com/installing-and-configuring-odbc-driver-on-linux.html

   /etc/odbcinst.ini might be the key

   

   -install fresh. cx and odbc separately

   - odbc - j 
   - /etc/odbcinst.ini for odbc

   -cx - just follow instructions

   

   ## 10/24 mrng 

   inst basic, .bash file for env var, odbc

   1. upload zip file(s)
   2. sudo mkdir -p /opt/oracle
   3. sudo mv *.zip /opt/oracle/
   4. cd /opt/oracle
   5. sudo unzip instantclient-basic-linux.x64-19.9.0.0.0dbru.zip
   6. sudo apt-get install libaio1

   Instead of the following:

   1. ```shell
      vim ~/.bashrc
      
      export LD_LIBRARY_PATH=/opt/oracle/instantclient_19_8:$LD_LIBRARY_PATH 
      
      :wq!
      source ~/.bashrc
      
      ```

   8.  do this instead:

   9. ```
      sudo sh -c "echo /opt/oracle/instantclient_19_9 > \
            /etc/ld.so.conf.d/oracle-instantclient.conf"
        sudo ldconfig
      ```

   good to go for cx_Oracle.

   I still get time-out error. which shd be sorted with devops

   for odbc: odbc_update_ini.sh <ODBCDM_Home> [<Install_Location> <Driver_Name> <DSN> <ODBCINI>]

   ```
   odbc_update_ini.sh /etc /opt/oracle/ oracleodbc dssprod /home/jupyter/.odbc.ini
   ```

   dsn = oracle

   driver_name = oracle_driver

   username=harini.palanisamy

   odbcinst -j

   

   1. 
   2. 
   3. rm , rm -d
   4. touch 
