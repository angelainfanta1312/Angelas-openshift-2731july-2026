# Day 3

## Info - You can find Red Hat Base images here
<pre>
https://catalog.redhat.com/en/software/containers/explore
</pre>

## Info - You can find other Red Hat Openshift compatible images here
<pre>
https://catalog.redhat.com/en/search?searchType=Containers
</pre>  

## Info - API Server
<pre>
- is the heart of kubernetes/openshift
- anything and everything in Kubernetes/Openshift is co-ordinated by API server, hence this is
  one of the most critical components
- API server only performs database activities like
  - it creates a new record in etcd datastore
  - it updates an existing record in the etcd datastore
  - it retrieves an existing record from the etcd datastore
  - it deletes an existing record from the etcd datastore
  - everytime some new record are added, existing records updated, existing records deleted it will
    send events to registered controllers
</pre>

## Info - etcd
<pre>
- it is distributed database which stores data in key/value format
- it is an idendepent opensource project used by Kubernetes & openshift
- this can be used outside the scope of K8s/Openshift as well
</pre>

## Info - Scheduler
<pre>
- is a Pod
- is one of the Control Plane components that runs in the master node
- it is responsible to identify a healthy node where a new Pod can be deployed
- scheduler by itself can't deploy a pod into any node, it is allowed only to share it scheduling
  recommendations to API Server via REST call
</pre>

## Info - Controller Managers
<pre>
- the real doers are controllers
- is a collection of many in-built controllers
- for each type of resource supported in Openshift/Kubernetes there is one Controller
- For example
  - Deployment Controller manages Deployment resource ( stateless application )
  - ReplicaSet Controller manages ReplicaSet resource
  - Job Controller manages Job resource
  - StatefulSet Controller manages Statefult resource ( stateful application )
  - DaemonSet Controller manages DaemonSet resource
- Controller is a application that runs within a Pod container
- they have unrestricted access within the Kubernetes/Openshift cluster
- they can monitor specific type of resources created/updated/deleted under any project within the cluster
- they get notified by API Server via events
- Controllers if it needs to communicate something to API Server, it make a REST API call
</pre>

## Lab - Login to openshift from command-line
```
cat ~/openshift.txt
# Server 1
oc login --username=kubeadmin --password=7ygAQ-3ovES-yevQV-shmAD https://api.ocp4.palmeto.org:6443 --insecure-skip-tls-verify

# Server 2
oc login --username=kubeadmin --password=7ygAQ-3ovES-yevQV-shmAD https://api.ocp4.palmeto.org:6443 --insecure-skip-tls-verify
```

## Lab - Deploying your application in declarative style into Openshift
```
# Delete your existing project ( make sure it is your project )
oc delete project jegan

# Create new project
oc new-project jegan

# Auto-generate the declarative manifest script for nginx deployment
oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.26 --replicas=3 --dry-run=client -o yaml

oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.26 --replicas=3 --dry-run=client -o yaml > nginx-deploy.yml

# create command must be used only the first time
# because create will assume the nginx deploy doesn't exist already, if exists it reports error and stops there
# save-config will store the file name with which we created the resources in declarative style
# later when we make delta changes to yaml file and when we apply the changes it is going to validate are we using the same file
# that we originally used to create the set of resources
oc create -f nginx-deploy.yml --save-config=true

oc get deploy,rs,po
```

## Info - What happens internally when we run the below command
```
oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.26 --replicas=3
```
![internals](openshift-internals.png)


## Lab - Creating an internal service for nginx deployment in declarative style
```
oc project jegan

oc get pods
oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml
oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml > nginx-clusterip-svc.yml

oc apply -f nginx-clusterip-svc.yml
oc get svc
oc describe svc/nginx
```

## Lab - Creating an external nodeport service for nginx deployment in declarative style
```
oc project jegan

oc get pods

# Delete existing clusterip service
oc delete -f nginx-clusterip-svc.yml

oc expose deploy/nginx --type=NodePort --port=8080 --dry-run=client -o yaml
oc expose deploy/nginx --type=NodePort --port=8080 --dry-run=client -o yaml > nginx-nodeport-svc.yml

oc apply -f nginx-nodeport-svc.yml
oc get svc
oc describe svc/nginx
```

