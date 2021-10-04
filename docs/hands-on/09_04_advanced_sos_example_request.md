---
title: 9.4. Advanced SOS Example Requests
layout: page
---

## Workflow Overview

In the last section were examples given how to insert a new sensor into the SOS (`InsertSensor`),
how to insert a new observation for a registered sensor (`InsertObservation`), how to get metadata about the
SOS instance itself (`GetCapabilities`), how to get information about the available observations
(`GetDataAvailability`) and how to get observation data (`GetObservation`). In this section
the same operations are used, but this time the example is a complex observation with multiple results.

The workflow would be

* [InsertSensor](#insertsensor)
* [InsertObservation](#insertobservation)
* [GetCapabilities](#getcapabilities)
* [GetDataAvailability](#getdataavailability)
* [GetObservation](#getobservation)

### InsertSensor

Again an exemplar `InsertSensor` request against a local installation of the SOS ([http://localhost:8080/52n-sos-webapp/service](http://localhost:8080/52n-sos-webapp/service))
using POX binding is illustrated in the below example. This time the observation of the sensor is a complex type
(`<sos:observationType>`). The sensor in this example is a weather sensor measuring
the _air temperature_, providing a _wmo (world meteorological organization) weather code_, giving information if it _is freezing_, observing the
_wind direction_, measuring the _wind speed_ and commiting a _manuel observation_. The sensor is located
at the office of 52°North GmbH in the city of Muenster.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<swes:InsertSensor
    xmlns:swes="http://www.opengis.net/swes/2.0"
    xmlns:sos="http://www.opengis.net/sos/2.0"
    xmlns:swe="http://www.opengis.net/swe/2.0"
    xmlns:sml="http://www.opengis.net/sensorml/2.0"
    xmlns:gml="http://www.opengis.net/gml/3.2"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:gco="http://www.isotc211.org/2005/gco"
    xmlns:gmd="http://www.isotc211.org/2005/gmd" service="SOS" version="2.0.0" xsi:schemaLocation="http://www.opengis.net/sos/2.0 http://schemas.opengis.net/sos/2.0/sosInsertSensor.xsd  http://www.opengis.net/swes/2.0 http://schemas.opengis.net/swes/2.0/swes.xsd">
    <!-- reference to a known procedure description format such as “http://www.opengis.net/sensorML/1.0.1” or “http://www.opengis.net/sensorML/2.0” -->
    <swes:procedureDescriptionFormat>http://www.opengis.net/sensorml/2.0</swes:procedureDescriptionFormat>
    <!-- procedure/ sensor description using the format specified in parameter procedureDescriptionFormat, e.g. a valid SensorML document -->
    <swes:procedureDescription>
        <sml:PhysicalSystem gml:id="Weather_Sensor">
            <!-- unique identifier of the procedure/ sensor (used for references) -->
            <gml:identifier codeSpace="uniqueID">Weather_Sensor_1234</gml:identifier>
            <sml:identification>
                <sml:IdentifierList>
                    <!-- long and short description of the procedure/ sensor -->
                <sml:identifier>
                        <sml:Term definition="urn:ogc:def:identifier:OGC:1.0:longName">
                            <sml:label>longName</sml:label>
                            <sml:value>Weather Sensor 1234 at the 52°North GmbH office building</sml:value>
                        </sml:Term>
                    </sml:identifier>
                    <sml:identifier>
                        <sml:Term definition="urn:ogc:def:identifier:OGC:1.0:shortName">
                            <sml:label>shortName</sml:label>
                            <sml:value>Weather Sensor 1234</sml:value>
                        </sml:Term>
                    </sml:identifier>
                </sml:IdentifierList>
            </sml:identification>
            <!-- offering of the procedure/ sensor -->
            <sml:capabilities name="offerings">
                <sml:CapabilityList>
                    <!-- Special capabilities used to specify offerings. -->
                    <!-- Parsed and removed during InsertSensor/UpdateSensorDescription, added during DescribeSensor. -->
                    <!-- Offering is generated if not specified. -->
                    <sml:capability name="offeringID">
                        <swe:Text definition="urn:ogc:def:identifier:OGC:offeringID">
                            <!-- name of the offering -->
                            <swe:label>Weather Sensor 1234 Offering</swe:label>
                            <!-- unique identifier of the offering (used for references) -->
                            <swe:value>Weather_Sensor_1234_offering</swe:value>
                        </swe:Text>
                    </sml:capability>
                </sml:CapabilityList>
            </sml:capabilities>
            <sml:capabilities name="metadata">
                <sml:CapabilityList>
                    <!-- status indicates, whether sensor is insitu (true) or remote (false) -->
                    <sml:capability name="insitu">
                        <swe:Boolean definition="insitu">
                            <swe:value>true</swe:value>
                        </swe:Boolean>
                    </sml:capability>
                    <!-- status indicates, whether sensor is mobile (true) or fixed/stationary (false) -->
                    <sml:capability name="mobile">
                        <swe:Boolean definition="mobile">
                            <swe:value>false</swe:value>
                        </swe:Boolean>
                    </sml:capability>
                </sml:CapabilityList>
            </sml:capabilities>
            <!-- feature of interest of the procedure/ sensor -->
            <sml:featuresOfInterest>
                <sml:FeatureList definition="http://www.opengis.net/def/featureOfInterest/identifier">
                    <!-- name of the feature of interest -->
                    <swe:label>Muenster</swe:label>
                    <!-- unique identifier of the feature of interest (used for references) -->
                    <sml:feature xlink:href="Muenster"/>
                </sml:FeatureList>
            </sml:featuresOfInterest>
            <sml:inputs>
                <sml:InputList>
                    <!-- input of the procedure/ sensor (multiple inputs possible) -->
                    <sml:input name="weather">
                        <sml:ObservableProperty definition="weather"/>
                    </sml:input>
                </sml:InputList>
            </sml:inputs>
            <sml:outputs>
                <sml:OutputList>
                    <!-- output of the procedure/ sensor (multiple output possible) -->
                    <sml:output name="weather_observation">
                        <!-- this complex observation has multiple results -->
                        <swe:DataRecord>
                            <swe:field name="air_temperature">
                                <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P07/current/CFSN0023/">
                                    <swe:label>air_temperature</swe:label>
                                    <swe:uom code="degC"/>
                                </swe:Quantity>
                            </swe:field>
                            <swe:field name="wmo_weather_code">
                                <swe:Category definition="https://www.nodc.noaa.gov/archive/arc0021/0002199/1.1/data/0-data/HTML/WMO-CODE/WMO4677.HTM">
                                    <swe:codeSpace xlink:href="NOT_DEFINED"/>
                                </swe:Category>
                            </swe:field>
                            <swe:field name="isFreezing">
                                <swe:Boolean definition="isFreezing"/>
                            </swe:field>
                            <swe:field name="wind_direction">
                                <swe:Quantity definition="wind_direction">
                                    <swe:uom code="degree"/>
                                </swe:Quantity>
                            </swe:field>
                            <swe:field name="wind_speed">
                                <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P25/current/WINDS/">
                                    <swe:uom code="m/s"/>
                                </swe:Quantity>
                            </swe:field>
                            <swe:field name="manuel_observation">
                                <swe:Text definition="manuel_observation"/>
                            </swe:field>
                        </swe:DataRecord>
                    </sml:output>
                </sml:OutputList>
            </sml:outputs>
            <!-- position of the procedure/ sensor -->
            <sml:position>
                <swe:Vector referenceFrame="urn:ogc:def:crs:EPSG::4326">
                    <swe:coordinate name="easting">
                        <swe:Quantity axisID="x">
                            <swe:uom code="degree"/>
                            <swe:value>7.651968812254194</swe:value>
                        </swe:Quantity>
                    </swe:coordinate>
                    <swe:coordinate name="northing">
                        <swe:Quantity axisID="y">
                            <swe:uom code="degree"/>
                            <swe:value>51.935101100104916</swe:value>
                        </swe:Quantity>
                    </swe:coordinate>
                    <swe:coordinate name="altitude">
                        <swe:Quantity axisID="z">
                            <swe:uom code="m"/>
                            <swe:value>52.0</swe:value>
                        </swe:Quantity>
                    </swe:coordinate>
                </swe:Vector>
            </sml:position>
        </sml:PhysicalSystem>
    </swes:procedureDescription>
    <!-- reference to a dedicated observable property/ phenomenon that is measured by the inserted procedure/ sensor (multiple values possible) -->
    <swes:observableProperty>weather_observation</swes:observableProperty>
    <swes:metadata>
        <sos:SosInsertionMetadata>
            <!-- reference to an observation type which is produced by the procedure/ sensor (multiple values possible) -->
            <sos:observationType>http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_ComplexObservation</sos:observationType>
            <!-- reference to the type of the feature of interest which is observed by the procedure/ sensor -->
            <sos:featureOfInterestType>http://www.opengis.net/def/samplingFeatureType/OGC-OM/2.0/SF_SamplingPoint</sos:featureOfInterestType>
        </sos:SosInsertionMetadata>
    </swes:metadata>
</swes:InsertSensor>
```

> ####### Activity 1
>
> 1. Copy the above `InsertSensor` request (mark the request and CTRL + C)
> 1. Paste the request in the field of the `Request` section
>     * Click in the request field
>     * Mark the content (CTRL + A)
>     * Delete the content (del)
>     * Insert the copied request (CTRL + V)
> 1. Click the `Send` button

The response is similar to the example in the previous section. The SOS responds with a pointer
to the created sensor instance (`<swes:assignedProcedure>`) as well as a pointer to the created offering associated
to the inserted sensor/procedure (`<swes:assignedOffering>`). This references are needed for the next step to insert
an observation.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<swes:InsertSensorResponse xmlns:swes="http://www.opengis.net/swes/2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/swes/2.0 http://schemas.opengis.net/swes/2.0/swesInsertSensor.xsd">
  <!-- reference to the created procedure/ sensor instance -->
  <swes:assignedProcedure>Weather_Sensor_1234</swes:assignedProcedure>
  <!-- reference to the created offering associated to the inserted procedure/ sensor -->
  <swes:assignedOffering>Weather_Sensor_1234_offering</swes:assignedOffering>
</swes:InsertSensorResponse>
```

### InsertObservation

As previous this example of an `InsertObservation` request  uses the reference to the procedure/ sensor and
the reference to the offering of the `InsertSensor` response. In this example the observation has multiple values
which are the air temperature of 18.2°C, the wmo weather code 80, the information that it is not freezing, the wind
direction of 312 degree, the wind speed of 7.6 m/s and the manuel text "slight rain shower". The observation
in Muenster applies for 09:37:12 o'clock local time on the 6. August 2021

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sos:InsertObservation
    xmlns:sos="http://www.opengis.net/sos/2.0"
    xmlns:swes="http://www.opengis.net/swes/2.0"
    xmlns:swe="http://www.opengis.net/swe/2.0"
    xmlns:sml="http://www.opengis.net/sensorML/1.0.1"
    xmlns:gml="http://www.opengis.net/gml/3.2"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    xmlns:om="http://www.opengis.net/om/2.0"
    xmlns:sams="http://www.opengis.net/samplingSpatial/2.0"
    xmlns:sf="http://www.opengis.net/sampling/2.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" service="SOS" version="2.0.0" xsi:schemaLocation="http://www.opengis.net/sos/2.0 http://schemas.opengis.net/sos/2.0/sos.xsd          http://www.opengis.net/samplingSpatial/2.0 http://schemas.opengis.net/samplingSpatial/2.0/spatialSamplingFeature.xsd">
    <!-- reference to an offering (multiple offerings possible) -->
    <sos:offering>Weather_Sensor_1234_offering</sos:offering>
    <sos:observation>
        <om:OM_Observation gml:id="o1">
            <!-- reference to the type of the observation -->
            <om:type xlink:href="http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_ComplexObservation"/>
            <!-- period in time for which the observation applies -->
            <om:phenomenonTime>
                <gml:TimeInstant gml:id="phenomenonTime">
                    <gml:timePosition>2021-08-06T09:37:12.000+02:00</gml:timePosition>
                </gml:TimeInstant>
            </om:phenomenonTime>
            <!-- point in time when the observation was published (here: reference to phenomenonTime) -->
            <om:resultTime xlink:href="#phenomenonTime"/>
            <!-- reference to the procedure/ sensor -->
            <om:procedure xlink:href="Weather_Sensor_1234"/>
            <!-- reference to the observed property/ phenomenon -->
            <om:observedProperty xlink:href="weather_observation"/>
            <!-- feature of interest of the observation -->
            <om:featureOfInterest>
                <sams:SF_SpatialSamplingFeature gml:id="ssf_muenster">
                    <!-- unique identifier of the feature of interest -->
                    <gml:identifier codeSpace="">Muenster</gml:identifier>
                    <!-- name of the feature of interest -->
                    <gml:name>Muenster</gml:name>
                    <!-- reference to the geometry type of the feature of interest -->
                    <sf:type xlink:href="http://www.opengis.net/def/samplingFeatureType/OGC-OM/2.0/SF_SamplingPoint"/>
                    <!-- reference to the sampled feature (here: no sampled feature) -->
                    <sf:sampledFeature xlink:href="http://www.opengis.net/def/nil/OGC/0/unknown"/>
                    <!-- geometry of the feature of interest -->
                    <sams:shape>
                        <gml:Point gml:id="p_muenster">
                            <gml:pos srsName="http://www.opengis.net/def/crs/EPSG/0/4326">51.935101100104916 7.651968812254194</gml:pos>
                        </gml:Point>
                    </sams:shape>
                </sams:SF_SpatialSamplingFeature>
            </om:featureOfInterest>
            <!-- result of the observation -->
            <om:result xsi:type="swe:DataRecordPropertyType">
                <!-- this complex observation has multiple results -->
                <swe:DataRecord>
                    <swe:field name="air_temperature">
                        <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P07/current/CFSN0023/">
                            <swe:uom code="degC"/>
                            <swe:value>18.2</swe:value>
                        </swe:Quantity>
                    </swe:field>
                    <swe:field name="wmo_weather_code">
                        <swe:Category definition="https://www.nodc.noaa.gov/archive/arc0021/0002199/1.1/data/0-data/HTML/WMO-CODE/WMO4677.HTM">
                            <swe:codeSpace xlink:href="NOT_DEFINED"/>
                            <swe:value>80</swe:value>
                        </swe:Category>
                    </swe:field>
                    <swe:field name="isFreezing">
                        <swe:Boolean definition="isFreezing">
                            <swe:value>false</swe:value>
                        </swe:Boolean>
                    </swe:field>
                    <swe:field name="wind_direction">
                        <swe:Quantity definition="wind_direction">
                            <swe:uom code="degree"/>
                            <swe:value>312</swe:value>
                        </swe:Quantity>
                    </swe:field>
                    <swe:field name="wind_speed">
                        <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P25/current/WINDS/">
                            <swe:uom code="m/s"/>
                            <swe:value>7.6</swe:value>
                        </swe:Quantity>
                    </swe:field>
                    <swe:field name="manuel_observation">
                        <swe:Text definition="manuel_observation">
                            <swe:value>slight rain shower</swe:value>
                        </swe:Text>
                    </swe:field>
                </swe:DataRecord>
            </om:result>
        </om:OM_Observation>
    </sos:observation>
</sos:InsertObservation>
```

> ####### Activity 2
>
> 1. Copy the above `InsertObservation` request (mark the request and CTRL + C)
> 1. Paste the request in the field of the `Request` section
>     * Click in the request field
>     * Mark the content (CTRL + A)
>     * Delete the content (del)
>     * Insert the copied request (CTRL + V)
> 1. Click the `Send` button

A successful insertion results in an instance of an _insert observation response_ (`<sos:InsertObservationResponse>`).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sos:InsertObservationResponse xmlns:sos="http://www.opengis.net/sos/2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/sos/2.0 http://schemas.opengis.net/sos/2.0/sosInsertObservation.xsd"/>
```

### GetCapabilities

The `GetCapabilities` request is identical to the example in the last section.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sos:GetCapabilities
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:sos="http://www.opengis.net/sos/2.0"
    xmlns:ows="http://www.opengis.net/ows/1.1"
    xmlns:swe="http://www.opengis.net/swe/2.0" service="SOS" xsi:schemaLocation="http://www.opengis.net/sos/2.0 http://schemas.opengis.net/sos/2.0/sosGetCapabilities.xsd">
    <!-- submit the accepted version -->
    <ows:AcceptVersions>
        <ows:Version>2.0.0</ows:Version>
    </ows:AcceptVersions>
    <!-- response document includes this sections and omits all others -->
    <ows:Sections>
        <ows:Section>OperationsMetadata</ows:Section>
        <ows:Section>ServiceIdentification</ows:Section>
        <ows:Section>ServiceProvider</ows:Section>
        <ows:Section>FilterCapabilities</ows:Section>
        <ows:Section>Contents</ows:Section>
    </ows:Sections>
</sos:GetCapabilities>
```

> ####### Activity 3
>
> 1. Copy the above `GetCapabilities` request (mark the request and CTRL + C)
> 1. Paste the request in the field of the `Request` section
>     * Click in the request field
>     * Mark the content (CTRL + A)
>     * Delete the content (del)
>     * Insert the copied request (CTRL + V)
> 1. Click the `Send` button

Again the response document of the example request contains the most relevant sections. These are the
_service identification_ which provides metadata about the service itself (`<ows:ServiceIdentification>`),
the _service provider_ which offers metadata about the provider/ organization (`<ows:ServiceProvider>`),
the _operations metadata_ which contains metadata about the offered operations (`<ows:OperationsMetadata>`),
the _filter capabilities_ which includes metadata about the supported filter functionalities
(`<sos:filterCapabilities>`) and the _contents_ which contains metadata about available observation offerings
(`<sos:contents>`). The following response of the exemplar `GetCapabilities` request is not completely shown.
Some parts are omitted to enable better readability.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sos:Capabilities xmlns:sos="http://www.opengis.net/sos/2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:swes="http://www.opengis.net/swes/2.0" xmlns:gml="http://www.opengis.net/gml/3.2" version="2.0.0" updateSequence="2021-08-12T14:16:09.713+02:00" xsi:schemaLocation="http://www.opengis.net/sos/2.0 http://schemas.opengis.net/sos/2.0/sosGetCapabilities.xsd">
  <!-- metadata about the service itself, such as title, version, language -->
  <ows:ServiceIdentification>
    <ows:Title xml:lang="eng">52N SOS</ows:Title>
    <ows:Abstract xml:lang="eng">52North Sensor Observation Service - Data Access for the Sensor Web</ows:Abstract>
    <ows:ServiceType>OGC:SOS</ows:ServiceType>
    <ows:ServiceTypeVersion>1.0.0</ows:ServiceTypeVersion>
    <ows:ServiceTypeVersion>2.0.0</ows:ServiceTypeVersion>
    <ows:Profile>http://www.opengis.net/extension/SOSDO/1.0/observationDeletion</ows:Profile>
    <ows:Profile>http://www.opengis.net/extension/SOSDO/2.0/observationDeletion</ows:Profile>
    <!-- ... -->
    <ows:Fees>NONE</ows:Fees>
    <ows:AccessConstraints>NONE</ows:AccessConstraints>
  </ows:ServiceIdentification>
  <!-- metadata about the provider/ organization, such as contact information -->
  <ows:ServiceProvider>
    <ows:ProviderName>52North</ows:ProviderName>
    <ows:ProviderSite xlink:href="TBA" xlink:title="TBA"/>
    <ows:ServiceContact>
      <ows:IndividualName>TBA</ows:IndividualName>
      <ows:PositionName>TBA</ows:PositionName>
      <ows:ContactInfo>
        <ows:Phone>
          <ows:Voice>+49(0)251/396 371-0</ows:Voice>
          <ows:Facsimile>TBA</ows:Facsimile>
        </ows:Phone>
        <ows:Address>
          <ows:DeliveryPoint>Martin-Luther-King-Weg 24</ows:DeliveryPoint>
          <ows:City>Münster</ows:City>
          <ows:AdministrativeArea>North Rhine-Westphalia</ows:AdministrativeArea>
          <ows:PostalCode>48155</ows:PostalCode>
          <ows:Country>Germany</ows:Country>
          <ows:ElectronicMailAddress>info@52north.org</ows:ElectronicMailAddress>
        </ows:Address>
        <ows:OnlineResource xlink:href="http://52north.org/swe"/>
        <ows:HoursOfService>TBA</ows:HoursOfService>
        <ows:ContactInstructions>TBA</ows:ContactInstructions>
      </ows:ContactInfo>
      <ows:Role codeSpace="TBA">TBA</ows:Role>
    </ows:ServiceContact>
  </ows:ServiceProvider>
  <!-- metadata about the offered operations, including which operations are offered (like GetCapabilities, DescribeSensor, GetObservation, …), available bindings (e.g. POX, KVP, SOAP, JSON) and a URL endpoint -->
  <ows:OperationsMetadata>
    <!-- ... -->
    </ows:Operation>
    <ows:Operation name="DescribeSensor">
      <ows:DCP>
        <ows:HTTP>
          <ows:Get xlink:href="http://localhost:8080/52n-sos-webapp/service">
            <ows:Constraint name="Content-Type">
              <ows:AllowedValues>
                <ows:Value>application/x-kvp</ows:Value>
              </ows:AllowedValues>
            </ows:Constraint>
          </ows:Get>
          <ows:Post xlink:href="http://localhost:8080/52n-sos-webapp/service">
            <ows:Constraint name="Content-Type">
              <ows:AllowedValues>
                <ows:Value>application/exi</ows:Value>
                <ows:Value>application/json</ows:Value>
                <ows:Value>application/soap+xml</ows:Value>
                <ows:Value>application/xml</ows:Value>
                <ows:Value>text/xml</ows:Value>
              </ows:AllowedValues>
            </ows:Constraint>
          </ows:Post>
        </ows:HTTP>
      </ows:DCP>
      <ows:Parameter name="procedure">
        <ows:AllowedValues>
          <ows:Value>Weather_Sensor_1234</ows:Value>
        </ows:AllowedValues>
      </ows:Parameter>
      <ows:Parameter name="procedureDescriptionFormat">
        <ows:AllowedValues>
          <ows:Value>http://inspire.ec.europa.eu/schemas/ompr/3.0</ows:Value>
          <ows:Value>http://www.opengis.net/def/timeseries/observationProcess</ows:Value>
          <ows:Value>http://www.opengis.net/sensorML/1.0.1</ows:Value>
          <ows:Value>http://www.opengis.net/sensorml/2.0</ows:Value>
          <ows:Value>http://www.opengis.net/waterml/2.0/observationProcess</ows:Value>
        </ows:AllowedValues>
      </ows:Parameter>
      <ows:Parameter name="validTime">
        <ows:AnyValue/>
      </ows:Parameter>
    </ows:Operation>
    <ows:Operation name="GetCapabilities">
      <ows:DCP>
        <ows:HTTP>
          <ows:Get xlink:href="http://localhost:8080/52n-sos-webapp/service">
            <ows:Constraint name="Content-Type">
              <ows:AllowedValues>
                <ows:Value>application/x-kvp</ows:Value>
              </ows:AllowedValues>
            </ows:Constraint>
          </ows:Get>
          <ows:Post xlink:href="http://localhost:8080/52n-sos-webapp/service">
            <ows:Constraint name="Content-Type">
              <ows:AllowedValues>
                <ows:Value>application/exi</ows:Value>
                <ows:Value>application/json</ows:Value>
                <ows:Value>application/soap+xml</ows:Value>
                <ows:Value>application/xml</ows:Value>
                <ows:Value>text/xml</ows:Value>
              </ows:AllowedValues>
            </ows:Constraint>
          </ows:Post>
        </ows:HTTP>
      </ows:DCP>
      <ows:Parameter name="AcceptFormats">
        <ows:AllowedValues>
          <ows:Value>application/xml</ows:Value>
        </ows:AllowedValues>
      </ows:Parameter>
      <ows:Parameter name="AcceptVersions">
        <ows:AllowedValues>
          <ows:Value>1.0.0</ows:Value>
          <ows:Value>2.0.0</ows:Value>
        </ows:AllowedValues>
      </ows:Parameter>
      <ows:Parameter name="Sections">
        <ows:AllowedValues>
          <ows:Value>All</ows:Value>
          <ows:Value>Contents</ows:Value>
          <ows:Value>FilterCapabilities</ows:Value>
          <ows:Value>InsertionCapabilities</ows:Value>
          <ows:Value>OperationsMetadata</ows:Value>
          <ows:Value>ServiceIdentification</ows:Value>
          <ows:Value>ServiceProvider</ows:Value>
        </ows:AllowedValues>
      </ows:Parameter>
      <ows:Parameter name="updateSequence">
        <ows:NoValues/>
      </ows:Parameter>
    </ows:Operation>
    <!-- ... -->
  </ows:OperationsMetadata>
  <!-- metadata about the supported spatial and temporal filter functionalities extension: container for extensions -->
  <sos:filterCapabilities>
    <fes:Filter_Capabilities>
      <fes:Conformance>
        <fes:Constraint name="ImplementsAdHocQuery">
          <ows:NoValues/>
          <ows:DefaultValue>false</ows:DefaultValue>
        </fes:Constraint>
        <fes:Constraint name="ImplementsExtendedOperators">
          <ows:NoValues/>
          <ows:DefaultValue>false</ows:DefaultValue>
        </fes:Constraint>
        <!-- ... -->
      </fes:Conformance>
      <fes:Spatial_Capabilities>
        <fes:GeometryOperands>
          <fes:GeometryOperand xmlns:ns="http://www.opengis.net/gml/3.2" name="ns:Envelope"/>
        </fes:GeometryOperands>
        <fes:SpatialOperators>
          <fes:SpatialOperator name="BBOX">
            <fes:GeometryOperands>
              <fes:GeometryOperand xmlns:ns="http://www.opengis.net/gml/3.2" name="ns:Envelope"/>
            </fes:GeometryOperands>
          </fes:SpatialOperator>
        </fes:SpatialOperators>
      </fes:Spatial_Capabilities>
      <fes:Temporal_Capabilities>
        <fes:TemporalOperands>
          <fes:TemporalOperand xmlns:ns="http://www.opengis.net/gml/3.2" name="ns:TimeInstant"/>
          <fes:TemporalOperand xmlns:ns="http://www.opengis.net/gml/3.2" name="ns:TimePeriod"/>
        </fes:TemporalOperands>
        <fes:TemporalOperators>
          <fes:TemporalOperator name="Before">
            <fes:TemporalOperands>
              <fes:TemporalOperand xmlns:ns="http://www.opengis.net/gml/3.2" name="ns:TimeInstant"/>
              <fes:TemporalOperand xmlns:ns="http://www.opengis.net/gml/3.2" name="ns:TimePeriod"/>
            </fes:TemporalOperands>
          </fes:TemporalOperator>
          <fes:TemporalOperator name="After">
            <fes:TemporalOperands>
              <fes:TemporalOperand xmlns:ns="http://www.opengis.net/gml/3.2" name="ns:TimeInstant"/>
              <fes:TemporalOperand xmlns:ns="http://www.opengis.net/gml/3.2" name="ns:TimePeriod"/>
            </fes:TemporalOperands>
          </fes:TemporalOperator>
          <!-- ... -->
        </fes:TemporalOperators>
      </fes:Temporal_Capabilities>
    </fes:Filter_Capabilities>
  </sos:filterCapabilities>
  <!-- metadata about available observation offerings including their associated properties such as identifier, procedure, responseFormats, temporal and spatial aspects, ... -->
  <sos:contents>
    <sos:Contents>
      <swes:offering>
        <sos:ObservationOffering xmlns:ns="http://www.opengis.net/sos/2.0">
          <!-- unique identifier of the offering -->
          <swes:identifier>Weather_Sensor_1234_offering</swes:identifier>
          <!-- name of the offering -->
          <swes:name codeSpace="http://www.opengis.net/def/nil/OGC/0/unknown">Weather Sensor 1234 Offering</swes:name>
          <!-- unique identifier of the procedure/ sensor -->
          <swes:procedure>Weather_Sensor_1234</swes:procedure>
          <!-- available formats for the sensor descriptions -->
          <swes:procedureDescriptionFormat>http://inspire.ec.europa.eu/schemas/ompr/3.0</swes:procedureDescriptionFormat>
          <swes:procedureDescriptionFormat>http://www.opengis.net/def/timeseries/observationProcess</swes:procedureDescriptionFormat>
          <swes:procedureDescriptionFormat>http://www.opengis.net/sensorML/1.0.1</swes:procedureDescriptionFormat>
          <swes:procedureDescriptionFormat>http://www.opengis.net/sensorml/2.0</swes:procedureDescriptionFormat>
          <swes:procedureDescriptionFormat>http://www.opengis.net/waterml/2.0/observationProcess</swes:procedureDescriptionFormat>
          <!-- unique identifier of the observed property/ phenomena -->
          <swes:observableProperty>weather_observation</swes:observableProperty>
          <swes:relatedFeature>
            <swes:FeatureRelationship>
              <swes:target xlink:href="Muenster"/>
            </swes:FeatureRelationship>
          </swes:relatedFeature>
          <!-- bounding box of the observed area -->
          <sos:observedArea>
            <gml:Envelope srsName="http://www.opengis.net/def/crs/EPSG/0/4326">
              <gml:lowerCorner>51.935101100104916 7.651968812254194</gml:lowerCorner>
              <gml:upperCorner>51.935101100104916 7.651968812254194</gml:upperCorner>
            </gml:Envelope>
          </sos:observedArea>
          <!-- period in time for which the observations apply -->
          <sos:phenomenonTime>
            <gml:TimePeriod gml:id="phenomenonTime_1">
              <gml:beginPosition>2021-08-06T07:37:12.000Z</gml:beginPosition>
              <gml:endPosition>2021-08-06T07:37:12.000Z</gml:endPosition>
            </gml:TimePeriod>
          </sos:phenomenonTime>
          <!-- period in time when the observations were published -->
          <sos:resultTime>
            <gml:TimePeriod gml:id="resultTime_1">
              <gml:beginPosition>2021-08-06T07:37:12.000Z</gml:beginPosition>
              <gml:endPosition>2021-08-06T07:37:12.000Z</gml:endPosition>
            </gml:TimePeriod>
          </sos:resultTime>
          <!-- available response formats -->
          <sos:responseFormat>application/json</sos:responseFormat>
          <sos:responseFormat>application/netcdf</sos:responseFormat>
          <!-- ... -->
          <!-- reference to the observation type -->
          <sos:observationType>http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_ComplexObservation</sos:observationType>
          <!-- reference to the type of the associated feature of interest -->
          <sos:featureOfInterestType>http://www.opengis.net/def/samplingFeatureType/OGC-OM/2.0/SF_SamplingPoint</sos:featureOfInterestType>
        </sos:ObservationOffering>
      </swes:offering>
    </sos:Contents>
  </sos:contents>
</sos:Capabilities>
```

### GetDataAvailability

Compared to the example of the previous section only the identifiers changed. As before the following
`GetDataAvailability` request contains examples for each filter option.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<gda:GetDataAvailability
    xmlns:gda="http://www.opengis.net/sosgda/1.0"
    xmlns:swes="http://www.opengis.net/swes/2.0"
    xmlns:fes="http://www.opengis.net/fes/2.0"
    xmlns:gml="http://www.opengis.net/gml/3.2"
    xmlns:swe="http://www.opengis.net/swe/2.0" service="SOS" version="2.0.0">
    <!-- response document includes data of this procedure and omits all others (optional, multiple values possible) -->
    <gda:procedure>Weather_Sensor_1234</gda:procedure>
    <!-- response document includes data of this observed property and omits all others (optional, multiple values possible) -->
    <gda:observedProperty>weather_observation</gda:observedProperty>
    <!-- response document includes data of this feature of interest and omits all others (optional, multiple values possible) -->
    <gda:featureOfInterest>Muenster</gda:featureOfInterest>
    <!-- response document includes data of this offering and omits all others (optional, multiple values possible) -->
    <gda:offering>Weather_Sensor_1234_offering</gda:offering>
</gda:GetDataAvailability>
```

> ####### Activity 4
>
> 1. Copy the above `GetDataAvailability` request (mark the request and CTRL + C)
> 1. Paste the request in the field of the `Request` section
>     * Click in the request field
>     * Mark the content (CTRL + A)
>     * Delete the content (del)
>     * Insert the copied request (CTRL + V)
> 1. Click the `Send` button

The response document of the `GetDataAvailability` contains the _procedure/ sensor_, _observed property/ phenomenon_,
_feature of interest_, _phenomenon time_, _offering_ and _description formats_.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<gda:GetDataAvailabilityResponse xmlns:gda="http://www.opengis.net/sosgda/2.0" xmlns:gml="http://www.opengis.net/gml/3.2" xmlns:swe="http://www.opengis.net/swe/2.0" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/sosgda/2.0 http://waterml2.org/schemas/gda/2.0/gda.xsd">
  <gda:dataAvailabilityMember gml:id="dam_1">
    <!-- procedure/ sensor of the observation -->
    <gda:procedure xlink:href="Weather_Sensor_1234" xlink:title="Weather Sensor 1234"/>
    <!-- observed property/ phenomenon of the observation -->
    <gda:observedProperty xlink:href="weather_observation" xlink:title="weather_observation"/>
    <!-- feature of interest of the observation -->
    <gda:featureOfInterest xlink:href="Muenster" xlink:title="Muenster"/>
    <!-- period in time for which the observation applies -->
    <gda:phenomenonTime>
      <gml:TimePeriod gml:id="tp_1">
        <gml:beginPosition>2021-08-06T07:37:12.000Z</gml:beginPosition>
        <gml:endPosition>2021-08-06T07:37:12.000Z</gml:endPosition>
      </gml:TimePeriod>
    </gda:phenomenonTime>
    <!-- offering of the observation -->
    <gda:offering xlink:href="Weather_Sensor_1234_offering" xlink:title="Weather Sensor 1234 Offering"/>
    <!-- available description formats -->
    <gda:formatDescriptor>
      <gda:procedureDescriptionFormatDescriptor>
        <gda:procedureDescriptionFormat>http://www.opengis.net/sensorml/2.0</gda:procedureDescriptionFormat>
      </gda:procedureDescriptionFormatDescriptor>
      <gda:observationFormatDescriptor>
        <gda:responseFormat>http://www.opengis.net/om/2.0</gda:responseFormat>
        <gda:observationType>http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_ComplexObservation</gda:observationType>
      </gda:observationFormatDescriptor>
    </gda:formatDescriptor>
  </gda:dataAvailabilityMember>
</gda:GetDataAvailabilityResponse>
```

### GetObservation

Also the following `GetObservation` request contains an example for each filter option:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sos:GetObservation
    xmlns:sos="http://www.opengis.net/sos/2.0"
    xmlns:fes="http://www.opengis.net/fes/2.0"
    xmlns:gml="http://www.opengis.net/gml/3.2"
    xmlns:swe="http://www.opengis.net/swe/2.0"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    xmlns:swes="http://www.opengis.net/swes/2.0"
    xmlns:sosrf="http://www.opengis.net/sosrf/1.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" service="SOS" version="2.0.0" xsi:schemaLocation="http://www.opengis.net/sos/2.0 http://schemas.opengis.net/sos/2.0/sos.xsd">
    <!-- response document includes observation of this procedure and omits all others (optional, multiple values possible) -->
    <sos:procedure>Weather_Sensor_1234</sos:procedure>
    <!-- response document includes observation of this offering and omits all others (optional, multiple values possible) -->
    <sos:offering>Weather_Sensor_1234_offering</sos:offering>
    <!-- response document includes observation of this observed property and omits all others (optional, multiple values possible) -->
    <sos:observedProperty>weather_observation</sos:observedProperty>
    <!-- observations are filtered by time (optional) -->
    <sos:temporalFilter>
        <fes:During>
            <fes:ValueReference>phenomenonTime</fes:ValueReference>
            <gml:TimePeriod gml:id="tp_1">
                <gml:beginPosition>2021-08-06T09:00:00.000+02:00</gml:beginPosition>
                <gml:endPosition>2021-08-06T10:00:00.000+02:00</gml:endPosition>
            </gml:TimePeriod>
        </fes:During>
    </sos:temporalFilter>
    <!-- response document includes observation of this feature of interest and omits all others (optional, multiple values possible) -->
    <sos:featureOfInterest>Muenster</sos:featureOfInterest>
    <!-- observations are filtered by location (optional) -->
    <sos:spatialFilter>
        <fes:BBOX>
            <fes:ValueReference>om:featureOfInterest/sams:SF_SpatialSamplingFeature/sams:shape</fes:ValueReference>
            <gml:Envelope srsName="http://www.opengis.net/def/crs/EPSG/0/4326">
                <gml:lowerCorner>7.5 51.5</gml:lowerCorner>
                <gml:upperCorner>8.5 52.5</gml:upperCorner>
            </gml:Envelope>
        </fes:BBOX>
    </sos:spatialFilter>
    <!-- accepted response format (optional) -->
    <sos:responseFormat>http://www.opengis.net/om/2.0</sos:responseFormat>
</sos:GetObservation>
```

> ####### Activity 6
>
> 1. Copy the above `GetObservation` request (mark the request and CTRL + C)
> 1. Paste the request in the field of the `Request` section
>     * Click in the request field
>     * Mark the content (CTRL + A)
>     * Delete the content (del)
>     * Insert the copied request (CTRL + V)
> 1. Click the `Send` button

The response document to the `GetObservation` request can hold multiple observations. In this case it
only includes the observation which was inserted earlier, but this time the observation holds multiple values.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sos:GetObservationResponse xmlns:sos="http://www.opengis.net/sos/2.0" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:swes="http://www.opengis.net/swes/2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/om/2.0 http://schemas.opengis.net/om/2.0/observation.xsd http://www.opengis.net/sos/2.0 http://schemas.opengis.net/sos/2.0/sos.xsd">
  <sos:observationData>
    <om:OM_Observation xmlns:om="http://www.opengis.net/om/2.0" xmlns:gml="http://www.opengis.net/gml/3.2" gml:id="o_32">
      <!-- reference to the observation type -->
      <om:type xlink:href="http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_ComplexObservation"/>
      <!-- period in time for which the observation applies -->
      <om:phenomenonTime>
        <gml:TimeInstant xmlns:gml="http://www.opengis.net/gml/3.2" gml:id="phenomenonTime_32">
          <gml:timePosition>2021-08-06T07:37:12.000Z</gml:timePosition>
        </gml:TimeInstant>
      </om:phenomenonTime>
      <!-- point in time when the observation was published (here: reference to phenomenonTime) -->
      <om:resultTime xlink:href="#phenomenonTime_32"/>
      <!-- procedure/ sensor of the observation -->
      <om:procedure xlink:href="Weather_Sensor_1234" xlink:title="Weather Sensor 1234"/>
      <!-- observed property/ phenomenon of the observation -->
      <om:observedProperty xlink:href="weather_observation" xlink:title="weather_observation"/>
      <!-- feature of interest of the observation -->
      <om:featureOfInterest xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="Muenster" xlink:title="Muenster"/>
      <!-- result of the observation (here: multiple values) -->
      <om:result xmlns:ns="http://www.opengis.net/swe/2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xlink="http://www.w3.org/1999/xlink" xsi:type="ns:DataRecordPropertyType">
        <ns:DataRecord definition="weather_observation">
          <ns:identifier>weather_observation</ns:identifier>
          <ns:label>weather_observation</ns:label>
          <ns:field name="http___vocab.nerc.ac.uk_collection_P07_current_CFSN0023_">
            <ns:Quantity definition="http://vocab.nerc.ac.uk/collection/P07/current/CFSN0023/">
              <ns:identifier>http://vocab.nerc.ac.uk/collection/P07/current/CFSN0023/</ns:identifier>
              <ns:uom code="degC"/>
              <ns:value>18.2</ns:value>
            </ns:Quantity>
          </ns:field>
          <ns:field name="https___www.nodc.noaa.gov_archive_arc0021_0002199_1.1_data_0-data_HTML_WMO-CODE_WMO4677.HTM">
            <ns:Category definition="https://www.nodc.noaa.gov/archive/arc0021/0002199/1.1/data/0-data/HTML/WMO-CODE/WMO4677.HTM">
              <ns:identifier>https://www.nodc.noaa.gov/archive/arc0021/0002199/1.1/data/0-data/HTML/WMO-CODE/WMO4677.HTM</ns:identifier>
              <ns:codeSpace xlink:href="NOT_DEFINED"/>
              <ns:value>80</ns:value>
            </ns:Category>
          </ns:field>
          <ns:field name="isFreezing">
            <ns:Boolean definition="isFreezing">
              <ns:identifier>isFreezing</ns:identifier>
              <ns:value>false</ns:value>
            </ns:Boolean>
          </ns:field>
          <ns:field name="wind_direction">
            <ns:Quantity definition="wind_direction">
              <ns:identifier>wind_direction</ns:identifier>
              <ns:uom code="degree"/>
              <ns:value>312.0</ns:value>
            </ns:Quantity>
          </ns:field>
          <ns:field name="http___vocab.nerc.ac.uk_collection_P25_current_WINDS_">
            <ns:Quantity definition="http://vocab.nerc.ac.uk/collection/P25/current/WINDS/">
              <ns:identifier>http://vocab.nerc.ac.uk/collection/P25/current/WINDS/</ns:identifier>
              <ns:uom code="m/s"/>
              <ns:value>7.6</ns:value>
            </ns:Quantity>
          </ns:field>
          <ns:field name="manuel_observation">
            <ns:Text definition="manuel_observation">
              <ns:identifier>manuel_observation</ns:identifier>
              <ns:value>slight rain shower</ns:value>
            </ns:Text>
          </ns:field>
        </ns:DataRecord>
      </om:result>
    </om:OM_Observation>
  </sos:observationData>
</sos:GetObservationResponse>
```

### DescribeSensor

The `DescribeSensor` operation requests the sensor/procedure description usually encoded in SensorML
standard or any other suitable format.

| Parameter Name| Description| Mandatory|
| -----| -----| -----|
| service| fixed value “SOS”| no|
| request| fixed value “DescribeSensor”| yes|
| version| indicates the service version, e.g. “2.0.0”| yes|
| extension| specific extension, e.g. “language”| no|
| procedure | reference to a dedicated procedure| yes|
| procedureDescriptionFormat| Used to specify the desired response format, the default format for SOS 2.0 is `http://www.opengis.net/sensorML/1.0.1` | yes|
| validTime | Used to filter sensor description with regard to temporal properties| no|

The following `DescribeSensor` request contains an example for each filter option:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<swes:DescribeSensor service="SOS" version="2.0.0"
    xmlns:swes="http://www.opengis.net/swes/2.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:gml="http://www.opengis.net/gml/3.2" xsi:schemaLocation="http://www.opengis.net/swes/2.0 http://schemas.opengis.net/swes/2.0/swes.xsd">
    <swes:procedure>Weather_Sensor_1234</swes:procedure>
    <swes:procedureDescriptionFormat>http://www.opengis.net/sensorml/2.0</swes:procedureDescriptionFormat>
</swes:DescribeSensor>
```

> ####### Activity 6
>
> 1. Copy the above `DescribeSensor` request (mark the request and CTRL + C)
> 1. Paste the request in the field of the `Request` section
>     * Click in the request field
>     * Mark the content (CTRL + A)
>     * Delete the content (del)
>     * Insert the copied request (CTRL + V)
> 1. Click the `Send` button

The response document to the `DescribeSensor` request can hold multiple procedure description if `UpdateSensorDescription` for the requested sensor were performed and the are in the extent of the requested validTime filter. In this case it
only includes one procedure description.

```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <swes:DescribeSensorResponse xmlns:swes="http://www.opengis.net/swes/2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:gml="http://www.opengis.net/gml/3.2" xmlns:gmd="http://www.isotc211.org/2005/gmd" xmlns:gco="http://www.isotc211.org/2005/gco" xsi:schemaLocation="http://www.opengis.net/swes/2.0 http://schemas.opengis.net/swes/2.0/swesDescribeSensor.xsd">
      <swes:procedureDescriptionFormat>http://www.opengis.net/sensorml/2.0</swes:procedureDescriptionFormat>
      <swes:description>
        <swes:SensorDescription>
          <swes:validTime>
            <gml:TimePeriod gml:id="tp_079f9d07a638a7fa0804eb0be6b8082c9e8dc3c5d75b8ba785910f158593ff9c">
              <gml:beginPosition>2021-09-23T13:00:02.828Z</gml:beginPosition>
              <gml:endPosition indeterminatePosition="unknown"/>
            </gml:TimePeriod>
          </swes:validTime>
          <swes:data>
            <sml:PhysicalSystem xmlns:sml="http://www.opengis.net/sensorml/2.0" xmlns:swe="http://www.opengis.net/swe/2.0" xmlns:xlink="http://www.w3.org/1999/xlink" gml:id="Weather_Sensor">
              <!--unique identifier of the procedure/ sensor (used for references)-->
              <gml:identifier codeSpace="uniqueID">Weather_Sensor_1234</gml:identifier>
              <sml:keywords>
                <sml:KeywordList>
                  <sml:keyword>Weather_Sensor_1234_offering</sml:keyword>
                  <sml:keyword>Weather Sensor 1234 at the 52°North GmbH office building</sml:keyword>
                  <sml:keyword>weather_observation</sml:keyword>
                  <sml:keyword>Weather_Sensor_1234</sml:keyword>
                  <sml:keyword>Weather Sensor 1234</sml:keyword>
                </sml:KeywordList>
              </sml:keywords>
              <sml:identification>
                <sml:IdentifierList>
                  <sml:identifier>
                    <sml:Term definition="urn:ogc:def:identifier:OGC:1.0:longName">
                      <sml:label>longName</sml:label>
                      <sml:value>Weather Sensor 1234 at the 52°North GmbH office building</sml:value>
                    </sml:Term>
                  </sml:identifier>
                  <sml:identifier>
                    <sml:Term definition="urn:ogc:def:identifier:OGC:1.0:shortName">
                      <sml:label>shortName</sml:label>
                      <sml:value>Weather Sensor 1234</sml:value>
                    </sml:Term>
                  </sml:identifier>
                  <sml:identifier>
                    <sml:Term definition="urn:ogc:def:identifier:OGC:uniqueID">
                      <sml:label>uniqueID</sml:label>
                      <sml:value>Weather_Sensor_1234</sml:value>
                    </sml:Term>
                  </sml:identifier>
                </sml:IdentifierList>
              </sml:identification>
              <!--offering of the procedure/ sensor-->
              <!--feature of interest of the procedure/ sensor-->
              <sml:validTime>
                <gml:TimePeriod gml:id="tp_f41ee0624573d867c0288973770b796b7bf92f9105ed97abbb737152b010269a">
                  <gml:beginPosition>2021-09-23T13:00:02.828Z</gml:beginPosition>
                  <gml:endPosition indeterminatePosition="unknown"/>
                </gml:TimePeriod>
              </sml:validTime>
              <sml:capabilities name="metadata">
                <sml:CapabilityList>
                  <sml:capability name="insitu">
                    <swe:Boolean definition="insitu">
                      <swe:value>true</swe:value>
                    </swe:Boolean>
                  </sml:capability>
                  <sml:capability name="mobile">
                    <swe:Boolean definition="mobile">
                      <swe:value>false</swe:value>
                    </swe:Boolean>
                  </sml:capability>
                </sml:CapabilityList>
              </sml:capabilities>
              <sml:capabilities name="offerings">
                <sml:CapabilityList>
                  <sml:capability name="Weather_Sensor_1234_Offering">
                    <swe:Text definition="http://www.opengis.net/def/offering/identifier">
                      <swe:label>Weather Sensor 1234 Offering</swe:label>
                      <swe:value>Weather_Sensor_1234_offering</swe:value>
                    </swe:Text>
                  </sml:capability>
                </sml:CapabilityList>
              </sml:capabilities>
              <sml:contacts>
                <sml:ContactList>
                  <sml:contact>
                    <gmd:CI_ResponsibleParty>
                      <gmd:individualName>
                        <gco:CharacterString>TBA</gco:CharacterString>
                      </gmd:individualName>
                      <gmd:positionName>
                        <gco:CharacterString>TBA</gco:CharacterString>
                      </gmd:positionName>
                      <gmd:contactInfo>
                        <gmd:CI_Contact>
                          <gmd:phone>
                            <gmd:CI_Telephone>
                              <gmd:voice>
                                <gco:CharacterString>+49(0)251/396 371-0</gco:CharacterString>
                              </gmd:voice>
                              <gmd:facsimile>
                                <gco:CharacterString>TBA</gco:CharacterString>
                              </gmd:facsimile>
                            </gmd:CI_Telephone>
                          </gmd:phone>
                          <gmd:address>
                            <gmd:CI_Address>
                              <gmd:deliveryPoint>
                                <gco:CharacterString>Martin-Luther-King-Weg 24</gco:CharacterString>
                              </gmd:deliveryPoint>
                              <gmd:city>
                                <gco:CharacterString>Münster</gco:CharacterString>
                              </gmd:city>
                              <gmd:administrativeArea>
                                <gco:CharacterString>North Rhine-Westphalia</gco:CharacterString>
                              </gmd:administrativeArea>
                              <gmd:postalCode>
                                <gco:CharacterString>48155</gco:CharacterString>
                              </gmd:postalCode>
                              <gmd:country>
                                <gco:CharacterString>Germany</gco:CharacterString>
                              </gmd:country>
                              <gmd:electronicMailAddress>
                                <gco:CharacterString>info@52north.org</gco:CharacterString>
                              </gmd:electronicMailAddress>
                            </gmd:CI_Address>
                          </gmd:address>
                          <gmd:onlineResource xlink:href="http://52north.org/swe"/>
                          <gmd:hoursOfService>
                            <gco:CharacterString>TBA</gco:CharacterString>
                          </gmd:hoursOfService>
                          <gmd:contactInstructions>
                            <gco:CharacterString>TBA</gco:CharacterString>
                          </gmd:contactInstructions>
                        </gmd:CI_Contact>
                      </gmd:contactInfo>
                      <gmd:role>
                        <gmd:CI_RoleCode codeList="http://www.isotc211.org/2005/resources/Codelist/gmxCodelists.xml#CI_RoleCode/" codeListValue="CI_RoleCode_pointOfContact">Point of Contact</gmd:CI_RoleCode>
                      </gmd:role>
                    </gmd:CI_ResponsibleParty>
                  </sml:contact>
                </sml:ContactList>
              </sml:contacts>
              <sml:featuresOfInterest>
                <sml:FeatureList definition="http://www.opengis.net/def/featureOfInterest/identifier">
                  <!--name of the feature of interest-->
                  <swe:label>Muenster</swe:label>
                  <!--unique identifier of the feature of interest (used for references)-->
                  <sml:feature xlink:href="Muenster"/>
                </sml:FeatureList>
              </sml:featuresOfInterest>
              <sml:inputs>
                <sml:InputList>
                  <!--input of the procedure/ sensor (multiple inputs possible)-->
                  <sml:input name="weather">
                    <sml:ObservableProperty definition="weather"/>
                  </sml:input>
                </sml:InputList>
              </sml:inputs>
              <sml:outputs>
                <sml:OutputList>
                  <sml:output name="weather_observation">
                    <swe:DataRecord>
                      <swe:field name="air_temperature">
                        <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P07/current/CFSN0023/">
                          <swe:label>air_temperature</swe:label>
                          <swe:uom code="degC"/>
                        </swe:Quantity>
                      </swe:field>
                      <swe:field name="wmo_weather_code">
                        <swe:Category definition="https://www.nodc.noaa.gov/archive/arc0021/0002199/1.1/data/0-data/HTML/WMO-CODE/WMO4677.HTM">
                          <swe:codeSpace xlink:href="NOT_DEFINED"/>
                        </swe:Category>
                      </swe:field>
                      <swe:field name="isFreezing">
                        <swe:Boolean definition="isFreezing"/>
                      </swe:field>
                      <swe:field name="wind_direction">
                        <swe:Quantity definition="wind_direction">
                          <swe:uom code="degree"/>
                        </swe:Quantity>
                      </swe:field>
                      <swe:field name="wind_speed">
                        <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P25/current/WINDS/">
                          <swe:uom code="m/s"/>
                        </swe:Quantity>
                      </swe:field>
                      <swe:field name="manuel_observation">
                        <swe:Text definition="manuel_observation"/>
                      </swe:field>
                    </swe:DataRecord>
                  </sml:output>
                </sml:OutputList>
              </sml:outputs>
              <!--position of the procedure/ sensor-->
              <sml:position>
                <swe:Vector referenceFrame="urn:ogc:def:crs:EPSG::4326">
                  <swe:coordinate name="easting">
                    <swe:Quantity axisID="x">
                      <swe:uom code="degree"/>
                      <swe:value>7.651968812254194</swe:value>
                    </swe:Quantity>
                  </swe:coordinate>
                  <swe:coordinate name="northing">
                    <swe:Quantity axisID="y">
                      <swe:uom code="degree"/>
                      <swe:value>51.935101100104916</swe:value>
                    </swe:Quantity>
                  </swe:coordinate>
                  <swe:coordinate name="altitude">
                    <swe:Quantity axisID="z">
                      <swe:uom code="m"/>
                      <swe:value>52.0</swe:value>
                    </swe:Quantity>
                  </swe:coordinate>
                </swe:Vector>
              </sml:position>
            </sml:PhysicalSystem>
          </swes:data>
        </swes:SensorDescription>
      </swes:description>
    </swes:DescribeSensorResponse>
```

### Additional InsertObservations

Now we insert some additional observation.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sos:InsertObservation
    xmlns:sos="http://www.opengis.net/sos/2.0"
    xmlns:swes="http://www.opengis.net/swes/2.0"
    xmlns:swe="http://www.opengis.net/swe/2.0"
    xmlns:sml="http://www.opengis.net/sensorML/1.0.1"
    xmlns:gml="http://www.opengis.net/gml/3.2"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    xmlns:om="http://www.opengis.net/om/2.0"
    xmlns:sams="http://www.opengis.net/samplingSpatial/2.0"
    xmlns:sf="http://www.opengis.net/sampling/2.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" service="SOS" version="2.0.0" xsi:schemaLocation="http://www.opengis.net/sos/2.0 http://schemas.opengis.net/sos/2.0/sos.xsd          http://www.opengis.net/samplingSpatial/2.0 http://schemas.opengis.net/samplingSpatial/2.0/spatialSamplingFeature.xsd">
  <!-- reference to an offering (multiple offerings possible) -->
  <sos:offering>Weather_Sensor_1234_offering</sos:offering>
  <sos:observation>
    <om:OM_Observation gml:id="o2">
      <om:type xlink:href="http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_ComplexObservation"/>
      <om:phenomenonTime>
        <gml:TimeInstant gml:id="phenomenonTime">
          <gml:timePosition>2021-08-06T09:38:12.000+02:00</gml:timePosition>
        </gml:TimeInstant>
      </om:phenomenonTime>
      <om:resultTime xlink:href="#phenomenonTime"/>
      <om:procedure xlink:href="Weather_Sensor_1234"/>
      <om:observedProperty xlink:href="weather_observation"/>
      <om:featureOfInterest xlink:href="Muenster"/>
      <om:result xsi:type="swe:DataRecordPropertyType">
        <!-- this complex observation has multiple results -->
        <swe:DataRecord>
          <swe:field name="air_temperature">
            <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P07/current/CFSN0023/">
              <swe:uom code="degC"/>
              <swe:value>18.3</swe:value>
            </swe:Quantity>
          </swe:field>
          <swe:field name="wmo_weather_code">
            <swe:Category definition="https://www.nodc.noaa.gov/archive/arc0021/0002199/1.1/data/0-data/HTML/WMO-CODE/WMO4677.HTM">
              <swe:codeSpace xlink:href="NOT_DEFINED"/>
              <swe:value>81</swe:value>
            </swe:Category>
          </swe:field>
          <swe:field name="isFreezing">
            <swe:Boolean definition="isFreezing">
              <swe:value>false</swe:value>
            </swe:Boolean>
          </swe:field>
          <swe:field name="wind_direction">
            <swe:Quantity definition="wind_direction">
              <swe:uom code="degree"/>
              <swe:value>313</swe:value>
            </swe:Quantity>
          </swe:field>
          <swe:field name="wind_speed">
            <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P25/current/WINDS/">
              <swe:uom code="m/s"/>
              <swe:value>7.5</swe:value>
            </swe:Quantity>
          </swe:field>
          <swe:field name="manuel_observation">
            <swe:Text definition="manuel_observation">
              <swe:value>slight rain shower</swe:value>
            </swe:Text>
          </swe:field>
        </swe:DataRecord>
      </om:result>
    </om:OM_Observation>
  </sos:observation>
  <sos:observation>
    <om:OM_Observation gml:id="o3">
      <om:type xlink:href="http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_ComplexObservation"/>
      <om:phenomenonTime>
        <gml:TimeInstant gml:id="phenomenonTime2">
          <gml:timePosition>2021-08-06T09:39:12.000+02:00</gml:timePosition>
        </gml:TimeInstant>
      </om:phenomenonTime>
      <om:resultTime xlink:href="#phenomenonTime2"/>
      <om:procedure xlink:href="Weather_Sensor_1234"/>
      <om:observedProperty xlink:href="weather_observation"/>
      <om:featureOfInterest xlink:href="Muenster"/>
      <om:result xsi:type="swe:DataRecordPropertyType">
        <!-- this complex observation has multiple results -->
        <swe:DataRecord>
          <swe:field name="air_temperature">
            <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P07/current/CFSN0023/">
              <swe:uom code="degC"/>
              <swe:value>18.5</swe:value>
            </swe:Quantity>
          </swe:field>
          <swe:field name="wmo_weather_code">
            <swe:Category definition="https://www.nodc.noaa.gov/archive/arc0021/0002199/1.1/data/0-data/HTML/WMO-CODE/WMO4677.HTM">
              <swe:codeSpace xlink:href="NOT_DEFINED"/>
              <swe:value>80</swe:value>
            </swe:Category>
          </swe:field>
          <swe:field name="isFreezing">
            <swe:Boolean definition="isFreezing">
              <swe:value>false</swe:value>
            </swe:Boolean>
          </swe:field>
          <swe:field name="wind_direction">
            <swe:Quantity definition="wind_direction">
              <swe:uom code="degree"/>
              <swe:value>312</swe:value>
            </swe:Quantity>
          </swe:field>
          <swe:field name="wind_speed">
            <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P25/current/WINDS/">
              <swe:uom code="m/s"/>
              <swe:value>7.5</swe:value>
            </swe:Quantity>
          </swe:field>
          <swe:field name="manuel_observation">
            <swe:Text definition="manuel_observation">
              <swe:value>slight rain shower</swe:value>
            </swe:Text>
          </swe:field>
        </swe:DataRecord>
      </om:result>
    </om:OM_Observation>
  </sos:observation>
  <sos:observation>
    <om:OM_Observation gml:id="o4">
      <om:type xlink:href="http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_ComplexObservation"/>
      <om:phenomenonTime>
        <gml:TimeInstant gml:id="phenomenonTime3">
          <gml:timePosition>2021-08-06T09:40:12.000+02:00</gml:timePosition>
        </gml:TimeInstant>
      </om:phenomenonTime>
      <om:resultTime xlink:href="#phenomenonTime3"/>
      <om:procedure xlink:href="Weather_Sensor_1234"/>
      <om:observedProperty xlink:href="weather_observation"/>
      <om:featureOfInterest xlink:href="Muenster"/>
      <om:result xsi:type="swe:DataRecordPropertyType">
        <!-- this complex observation has multiple results -->
        <swe:DataRecord>
          <swe:field name="air_temperature">
            <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P07/current/CFSN0023/">
              <swe:uom code="degC"/>
              <swe:value>18.0</swe:value>
            </swe:Quantity>
          </swe:field>
          <swe:field name="wmo_weather_code">
            <swe:Category definition="https://www.nodc.noaa.gov/archive/arc0021/0002199/1.1/data/0-data/HTML/WMO-CODE/WMO4677.HTM">
              <swe:codeSpace xlink:href="NOT_DEFINED"/>
              <swe:value>79</swe:value>
            </swe:Category>
          </swe:field>
          <swe:field name="isFreezing">
            <swe:Boolean definition="isFreezing">
              <swe:value>false</swe:value>
            </swe:Boolean>
          </swe:field>
          <swe:field name="wind_direction">
            <swe:Quantity definition="wind_direction">
              <swe:uom code="degree"/>
              <swe:value>313</swe:value>
            </swe:Quantity>
          </swe:field>
          <swe:field name="wind_speed">
            <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P25/current/WINDS/">
              <swe:uom code="m/s"/>
              <swe:value>7.5</swe:value>
            </swe:Quantity>
          </swe:field>
          <swe:field name="manuel_observation">
            <swe:Text definition="manuel_observation">
              <swe:value>slight rain shower</swe:value>
            </swe:Text>
          </swe:field>
        </swe:DataRecord>
      </om:result>
    </om:OM_Observation>
  </sos:observation>
  <sos:observation>
    <om:OM_Observation gml:id="o5">
      <om:type xlink:href="http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_ComplexObservation"/>
      <om:phenomenonTime>
        <gml:TimeInstant gml:id="phenomenonTime4">
          <gml:timePosition>2021-08-06T09:41:12.000+02:00</gml:timePosition>
        </gml:TimeInstant>
      </om:phenomenonTime>
      <om:resultTime xlink:href="#phenomenonTime4"/>
      <om:procedure xlink:href="Weather_Sensor_1234"/>
      <om:observedProperty xlink:href="weather_observation"/>
      <om:featureOfInterest xlink:href="Muenster"/>
      <om:result xsi:type="swe:DataRecordPropertyType">
        <!-- this complex observation has multiple results -->
        <swe:DataRecord>
          <swe:field name="air_temperature">
            <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P07/current/CFSN0023/">
              <swe:uom code="degC"/>
              <swe:value>18.3</swe:value>
            </swe:Quantity>
          </swe:field>
          <swe:field name="wmo_weather_code">
            <swe:Category definition="https://www.nodc.noaa.gov/archive/arc0021/0002199/1.1/data/0-data/HTML/WMO-CODE/WMO4677.HTM">
              <swe:codeSpace xlink:href="NOT_DEFINED"/>
              <swe:value>81</swe:value>
            </swe:Category>
          </swe:field>
          <swe:field name="isFreezing">
            <swe:Boolean definition="isFreezing">
              <swe:value>false</swe:value>
            </swe:Boolean>
          </swe:field>
          <swe:field name="wind_direction">
            <swe:Quantity definition="wind_direction">
              <swe:uom code="degree"/>
              <swe:value>312</swe:value>
            </swe:Quantity>
          </swe:field>
          <swe:field name="wind_speed">
            <swe:Quantity definition="http://vocab.nerc.ac.uk/collection/P25/current/WINDS/">
              <swe:uom code="m/s"/>
              <swe:value>7.6</swe:value>
            </swe:Quantity>
          </swe:field>
          <swe:field name="manuel_observation">
            <swe:Text definition="manuel_observation">
              <swe:value>slight rain shower</swe:value>
            </swe:Text>
          </swe:field>
        </swe:DataRecord>
      </om:result>
    </om:OM_Observation>
  </sos:observation>
</sos:InsertObservation>
```

> ####### Activity 7
>
> 1. Copy the above `InsertObservation` request (mark the request and CTRL + C)
> 1. Paste the request in the field of the `Request` section
>     * Click in the request field
>     * Mark the content (CTRL + A)
>     * Delete the content (del)
>     * Insert the copied request (CTRL + V)
> 1. Click the `Send` button
