# terraform-aro
# Using Terraform to build an ARO cluster

Azure Red Hat OpenShift (ARO) is a fully-managed turnkey application platform.

## Setup

Using the code in the repo will require having the following tools installed:

- The Terraform CLI
- The OC CLI
- Azure CLI

## Create the ARO cluster and required infrastructure

### Public ARO cluster

1. Modify the `variable.tf` var file, or modify the following command to customize your cluster.

   ```bash
   terraform init
   terraform plan -var "cluster_name=my-tf-cluster" -out aro.plan
   terraform apply aro.plan
   ```
 ## Test Connectivity

1. Get the ARO cluster's console URL.

   ```bash
   ARO_URL=$(az aro show -n $AZR_CLUSTER -g $AZR_RESOURCE_GROUP -o json | jq -r '.apiserverProfile.url')
   echo $ARO_URL
   ```

2. Get the ARO cluster's credentials.

   ```bash
   ARO_USERNAME=$(az aro list-credentials -n $AZR_CLUSTER -g $AZR_RESOURCE_GROUP -o json | jq -r '.kubeadminUsername')
   ARO_PASSWORD=$(az aro list-credentials -n $AZR_CLUSTER -g $AZR_RESOURCE_GROUP -o json | jq -r '.kubeadminPassword')
   echo $ARO_PASSWORD
   echo $ARO_USERNAME
   ```

### Public Test Connectivity

1. Log into the cluster using oc login command from the create admin command above. ex.

    ```bash
    oc login $ARO_URL -u $ARO_USERNAME -p $ARO_PASSWORD
    ```

2. Check that you can access the Console by opening the console url in your browser.

## Cleanup

1. Delete Cluster and Resources

    ```bash
    terraform destroy -auto-approve "aro.plan"
    ```
