
# Creating an AKS Cluster with istio add on

```bash

# Set environment variables

export CLUSTER=myAksCluster
export RESOURCE_GROUP=myResourcegroup
export LOCATION=eastus

#Install mesh during cluster creation

az group create --name ${RESOURCE_GROUP} --location ${LOCATION}

az aks create --resource-group ${RESOURCE_GROUP} --name ${CLUSTER} --enable-asm --generate-ssh-keys

# Verify successful installation
az aks show --resource-group ${RESOURCE_GROUP} --name ${CLUSTER}  --query 'serviceMeshProfile.mode'

# get credentials
az aks get-credentials --resource-group ${RESOURCE_GROUP} --name ${CLUSTER}

# view the pods by istion namespace
kubectl get pods -n aks-istio-system


#  Enable sidecar injection

az aks show --resource-group ${RESOURCE_GROUP} --name ${CLUSTER}  --query 'serviceMeshProfile.istio.revisions'

kubectl label namespace default istio.io/rev=asm-X-Y


# Enable external ingress gateway

az aks mesh enable-ingress-gateway --resource-group $RESOURCE_GROUP --name $CLUSTER --ingress-gateway-type external

kubectl get svc aks-istio-ingressgateway-external -n aks-istio-ingress

# delete the resource group
az group delete --name ${RESOURCE_GROUP} --yes --no-wait

```
## Goal: 80% traffic to httpd and 20% traffic to nginx

### step 1:
  * creating deployment for 2 webservers
### step 2:
  * create a destinationRule istio

### step 3:
  * Create a istio Virtual Service with traffic splitter

### step 4:
  * Step 3: Create a istio gateway

### step 5:
  * Enabling external gateway in Azure