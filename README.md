# postgis [![Docker Automated buil](https://img.shields.io/docker/automated/openmaptiles/postgis.svg)]()

A custom PostgreSQL Docker image based off GEOS 3.5 and PostGIS 2.2.

## Usage

Run a PostgreSQL container and mount it to a persistent data directory outside.

In this example we start up the container and create a database `osm` with the owner `osm` and password `osm`
and mount our local directory `./data` as storage.

```bash
docker run \
    -v $(pwd)/data:/var/lib/postgresql/data \
    -e POSTGRES_DB="osm" \
    -e POSTGRES_USER="osm" \
    -e POSTGRES_PASSWORD="osm" \
    -d openmaptiles/postgis
```

## Build

```bash
docker build -t openmaptiles/postgis .
```