---
title: Unbreakable Delivery Pipeline
description: Reviews and consolidates the concepts of a continuous delivery pipeline that prevents bad code changes from impacting end users.
weight: 35
keywords: [self-healing, quality gates]
aliases:
---

This use case reviews and consolidates the concepts of a continuous delivery pipeline that prevents bad code changes from impacting your end users.

## About this use case

The goal of the *Unbreakable Delivery Pipeline* is to implement a pipeline that prevents bad code changes from impacting your end users. This pipeline relies on three concepts known as Shift-Left, Shift-Right, and Self-Healing. More precisely,

* **Shift-Left** is the ability to pull data for specific entities (processes, services, or applications) through an automation API and feed it into the tools that are used to decide on whether to stop the pipeline or keep it running,

* **Shift-Right** is the ability to push deployment information and metadata to your monitoring solution (e.g., to differentiate BLUE vs GREEN deployments), to push build or revision numbers of a deployment, or to notify about configuration changes,

* **Self-Healing** is the ability for smart auto-remediation that addresses the root cause of a problem and not the symptom.

Exactly these three concepts have been applied in the use cases:

1. [Deployments with Quality Gates](../deployments-with-quality-gates/): 
    In this particular use case, the *carts* service has been changed, which intentionally slowed down the execution of the *addToCarts* function. After changing the service, it has been deployed to the development environment. Although the service has passed the quality gates (functional checks) in the development environment, the service has not passed the quality gate in the staging environment due to the increase of the response time detected by a performance test. This demonstrates an early break of the delivery pipeline based on automated quality gates. Hence, exploiting the concepts of **Shift-Left**, the delivery pipeline has been stopped and immediate feedback to the development team can be provided.

    Additionally, the used pipelines for deploying, testing, and evaluating have pushed information to our monitoring solution (Dynatrace in this case). This information can be used in following steps and provides the basis for **Shift-Right**. Summarizing, the pipeline stopped due to a - intentionally introduced - failure in a service. This was early enough to not deploy this faulty service version into production.

1. [Runbook Automation and Self-healing](../runbook-automation-and-self-healing/): 
    This use case has leveraged the power of runbook automation to build **Self-Healing** applications. Thus, a configuration change in the production environment caused troubles that were detected by Dynatrace, which then created a problem ticket. Due to this problem ticket and a corresponding notification, keptn became aware of it. Subsequently, keptn forwarded this problem to ServiceNow, which created an incident. This incident triggered a workflow that was able to remediate the issue at runtime and healed the application.