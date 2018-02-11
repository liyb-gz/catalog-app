# Catalog App - A Udacity Full Stack Nanodegree Project

## What is this?

This is an application that helps you organize your catagories and items. It can be anything: your favorite singers and their songs, topics that interest you and the related books, you name it. Anything that can be organized in the category-item way can be managed by this app.

## What do I need to install this app on my own computer?

To install this app, you need the followings installed:

- [Python 2](https://www.python.org/downloads/release/python-2714/ "Python 2")
- [Flask](http://flask.pocoo.org/)
- [Flask-login](https://flask-login.readthedocs.io/en/latest/)
- [SQLAlchemy](https://www.sqlalchemy.org/)
- [Googe API Client Libraries for Python](https://developers.google.com/api-client-library/python/)

But if you want to run it on Udacity's vagrant VM, you already have **Python**, **Flask** and **SQLAlchemy** installed. Please refer to **[Flask-login](https://flask-login.readthedocs.io/en/latest/)** and **[Googe API Client Libraries for Python](https://developers.google.com/api-client-library/python/)**'s website for installation.

## I have everything ready, how do I install it?

When you are ready for the installation, follow the steps below:

1. Get the source file of this app from Github:
	- If you have Git installed on your computer, open a console and run this command: `git clone https://github.com/liyb-gz/catalog-app.git`
	- If you don't have Git, download the ZIP file from this Github repository (come on, you can find the "**Download ZIP**" button!) and unzip it.
3. If you want to use Udacity's vagrant VM but haven't set it up, follow this [guide](https://www.udacity.com/wiki/ud197/install-vagrant) to set it up.
4. Open the "catalog" folder.

Now you are ready to run the app!

## How to run this app

To run the app, follow the steps below:

1. Run this command in the "catalog" folder: `python models.py`. This command is for creating a SQLite database file on your machine, as the database file is not included in this Github repository.
2. Run this command: `python application.py` to run the app on the local server.
3. If you didn't change the settings, you can visit the app through [http://localhost:8000](http://localhost:8000). 

Enjoy!


## Notes for the Grader (Linux Server Config Project)

- The IP address of the server is: [54.169.136.16](http://54.169.136.16/) and the port to access SSH is: 2200.
- The complete URL to this web applcation is: [http://54.169.136.16.xip.io/](http://54.169.136.16.xip.io/). Actually it is just [http://54.169.136.16/](http://54.169.136.16/), but as Google OAuth doesn't support the redirect URI to be bare IP address, I have to use the wildcard DNS service provided by [xip.io](http://xip.io/), otherwise users can't log in.

### Software installed on the server:
-  Apache 2
-  libapache2-mod-wsgi
-  Python Package Index (pip)
-  PostgreSQL

### Python packages installed on the server:
-  flask
-  flask-login
-  flask-sqlachemy
-  psycopg2
-  google-auth
-  google-auth-httplib2
-  google-auth-oauthlib

### Configs done on the server:
- Update and upgrade all packages: `sudo apt update && sudo apt upgrade`
- Change the SSH port by editing `/etc/ssh/sshd_config` file, save it, and restart the sshd service: `sudo service sshd restart`;
- Set up UFW:
   - Set default to deny all incoming: `sudo ufw default deny incoming`
   - Set default to allow all outgoing: `sudo ufw default allow outgoing`
   - Set HTTP to be allowed: `sudo ufw allow http`
   - Set SSH to be allowed: `sudo ufw allow 2200/tcp`
   - Set NTP to be allowed: `sudo ufw allow ntp`
   - Enable the firewall: `sudo ufw enable`
- Create **grader** user, prepare its SSH login and grant sudo access to it:
   - Create the **grader** user: `sudo adduser grader` and follow the instruction to set password;
   - Generate SSH Keypair on my own computer: `ssh-keygen`
   - Edit or create the `/home/grader/.ssh/authorized_keys`, and paste the generated public key into it
   - Set the mode of the **.ssh** folder to 700 and **authorized_keys** file to 600 using `chmod`
   - Create a new file in `/etc/sudoers.d/` and fill in the **grader** user's info, then set the file to mode 440
   - SSH password login is by default disabled, so no setting is required.
- Set the Amazon firewall to allow the 2200, 80 and 123 ports, and deny the 22 port on the AWS console page.
- Git clone this project to the server and get it running:
    - `sudo git clone https://github.com/liyb-gz/catalog-app.git --separate-git-dir=/path/to/.git/directory` to separate the **.git** directory, make sure it is not accessible from the outside.
    - Edit the `/etc/apache2/sites-enabled/000-default.conf` file, add this line `WSGIScriptAlias / /path/to/app.wsgi`, and also the `DocumentRoot` line if necessary.
    - Create the **app.wsgi** file, edit it until the app can be run properly (in this stage, database related pages do not matter).
- Set the PostgreSQL
    - Switch to user **postgres** and add a user to the PostgreSQL specifically for this app using the `createuser` command;
    - Create a new database specifically for the app using the `createdb` command;
    - Change the new database's owner to the new user: `ALTER DATABASE db_name OWNER TO new_owner;`
    - Edit the app config, change the database URL to something like: `postgresql://<username>:<password>@localhost/<db_name>`

After all these settings, the app is up and running on the server.
