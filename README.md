# AKS Networking Specialist Assessments Practical Labs

docker run -it sturrent/<LAB_NAME>:latest

aks-l200lab -h

## Lab1

az login --use-device-code

aks-l200lab -g l200lab1 -n aks1 -l 1

NODE_RESOURCE_GROUP="$(az aks show -g l200lab1 -n aks1 --query nodeResourceGroup -o tsv)"
CLUSTER_NSG="$(az network nsg list -g $NODE_RESOURCE_GROUP --query [0].name -o tsv)"

az network nsg rule list -g $NODE_RESOURCE_GROUP --nsg-name $CLUSTER_NSG -o table
az network nsg rule delete -g $NODE_RESOURCE_GROUP --nsg-name $CLUSTER_NSG -n SecRule1 -o table
az network nsg rule list -g $NODE_RESOURCE_GROUP --nsg-name $CLUSTER_NSG -o table

kubectl -n kube-system delete po -l component=tunnel

aks-l200lab -g l200lab1 -n aks1 -l 1 -v

# Lab2

az login --use-device-code

aks-l200lab -g l200lab2 -n aks2 -l 2

kubectl get -n kube-system pods,svc -l "k8s-app in (kube-dns,coredns-autoscaler)"

kubectl describe pod coredns-dc97c5f55-fzktg -n kube-system

kubectl logs coredns-dc97c5f55-fzktg -n kube-system

kubectl edit configmap coredns-custom -n kube-system

kubectl delete pod --namespace kube-system -l k8s-app=kube-dns

aks-l200lab -g l200lab2 -n aks2 -l 2 -v

# Lab3=

az login --use-device-code

aks-l200lab -g l200lab3 -n aks3 -l 3

[Coming breasking change] In the coming release, the default behavior will be changed as follows when sku is Standard and zone is not provided: For zonal regions, you will get a zone-redundant IP indicated by zones:["1","2","3"]; For non-zonal regions, you will get a non zone-redundant IP indicatd by zones:[].

Cluster has a service called azure-load-balancer in pending state on the kube-system namespace, the requirement is to have it working with the existing IP that was configured there...
Cluster uri == /subscriptions/60796668-xxxx-xxxx-xxxx-74f9e7dba880/resourcegroups/l200lab3/providers/Microsoft.ContainerService/managedClusters/aks3

kubectl get svc -A

kubectl describe svc azure-load-balancer -n kube-system 

kubectl edit svc azure-load-balancer -n kube-system

kubectl get svc -A

aks-l200lab -g l200lab3 -n aks3 -l 3 -v

# Lab4

az login --use-device-code

aks-l200lab -g l200lab4 -n aks4 -l 4

/subscriptions/60796668-xxxx-xxxx-xxxx-74f9e7dba880/resourcegroups/l200lab4/providers/Microsoft.ContainerService/managedClusters/aks4

az aks update \
  --resource-group l200lab4 \
  --name aks4 \
  --disable-cluster-autoscaler

AzureContainer > Overview > Wiki > Operations Fail with code VMAgentStatusCommunicationError or VMExtensionProvisioningTimeout

Azure Portal > Resource Group > l200lab4 > l200lab4_vnet > DNS servers > Defaults

az vmss run-command invoke -g MC_l200lab4_aks4_eastus2 -n aks-nodepool1-33023046-vmss --command-id RunShellScript --instance-id 10 --scripts " nc -vz mcr.microsoft.com 443 "

aks-l200lab -g l200lab4 -n aks4 -l 4 -v

//== Lab5 ==

az login --use-device-code

aks-l200lab -g l200lab5 -n aks5 -l 5

Cluster has two deployments fe-pod and be-pod. The pod on be-pod is sending data to pod in fe-pod over port 8080.
The data is beeing send every 5 seconds and it has the secret phrase in plain text. Setup a capture on the fe-pod and analyse the tcp stream to get the secret phrase.
Hint: you can use something like https://github.com/eldadru/ksniff to caputer the traffic and analyse the tcp stream with Wireshark.

Cluster uri == /subscriptions/60796668-xxxx-xxxx-xxxx-74f9e7dba880/resourcegroups/l200lab5/providers/Microsoft.ContainerService/managedClusters/aks5

kubectl get pods

kubectl exec -it fe-pod-79895f9b87-v2rq6 -- /bin/sh

kubectl cp [namespace]/[pod_name]:/path/nameofthecapture.cap /path/destination_folder/nameofthecapture.cap

kubectl cp fe-pod-79895f9b87-v2rq6:/tmp/testcapture.pcap testcapture.pcap 

Wireshark > Follow > TCP Stream

aks-l200lab -g l200lab5 -n aks5 -l 5 -v
