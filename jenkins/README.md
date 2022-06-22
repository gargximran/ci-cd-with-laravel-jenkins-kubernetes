# Steps to follow to run docker compose with jenkins

### > Step One
Make sure your ***/var/run/docker.sock*** file have right permission for container

```bash
sudo chmod 666 /var/run/docker.sock
```

### > Step Two
Start the container with docker compose

######With sudo user

```bash
sudo docker-compose up
```
######Without sudo user

```bash
docker-compose up
```

#####'''application will Start on port 8080'''
Open jenkins in http://localhost:8080 and setup initial setup. 

### > Step Three
Now set up jenkin for pipeline

**follow this steps: *> Manage Jenkins > Manage Credentials > Jenkins > Global credentials > Add Credentials > (fill the form with your detail for setting docker credential)***

```

Kind: Username with password
Scope: Global
Username: your docker hub Username
password: your docker hub password
ID: dockerhub


```

This credential for setting up your docker hub registry.

### > Step Four
Add New Job in Jenkins

```
Item Name: your cloise

Type: pipeline

press ***OK***

Definition: Pipeline script from SCM
SCM: Git
Repository URL: https://github.com/gargximran/ci-cd-with-laravel-jenkins-kubernetes.git

Credentials: -none-

Branch Specifier: */main

Script Path: Jenkinsfile

***Press Save***

```



### > Step Five
if you want to intigrate kubectl controlplate in cluster then follow this steps.


    Install this Plugin > ***SSH Agent***
#####Set Credential for Server access
    Scope: Global
    ID: kubernetes_control_plane_cred
    Username: as your
    Private Key: your private key for server


***Manage Jenkins > Configure System > Global Properties > Environment Variable > Add ***

```
Name: kubectl_client_server_ip
Value: your cluster ip


---
Name: kubectl_client_server_username
Value: your cluster username
```


### > Step Five
Got to inside the job and Press on ***Build Now***
Pipeline will start 
***After successfully build please check to your docker registry to see the output image.***



----------------------------------------------

***If anything goes wrong! I suggest you to check the credential validity***

