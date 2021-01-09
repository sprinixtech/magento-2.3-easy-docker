# Magento 2.3.* Easy installation on Docker
Run Magento 2.3.* in docker containers easily using docker-compose

## About This Repo
Each file in this repo has it's own significance

**|_ .docker/** [Holds configuration files]\
**|_____ Dockerfile** [This file is responsible for creating app container, required php version is specified here]\
**|_____ magento-php.ini** [Here you can set php variables]\
**|_ source/**           [This is the folder where your Magento's source code land]\
**|_ .env.sample**       [This is a sample file for creating .env which is a must for running containers]\
**|_ docker-compose.yml**       [This is the main configuration file that is responsible for managing all containers.]

## Supported Stack
**PHP:** Version 7.2.26

**MySql:** MariaDB 10.2

**Server Version:** Apache/2.4.38 (Debian)

**PhpMyAdmin**

## Requirements
You must be familiar with linux commands. If you know docker commands then it is plus point for you.

### Dependency
- **docker** : 
Install docker by following any suitable tutorial that best describes for your linux distribution. You can follow digitalocean's blog https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04.

OR

Install docker by following official guide https://docs.docker.com/engine/install/ and choose your distribution. In my case I am using ubuntu. Follow this if you are good in linux.

- **docker-compose** : Then install docker compose tool. Helpful link https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-18-04.

OR

You can follow official guide to install docker-compose https://docs.docker.com/compose/install/.

### Environment Verification
- check docker is running from not root and without sudo if bellow command executes successfully for your user.
`docker run hello-world`
if this gives error then you missed adding your user to docker group. Follow below link to resolve this
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04#step-2-%E2%80%94-executing-the-docker-command-without-sudo-optional

if it works then you will see "Hello from Docker!" in output content.

- check docker-composer command running fine for your user by running

`docker-compose --version`

This should output installed version of docker compose.

- Make sure ports 80, 3306, 8080 or anyother defined in docker-compose.yml and .env are not occupied in your system. Otherwise you will have to either stop that service to free up these ports or you have choice to change port in docker-compose.yml and .env file.

### How it works
If you are familiar with docker then you already know but if you don't then don't worry you can work without knowing it. You just need to memorise few commands we will use here.
You can learn about docker at https://www.docker.com/why-docker.

### Configure Magento 2 environment with docker
1. Clone this repo to your workspace where you want to see your Magento application. Let's assume you cloned in /var/www/html/sprinix/

`cd /var/www/html/sprinix/`

`git clone https://github.com/gulshankumar/magento-2.3-easy-docker.git .`

2. Now you can see files cloned in your directory.
3. Copy .env.sample to .env

`cp .env.sample .env`

4. Update .env file for environment variables:

 **PROJECT_NAME:** sprinix(Here I assume sprinix is your Magento application name),\
 **APP_USER:** sprinix (This should hold your current user name which you can check by running command "whoami".)\
 **USER_ID:** 1000 (This will hold your current user's id which you can find by running "echo $UID" or just by "id" command. In most cases it is 1000 for default user.)
 5. Now run "docker-compose build", this may take sometime to download image & prepare build.
 
 `docker-compose build`
 
 6. Once build ready, just run "docker-compose up -d". 
 
 `docker-compose up -d`
 
 That's it!
 
 IF you see an error the check you are not running any web server in your host machine. If you are already running a web server in your host then you will have to change port number for your app server inside docker-compose.yml. Just change "80:80" to "8001:80" or something else; 
 7. Now open localhost(or with port number if port changed in docker-compose.yml) in your browser, if you see It Worked! then you are all set to install Magento.
 
 ### Install Magento 2 with docker
 After following the above steps now it is the time to start Magento Installation
 
 #### New Magento 2 Installation
 - Download Magento 2.3 source code zip and extract in source folder. In my case it is `/var/www/sprinix/source/`.
 - Now move into app container in order to set permissions, use `docker exec -it sprinix_app bash` where sprinix is your project name you defined in .env file.
  
  `docker exec -it sprinix_app bash`
  
  this command will take you into the app container.
  Now run permission command
  
  `find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +`
  
  `find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +`

exit the container just by typing `exit`.
 - Now create a new MySQL DB for magento to use. To create new DB just browser `localhost:8080` in your browser that will open phpMyAdmin, and you can create your DB for Magento.
 - After setting proper permissions and DB, you can start Magento web setup wizard by visiting localhost in your browser. But in Magento 2.4 it is no longer supported so you will have to follow command line installation steps.
 - While setting up it will ask you for many configuration values that you can set by yourself but when it asks mysql host which is `mysql` for this environment DB username & password will be `root` as defined in `docker-compose.yml`.
 - That's It!
 
 #### Install Magento 2 by cloning repo
 - If you choose to clone magento source from repo then you you can clone it directly into the source folder `/var/www/sprinix/source/`.
 - Copy auth.json.sample to auth.json if not already created.
 
 `cp auth.json.sample auth.json`
 
 And update your Magento's auth-key and auth-secret which you can get from your Magento account. Or Google it how to get Magento auth keys.
 - Now move into the app container.
 `docker exec -it sprinix_app bash`
 
 switch to user
 
 `su username` Here username is the same which you updated in .env for APP_USER.
 - Run `composer install`
 - Follow other installation steps described in the above section
 
#### Configure Existing Magento 2 project
- Put source code in `./source` directory
- Create a new db using phpmyadmin
- Import DB using command `docker exec -i sprinix_mysql mysql -u root -proot db_name < mysqldumpfilename.sql` where sprinix is project name.
- Now update your project URL in core_config_data for project base URL.


### Accessing PhpMyAdmin
You can access phpmyadmin at `localhost:8080` where port 8080 is the same as you defined in your docker-compose.yml.

It will not ask you for username and password but by default it is root, you can update this in docker-compose.yml before running first `docker-compose up -d`.

### Running Magento commands
To run php commands you will have to enter into the app container.

Run below command to enter into app container

`docker exec -it sprinix_app bash` or `docker-compose exec app bash`

where sprinix_app is the name of container derived from project name specified in .env.

When you are in you will have to switch to filesystem user.

`su sprinix`

OR 

Run single command to enter into the container and switch to the user.

`docker exec -it sprinix_app su sprinix` or `docker-compose exec app su sprinix`

Once you entered into the container you can execute php commands like

`php bin/magento deploy:mode:show`

And use `exit` to come out of container

### Shutting down containers
Once you have completed your work you can down these containers to free up your localhost ports so that you can start other project.

Just run below command in the directory where your project's docker-compose.yml file exists.

`docker-compose down`

### starting project
Run below command

`docker-compose up -d`

### How to check running containers

`docker ps`

### how to stop a container 

`docker stop container_name` or `docker stop container_id`


### Import and Export Database
##### Import DB
Run below command to import db

`docker exec -i sprinix_mysql mysql -u root -proot db_name < mysqldumpfilename.sql`

##### Export DB

`docker exec -i sprinix_mysql mysql -u root -proot db_name > mysqldumpfilename.sql`

#### Troubleshooting
- If you see Error "ERROR: Couldn't connect to Docker daemon - you might need to run `docker-machine start default`." then make sure all the directories in the current folder has the same ownership. 
You can update same ownership to .db-data directory using sudo

`sudo chown -R username:username .db-data`

if you don't care about db imported then you just need to delete .db-data directory.

- When you start your computer and you see some containers are running by executing `docker ps` then you can stop them by runnig `docker stop container_id` so that it would not create issue when you are starting a project with `docker-compose up -d` 
 
##### Warning
This is not intentionally created for production but you can use at your own risk.

### Support
If you have any issue You can write to me at gulshan.4dream@gmail.com or gulshan@sprinix.com.
