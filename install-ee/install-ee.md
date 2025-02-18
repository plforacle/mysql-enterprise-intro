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

- Completed Labs 1 and 2
- or a working Oracle Linux machine

  
## Task 1: Download OTN MySQL Enterprise Edition  

1. Connect to **myserver** instance using Cloud Shell (**Example:** ssh -i ~/.ssh/id_rsa opc@132.145.17….)

    ```
    <copy>ssh -i ~/.ssh/id_rsa opc@<your_compute_instance_ip></copy>
    ```

    ![CONNECT](./images/ssh-login-2.png " ")

1. Get  OTN MySQL Enterprise Edition package

    ```
    <copy>wget 'https://objectstorage.us-ashburn-1.oraclecloud.com/p/_85tMv-_I0WRJRAuHI9StGHfo3WXtAsSbpslsOIqIu2hsHgmKc8n7zmhk-5KvVw8/n/idazzjlcjqzj/b/mysql-ee-downloads/o/Oracle%20Technical%20Resource(OTR)/mysql-enterprise-9.2.0_el8_x86_64_bundle.tar'</copy>
    ```


## Task 1: Install MySQL Enterprise Edition using Linux RPM's



You may now **proceed to the next lab**.



## Learn More

- [MySQL Enterprise Edition](https://www.oracle.com/mysql/enterprise/)
- [MySQL Linux Installation](https://dev.mysql.com/doc/en/binary-installation.html)
- [MySQL Shell Installation](https://dev.mysql.com/doc/mysql-shell/en/mysql-shell-install.html)

## Acknowledgements

- **Author** - Perside Foster, MySQL Solution Engineering
- **Contributor** - Nick Mader, MySQL Global Channel Enablement & Strategy Director
- **Last Updated By/Date** - Perside Foster, MySQL Solution Engineering, March  2025
