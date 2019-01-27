# linuxServerConfiguraton

## Project Overview
You will take a baseline installation of a Linux server and prepare it to host your web applications. You will secure your server from a number of attack vectors, install and configure a database server, and deploy one of your existing web applications onto it.

## Why this Project?
A deep understanding of exactly what your web applications are doing, how they are hosted, and the interactions between multiple systems are what define you as a Full Stack Web Developer. In this project, you’ll be responsible for turning a brand-new, bare bones, Linux server into the secure and efficient web application host your applications need.

## What will I Learn?
You will learn how to access, secure, and perform the initial configuration of a bare-bones Linux server. You will then learn how to install and configure a web and database server and actually host a web application.

## How will I complete this project?
This project is linked to the [Configuring Linux Web Servers course](https://classroom.udacity.com/courses/ud299), which teaches you to secure and set up a Linux server. By the end of this project, you will have one of your web applications running live on a secure web server.

To complete this project, you'll need a Linux server instance. We recommend using [Amazon Lightsail](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Flightsail.aws.amazon.com%2Fls%2Fwebapp%3Fstate%3DhashArgs%2523%26isauthcode%3Dtrue&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fparksidewebapp&forceMobileApp=0) for this. If you don't already have an Amazon Web Services account, you'll need to set one up. Once you've done that, here are the steps to complete this project.

### Get your server.
1. Start a new Ubuntu Linux server instance on Amazon Lightsail. There are full details on setting up your Lightsail instance on the next page.
2. Follow the instructions provided to SSH into your server.
### Secure your server.
3. Update all currently installed packages.
4. Change the SSH port from 22 to 2200. Make sure to configure the Lightsail firewall to allow it.
5. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
### Give grader access.
In order for your project to be reviewed, the grader needs to be able to log in to your server.
6. Create a new user account named grader.
7. Give grader the permission to sudo.
8. Create an SSH key pair for grader using the ssh-keygen tool.
### Prepare to deploy your project.
9. Configure the local timezone to UTC.
10. Install and configure Apache to serve a Python mod_wsgi application.
 * If you built your project with Python 3, you will need to install the Python 3 mod_wsgi package on your server: 
 * ```$ sudo apt-get install libapache2-mod-wsgi-py3```.
11. Install and configure PostgreSQL:
 * Do not allow remote connections
 * Create a new database user named catalog that has limited permissions to your catalog application database.
12. Install git.i
### Deploy the Item Catalog project.
13. Clone and setup your Item Catalog project from the Github repository you created earlier in this Nanodegree program.
14. Set it up in your server so that it functions correctly when visiting your server’s IP address in a browser. Make sure that your .git directory is not publicly accessible via a browser!

## Log in details for grader
* IP: 34.221.14.169
* Port: 2200
* URL: [http://ec2-34-221-14-169.us-west-2.compute.amazonaws.com/](http://ec2-34-221-14-169.us-west-2.compute.amazonaws.com/)

## Requirements
Below is a set of requirements with steps on how they were achieved 
1. Github repository with [README.md](README.md).
2. Your README.md file should include all of the following:
    * i. The IP address and SSH port so your server can be accessed by the reviewer.
        * IP: 35.166.158.167 Port: 2200
    * ii. The complete URL to your hosted web application.    
        * URL:[http://ec2-34-221-14-169.us-west-2.compute.amazonaws.com/](http://ec2-34-221-14-169.us-west-2.compute.amazonaws.com/)
    * iii. A summary of software you installed and configuration changes made.
        * Linux Machine Software
            * Update: `sudo apt-get updatee`
            * Upgrade: `sudo apt-get upgrade`
            * Finger: `sudo apt-get install finger`
            * NTP: `sudo apt-get install ntp`
        * Grader User Setup
            * Create user: `sudo adduser grader`
            * Grant user permission to execute sudo command by running: `sudo vim /etc/sudoers.d/grader`
            * Add the following line in the file `grader ALL=(ALL:ALL) ALL`
        * Configure key-based authentication
            * Create ssh-key on your personal machine by running: `sshkey-gen -f ~/.ssh/someFilename`
            * Switch to the grader user by running: `su grader`
            * Copy the contents of the public key on your machine to the remote lightsail ubuntu machine `cat ~/.ssh/someFilename.pub`
            * Paste it in under the user grader's authorized_keys file: `vim ~/.ssh/authorized_keys`
            * Change the file permissions to the following: `sudo chmod 700 ~/.ssh` & `sudo chmod 644 ~/.ssh/authorized_keys`
            * Verify it is working by remote logging in to grader using the private key `ssh -i ~/.ssh/lightsail grader@34.221.14.169`
       * SSH Configurations
            * Edit the following file `sudo vim /etc/ssh/sshd_config`
            * Disable password based authentication by giving the value of no to the Password Authentication variable ex: `PasswordAuthentication no` 
            * Change SSH service port from 22 to 2200 by modifying the value of the Port variable ex: `Port 2200`
            * Disable SSH root log in by modifying the value of the value of the PermitRootLogin varibale ex: `PermitRootLogin no`
            * Restart the SSH service: `sudo service ssh restart`
       * Firewall Setup
            * `sudo ufw allow 2200/tcp`
            * `sudo ufw allow 80/tcp`
            * `sudo ufw allow 123/udp`
            * `sudo ufw enable`
       * Apache Install & Other Server Requirements
            * `sudo apt-get install apache2`
            * Install mod-wsgi `sudo apt-get install libapache2-mod-wsgi python-dev`
            * Enable wsgi: `sudo a2enmod wsgi`
            * Create app folder `sudo mkdir -p /var/www/catalog`
            * Create wsgi file for the app (catalog.wsgi) inside he catalog folder and add the below code:
                ```
                import sys
                import logging
                logging.basicConfig(stream=sys.stderr)
                sys.path.insert(0, "/var/www/catalog/")
                from catalog import app as application
                application.secret_key = ''
                ```
            * Copy catalog project inside the catalog folder `git clone https://github.com/Fernie-Hacks/catalogProject.git`
            * Rename application name `mv views.py  __init__.py`
        * Virtual Environment Setup
            * `sudo apt install python-pip`
            * `sudo pip install virtualenv`
            * `sudo virtualenv venv`
            * `source venv/bin/activate`
            * `sudo chmod -R 777 venv`
        * Catalog Project dependencies
            * `pip install Flask`
            * `pip install sqlalchemy`
            * `pip install passlib`
            * `pip install itsdangerous`
            * `pip install flask`
            * `pip install flask-bootstrap`
            * `pip install flask-httpauth`
            * `pip install request`
            * `pip install requests`
            * `pip install oauth2client`
        * PostgreSQL
            * `sudo apt-get install libpq-dev python-dev`
            * `sudo apt-get install postgresql postgresql-contrib`
            * `sudo su - postgres`
            * `psql`
    * iv. A list of any third-party resources you made use of to complete this project.
    
3. Locate the SSH key you created for the grader user.

4. During the submission process, paste the contents of the grader user's SSH key into the "Notes to Reviewer" field.

