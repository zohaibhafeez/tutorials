# Create Microsoft SQL 2019 RTM pod in Kubernets on Docker for Windows
## Pre-requisites: 

Install Docker for Windows 

Go to Docker settings > Kubernetes and share the C drive and Apply:
![Docker Settings Shared Drives](https://github.com/zohaibhafeez/Kubernetes/blob/master/SQL%202019%20Ubuntu%20with%20Docker%20for%20Windows%20Kubernetes%20using%20HostPath/Docker-Settings-Shared-Drive.png) 
Under Docker Settings > Kubernetes, check the following and Apply:
![Docker Settings Enable Kubernetes](https://github.com/zohaibhafeez/Kubernetes/blob/master/SQL%202019%20Ubuntu%20with%20Docker%20for%20Windows%20Kubernetes%20using%20HostPath/Docker-Settings-Enable-Kubernetes.png)

Enable Kubernetes.

Deploy Docker Stack to Kubernetes by default.

#### Create volume and claim using HostPath:

kubectl apply -f localdev-sqlpv.yml

#### Create SQL 2019 pod with Service:

kubectl apply -f localdev-sql.yml

