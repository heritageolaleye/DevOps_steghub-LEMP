# 📘 LEMP Stack Implementation on AWS (EC2)
> **Linux | Nginx | MySQL | PHP**

## 🧭 Introduction

The **LEMP stack** is a popular web development platform made up of four components: Linux, Nginx, MySql, and PHP. This guide will walk you through the configuration and usage of the LEMP Stack on AWS.

## Step 0: Prerequisites

1. Launch an EC2 Instance: Start by launching a t3.micro EC2 instance (or any compute engine) running Ubuntu 24.04 LTS or later. Choose a region close to your target audience. (This guide assumes you’re using AWS, but you can adapt these steps for other cloud providers.)

2. Create an SSH Key Pair: Name it my-ec2-key (or any name you prefer) to access your instance via SSH on port 22.

3. Configure Security Group: Set up your instance’s security group with these inbound rules:

- Allow HTTP traffic (port 80) from anywhere.
- Allow HTTPS TRAFFIC (PORT 443) from anywhere.
- Allow SSH traffic (port 22) from anywhere (this is usually enabled by default).

4. Download the Private SSH Key: If you're using Linux or macOS, change the permissions before use:

```bash
ssh -i "my-ec2-key.pem" ubuntu@<instance-ip>
```
![cheese!](/images/lemp.jpg)
  
![lemp2!](/images/lemp2.jpg)

![lemp3!](/images/lemp3.png)

## ⚙️ Step 1: Install Nginx

# 1. Update and Upgrade Packages:

```bash
sudo apt update && sudo apt upgrade -y
```

![lemp4!](/images/lemp4.jpg)

2. Install Nginx

```bash
sudo apt install nginx -y
```
![lemp5!](/images/lemp5.jpg)

3. Check Nginx Status

```bash
sudo systemctl status Nginx
```
![lemp6!](/images/lemp6.jpg)

4. Test Nginx:

```bash
curl http:localhost:80
```
![lemp7!](/images/lemp7.jpg)

5. Access Nginx in your Browser.

```bash
http://51.21.162.171
```

![lemp8!](/images/lemp8.jpg)

## Step 2: Install MySQL
1. Install MySQL

```bash
sudo apt install mysql-server -y
```
![lemp9!](/images/lemp9.jpg)

![lemp10!](/images/lemp10.jpg)

# 2. Log into MySQL and set Root password

```bash
sudo mysql
```
![lemp11!](/images/lemp11.jpg)

#3. S=Secure MySQL:

```bash
sudo mysql_secure_installation
```
![lemp12!](/images/lemp12.jpg)

#4. Test MySQL Login:

```bash
sudo mysql -p
```

![lemp13!](/images/lemp13.jpg)

## Step 3: Install PHP

1. Install PHP:

```bash
sudo apt install php-fpm php-mysql
```

![lemp14!](/images/lemp14.jpg)

## Step 4: Configure Nginx for PHP
1. Create a Web Directory and change ownership:

```bash
sudo mkdir /var/www/ProjectLEMP
```
![lemp15](/images/lemp15.jpg)

2. Create Nginx config file:

```bash
sudo vi /etc/nginx/sites-available/ProjectLEMP
```
Paste the following configuration:

```bash
server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
    }
}
```

![lemp16!](/images/lemp16.jpg)

3. Activate the configuration:
 
 ```bash
 sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
 ```

 ![lemp17!](/images/lemp17.jpg)

 4. Test Nginx Configuration:

 ```bash
 sudo nginx -t
 ```

 5. Disable Default Config:
 
 ```bash
 sudo unlink /etc/nginx/sites-enabled/default
 ```
 ![lemp18!](/images/lemp18.jpg)

 6. Create an Index Page:

 ```bash
 sudo echo 'Hello LEMP from hostname:' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) ' with public IP ' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
 ```
 ![lemp19!](/images/lemp19.jpg)


 Now access the site via public IP in a browser.

 ![lemp20!](/images/lemp20.jpg)

 ## Step 5: Test PHP Requests
 
 1. Create a sample PHP File:

 ```bash
 sudo vi /var/www/projectLEMP/info.php
 ```

 Add the following code:

 ```bash
 <?php 
 phpinfo(); 
 ```

 2. Access via Browser:

 http://13.61.12.80/index.php

 ![lemp21!](/images/lemp21.jpg)

 ## Step 6: Create and Retrieve Data from MySQL

 1. Log into MySQL:

 ```bash
 sudo mysql -p
 ```

 2. Create a Database:

 ```bash
 CREATE DATABASE example_database;
 ```

 3. Create a User and Grant Privileges:

 ```bash
 CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
 GRANT ALL ON example_database.* TO 'example_user'@'%';
 ```

 ![lemp22!](/images/lemp22.jpg)

 4. Login as New User:

 ```bash
 mysql -u example_user -p
 ```

 5. Create a Table:

 ```bash
 USE example_database;
CREATE example_database.todo_list (
    item_id INT AUTO_INCREMENT,
    content VARCHAR(255),
    PRIMARY KEY (item_id)
);
```

5. Insert Records:

![lemp23](/images/lemp23.jpg)

6. Verify Data:

![lemp24](/images/lemp24.jpg)

7.Create a PHP Script to Retrieve Data:

```bash
sudo vi /var/www/projectLEMP/todo_list.php
```
Add this code:

```bash
<?php
$user = "user1";
$password = "RootPass.3";
$database = "todo_database";
$table= "todo_list";

try {
    $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
    echo "<h2>TODO List</h2><ol>";
    foreach ($db->query("SELECT content FROM $table") as $row) {
        echo "<li>" . $row['content'] . "</li>";
    }
    echo "</ol>";
} catch (PDOException $e) {
    echo "Connection failed: " . $e->getMessage();
}
?>
```
8. Access Your To-Do List:

http://13.61.12.80/todolist.php

![lemp25](/images/lemp25.jpg)





