# openshift-2731july-2026

## Pre-test link
<pre>
https://forms.cloud.microsoft/r/Mrgt95bg0Q  
</pre>  

## About about lab environment
<pre>
We have got two servers with below configurations
- Dell PowerEdge R840 Server
- Intel Xeon Processor with 192 CPU Cores
- 1 TB RAM
- 21 TB SSD
</pre>

## Linux Login Credentials
<pre>
username - yourname
password - palmeto@123
</pre>

#### Server 1 (192.168.10.200)
![Server1](server1.png)

#### Server 2 (192.168.10.201)
![Server2](server2.png)

## Lab - Getting used to Openshift projects

Listing all projects
```
oc get projects
oc get project
oc get namespaces
oc get namespace
oc get ns
kubectl get namespaces
```

Creating new project
```
oc new-project jegan
```

Switching between projects
```
oc project default
oc project jegan
```

Finding currently active project
```
oc project
```
