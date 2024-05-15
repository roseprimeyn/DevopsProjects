# Web Stack Implementation (LAMP STACK) in AWS

## Use Case

Autumns LLC, a boutique bakery specializing in handcrafted cakes, was experiencing growing pains. Their website, built on a basic shared hosting plan, struggled to handle surges in traffic during holidays and peak seasons. The slow loading times frustrated customers and led to missed sales opportunities. Additionally, security concerns loomed as the hosting plan offered limited control and outdated software.

## Prerequisites

## 1. Register an AWS account: Follow AWS instructions.
   
Launch an EC2 Instance:
   
 •  Select your preferred region.
 
 •  Launch a t2.micro instance from the Ubuntu Server 20.04 LTS (HVM) family.
 
 •  Important: Save your private key (.pem file) securely and don't share it!

## 2. Connect to the EC2 Terminal:
   
  •	Mac/Linux: Use the built-in terminal and ssh command.

  •	Windows: Download and install Putty (http://www.9bis.net/kitty/#!pages/download.md).

## Connecting Instructions (Specific to OS):

  •	Mac/Linux: No conversion needed, use the downloaded private key. 

   o	Change directory to where the key is saved (e.g., cd ~/Downloads).
   
   o	Set permissions for the key file: sudo chmod 0400 <private-key-name>.pem
   
   o	Connect: ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>

  •	Windows (Putty): 

   o	Connect: ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>

## Congratulations!  We have created  Linux Server in the Cloud and our set up looks like this now: (You are the client)

![pic 1](https://github.com/roseprimeyn/DevopsProjects/assets/121585728/9a4db5a0-df71-4973-9b9b-43664218b17c)

# STEP 1 — INSTALLING APACHE AND UPDATING THE FIREWALL

  1.	Update package list: sudo apt update
   
  2.	Install Apache: sudo apt install apache2
   
  3.	Verify Apache is running: sudo systemctl status apache2 (look for green status)

  4.	Open port 80 (default web traffic port) in your EC2 security group to allow inbound traffic.

  5.	Install Apache using Ubuntu’s package manager ‘apt’:

## Test Apache Locally and Publicly:

  •	Local: 

   o	Open terminal: curl http://localhost:80 or curl http://127.0.0.1:80 (both achieve the same)

  •	Public: 

   o	Open a web browser and navigate to http://<Public-IP-Address>:80

   o	Alternatively, get your Public IP with: curl -s http://169.254.169.254/latest/meta-data/public-ipv4

## You should see the default Apache web page if successful.

![Pic 2](https://github.com/roseprimeyn/DevopsProjects/assets/121585728/79e4713c-163d-4d58-aa47-c792a05cfe4c)

# STEP 2: Installing MySQL

  1.	Install MySQL: sudo apt install mysql-server
   
  2.	Confirm installation with Y when prompted.

  3.	Log in to MySQL console: sudo mysql (as root user)

  4.	Secure MySQL by running the pre-installed security script:
   
   o	Set a strong password for the root user (e.g., ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';)

   o	Exit the MySQL shell: mysql> exit

   o	Start the interactive security script: sudo mysql_secure_installation

   o	Follow the prompts to configure password validation and remove unnecessary accounts.

  5.	Exit the script and test login with the new password: sudo mysql -p

## Your MySQL server is now installed and secured.

# STEP 3: Installing PHP

  1.	Install PHP and related modules: sudo apt install php libapache2-mod-php php-mysql

  2.	Verify PHP version: php -v

## At this point, your LAMP stack is fully functional!

## Note: To host a website, you'll need to configure a virtual host, covered in the next step.

# STEP 4: Creating a Virtual Host

This guide uses projectlamp as the domain name (replace with your desired name).

  1.	Create a directory for your website: sudo mkdir /var/www/projectlamp

  2.	Assign ownership: sudo chown -R $USER:$USER /var/www/projectlamp

  3.	Create a virtual host configuration file: 

   o	`sudo vi /etc/apache2/sites-available/projectlamp.conf (replace vi with your preferred editor like nano if needed)

  4.	Paste the following configuration:

<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
        
  5.	Save and close the file.

  6.	Enable the new virtual host: sudo a2ensite projectlamp

  7.	(Optional) Disable the default website: sudo a2dissite 000-default (if not using a custom domain)

  8.	Check for configuration errors: sudo apache2ctl configtest

  9.	Reload Apache: sudo systemctl reload apache2

## Test your website:

  1.	Create an index.html file in the project directory:

   sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html

  2.	Open your web browser and navigate to: 

   o	http://<Public-IP-Address>:80 (or your domain name if configured)

You should see the "Hello LAMP" message, indicating a successful virtual host setup.


![Pic 3](https://github.com/roseprimeyn/DevopsProjects/assets/121585728/7e2a300d-4236-4028-b104-958e4afaa043)


Important Note: This index.html is temporary. Remove it once you deploy your actual website content (e.g., PHP files).

# STEP 5: Enabling PHP on the Website

By default, Apache prioritizes .html files over .php. To serve PHP files:

  1.	Edit the directory index configuration: sudo vi /etc/apache2/mods-enabled/dir.conf

  2.	Change the order in the DirectoryIndex directive:

   DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm

  3.	Save and close the file.

  4.	Reload Apache: sudo systemctl reload apache2

## Test PHP:

  1.	Create a test.php file in your project directory:

<?php

phpinfo();

?>

  2.	Open your website in the browser. You should see the PHP information page.

  3.	Once finished, remove the test.php file: sudo rm /var/www/projectlamp/test.php (as it contains sensitive information)

![pic 4](https://github.com/roseprimeyn/DevopsProjects/assets/121585728/0db65261-192c-483c-8489-fa0330d7fc6e)

## Congratulations! You've successfully implemented a LAMP stack on your AWS server.

# Lesson Learned & Challenge: LAMP Stack on AWS

## Lesson: A LAMP stack on AWS provides a robust, scalable, and cost-effective foundation for web applications. By following these steps, you gained valuable experience in:

  •	Cloud Infrastructure: Launching and managing a virtual server instance (EC2) in the AWS cloud.

  •	Web Server Configuration: Installing and configuring Apache to serve web content securely.

  •	Database Management: Setting up a MySQL database and implementing security best practices.

  •	Dynamic Content Integration: Integrating PHP to create interactive web experiences.

  •	Virtual Host Management: Configuring Apache to handle requests for specific domains or subdomains.

This knowledge empowers you to build and deploy functional web applications on the AWS platform.

## Challenge: Optimizing Performance and Security for Growth

While a basic LAMP stack is a strong starting point, it requires ongoing optimization as your website scales:

  •	Performance Bottlenecks: Increased traffic can strain your server's resources. You'll need to consider scaling your EC2 instance type or explore options like Auto Scaling for automatic resource adjustments.

  •	Security Threats: Web applications are vulnerable to attacks. Implementing a Web Application Firewall (WAF) like AWS WAF can significantly enhance security.

  •	Content Delivery: Users in distant locations may experience slow loading times. Utilizing a Content Delivery Network (CDN) like AWS CloudFront distributes static content geographically, improving global website performance.
