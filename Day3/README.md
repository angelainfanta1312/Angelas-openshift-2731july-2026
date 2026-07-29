# Day 3

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
