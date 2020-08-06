
#Digital Ocean Notes

- Courtest: https://www.youtube.com/playlist?list=PLu0W_9lII9ahOwlLGfKljH86ni_muVoi7

- VPS (Virtual Private Server)
Buy hosting services -> shared hosting -> cPanel(software)

##Create a Droplet from dashboard
	- Configure OS
	- Set hostname
	- Set other config
	- Result: Will get an IP, credentials

##Console Login
	- Click on ... opposite to the Droplet
	- Select "Access Console"

	username: root
	password: (the one you get via email)

	Successful Login

##Configurations
	Commands:
	ls : list all dir/files
	pwd: present working directory
	cd / : change dir to root

###Create/Add User Via Console
	- Always use another user rather root to manipulate site in production
	Command: adduser malik
	Enter Password: 123123
	Enter other fields

###Give Permissions to User
	CMD: usermod -aG sudo malik 
	Now malik can become root via entering sudo, upon login

###Setup Firewall
	ufw app list : list all apps in firewall
	ufw status : current firewall status
	ufw allow OpenSSH : allow ufw to use OpenSSH, so that we can be able to connect via SSH
	ufw enable : now firewall is active

##Installing LAMP (Linux, Apache, MySQL & PHP) Stack On DigitalOcean
	sudo su : to become root, avoid it and perform actions with non root users so that later it can be reversed.
	
	sudo apt update
	
	sudo apt install apache2  // apache can serve websites
		Apache2 runs on ports: 80, 443/tcp

	sudo ufw app list
	sudo ufw app info "Apache Full"
	sudo ufw allowin "Apache Full"	// allow apache connections


###MySQL via Console

	sudo apt install mysql-server // Install

	// Configurations
	sudo mysql
	SELECT user, authentication_string, plugin, host from mysql.user;

	// User Configurations
	// Set mysql password
	ALTER user 'root'@'localhost' identified with mysql_native_password by 'MyPassord123'

	FLASH PRIVILEGES:		// with semicolon
	exit

	// Now login again with password

	sudo mysql -p 	// Then enter password to login

###Install PHP with a few libraries

	sudo apt install php libapache2-mod-php php-mysql

	// Libraries "libapache2-mod-php" and "php-mysql" to make connection between both

	// Edit a file in nano editor
	sudo nano /etc/apache2/mods-enabled/dir.conf
	// Change the order of execution move "index.php" at first position
	Ctrl+X => Y => Enter 	// to save changes

	sudo service apache2 restart

###Installing phpmyadmin with extensions
	sudo apt install phpmyadmin php-mbstring php-gettext
	Select apache2 via <space> then Enter to continue

	// Enable PHP modes
	sudo phpenmode mbstring
	sudo service apache2 restart

	Just enter IP of droplet you'll land in index.php default file
	if you go to IP/phpmyadmin you will see DB management software phpmyadmin

	- Make a separate use for phpmyadmin, never use root
	sudo mysql -p
	create user 'malik'@'localhost' identified by 'Password123'
	grant all privileges on *.* to 'malik'@'localhost' with grant option;

###Website files hosting directory
	cd /var/www
	ls // list all
	ls -lart // list all with details
	cd html 	// by default dir in www

	// Create a new file here
	sudo nano index.php

	sudo mkdir website 	// create a website dir
	sudo chown -R $USER:$USER website 	// website dir will now handled/named on current user malik not root

	cd website
	cd /etc/apache2/sites-availabe/		// show configuration files of all sites
	ls
	000-default.conf 	default-ssl.conf 	// files in this dir

	// copy this file in wesite
	sudo cp 000-default.conf website.conf
	sudo nano website.conf

	-Change Server Admin Email: malickateeq@gmail.com
	-DocumentRoot:	/var/www/website

	Add following
	ServerName website.com
	ServerAlias www.website.com

	Save and exit

	// Enable this website
	sudo a2ensite website.conf

	// Disable a site
	sudo a2dissite 000-default.conf

	sudo service apache2 restart

	// Test files configutaions
	sudo apache2ctl configtest

	// To host multiple websites
	add a new123.conf file
	add website contents in var/www/new_website/


### Domain Name pointing to Droplet IP

### Imp files
	/etc/apache2/ports.conf
	http -> port 80
	https -> port 443 (SSL)

	/etc/apache2/sites-available/
	Configure different sites, per-site virtual hosts can be stored

	/etc/apache2/sites-enabled/
	All enabled sites

	/etc/apache2/access.log
	All logs who accessed what.

	/etc/apache2/error.log
	All error logs will be here

## Connect droplet via FileZilla
	Host: Droplet IP
	Username: root or malik
	Password: pass
	Port: 22

	Same for Putty

## Git Deployment / SSH
	In Git Bash
	ssh root@IP
	Enter Password

	Generate SSH Key-pairs from GitBash
	ssh-keygen
	ssh-keygen -t rsa
	cat ~/.ssh/id_rsa.pud 	// display a public key


	Deploy this key to droplet
	ssh-copy-id root@IP
	Enter Password:
	Done.

	Now upon root@IP will automatically logged in no need to enter password again
	
