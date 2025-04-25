# Build and Test Web Application - LMPF: Linux, MySQL, Python, Flask

## Introduction

MySQL Enterprise Edition integrates seamlessly with the LMPF (Linux, Apache, MySQL, PHP) stack, enhancing open-source capabilities with enterprise features. MySQL EE works with the LMPF stack by:

- Running JavaScript functions in database
- Using secure PHP connections (PDO)
- Maintaining Apache/Linux compatibility

After installing the LMPF Stack , you will Deploy and test the "Sakila Film Library with Time Converter" web application. This application displays the Sakila.Film data while providing a time conversion tool. Users can enter seconds and convert them to either HH:MM:SS format or written time descriptions using MySQL Enterprise Edition's JavaScript function. This LMPF-based application demonstrates practical use of database features within MySQL Enterprise Edition.

**Note:** The application code in this lab is intended for educational purposes only. It is designed to help developers learn and practice application development skills with MySQL Enterprise Edition. The code is not designed to be used in a production environment

_Estimated Lab Time:_ 15 minutes

### Objectives

In this lab, you will be guided through the following tasks:

- Install  Python and Flask
- Learn to create Python / MYSQL Connect Application
- Deploy the Sample LMPF WEB Application

### Prerequisites

- An Oracle Trial or Paid Cloud Account
- Some Experience with MySQL SQL and  PHP
- Completed Lab 3

## Task 1: Setup Python Flask Environment

1. Install Python
   ```bash
   sudo dnf install python39 python39-devel python39-pip -y
   ```

2. Install required packages
   ```bash
   sudo dnf install gcc -y
   ```

3. Create a virtual environment
   ```bash
   mkdir /var/www/flask_app
   cd /var/www/flask_app
   python3.9 -m venv venv
   source venv/bin/activate
   ```

4. Install Flask and related packages
   ```bash
   pip install flask flask-sqlalchemy pymysql gunicorn
   ```

5. Set up Gunicorn as a service
   ```bash
   sudo nano /etc/systemd/system/flask_app.service
   ```
   
   Add the following:
   ```
   [Unit]
   Description=Gunicorn instance to serve Flask application
   After=network.target

   [Service]
   User=apache
   Group=apache
   WorkingDirectory=/var/www/flask_app
   Environment="PATH=/var/www/flask_app/venv/bin"
   ExecStart=/var/www/flask_app/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:5000 app:app

   [Install]
   WantedBy=multi-user.target
   ```

6. Enable and start Flask
   ```bash
   sudo systemctl enable flask_app
   sudo systemctl start flask_app
   ```

7. Configure firewall
   ```bash
   sudo firewall-cmd --permanent --add-port=5000/tcp
   sudo firewall-cmd --reload
   ```

## Task 2: Deploy Sakila Film Web / MySQL JavaScript Stored Function Application

1. Go to the development folder

    ```bash
    <copy>cd /var/www/flask_app/</copy>
    ```

2. Download application code

    ```bash
    <copy> sudo wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/ojnCuO6Nk8l9tVyocciB9GpJgYR5CyZZ_bgr2-emm9lGxn-Tdf1rqeHd1NgcjgdQ/n/idazzjlcjqzj/b/livelab_apps/o/sakila-web-python.zip</copy>
    ```

3. unzip Application code

    ```bash
    <copy>sudo unzip sakila-web-python.zip</copy>
    ```

    ```bash
    <copy>tree sakila-web-python</copy>
    ```

4. Set proper permissions

    ```bash
    <copy>sudo chown -R apache:apache /var/www/flask_app</copy>
    ```

5. Set up for development testing

    ```bash
    <copy>cd /var/www/flask_app</copy>
    ```
    ```bash
    <copy>source venv/bin/activate</copy>
    ```

    ```bash
    <copy>python app.py</copy>
    ```

4. Run the application as follows (Use your coupute IP address):

    http://132.145.../sakila-web-python.php/

    ![Sakila Web](./images/sakila-list.png "Sakila Web")

5. Test the application with following examples(Enter seconds, then select **short** or **long** format):

    a. Test Case 1 - Movie Length:
    - Input: 7200 seconds (typical movie)
    - Short format: 02:00:00
    - Long format: 2 hours

    b. Test Case 2 - TV Episode:
    - Input: 1350 seconds (22.5 minute show)
    - Short format: 00:22:30
    - Long format: 22 minutes 30 seconds

    c. Test Case 3 - Long Film:
    - Input: 18105 seconds (Lord of the Rings style)
    - Short format: 05:01:45
    - Long format: 5 hours 1 minute 45 seconds

    d. Test Case 4 - Short Clip:
    - Input: 90 seconds (quick scene)
    - Short format: 00:01:30
    - Long format: 1 minute 30 seconds


## Learn More

- [Install Apache and PHP on an Oracle Linux Instance](https://docs.oracle.com/en-us/iaas/developer-tutorials/tutorials/apache-on-oracle-linux/01-summary.htm)


## Acknowledgements

- **Author** - Perside Foster, MySQL Solution Engineering
- **Contributor** - Nick Mader, MySQL Global Channel Enablement & Strategy Director,
Selena Sanchez, MySQL Staff Solutions Engineer 
- **Last Updated By/Date** - Perside Foster, MySQL Solution Engineering, March  2025