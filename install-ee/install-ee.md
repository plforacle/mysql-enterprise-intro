# INSTALL - MYSQL ENTERPRISE EDITION

## Introduction

Developers can access the full range of MySQL Enterprise Edition features for free while learning, developing, and prototyping.You wil be using the latest available download  from Oracle Technical Resources (OTR, formerly Oracle Technology Network, OTN). For more details review the   [MySQL Enterprise Edition Downloads page] https://www.oracle.com/mysql/technologies/mysql-enterprise-edition-downloads.html

_Estimated Time:_ 10 minutes

### Objectives

In this lab, you will be guided through the following tasks:

- Install MySQL Enterprise Edition
- Install MySQL Shell and Connect to MySQL Enterprise 
- Start and test MySQL Enterprise Edition Install


### Prerequisites

This lab assumes you have:

- Completed Labs 1 
- or a working Oracle Linux machine

## Task 1: Get MySQL Enterprise Edition Download from Oracle Technology Network (OTN)

1. Connect to **myserver** instance using Cloud Shell (**Example:** ssh -i  ~/.ssh/id_rsa opc@132.145.17….)

    ```
    <copy>ssh -i ~/.ssh/id_rsa opc@<your_compute_instance_ip></copy>
    ```

    ![CONNECT](./images/ssh-login-2.png " ")

1. Create a new directory named "tmp"
    ```
    <copy>mkdir tmp</copy>
    ```
2. Navigate into the "tmp" directory
    ```
    <copy>cd tmp</copy>
    ```
3. Get  OTN MySQL Enterprise Edition package
    ```
    <copy>wget 'https://objectstorage.us-ashburn-1.oraclecloud.com/p/_85tMv-_I0WRJRAuHI9StGHfo3WXtAsSbpslsOIqIu2hsHgmKc8n7zmhk-5KvVw8/n/idazzjlcjqzj/b/mysql-ee-downloads/o/Oracle%20Technical%20Resource(OTR)/mysql-enterprise-9.2.0_el8_x86_64_bundle.tar'</copy>
    ```
4. Extract the contents of the "mysql-enterprise-9.2.0el8x8664bundle.tar" archive file
    ```
    <copy>tar xvf mysql-enterprise-9.2.0el8x8664bundle</copy>
    ```

## Task 2: Install MySQL Enterprise Edition

1. Import the MySQL repository GPG key using the RPM package manager
    ```
    <copy>sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023</copy>
    ```
2. Install the "yum-utils" package using the Yum package manager
    ```
    <copy>sudo yum install yum-utils</copy>
    ```
3. Add a new Yum repository configuration file located at "/home/opc/tmp"
    ```
    <copy>sudo yum-config-manager --add file:///home/opc/tmp</copy>
    ``` 
4. Disable the "mysql" module using the Yum package manager
    ```
    <copy>sudo yum module disable mysql</copy>
    ```
5. Install the "mysql-commercial-server" package
    ```
    <copy>sudo yum install mysql-commercial-server</copy>
    ```
6. Install the "mysql-shell-commercial" package
    ```
    <copy>sudo yum install mysql-shell-commercial</copy>
    ``` 

## Task 3: Configure and Start MySQL Enterprise Edition

1. Start the MySQL server using the systemd system and service manager
    ```
    <copy>sudo systemctl start mysqld</copy>
    ```
2. Check the status of the MySQL server service
    ```
    <copy>sudo systemctl status mysqld</copy>
    ```  
3. List all running processes and filter for those containing "mysqld" in their command line
    ```
    <copy>ps -ef | grep mysqld</copy>
    ``` 
4. Search for the phrase "temporary password" in the "/var/log/mysqld.log" file, ignoring case sensitivity
    ```
    <copy>sudo grep -i 'temporary password' /var/log/mysqld.log</copy>
    ```
    
5. Connect to MySQL with MySQL Shell
    ```
    <copy>mysqlsh -uroot -hlocalhost -p</copy>
    ```  


You may now **proceed to the next lab**.

## Learn More

- [MySQL Enterprise Edition](https://www.oracle.com/mysql/enterprise/)
- [MySQL Linux Installation](https://dev.mysql.com/doc/en/binary-installation.html)
- [MySQL Shell Installation](https://dev.mysql.com/doc/mysql-shell/en/mysql-shell-install.html)

## Acknowledgements

- **Author** - Perside Foster, MySQL Solution Engineering
- **Contributor** - Nick Mader, MySQL Global Channel Enablement & Strategy Director
- **Last Updated By/Date** - Perside Foster, MySQL Solution Engineering, March  2025
