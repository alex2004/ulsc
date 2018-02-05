# Udacity - Linux Server Configuration
This are the instructuions to set up an Ubuntu Linux server to host a simple web application built with Flask.

The instructions written for hosting the app called Nuevo M&eacute;xico on an Amazon Lightsail instance but can easily be adapted to other apps / providers.

## 1. Details specific to the server I set up
The IP address is 34.206.52.37.

The SSH port used is `2200`.

The URL to the hosted webpage is: http://34.206.52.37/ or http://ec2-34-206-52-37.compute-1.amazonaws.com.

## 2. Software required during the installation
- Apache2
- mod_wsgi
- PostgreSQL
- git
- pip
- virtualenv
- httplib2
- Python Requests
- oauth2client
- SQLAlchemy
- Flask
- libpq-dev
- Psycopg2

## 3. Configuration steps
1. Get your server.

    - Start a new Ubuntu Linux server instance on Amazon Lightsail. 
        
        * Log in!
        * Create an instance and choose Ubuntu (OS only)
        * Choose an instance plan and give your instance a name
        * Wait for the instance to startup
        * Follow the instructions to ssh into your server

2. Secure your server.

    - Update all packages.
    
            sudo apt-get update
            sudo apt-get upgrade
            
        auto upgrades run
            sudo dpkg-reconfigure --priority=low unattended-upgrades
    - Change port to SSH in from 22 to 2200. Don't forget to configure the Lightsail firewall to allow it.
            
            sudo vim /etc/ssh/sshd_config
        change port form 22 to 2200
        
        
    - Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123). Use the following commands:
        
            sudo ufw allow 2200/tcp
            sudo ufw allow 80/tcp
            sudo ufw allow 123/tcp
            sudo ufw enable

3. Give grader access.

    Allow the grade to login to your server:

    - create a new user named grader by using:
    
            sudo adduser grader
    - Give grader the permission to sudo.
    
            sudo vim /etc/sudoers.d/grader
        add text `grader ALL=(ALL) NOPASSWD:ALL`
    - Create an SSH key pair for grader using the ssh-keygen tool.
            
                ssh-keygen -t rsa
                
            To login
                ssh -i .ssh/grader_udacity grader@(YOUR SERVER IP HERE) -p 2200

4. Prepare to deploy your project.

    - Configure the local timezone to UTC.
    
            sudo dpkg-reconfigure tzdata
            
        * select none of the above, then UTC
            
    - Install and configure Apache to serve a Python mod_wsgi application.      

    - Install and configure PostgreSQL:
    
            sudo apt-get install postgresql

5. Do not allow remote connections
    
    Create a new database user named catalog that has limited permissions to your catalog application database.
    
        https://help.ubuntu.com/community/PostgreSQL

6. Deploy the Item Catalog project.
    
## Resources
https://help.ubuntu.com/community/PostgreSQL

Flask [documentation](http://flask.pocoo.org/docs/0.12/installation/) on virtualenv

tutorialspoint [tutorial](https://www.tutorialspoint.com/postgresql/postgresql_create_database.htm) on creating a database with PostgreSQL

Flask [documenation](http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/#working-with-virtual-environments) on making a virtualenv work with mod_wsgi

Techarena51 [tutorial](https://techarena51.com/index.php/one-to-many-relationships-with-flask-sqlalchemy/) on one-to-many relationships with SQLAlchemy and Flask
