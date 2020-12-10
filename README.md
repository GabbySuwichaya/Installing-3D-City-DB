- [3D-City-DB & Importer-Exporter](#3d-city-db---importer-exporter)
  * [Prerequisite-Installation](#prerequisite-installation)
    + [1. Install `Java runtime environment`.](#1-install--java-runtime-environment-)
    + [2. Install `PostgreSQL`.](#2-install--postgresql-)
    + [3. Install `PostGIS`.](#3-install--postgis-)
    + [4. Install `pgAdmin 4` (optional).](#4-install--pgadmin-4---optional-)
      - [Possilbe errors](#possilbe-errors)
    + [5. Install `Google Earth Pro` (optional).](#5-install--google-earth-pro---optional-)
  * [Quick-Start.](#quick-start)
    + [Settings](#settings)
      - [Install `3D-City-DB Importer/Exporter` (v.4.2.3)](#install--3d-city-db-importer-exporter---v423-)
      - [Creat DATABASE](#creat-database)
        * [Example of `CONNECTION_DETAILS.sh`.](#example-of--connection-detailssh-)
        * [How to create another database with different Connection Details using `pgAdmin 4`.](#how-to-create-another-database-with-different-connection-details-using--pgadmin-4-)
    + [Import-Export-visualize](#import-export-visualize)
  * [PostgreSQL Storage Problem in root dir.](#postgresql-storage-problem-in-root-dir)

_________________________________________________________________________________________________

#  3D-City-DB & Importer-Exporter

Before you start, you may skim through the [first step of the official 3D City DB document](https://3dcitydb-docs.readthedocs.io/en/release-v4.2.3/intro/index.html). The entire section is about installation and preparation. 

Before you do anything, you should notice the following

1.  `3D-City-DB` and `3D-City-DB Importer/Exporter` are two different packages. However, the `3D-City-DB` is a part of `3D-City-DB Importer/Exporter`. 
     - Therefore, if you have downloaded and installed `3D-City-DB Importer/Exporter` , you don't need to install `3D-City-DB`. 
     - This means you need to install the prerequisite packages for both `3D-City-DB Importer/Exporter` and  `3D-City-DB` **before** installing  `3D-City-DB Importer/Exporter`. 

2. The prerequisite packages are 
   - Java 8 Runtime Environment (or higher). You have to install both JDK and JRE. for `3D-City-DB Importer/Exporter`.
   - `Oracle` or `PostgreSQL` database server `3D-City-DB`. If you choose `Oracle`, you need `Oracle` 10g R2 or higher. On the contrary, if you choose `PostgreSQL` , you need `PostgreSQL`  9.6 or higher with the `PostGIS` extension 2.3 or higher. (I have chosen `PostgreSQL` + `PostGIS` . )
   
3. After all the installations ( all the prerequisite packages and `3D-City-DB Importer/Exporter`), you will have to find another package to visualize the extracted 3D model. 
   - You can use the `3DCityDB-Web-Map-Client` that is also provided in the `3D-City-DB Importer/Exporter`; however, `3DCityDB-Web-Map-Client` also requires installation for another set of the prerequisite packages.  You need a GPU suppor to visualize the 3D model. 
   - You can choose other package that is not `3DCityDB-Web-Map-Client`. You should choose the one based on the format of the extracted files. For example, in this tutorial, I have choose the Google Earth Pro to visualize the extracted .KML model file. 

**Note:** There is no ADE extension for `3D-City-DB Importer/Exporter` version 4.2.3. But there will be one for the future version (4.3) which will be updated by the end of Q1 2021 accoding to this [issue](https://github.com/3dcitydb/iur-ade-citydb/issues/1). So, please keep your eyes on the repo (https://github.com/3dcitydb/iur-ade-citydb). 


## Prerequisite-Installation

My initial setting is Ubuntu 20.04.  

### 1. Install `Java runtime environment`. 

I have installed JDK 14 following the [installation-java-on-ubuntu-20-04](https://linuxconfig.org/how-to-install-java-on-ubuntu-20-04-lts-focal-fossa-linux) . 

To see which package is available for your Ubuntu. 
```
$ sudo apt search openjdk
```
My ubuntu has an option to install JDK 14; therefore, I have chosen it. 
```
$ sudo apt install openjdk-14-jdk
```
Then, you can check the installation with `$ java --version`

### 2. Install `PostgreSQL`. 

   - **Step 1:** Update the system. A reboot is necessary after an upgrade.
```
sudo apt update
sudo apt -y upgrade 
sudo reboot
```
   - **Step 2:** Add PostgreSQL repository. We need to import GPG key and add PostgreSQL 12 repository into our Ubuntu machine. 
```
sudo apt -y install gnupg2
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```
After importing GPG key, add repository contents to your Ubuntu:

```
echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list
```
   - **Step 3:** Install PostgreSQL 12 on Ubuntu 20.04/18.04/16.04 LTS.
```
sudo apt update
sudo apt -y install postgresql-12 postgresql-client-12
```
The PostgreSQL service is started and set to come up after every system reboot.
```
$ systemctl status postgresql.service 
 ● postgresql.service - PostgreSQL RDBMS
    Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor preset: enabled)
    Active: active (exited) since Sun 2019-10-06 10:23:46 UTC; 6min ago
  Main PID: 8159 (code=exited, status=0/SUCCESS)
     Tasks: 0 (limit: 2362)
    CGroup: /system.slice/postgresql.service
 Oct 06 10:23:46 ubuntu18 systemd[1]: Starting PostgreSQL RDBMS…
 Oct 06 10:23:46 ubuntu18 systemd[1]: Started PostgreSQL RDBMS.

$ systemctl status postgresql@12-main.service 
 ● postgresql@12-main.service - PostgreSQL Cluster 12-main
    Loaded: loaded (/lib/systemd/system/postgresql@.service; indirect; vendor preset: enabled)
    Active: active (running) since Sun 2019-10-06 10:23:49 UTC; 5min ago
  Main PID: 9242 (postgres)
     Tasks: 7 (limit: 2362)
    CGroup: /system.slice/system-postgresql.slice/postgresql@12-main.service
            ├─9242 /usr/lib/postgresql/12/bin/postgres -D /var/lib/postgresql/12/main -c config_file=/etc/postgresql/12/main/postgresql.conf
            ├─9254 postgres: 12/main: checkpointer   
            ├─9255 postgres: 12/main: background writer   
            ├─9256 postgres: 12/main: walwriter   
            ├─9257 postgres: 12/main: autovacuum launcher   
            ├─9258 postgres: 12/main: stats collector   
            └─9259 postgres: 12/main: logical replication launcher   
 Oct 06 10:23:47 ubuntu18 systemd[1]: Starting PostgreSQL Cluster 12-main…
 Oct 06 10:23:49 ubuntu18 systemd[1]: Started PostgreSQL Cluster 12-main.

$ systemctl is-enabled postgresql
enabled   
```
   - **Step 4:** Test PostgreSQL Connection.
   
During installation, a postgres user is created automatically. This user has full superadmin access to your entire PostgreSQL instance. Before you switch to this account, your logged in system user should have sudo privileges.
```
$ sudo su - postgres
```

You should reset password. Pick a password and you need to **remember** it. 
**You will need it with _3D City DB_ and _pgAdmin 4_.**  
```
$ psql -c "alter user postgres with password 'StrongAdminP@ssw0rd'"
```
For example, `$ psql -c "alter user postgres with password 'GabbyP@ssw0rd'"`

Start PostgreSQL prompt by using the command:
```
$ psql
```
You can try creating database 
```
$ psql
psql (12.0 (Ubuntu 12.0-1.pgdg18.04+1))
Type "help" for help.

postgres=# \conninfo
You are connected to database "postgres" as user "postgres" via socket in "/var/run/postgresql" at port "5432".

postgres=# CREATE DATABASE mytestdb;
CREATE DATABASE

postgres=# \l
```

### 3. Install `PostGIS`. 

**Step 1:** Install `postgis`. Make sure to select the `Postgis` that match with `PostgreSQL`. With PostgreSQL 12, I have to install `Postgis`  with the following command:
```
sudo apt install postgis postgresql-12-postgis-3
```
**Step 2:** Enable PostGIS on Ubuntu 20.04/18.04

```
sudo -i -u postgres
```
### 4. Install `pgAdmin 4` (optional). 

This is only optional. This tool may be useful for checking the created database. 

Load the repo...
```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list
```

Install  `pgAdmin 4` ...  

```
sudo apt update
sudo apt install pgadmin4 pgadmin4-apache2

```
During installation, you’re asked to configure initial user account and password . 
The previous one that you have set. For example ... 

```
user: postgres
pass: GabbyP@ssw0rd
```

After the isntallation is finished, `pgAdmin 4` is activated with...

```
$ pgadmin4
```

How to use `pgAdmin4` is provided in https://computingforgeeks.com/how-to-install-pgadmin-4-on-ubuntu/.

#### Possilbe errors

Before I can run `$ pgadmin4`, I get errors related to `apache2`. 

You can verify this by using the following command. 
```
$ systemctl status apache2
```
If you cannot Start Apache2 (by getting the error `AH00072: make_sock: could not bind to address [::]:80`) similar to 
https://askubuntu.com/questions/1145403/cannot-start-apache2... 

The you may use the following steps to solve the problem. 

- **The 1st problem.**  The port 80 was used for Internet connections in my computer...
You can verify this by 
```
sudo lsof -i TCP:80
```
Therefore I have to change the port number from 80 to other number. (Here I choose 3000 instead)
You need to change the following files:
```
/etc/apache2/ports.conf 
/etc/apache2/sites-enabled/000-default.conf 
```
For example,  you can use `nano` to change the port number:  
```
sudo nano /etc/apache2/ports.conf 
```
Replace `80` with `3000`. Then, save changes with `Ctrl + O` and click `Enter`. Then, exit with `Ctrl + X`   

- **The 2nd problem.**  If you still see the warning that `Apache could not reliably determine server`
https://askubuntu.com/questions/256013/apache-error-could-not-reliably-determine-the-servers-fully-qualified-domain-n  

Then, this can be fixed by adding a phrase`ServerName localhost` into the last line of `/etc/apache2/apache2.conf`.
```
sudo nano /etc/apache2/apache2.conf
```
Then go to the last line and type 
```
ServerName localhost
```
Then, save changes with `Ctrl + O` and click `Enter`. Then, exit with `Ctrl + X`.

And then restart apache by typing into the terminal:

```
sudo systemctl reload apache2
```
If you still have the problem, you may try the full instruction from the first answer (with about 575 likes.).   

### 5. Install `Google Earth Pro` (optional).

```
$ wget -O ~/google-earth.deb https://dl.google.com/dl/earth/client/current/google-earth-pro-stable_current_amd64.deb
$ sudo dpkg -i ~/google-earth.deb
```
  
Ref: https://linuxconfig.org/how-to-install-google-earth-on-ubuntu-20-04-focal-fossa-linux

## Quick-Start.


### Settings

#### Install `3D-City-DB Importer/Exporter` (v.4.2.3)

- Open site https://www.3dcitydb.org/3dcitydb/downloads/ and download `3DCityDB-Importer-Exporter-4.x.x-Setup.jar`. Then, run the .jar with double clicks. I would recommend install all the plugins as you may need them in future. 

#### Creat DATABASE

To create the database, you will have to: 
- (1) Configure the `CONNECTION_DETAILS.sh` in `3DCityDB-Importer-Exporter/3dcitydb/postgresql/ShellScripts/Unix` (if you are using Ubuntu with `postgresql`);
- (2) Run `CREATE_DB.sh`.  
- (3) If you have done this before and want to change the SRID and ESPG, you will not be sucessful when running `CREATE_DB.sh`. To solve this problem, you have to run `DROP_DB.sh`. 

##### Example of `CONNECTION_DETAILS.sh`. 

```
#!/bin/bash
# Provide your database details here ------------------------------------------
export PGBIN=path_to_psql
export PGHOST=localhost
export PGPORT=5432
export CITYDB=CityModelX
export PGUSER=postgres
#------------------------------------------------------------------------------
```

##### How to create another database with different Connection Details using `pgAdmin 4`.

To do this, you can follow the instruction in Exercise 2: 3D City Database Setup. 
http://www.3dcitydb.org/3dcitydb/fileadmin/TUM_Workshop/Documents/Tutorial.pdf


### Import-Export-visualize


- You can follow the instruction in http://www.3dcitydb.org/3dcitydb/fileadmin/TUM_Workshop/Documents/Tutorial.pdf

- You can download .gml file used in the tutorial from https://github.com/3dcitydb/importer-exporter/issues/80#issuecomment-476100406 ... 

- If you cannot view Map, you may have to install the more recent version from https://github.com/3dcitydb/importer-exporter by building form source see https://github.com/3dcitydb/importer-exporter#building. You don't need to uninstall the previously installed package (.jar).  


## PostgreSQL Storage Problem in root dir.

After using the package for a while, you may encounter with `/` is getting full. 
With large .gml files dataset, you can get `+10GB` very easily. 

To verirfy if PostgreSQL is the cause of the problem, you can check with 

$ sudo apt install baobab
$ sudo baobab

You may look for `/var/lib/postgresql/12/main`... and see if the lumpsum storage is owned by this directory. 

If the answer is YES, then there are two approaches to address the problem:

- Use VACUMM to delete files in `/var/lib/postgresql/12/main`... 
- Move the data directory from  `/var/lib/postgresql/12/main` to other place. << I think this one is less riskly.  

I have used my home directory to store the file generated from PostgreSQL such as`/home/user-gabby`.

The steps are ...
 
- **Step 1.** Verfiy if `data_directory`  of PostgreSQL points to `/var/lib/postgresql/12/main`:
```
$ sudo su - postgres
postgres=#SHOW data_directory;
```
If you get the following results, then the answer is YES.
```
Output
       data_directory       
------------------------------
/var/lib/postgresql/12/main
(1 row)
```
Once you’ve confirmed the directory on your system, type `\q` abd then `exit` to quit.

- **Step 2.** Shut down PostgreSQL before we actually make changes to the data directory.
 
