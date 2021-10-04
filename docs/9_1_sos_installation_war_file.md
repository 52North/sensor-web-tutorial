---
title: 9.1. SOS Installation WAR-File
layout: page
---

## Introduction

This tutorial shows you how to install a 52°North Sensor Observation Service (SOS). To be able to install the
__52°North SOS__ the following software has to be downloaded and installed:

- __Java Runtime Enviroment__ (JRE) 8.0 or higher
- __Application server__ compatible to Java Servlet-API 2.5 or higher
- Running __database managment system__

For Windows systems we provide a tutorial, how to setup the system for the installation of the SOS.
In the tutorial we use __Apache Tomcat__ as the application server
and __PostgreSQL/PostGIS__ as the database management system: [Tutorial](89_installation_requirments_for_windows.md)

If you want to use different software, have an other operating system
or want to build the SOS from source you can find more information here:
[52°North SOS Documentation](https://wiki.52north.org/SensorWeb/SensorObservationServiceVDocumentation){target=_blank}

### Installing the Webapp

#### Download and deploy

When your system matches the requirments above, download the  __war-file__: [52°North SOS workshop version](http://52north.org/delivery/SensorWeb/Workshops/Frejus_2021/52n-sos-webapp.war){target=_blank}

> ####### Activity 1
>  
> 1. Save the file to `/home/demo/`
> 1. Open a terminal [see](09_hands-on/#open-a-terminal){target=_blank}
> 1. Copy the file `52n-sos-webapp.war` into the folder `TOMCAT_BASE/webapps`
> 	* Type `sudo cp /home/demo/52n-sos-webapp.war /opt/tomcat/webapps/`
> 1. Press *enter*
>

After a moment the __war-file__ gets converted and in the folder should be a new
folder `52n-sos-webapp`. If this is the case than you can reach the webapp with this URL:
[http://localhost:8080/52n-sos-webapp/](http://localhost:8080/52n-sos-webapp/){target=_blank}

If the download does not work, you find an already downloaded version in `/home/demo/webapps/`:

  * Alternative `sudo cp /home/demo/52n-sos-webapp.war /opt/tomcat/webapps/`

#### Installation

> ####### Activity 2
>
> 1. Click the link or open a *browser* and enter the following URL:
>
>>  [http://localhost:8080/52n-sos-webapp/](http://localhost:8080/52n-sos-webapp/){target=_blank}

When you succesfully reach the service it should look like this:

![webappStartpage.PNG](images/webappStartpage.PNG "52°North SOS Startpage")

You can start the installation process by clicking `here` on the link in the *red banner*.

#####  Welcome page

The installation process starts with the Welcome-page.

![SetupSOS_1.PNG](images/SetupSOS_1.PNG "52°North SOS Installation Wizard Welcome-Page")

> ####### Activity 3
>
> 1. Click the blue `Start` button.

#####  Select the datasource

In the next steps you configure your datasource. 

First you need to select the database managment system which you are using as the datasource.

The 52N SOS supports the database managment system:

* H2/GeoDB (*file based* and *in memory*)
* MySQL/MariaDB
* Oracle Spatial
* PostgreSQL/PostGIS
* SQL Server
  
The *Custom* datasource should be selected if you do not use the created database model but views or adjusted hibernate mapping files. In this case the *existing* database model would not be validated against the expected!

![SetupSOS_2.png](images/SetupSOS_2.png "52°North SOS Installation Wizard Datasource Configuration")

> ####### Activity 4
>
> 1. In this tutorial we select `PostgreSQL/PostGIS` as datasource.

Next you need to set the parameters of your database.

###### Database Configuration

In the *Database configuration* you define the connection parameters and credentials of the database.

![SetupSOS_3.png](images/SetupSOS_3.png "52°North SOS Installation Wizard Datasource Configuration")

> ####### Activity 5
>
> As we have created the `sensorweb` database, we have to change the `Database` paramert
>
> 1. Change `Database` from `sos`to `sensorweb`

###### Advandced Database Configuration

The *Advandced Database configuration* provides additional settings for the database like the *schema* or *minimum and maximum connections*.

You can also define the database model selecting other `Database concept`s, `Database extension`s and `Feature concept`s.

The `Database concept` provides three models:

* Simple database model (minimal tables and columns, `No transactional support!`)
* Transactional database model (default)
* eReporting database model (extended transactional model for eReporting)

The `Database concept` provides two selections:

* Default database model
* Extended model to support Samplings/MeasuringPrograms

The `Feature concept` provides two selections:

* Default feature concept
* Extended feature concept (support for storing full WaterML 2.0 MonitoringPoint)


In this tutorial we use the **default configuration**!

![SetupSOS_4.png](images/SetupSOS_4.png "52°North SOS Installation Wizard Datasource Configuration")

> ####### Activity 6
>  
>  * No, we use the default configuration

###### Actions

Under Actions you can chose if you want to create new table, delete all extings tables or update all extings tables in your database. If you use the database for the first time you do not want to change
the settings and leave only `Create tables` marked. When the database model already exists `unselect` the `Create tables`. 

![SetupSOS_5.png](images/SetupSOS_5.png "52°North SOS Installation Wizard Datasource Configuration")

> ####### Activity 7
> 
> 1. Press `Next` button

##### Settings

On the *Settings* installation page you can define several configuration parameter of the SOS.

This includes parameter for the SOS Capabilities (service provider and identification), CRS of the datasource, service parameter and specific parameter for some use cases.

All these settings can be changed later in the [administrative backend](9_2_sos_admin_interface.md).

###### Service Provider

Here you can define the information about the `Service Provider` which is provided in the Capabilities.

![SetupSOS_6.png](images/SetupSOS_6.png "52°North SOS Installation Wizard Optional Settings")

You can also upload a service provider file which overrides the above settings.

![SetupSOS_7.png](images/SetupSOS_7.png "52°North SOS Installation Wizard Optional Settings")

###### Service Identification

Here you can define the information about the `Service Identification` like:

* Title
* Abstract

This information would be provided in the Capabilities.

###### Service

The `Service` settings provides the definition of

* SOS URL
* Strinct SpatialFiltering
* Create/Update FOI geometry
* List only parent offerings
* Insert procedure/FOI via InsertResult
* ...

![SetupSOS_7_1.png](images/SetupSOS_7_1.png "52°North SOS Installation Wizard Optional Settings")

###### Miscellaneous

The `Miscellaneous` settings provides the definition of

* Separator
* Prefixes
* HTTP Status
* GetDataAVailability v2.0
* Identifier for nothing/easting/altitude in SweCoordinates

###### Transactional Security
The `Transactional Security` settings provides the definition of

* En-/Disable the simple transactinal security
* Configure the allowed IPs or the Authorization token

###### CRS
The `CRS` settings provides the definition of

* Axis order in the database
* The default CRS for database and responses
* Supported CRS

###### Procedure Description
The `Procedure Description` settings provides the definition of

* Enrich the SensorML description with additional information (from database)
* Define some parameter used for SensorML generation

###### EXI Binding
The `EXI Binding` settings provides the configuration of how EXI should be compressed

###### Streaming
The `Streaming` settings provides the definition dis-/enable the XML Streaming and define the chunk size for database queries

###### eReporting
The `eReporting` settings provides the definition of eReporting specific parameter

###### FlexibleIdentifier
The `FlexibleIdentifier` settings allows to define for which Objects the human readable name should be used in the response as identifier (if available)

###### I18N
The `I18N` settings provides the definition of default language

###### ISNPIRE
The `ISNPIRE` settings provides the definition of

* Enable INSPIRE
* Definition of INSPIRE metadata (used in the Capabilities)

###### Identifier Prefix
The `Identifier Prefix` settings provides the definition of
Define prefxes for the identifier (mainly for data not inserted via SOS-T)

###### UVF Encoding
The `UVF Encoding` settings provides the definition of
Special format for exchanging data (customer)

###### OceanSITES netCDF,
The `OceanSITES netCDF` settings provides the definition of OceanSITES specific netCDF parameter

###### netCDF
The `netCDF` settings provides the definition of netCDF output parameter/definitions

###### Procedure request/response handling
The `Procedure request/response handling` settings provides the definition of

- Allow only requesting of procedure instances/aggregations
- Add outputs or encode  child procedure in SensorML

> ####### Activity 8
> 
> 1. Select the `Service Provider` tab
> 	* Change the default definitions
> 		* *Name* -> e.g. `SIST`
> 		* *Website* -> e.g. `https://sist.cnrs.fr/`
> 		* *Phone*
> 		* *Mail-Address*
> 		* *Address*
> 		* *Postal Code*
> 		* *City*
> 		* *State*
> 		* *Country* -> `France`
> 		* ...
> 1. Select the `Service Identification` tab
> 	* Change the default definitions
> 		* *Title* -> `SIST SOS`
> 		* *Keywords* -> `SIST,CNRS, Sensor Web Workshop, Frejus`
> 		* *Abstract* -> `SIST Sensor Observation Service for Sensor Web Workshop`
> 1. Select the `Transactional Security` tab
> 	* *Uncheck* `Transactional security active`
> 1. Select the `Service` tab
> 	* *Uncheck* `Block restrictionless requests`
> 1. Select the `Miscellaneous` tab
> 	* Change *Token separator* to `#`
> 	* Change *Token separator* to `@`
> 1. `Save` changes

##### Finish installation

You finish your installation by setting a username and a password for the SOS.

![SetupSOS_8.png](images/SetupSOS_8.png "52°North SOS Installation Wizard Optional Settings")

> ####### Activity 8
>
> 1. Type `admin` in the *Username" field
> 1. Type `password` in the *Password" field
> 1. Click `Install`

Dependending on the configuration the installation takes a while to create the database model, load the classes and harvest the metadata (if the database was not empty).

![webappInstallCompleted.PNG](images/webappInstallCompleted.PNG "52°North SOS Startpage")

You now have succesfully installed the __52°North SOS__.

#### Additional 

Additional installation/configuration steps which are not part of this hands-on

##### Download latest release

When your system matches the requirments above, download the package including the __war-file__: [52°North SOS latest version](https://github.com/52North/SOS/releases){target=_blank}

Unzip the package and browse to the folder `UNZIPPED_PACKAGE/bin/target` where the file `52n-sos-webapp.war`
is located. Copy the file `52n-sos-webapp.war` into the folder `/opt/tomcat/webapps`. Make sure your Tomcat
and PostgreSQL are running. After a moment the __war-file__ gets converted and in the folder should be a new
folder `52n-sos-webapp`. If this is the case than you can reach the webapp with this URL:
[http://localhost:8080/52n-sos-webapp/](http://localhost:8080/52n-sos-webapp/){target=_blank}

##### Upload a configuration file

The installation process starts with the Welcome-page.

![SetupSOS_1.PNG](images/SetupSOS_1.PNG "52°North SOS Installation Wizard Welcome-Page")

If you have an exported configuration file of a previous SOS installation you can upload this file here.
For that click *Browse* in the *Upload a previous configuration file* section, select the file and click `Upload`.

Otherwise you can start the installation without a configuration file by clicking the blue `Start` button.