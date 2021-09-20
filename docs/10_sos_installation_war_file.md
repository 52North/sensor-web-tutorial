---
title: 10. SOS Installation WAR-File
layout: page
---

## Introduction

This tutorial shows you how to install a Sensor Observation Service (SOS). To be able to install the
__52°North SOS__ the following software has to be downloaded and installed:

- __Java Runtime Enviroment__ (JRE) 8.0 or higher
- __Application server__ compatible to Java Servlet-API 2.5 or higher
- Running __database managment system__

For Windows systems we provide a tutorial, how to setup the system for the installation of the SOS.
In the tutorial we use __Apache Tomcat__ as the application server
and __PostgreSQL/ PostGIS__ as the database management system: [Tutorial](89_installation_requirments_for_windows.md)

If you want to use different software, have an other operating system
or want to build the SOS from source you can find more information here:
[52°North SOS 5.x Documentation](https://wiki.52north.org/SensorWeb/SensorObservationServiceVDocumentation)

## Installing the Webapp

When your system matches the requirments above, download the package including the __war-file__: [52°North SOS latest version](https://github.com/52North/SOS/releases)

Unzip the package and browse to the folder `UNZIPPED_PACKAGE/bin/target` where the file `52n-sos-webapp.war`
is located. Copy the file `52n-sos-webapp.war` into the folder `TOMCAT_BASE/webapps`. Make sure your Tomcat
and PostgreSQL are running. After a moment the __war-file__ gets converted and in the folder should be a new
folder `52n-sos-webapp`. If this is the case than you can reach the webapp with this URL:
[http://localhost:8080/52n-sos-webapp/](http://localhost:8080/52n-sos-webapp/)

When you succesfully reach the service it should look like this:

![webappStartpage.PNG](images/webappStartpage.PNG "52°North SOS Startpage")

You can start the installation process by clicking on the link in the red banner. Follow the instructions along
and you will succesfully install your SOS.

![webappInstallCompleted.PNG](images/webappInstallCompleted.PNG "52°North SOS Startpage")