# Installation for 3D-City-DB & Importer-Exporter

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

1. Install `Java runtime environment`. I have installed JDK 14 following the [installation-java-on-ubuntu-20-04](https://linuxconfig.org/how-to-install-java-on-ubuntu-20-04-lts-focal-fossa-linux) . 

To see which package is available for your Ubuntu. 
```
$ sudo apt search openjdk
```
My ubuntu has an option to install JDK 14; therefore, I have chosen it. 
```
$ sudo apt install openjdk-14-jdk
```
Then, you can check the installation with `$ java --version`

2. Install `PostgreSQL`. 



## Quick-Start.


### Settings

### Creat-DATABASE.sh

### Database-Connection 

### Import 

### Export

### Visualize


## PostgreSQL Storage Problem in root dir
