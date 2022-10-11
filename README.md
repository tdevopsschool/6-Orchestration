## Prerequirements
### Install
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html)
- [AWS Authentificator](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html)
### Get from the school chat
- Access key ID
- Secret access key
- Region
### Execute
```
$ aws configure
  AWS Access Key ID - XXXXXXXXXX
  AWS Secret Access Key - XXXXXXXXXX
  Default region name - XXXXXXXXXX
  Default output format - json
  
$ aws eks update-kubeconfig --name school
```
---
### Or you may use [Rancher desktop](https://rancherdesktop.io) to setup local Kubernetes cluster!
> [Minikube](https://minikube.sigs.k8s.io/docs/) is OK as well, but we recommend Rancher as more easy tool.

## Homework 1
*[#homework]() [#orchestration1]()*
1. Create your Namespace
2. Create a deployment:
- Nginx application with 3 replicas
- Scale down it to 1 replica
- Change Nginx image to another one
- Rollback to previous Nginx deployment with 3 replicas
3. Create a staefulset:
- Mongo DB with 3 replicas
4. Send scrrenshots with version of **deployment** and **statefulset** in the chat with homework hashtags

### Useful commands
```
# Apply yaml file
$ kubectl apply -f {YAML_FILE}

# Describe kubernetes object
$ kubectl describe {KUBERNETES_OBJECT_TYPE} {OBJECT_NAME}

# Get kubernetes object info and status
$ kubectl get {KUBERNETES_OBJECT_TYPE} -n {NAMESPACE} [-o wide]

# Deployment rollout history
$ kubectl rollout history deployment {DEPLOYMENT_NAME} -n {NAMESPACE}

# Rollback to previous version of a deployment
$ kubectl rollout undo deployment {DEPLOYMENT_NAME} --to-revision={N} -n {NAMESPACE}

# Get pod's logs
$ kubectl logs {POD_NAME}

# Scaling of ReplicaSet, Deployemnt, Statefulset
$ kubectl scale {KUBERNETES_OBJECT_TYPE} {OBJECT_NAME} --replicas=3 -n {NAMESPACE}

# Edit any kubernetes object as yaml file
$ kubectl edit {KUBERNETES_OBJECT_TYPE}/{OBJECT_NAME} -n {NAMESPACE}

# Get a Shell to a Running Container
$ kubectl exec -it {POD_NAME} -- /bin/sh

# Port forwarding to access a pod with 80 port as localhost:8080 application in browser or terminal
$ kubectl port-forward pods/{NAME_OF_POD} 8080:80
```

## Links 1
- [Run a Stateless Application Using a Deployment](https://kubernetes.io/docs/tasks/run-application/run-stateless-application-deployment/)
- [Running MongoDB on Kubernetes with StatefulSets](https://kubernetes.io/blog/2017/01/running-mongodb-on-kubernetes-with-statefulsets/)
- [Use Port Forwarding to Access Applications in a Cluster](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/#forward-a-local-port-to-a-port-on-the-pod)
- [Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug/debug-application/get-shell-running-container/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## Adiitional links
- [CERTIFIED KUBERNETES ADMINISTRATOR (CKA)](https://www.cncf.io/certification/cka/)
- [The Complete Guide on Getting Certified in Kubernetes(CKA) and getting Hands-on](https://medium.com/devopsturkiye/the-complete-guide-on-getting-certified-in-kubernetes-cka-and-getting-hands-on-a6f4d18bb54b)
- [Complete Kubernetes Tutorial for Beginners](https://www.youtube.com/playlist?list=PLy7NrYWoggjziYQIDorlXjTvvwweTYoNC)
