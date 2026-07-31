# Day 4

## Lab - Securing your application using https url (edge route )
```
oc delete project jegan-project
oc new-project jegan-project
oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.28 --replicas=3
oc get deploy,pods

# The command below creates a clusterip internal service for nginx deployment 
oc expose deploy/nginx --port=8080

openssl version
# Let's generate a private key
openssl genrsa -out key.key

# Let's create a public key using the private key for your organization domain
openssl req -new -key key.key -out csr.csr -subj="/CN=nginx-jegan.apps.ocp4.palmeto.org"

# Sign the public key using the private key and generate certificate(.crt)
openssl x509 -req -in csr.csr -signkey key.key -out crt.crt

# The command below creates an edge(https) route
oc create route edge --service nginx --hostname nginx-jegan.apps.ocp4.palmeto.org --key key.key --cert crt.crt

oc get route
curl --insecure https://nginx-jegan.apps.ocp4.palmeto.org
```


## Info - Ingress
<pre>
- is not service ( not a service like ClusterIP, NodePort or LoadBalancer )
- it is routing/forwarding rules
- For ingress to work in your Kubernetes/Openshift cluster, we need 3 things
  1. Ingress rule
     - annotation in the yaml file must mention which Ingress Controller must be used
  2. Ingress Controller
     - example
       - Nginx Ingress Controller
       - HAProxy Ingress Controller
       - F5 Ingress Controller
  3. Load Balancer
     - Nginx Load Balancer
     - HAProxy Load Balancer
     - F5 Load Balancer
- The Ingress Controller installed in your Openshift cluster keeps watching for Ingress rules defined in any project namespace
- As soon as the Ingress Controller detected an ingress rule, the Ingress Controller retrieves the rules
  defined in the Ingress, and it configures the respective Load Balancer with those rules dynamically at runtime
- Behind the Ingress there will be multiple services ( ClusterIP, NodePort & LoadBalancer )
- Behind Service -> there will be many Pods 
- Behind Ingress -> there will be many services ( internal, external )
- The Openshift route is based on Kubernetes Ingress
- the difference between Ingress and OpenShift Route is
  - behind Route there will only one Service normally (100% routes to single service )
  - behing an Ingress there will be multiple Services
</pre>

## Lab - Ingress
Let's delete and recreate a new project
```
oc delete project jegan-project
oc new-project jegan-project

cd ~/openshift-2731july-2026
git pull
cd Day4/ingress
oc apply -f nginx-deploy.yml
oc apply -f hello-deploy.yml
oc apply -f nginx-svc.yml
oc apply -f hello-svc.yml
oc describe svc/nginx
oc describe svc/hello
oc apply -f ingress.yml
oc get ingress

curl http://tektutor.apps.ocp4.palmeto.org/hello
curl http://tektutor.apps.ocp4.palmeto.org/nginx
```
