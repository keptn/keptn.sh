---
title: Keptn Bridge
description: Explains how the access the Keptn Bridge.
weight: 21
keywords: [bridge]
---

The Keptn Bridge is the user interface of Keptn and presents all projects and services managed by Keptn. It is automatically installed with your Keptn deployment.

## Expose/Lockdown Bridge

The Keptn Bridge is not publicly accessible by default.

* To expose the Keptn Bridge, execute the following command. It is then available on: `https://bridge.keptn.YOUR.DOMAIN/`

```console
keptn configure bridge --action=expose
```

**Note:** This command shows the warning `Warning: Make sure to enable basic authentication as described here: ...`. Please follow the warning and enable basic authentication explained [below](./#enable-authentication).

* To lockdown the Keptn Bridge:

```console
keptn configure bridge --action=lockdown
```

## Configure Basic Authentication

The Keptn Bridge has a basic authentication feature, which can be controlled by setting the following two environment variables:

* `BASIC_AUTH_USERNAME` - username
* `BASIC_AUTH_PASSWORD` - password

### Enable Authentication

To enable this feature, a secret has to be created that holds the two variables. This secret has to be applied within the Kubernetes deployment for the Keptn Bridge.

* Create the secret using:

    ```console
    kubectl -n keptn create secret generic bridge-credentials --from-literal="BASIC_AUTH_USERNAME=<USERNAME>" --from-literal="BASIC_AUTH_PASSWORD=<PASSWORD>"
    ```

    **Note:** Replace `<USERNAME>` and `<PASSWORD>` with the desired credentials.

    <details><summary>*If you are using Keptn 0.6.1 or older, please click here.*</summary>
    <p>

    * Edit the deployment of the bridge using:

    ```console
    kubectl -n keptn edit deployment bridge
    ```
      
    * Add the secret to the `bridge` container, as shown below:

    ```yaml
    ...
    spec:
      containers:
      - name: bridge
        image: keptn/bridge2:0.6.1
        imagePullPolicy: Always
        # EDIT STARTS HERE
        envFrom:
          - secretRef:
              name: bridge-credentials
              optional: true
        # EDIT ENDS HERE
        ports:
        - containerPort: 3000
        ...
    ```

    </p>
    </details>


* Restart the pod of the Keptn Bridge by executing:

    ```console
    kubectl -n keptn delete pods --selector=run=bridge
    ```

### Disable Authentication

* To disable the basic authentication, delete the secret by executing: 

    ```console
    kubectl -n keptn delete secret bridge-credentials
    ```

* Restart the respective pod of the Keptn Bridge by executing:

    ```console
    kubectl -n keptn delete pods --selector=run=bridge
    ```

## Views in Keptn Bridge

### Project view

The Keptn Bridge provides an easy way to browse all events that are sent within Keptn. When you access the Keptn Bridge, all projects will be shown on the start screen. When clicking on a project, the stages of this project and all onboarded services are shown on the next view.

  {{< popup_image
  link="./assets/bridge_empty.png"
  caption="Keptn Bridge project view">}}

When selecting one service, all events that belong to this service are listed on the right side. Please note that this list only represents the start of a deployment (or problem) of a new artifact. More information on the executed steps can be revealed when you click on one event.

### Event Stream

When selecting an event, the Keptn Bridge displays all other events that are in the same Keptn context and belong to the selected entry point. As can be seen in the screenshot below, the entry point around 4:03 pm has been selected and all events belonging to this entry point are displayed on the right side.

  {{< popup_image
  link="./assets/bridge_details.png"
  caption="Keptn Bridge event stream">}}

## Deep links into Keptn Bridge

For integration of the Keptn Bridge into DevOps tools, a list of following deep links is provided: 

- `project/:projectName`
  - Opens project view of the project specified by `projectName`.
- `project/:projectName/:serviceName`
  - Opens project view of the project specified by `projectName` and expands the service specified by `serviceName`.
- `project/:projectName/:serviceName/:contextId`
  - Opens project view of `projectName`, expands the service specified by `serviceName`, and selects root event of *keptn context* specified by `contextId`.
- `project/:projectName/:serviceName/:contextId/:eventId`: 
  - Opens project view of `projectName`, expands the service specified by `serviceName`, and selects root event of *keptn context* specified by `contextId`. 
  - Finally, Keptn Bridge scrolls to event with the `eventId`.
- `trace/:shkeptncontext`
  - Loads that root event and redirects to `project/:projectName/:serviceName/:contextId`
- `trace/:shkeptncontext/:stage`
  - Loads that root event and redirects to `project/:projectName/:serviceName/:contextId/:eventId` where eventId is the id of the first event of the specific stage.
- `trace/:shkeptncontext/:eventtype`
  - Loads that root event and redirects to `project/:projectName/:serviceName/:contextId/:eventId` where eventId is the id of the last event with the specific event type.

## Early Access Version of Keptn Bridge

<!--
Right now there is no early access version of Keptn Bridge available. You can upgrade to the latest version (0.6.2) by executing the following commands:

```console
kubectl -n keptn set image deployment/bridge bridge=keptn/bridge2:0.6.2 --record
```
-->

There is an early access version of Keptn Bridge available (compatible with Keptn 0.6.2):

  {{< popup_image
  link="./assets/bridge_eap.png"
  caption="Keptn Bridge EAP">}}

* To install it, you have to update the Docker images of *Keptn Bridge* deployment by executing the following commands:

```console
kubectl -n keptn set image deployment/bridge bridge=keptn/bridge2:0.7.0-eap20200617 --record
```


* To restore the old version of bridge, configuration-service and mongodb-datastore (as delivered with Keptn 0.6.2), you can use the following commands:

```console
kubectl -n keptn set image deployment/bridge bridge=keptn/bridge2:0.6.2 --record
```


If you have any questions or feedback regarding Keptn Bridge, please contact us through our [Keptn Community Channels](https://github.com/keptn/community)!
