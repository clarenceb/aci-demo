# ACI demo

az group create -n perth-demo -l australiaeast

source acr-vars.env

az container create --resource-group perth-demo \
    --name aci-demo-perth \
    --image ${ACR_LOGIN_SERVER}/hackfest/service-tracker-ui:1.0 \
    --cpu 1 --memory 1 \
    --registry-login-server ${ACR_LOGIN_SERVER} \
    --registry-username ${ACR_USER} \
    --registry-password ${ACR_PASSWORD} \
    --dns-name-label aci-demo-perth \
    --ports 8080

az container show --resource-group perth-demo --name aci-demo-perth --query instanceView.state
az container show --resource-group perth-demo --name aci-demo-perth --query ipAddress.fqdn

cutl http://aci-demo-perth.australiaeast.azurecontainer.io:8080

az container logs --resource-group perth-demo --name aci-demo-perth

az container delete --name aci-demo-perth -g perth-demo
