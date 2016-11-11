---
title: 5. Web Services
layout: page
---

## Sensor Web Services

### Sensor Observation Service

The **OGC Sensor Observation Service (SOS)** is a Web Service that allows users
to request observations and associated metadata of sensors. Within the context
of the SWE framework, the SOS represents the core service to access sensor data
in an interoperable and standardized manner. The service specification
(version 2.0), which can be downloaded from

> [http://www.opengeospatial.org/standards/sos](http://www.opengeospatial.org/standards/sos),

defines several mandatory as well as some optional operations, as introduced in
the following. With regard to the core operations
(`GetCapabilities`, `DescribeSensor`, `GetObservation`), a description, the
service interface (request parameters) and an example is presented. For most
optional operations, only a description of the method as well as the request
parameters is included. Most of the subsequent information was extracted from
the SOS 2.0 standard specification ([OGC 2012](99_bibliography.html)). A valid SOS instance is available
via the URL

> [http://insitu.webservice-energy.org/52n-sos-webapp](http://insitu.webservice-energy.org/52n-sos-webapp).

It will be used throughout this section to present examples of the main operations.

#### GetCapabilities

Similar to other OGC services, the `GetCapabilities` operation responds with
metadata about the service instance itself. Amongst others, the Capabilities
document lists available _sensors/procedures_, _offerings_, _observedProperties_,
_featuresOfInterest_ as well as spatial and temporal bounding boxes of the
available _observations_. A `GetCapabilities` request may contain the following
parameters:

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “GetCapabilities”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| acceptVersions| submit accepted versions, e.g. “2.0.0”| no|
| acceptFormats| preferred response formats| no|
| updateSequence| service metadata document version, value is “increased” whenever any change is made in complete service metadata document| no|
| sections| include only relevant sections within the response document and omit the rest| no|

A full response document contains several sections offering metadata. The most relevant sections are:

* **serviceIdentification**: metadata about the service itself, such as title, versions, languages

* **serviceProvider**: metadata about the provider/organization, such as contact information

* **operationsMetadata**: metadata about the offered operations, including which operations are offered (like GetCapabilites, DescribeSensor, GetObservation, …), available bindings (e.g. POX, KVP, SOAP, JSON) and a URL endpoint.

* **contents**: metadata about available observation offerings including their associated properties such as identifier, procedure, reponseFormats, temporal and spatial aspects, …

* **filterCapabilities**: metadata about the supported spatial and temporal filter functionalities
extension: container for extensions

Sending a capabilities request
(e.g. [http://insitu.webservice-energy.org/52n-sos-webapp/sos?service=SOS&REQUEST=GetCapabilities](http://insitu.webservice-energy.org/52n-sos-webapp/sos?service=SOS&REQUEST=GetCapabilities)),
results in a full capabilities document. As an excerpt, the following Listing
presents an exemplary observation offering from the content section of the
Capabilities document.

~~~ xml


<?xml version="1.0" encoding="UTF-8"?>
<sos:ObservationOffering xmlns:sos="http://www.opengis.net/sos/2.0" xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:gml="http://www.opengis.net/gml/3.2" xmlns:swe="http://www.opengis.net/swe/2.0" xmlns:swes="http://www.opengis.net/swes/2.0" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" service="SOS" version="2.0.0" xsi:schemaLocation="http://www.opengis.net/sos/2.0 http://schemas.opengis.net/sos/2.0/sos.xsd">
   <!-- identifier and name of the offering -->
   <swes:identifier>ENTPE-BH-KZ-CH1-QCfull</swes:identifier>
   <swes:name codeSpace="eng">ENTPE-BH-KZ-CH1-QCfull</swes:name>
   <swes:procedure>ENTPE-BH-KZ-CH1-QCfull</swes:procedure>
   <!-- available description format for sensor descriptions; here SensorML 1.0.1 and 2.0 as well as WaterML 2.0 -->
   <swes:procedureDescriptionFormat>http://www.opengis.net/sensorML/1.0.1</swes:procedureDescriptionFormat>
   <swes:procedureDescriptionFormat>http://www.opengis.net/sensorml/2.0</swes:procedureDescriptionFormat>
   <swes:procedureDescriptionFormat>http://www.opengis.net/waterml/2.0/observationProcess</swes:procedureDescriptionFormat>
   <!-- observed phenomena -->
   <swes:observableProperty>Direct Horizontal Irradiance</swes:observableProperty>
   <!-- bounding box of observed area -->
   <sos:observedArea>
      <gml:Envelope srsName="http://www.opengis.net/def/crs/EPSG/0/4326">
         <gml:lowerCorner>45.7786 4.9225</gml:lowerCorner>
         <gml:upperCorner>45.7786 4.9225</gml:upperCorner>
      </gml:Envelope>
   </sos:observedArea>
   <!-- period in time for which the mesaurment applies -->
   <sos:phenomenonTime>
      <gml:TimePeriod gml:id="phenomenonTime_122">
         <gml:beginPosition>2006-01-01T00:00:00.000Z</gml:beginPosition>
         <gml:endPosition>2016-01-01T00:00:00.000Z</gml:endPosition>
      </gml:TimePeriod>
   </sos:phenomenonTime>
   <!-- period in time for when the mesaurment was performed -->
   <sos:resultTime>
      <gml:TimePeriod gml:id="resultTime_122">
         <gml:beginPosition>2006-01-01T00:01:00.000Z</gml:beginPosition>
         <gml:endPosition>2016-01-01T00:00:00.000Z</gml:endPosition>
      </gml:TimePeriod>
   </sos:resultTime>
   <!-- available response formats; here JSON, O&amp;M 2.0 WaterML 2.0 -->
   <sos:responseFormat>application/json</sos:responseFormat>
   <sos:responseFormat>http://dd.eionet.europa.eu/schemaset/id2011850eu-1.0</sos:responseFormat>
   <sos:responseFormat>http://www.opengis.net/om/2.0</sos:responseFormat>
   <sos:responseFormat>http://www.opengis.net/waterml-dr/2.0</sos:responseFormat>
   <sos:responseFormat>http://www.opengis.net/waterml/2.0</sos:responseFormat>
   <!-- observation type; here Measurement -->
   <sos:observationType>http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_Measurement</sos:observationType>
   <!-- type of associated featureOfInterest -->
   <sos:featureOfInterestType>http://www.opengis.net/def/samplingFeatureType/OGC-OM/2.0/SF_SamplingPoint</sos:featureOfInterestType>
</sos:ObservationOffering>
~~~

#### DescribeSensor

The `DescribeSensor` operation can be used to retrieve a detailed sensor
description about a certain sensor, encoded in a Sensor Model Language
(SensorML) version 1.0.1 or 2.0 document. As sensor configuration and
calibration might change over time, several different instances of a sensor
description for the same sensor can be stored by a SOS, each being valid for
a certain amount of time. To request a certain sensor description the following
request parameters are offered:

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “DescribeSensor”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| procedure| reference to the target sensor/procedure | yes|
| procedureDescriptionFormat| selects the target description format identifier| yes|
| validTime| Time instant or period for which the sensor description is retrieved| no|

For instance, an exemplar DescribeSensor request (using the binding POX) against
the URL [http://insitu.webservice-energy.org/52n-sos-webapp/service](http://insitu.webservice-energy.org/52n-sos-webapp/service)
might look like:

~~~ xml
<?xml version="1.0" encoding="UTF-8"?>
<swes:DescribeSensor xmlns:swes="http://www.opengis.net/swes/2.0" xmlns:gml="http://www.opengis.net/gml/3.2" xmlns:swe="http://www.opengis.net/swe/2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" service="SOS" version="2.0.0" xsi:schemaLocation="http://www.opengis.net/swes/2.0 http://schemas.opengis.net/swes/2.0/swes.xsd http://www.opengis.net/swe/2.0 http://schemas.opengis.net/sweCommon/2.0/swe.xsd">
   <!-- reference to sensor/procedure using its identifier -->
   <swes:procedure>ENTPE-BH-KZ-CH1-QCfull</swes:procedure>
   <!-- definition of sensor description format; here SensorML 1.0.1 -->
   <swes:procedureDescriptionFormat>http://www.opengis.net/sensorML/1.0.1</swes:procedureDescriptionFormat>
   <!-- optional validTime: if not set, latest sensor description is returned -->
   <swes:validTime>
      <gml:TimePeriod gml:id="validTime">
         <gml:beginPosition>2014-08-14T15:02:43.219</gml:beginPosition>
         <gml:endPosition>2014-08-14t15:02:47</gml:endPosition>
      </gml:TimePeriod>
   </swes:validTime>
</swes:DescribeSensor>
~~~

With regard to the request,the following Listing presents the basic response
structure. In particular, the SensorML description is included within
_DescribeSensorResponse/description/_ _SensorDescription/data/_. The content
is omitted here by intention.

~~~ xml
<?xml version="1.0" encoding="UTF-8"?>
<swes:DescribeSensorResponse xmlns:swes="http://www.opengis.net/swes/2.0" xmlns:sml="http://www.opengis.net/sensorML/1.0.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   <swes:procedureDescriptionFormat>http://www.opengis.net/sensorML/1.0.1</swes:procedureDescriptionFormat>
   <swes:description>
      <swes:SensorDescription>
         <swes:data>
            <sml:Component>
               <!-- details omitted for brevity -->
            </sml:Component>
         </swes:data>
      </swes:SensorDescription>
   </swes:description>
</swes:DescribeSensorResponse>
~~~

#### GetObservation

The `GetObservation` operation represents the core operation of the SOS to
request observation data encoded in Observations & Measurements (O&M) standard
or any other suitable format (O&M is recommended though for interoperability).
In general a SOS might host a huge number of observations, each composed within
a certain offering. For this reason, several filter options can be submitted in
a GetObservation request, as listed in the below table. Note that although all
filter options are not mandatory, at least some filters should be set in order
to reduce the length of the response document.

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “GetObservation”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| featureOfInterest| reference to a dedicated feature of interest; used to filter observations by feature of interest| no|
| observedProperty| reference to a dedicated observable property / phenomenon; used to filter observations by observable property| no|
| offering| reference to a dedicated offering which composes of observations from a certain procedure and observable property| no|
| procedure| reference to a dedicated procedure; used to filter observations by procedure| no|
| spatialFilter| Used to filter observation with regard to spatial properties| no|
| temporalFilter| Used to filter observation with regard to temporal properties| no|
| responseFormat| Used to specify the desired response format, the default format for SOS 2.0 is http://www.opengis.net/om/2.0 | no|

An exemplar GetObservation request against
[http://insitu.webservice-energy.org/52n-sos-webapp/service](http://insitu.webservice-energy.org/52n-sos-webapp/service)
using POX binding is illustrated in the below example.
It contains an example for each filter option.

~~~ xml
<?xml version="1.0" encoding="UTF-8"?>
<sos:GetObservation xmlns:sos="http://www.opengis.net/sos/2.0" xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:gml="http://www.opengis.net/gml/3.2" xmlns:swe="http://www.opengis.net/swe/2.0" xmlns:swes="http://www.opengis.net/swes/2.0" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" service="SOS" version="2.0.0" xsi:schemaLocation="http://www.opengis.net/sos/2.0 http://schemas.opengis.net/sos/2.0/sos.xsd">
   <!-- optional sensor identifier, multiple values possible -->
   <sos:procedure>ENTPE-BH-KZ-CH1-QCfull</sos:procedure>
   <!-- optional offering identifier, multiple values possible -->
   <sos:offering>ENTPE-BH-KZ-CH1-QCfull</sos:offering>
   <!-- optional phenomena isentifier, multiple values possible -->
   <sos:observedProperty>Direct Horizontal Irradiance</sos:observedProperty>
   <!-- optional definition of a period in time, for which observation should be returned -->
   <sos:temporalFilter>
      <fes:During>
         <fes:ValueReference>phenomenonTime</fes:ValueReference>
         <gml:TimePeriod gml:id="tp_1">
            <gml:beginPosition>2014-11-19T14:00:00.000+01:00</gml:beginPosition>
            <gml:endPosition>2014-11-19T15:00:00.000+01:00</gml:endPosition>
         </gml:TimePeriod>
      </fes:During>
   </sos:temporalFilter>
   <!-- optional identifier of featureOfInterest, multiple values possible -->
   <sos:featureOfInterest>ENTPE</sos:featureOfInterest>
   <!-- optional definition of a spatial filter for which observations should be returned -->
   <sos:spatialFilter>
      <fes:BBOX>
         <fes:ValueReference>om:featureOfInterest/sams:SF_SpatialSamplingFeature/sams:shape</fes:ValueReference>
         <gml:Envelope srsName="http://www.opengis.net/def/crs/EPSG/0/4326">
            <gml:lowerCorner>0 0</gml:lowerCorner>
            <gml:upperCorner>60 60</gml:upperCorner>
         </gml:Envelope>
      </fes:BBOX>
   </sos:spatialFilter>
   <!-- optional definition of the response format; here O&amp;M 2.0 -->
   <sos:responseFormat>http://www.opengis.net/om/2.0</sos:responseFormat>
</sos:GetObservation>
~~~

As a result, the SOS responds with a `GetObservationResponse` document
comprising zero or multiple observationData entries, each storing an instance
of an observation (e.g. an O&M document, if the associated response format was
requested). The following listing provides an excerpt from the response to the
previously presented request.

~~~ xml
<?xml version="1.0" encoding="UTF-8"?>
<sos:GetObservationResponse xmlns:sos="http://www.opengis.net/sos/2.0" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/om/2.0 http://schemas.opengis.net/om/2.0/observation.xsd http://www.opengis.net/sos/2.0 http://schemas.opengis.net/sos/2.0/sos.xsd">
   <!-- exemplar entry -->
   <sos:observationData>
      <!-- observation with uniwue identifier -->
      <om:OM_Observation xmlns:om="http://www.opengis.net/om/2.0" xmlns:gml="http://www.opengis.net/gml/3.2" gml:id="o_129567565">
         <!-- observation type; here Measurement -->
         <om:type xlink:href="http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_Measurement" />
         <!-- period in time for which the mesaurment applies -->
         <om:phenomenonTime>
            <gml:TimePeriod gml:id="phenomenonTime_129567565">
               <gml:beginPosition>2014-11-19T13:39:00.000Z</gml:beginPosition>
               <gml:endPosition>2014-11-19T13:40:00.000Z</gml:endPosition>
            </gml:TimePeriod>
         </om:phenomenonTime>
         <!-- point in time when the measurement was observed -->
         <om:resultTime>
            <gml:TimeInstant gml:id="ti_9FC93B1C00320A5DC5A324BE1DBF4C6754E7F746">
               <gml:timePosition>2014-11-19T13:40:00.000Z</gml:timePosition>
            </gml:TimeInstant>
         </om:resultTime>
         <!-- reference to sensor/procedure using its identifier -->
         <om:procedure xlink:href="ENTPE-BH-KZ-CH1-QCfull" />
         <!-- reference to phenomena using its identifier -->
         <om:observedProperty xlink:href="Direct Horizontal Irradiance" />
         <!-- reference to featureOfInterest using its identifier -->
         <om:featureOfInterest xlink:href="ENTPE" xlink:title="ENTPE" />
         <!-- result of the observation -->
         <om:result xmlns:ns="http://www.opengis.net/gml/3.2" uom="W m-2" xsi:type="ns:MeasureType">119.0</om:result>
      </om:OM_Observation>
   </sos:observationData>
   <!-- other entries omitted for brevity -->
</sos:GetObservationResponse>
~~~

#### Operations of Enhanced Extension

The _Enhanced Extension_ of the SOS defines two additional optional operations,
as introduced subsequently. For each operation, its task as well as specified
request parameters are presented. The operations are:

##### GetFeatureOfInterest

This operation is used for requesting a set of features that are the target of
an observation, encoded in GML. As request parameters, the following can be used:

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “DescribeSensor”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| featureOfInterest| reference to a dedicated feature of interest; used to filter the returned feature set by feature of interest| no|
| observedProperty| reference to a dedicated observable property / phenomenon; used to filter the returned feature set by observable property| no|
| procedure| reference to a dedicated procedure; used to filter the returned feature set by procedure| no|
| spatialFilter| Used to filter features with regard to spatial properties| no|

##### GetObservationById

This operation is used for requesting a certain observation by its unique
identifier. As response, the queried instance of OM_Observation is returned,
encoded as O&M.

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “DescribeSensor”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| observation| reference to a dedicated observation identifier| yes|

#### Operations of Transactional Extension

The Transactional Extension of the SOS defines four additional optional
operations, as introduced subsequently. For each operation, its task as well
as specified request parameters are presented.

##### InsertSensor

This operation is used to insert a new sensor into the SOS. The available
request parameters are listed below. In response to a successful `InsertSensor`
request, the SOS responds with a pointer to the created sensor instance as well
as a pointer to the created offering associated to the inserted sensor/procedure.

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “DescribeSensor”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| procedureDescriptionFormat| reference to a known procedure description format, such as “http://www.opengis.net/sensorML/1.0.1” or “http://www.opengis.net/sensorML/2.0”| yes|
| procedureDescription| the procedure/sensor description using the format specified in parameter procedureDescriptionFormat, e.g. a valid SensorML document| yes|
| observedProperty| reference to a dedicated observable property / phenomenon that is measured by the inserted sensor/procedure;| yes|
| metadata/sosInsertionMetadata/observationType| Reference to an observation type which is produced by the sensor/procedure| yes|
| metadata/sosInsertionMetadata/featureOfInterestType| Reference to the type of the feature of interest which is observed by the sensor/procedure| yes|

##### UpdateSensorDescription

This operation is used for updating the description of an existing sensor.
The available request parameters are listed in the following. In response to a
successful `UpdateSensorDescription` request, the SOS responds with a pointer
to the created sensor instance as well as a pointer to the created offering
associated to the inserted sensor/procedure:

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “DescribeSensor”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| procedure| reference to a dedicated procedure, for which the description shall be updated| yes|
| procedureDescriptionFormat| reference to a known procedure description format, such as “http://www.opengis.net/sensorML/1.0.1” or “http://www.opengis.net/sensorML/2.0”| yes|
| description| contains the new sensor description according to the specified procedureDescriptionFormat within property data; optional property validTime might store the time instant or period for which the description is valid| yes|

##### DeleteSensor

This operation is used to delete an existing sensor from the SOS. The available
request parameters are listed below. In response, the SOS provides a pointer
to the deleted sensor instance. As a result, the associated observation offerings
shall no longer be listed within the SOS capabilities. Each SOS instance may
individually decide on further impacts of this request, e.g whether the procedure
and all of its observations shall be removed completely from the SOS, or whether
it is just prohibited to insert new data for that sensor.

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “DescribeSensor”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| procedure| | reference to a dedicated procedure| yes|

##### InsertObservation

This operation is used to insert new observations for registered sensors to
the SOS. The available request parameters are listed in the following. A successful
insertion results in an instance of `InsertObservationResponse`.

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “DescribeSensor”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| observation| the instance of OM_Observation that shall be inserted| yes|
| offering| reference to an existing offering to which the observation(s) shall be added | no|

#### Operations of Result Handling Extension

The Result Handling Extension of the SOS defines four additional optional
operations, as introduced subsequently. For each operation, its task as well
as specified request parameters are presented. The operations are:

##### InsertResultTemplate

This operation is used for inserting a result template into a SOS server
that describes the structure of the values of subsequently executed
`InsertResult` of `GetResult` requests. In addition it contains a complete
observation template comprising observation metadata such as procedure, observed
property and feature of interest related to the results. The available request
parameters are listed below. In response, the SOS provides a pointer to the
inserted result template instance.

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “DescribeSensor”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| proposedTemplate| A description of the result template to insert. It comprises of the following for mandatory properties. `offering`: a pointer to the observation offering, to which results will be added; `observationTemplate`: a template for instances of OM_Observation associated to the results; `resultStructure`: specifies the structure of the results; `resultEncoding`: specifies the encoding of the results| yes|

##### InsertResult

This operation is used for uploading raw results values accordingly to the
structure and encoding defined in the `InsertResultTemplate` request. This is
useful, if the remaining metadata elements of an observation are equal
(according to the specified _observationTemplate_ within the `InsertResultTemplate`
request). The available request parameters are listed in in the following. A successful
insertion results in an instance of `InsertResultResponse`.

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “DescribeSensor”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| template| reference to an existing resultTemplate, which defines structure and encoding of the result| yes|
| resultValues| the observation results stat shall be inserted| yes|

##### GetResultTemplate

This operation is used for getting the result structure and encoding for
specific parameter constellations. In particular this is a necessary preliminary
step to correctly interpret retrieved raw result values from a subsequent
`GetResult` request. The available request parameters are listed below. In response,
the SOS provides the structure and encoding of the queried result template within
the parameters _resultStructure_ and _resultEncoding_ of the associated
`GetResultTemplateResponse`.

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “DescribeSensor”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| offering| reference to a dedicated offering, for which results will be retrieved by subsequent `GetResult` requests; Implicitly, this also identifies the procedure/sensor, as one offering always refers to one procedure| yes|
| observedProperty| Reference to a dedicated observed property| yes|


##### GetResult

This operation is used for retrieving raw result data for specific parameter
constellations. In contrast to a GetObservation response, the returned document
only contains the raw result without the remaining observation metadata
components. Similar to the GetObservation operation, several filters can be
submitted using request parameters listed in the following table.

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “DescribeSensor”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| featureOfInterest| reference to a dedicated feature of interest; used to filter results by feature of interest| no|
| observedProperty| reference to a dedicated observable property / phenomenon; used to filter results by observable property| no|
| offering| reference to a dedicated offering which composes of results from a certain procedure and observable property| no|
| spatialFilter| Used to filter results with regard to spatial properties| no|
| temporalFilter| Used to filter results with regard to temporal properties| no|


### Sensor Planning Service
