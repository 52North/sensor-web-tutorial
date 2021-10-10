---
title: 6a. Introduction and Exercise: Database Modelling for the 52°North Sensor Web Server
layout: page
---

## Mapping your own database to the 52°North Sensor Web database model

As introduced before, two very typical approaches for serving your data with the 52°North Sensor Web Server comprise the creation of database views to emulate the 52°North data model or to use other external tools to copy your existing data into the 52°North Sensor Web database.
However, in order to provide you with the necessary knowledge to implement these workflows, it is important to understand how the data model of the 52°North Sensor Web Server is structures. Consequently, this exercise introduces first the core elements of the 52°North Sensor Web Data model.
Afterwards we will conduct an exercise in which you will write down, how you map the content of your existing data to the elements of the 52°North Sensor Web data model.
The following diagram outlines the general structure of the 52°North Sensor Web data model. Please note, there are some further tables in the data model which will not be discussed in this case. These tables are either not necessary for the normal operation of the SOS, or they only contain management information which does not require a conceptual mapping.

![DB_Model_Overview.png](images/DB_Model_Overview.png "Overview of the database model")

Before looking at the main contents of the tables, we first explain the meaning of these tables:
* Dataset: This is the central tables for organizing the database content. It organizes the observation data into datasets and defines common properties (e.g., observed parameter, unit of measurement, etc.) that are common to all observation belonging to a common dataset. Also, the Dataset table makes certain operations of the Sensor Web server more efficient, because it keeps track of the time stamp of the first and last value of a dataset.
* Observation: This tables contain the individual observations; mainly this includes the time stamps as well the values of the observation (the value that was measured).
* Unit: This table is used for storing the units of measurements used by the data sets.
* Category: In the 52°North Helgoland API it is possible to group data sets into Categories. These categories can be completely user-defined (e.g., organizations, projects, regions, thematical categories, etc.). This table is used for managing these categories (mainly mapping the category names to ids)
* Feature: The so-called Features are the geographical objects for which observations provide values. This can be cities, countries, measurement stations, etc.). Thus, this table contain mainly the names/ids of the Features as well as their geo-reference (e.g., coordinates).
* Offering: This table is specific to the SOS standard. It manages the groupings of observations which are called “Offerings”.
* Procedure: This table manages the procedures (e.g., sensors, human observers, models), which have generated the observations. Besides the names/ids of the procedures, this tables may contain comprehensive descriptions of the procedures (e.g., complete SensorML encoded sensor description documents)
* Phenomenon: This table manages the parameters which are observed by the sensors (the so-called observed properties)
* Platform: This table can be used to manage platforms on which sensors are mounted (e.g., research vessels, sensor stations, etc.)
   
Besides these core tables, there are the following further tables which are necessary for operating the OGC SensorThings API module:
* Location: This refers to the location of Things to which sensors are attached
* Historical location: This refers to past locations of Things. However, if sensors are not moved, this table can remain empty.
* Platform - Location: This table ensures that platforms are linked to their locations; as locations may change, this table is necessary so that also historical locations can be linked to the platform

After this general introduction, we will now have a closer look at the different tables. In order to ensure good readability, we will focus on the important fields which are critical when creating a mapping of your own data to the 52°North Sensor Web database.

### Dataset
![DB_Model_Dataset.png](images/DB_Model_Dataset.png "Overview of the Dataset table")

**Description**: Storage of the dataset, the core table of the whole database model.

| column | comment | NOT-NULL | default | SQL type |
| --- | --- | --- | --- | --- | --- |
| dataset_id | PK column of the table | true | - | int8 |
| dataset_type | Indicator whether the dataset provides individualObservation (individual observations), timeseries (timeseries obervations) or trajectories (trajectory observations). | true | 'not_initialized' | varchar(255) |
| observation_type | Indicator whether the dataset observations are of type simple (a simple observation, e.g. a scalar value like the temperature) or profile (profile observations) | true | 'not_initialized' | varchar(255) |
| value_type | Indicator of the type of the single values. Valid values are quantity (scalar values), count (integer values), text (textual values), category (categorical values), bool (boolean values), reference (references, e.g. link to a source, photo, video) | true | 'not_initialized' | varchar(255) |
| fk_procedure_id | Reference to the procedure that belongs that belongs to this dataset. | true | - | int8 |
| fk_phenomenon_id | Reference to the phenomenon that belongs that belongs to this dataset. | true | - | int8 |
| fk_offering_id | Reference to the offering that belongs that belongs to this dataset. | true | - | int8 |
| fk_category_id | Reference to the category that belongs that belongs to this dataset. | true | - | int8 |
| fk_feature_id | Reference to the feature that belongs that belongs to this dataset. | false | - | int8 |
| fk_platform_id | Reference to the platform that belongs that belongs to this dataset. | false | - | int8 |
| fk_unit_id | Reference to the unit of the observations that belongs to this dataset. | false | - | int8 | 
| is_deleted | Flag that indicates if this dataset is deleted | true | 0 | int2 |
| is_disabled | Flag that indicates if this dataset is disabled for insertion of new data | true | 0 | int2 || is_published | Flag that indicates if this dataset should be published | true | 1 | int2 |
| is_mobile | Flag that indicates if the procedure is mobile (1/true) or stationary (0/false). | false | 0 | int2 |
| is_insitu | Flag that indicates if the procedure is insitu (1/true) or remote (0/false). | false | 1 | int2 |
| is_hidden | Flag that indicates if this dataset should be hidden, e.g. for sub-datasets of a complex datasets | true | 0 | int2 |
| origin_timezone | Define the origin timezone of the dataset timestamps. Possible values are offset (+02:00), id (CET) or full name (Europe/Berlin). It no time zone is defined, UTC would be used as default. | false | - | varchar(40) |
| first_time | The timestamp of the temporally first observation that belongs to this dataset. | false | - | timestamp with time zone |
| last_time | The timestamp of the temporally last observation that belongs to this dataset. | false | - | timestamp with time zone |
| first_value | The value of the temporally first observation that belongs to this dataset. | false | - | numeric(20, 10) |
| last_value | The value of the temporally last quantity observation that belongs to this dataset. | false | - | numeric(20, 10) |
| fk_first_observation_id | Reference to the temporally first observation in the observation table that belongs to this dataset. | false | - | int8 |
| fk_last_observation_id | Reference to the temporally last observation in the observation table that belongs to this dataset. | false | - | int8 |
| decimals | Number of decimals that should be present in the output of the observation values. If no value is set, all decimals would be present. | false | - | int4 |
| identifier | Unique identifier of the dataset which can be used for filtering, e.g. GetObservationById in the SOS and can be encoded in WaterML 2.0 oder TimeseriesML 1.0 outputs. | false | - | varchar(255) |
| fk_identifier_codespace_id | The codespace of the dataset identifier, reference to the codespace table. Can be null. | false | - | int8 |
| name | The human readable name of the dataset. | false | - | varchar(255) |
| fk_name_codespace_id | The codespace of the dataset name, reference to the codespace table. Can be null. | false | - | int8 |
| description | A short description of the dataset | false | - | text | 

![DB_Model_Observation.png](images/DB_Model_Observation.png "Overview of the Observation table")
Observation

![DB_Model_Unit.png](images/DB_Model_Unit.png "Overview of the Unit table")
Unit

![DB_Model_Feature.png](images/DB_Model_Feature.png "Overview of the Feature table")
Feature

### Category
![DB_Model_Category.png](images/DB_Model_Category.png "Overview of the Category table")

**Description**: Storage of the categories which should be used to group the data.

| column | meaning | NOT-NULL | default | SQL type |
| --- | --- | --- | --- | --- |
| category_id | PK column of the table | true | - | int8 |
| identifier | Unique identifier of the category which can be used for filtering. Should be a URI, UUID. E.g. http://www.example.org/123, 123-321 | true | - | varchar(255) |
| name | The human readable name of the category. | false | - | varchar(255) |
| description | A short description of the category. | false | - | text |


![DB_Model_Offering.png](images/DB_Model_Offering.png "Overview of the Offering table")
Offering

![DB_Model_Procedure.png](images/DB_Model_Procedure.png "Overview of the Procedure table")
Procedure

![DB_Model_Phenomenon.png](images/DB_Model_Phenomenon.png "Overview of the Phenomenon table")
Phenomenon

![DB_Model_Platform.png](images/DB_Model_Platform.png "Overview of the Platform table")
Platform

![DB_Model_STA_Tables.png](images/DB_Model_STA_Tables.png "Overview of the SensorThings API-specic tables")
SensorThings API-specific Tables
