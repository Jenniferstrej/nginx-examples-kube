# Introduction to Kubernetes with Minikube

## Pre requisites

* Familiarity with a Container runtime (e.g. Docker)

* [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/) - Minikube has its own pre-requisites. Please check page.
_If you have problems installing Minikube locally, you can use the Katakoda terminal provided on https://kubernetes.io/docs/tutorials/hello-minikube/#create-a-minikube-cluster however you may have problems running the port forward command later_

* [Git](https://www.linode.com/docs/development/version-control/how-to-install-git-on-linux-mac-and-windows/)

### Workshop Contents
1. Kubernetes Architecture
2. Namespaces
3. Pods
4. ReplicaSets
5. Deployments
   - Create
   - Edit
   - Rolling Upgrade Strategy
   - Rollout
   - Scale Deployment up and Down
6. Services
7. Port forward

### Bonus

8. ConfigMaps
9. Secrets


## Doing the workshop

Clone this repository with `git clone https://github.com/Jenniferstrej/nginx-examples-kube.git`
Kubernetes docs https://kubernetes.io 

## Kubernetes Architecture

[TODO]

## Namespaces

Create a namespace e.g. `kubectl create namespace class`

## Pods

Create a pod manifest with the image `jenniferstrej/jen-nginx:1.1`

(Answer: https://github.com/Jenniferstrej/nginx-examples-kube/blob/1.1/jen-nginx-pod.yaml)

Create the pod on your Minikube cluster under the `class` namespace

(Answer: `kubectl apply -f jen-nginx-pod.yaml`)

## ReplicaSets

Try to delete the pod above. What happens?

(Answer: `kubectl delete pod jen-nginx-[podid]`. The pod is deleted and is not recreated.)

Create a ReplicaSet.

(Answer: )

