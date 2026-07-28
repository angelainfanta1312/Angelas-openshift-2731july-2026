# Day 2

## Info - Container Orchestration Platform Overview
<pre>
- motivation to use Container Orchestration Platform
  - it offers every features that are required to make you application highly available (HA)
  - it has in-built monitoring tools, to check the health of your application, check if your application is live,
    check your application is ready to serve
  - it has provides performance metrics out of the box
  - it has features to check your application logs
  - it has features to scale up/down your application instances based on user-traffic either manually or automatically
  - it has features to upgrade/downgrade your application from one version to other without any downtime
  - it supports different deployment strategies
    - blue-green, A/B, canary kind of deployment strategies
  - it supports different types of services to make your application accessible within the container orchestration 
    platform or for external
  - some container orchestration platform they also offer CI/CD
  - self-healing
- in order to deploy an application into Container Orchestration Platform, they must be containerized first
  i.e you should have a container image that package your application and all its dependencies as a container images
- Container Orchestration Platform examples
  1. Docker SWARM
  2. Google Kubernetes
  3. Rancher
  4. Red Hat Openshift
  5. AWS eks ( Managed Kubernetes services from AWS )
  6. aks ( Managed Kubernetes service from Azure )
  7. ROSA ( Managed Red Hat Openshift from AWS )
  8. ARO ( Managed Red Hat Openshift from Azure )
</pre>
