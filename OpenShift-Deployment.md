# Deployment to OpenShift

## Create an Azure Org (https://dev.azure.com/)
   ![](./assets/azure-org.png)
## Create an Azure Project
   ![](./assets/azure-project.png)
## Create Pipeline point to the git repository
   ![](./assets/azure-pipeline.png)
## Look for OpenShift extension provided by Red Hat
   ![](./assets/azure-Browse%20Marketplace.png)
## Install OpenShift Extension
   ![](./assets/azure-Install%20Openshift%20Extension.png)
## Create Service Connection under Project Settings
   1) Provide Server URL ( OpenShift API server url)
   2) Provide Token for API Token
   ![](./assets/azure-%20Service%20Connection.png) 
   ### Note the service connection name should match the name in the pipeline
   ![](./assets/azure-openshift%20connection%20in%20pipeline.png)

## Run Pipeline
   
   Pipeline will use Imperative commands to deploy the react webapp to OpenShift.
   


# Run Agents on OpenShift

### Create an PAT (Personal Access Token)

![](./assets/azure-pat%20token.png)
Scope Required : Deployment Groups ( Read and Manage )
Store the token it needs to be reused to create the secret.

### Deploying the OpenShift Agent

Run the following commands to setup the prerequiste before deploying the agent.

```
    oc new-project azure-build

    oc create sa azure-build-sa

    oc adm policy add-scc-to-user anyuid -z azure-build-sa

    oc create secret generic azdevops \
    --from-literal=AZP_URL=https://dev.azure.com/<<YOUR ORG>> \
    --from-literal=AZP_TOKEN=<<YOUR PERSONAL ACCESS TOKEN>> \
    --from-literal=AZP_POOL="openshift Agent Pool"
```

Deploy the agent 
```
    oc create -f ./assets/resources/deployment.yaml 
```    

### Changes to Pipeline to use the agents
![](./assets/azure-agentpool%20pipeline.png)
Update the pool to the agent pool name in `azure-pipeline.yaml`



### Verifying the agents
Under Agent pool list the agents under the agents tab like shown below. This should list the pod names from OpenShift in Azure.

![](./assets/azure-agent%20verification.png)

If multiple pods are running under the deployment created all the pods should show here.







