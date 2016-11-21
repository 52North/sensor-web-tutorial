---
title: 9. Hands-On
layout: page
---

## Hands-On Sensor Web Open Source Software!

In order to get the most easy jump-start on Sensor Web software, we have
developed a set of [Docker](https://www.docker.com/) containers. You will
require Docker running on your machine in order to use the following examples.
Instructions for all supported platform are available at:

> [https://www.docker.com/products/docker](https://www.docker.com/products/docker)


### 52°North Sensor Web Docker Containers

This section provides a brief overview on the available Docker images for
the Sensor Web, created and maintained by 52°North:

> [https://hub.docker.com/r/52north/](https://hub.docker.com/r/52north/)

[mdillon](https://hub.docker.com/r/mdillon/postgis/)
provides docker images of PostgreSQL with PostGIS extensions and templates installed.
We have based our example databases on these images.

#### SOS Setup

The **52north/sos-configured** docker image consists of a pre-configured
SOS instance running in a Tomcat container. It has the following settings:

* Database connection: PostgreSQL on `postgres:5432` with credentials `postgres:postgres`
(the DB is running in a DNS named (`postgres`) docker container)
* Administrator account with credentials `admin:admin`
* Transactional operations enabled

The **52north/sos** docker image is a simple instance of a non-configured SOS.
This image requires the installation procedure to be executed.

##### Using an Empty Database

For this example, we use the non-configured **52north/sos** docker image.
Run the following docker commands:

1. Start the postgres container:
`docker run -e "POSTGRES_DB=sos" --name sos-empty-postgres -p 5432:5432 mdillon/postgis:9.5`
1. Run the **sos** docker image:
`docker run --link sos-empty-postgres:postgres -p 8080:8080 52north/sos:4.3.8`

Alternatively, you can use the following **docker-compose.yml** file (run with
`docker-compose build && docker-compose up`):

```yml
version: '2'
services:
  postgres-db-empty:
    image: mdillon/postgis:9.5
    ports:
      - 5432:5432
    expose:
      - 5432
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=sos
  sos-service:
    image: 52north/sos:4.3.8
    ports:
      - 8080:8080
    links:
      - postgres-db-empty:postgres
```


Now, access [http://localhost:8080/52n-sos-webapp](http://localhost:8080/52n-sos-webapp).
The landing page will tell you to execute the installation procedure. Follow this
guided procedure. In particular, apply the following properties:

* as the _Datasource_ select `PostgreSQL/PostGIS`
* in the _Database Configuration_ section, make sure to set the
_Host_ to `postgres`

You will have your SOS running in a few seconds.

##### Using a Pre-filled Database

For this example, we use the pre-configured **52north/sos-configured** docker image.
A database with some sample data is available as the **52north/sos-weather-postgres**
docker image. Execute the following docker commands:

1. Start the postgres container:
`docker run --name sos-weather-postgres -p 5432:5432 52north/sos-weather-postgres:4.3.8`
1. Run the **sos-configured** docker image:
`docker run --link sos-weather-postgres:postgres -p 8080:8080 52north/sos-configured:4.3.8`


Alternatively, you can use the following **docker-compose.yml** file (run with
`docker-compose build && docker-compose up`):

```yml
version: '2'
services:
  sos-weather-postgres:
    image: 52north/sos-weather-postgres:4.3.8
    ports:
      - 5432:5432
    expose:
      - 5432
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=sos
  sos-service:
    image: 52north/sos-configured:4.3.8
    ports:
      - 8080:8080
    links:
      - sos-weather-postgres:postgres
```

The SOS is now up and running at [http://localhost:8080/52n-sos-webapp](http://localhost:8080/52n-sos-webapp).

#### REST API Setup

Both the **52north/sos** and the **52north/sos-configured** image come with the
Series REST API bundled in the webapp. You can access the API at:
[http://localhost:8080/52n-sos-webapp/api/v1/](http://localhost:8080/52n-sos-webapp/api/v1/).

#### Helgoland (JS Client) Setup

The images also contain the Helgoland JavaScript client. You can access the
client at:
[http://localhost:8080/52n-sos-webapp/static/client/helgoland/#/map](http://localhost:8080/52n-sos-webapp/static/client/helgoland/#/map)

#### More Example Data

##### SOS Solar Irradiance Example Database

This database contains in-situ data for Solar Irrandiance as well
as data derived by several models. The data is located at a station
in [Signes, France](https://www.google.de/maps/search/signes/@43.2760265,5.7793974,12z,),
and contains data for the first two weeks of September 2015.

The provided data is an excerpt of the database provided on
[http://webservice-energy.org/](http://webservice-energy.org/). This platform is
an effort carried out by the center
[Observation, Impacts, Energy](http://www.oie.mines-paristech.fr/Accueil/) (O.I.E.)
of [MINES ParisTech / ARMINES](http://www.mines-paristech.fr/) and also from the
[SoDa](http://www.soda-pro.com/) Team toward the Energy and Environmental Community.

###### Access

The database is available as a docker container:

> https://hub.docker.com/r/52north/sos-solar-postgres/

For usage, adjust the above docker commands (or `docker-compose.yml`) with the corresponding image:
`52north/sos-solar-postgres:4.3.8`.

###### Acknowledgement

The data contained in this sample database has been kindly provided by
[Solaïs](http://www.solais.fr/en/).

[![solais](images/solais.png)](http://www.solais.fr/en/)

Addtionally, we thank
[Mines ParisTech](http://www.mines-paristech.fr/)
(i.e. [Lionel Menard](http://www.mines-paristech.fr/Services/Annuaire/lionel-menard))
for supporting the collection of the sample data.

[![mines-paristech](images/mines-paristech.png)](http://www.mines-paristech.fr/)
