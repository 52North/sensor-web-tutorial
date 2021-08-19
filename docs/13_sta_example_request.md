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

To insert data the first step is to create a `Thing` with a HTTP POST request. The URI uses two components
which are first the _service root URI_ and second the _resource path_. Against a local installation
of the STA the request looks like this:

> [http://localhost:8080/52n-sensorthings-webapp/Things](http://localhost:8080/52n-sensorthings-webapp/Things)

The body of the request is a json document which is shown in the following. It contains a unique _identifier_,
a _name_ and a _description_ for the `Thing` as well as the _properties_ of the `Thing`:

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

The response contains the parameters which were inserted by the request as well as a link to the `Thing` itself,
a link to the `Datastream`, a link to the `Location` and a link to the `HistoricalLocations`. For now the links
except the selflink do not lead to any data because the data is not yet created.

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

After that a `Location` needs to be added. In this example the URI for the POST request looks like this:

> [http://localhost:8080/52n-sensorthings-webapp/Locations](http://localhost:8080/52n-sensorthings-webapp/Locations)

The body of the request contains similiar to the request before the unique _identifier_, the _name_ and
_description_ of the `Location`. The _encoding Type_  holds the information how the location is encoded and
the _location_ describes where the linked `Thing` is located. The `Location` is linked to the `Thing` by
using the unique identifier of the `Thing`:

~~~json
{
  "@iot.id": "building_127",
  "name": "52°North",
  "description": "office building of the 52°North GmbH",
  "encodingType": "application/vnd.geo+json",
  "location": {
    "type": "Feature",
    "geometry":{
      "type": "Point",
      "coordinates": [7.651968812254194,51.935101100104916]
    }
  },
  "Things": [
      {
          "@iot.id": "weather_station_1234"
      }
  ]
}
~~~

The response shows the parameters which were send by the request and links to the `Location` itself,
the `Thing` and the `HistoricalLocations`. The link to the `Thing` leads to the weather station
which was added beforehand.

~~~json
{
    "@iot.id": "building_127",
    "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Locations(building_127)",
    "name": "52°North",
    "description": "office building of the 52°North GmbH",
    "encodingType": "application/vnd.geo+json",
    "location": {
        "type": "Point",
        "coordinates": [
            7.65196881,
            51.9351011
        ],
        "crs": {
            "type": "name",
            "properties": {
                "name": "EPSG:4326"
            }
        }
    },
    "Things@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Locations(building_127)/Things",
    "HistoricalLocations@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Locations(building_127)/HistoricalLocations"
}
~~~

## Create Sensor

In the next step a `Sensor` is created. In the following the URI for the POST request is shown:

> [http://localhost:8080/52n-sensorthings-webapp/Sensors](http://localhost:8080/52n-sensorthings-webapp/Sensors)

Again the body of the request contains the unique _identifier_, the _name_ and the _descripition_ of the entity.
The _encoding Type_ shows how the _metadata_ of the `Sensor` are encoded and the _metadata_ links information
about the `Sensor`:

~~~json
{
  "@iot.id": "thermometer_1",
  "name": "Thermometer",
  "description": "This thermometer measures the air temperatur",
  "encodingType": "application/pdf",
  "metadata": "http://example.org/Thermometer.pdf"
}
~~~

The response holds the parameters of the request, a link to the `Sensor` itself and a link to the `Datastream`
which is added later.

~~~json
{
    "@iot.id": "thermometer_1",
    "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Sensors(thermometer_1)",
    "name": "Thermometer",
    "description": "This thermometer measures the air temperatur",
    "encodingType": "application/pdf",
    "metadata": "http://example.org/Thermometer.pdf",
    "Datastreams@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Sensors(thermometer_1)/Datastreams"
}
~~~

To present later in the tutorial the filter opitions a futher `Sensor` is added:

~~~json
{
  "@iot.id": "barometer_2",
  "name": "Barometer",
  "description": "This barometer measures the air pressure",
  "encodingType": "application/pdf",
  "metadata": "http://example.org/Barometer.pdf"
}
~~~

## Create ObservedProperty

Next an `ObservedProperty` needs to be created. The URI for the POST request shown below:

> [http://localhost:8080/52n-sensorthings-webapp/ObservedProperties](http://localhost:8080/52n-sensorthings-webapp/ObservedProperties)

The body of the request contains a unique _identifier_, a _description_, a _name_ and a link to the _definition_
of the `ObservedProperty`:

~~~json
{
  "@iot.id": "air_temperature",
  "description": "Air temperature is the bulk temperature of the air, not the surface (skin) temperature.",
  "name": "Air Temperature",
  "definition": "http://vocab.nerc.ac.uk/collection/P07/current/CFSN0023/"
}
~~~

The parameters of the request are part of the response as well as a link to the `ObservedProperty` itself and
a link to the `Datastream` which is added in the next step:

~~~json
{
    "@iot.id": "air_temperature",
    "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/ObservedProperties(air_temperature)",
    "name": "Air Temperature",
    "description": "Air temperature is the bulk temperature of the air, not the surface (skin) temperature.",
    "definition": "http://vocab.nerc.ac.uk/collection/P07/current/CFSN0023/",
    "Datastreams@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/ObservedProperties(air_temperature)/Datastreams"
}
~~~

The second `Sensor` which was added beforehand observes a different phenomenon so a second `ObservedProperty`
needs to be created: 

~~~json
{
  "@iot.id": "air_pressure",
  "description": "Air pressure is the force per unit area which would be exerted when the moving gas molecules of which the air is composed strike a theoretical surface of any orientation.",
  "name": "Air Pressure",
  "definition": "http://vocab.nerc.ac.uk/collection/P07/current/CFSN0015/"
}
~~~

## Create Datastream

After that a `Datastream` is created. The URI for the POST request looks like this:

> [http://localhost:8080/52n-sensorthings-webapp/Datastreams](http://localhost:8080/52n-sensorthings-webapp/Datastreams)

The body of the `Datastream` contains as the other entities a unique _identifier_, a _name_ and a _description_.
Moreover it holds a _unit of measurement_ and an _observation Type_. A `Sensor`, an `ObservedProperty`
and a `Thing` are linked by there unique _idenitifier_:

~~~json
{
  "@iot.id": "thermometer_readings_102",
  "name": "Thermometer Readings",
  "description": "This is the reading of a thermometer.",
  "unitOfMeasurement": {
    "name": "degree Celsius",
    "symbol": "°C",
    "definition": "http://unitsofmeasure.org/ucum.html#para-30"
  },
  "observationType": "http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_Measurement",
  "Sensor": {
      "@iot.id": "thermometer_1"
  },
  "ObservedProperty": {
      "@iot.id": "air_temperature"
  },
  "Thing": {
      "@iot.id": "weather_station_1234"
  }
}
~~~

Again the response contains the parameters of the request as well as links to the `ObservedProperty`,
the `Observations` which are added last, the `Thing` and the `Sensor`:

~~~json
{
    "@iot.id": "thermometer_readings_102",
    "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Datastreams(thermometer_readings_102)",
    "name": "Thermometer Readings",
    "description": "This is the reading of a thermometer.",
    "observationType": "http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_Measurement",
    "unitOfMeasurement": {
        "name": "degree Celsius",
        "symbol": "°C",
        "definition": "http://unitsofmeasure.org/ucum.html#para-30"
    },
    "observedArea": null,
    "properties": {
        "categoryId": 1,
        "categoryName": "DEFAULT_STA_CATEGORY",
        "categoryDescription": "DEFAULT_STA_CATEGORY"
    },
    "ObservedProperty@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Datastreams(thermometer_readings_102)/ObservedProperty",
    "Observations@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Datastreams(thermometer_readings_102)/Observations",
    "Thing@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Datastreams(thermometer_readings_102)/Thing",
    "Sensor@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Datastreams(thermometer_readings_102)/Sensor"
}
~~~

For the second `Sensor` a second `Datastream` needs to be created.

~~~json
{
  "@iot.id": "barometer_readings_103",
  "name": "Barometer Readings",
  "description": "This is the reading of a barometer.",
  "unitOfMeasurement": {
    "name": "Pascal",
    "symbol": "Pa",
    "definition": "http://unitsofmeasure.org/ucum.html#para-30"
  },
  "observationType": "http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_Measurement",
  "Sensor": {
      "@iot.id": "barometer_2"
  },
  "ObservedProperty": {
      "@iot.id": "air_pressure"
  },
  "Thing": {
      "@iot.id": "weather_station_1234"
  }
}
~~~

## Create FeatureOfInterest

Next a `FeatureOfInterest` needs to be created. The URI for the POST request is shown in the following:

> [http://localhost:8080/52n-sensorthings-webapp/FeaturesOfInterest](http://localhost:8080/52n-sensorthings-webapp/FeaturesOfInterest)

The body of the `FeatureOfInterest` contains a unique _identifier_, a _name_, a _description_,
an _encoding Type_ for the _feature_ and a description of the _feature_:

~~~json
{
  "@iot.id": "muenster",
  "name": "Muenster",
  "description": "This is the city of Muenster.",
  "encodingType": "application/vnd.geo+json",
  "feature": {
    "type": "Feature",
    "geometry":{
      "type": "Point",
      "coordinates": [7.651968812254194,51.935101100104916]
    }
  }
}
~~~

The response holds the parameters of the request, a link to the `FeatureOfInterest` itself and a link
to the `Observations`:

~~~json
{
    "@iot.id": "muenster",
    "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/FeaturesOfInterest(muenster)",
    "name": "Muenster",
    "description": "This is the city of Muenster.",
    "encodingType": "application/vnd.geo+json",
    "feature": {
        "type": "Point",
        "coordinates": [
            7.65196881,
            51.9351011
        ],
        "crs": {
            "type": "name",
            "properties": {
                "name": "EPSG:4326"
            }
        }
    },
    "Observations@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/FeaturesOfInterest(muenster)/Observations"
}
~~~

## Create Observation

The `Observations` are created last. The URI of the POST request is presented below:

> [http://localhost:8080/52n-sensorthings-webapp/Observations](http://localhost:8080/52n-sensorthings-webapp/Observations)

The body of the request contains the _phenomenon time_ which is the time the observation was taken,
the _result time_ which is the time the observation was published, the _result_ of the observation.
The `Observation` is linked to the `Datastream` and `FeatureOfInterest` by there unique identifier:

~~~json
{
  "phenomenonTime": "2021-08-17T15:57:53.00+02:00",
  "resultTime": "2021-08-17T15:57:53.00+02:00",
  "result": 16.4,
  "Datastream": {
      "@iot.id": "thermometer_readings_102"
  },
  "FeatureOfInterest": {
      "@iot.id": "muenster"
  }
}
~~~

The response contains the parameters of the request and links to the `Observation` itself, the `Datastream`
and `FeatureOfInterest`:

~~~json
{
    "@iot.id": "656145a4-9124-4af2-ad99-2707e6fb4088",
    "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(656145a4-9124-4af2-ad99-2707e6fb4088)",
    "result": "16.4",
    "resultTime": "2021-08-17T13:57:53Z",
    "phenomenonTime": "2021-08-17T13:57:53.000Z",
    "Datastream@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(656145a4-9124-4af2-ad99-2707e6fb4088)/Datastream",
    "FeatureOfInterest@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(656145a4-9124-4af2-ad99-2707e6fb4088)/FeatureOfInterest"
}
~~~

To make it possible to filter the `Observations` more `Observations` are added:

~~~json
{
  "phenomenonTime": "2021-08-17T16:58:16.00+02:00",
  "resultTime": "2021-08-17T16:58:16.00+02:00",
  "result": 16.2,
  "Datastream": {
      "@iot.id": "thermometer_readings_102"
  },
  "FeatureOfInterest": {
      "@iot.id": "muenster"
  }
}
~~~

~~~json
{
  "phenomenonTime": "2021-08-17T17:58:08.00+02:00",
  "resultTime": "2021-08-17T17:58:08.00+02:00",
  "result": 15.9,
  "Datastream": {
      "@iot.id": "thermometer_readings_102"
  },
  "FeatureOfInterest": {
      "@iot.id": "muenster"
  }
}
~~~

~~~json
{
  "phenomenonTime": "2021-08-17T18:58:22.00+02:00",
  "resultTime": "2021-08-17T18:58:22.00+02:00",
  "result": 15.3,
  "Datastream": {
      "@iot.id": "thermometer_readings_102"
  },
  "FeatureOfInterest": {
      "@iot.id": "muenster"
  }
}
~~~

~~~json
{
  "phenomenonTime": "2021-08-17T19:58:13.00+02:00",
  "resultTime": "2021-08-17T19:58:13.00+02:00",
  "result": 14.8,
  "Datastream": {
      "@iot.id": "thermometer_readings_102"
  },
  "FeatureOfInterest": {
      "@iot.id": "muenster"
  }
}
~~~

Also for the second `Sensor` `Observations` are added:

~~~json
{
  "phenomenonTime": "2021-08-17T15:57:53.00+02:00",
  "resultTime": "2021-08-17T15:57:53.00+02:00",
  "result": 101580,
  "Datastream": {
      "@iot.id": "barometer_readings_103"
  },
  "FeatureOfInterest": {
      "@iot.id": "muenster"
  }
}
~~~

~~~json
{
  "phenomenonTime": "2021-08-17T16:58:16.00+02:00",
  "resultTime": "2021-08-17T16:58:16.00+02:00",
  "result": 101550,
  "Datastream": {
      "@iot.id": "barometer_readings_103"
  },
  "FeatureOfInterest": {
      "@iot.id": "muenster"
  }
}
~~~

~~~json
{
  "phenomenonTime": "2021-08-17T17:58:08.00+02:00",
  "resultTime": "2021-08-17T17:58:08.00+02:00",
  "result": 101590,
  "Datastream": {
      "@iot.id": "barometer_readings_103"
  },
  "FeatureOfInterest": {
      "@iot.id": "muenster"
  }
}
~~~

~~~json
{
  "phenomenonTime": "2021-08-17T18:58:22.00+02:00",
  "resultTime": "2021-08-17T18:58:22.00+02:00",
  "result": 101580,
  "Datastream": {
      "@iot.id": "barometer_readings_103"
  },
  "FeatureOfInterest": {
      "@iot.id": "muenster"
  }
}
~~~

~~~json
{
  "phenomenonTime": "2021-08-17T19:58:13.00+02:00",
  "resultTime": "2021-08-17T19:58:13.00+02:00",
  "result": 101540,
  "Datastream": {
      "@iot.id": "barometer_readings_103"
  },
  "FeatureOfInterest": {
      "@iot.id": "muenster"
  }
}
~~~