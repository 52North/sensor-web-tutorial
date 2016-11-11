---
title: 8. Internet of Things
layout: page
---

## Sensor Web and the Internet of Things

> TBD: Simon proof-read!

The keywords Internet of Things and Web of Things denote current approaches
to bring smart devices into the Web. In the context of the Sensor Web, such
smart devices can be sensors that measure or compute certain phenomena and
bring the results into the Web. From a technological perspective, this means
that the smart device (e.g. a sensor) is accessible via a unique URL identifier
and its functionalities (e.g. observation results) addressable via standard
HTTP operations (GET, POST, PUT, etc.), thus realizing a RESTful access to
sensor data ([Bröring et al, 2011 a](99_bibliography.html)). This section intends to give an overview
of how the Web of Things approach can be linked to the SWE
framework/infrastructure. According to [Bröring et al. (2011 b)](99_bibliography.html), a smart
device may respond to data requests with formats standardized by SWE. To
be precise, metadata about the sensor itself should be encoded via SensorML
and observation data as O&M. With this format restrictions, any other
application/component may retrieve the sensor data of the smart device in
an interoperable standardized manner.

However, considering the access to sensor data, SWE’s goal is to standardize
the query interface as well as other services (like tasking, subscribing)
through dedicated Web Services (SOS, SPS, SES). As a smart device, each
individual sensor may offer RESTful access to its sensor data, but this
service interface may not conform to the service interface
specified/standardized by SWE. For this reason, the OGC specified the
**SensorThings API** to access data from smart sensors in an interoperable
way ([Liang et al, 2016](99_bibliography.html)). The SensorThings API is part of SWE and bases on
the data model of O&M. Basically, it can be considered as a lightweight SWE
profile designed specifically for resource-restricted smart devices. The
keyword lightweight indicates that in contrast to other SWE services like
SOS or SPS, the SensorThings API may simplify the connections between
devices-to-devices and devices-to-applications through usage of efficient
REST binding including a reduced JSON encoding and the MQTT protocol for
sensor access. Liang et al. stress that the classical SWE services are too
heavyweight for combination with smart sensors. But they also highlight, that
the complexity of SOS, SPS and SES are well justified for more complex use
case scenarios, where the Web of Things approach is not applicable. Overall,
the following table compares the SensorThings API to the SOS.


|           | SensorThings API   | Sensor Observation Service   |
| ------------- | ------------- | ------------- |
| Encoding| JSON| XML|
| Architecture Style| Resource Oriented Architecture| Service Oriented Architecture|
| Binding| REST| SOAP|
| Inserting new sensors or observations| HTTP POST (e.g., CRUD)| using SOS specific interfaces,<br/>  e.g. InsertSensor(), InsertObservation()|
| Deleting existing sensors| HTTP DELETE| using SOS specific interfaces,<br/>  i.e. DeleteSensor()|
| Pagination| $top, $skip, $nextLink| Not Supported|
| Pub/Sub Support| MQTT and SensorThings MQTT Extension| Not Supported|
| Updating properties of existing sensors or observations| HTTP PATCH and JSON PATCH| Not Supported|
| Deleting observations| HTTP DELETE| Not Supported|
| Linked data support| JSON-LD| Not Supported|
| Return properties subset| $select| Not Supported|
| Request multiple O&M entities <br/>(e.g., FeatureOfInterest and Observation)<br/> in one request/response| $expand| Not Supported|

Overall, the SensorThings API may be facilitated to connect to heterogeneous
single smart sensors using a homogeneous standardized RESTful interface. However,
this Web of Things approach is only applicable for simple and concrete use cases.
Considering more complex scenarios like disaster management, where sensor data
from various sensors is required for a composed analyzation, the Web of Things
approach may not be applicable ([Bröring et al, 2011 b](99_bibliography.html)).

In conclusion, SWE and the Web of Things show potential to be combined. Smart
sensors can encode their data using standardized SWE formats
([Bröring et al., 2011 b](99_bibliography.html)) and their data can be offered
through the standardized SWE SensorThings API ([Liang et al., 2016](99_bibliography.html)). Especially
for simple use cases, where the full functionality on data ﬁltering, sensor
discovery, tasking and event handling, as provided by classical SWE services
(SOS, SPS, SES), are not required, the Web of Things approach can be facilitated
to provide access to the sensor data of the smart sensor. However, with regard
to complex scenarios, the Web of things approach might not be applicable, as
highlighted by [Bröring et al. (2011 a)](99_bibliography.html)
and [Liang et al. (2016)](99_bibliography.html).
