# Cluster Node Scaling
If you ever need to scale down a test cluster to 0 nodes in order to reduce costs on your cloud platform, the below commands might be helpful


# Google Cloud Platform
To scale a GCP cluster down to 0 nodes, run the below command
```
gcloud container clusters resize nigel-gke-cluster --zone=europe-west2-a --num-nodes=0
```

To scale a GCP cluster back to your desired 3 nodes, run the below command
```
gcloud container clusters resize nigel-gke-cluster --zone=europe-west2-a --num-nodes=3
```

If you are unable to scale a cluster down to 0, or if it auto-scales back to a default numbner, you will need to explicitly specify the desired node count within the 'gcloud container cluster create' command:

```
gcloud container clusters create nigel-gke-cluster --zone europe-west2-a --machine-type e2-standard-8 --num-nodes 3
```

# Google Cloud Platform
To scale an EKS cluster down to 0 nodes, run the below command
```
eksctl scale nodegroup --cluster nigel-eks-cluster2 --name ng-590ee331 --nodes 0
```
To scale an EKS cluster back to your desired 3 nodes, run the below command
```
eksctl scale nodegroup --cluster nigel-eks-cluster2 --name ng-590ee331 --nodes 3
```

