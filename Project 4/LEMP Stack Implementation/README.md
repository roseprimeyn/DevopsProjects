# Web Stack Implementation (LEMP STACK) in AWS

# Use Case
Sugar Rush Studios' current web hosting solution is struggling to keep pace with their rapid client acquisition. 
Their website, which serves as their primary lead generation tool, frequently experiences slow loading times and occasional outages during peak business hours. This inconsistency is jeopardizing their ability to showcase their design expertise and convert potential clients. Additionally, the limited control and outdated software offered by their current provider raise security concerns.

##Solution: Implement a LEMP stack (Linux, Nginx, MySQL, PHP) on an AWS server. This solution offers:

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

6.	Installing PHP
o	Install PHP and related modules: sudo apt install php-fpm php-mysql

7.	Configuring Nginx for PHP
o	Create a new server block configuration file for your website (e.g., /etc/nginx/sites-available/projectLEMP).
o	Configure the server block to listen on port 80, set the document root for your website files, and define how to handle PHP requests using fastcgi-php.conf.
o	Activate the configuration by linking it to the sites-enabled directory: sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
o	Test the configuration for syntax errors: sudo nginx -t
o	Reload Nginx to apply the changes: sudo systemctl reload nginx

8.	Testing PHP
o	Create a test PHP file (e.g., /var/www/projectLEMP/info.php) with phpinfo(); to display PHP configuration details.
o	Access the test file in your browser: http://<Public-IP-Address>/info.php (remove the file after testing)

9.	Optional: Connecting to MySQL from PHP (This step requires additional configuration)
o	Create a database and user with proper permissions in MySQL.
o	Modify your PHP script to connect to the database and retrieve data.

