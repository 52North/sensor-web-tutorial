---
title: 3. SensorML
layout: page
---

## Sensor Model Language

Sensors in general can be classified into the following categories: _stationary_,
_mobile_, _in-situ_ and _remote_. Sensors can be installed at a fixed location
(_stationary_) or be mounted on a _mobile_ device to operate in arbitrary locations.
_In-situ_ and _remote_ indicate, whether the sensor performs the measurement using
local input from within its direct environment (_in-situ_) or computes
measurements for non-local data (_remote_). In addition, the process, how sensors
collect and/or compute data, may vary dramatically from use case to use case.
In consequence, a common generic way to describe sensor instances is necessary.
To satisfy these various requirements, the OGC developed the **Sensor Model Language
(SensorML)** standard, which can be retrieved as version 2.0 from the URL:

> [http://www.opengeospatial.org/standards/sensorml](http://www.opengeospatial.org/standards/sensorml)

Within the SWE framework,
SensorML plays an important role when exchanging metadata about sensors
themselves. It represents a uniform description format to describe relevant
sensor metadata such as calibration parameters, input/output as well as location
definitions, pre- or post-measurement actions/transformations or any other
information, which is necessary to correctly interpret and process observations
(OGC 2011). Within this scope, a sensor is denoted as a _process_, which may be a
physical or virtual component transforming dedicated input data to one or more
outputs via a certain transformation/method. For each degree of complexity,
SensorML provides means to describe the whole process metadata in an
interoperable way.

According to the standard version 2.0 (Botts & Robin, 2014), a process is
modeled as an instance of _AbstractProcess_ class, which denotes an abstract
generic set of properties applicable for various processes. It inherits several
metadata elements from the class _DescribedObject_, as shown in the below figure.
Note that the figure is simplified compared to the official standard:

![sml1.png](images/sml1.png "Abstract process model from the SensorML standard (Botts & Robin, 2014) - simplified figure")

From simple hardware devices that measure natural phenomena like temperature, up
to complex computational processes, which take several inputs and produce one or
more outputs, the SensorML standard offers generic metadata elements. As seen in
the above figure, several base metadata elements like _identification_, _keywords_,
_contacts_ or _validTime_ are inherited from _DescribedObject_. _AbstractProcess_
comprises additional process-related properties including sensor _configuration_
(settings/calibration), _inputs_ and _outputs_ as well as other parameters or modes.
In addition, references to the _featuresOfInterest_ can be stored. Instances of
_AbstractProcess_ (e.g. _SimpleProcess_) have to define a _method_ property, describing
the algorithm, how input data is transformed to output data, as modeled
according to the following figure.

![sml2.png](images/sml2.png "SimpleProcess and ProcessMethod modelling (Botts & Robin, 2014) - simplified figure")

While the presented _SimpleProcess_ is useful to model (virtual) computations,
physical sensors can be defined according to the below figure. Compared to a virtual
process, additional information about location and temporal aspects of the
sensor can be set. If the single process is only part of a composed process,
then this may be reflected in the _attachedTo_ property.

![sml3.png](images/sml3.png "Modelling of physical sensors (Botts & Robin, 2014) - simplified figure")

To illustrate this quite abstract description, a concrete example shall
demonstrate how to model sensor/procedure metadata within SensorML 2.0.
At the website

> [http://www.sensorml.com/sensorML-2.0/examples/index.html](http://www.sensorml.com/sensorML-2.0/examples/index.html)

various exemplar SensorML documents/snippets are available showcasing the
features of SensorML within various use cases. As a concrete example this
document uses an exemplar sensor description of the SOS instance within the
NeXOS project (NeXOS Project, 2016) available at

> [http://hspeed.trios.de:8888/52n-sos-webapp](http://hspeed.trios.de:8888/52n-sos-webapp).

Sending a DescribeSensor request including the parameters

`"procedure=http://www.nexosproject.eu/resources/procedures/trios/LISA/3075" and "sensorDescriptionFormat=http://www.opengis.net/sensorml/2.0"`

(e.g. using a [KVP request](http://hspeed.trios.de:8888/52n-sos-webapp/service?service=SOS&version=2.0.0&request=DescribeSensor&procedure=http://www.nexosproject.eu/resources/procedures/trios/LISA/3075&procedureDescriptionFormat=http://www.opengis.net/sensorml/2.0))
will result in the following sensor description of a water sensor measuring
the phenomenon "Specific Absorption Coefficient at 254nm":

~~~ xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- surrounding XML DescribeSensorResponse tag -->
<swes:DescribeSensorResponse xmlns:swes="http://www.opengis.net/swes/2.0" xmlns:gml="http://www.opengis.net/gml/3.2" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/swes/2.0 http://schemas.opengis.net/swes/2.0/swesDescribeSensor.xsd http://www.isotc211.org/2005/gco http://schemas.opengis.net/iso/19139/20070417/gco/gco.xsd http://www.opengis.net/gml/3.2 http://schemas.opengis.net/gml/3.2.1/gml.xsd http://www.isotc211.org/2005/gmd http://schemas.opengis.net/iso/19139/20070417/gmd/gmd.xsd http://www.opengis.net/sensorml/2.0 http://schemas.opengis.net/sensorML/2.0/sensorML.xsd">
   <!-- identifier of the description format; here SensorML 2.0 -->
   <swes:procedureDescriptionFormat>http://www.opengis.net/sensorml/2.0</swes:procedureDescriptionFormat>
   <swes:description>
      <swes:SensorDescription>
         <!-- period in time for which the document is valid -->
         <swes:validTime>
            <gml:TimePeriod gml:id="tp_2C8F45A1AE251D4DF0D4ECAD33255DDAED5A8B57">
               <gml:beginPosition>2015-10-15T11:08:43.869Z</gml:beginPosition>
               <gml:endPosition indeterminatePosition="unknown" />
            </gml:TimePeriod>
         </swes:validTime>
         <swes:data>
            <!-- start of embedded SensorML document characterizing a physical sensor component -->
            <sml:PhysicalComponent xmlns:sml="http://www.opengis.net/sensorml/2.0" xmlns:gco="http://www.isotc211.org/2005/gco" xmlns:gmd="http://www.isotc211.org/2005/gmd" xmlns:swe="http://www.opengis.net/swe/2.0" xmlns:xlink="http://www.w3.org/1999/xlink" gml:id="pc_E592E3CE0B79A08C646D06A21F4F94C569A4A46E" xsi:schemaLocation="http://www.opengis.net/sensorml/2.0 http://schemas.opengis.net/sensorML/2.0/sensorML.xsd http://www.opengis.net/swe/2.0 http://schemas.opengis.net/sweCommon/2.0/swe.xsd">
               <!-- unique identifier of the sensor -->
               <gml:identifier codeSpace="uniqueID">http://www.nexosproject.eu/resources/procedures/trios/LISA/3075</gml:identifier>
               <sml:keywords>
                  <sml:KeywordList>
                     <!-- list of characteristic keywords -->
                     <sml:keyword>Earth Science &gt; Oceans &gt; Sensors</sml:keyword>
                     <sml:keyword>Sensors</sml:keyword>
                     <sml:keyword>Projects:ESONET Platforms: Fixed Observation Stations</sml:keyword>
                  </sml:KeywordList>
               </sml:keywords>
               <sml:identification>
                  <sml:IdentifierList>
                     <!-- additional (optional) custom identifier(s) -->
                     <sml:identifier>
                        <sml:Term definition="http://www.nexosproject.eu/dictionary/definitions.html#modelID">
                           <sml:label>Model ID</sml:label>
                           <sml:value>LISA</sml:value>
                        </sml:Term>
                     </sml:identifier>
                     <!-- other identifiers omitted-->
                  </sml:IdentifierList>
               </sml:identification>
               <!-- period in time for which the sensor description is valid -->
               <sml:validTime>
                  <gml:TimePeriod gml:id="validityPeriod">
                     <gml:beginPosition>2015-01-01T00:00:00</gml:beginPosition>
                     <gml:endPosition>2039-12-31T00:00:00</gml:endPosition>
                  </gml:TimePeriod>
               </sml:validTime>
               <!-- characteristic custom sensor properties, each consisting of label and value -->
               <sml:characteristics name="LISACaracteristics">
                  <sml:CharacteristicList>
                     <sml:characteristic name="detectorType">
                        <swe:Text definition="http://www.nexosproject.eu/dictionary/definitions.html#detectorType">
                           <swe:label>Detector Type</swe:label>
                           <swe:value>silicon photodiode</swe:value>
                        </swe:Text>
                     </sml:characteristic>
                     <sml:characteristic name="lightSource">
                        <swe:Text definition="http://www.nexosproject.eu/dictionary/definitions.html#lightSource">
                           <swe:label>Light Source</swe:label>
                           <swe:value>254nm UV-LED and 530nm LED</swe:value>
                        </swe:Text>
                     </sml:characteristic>
                  </sml:CharacteristicList>
               </sml:characteristics>
               <!-- capabilities section -->
               <sml:capabilities name="offerings">
                  <sml:CapabilityList>
                     <sml:capability name="LISA-3075-data">
                        <swe:Text definition="http://www.opengis.net/def/offering/identifier">
                           <swe:label>LISA-3075-data</swe:label>
                           <swe:value>http://www.nexosproject.eu/resources/offerings/trios/LISA/3075/data</swe:value>
                        </swe:Text>
                     </sml:capability>
                     <sml:capability name="LISA-3075-housekeeping">
                        <swe:Text definition="http://www.opengis.net/def/offering/identifier">
                           <swe:label>LISA-3075-housekeeping</swe:label>
                           <swe:value>http://www.nexosproject.eu/resources/offerings/trios/LISA/3075/housekeeping</swe:value>
                        </swe:Text>
                     </sml:capability>
                  </sml:CapabilityList>
               </sml:capabilities>
               <!-- contact information about organization or individual contact persons -->
               <sml:contacts>
                  <sml:ContactList>
                     <sml:contact>
                        <gmd:CI_ResponsibleParty>
                           <gmd:individualName>
                              <gco:CharacterString>TBA</gco:CharacterString>
                           </gmd:individualName>
                           <gmd:organisationName>
                              <gco:CharacterString>52North</gco:CharacterString>
                           </gmd:organisationName>
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
                                 <gmd:onlineResource xlink:href="http://52north.org/swe" />
                              </gmd:CI_Contact>
                           </gmd:contactInfo>
                           <gmd:role>
                              <gmd:CI_RoleCode codeList="http://www.isotc211.org/2005/resources/Codelist/gmxCodelists.xml#CI_RoleCode/" codeListValue="CI_RoleCode_pointOfContact">Point of Contact</gmd:CI_RoleCode>
                           </gmd:role>
                        </gmd:CI_ResponsibleParty>
                     </sml:contact>
                  </sml:ContactList>
               </sml:contacts>
               <!-- external resource for documentation -->
               <sml:documentation>
                  <sml:DocumentList>
                     <sml:document>
                        <gmd:CI_OnlineResource>
                           <gmd:linkage>
                              <gmd:URL>http://www.trios.de/index.php?option=com_jdownloads&amp;Itemid=100&amp;view=finish&amp;cid=127&amp;catid=19&amp;lang=de</gmd:URL>
                           </gmd:linkage>
                           <gmd:description>
                              <gco:CharacterString>Web site with documentation</gco:CharacterString>
                           </gmd:description>
                        </gmd:CI_OnlineResource>
                     </sml:document>
                  </sml:DocumentList>
               </sml:documentation>
               <!-- information on historic sensor related events; here about installation and deployment -->
               <sml:history>
                  <sml:EventList>
                     <sml:event>
                        <sml:Event>
                           <swe:label>Installation</swe:label>
                           <sml:documentation>
                              <sml:DocumentList>
                                 <sml:document>
                                    <gmd:CI_OnlineResource>
                                       <gmd:linkage>
                                          <gmd:URL>http://trios.de/lisa3075_installation.log</gmd:URL>
                                       </gmd:linkage>
                                    </gmd:CI_OnlineResource>
                                 </sml:document>
                              </sml:DocumentList>
                           </sml:documentation>
                           <sml:time>
                              <gml:TimeInstant gml:id="installationDate">
                                 <gml:timePosition>2015-10-12T11:30:00.000Z</gml:timePosition>
                              </gml:TimeInstant>
                           </sml:time>
                        </sml:Event>
                     </sml:event>
                     <sml:event>
                        <sml:Event>
                           <swe:label>Deployment</swe:label>
                           <sml:documentation>
                              <sml:DocumentList>
                                 <sml:document>
                                    <gmd:CI_OnlineResource>
                                       <gmd:linkage>
                                          <gmd:URL>http://trios.de/lisa3075_installation.log</gmd:URL>
                                       </gmd:linkage>
                                    </gmd:CI_OnlineResource>
                                 </sml:document>
                              </sml:DocumentList>
                           </sml:documentation>
                           <sml:time>
                              <gml:TimeInstant gml:id="deploymentDate">
                                 <gml:timePosition>2015-10-12T12:02:00.000Z</gml:timePosition>
                              </gml:TimeInstant>
                           </sml:time>
                        </sml:Event>
                     </sml:event>
                  </sml:EventList>
               </sml:history>
               <!-- reference to featureOfInterest using the unique identifier -->
               <sml:featuresOfInterest>
                  <sml:FeatureList definition="http://www.opengis.net/def/featureOfInterest/identifier">
                     <swe:label>featuresOfInterest</swe:label>
                     <sml:feature xlink:href="http://www.nexosproject.eu/resources/features/trios/buoy/TriOS1/" />
                  </sml:FeatureList>
               </sml:featuresOfInterest>
               <!-- definition of process inputs as observableProperty; here indicating phenomenon water -->
               <sml:inputs>
                  <sml:InputList>
                     <sml:input name="water">
                        <sml:ObservableProperty definition="http://www.nexosproject.eu/dictionary/definitions.html#water" />
                     </sml:input>
                  </sml:InputList>
               </sml:inputs>
               <!-- definition of process outputs; here indicating a numeric quantity value -->
               <sml:outputs>
                  <sml:OutputList>
                     <sml:output name="SAC254">
                        <swe:Quantity definition="http://www.nexosproject.eu/dictionary/definitions.html#SAC254">
                           <swe:label>Specific Absorption Coefficient at 254nm</swe:label>
                           <swe:uom code="1/m" />
                        </swe:Quantity>
                     </sml:output>
                  </sml:OutputList>
               </sml:outputs>
               <!-- definition of sensor position comprising coordinate refrence system and coordinates  -->
               <sml:position>
                  <swe:DataRecord>
                     <swe:field name="longitude">
                        <swe:Quantity referenceFrame="urn:ogc:crs:EPSG:4326" axisID="Lon">
                           <swe:uom code="deg" />
                           <swe:value>8.163906</swe:value>
                        </swe:Quantity>
                     </swe:field>
                     <swe:field name="latitude">
                        <swe:Quantity referenceFrame="urn:ogc:crs:EPSG:4326" axisID="Lat">
                           <swe:uom code="deg" />
                           <swe:value>53.243653</swe:value>
                        </swe:Quantity>
                     </swe:field>
                     <swe:field name="altitude">
                        <swe:Quantity referenceFrame="urn:ogc:crs:EPSG:5715" axisID="Z">
                           <swe:uom code="m" />
                           <swe:value>12.452235</swe:value>
                        </swe:Quantity>
                     </swe:field>
                  </swe:DataRecord>
               </sml:position>
            </sml:PhysicalComponent>
         </swes:data>
      </swes:SensorDescription>
   </swes:description>
</swes:DescribeSensorResponse>
~~~

The description starts with some general metadata
such as an informal characteristic keywords and important identification.
The latter is relevant to uniquely identify this specific sensor by its
formal and unique ID (`<gml:identifier codeSpace="uniqueID">`) but may
comprise additional informal short or long identifiers. An important
information is stored within the parameter `validTime`. It indicates the
point/period in time for which the document is valid. For instance, a
new parametrization of the procedure might require a new version of the
sensor description. In this case, the presented SensorML 2.0 document is
valid from `2015-01-01T00:00:00` until `2039-12-31T00:00:00`. Within a
`characteristics` section, informal characteristics of the sensor are
provided. In this case, specifications of the `detectorType` and the
`lightSource` are given. The subsequent `capabilities` section comprises
references to related `offerings`. Any contact information about the
occupying organization can be provided by the `contacts` section. In the
section `documentation` links to external documents containing sensor
information are included. The same applies to the section `history`, which
comprises hints on historic events such as _installation_ or _deployment_
details.

As another important aspect of a sensor description the `featureOfInterest`
section stores a reference to the feature that is observed by the sensor.
The remaining three sections of the exemplary document are vital aspects
of a sensor description. First, the in- and outputs of the procedure are
specified within their corresponding sections. Note that this is a quite
simple example of a sensor description specifying one input and one output.
More complex sensor constellations might specify multiple in- and outputs
as well as other more complex sensor settings, especially, if a certain
procedure is only a part of a sensor system. In that case, the dependencies
between each sensor component have to be described in addition. Finally,
the geolocation is specified within the position tag specifying the
coordinate reference system as well as the geometry.
