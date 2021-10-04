---
title: SOS/ STA Installation Requirements for Windows
layout: page
---

## Introduction

To be able to install the __52°North SOS__ or __52°North STA__ the following software needs to be installed:

- __Java Runtime Environment__ (JRE) 8.0 or higher
- __Application server__ compatible to Java Servlet-API 2.5 or higher
- Running __database management system__

This tutorial shows you how to set up your Windows System to match the requirements. For this tutorial
we use __Apache Tomcat__ as the application server and __PostgreSQL/ PostGIS__ as the
database management system. If you want to use different software, have an other operating system
or want to build the SOS from source you can find more information here:
[52°North SOS 5.x Documentation](https://wiki.52north.org/SensorWeb/SensorObservationServiceVDocumentation)

### Installing Java

First you need to download and install a __Java Runtime Environment__ (JRE) or __Java Development Kit__ (JDK) with the
__version 8.0 or higher__: [Download](https://www.oracle.com/java/technologies/javase-downloads.html)

During the installation follow the instructions of the installer.

### Installing Tomcat

After you installed Java you need to download and install an __Apache Tomcat server version 8 or higher__:
[Download](https://tomcat.apache.org/download-90.cgi)

- __Do not use Tomcat version 8.0.8__ because there is a known bug in these releases whereby the SOS installation fails
- __Do not use Tomcat version 10__ because Tomcat has changed from javax to jakarta and the SOS installation does not
yet work

During the installation follow the instructions of the installer.

After the installation Tomcat is added to the Windows autostart. You can deactivated the autostart function of the
application in the Taskmanager. If you want to start the application manually you can do this by navigating into
the installation folder `Apache Software Foundation/Tomcat 9.0/bin` and executing `Tomcat9w.exe`. Mind that the
names are different for other versions of Tomcat.

![tomcatControl.PNG](images/tomcatControl.PNG "Apache Tomcat 9.0 Tomcat9 Properties")

This window will show up when `Tomcat9w.exe` is executed.

1. Here you can start and stop the Tomcat server

If your Tomcat server is running you can check if it was successfully set up by entering this URL in your
internet browser except you have chosen a different port for the installation:
[http://localhost:8080/](http://localhost:8080/)

If the Tomcat server is running you should see this page:

![tomcatStartpage.PNG](images/tomcatStartpage.PNG "Apache Tomcat 9.0 Startpage")

### Installing PostgreSQL/ PostGIS

Now you need to download and install __PostgreSQL version 9 or higher__: [Download](https://www.postgresql.org/download/)

The installer includes the __PostgreSQL server__, __pgAdmin__ which is a graphical tool for managing and developing
your databases, and __StackBuilder__ which is a package manager that can be used to download and install additional
PostgreSQL tools and drivers. Follow the instructions of the installer.

In the next step you use the __StackBuilder__ application to add the __PostGIS__ extension to PostgreSQL.

![stackBuilder.PNG](images/stackBuilder.PNG "Stack Builder 4.2.1")

After that you need to create a new database which has the PostGIS template. You can use __pgAdmin__ to create the
database. With the command `create extension postgis;` you can add the PostGIS extension.

Now your Windows system is prepared to install the __52°North SOS__ or __52°North STA__.
