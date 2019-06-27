# Usecase "SaaS - API Key": Deploy a simple API on 3scale SaaS

In this usecase, a [Tekton pipeline](pipeline.yaml) will deploy an API described by an [OpenAPI Specification file](swagger.yaml) on a 3scale SaaS instance. The API is secured using API Keys as described in the OAS.

## Pre-requisites

Make sure you completed the [SETUP guide](../SETUP.md).

## Installation

Deploy the pipeline:

```sh
oc apply -f saas-usecase-apikey/pipeline.yaml
```

## Deployment

```sh
m4 -D__SAAS_DEVELOPER_ACCOUNT_ID__=$SAAS_DEVELOPER_ACCOUNT_ID < saas-usecase-apikey/env-saas.yaml | oc apply -f -
```
