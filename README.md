# ACI demo

Run a single container with Azure Container Instances.

## Prep

* Azure subscription
* Azure CLI setup
* Create file `acr-vars.env` based on `acr-vars.env.exanmple`

```sh
source acr-vars.env

az group create -n ${RGROUP} -l ${REGION}

source acr-vars.env

az container create --resource-group ${RGROUP} \
    --name ${DEMO_NAME} \
    --image ${ACR_LOGIN_SERVER}/${DEMO_IMAGE} \
    --cpu 1 --memory 1 \
    --registry-login-server ${ACR_LOGIN_SERVER} \
    --registry-username ${ACR_USER} \
    --registry-password ${ACR_PASSWORD} \
    --dns-name-label ${DEMO_NAME} \
    --ports ${DEMO_PORT}

az container show --resource-group ${RGROUP} --name ${DEMO_NAME} --query instanceView.state
az container show --resource-group ${RGROUP} --name ${DEMO_NAME} --query ipAddress.fqdn

curl http://${DEMO_NAME}.${REGION}.azurecontainer.io:${DEMO_PORT}

az container logs --resource-group ${RGROUP} --name ${DEMO_PORT}

az container delete --name ${DEMO_PORT} -g ${RGROUP}
```

## Resources

* [Quickstart: Deploy a container instance in Azure using the Azure CLI](https://docs.microsoft.com/en-au/azure/container-instances/container-instances-quickstart)