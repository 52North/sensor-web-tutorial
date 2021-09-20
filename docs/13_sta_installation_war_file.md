---
title: 13. STA Installation WAR-File
layout: page
---

## Introduction

This tutorials shows you how to install your first Sensor Things API (STA). To be able to install the
__52°North STA__ the following software have to be downloaded and installed:

- __Java Runtime Enviroment__ (JRE) 8.0 or higher
- __Application server__ compatible to Java Servlet-API 2.5 or higher
- Running __database managment system__

In this tutorial we use __Apache Tomcat__ as the applicaition server and __PostgreSQL/ PostGIS__ as the
database managment system.

## Installing Java

First you need to download and install a __Java Runtime Enviroment__ (JRE) or __Java Development Kit__ (JDK) with the
__version 8.0 or higher__: [Download](https://www.oracle.com/java/technologies/javase-downloads.html)

During the installation follow the instructions of the installer.

## Installing Tomcat

After you installed Java you need to download and install an __Apache Tomcat server version 8 or higher__:
[Download](https://tomcat.apache.org/download-90.cgi)

- __Do not use Tomcat version 8.0.8__ because there is a known bug in these releases whereby the STA installation fails
- __Do not use Tomcat version 10__ because Tomcat has changed from javax to jakarta and the STA installation does not
yet work

During the installation follow the instructions of the installer.

After the installation Tomcat is added to the Windows autostart. You can deactived the autostart function of the
application in the Taskmanager. If you want to start the application manually you can do this by navigating into
the installation folder `Apache Software Foundation/Tomcat 9.0/bin` and executing `Tomcat9w.exe`. Mind that the
names are different for other versions of Tomcat.

![tomcatControl.PNG](images/tomcatControl.PNG "Apache Tomcat 9.0 Tomcat9 Properties")

This window will show up when `Tomcat9w.exe` is executed.

1. Here you can start and stop the Tomcat server

If your Tomcat server is running you can check if it was succesfully set up by entering this URL in your
internet browser except you choosed a different port for the installation:
[http://localhost:8080/](http://localhost:8080/)

If the Tomcat server is running you should see this page:

![tomcatStartpage.PNG](images/tomcatStartpage.PNG "Apache Tomcat 9.0 Startpage")

## Installing PostgreSQL/ PostGIS

Now you need to download and install __PostgreSQL version 9 or higher__: [Download](https://www.postgresql.org/download/)

The installer includes the __PostgreSQL server__, __pgAdmin__ which is a graphical tool for managing and developing
your databases, and __StackBuilder__ which is a package manager that can be used to download and install additional
PostgreSQL tools and drivers. Follow the instructions of the installer.

In the next step you use the __StackBuilder__ application to add the __PostGIS__ extension to PostgreSQL.

![stackBuilder.PNG](images/stackBuilder.PNG "Stack Builder 4.2.1")

After that you need to create a new database which has the PostGIS template. You can use __pgAdmin__ to create the
database. With the command `create extension postgis;` you can add the PostGIS extension.

## Installing the Webapp

Download the package including the __war-file__: [52°North STA latest version](https://github.com/52North/sensorweb-server-sta/releases/tag/v3.0.0)

Unzip the package and browse to the folder `UNZIPPED_PACKAGE/bin/target` where the file `52n-sensorthings-webapp.war`
is located. Copy the file `52n-sensorthings-webapp.war` into the folder `TOMCAT_BASE/webapps`. Make sure your Tomcat
and PostgreSQL are running. After a moment the __war-file__ gets converted and in the folder should be a new
folder `52n-sensorthings-webapp`. Next navigate in this new folder to the `application.yml` and open it with
an editor (`TOMCAT_BASE\webapps\52n-sensorthings-webapp\WEB-INF\classes`). You need to adjust the following
settings manually:

- __server__ -> __rootURL__: Used for response serialization + url parsing, external URL Must be set correctly!
- __server__ -> __servlet__ -> __context-path__: The path of the running STA (name of the applet in the
Apache Tomcat).
- __spring__ -> __datasource__: Connection information to the database
	- __username__: Database user name
	- __password__: Database user password
	- __url__: URL to the database (last value is the database name)
	- __jpa__ -> __properties__ -> __hibernate__ -> __hbm2ddl___ -> __auto__: If you use the STA together
	with the SOS, set the setting to `none`!


If this is the case than you can reach the webapp with this URL:
[http://localhost:8080/52n-sensorthings-webapp/](http://localhost:8080/52n-sensorthings-webapp/)

When you successfully reach the service the response should be a json document looking like this:

~~~json
{
    "value": [
        {
            "name": "Datastreams",
            "url": "http://localhost:8080/52n-sensorthings-webapp/Datastreams"
        },
        {
            "name": "Observations",
            "url": "http://localhost:8080/52n-sensorthings-webapp/Observations"
        },
        {
            "name": "Things",
            "url": "http://localhost:8080/52n-sensorthings-webapp/Things"
        },
        {
            "name": "Locations",
            "url": "http://localhost:8080/52n-sensorthings-webapp/Locations"
        },
        {
            "name": "HistoricalLocations",
            "url": "http://localhost:8080/52n-sensorthings-webapp/HistoricalLocations"
        },
        {
            "name": "Sensors",
            "url": "http://localhost:8080/52n-sensorthings-webapp/Sensors"
        },
        {
            "name": "ObservedProperties",
            "url": "http://localhost:8080/52n-sensorthings-webapp/ObservedProperties"
        },
        {
            "name": "FeaturesOfInterest",
            "url": "http://localhost:8080/52n-sensorthings-webapp/FeaturesOfInterest"
        }
    ],
    "serverSettings": {
        "conformance": [
            "http://www.opengis.net/spec/iot_sensing/1.1/req/datamodel",
            "http://www.opengis.net/spec/iot_sensing/1.1/req/resource-path/resource-path-to-entities",
            "http://www.opengis.net/spec/iot_sensing/1.1/req/request-data",
            "http://www.opengis.net/spec/iot_sensing/1.1/req/create-update-delete",
            "https://github.com/52North/sensorweb-server-sta/extension/server-properties.md",
            "https://github.com/52North/sensorweb-server-sta/extension/server-version.md"
        ],
        "https://github.com/52North/sensorweb-server-sta/extension/server-properties.md": {
            "escapeId": true,
            "implicitExpand": false,
            "updateFOI": false,
            "variableEncodingType": false,
            "isMobile": false,
            "mqttReadOnly": false,
            "httpReadOnly": false,
            "mqttPublishTopics": "Observations",
            "observation": {
                "samplingGeometry": "http://www.opengis.net/def/param-name/OGC-OM/2.0/samplingGeometry",
                "verticalFrom": "verticalFrom",
                "verticalTo": "verticalTo",
                "verticalFromTo": "vertical"
            }
        },
        "https://github.com/52North/sensorweb-server-sta/extension/server-version.md": {
            "git.repository": "https://github.com/52North/sensorweb-server-sta",
            "git.lastCommitDate": "2020-12-16 12:55:46+0100",
            "git.path": "d23185a8b80110405841d7a230345d2ebf26a53a",
            "git.lastCommitMessage": "[maven-release-plugin] prepare release v3.0.0",
            "git.revision": "d23185a8b80110405841d7a230345d2ebf26a53a",
            "project.name": "52North SensorThingsAPI",
            "project.version": "3.0.0",
            "project.time": "2020-12-16T12:04:42.179Z",
            "git.builddate": "2020-12-16 13:04:46+0100"
        }
    }
}
~~~

Now you successfully installed your first STA.