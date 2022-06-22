## Build Explanation
I use ubuntu docker image and build new image from that and make new image ready for deploy **Laravel application**


## Deploy Explanation

#### Deployment
Deployment have ***node selector***

Please use this command to your cluster for attach label in a node. other wise please comment out the **nodeSelector** block inside ***deployment.yaml*** file.
```bash

kubectl label nodes ${node name} level=one

```

```bash
kubectl apply -f deployment.yaml

```
Use this command from control plane of cluster to deploy this deployment yaml file inside kubernetes. ***Please adjust the file path based on your***

#### Service 
Service type for this deployment is ***ClusterIP type***. I will pass this service as proxy pass from nginx config.

```bash
kubectl apply -f service.yaml

```