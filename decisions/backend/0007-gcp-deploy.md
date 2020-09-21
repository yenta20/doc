# 7. gcp deploy

Date: 2020-09-20

## Status

In progress

## Context

Useful console commands cheatsheet mostly related to the deployment

## Decision

### Initial steps

1. Create project on GCP
2. Install gcloud tools locally (https://cloud.google.com/sdk/docs/install)
3. Login into the GCP account from the console (https://cloud.google.com/sdk/gcloud/reference/auth/login)

- login into the account: ``` gcloud auth login```
- view local accounts (to be sure you are logged in correctly): ``` gcloud auth list ```

### Kubernetes

Check gcloud configurations list
```gcloud config configurations list```

Check the current kubernetes context
```kubectl config current-context```

Switch to the recon-nonprod cluster 
```gcloud container clusters get-credentials recon-nonprod```

But previous command would work only if region and zone are set
```
gcloud config set compute/region us-west2
gcloud config set compute/zone us-west2-a
```

#### Additional useful commands

Restart deployment after deploy
```kubectl rollout restart deployment/api-deployment```

Stop deployment and start it again
```
kubectl scale  --replicas=0 deployment/api-deployment
kubectl scale  --replicas=1 deployment/api-deployment
```

### Database

1. Create PostgreSQL instance on GCP
2. Create the database (recon)
3. Create the user

4. Install Cloud SQL Proxy locally (MacOS, full manual here https://cloud.google.com/sql/docs/mysql/authorize-proxy)
```
curl -o cloud_sql_proxy https://dl.google.com/cloudsql/cloud_sql_proxy.darwin.amd64
chmod +x cloud_sql_proxy
```

5. Enable the Cloud SQL Admin API 

6. Generate the instance connection name

Get instances list ```gcloud sql instances list --format="value(name)”``` (recon-nonprod)
Get connection name ```gcloud sql instances describe <instance-name> --format="value(connectionName)"```

7. Run Cloud SQL Proxy
```.\cloud_sql_proxy -instances=yenta-non-prod:us-west2:recon-nonprod=tcp:5432```

8. Create PostGIS extension (use IDE or concole to run SQL command)
``` CREATE EXTENSION postgis; ```


## Consequences

What becomes easier or more difficult to do and any risks introduced by the change that will need to be mitigated.
