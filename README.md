# Frappe-ERPNext-Version-14--in-Ubuntu-22.04-LTS
A complete Guide to Install Frappe/ERPNext version 14  in Ubuntu 22.04 LTS



### Pre-requisites 

      Python 3.6+
      Node.js 14+
      Redis 5                                       (caching and real time updates)
      MariaDB 10.3.x / Postgres 9.5.x               (to run database driven apps)
      yarn 1.12+                                    (js dependency manager)
      pip 20+                                       (py dependency manager)
      wkhtmltopdf (version 0.12.5 with patched qt)  (for pdf generation)
      cron                                          (bench's scheduled jobs: automated certificate renewal, scheduled backups)
      NGINX                                         (proxying multitenant sites in production)

### STEP 1 Update and Upgrade Packages
    sudo apt-get update -y
    sudo apt-get upgrade -y

### STEP 2 Create a new user – (bench user)
    sudo adduser [frappe-user]
    usermod -aG sudo [frappe-user]
    su [frappe-user] 
    cd /home/[frappe-user]
    
### STEP 3 Install git
    sudo apt-get install git

### STEP 4 install python-dev

    sudo apt-get install python3-dev

### STEP 5 Install setuptools and pip (Python's Package Manager).

    sudo apt-get install python3-setuptools python3-pip

### STEP 6 Install virtualenv
    
    sudo apt-get install virtualenv
    sudo apt install python3.10-venv
    

### STEP 7 Install MariaDB

    sudo apt-get install software-properties-common
    sudo apt install mariadb-server
    sudo mysql_secure_installation
    
    
      In order to log into MariaDB to secure it, we'll need the current
      password for the root user. If you've just installed MariaDB, and
      haven't set the root password yet, you should just press enter here.

      Enter current password for root (enter for none): # PRESS ENTER
      OK, successfully used password, moving on...
      
      
      Switch to unix_socket authentication [Y/n] Y
      Enabled successfully!
      Reloading privilege tables..
       ... Success!
 
      Change the root password? [Y/n] Y
      New password: 
      Re-enter new password: 
      Password updated successfully!
      Reloading privilege tables..
       ... Success!

      Remove anonymous users? [Y/n] Y
       ... Success!
 
       Disallow root login remotely? [Y/n] Y
       ... Success!

       Remove test database and access to it? [Y/n] Y
       - Dropping test database...
       ... Success!
       - Removing privileges on test database...
       ... Success!
 
       Reload privilege tables now? [Y/n] Y
       ... Success!

 
    
    
    
### STEP 8  MySQL database development files

    sudo apt-get install libmysqlclient-dev

### STEP 9 Edit the mariadb configuration ( unicode character encoding )

    sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf

add this to the 50-server.cnf file

    
     [server]
     user = mysql
     pid-file = /run/mysqld/mysqld.pid
     socket = /run/mysqld/mysqld.sock
     basedir = /usr
     datadir = /var/lib/mysql
     tmpdir = /tmp
     lc-messages-dir = /usr/share/mysql
     bind-address = 127.0.0.1
     query_cache_size = 16M
     log_error = /var/log/mysql/error.log
    
     [mysqld]
     innodb-file-format=barracuda
     innodb-file-per-table=1
     innodb-large-prefix=1
     character-set-client-handshake = FALSE
     character-set-server = utf8mb4
     collation-server = utf8mb4_unicode_ci      
     
     [mysql]
     default-character-set = utf8mb4

Now press (Ctrl-X) to exit

    sudo service mysql restart

### STEP 10 install Redis
    
    sudo apt-get install redis-server

### STEP 11 install Node.js 14.X package

    sudo apt install curl 
    curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
    source ~/.profile
    nvm install 16.15.0

### STEP 12  install Yarn

    sudo apt-get install npm

    sudo npm install -g yarn

### STEP 13 install wkhtmltopdf

    sudo apt-get install xvfb libfontconfig wkhtmltopdf
    

### STEP 14 install frappe-bench

    sudo pip3 install frappe-bench
    
    bench --version
    
### STEP 15 initilise the frappe bench & install frappe latest version 

    bench init frappe-bench --frappe-branch version-14
    
    cd frappe-bench/
    bench start
    
### STEP 16 create a site in frappe bench 
    
    bench new-site dcode.com
    
    bench use dcode.com

### STEP 17 install ERPNext latest version in bench & site

    
    bench get-app payments
    
    bench get-app erpnext --branch version-14
    ###OR
    bench get-app https://github.com/frappe/erpnext --branch version-14

    bench --site dcode.com install-app erpnext
    
    bench start
    
### Step 18 setup production
    
    sudo bench setup production dcode-frappe
    bench restart

#### If bench restart is not worked run the following command again with all Questions Yes
    
    sudo bench setup production dcode-frappe
    
#### if js and css file is not loading on login window run the following command

    sudo chmod o+x /home/dcode-frappe
    
    

    
