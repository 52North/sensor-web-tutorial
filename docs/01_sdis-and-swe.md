---
title: 1. SDIs and the Sensor Web
layout: page
---

## Exemplary Use Case Scenario

In times of climate change and global warming, the exploitation of renewable
resources is of high relevance. In particular, solar energy may be collected
and transformed into electricity with the help of solar plants. In this context,
the Surface Solar Irradiance (SSI) is an indicator for the potential of solar
energy. Via associated SSI measurements, suitable locations for installation of
new as well as monitoring of existing solar plants becomes possible. In
addition, when collecting SSI measurements of the same location over a period of
time, the energy output from solar irradiation can be forecast with respect to
storage and planning ([Menard n.y](99_bibliography.md)). For this reason, SSI data can measured in
several ways, e.g. in-situ pyranometric sensors, satellite image processing or
numerical weather models. Each of these options may be defined as a sensor,
which takes certain inputs and produces SSI data as output. However, each sensor
uses a different access interface as well as input- and output definitions. As a
result, researchers and consumers of the SSI data have to deal with
heterogeneous sensor interfaces, making it hard to combine measurements of
different sources.

To overcome this heterogeneity, standardized formats and services have to be
employed for interoperable exchange of SSI data. According to Menard et al.
(2015), the Sensor Web Enablement (SWE) Framework offers this very
functionality, open standards for sensor data encoding and exchange as well as
several Web services to homogeneously access sensor data from heterogeneous
sensors. Within this Best Practice Guide, the central components of the SWE
framework are introduced to provide basic knowledge on how to use them.

## Spatial Data Infrastructures (SDI)

A Spatial Data Infrastructure denotes a collection of technologies, policies and
institutional arrangements to serve as a basis for discovery, access and
processing of geospatial data (Global Spatial Data Infrastructure Association,
2012). It comprises standards, specifications and agreements to ensure
interoperability and better access to spatial data. Some examples are the EU
Initiative INSPIRE (Infrastructure for Spatial Information in the European
Community) or GEOSS (Global Earth Observation System of Systems). The latter is
of particular interest, as it aims to establish a global public infrastructure
to observe and offer near-real-time environmental data including measured data
from various sensors ([GEO Group on Earth Observations, 2016](99_bibliography.md)). As a crucial part
of the GEOSS infrastructure, standardized formats and services for sensor data
exchange and access can be offered by the SWE framework, realizing a specific
SDI for sensor data. The SWE framework and its relevant parts are addressed in
the subsequent sections.

## What does SWE stand for?

SWE is an acronym for Sensor Web Enablement and comprises standardized formats
and Web service interfaces in the fields of sensor data ([Bröring et al, 2011 a](99_bibliography.md)).
Since 2003, the standardization organisation the Open Geospatial Consortium
(OGC) follows the vision to make heterogeneous sensor data (like remote
satellite / airborne imaging data, in-situ monitoring sensors or even virtual
computations) available for discovery, access and use via interoperable formats
and Web services. Through standardized formats and services the
heterogeneity/complexity of different sensor types and measurement outputs is
hidden from end-users. In particular, as main formats, the Observation &
Measurement (O&M) standard defines how to model observations and the Sensor
Model Language (SensorML) standard may be used to represent metadata about the
observing sensor itself. As the most prominent Web service, the Sensor
Observation Service (SOS) grants access to stored sensor data. Section 5
continues with a detailed overview of all components within the SWE framework,
covering sensor device aspects as well as the standardized formats and services.
In general, [Bröring et al (2011 a)](99_bibliography.md) provide detailed background information as
well as historical developments and an overview of the current state. For any
aspects not covered within this guide, please refer to this article.

## 4. Terms and Definitions within SWE
To better understand the subsequent sections, important and frequently used
terms are defined. Note that the introduced terms relate to the version 2.0 of
the respective standards. For additional information about each term, [Bröring et al. (2011 a)](99_bibliography.md)'s
article or the official specification of the Sensor
Observation Service ([OGC, 2012](99_bibliography.md)) can be consulted.

| Term          | Definition    |
| ------------- | ------------- |
| Feature of Interest (FoI) | Georeferenced abstraction of a real world feature carrying the observed property |
| Observed Property / Phenomenon | The measurable property that is observed by a sensor; Examples are temperature, precipitation quantity, humidity |
| Procedure | The process that performs the measurement; Can be a physical sensor (a hardware device) or a virtual sensor (e.g. software that produces computations or simulations as measurement) |
| Offering | groups collections of observations produced by one procedure and provides additional metadata of the observations |
| Result | The value of the measurement of an observed property |
| Phenomenon Time | Time associated to the observation's result |
| Result Time | Time when the result has been produced |
| Valid time | Time period for which the observations result is valid |
| Observation | Composition of the above properties;<br/>An _observation_ is captured by a _procedure_ that measures an _observable property_ of a _feature of interest_ at a certain _phenomenon time_ and stores the measurement as the _result_. |
