# 17. Creating a new microservice.

Date: 2021-09-07

## Status

In progress

## Context

Creating a new microservice by steps.

## Step1
- Create new pkg in cmd/{{NAME}}
- Create main file & config file

## Step 2
- Create Dockerfile
- Update docker-compose.yaml & scripts/local_launch.sh
- Create k8s/{{NAME}}/deployment_template.yaml

## Step 3
- Update infrastructure/project-infrastructure/k8s_config_map.tf
- Update infrastructure/project-infrastructure/variables.tf

## Step 4
- Update .github/workflows/deploy_non_prod.yml
- Update .github/workflows/deploy_prod.yml
- Update .github/workflows/go_terraform.yml

## Step 5
- Update k8s/job/gcr_cleaner.yaml
