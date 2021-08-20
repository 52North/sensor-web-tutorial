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

## Create Data

### Create Thing

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

### Create Location

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

The response shows the parameters which were send by the request and the links to the `Location` itself,
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

### Create Sensor

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

To present later in the tutorial the filter opitions a second `Sensor` is added:

~~~json
{
  "@iot.id": "barometer_2",
  "name": "Barometer",
  "description": "This barometer measures the air pressure",
  "encodingType": "application/pdf",
  "metadata": "http://example.org/Barometer.pdf"
}
~~~

### Create ObservedProperty

Next an `ObservedProperty` needs to be created. The URI for the POST request is shown below:

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

### Create Datastream

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
the `Observations`, the `Thing` and the `Sensor`:

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

### Create FeatureOfInterest

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

### Create Observation

The `Observations` are created last. The URI of the POST request is presented below:

> [http://localhost:8080/52n-sensorthings-webapp/Observations](http://localhost:8080/52n-sensorthings-webapp/Observations)

The body of the request contains the _phenomenon time_ which is the time the observation was taken,
the _result time_ which is the time the observation was published and the _result_ of the observation.
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

## Read Data

### Read Sensor

The data in the STA can be read by sending a HTTP GET request. If a certain `Sensor` is requested
the URI consists out of the _service root URI_, the _resource path_ and the _identifier_. For the
inserted example data the URI looks like this:

> [http://localhost:8080/52n-sensorthings-webapp/Sensors(thermometer_1)](http://localhost:8080/52n-sensorthings-webapp/Sensors(thermometer_1))

The response is a json document. It contains the parameters of the `Sensor` and links to the `Sensor` itself
and the `Datastream`:

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

### Read Datastreams with a certain ObservedProperty

All `Datastreams` which are linked to a certain `ObservedProperty` can be read by using the URI for the
`ObservedProperty` and expanding it with the _ressource path_ of the linked `Datastream`. The request for
the example is shown below:

> [http://localhost:8080/52n-sensorthings-webapp/ObservedProperties(air_temperature)/Datastreams](http://localhost:8080/52n-sensorthings-webapp/ObservedProperties(air_temperature)/Datastreams)

The response document contains all `Datastreams` which are linked to the certain `ObservedProperty` (here:
one datastream "thermometere_readings_102"):

~~~json
{
    "@iot.count": 1,
    "value": [
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
            "phenomenonTime": "2021-08-17T13:57:53.000Z/2021-08-17T17:58:13.000Z",
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
    ]
}
~~~

### Read Observations of a certain Datastream

This is another example of requesting all entities which are linked to a different entity. This time all
`Observations` with a certain `Datastream` are requested:

> [http://localhost:8080/52n-sensorthings-webapp/Datastreams(thermometer_readings_102)/Observations](http://localhost:8080/52n-sensorthings-webapp/Datastreams(thermometer_readings_102)/Observations)

The response contains the five `Observations` which were created for the `Datastream`:

~~~json
{
    "@iot.count": 5,
    "value": [
        {
            "@iot.id": "45725aa8-6aab-4861-8b0c-80650e8ec5a0",
            "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(45725aa8-6aab-4861-8b0c-80650e8ec5a0)",
            "result": "15.3000000000",
            "resultTime": "2021-08-17T16:58:22Z",
            "phenomenonTime": "2021-08-17T16:58:22.000Z",
            "Datastream@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(45725aa8-6aab-4861-8b0c-80650e8ec5a0)/Datastream",
            "FeatureOfInterest@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(45725aa8-6aab-4861-8b0c-80650e8ec5a0)/FeatureOfInterest"
        },
        {
            "@iot.id": "656145a4-9124-4af2-ad99-2707e6fb4088",
            "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(656145a4-9124-4af2-ad99-2707e6fb4088)",
            "result": "16.4000000000",
            "resultTime": "2021-08-17T13:57:53Z",
            "phenomenonTime": "2021-08-17T13:57:53.000Z",
            "Datastream@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(656145a4-9124-4af2-ad99-2707e6fb4088)/Datastream",
            "FeatureOfInterest@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(656145a4-9124-4af2-ad99-2707e6fb4088)/FeatureOfInterest"
        },
        {
            "@iot.id": "cb2c0cc6-c9c8-4e47-a665-9029a0ff6ac4",
            "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cb2c0cc6-c9c8-4e47-a665-9029a0ff6ac4)",
            "result": "15.9000000000",
            "resultTime": "2021-08-17T15:58:08Z",
            "phenomenonTime": "2021-08-17T15:58:08.000Z",
            "Datastream@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cb2c0cc6-c9c8-4e47-a665-9029a0ff6ac4)/Datastream",
            "FeatureOfInterest@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cb2c0cc6-c9c8-4e47-a665-9029a0ff6ac4)/FeatureOfInterest"
        },
        {
            "@iot.id": "cf46d95a-6372-45d1-a201-619d386074c0",
            "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cf46d95a-6372-45d1-a201-619d386074c0)",
            "result": "16.2000000000",
            "resultTime": "2021-08-17T14:58:16Z",
            "phenomenonTime": "2021-08-17T14:58:16.000Z",
            "Datastream@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cf46d95a-6372-45d1-a201-619d386074c0)/Datastream",
            "FeatureOfInterest@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cf46d95a-6372-45d1-a201-619d386074c0)/FeatureOfInterest"
        },
        {
            "@iot.id": "fd612941-cb7e-4f0d-997f-13b684e178c0",
            "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(fd612941-cb7e-4f0d-997f-13b684e178c0)",
            "result": "14.8000000000",
            "resultTime": "2021-08-17T17:58:13Z",
            "phenomenonTime": "2021-08-17T17:58:13.000Z",
            "Datastream@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(fd612941-cb7e-4f0d-997f-13b684e178c0)/Datastream",
            "FeatureOfInterest@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(fd612941-cb7e-4f0d-997f-13b684e178c0)/FeatureOfInterest"
        }
    ]
}
~~~

### Read Observations filtered by resultTime

This example shows how to add a filter to the request. It is equal to the example before, but this time a
parameter is added to the GET request to filter the `Observations` by the result time:

> [http://localhost:8080/52n-sensorthings-webapp/Datastreams(thermometer_readings_102)/Observations?$filter=resultTime ge 2021-08-17T14:00:00Z and resultTime le 2021-08-17T16:00:00Z](http://localhost:8080/52n-sensorthings-webapp/Datastreams(thermometer_readings_102)/Observations?$filter=resultTime ge 2021-08-17T14:00:00Z and resultTime le 2021-08-17T16:00:00Z)

The response document only contains `Observations` which were published in the requested time period:

~~~json
{
    "@iot.count": 2,
    "value": [
        {
            "@iot.id": "cb2c0cc6-c9c8-4e47-a665-9029a0ff6ac4",
            "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cb2c0cc6-c9c8-4e47-a665-9029a0ff6ac4)",
            "result": "15.9000000000",
            "resultTime": "2021-08-17T15:58:08Z",
            "phenomenonTime": "2021-08-17T15:58:08.000Z",
            "Datastream@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cb2c0cc6-c9c8-4e47-a665-9029a0ff6ac4)/Datastream",
            "FeatureOfInterest@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cb2c0cc6-c9c8-4e47-a665-9029a0ff6ac4)/FeatureOfInterest"
        },
        {
            "@iot.id": "cf46d95a-6372-45d1-a201-619d386074c0",
            "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cf46d95a-6372-45d1-a201-619d386074c0)",
            "result": "16.2000000000",
            "resultTime": "2021-08-17T14:58:16Z",
            "phenomenonTime": "2021-08-17T14:58:16.000Z",
            "Datastream@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cf46d95a-6372-45d1-a201-619d386074c0)/Datastream",
            "FeatureOfInterest@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cf46d95a-6372-45d1-a201-619d386074c0)/FeatureOfInterest"
        }
    ]
}
~~~

### Read Datastreams with a certain FeatureOfInterest

In this example is shown how to request all `Datastreams` with the same `FeatureOfInterest`. This request is
more complicated because the `FeatureOfInterest` is only indirectly linked to the `Datastream` by the
`Observations`. For this request a filter needs to be used:

> [http://localhost:8080/52n-sensorthings-webapp/Datastreams?$filter=Observations/FeatureOfInterest/id eq 'muenster'](http://localhost:8080/52n-sensorthings-webapp/Datastreams?$filter=Observations/FeatureOfInterest/id eq 'muenster')

The response holds both `Datastreams` which were created because there `Observations` are linked to the same
`FeatureOfInterest`:

~~~json
{
    "@iot.count": 2,
    "value": [
        {
            "@iot.id": "barometer_readings_103",
            "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Datastreams(barometer_readings_103)",
            "name": "Barometer Readings",
            "description": "This is the reading of a barometer.",
            "observationType": "http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_Measurement",
            "unitOfMeasurement": {
                "name": "Pascal",
                "symbol": "Pa",
                "definition": "http://unitsofmeasure.org/ucum.html#para-30"
            },
            "observedArea": null,
            "phenomenonTime": "2021-08-17T13:57:53.000Z/2021-08-17T17:58:13.000Z",
            "properties": {
                "categoryId": 1,
                "categoryName": "DEFAULT_STA_CATEGORY",
                "categoryDescription": "DEFAULT_STA_CATEGORY"
            },
            "ObservedProperty@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Datastreams(barometer_readings_103)/ObservedProperty",
            "Observations@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Datastreams(barometer_readings_103)/Observations",
            "Thing@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Datastreams(barometer_readings_103)/Thing",
            "Sensor@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Datastreams(barometer_readings_103)/Sensor"
        },
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
            "phenomenonTime": "2021-08-17T13:57:53.000Z/2021-08-17T17:58:13.000Z",
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
    ]
}
~~~

### Read Observations with expanded FeatureOfInterest

In this example is shown how to structure the response document. In this request again all `Observations`
of a certain `Datastream` are requested. But this time parameters are added to the GET request to order
the observations ascending by the phenomenon time, to request the top two`Observations`, to skip
the first two `Observations` and to expand the linked `FeatureOfInterest`:

> [http://localhost:8080/52n-sensorthings-webapp/Datastreams(thermometer_readings_102)/Observations?$orderby=phenomenonTime asc&$top=2&$skip=2&$expand=FeatureOfInterest](http://localhost:8080/52n-sensorthings-webapp/Datastreams(thermometer_readings_102)/Observations?$orderby=phenomenonTime asc&$top=2&$skip=2&$expand=FeatureOfInterest)

The response document contains the requested `Observations`:

~~~json
{
    "@iot.count": 5,
    "@iot.nextLink": "http://localhost:8080/52n-sensorthings-webapp/Datastreams(thermometer_readings_102)/Observations?$top=2&$orderby=phenomenonTime asc&$expand=FeatureOfInterest($top=100)&$skip=4",
    "value": [
        {
            "@iot.id": "cb2c0cc6-c9c8-4e47-a665-9029a0ff6ac4",
            "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cb2c0cc6-c9c8-4e47-a665-9029a0ff6ac4)",
            "result": "15.9000000000",
            "resultTime": "2021-08-17T15:58:08Z",
            "phenomenonTime": "2021-08-17T15:58:08.000Z",
            "Datastream@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(cb2c0cc6-c9c8-4e47-a665-9029a0ff6ac4)/Datastream",
            "FeatureOfInterest": {
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
        },
        {
            "@iot.id": "45725aa8-6aab-4861-8b0c-80650e8ec5a0",
            "@iot.selfLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(45725aa8-6aab-4861-8b0c-80650e8ec5a0)",
            "result": "15.3000000000",
            "resultTime": "2021-08-17T16:58:22Z",
            "phenomenonTime": "2021-08-17T16:58:22.000Z",
            "Datastream@iot.navigationLink": "http://localhost:8080/52n-sensorthings-webapp/Observations(45725aa8-6aab-4861-8b0c-80650e8ec5a0)/Datastream",
            "FeatureOfInterest": {
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
        }
    ]
}
~~~
