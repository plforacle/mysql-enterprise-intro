# INSTALL - MYSQL ENTERPRISE EDITION

## Introduction

Detailed Installation of MySQL Enterprise Edition 8 and MySQL Shell on Linux

Objective: RPM Installation of MySQL 8 Enterprise on Linux


RPM Installation of MySQL Enterprise 8 on Linux

Estimated Time: 15 minutes

### Objectives

In this lab, you will:

* Install MySQL Enterprise Edition
* Install MySQL Shell and Connect to MySQL Enterprise 
* Start and test MySQL Enterprise Edition Install


### Prerequisites

This lab assumes you have:
* A working Oracle Linux machine
* The MySQL Enterprise rpms for Oracle Linux inside /workshop directory


### Lab standard

Pay attention to the prompt, to know where execute the commands 
* ![green-dot](./images/green-square.jpg) shell>  
  The command must be executed in the Operating System shell
* ![blue-dot](./images/blue-square.jpg) mysql>  
  The command must be executed in a client like MySQL, MySQL Shell or similar tool
* ![yellow-dot](./images/yellow-square.jpg) mysqlsh>  
  The command must be executed in MySQL shell
  
## Task 1: Install MySQL Enterprise Edition using Linux RPM's


1. Connect to **myserver** instance using Cloud Shell (**Example:** ssh -i ~/.ssh/id_rsa opc@132.145.17….)

    ```
    <copy>ssh -i ~/.ssh/id_rsa opc@<your_compute_instance_ip></copy>
    ```

    ![CONNECT](./images/ssh-login-2.png " ")


2. We have the required software available in **/workshop** directory. But in security perspective, it's important to install only the software that is required. For this reason create a directory called not_needed and move there the rpms that we don't need

  - Let's check the directory content
  **![green-dot](./images/green-square.jpg) shell>**  
      ```
      <copy>cd /workshop</copy>
      ```
  **![green-dot](./images/green-square.jpg) shell>**  
      ```
      <copy>ls -l</copy>
      ```

  - Let's create the new directory  
  **![green-dot](./images/green-square.jpg) shell>**  
      ```
      <copy>mkdir not_needed</copy>
      ```

  - Move the devel package that contains development header files and libraries for MySQL database client applications  
  **![green-dot](./images/green-square.jpg) shell>**  
      ```
      <copy>mv mysql-commercial-devel* not_needed/</copy>
      ```

  - Move the MySQL Router package, because is usually not installed in the mysql servers, but in the application servers  
  **![green-dot](./images/green-square.jpg) shell>**  
      ```
      <copy>mv mysql-router-commercial* not_needed/</copy>
      ```

3. You can see that the only packages are server and clients.

 **![green-dot](./images/green-square.jpg) shell>** 
    ```
    <copy>ls -l</copy>
    ```

3. Now install the RPM's.

 **![green-dot](./images/green-square.jpg) shell>** 
    ```
    <copy>sudo yum -y install *.rpm</copy>
    ```


## Task 2: Start and test MySQL Enterprise Edition Install


1.	Start your new mysql instance

 **![green-dot](./images/green-square.jpg) shell>** 
    ```
    <copy>sudo systemctl start mysqld</copy>
    ```

2.	Verify that process is running and listening on the default ports (3306 for MySQL standard protocol and 33060 for MySQL XDev protocol)

  **![green-dot](./images/green-square.jpg) shell>** 
    ```
    <copy>ps -ef | grep mysqld</copy>
    ```

  **![green-dot](./images/green-square.jpg) shell>** 
    ```
    <copy>netstat -an | grep 3306</copy>
    ```


3.	Another way is searching the message “ready for connections” in error log as one of the last 

  **![green-dot](./images/green-square.jpg) shell>** 
    ```
    <copy>sudo grep -i ready /var/log/mysqld.log</copy>
    ```

4.	Retrieve root password for first login:

  **![green-dot](./images/green-square.jpg) shell>** 
    ```
    <copy>sudo grep -i 'temporary password' /var/log/mysqld.log</copy>
    ```

5. Login to the the mysql-enterprise, change temporary password and check instance the status

    **![green-dot](./images/green-square.jpg) shell>** 
     ```
    <copy>mysqlsh root@localhost</copy>
    ```

6. Create New Password for MySQL Root

 **![yellow-dot](./images/yellow-square.jpg) mysqlsh>**
    ```
    <copy>ALTER USER 'root'@'localhost' IDENTIFIED BY 'Welcome1!';</copy>
    ```

 **![yellow-dot](./images/yellow-square.jpg) mysqlsh>**
    ```
    <copy>\status</copy>
    ```


7.	Create a new administrative user called 'admin' with remote access and full privileges

 **![yellow-dot](./images/yellow-square.jpg) mysqlsh>**
    ```
    <copy>CREATE USER 'admin'@'%' IDENTIFIED BY 'Welcome1!';</copy>
    ```

 **![yellow-dot](./images/yellow-square.jpg) mysqlsh>**
    ```
    <copy>GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;</copy>
    ```

 **![yellow-dot](./images/yellow-square.jpg) mysqlsh>**
    ```
    <copy>\quit</copy>
    ```

8.	Login as the new user, saving the password. MySQL Shell save the password in a secure file (mysql_config_editor is the default) and set history autosave

  **![green-dot](./images/green-square.jpg) shell>** 
    ```
  <copy>mysqlsh admin@127.0.0.1</copy>
  ```

 **![yellow-dot](./images/yellow-square.jpg) mysqlsh>**
    ```
    <copy>\option -l</copy>
    ```

 **![yellow-dot](./images/yellow-square.jpg) mysqlsh>**
    ```
    <copy>\option --persist history.autoSave true</copy>
    ```

 **![yellow-dot](./images/yellow-square.jpg) mysqlsh>**
    ```
    <copy>\option history.autoSave</copy>
    ```
 **![yellow-dot](./images/yellow-square.jpg) mysqlsh>**
    ```
    <copy>\quit</copy>
    ```

## Task 3: Import Sample Databases

1. Import the employees demo database that is in /workshop/databases folder.

  **![green-dot](./images/green-square.jpg) shell>** 
    ```
    <copy>cd /workshop/database</copy>
    ```

  **![green-dot](./images/green-square.jpg) shell>** 
    ```
    <copy>mysqlsh admin@127.0.0.1 < ./employees.sql</copy>
    ```
You may now **proceed to the next lab**

## Learn More

* [MySQL Linux Installation](https://dev.mysql.com/doc/en/binary-installation.html)
* [MySQL Shell Installation](https://dev.mysql.com/doc/mysql-shell/en/mysql-shell-install.html)

## Acknowledgements

* **Author** - Dale Dasker, MySQL Solution Engineering
* **Last Updated By/Date** - Perside Foster, MySQL Solution Engineering, August 2024
