# Web Stack Implementation (LEMP STACK) in AWS

# Use Case
Sugar Rush Studios' current web hosting solution is struggling to keep pace with their rapid client acquisition. 
Their website, which serves as their primary lead generation tool, frequently experiences slow loading times and occasional outages during peak business hours. This inconsistency is jeopardizing their ability to showcase their design expertise and convert potential clients. Additionally, the limited control and outdated software offered by their current provider raise security concerns.

## Solution: Implement a LEMP stack (Linux, Nginx, MySQL, PHP) on an AWS server. This solution offers:

    •	Scalability: The ability to easily adjust server resources (CPU, RAM) to handle spikes in traffic without compromising performance.

    •	Performance: Nginx, a high-performance web server, ensures fast loading times and a smooth user experience.

    •	Security: Greater control over server configuration allows for a more secure environment compared to shared hosting plans.

    •	Customization: The LEMP stack allows Sugar Rush Studios to tailor their server environment to their specific needs and integrate with their preferred development tools.

#  Prerequisites:

    •	An AWS account with a free tier option

    •	Basic knowledge of Linux commands and terminal usage

# STEP 1 – INSTALLING THE NGINX WEB SERVER

1.	Setting Up the Server
      o	Log in to your AWS account and create a new EC2 instance with Ubuntu Server 22.04 LTS.
      o	Connect to your instance using SSH.

2.	Installing Nginx
   
      o	Update the server's package list: sudo apt update
  
      o	Install Nginx: sudo apt install nginx
  
      o	Verify Nginx is running: sudo systemctl status nginx
  
      o	Open port 80 in your EC2 security group to allow web traffic.

  ![Pic 1](https://github.com/roseprimeyn/DevopsProjects/assets/121585728/70db38fe-0171-4ecb-953a-6550c499387c)

3.	Testing Nginx
   
       o	Check the local server response: curl http://localhost:80
   
       o	Visit your server's public IP address in a web browser: http://<Public-IP-Address>:80

   
![Pic 2](https://github.com/roseprimeyn/DevopsProjects/assets/121585728/fccf1391-d1ff-41d9-8381-53f9a6eb2c97)

# STEP 2 — INSTALLING MYSQL

1.	Installing MySQL
   
       o	Install MySQL: sudo apt install mysql-server
   
       o	Secure the MySQL installation by following the prompts.
   
      o	Set a strong password for the root user.
   

# STEP 3 – INSTALLING PHP

1.	Installing PHP
   
    o	Install PHP and related modules: sudo apt install php-fpm php-mysql


# STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR

1. Create a Server Block Configuration

     o Create a new server block configuration file for your website (e.g., /etc/nginx/sites-available/projectLEMP). 

     o You can use a text editor like nano:

        sudo nano /etc/nginx/sites-available/projectLEMP

     o Paste the following configuration into the file:

#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
    

![Pic 4](https://github.com/roseprimeyn/DevopsProjects/assets/121585728/dff0b3b4-0153-4914-8db4-d6b28b55c92e)




### Explanation of Directives:

listen 80: Defines the port Nginx listens on (default HTTP port).

server_name: Specifies the domain names or IP addresses this server block handles.

root: Sets the document root directory for your website files.

index: Defines the order Nginx prioritizes index files.

location /: Matches all requests and checks for file existence using try_files.

location ~ \.php$: Matches all PHP files and includes FastCGI configuration.

fastcgi_pass: Defines the socket location for PHP-FPM communication.

location ~ /\.ht: Denies access to files starting with ".ht" (e.g., .htaccess).


2. Activate and Test the Configuration

    o Activate the configuration by linking it to the sites-enabled directory:

         sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/

    o Test the configuration for syntax errors:

         sudo nginx -t

    o You shall see following message:

         nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
         nginx: configuration file /etc/nginx/nginx.conf test is successful

     
    o If there are no errors, reload Nginx to apply the changes:

       sudo systemctl reload nginx

3. Optional: Disable Default Nginx Host (if applicable)

    o If you have a default Nginx host configured, disable it to avoid conflicts:

       sudo unlink /etc/nginx/sites-enabled/default

4. Test Nginx with a Simple PHP Script

5. Reload Nginx to apply the changes: sudo systemctl reload nginx
   
    o Your new website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that your new server block works as expected:

          sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP'
          $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html

6. Access your website URL in a browse:

         http://<Public-IP-Address>:80  http://<Public-IP-Address>   OR   http://<Your-Domain-Name>

7. If you see the PHP information page, Nginx is successfully handling PHP requests.

Remember:

Replace projectLEMP with your actual domain name(s) in the server block configuration.

Remove the test index.php file after verification.

### Your LEMP stack is now fully configured. 

# STEP 5 – TESTING PHP WITH NGINX
 
Now that you've configured Nginx, let's verify it can handle PHP files properly.

1. Create a Test PHP File:

Open a terminal window.

2. Use nano to create a new file named info.php in your document root directory:

    sudo nano /var/www/projectLEMP/info.php

3. Add PHP Code:

Paste the following code into the info.php file. This code, called phpinfo(), displays information about your PHP environment:

      <?php
      phpinfo();   

Save and close the file using Ctrl+X followed by Y and Enter.

4. Access the Test Page:

Open a web browser and navigate to your website URL followed by /info.php:

     http://<server_domain_or_IP>/info.php

Replace <server_domain_or_IP> with your actual domain name or public IP address configured in Nginx.

Verify PHP Functionality:

If you see a detailed webpage with information about your PHP version, modules, and settings, congratulations! Nginx is successfully processing PHP files.

![Pic 5](https://github.com/roseprimeyn/DevopsProjects/assets/121585728/bd738a77-6077-4e49-8f6f-ea7ca8f7d345)

# STEP 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP

Setting Up a Test Database

1.	Connect to MySQL: We'll use the sudo mysql command to access the MySQL console as an administrator (root account).

2.	Create a Database: Run mysql> CREATE DATABASE example_database; to create a new database named example_database. You can choose a different name if you prefer.

3.	Create a User: We'll create a user named example_user specifically for this database. The command mysql> CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password'; creates the user with mysql_native_password authentication (replace 'password' with a strong password).

4.	Grant Permissions: The command mysql> GRANT ALL ON example_database.* TO 'example_user'@'%'; grants the example_user full access to the example_database. This way, the user can interact with the database but not modify others.

5.	Test User Access: Exit the MySQL console (mysql> exit) and log back in using mysql -u example_user -p (enter your password when prompted). The SHOW DATABASES; command should list example_database among available databases.

6.	Create a Table: Now, within the MySQL console, create a table named todo_list using the following command:

                              CREATE TABLE example_database.todo_list (
                              mysql>     item_id INT AUTO_INCREMENT,
                              mysql>     content VARCHAR(255),
                              mysql>     PRIMARY KEY(item_id)
                              mysql> );

This table will store to-do list items with an auto-incrementing ID and content description

7. Insert Sample Data: Use mysql> INSERT INTO example_database.todo_list (content) VALUES ("Your task description") to insert some items into the table. Repeat this command with different descriptions for multiple entries.
   
9. Verify Data: Run mysql> SELECT * FROM example_database.todo_list; to see the inserted data. You should see your items listed.
    
Connecting to MySQL from PHP

1.	Create a PHP Script: Use your preferred editor to create a new PHP file (e.g., todo_list.php) in your web server's document root directory (often /var/www/html or similar).

2. Connect to Database: The provided PHP script defines connection details like username, password, database name, and table name. It then uses PDO (a PHP library) to connect to the MySQL database. In case of connection errors, it throws an exception.

3. Fetch Data: The script queries the todo_list table to retrieve content and loops through each row. It then displays each item's content within an HTML list structure.

4.	Save and Run: Save the script (todo_list.php). You can access it in your web browser by visiting http://your_domain_or_IP/todo_list.php (replace with your website's domain name or IP address).

   ![Pic 6](https://github.com/roseprimeyn/DevopsProjects/assets/121585728/e78a87d1-dd34-41fb-a74f-c0f45876fdfd)

