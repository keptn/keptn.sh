---
title: Install
description: Setup Prometheus monitoring
weight: 1
icon: setup
keywords: setup
---

In order to evaluate the quality gates and allow self-healing in production, we have to set up monitoring to get the needed data.

## Prerequisites

- Keptn project and at least one onboarded service must be available.

## Setup Prometheus

After creating a project and service, you can setup Prometheus monitoring and configure scrape jobs using the Keptn CLI. 

1. To install the *prometheus-service*, execute: 

    ```console
    kubectl apply -f https://raw.githubusercontent.com/keptn-contrib/prometheus-service/release-0.3.2/deploy/service.yaml
    ```

1. Execute the following command to set up the rules for the *Prometheus Alerting Manager*:

    ```
    keptn configure monitoring prometheus --project=PROJECTNAME --service=SERVICENAME
    ```

## Verify Prometheus setup in your cluster

* To verify that the Prometheus scrape jobs are correctly set up, you can access Prometheus by enabling port-forwarding for the prometheus-service:

    ```console
    kubectl port-forward svc/prometheus-service 8080 -n monitoring
    ```

Prometheus is then available on [localhost:8080/targets](http://localhost:8080/targets) where you can see the targets for the service:
{{< popup_image link="./assets/prometheus-targets.png" caption="Prometheus Targets">}}