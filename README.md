# Magento 2.3.* Easy installation on Docker
Run Magento 2.3.* in docker container easily using docker-compose

## Supported Stack
PHP: Version 7.2.26

MySql: MariaDB 10.2

Server Version: Apache/2.4.38 (Debian)

PhpMyAdmin

## Requirements
You must be familiar with linux commands. If you know docker commands then it is plus point for you.

### Dependency
- docker : Install docker by following any suitable tutorial that best describes for your linux distribution. You can follow digitalocean's blog https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04.
- docker-compose : Then install docker compose tool. Helpful link https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-18-04.

### How it works
If you are familiar with docker then you already know but if ou don't then don't worry you can work without knowing it.
You can learn about docker at https://www.docker.com/why-docker.

### Accessing PhpMyAdmin
You can access phpmyadmin at localhost:8080 where port 8080 is defined in your 

### Configure Magento 2 environment with docker
1. Clone this repo to your workspace where ou want to see your development Magento applications. Let's assume you cloned in /var/www/html/sprinix/
2. Now move to that directory cd /var/www/sprinix/
3. Copy .env.sample to .env
4. Update .env file for environment variables:
 PROJECT_NAME: sprinix(Here I assume sprinix is your Magento application name),
 APP_USER: This should hold your current user name which you can check by running command "whoami".
 USER_ID: This will hold your current user's id which you can find by running "echo $UID" or just by "id" command.
 5. Now run "docker-compose build", this may take sometime to download image & prepare build.
 6. Once build ready, just run "docker-compose up -d". That's it!, IF you see an error the check you are not running any web server in your host machine. If you are already running a webs server in our host then you will have to change port number for your app server inside docker-compose.yml. Just change "80:80" to "8001:80"; 
 7. Now open localhost(or with port number if post customized) in your browser, if you see It Worked! then you are all set to install Magento.
 
 ### Install Magento 2 with docker
 After following the above steps now it is the time to start Magento Installation
 
 #### New Magento 2 Installation
 - Copy Magento 2 source code inside source folder. In my case it is /var/www/sprinix/source/.
 - Now move into app container in order to set permissions, use "docker exec -it sprinix_app bash" where sprinix is your project name.
 - After setting proper permissions you can start Magento web setup wizard by visiting localhost in your browser.
 - While setting up it will ask you for mysql host which is "mysql".
 - Mysql username and password is root as defined in docker-compose.yml
 
 
#### Configure Existing Magento 2 project
- Put source code in ./source directory
- Create a new db using phpmyadmin
- Import DB using command "docker exec -i sprinix_mysql mysql -u root -proot db_name < mysqldumpfilename.sql" where sprinix is project name.
- Now update your project URL in core_config_data for project base URL.

#### Troubleshooting
- If you see Error "ERROR: Couldn't connect to Docker daemon - you might need to run `docker-machine start default`." then make sure all the directories in the current folder has the same ownership. or Delete the .db-data directory
 
##### Warning
Do not use this for production version
