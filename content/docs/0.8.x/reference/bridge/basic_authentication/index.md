---
title: Basic Authentication
description: Enable/Disable basic authentication
weight: 21
keywords: [0.8.x-bridge]
---

The Keptn Bridge has a basic authentication feature, which can be controlled by setting the following two environment variables in the deployment of the bridge:

* `BASIC_AUTH_USERNAME` - username
* `BASIC_AUTH_PASSWORD` - password

## Enable Authentication

* Basic authentication is enabled when executing the below command. This command creates a secret with the two variables: `BASIC_AUTH_USERNAME` and `BASIC_AUTH_PASSWORD`. 

    ```
    keptn configure bridge --user=<USER> --password=<PASSWORD>
    ```

    **Note:** Replace `<USER>` and `<PASSWORD>` with the desired credentials.

* Restart the pod of the Keptn Bridge by executing:

    ```console
    kubectl -n keptn delete pods --selector=app.kubernetes.io/name=bridge
    ```

## Disable Authentication

* To disable the basic authentication, delete the secret by executing: 

    ```console
    kubectl -n keptn delete secret bridge-credentials
    ```

* Restart the pod of the Keptn Bridge by executing:

    ```console
    kubectl -n keptn delete pods --selector=app.kubernetes.io/name=bridge
    ```
## Configure Bridge Credentials

* You can use the Keptn API in order to set new credentials used for basic authentication:

    ```console
    BASIC_AUTH_PASSWORT=my-desired-password

    curl -X POST "${KEPTN_ENDPOINT}/api/v1/config/bridge" -H "accept: application/json" -H "Content-Type: application/json" -H "x-token: ${KEPTN_API_TOKEN}" -d "{ \"user\": \"keptn\", \"password\": \"${BASIC_AUTH_PASSWORT}\"}"
    ```