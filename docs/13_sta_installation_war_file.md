---
title: 13. STA Installation WAR-File
layout: page
---

## Introduction

This tutorial shows you how to install a Sensor Things API (STA). To be able to install the
__52°North STA__ the following software has to be downloaded and installed:

- __Java Runtime Enviroment__ (JRE) 8.0 or higher
- __Application server__ compatible to Java Servlet-API 2.5 or higher
- Running __database managment system__

For Windows systems we provide a tutorial, how to setup the system for the installation of the STA.
In the tutorial we use __Apache Tomcat__ as the application server
and __PostgreSQL/ PostGIS__ as the database management system: [Tutorial](89_installation_requirments_for_windows.md)

## Installing the Webapp

When your system matches the requirments above, download the package including the __war-file__: [52°North STA latest version](https://github.com/52North/sensorweb-server-sta/releases/tag/v3.0.0)

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

Now you successfully installed your STA.