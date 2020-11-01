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

# 	Accessing Oracle DB from GCP instance

Google's cloud platform is great for scaling infrastructure and it makes computing and building models so much easier. Data can be easily loaded onto compute instances from Google's BigQuery using python libraries. But what happens when you don't have the data stored in Google Cloud, but in another server? In this post, I am gonna walk through the steps to install the drivers necessary to be able to directly pull data from an Oracle database in using python libraries.

# VM Instances set-up

When a notebook instance is created in GCP, it automatically creates a Virtual Machine (VM) instance in which the notebook will be run. The VM instance has Debian OS installed. So the steps to install the instant clients needed to access the drivers would involve command line entries.

## Debian OS

It's a Linux based OS very similar to Ubuntu. This tutorial makes installation on GCP VM instance straight-forward, as it is specifically created for Debian OS.

# Python requirements

To run the query on a database, we need to be able to establish a connection to the database. This is done through Database connectivity libraries. If the driver is Java based, then JDBC library like cx_Oracle should be installed. The other way is to install an ODBC library like pyodbc.

# Oracle Database

If the Python installation is on the same machine as the database, a separate Oracle Instant Client installation is not required, because Oracle Database Client libraries come with an Oracle Database installation. Since we want to access the database from different machine, aka, Google Cloud VM instance, we will go through the steps to install the client libraries.

# What is an instant client?

Oracle is a proprietary database does not offer open source API abilities and restricts Oracle cloud license: http://www.oracle.com/us/corporate/pricing/cloud-licensing-070579.pdf. The licensing document by Oracle does not list GCP as its authorized vendor, which means we won't be able to access the Oracle database with the click of a few buttons, or with API key and passcode. Although GCP does not support Oracle DB, it is still possible to access the Oracle database from a VM instance directly. It requires installation of a client to communicate to the Oracle driver to access the data. Instant client acts as the bridge between the third party platform (aka GCP instance) and the Oracle database. 

# Using JDBC instant client

Installing instant client from: https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html

Oracle provides rpm and zip files for installing the clients on a Linux system. Debian needs files to be in .deb format to install. There are three ways to tackle this:

1. Install by converting rpm files to deb file

2. Install rpm directly using rpm file converter

3. Unzip file and work around the driver settings.

Detailed instructions for each option above is given in step 3.

# Detailed installation steps

## Step0: Configure network settings

Create a VM/notebook instance and ensure the firewall settings allow for connection to Oracle database.

## Step1: Download the clients from https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html

### a. Download zip / rpm files 

Download either the zip file or the rpm file in the link. Either Basic Package or the Basic Light Package should be fine. But I have used the Basic Package files for the purpose of this tutorial.

  ![img/image-20201026164019036.png](/img/image-20201026164019036.png)

### b. Download ODBC package (optional)

If you would like to work with ODBC library rather than JDBC library, the you also need to install an ODBC client package.

    ![img/image-20201026164207873.png](/img/image-20201026164207873.png)

The rpm file needn't be downloaded. It is also possible to directly get the file from url (refer step 3). 

## Step 2: launch shell

- Go to your Jupyter notebook instance -> upload zip/rpm files to the GCP instance home directory 

  ![/img/image-20201101101954057.png](/img/image-20201101101954057.png)

- Click the '+' button (new laucher) -> launch terminal

  ![/img/image-20201101102202405.png](/img/image-20201101102202405.png)

## Step 3: Shell commands

There are 3 methods with few variations for installing the instant clients. Method 1 and 2 uses rpm files and Method 3 uses zip files for installing the client packages. Method 1 & 2 are very similar and I have included Method 1 mostly as a learning exercise. It might help you understand the differences in rpm,deb and zip files in the context of Debian OS, if you are interested. My preferred method of installing the clients is Method 2.

In the terminal, enter the following in the same order:

### Method 1. Install by converting rpm files to deb file

```shell
bash
sudo mkdir -p /opt/oracle
```

Enter the commands in either Method 1a, or 1b, as per your preference:

#### Method 1a. Direct url

```shell
cd /opt/oracle
sudo wget https://download.oracle.com/otn_software/linux/instantclient/199000/oracle-instantclient19.9-basic-19.9.0.0.0-1.x86_64.rpm
```

#### Method 1b. Uploaded file

For using this method, remember to upload the rpm file to the home directory. The default home directory path for notebook instances is: `/home/jupyter/`

```shell
sudo mv *.rpm /opt/oracle
cd /opt/oracle
```

#### Common steps for both methods

Install alien to convert rpm files to deb files

```shell
sudo apt-get update
sudo apt-get install alien
```

Convert to deb file and install:

```shell
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

Remember to upload the rpm file to the home directory. The default home directory path for notebook instances is: `/home/jupyter/`

```shell
sudo mv *.rpm /opt/oracle
cd /opt/oracle
```

#### Common steps for both methods

This is where there is slight difference between method 1 and method 2. Instead of converting rpm file to deb file and then installing the package, we can directly install using alien.

Install alien to install rpm files

```shell
sudo apt-get update
sudo apt-get install alien
```

Install:

```shell
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


## Step 5: Additional steps for ODBC based library (optional)

This step is for installing Oracle's ODBC client package to enable an ODBC library can access the database.

``` shell
cd /opt/oracle/
```

sudo wget https://download.oracle.com/otn_software/linux/instantclient/199000/oracle-instantclient19.9-odbc-19.9.0.0.0-1.x86_64.rpm


```shell
sudo alien -i oracle-instantclient19.9-odbc-19.9.0.0.0-1.x86_64.rpm --scripts
```

Similar to JDBC client installation, this step can be done by unzipping as well.

## Step 6: Configuring ODB driver (if you followed step 5)

If you installed it with rpm file, then follow these steps to configure the driver.

Type the following to know the locations of the files required for the odbc driver:

```shell
odbcisnt -j
```

There are two files that needs to be configured. odbcinst.ini and odbc.ini

We can go and update the files in the installation folder under /etc/.. but let's just make changes in the default files that the client will look for, in case there are no settings configured in the installation folder.

```shell
sudo nano /etc/odbcinst.ini
```



<img src="C:\Users\harini.palanisamy\AppData\Roaming\Typora\typora-user-images\image-20201101104145557.png" alt="image-20201101104145557" style="zoom:50%;" />

If you used the zip file, enter the following: 

`[a_name_for_the_driver]`

`Driver=/opt/oracle/instantclient_19_9/libsqora.so.19.1`

enter `Ctrl + o ` `Enter` `Ctrl + x`

Make sure the names of the folder/files are same for the version that you are installing.

If you get 'file not found' error while connecting to the driver, the problem lies in the path of the driver. Locate the path where 'libsqora.so.*' file is located and point the driver to that path. If you followed the rpm methods, then the path probably looks like this:

`/usr/lib/oracle/19.9/client64/lib/libsqora.so.19.1`

To configure odbc.ini:

```shell
sudo nano /etc/odbc.ini
```

Enter the values as shown in the image. The value for driver should match the name you gave in the `odbcinst.ini` file. The value inside `[ ]` in this file is the DSN name. You can also set your username and password to connect to the database here.

After entering the values, enter: `Ctrl+o` `Enter` `Ctrl+x`


# Connecting to Oracle database

Import either an ODBC or JDBC connection library.

ODBC:

```python
import pyodbc # for ODBC client
print(pyodbc.drivers()) # This line should print the driver that you initialized in odbcinst.ini and odbc.ini files
```

```pytho
odbc_con = pyodbc.connect(Driver='{oracle_driver_name}', DBQ='dbq_name', UID='username', PWD='password')
# You can skip the username and pwd parameters if you had already passed it in the odbc.ini file
```

JDBC:

```python
import cx_Oracle # forJDBC client
jdbc_con = cx_Oracle.connect("username/password@hostname:port/databasename"
```


# Possible errors and some solutions

1. When you import, if you get a DatabaseError, contact dev ops. It's a network connection error and most often, just a simple refresh from their side will work

2. If you get 'file not found' error when connecting to odbc/jdbc drivers:

   - Check file paths and the file paths set in the environment variables. use `ls` and `cd` to find and locate the files. `libsqora.so` is the driver file for odbc and `libclntsh.so`. Locate these files and point the driver to these files based on which driver you want to use.

   - For JDBC connection, try this:

     ```shell
     sudo sh -c "echo /usr/lib/oracle/19.9/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf"
     sudo ldconfig
     ```

     in place of this:

     ```shell
     cd /home/jupyter/
     sudo nano .bashrc
     export LD_LIBRARY_PATH=/opt/oracle/instantclient_19_9:$LD_LIBRARY_PATH
     source .bashrc
     ```

     
# Resources

Useful resources based on which this tutorial is constructed:

Oracle's instant client download links based on OS: https://www.oracle.com/database/technologies/instant-client/downloads.html

Extra steps for ODBC client: https://www.oracle.com/database/technologies/releasenote-odbc-ic.html

Oracle's detailed call interface documentation: https://docs.oracle.com/en/database/oracle/oracle-database/19/lnoci/instant-client.html#GUID-AAB0378F-2C7B-41EB-ACAC-18DD5D052B01
