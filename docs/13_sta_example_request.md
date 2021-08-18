---
title: 13. STA Example Requests
layout: page
---

## Workflow Overview

This section explains by an example how to create and read data in the __Sensor Things API__ (STA).
The example is a weather station at the 52°North GmbH office building which measures the air temperature
and the air pressure. The STA data model groupes the data in the following classes which are called
entities:

- **Thing**, a `Thing` is an object of the physical world or the information world that is capable of
being identified and integrated into communication networks _(here: weather station)_
- **Location**, the `Location` entity locates the `Thing` or the `Things` it associated with _(here:
52°North GmbH office building)_
- **HistoricalLocation**, a `Thing's` `HistoricalLocation` entity set provides the times of the current
and previous location of the `Thing`
- **Datastream**, a `Datastream` groups a collection of `Observations` meauring the same `Observedproperty`
and produced by the same `Sensor` _(here: collection of thermometer observation or barometer observations)_
- **Sensor**, a `Sensor` is an instrument that observes a property or phenomenon with the goal
of producing an estimate of the value of the property (here: thermometer and barometer)
- **ObservedProperty**, an `ObservedProperty` specifies the phenomenenon of an `Observation` _(here:
air temperature and air pressure)_
- **Observation**, an `Observation` is the act of measuring or otherwise determining the value
of a property _(here: observations by the thermometer and barometer)_
- **FeatureOfInterest**, an `Observation` results in a value being assigned to a phenomenon. The phenomenon
is a property of a feature, the latter being the `FeatureOfInterest` of the `Observation` _(here: city of Muenster)_

To create new data in the STA the HTTP POST request is used and to read the HTTP GET request. For more
information read the documentation:
[https://docs.ogc.org/is/18-088/18-088.html](https://docs.ogc.org/is/18-088/18-088.html)

## Create Thing

To insert data the first step is to create a `Thing` with a HTTP POST request. Against a local installation
of the SensorThings Webapp the request looks like this
[http://localhost:8080/52n-sensorthings-webapp/Things](http://localhost:8080/52n-sensorthings-webapp/Things).
The body of the request is a json document which is shown in the following:

~~~json
{
  "@iot.id": "weather_station_1234",
  "name": "Weather Station 52°North",
  "description": "This is a weather station at the 52°North GmbH office building.",
  "properties": {
    "owner": "52°North GmbH"
  }
}
~~~

This is the response of the succesful POST request:

~~~json
{
    "@iot.id": "weather_station_1234",
    "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Things(weather_station_1234)",
    "name": "Weather Station 52°North",
    "description": "This is a weather station at the 52°North GmbH office building.",
    "properties": {
        "owner": "52°North GmbH"
    },
    "Datastreams@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Things(weather_station_1234)/Datastreams",
    "Locations@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Things(weather_station_1234)/Locations",
    "HistoricalLocations@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Things(weather_station_1234)/HistoricalLocations"
}
~~~

## Create Location

## Create Sensor

## Create ObservedProperty

## Create Datastream

## Create FeatureOfInterest

## Create Observation

