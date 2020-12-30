# 13. Timezone lookup

Date: 2020-12-30

## Status

In progress

## Context

We need timezone lookup (by GEO coordinates) functionality for POI, to be able to determine if Place is open now.

## Decision

Use PostGIS with special timezone boundary table, which can be created from GEO shape DB:
`https://github.com/evansiroky/timezone-boundary-builder/releases`

### Local environment setup

No manual steps required.
Postgres DB will be started from `https://hub.docker.com/repository/docker/vky8411/postgis` with all required modules and data:
- PostGIS
- shp2pgsql: PostGIS converter (`shp` -> `sql`)
- Timezone SQL dump

### GCP environment setup

Use below steps to load GEO shape DB in Cloud Postgres:
1. Download and unzip the latest release from: `https://github.com/evansiroky/timezone-boundary-builder/releases`
2. Install `shp2pgsql` tool if doesn't exist with: `brew install postgis`
3. Convert it into SQL dump: `shp2pgsql -D combined-shapefile-with-oceans.shp public.timezones > timezones.sql`
4. Open Cloud SQL tunnel: `cloud_sql_proxy -instances=<project_id>:<region>:<db_instance>=tcp:5432`
5. Load SQL dump into Postgres DB: `PGPASSWORD=<password> psql -h localhost -p 5432 -U <user> -d <db_name> -f timezones.sql`

## Consequences

Probably in the future, we can create a separate micro-service with local GEO shape DB lookup, instead of using Postgres.
