# 3. local run

Date: 2020-08-24

## Status

Accepted

## Context

How to run application locally for debug

## Decision

### Import service key

- Open *yenta-non-prod* GCP project
- Open *IAM & Admin/Service Accounts*
- Create new service key for the *local-run* service account (Action/Create key)
- Download key and rename it into *service-key.json*
- Create folder *secrets* on the same level with *services* folder (on the organization root level)

### Run application

- go to the services folder
- run *make dupb* (or *make dup* if containers are already built)

## Consequences

The current version writes images directly into the non-prod storage, so clear it after testing.

## TODO

Add terraform configuration and prepare & clean up steps for the local run.
