## ConfigMap for Setting up NGINX config file mount
In ***configMap.yaml*** file you will notice I am passing ***two location block*** for ***proxy_pass*** with the service name I used for *app1* and *app2* . I am using this config as volume mount inside nginx image to location */etc/nginx/nginx.conf*


Make sure you are deploying **configMap.yaml** before nginx deployment.

```bash
kubectl apply -f congifMap.yaml

```



### Deployment
In deployment yaml I am using volume mount for setting up configuration as I want in nginx.

```bash
kubectl apply -f deployment.yaml

```
Use this command from control plane of cluster to deploy this deployment yaml file inside kubernetes. ***Please adjust the file path based on your***

#### Service 
Service type for this deployment is ***NodePort type***. This service is exporting as expected result on port **30007**.

```bash
kubectl apply -f service.yaml

```
---
Now you can visit 
http://ip-address:30007/app1  for application one output
http://ip-address:30007/app2  for application two output