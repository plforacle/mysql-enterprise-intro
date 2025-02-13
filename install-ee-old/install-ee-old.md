# INSTALL - MYSQL ENTERPRISE EDITION

## Introduction

Detailed Installation of MySQL Enterprise Edition 8 and MySQL Shell on Linux
Objective: Tarball Installation of MySQL 8 Enterprise on Linux


Tarball Installation of MySQL Enterprise 8 on Linux

Estimated Time: 15 minutes

### Objectives

In this lab, you will:
* Install MySQL Enterprise Edition
* Start and test MySQL Enterpriese Edition Install
* Install MySQL Shell and Connect to MySQL Enterprise 


### Prerequisites

Test code
This lab assumes you have:
* An Oracle account
* All previous labs successfully completed

* Lab standard  
    - ![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell> the command must be executed in the Operating System shell
    - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql> the command must be executed in a client like MySQL, MySQL Workbench
    - ![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh> the command must be executed in MySQL shell
    
## Task 1: Install MySQL Enterprise Edition using Generic Linux Tar Image

**Note:** If not already connected with SSH
- connect to **myserver** instance using Cloud Shell (**Example:** ssh -i ~/.ssh/id_rsa opc@132.145.17….)
    ```
    <copy>ssh -i ~/.ssh/id_rsa opc@<your_compute_instance_ip></copy>
    ```
    ![CONNECT](./images/06connect01-signin.png " ")


1. Usually to run mysql  the user “mysql” is used, but because it is already available we show here how create a new one.
2. Create a new user/group for your MySQL service (mysqluser/mysqlgrp) and a add ‘mysqlgrp’ group to opc to help labs execution. 

  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo groupadd mysqlgrp</copy>
    ```
  
 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo useradd -r -g mysqlgrp -s /bin/false mysqluser</copy>
    ```
  
 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo usermod -a -G mysqlgrp opc</copy>
    ```

3. Close and reopen shell session or use “newgrp” command as below

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>newgrp - mysqlgrp</copy>
    ```


4.	Create new directory structure:

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo mkdir /mysql/ /mysql/etc /mysql/data</copy>
    ```

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo mkdir /mysql/log /mysql/temp /mysql/binlog</copy>
    ```

5.	Extract the tarball in your /mysql folder

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>cd /mysql/</copy>
    ```

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>**
    ```
    <copy>sudo tar xvf /workshop/mysql_8.0.28/mysql-commercial-8.0.28-linux-glibc2.12-x86&#95;64.tar.xz</copy>
    ```

6.	Create a symbolic link to mysql binary installation

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo ln -s mysql-commercial-8.0.28-linux-glibc2.12-x86&#95;64 mysql-latest</copy>
    ```

7.	Create a new configuration file my.cnf inside /mysql/etc
To help you we created one with some variables, please copy it

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo cp /workshop/my.cnf.first /mysql/etc/my.cnf</copy>
    ```

8.	For security reasons change ownership and permissions

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo chown -R mysqluser:mysqlgrp /mysql</copy>
    ```

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo chmod -R 755 /mysql</copy>
    ```

9. The following permission is for the Lab purpose so that opc account can make changes and copy files to overwrite the content

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo chmod -R 770 /mysql/etc</copy>
    ```

10.	initialize your database

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>**

    ```
    <copy>sudo /mysql/mysql-latest/bin/mysqld --defaults-file=/mysql/etc/my.cnf --initialize --user=mysqluser</copy>
    ```

## Task 2: Start and test MySQL Enterprise Edition Install

1.	Start your new mysql instance

  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo /mysql/mysql-latest/bin/mysqld --defaults-file=/mysql/etc/my.cnf --user=mysqluser &</copy>
    ```

2.	Verify that process is running

  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>ps -ef | grep mysqld</copy>
    ```

  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>netstat -an | grep 3306</copy>
    ```


3.	Another way is searching the message “ready for connections” in error log as one of the last

  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>grep -i ready /mysql/log/err&#95;log.log</copy>
    ```

4. Install the MySQL Shell command line utility

    **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
     ```
    <copy>sudo yum -y install /workshop/shell/mysql-shell-commercial-8.0.28-1.1.el8.x86_64.rpm</copy>
    ```

5.	Retrieve root password for first login:

  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>grep -i 'temporary password' /mysql/log/err&#95;log.log</copy>
    ```

6. Login to the the mysql-enterprise installation and check the status (you will be asked to change password)

    **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
     ```
    <copy>mysqlsh --uri root@localhost:3306 --sql -p </copy>
    ```

7. Create New Password for MySQL Root

 **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>**
    ```
    <copy>ALTER USER 'root'@'localhost' IDENTIFIED BY 'Welcome1!';</copy>
    ```

 **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>**
    ```
    <copy>\status</copy>
    ```

8.	Shutdown the service

 **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>**
    ```
    <copy>\quit</copy>
    ```


9.	Create a new administrative user called 'admin' with remote access and full privileges

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>mysqlsh --sql --uri root@127.0.0.1:3306 -p</copy>
    ```

 **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>**
    ```
    <copy>CREATE USER 'admin'@'%' IDENTIFIED BY 'Welcome1!';</copy>
    ```

 **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>**
    ```
    <copy>GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;</copy>
    ```

10.	Add the mysql bin folder to the bash profile

 **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>**
    ```
    <copy>\quit</copy>
    ```

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>nano /home/opc/.bash&#95;profile</copy>
    ```

11. After the value  **# User specific environment and startup programs**. Add the following line:
    ```
    <copy>PATH=$PATH:/mysql/mysql-latest/bin:$HOME/.local/bin:$HOME/bin</copy>
    ```

12. Save the changes, log out and log in again via ssh for the changes to take effect on the user profile.  Or you can source the .bash&#95;profile file to update your environment.

 **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
   <copy>source /home/opc/.bash&#95;profile</copy>
    ```

You may now **proceed to the next lab**

## Learn More

* [MySQL Linux Installation](https://dev.mysql.com/doc/en/binary-installation.html)
* [MySQL Shell Installation](https://dev.mysql.com/doc/mysql-shell/en/mysql-shell-install.html)

## Acknowledgements
* **Author** - Dale Dasker, MySQL Solution Engineering
* **Last Updated By/Date** - <Dale Dasker, April 2022
