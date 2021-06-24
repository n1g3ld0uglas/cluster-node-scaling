# Cluster Node Scaling
If you ever need to scale down a test cluster to 0 nodes in order to reduce costs on your cloud platform, the below commands might be helpful


# Google's Kubernetes Engine (GKE)
To scale a GKE cluster down to 0 nodes, run the below command
```
gcloud container clusters resize nigel-gke-cluster --zone=europe-west2-a --num-nodes=0
```

To scale a GKE cluster back to your desired 3 nodes, run the below command
```
gcloud container clusters resize nigel-gke-cluster --zone=europe-west2-a --num-nodes=3
```

If you are unable to scale a cluster down to 0, or if it auto-scales back to a default numbner, you will need to explicitly specify the desired node count within the 'gcloud container cluster create' command:

```
gcloud container clusters create nigel-gke-cluster --zone europe-west2-a --machine-type e2-standard-8 --num-nodes 3
```

# Amazon's Elastic Kubernetes Service (EKS)
To scale an EKS cluster down to 0 nodes, run the below command
```
eksctl scale nodegroup --cluster nigel-eks-cluster2 --name ng-590ee331 --nodes 0
```
To scale an EKS cluster back to your desired 3 nodes, run the below command
```
eksctl scale nodegroup --cluster nigel-eks-cluster2 --name ng-590ee331 --nodes 3
```
Similar to GKE, if you cannot scale down the cluster, or if it auto-scales, you will need to specify the desired maximum and minimum number of nodes for your cluster within the 'eksctl create nodegroup' command:
```
eksctl create nodegroup --cluster nigel-eks-cluster --node-type t3.xlarge  --nodes 3 --nodes-min 0 --nodes-max 3 --node-ami auto --max-pods-per-node 58
```

If you're having a hard time finding the cluster name and node group names, run the below commands:

```
eksctl get cluster
```

The cluster name in this case is 'nigel-eks-cluster2', sp we can get the node group associated with that cluster name:

```
eksctl get nodegroup --cluster nigel-eks-cluster2
```

# Microsoft's Azure Kubernetes Service (AKS)
Instead of scaling an AKS cluster down to 0 nodes, there's an option to outright stop the cluster:
```
az aks stop --name nigel-aks-cluster --resource-group nigel-aks-rg
```

Similarly, you can start the stopped cluster whenever you want - there's no prerequisite checks required here:
```
az aks start --name nigel-aks-cluster --resource-group nigel-aks-rg
```

However, there is also the option to scale down the node count in an agent pool - similar to EKS and GKE:
```
az aks nodepool scale --name agentpool --cluster-name nigel-aks-cluster --resource-group nigel-aks-rg  --node-count 0
```

To scale the cluster back up to 3 nodes, you can run the below command:
```
az aks scale --resource-group nigel-aks-rg --name nigel-aks-cluster --node-count 3 --nodepool-name agentpool
```

To get the AKS Node Pool in this case, run the below 'AKS show' command in the Azure CLI:
```
az aks show --resource-group nigel-aks-rg --name nigel-aks-cluster --query agentPoolProfiles
```

If you wish to expose the AKS Network Profile, run the below 'AKS show' command in the Azure CLI:
```
az aks show -g nigel-demo -n nigel-dem-aks --query 'networkProfile' --output json
```
