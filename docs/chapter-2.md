---
title: Sensor Web Best Practices - Chapter 2
layout: post
---

## Important components of SWE

Within this section, several base components of the SWE framework are denoted
and shortly described. Within the subsequent sections, certain key aspects are
elaborated in more detail. First, aspects about sensor data sources are covered.
Second, the relevant OGC standards are introduced.

### 5.1 Data Sources (Sensors)

Fundamentally, data sources of observations can be distinguished as follows. For
once, sensor entities capturing observations are trivial data sources, as they
are the producers of observations. However, a sensor might store the observation
within an arbitrary repository, e.g. databases or file-based formats like
NetCDF. Thus, any data repository persisting sensor metadata and observation
data can be used as data source. End-users are provided with a homogeneous query
interface via dedicated Web services to access sensor data, independent from the
actual data source. From the viewpoint of data providers, a concrete
implementation of these Web services may vary for different data sources. In
general, it is best practice to store captured data within a database, since
that ensures performant CRUD (Create, Retrieve, Update, Delete) operations on
the stored data.

Obviously, to store sensor data, certain rules with regard to the data schema
have to be considered. A concrete implementation of a SOS may define a different
schema to structure how sensor data is persisted. This schema definition is a
vital aspect of a service implementation in order to perform service operations
correctly. Using a data repository as data source, new observations can be added
dynamically. In theory, observations measured by a physical or virtual sensor
can be inserted into the data source directly. As a recommended alternative, the
OGC Sensor Observation Service offers transactional operations to insert or
remove data to/from the repository dynamically. Using transactional service
operations ensures that the service implementation performs the operation in
conformance to the expected data schema. Otherwise, the SOS might not be able to
access the data properly.

### 5.2 OGC Standards

The SWE framework comprises multiple formats for interoperable exchange of
sensor data as well as Web Service specifications to allow access to sensor data
as well as tasking of sensors. The following list gives an overview of central
SWE standards. Where applicable, a sufficient description of the standard is
included. For key standards, the short description references a subsequent
section within this document, providing more details. Note that only the most
recent versions (date 09.09.2016) of each standard are denoted.

Standardized **formats**:

* **Observations & Measurements (O&M)**: The O&M standard defines how an
Observation is encoded and structured, as elaborated in Section 6.2.

* **PUCK Protocol Standard**: PUCK specifies a protocol for RS232 and Ethernet
connected sensor instruments. Via dedicated commands, relevant sensor metadata
(e.g. sensor description encoded in SensorML) can be retrieved from the
instrument directly. In addition, offered configurations and settings can be
remotely applied to an instrument using dedicated PUCK operations. The official
specification is available under http://www.opengeospatial.org/standards/puck.

* **Sensor Model Language (SensorML)**: SensorML specifies a homogeneous
description of procedure metadata, describing the entity that measures an
observable property including details on how it is measured. In consequence, for
each different procedure type (e.g. physical sensors or virtual computations),
the same set of descriptive elements can be applied to provide an interoperable
sensor description. Section 6.1 presents an exemplar overview of how to encode
sensor metadata using SensorML.

Standardized **Web Services**:

* **Sensor Observation Service (SOS)**: The SOS is the central Web service within
the SWE framework. It allows the retrieval of various sensor data including
Sensor descriptions as SensorML, observation data encoded in O&M or specific
data like features or certain results. Moreover, certain transactional
operations are specified to add/remove new sensors as well as observations. An
extensive overview of the available operations and relevant other service
aspects are given in section 7.1.

* **Sensor Planning Service (SPS)**: Via dedicated operations offered by a SPS,
the sensors themselves can be tasked to perform certain measurements, as
elaborated in section 7.2.

* **SensorThings API**: As elaborated in section 10, the SensorThings API is
targeted to allow homogeneous access to heterogeneous smart sensors within the
Web of Things approach. The standard is located at
http://docs.opengeospatial.org/is/15-078r6/15-078r6.html.

Additional relevant concepts:

* **SWE Common Data Model**: This document defines data types shared by multiple
SWE specifications. Hence, it serves as a base document but offers no operations
or encodings. The specification is available from
http://www.opengeospatial.org/standards/swecommon.

* **SWE Service Model**: Within this document, common data types that are relevant
for other specifications are specified. In particular, the in- and output types
of the SOS operations are defined. The standard can be downloaded via a link on
the website http://www.opengeospatial.org/standards/swes.

Other services, e.g. offered by 52°North:

* **Discovery**: To discover sensor data repositories as well as available SWE
Web services offered by different suppliers, catalogue-based services can be
employed. According to Bröring et al. (2011 a), the standard OGC Catalogue
(Nebert et al.) is not sufficient to reflect the requirements of SWE, due to
different metadata models (SensorML versus ebRIM) and the dynamic nature of
sensor networks. In consequence, some enhancements have been proposed to allow
discovery and harvesting of sensor data through a catalogue-based
infrastructure. To give an overview of the resulting enhancements the following
table was adopted from Bröring et al.:

| Specification          | Description    |
| ------------- | ------------- |
| SensorML Profile for Discovery | Profile of SensorML ensuring the presence of a minimum set of metadata that is necessary to allow sensor discovery |
| SensorML-ebRIM Mapping | Mapping of SensorML elements into the ebRIM Catalogue information model in order to enable the management of sensor metadata by OGC Catalogues. |
| Sensor Instance Registry (SIR) | Web service interface for managing sensor metadata; this includes the collection of sensor metadata, management of sensor status information as well as functionality for pushing sensor metadata into OGC Catalogues. |
| Sensor Observable Registry (SOR) | Web service interface for accessing phenomenon definitions and for exploring semantic relationships between different phenomena. |

With the help of these additional components, standard OGC Catalogues can be
used to discover sensor data. For instances end-users may query potential
sensors observing certain observable properties, suppliers of SWE Web services
can make their services publicly available to others. For more detailed
information, please refer to Bröring et al. (2011 a) or 52°North (2016 a).

* **Sensor Event Service (SES)**: The SES realizes a publish-subscribe
notification service, where users may subscribe to so-called topics like
_Measurement_ or _SensorManagement_ to receive an associated notification on
occurring events. For instance, having subscribed to Measurements, all
subscribers receive an O&M encoded observation as soon as it is available. For
more information, please consult 52°North (2016 b).
