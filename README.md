# Kubernetes (K8)
![img.png](img.png)
## What is K8
Kubernetes is a container orchestration platform for scheduling and automating the deployment, management, and scaling of containerized applications.
It is a containerisation orchestration tool that that facilitates both declarative
configuration and automation. It is widely available and enables convenient management of a lot of
different containerisation services. Kubernetes manages containers for over 69% of companies.

## Benefits of K8 and why should we use it?
Kubernetes provides you with a framework to run distributed systems resiliently. It takes care of scaling and failover for your application, provides deployment patterns, and more. For example, Kubernetes can easily manage a canary deployment for your system.

- **Service discovery and load balancing:** Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable.
- **Storage orchestration:** Kubernetes allows you to automatically mount a storage system of your choice, such as local storages, public cloud providers, and more.
- **Automated rollouts and rollbacks:** You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.
- **Automatic bin packing:** You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources.
- **Self-healing:** Kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't advertise them to clients until they are ready to serve.
- **Secret and configuration management:** Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configuration.

## Drawbacks of K8
- **Kubernetes can be an overkill for simple applications:** If you do not intend to develop anything complex for a large or distributed audience (so, you are not building a worldwide online shop with thousands of customers for example) or with high computing resource needs (e.g. machine learning applications), there is not much benefit for you from the technical power of k8s.
- **Kubernetes is very complex and can reduce productivity:** Kubernetes is known for its complexity. Especially for developers not familiar with infrastructure technologies, it can be very hard to get used to the Kubernetes development workflow.
- **The transition to Kubernetes can be cumbersome:** Most companies cannot start on a green field, your existing software needs to be adapted to run smoothly with Kubernetes or at least alongside newly built application that will run on Kubernetes. It is hard to estimate how much effort this requires as this depends heavily on the software.
- **Kubernetes can be more expensive than its alternatives:** The previously mentioned disadvantages cost time of your engineers that is not spent on creating new “tangible” business value.


## Use cases
- **Simple Deployment of Stateless Applications:** A very popular stateless app to run on top of Kubernetes is nginx, the open-source web server. Running nginx on Kubernetes requires a deployment YAML file that describes the pod and underlying containers.
- **Deploy Stateful Data Services:** Using the same data structure and the same tools, it’s possible to create services on the platform with persistent storage. While it’s a little extra work to re-platform data services like databases onto the Kubernetes platform, it does make sense.
- **CI/CD Platform with Kubernetes:** Kubernetes’ open API brings many advantages to developers. The level of control means developers can integrate Kubernetes into their automated CI/CD workflow effortlessly. So even while Kubernetes doesn’t provide any CI/CD features out of the box, it’s very easy to add Kubernetes to a CI/CD pipeline.

## Competitors of K8
The following link provides more detail on the competitors for K8 `https://www.clickittech.com/devops/kubernetes-alternatives/`
- Amazon ECS
- Docker Swarm
- RedHat OpenShift
- Nomad
- AWS Fargate
## Managed services for K8
The following link provides more detail on the managed services for K8 `https://techgenix.com/top-managed-kubernetes-services/`
- Google Kubernetes Engine (GKE)
- Amazon Elastic Kubernetes Service (EKS)
- Microsoft Kubernetes Azure (AKS)
- IBM Cloud Kubernetes Service
- Oracle Container Engine for Kubernetes
- DigitalOcean Kubernetes

## Non-managed services for K8
When we deploy a cluster through kubeadm, kubespray, or even by doing it manually (the hard way), you have full access to the cluster and master all the other related management components. You’ll also have more control over the deployment and administration of your cluster. For example, you can implement multiple node groups or choose to have different instance types for different nodes. These options are not available with many Kubernetes managed services.

The following link provides more detail on the non-managed services for K8 `https://www.magalix.com/blog/provider-managed-vs.-self-managed-kubernetes`
- Kubespray
- Kubeadm
- Kops
- Kubetail
- Kubewatch
- Prometheus
- HELM

## Commands
- `kubectl get service` or `kubectl get svc` 
- `kubectl get nodes` Shows the nodes
- `kubectl get pods` Shows the running pods
- `kubectl create -f <file_name>.yml` Creates the pod using the code in the file
- `kubectl edit deploy <pod_name>` Edit deploy file in notepad
- `kubectl edit svc <pod_name>` Edit service file in notepad
- `kubectl delete deploy <pod_name>` Deletes the deploy pod 
- `kubectl delete service <pod_name>` Deletes the service pod 
- `kubectl describe pod POD_ID` Shows the logs of the pod to help debug
- `kubectl scale deploy node-app --replicas=8`

## Iteration 1
- Open the docker desktop app -> click on settings -> enable kubernetes and restart
- Delete the containers occupying ports 80, 27017 and 3000
- Delete any unused containers and images to free up space. Ensure you have these available on DockerHub before deleting. 
- Create a deploy YAML file `nginx_k8_deploy.yml` and put the following code
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  
spec:
  selector:
    matchLabels:
      app: nginx 

  replicas: 2 

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
          - containerPort: 80
```
- Create a service YAML file in the same directory `nginx-service.yml`
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-deployment
  namespace: default
  resourceVersion: "40883"
  uid: 9190ab75-d61c-4ff4-a3d1-0d293fa8d72e
spec:
  # clusterIP: 10.96.0.1
  # clusterIPs:
  # - 10.96.0.1
  # externalTrafficPolicy: Cluster
  # ipFamilies:
  # - IPv4
  # ipFamilyPolicy: SingleStack
  ports:
  - nodePort: 30442
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx
  sessionAffinity: None
  type: LoadBalancer
status:
  loadBalancer:
    ingress:
    - hostname: localhost
```
- Create both of the pods separately and check they are running
- Go to your localhost and nginx default page should be running

## Iteration 2 - Launching node app and db

- Create four files in a new directory
- `mongo_deploy.yml`, `mongo_service.yml`, `app_deploy.yml` and `app_service.yml`