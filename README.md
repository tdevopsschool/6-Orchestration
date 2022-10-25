## Local Prerequirements
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

### Or you may use [Rancher desktop](https://rancherdesktop.io) to setup local Kubernetes cluster!
> [Minikube](https://minikube.sigs.k8s.io/docs/) is OK as well, but we recommend Rancher as more easy tool.

## Gitlab to K8s cluster connection configuration
### Prepare Docker image
>You will use the image as base image of deployment job. You can insert installation command right in script section of deployment job, but will be much better to create a new git repo to build and store the prepared Docker image. 
1. In Dockerfile add installtion of tools list:
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html)
- [AWS Authentificator](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)
- [Helm](https://helm.sh/docs/intro/install/)
- [Helmfile](https://github.com/helmfile/helmfile#installation)
2. Build and push the image in Gitlab registry (or you may use another registry to store the image)
### Provide auth variables in deployment job
1. Use prepared Docker image for deployment job.
2. Configure variables in Gitlab group or [project level](https://docs.gitlab.com/ee/ci/variables/#add-a-cicd-variable-to-a-project) variables:
```
AWS_DEFAULT_REGION= XXXXXXXXXX
AWS_ACCESS_KEY_ID= XXXXXXXXXX
AWS_SECRET_ACCESS_KEY= XXXXXXXXXX
```
3. Make cluster connection in `before_script` section:
```
before_sctipt:
  - aws sts get-caller-identity # Check connection
  - aws eks update-kubeconfig --name school # Get kube config
script:
  - # kubectl or helm or helmfile 
```

### P.S.
> If [Connecting a Kubernetes cluster with GitLab](https://docs.gitlab.com/ee/user/clusters/agent/index.html) is working on current moment you may use this connection approach as well.

### Connections links
- [Amazon EKS cluster automation with GitLab CI/CD](https://aws.amazon.com/ru/blogs/containers/amazon-eks-cluster-automation-with-gitlab-ci-cd/)
- [Connecting a Kubernetes cluster with GitLab](https://docs.gitlab.com/ee/user/clusters/agent/index.html)

## Homework 1
*[#homework]() [#orchestration1]()*
1. Create your Namespace.
2. Create a deployment:
- Nginx application with 3 replicas
- Scale down it to 1 replica
- Change Nginx image to another one
- Rollback to previous Nginx deployment with 3 replicas
3. Create a staefulset:
- Mongo DB with 3 replicas
4. Send screenshots with version of **deployment** and **statefulset** in the chat with homework hashtags.

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

## Homework 2
*[#homework]() [#orchestration2]()*
1. Cleanup your Namespace.
2. Create and configure deployment in Gitlab Course project (use the pipelines from CI/CD homework):
- Create Deployment yamls for backend and frontend
- Create Deployment yaml for PostgreSQL DB with 1 replica and 1 PV
- Create Services for all components
- Create Ingress'. For Frontend use host format <NAMESPACE_NAME>.school.telekom.sh
- Add probes
- Add resource requests and limits
3. Do deploy from Gitlab by pre-configuring the connection to the Kubernetes cluster in .gitlab-ci.yml
4. Check that FE, Backend and DB are working together like entire application:
- Insert a new course entry and a school with random name, then create a user with your name
6. Send screenshots of browser with application UI (with the student entry) and URL in the chat with homework hashtags.

## Links 2
- [Service](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- [Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)
- [Resource Management for Pods and Containers](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)

## Homework 3-1
*[#homework]() [#orchestration3-1]()*
1. Create in Gitlab another one project for helm charts.
2. Create 3 Helm charts for course project components: Backend, Frontend, DB. Charts must include:
- Deployment
- Service
- Ingress
- PVC (for DB)
3. In values:
-  Image
-  Replica counts
-  Host for ingress
4. Publish charts in Gitlab Helm repository of your git project.
5. Create another **values.yaml** file in Cource project repo to separate Dev and Prod installation:
- Production installation should have 2 replicas for Frontend and Backend
- Development installation should have 1 replicas for Frontend and Backend
6. Create additional Namaspace in Kubernetes for Prod deployment.
7. Rewrite gitlab-ci.yaml to do helm chart deployment, using charts from Helm repository of helm chart git project.
8. Do deployments to Prod and Dev environments in Gitlab pipelines.
9. Check Course project application in Dev and Prod envs.
10. Send screenshots of published charts from Gitlab registry in the chat with homework hashtags.

## Links 3-1
- [Helm.sh](https://helm.sh)
- [Helm charts in the Package Registry](https://docs.gitlab.com/ee/user/packages/helm_repository/)

## Homework 3-2
*[#homework]() [#orchestration3-2]()*
1. Use helmfile to do deployement to Dev and Prod envs instead of using diffrent values.yaml files with pure Helm:
- Create helmfile.yaml in Course project repository
- Configure releases and values in helmfile.yaml
- Create values.yaml and go templates if necessary for independent Dev and Prod deployemnt 
- Rewrite gitlab-ci.yaml to do helmfile deployment
2. Do deployment and check
3. Send screenshots of helmfile.yaml in the chat with homework hashtags.

## Links 3-2
- [Helmfile documentation](https://github.com/roboll/helmfile)
- [How to declaratively run Helm charts using helmfile](https://medium.com/swlh/how-to-declaratively-run-helm-charts-using-helmfile-ac78572e6088)

## Additional links
- [CERTIFIED KUBERNETES ADMINISTRATOR (CKA)](https://www.cncf.io/certification/cka/)
- [The Complete Guide on Getting Certified in Kubernetes(CKA) and getting Hands-on](https://medium.com/devopsturkiye/the-complete-guide-on-getting-certified-in-kubernetes-cka-and-getting-hands-on-a6f4d18bb54b)
- [Complete Kubernetes Tutorial for Beginners](https://www.youtube.com/playlist?list=PLy7NrYWoggjziYQIDorlXjTvvwweTYoNC)
- [Kubernetes Debug algoritm](https://hsto.org/webt/u3/mo/qv/u3moqv94rhagxo6hlq-feuawtyc.png)
