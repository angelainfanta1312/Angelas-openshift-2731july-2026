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
