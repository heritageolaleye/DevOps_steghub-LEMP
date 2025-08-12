# 📘 LEMP Stack Implementation on AWS (EC2)
> **Linux | Nginx | MySQL | PHP**

## 🧭 Introduction

The **LEMP stack** is a popular web development platform made up of four components: Linux, Nginx, MySql, and PHP. This guide will walk you through the configuration and usage of the LEMP Stack on AWS.

## Step 0: Prerequisites

1. Launch an EC2 Instance: Start by launching a t2.micro EC2 instance (or any compute engine) running Ubuntu 24.04 LTS or later. Choose a region close to your target audience. (This guide assumes you’re using AWS, but you can adapt these steps for other cloud providers.)

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



