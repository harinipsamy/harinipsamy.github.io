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

Google's cloud platform is great for scaling infrastructure and it makes computing and building models so much easier. Data can be easily loaded onto compute instances from Google's BigQuery using python libraries. But what happens when you don't have the data stored in GBQ, but in another server? In this post, I am gonna walk through the steps to install the drivers necessary to be able to directly pull data from an Oracle database by running python code.

# VM Instances set-up

When a notebook instance is created in GCP, it automatically creates a Virtual Machine (VM) instance in which the notebook will be located. The VM instance has Debian OS. So the steps to install the drivers would involve commandline entries. 

## Debian OS

It's a Linux based OS very similar to Ubuntu. There might be slight changes to commands required like replacing 'yum' (in redhat based OS like fedora) with apt-get. This tutorial makes installation straight-forward, as it is specifically created for Debian OS


# Python requirements

To run the query on a database, we need to be able to establish a connection to the database. This is done through Database connectivity libraries. If the driver is Java based, then JDBC library like cx_Oracle should be installed. The other way is to install an ODBC library like pyodbc.  

# Oracle Database structure


# What is an instant client?


# Using JDBC instant client

Install driver from : https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html

Oracle provides rpm and zip files for installing the clients on a linux system. Debian needs files to be in .deb format to install. There are three ways to tackle this:

1. Install by converting rpm files to deb file

2. Install rpm file directly using rpm

3. Unzip file and work around the driver settings
but only rpm format is available. we need .deb files for debian os

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

 
# Resources

Useful resources based on which this tutorial is constructed:

Oracle's instant client download links based on OS:
https://www.oracle.com/database/technologies/instant-client/downloads.html

Extra steps for ODBC client:
https://www.oracle.com/database/technologies/releasenote-odbc-ic.html

Oracle's detailed call interface documentation:
https://docs.oracle.com/en/database/oracle/oracle-database/19/lnoci/instant-client.html#GUID-AAB0378F-2C7B-41EB-ACAC-18DD5D052B01


# Steps Overview

1. Install JDBC client
2. Insall ODBC client
2. Check if drivers are installed and connected
3. Update odbcinst.ini and odbc.ini (in case of ODBC drivers)



## what I remember

1. I installed drivers, set env vars with export, ldd ,etc. I uploaded dns config files (tnsnames.ora,sql, oracle.sh)
2. 

Next step: trying first with pyodbc library..

### pyodbc




```
Error: ('01000', "[01000] [unixODBC][Driver Manager]Can't open lib 'driver_name' : file not found (0) (SQLDriverConnect)")
```

1. driver not found - should either install driver or install instant client to access driver
2. odbcinst -j  #to get the odbc driver installed details
3. nano /path/to/file #to open/view files
  
