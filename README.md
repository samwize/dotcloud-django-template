Dotcloud + Django Bootstrap Template
==================================

This is a template to bootstrap your Dotcloud + Django + MySQL project. It follows [Dotcloud's Django guide](http://docs.dotcloud.com/tutorials/python/django/), and produces a working template. Effectively it saves you a couple of minutes/hours!

It has been tested for:
- Django 1.4
- MySQL 5.1.62
- Python 2.7
- Mac OS X Lion 10.7.4
- Dotcloud as of Jul 16, 2012

These are the steps to create a local development environment, and deploying to Dotcloud.


1) Setup virtualenvwrapper
-----------------------
[virtualenvwrapper](http://www.doughellmann.com/docs/virtualenvwrapper/) helps to create a new Python environment, so you can have different versions or package for different environment. This is optional, but it's a very good practice.

    sudo pip install virtualenvwrapper
    source /usr/local/bin/virtualenvwrapper.sh
    mkvirtualenv dotcloudenv

A new python environment (dotcloudenv) is created in eg. /Users/samwize/.virtualenvs/, and it is activated automatically.

These are other virtualenv commands to switch the environment. Do not use them now.

    workon anotherenv      # switch to another env
    deactivate             # Switch to system Python


2) Install all requirements (eg. Django 1.4)
----------------------------------------------
requirements.txt is a file in Dotcloud project that contains the dependencies.
  
    cd dotcloud-django-template
    pip install -r requirements.txt
    
Django 1.4 will be installed, and it is the only requirement. Other requirements (eg. redis) could be added into the file.


3) Install MySQL
-----------------------------
Download and install [mysql 5.1.62](http://downloads.mysql.com/archives.php?p=mysql-5.1&v=5.1.62). Choose Mac OS X 10.6 (AMD64, installer format).

Also install the Pref Pane, which helps to start/stop mysql server from Preference. You should not install the Startup Item. Start mysql.

    cd /usr/local/mysql
    sudo ./bin/mysqld_safe
    (Enter your password, if necessary)
    (Press Control-Z)
    
mysql is now running. You could try running it with username root, no password.

    /usr/local/mysql/bin/mysql



4) Install MySQL-Python
-----------------------------
Download [MySQL-Python-1.2.3](http://sourceforge.net/projects/mysql-python/files/)

In setup_posix.py, change mysql_config.path to
    
    mysql_config.path = "/usr/local/mysql/bin/mysql_config" 

Then setup MySQL-Python

    export ARCHFLAGS='-arch i386 -arch x86_64'
    python setup.py clean  
    python setup.py build  
    sudo python setup.py install

Alternatively, you could install with pip (somehow this didn't work for me).

    pip install mysql-python



5) Install Postgre
--------------------------

Although Postgre is not used, the setup code imports psycopg2, so to avoid the error, simply install psycopg2. It is much easier to install than MySQL anyway.

    pip install psycopg2


6) Final Setup 
------------------

The final step is basically Dotcloud's postinstall script.

    python createdb.py
    python myproject/manage.py syncdb
    python mkadmin.py

Done! You should now start the Django server without any problem!

    python myproject/manage.py runserver
    
    
7) Deploying to Dotcloud 
-------------------------

Deploying to Dotcloud is much easier than setting up your local development environment. But firstly, you need to [register and setup a Dotcloud account](http://docs.dotcloud.com/firststeps/install/).

Create an app.

    dotcloud create helloworldapp
    
Then you push and deploy!

    dotcloud push --all helloworldapp


That's it!