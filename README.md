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
   - Describe
   - Rolling Update Strategies
6. Services
7. Port forward
8. Editing Deployments
   - Edit
   - Rollout
   - Scale Deployment up and Down

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

(Answer: `kubectl -n class delete pod jen-nginx-[podid]`. The pod is deleted and is not recreated.)

Create a ReplicaSet.

(Answer: Go to https://github.com/Jenniferstrej/nginx-examples-kube/blob/1.1/jen-nginx-rs.yaml and `kubectl -n class apply -f jen-nginx-rs.yaml`)

Try to delete the pod above. It should be recreated now.

## Deployments

Create a deployment of the image jenniferstrej/jen-nginx:1.1 with 3 replicas

(Answer: Create this file locally https://github.com/Jenniferstrej/nginx-examples-kube/blob/1.1/jen-nginx-deploy.yaml and run `kubectl -n class apply -f jen-nginx-rs.yaml`)

__Rolling Update Strategies__

Run `kubectl describe deployment jen-nginx`

Default rolling update strategy is "RollingUpdateStrategy:  25% max unavailable, 25% max surge". What does it mean?

(Answer: When updating my deployment the pods will be gradually replaced so my application stays available at all time. It may show both versions for a short period.)

See documentation of Rolling Update strategies https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment.

## Services

A pod is visible to the Cluster via its pod IP, however that information is not reliable for a long period of time. We need a way to have our pod (or more pods if part of a ReplicaSet) reacheable via a cluster wide IP.

Exercise: Expose jen-nginx pods on port 80.

(Answer: Create this file locally https://github.com/Jenniferstrej/nginx-examples-kube/blob/1.1/jen-nginx-svc.yaml and run `kubectl -n class apply -f jen-nginx-svc.yaml`)

## Port forward

You can use Port forwarding to access applications in a cluster. We will forward a local port to a port on the pod.
We know our pod exposes port 80. We will forward our local port 8080 to our jen-nginx pods.

Run `kubectl -n class port-forward service/jen-nginx 8080:80`

Open your browser on the address http://127.0.0.1:8080/

You should see our welcome page.

## Editing Deployments 

Now that we have some user level visibility of our deployed application, let's deploy a new version of our app, using the docker image `jenniferstrej/jen-nginx:1.2`

Edit jen-nginx deployment with the image `jenniferstrej/jen-nginx:1.2`

(Answer: edit the `jen-nginx-deploy.yaml` and update the field image with the new version, then run `kubectl -n class apply -f jen-nginx-deploy.yaml` OR (imperative) `kubectl -n class edit deployment jen-nginx` and interactivelly edit the field image then save the file)

Run `kubectl rollout status deployment/jen-nginx`

You should see something like:

```
Waiting for deployment "jen-nginx" rollout to finish: 1 out of 3 new replicas have been updated...
Waiting for deployment "jen-nginx" rollout to finish: 1 out of 3 new replicas have been updated...
Waiting for deployment "jen-nginx" rollout to finish: 1 out of 3 new replicas have been updated...
Waiting for deployment "jen-nginx" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "jen-nginx" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "jen-nginx" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "jen-nginx" rollout to finish: 1 old replicas are pending termination...
Waiting for deployment "jen-nginx" rollout to finish: 1 old replicas are pending termination...
deployment "jen-nginx" successfully rolled out
```

Reload the app on the browser (if you stopped port forwarding run the command again so you can see your app on the web page)

You should see a background color change on the page.


__Rolling back a deployment__

Run `kubectl rollout history deployment/jen-nginx`

Observe your previous deployment revision
```
REVISION  CHANGE-CAUSE
[Number]        <none>
```

Rollback this deployment to the previous version

`kubectl rollout undo deployment/jen-nginx`

Check the status: `kubectl rollout status deployment/jen-nginx`

Reload your web page. It should display the page with a white background again.

__Scale Deployment__

Depending on the needs of your app (e.g. redundancy, resource needs, etc.) you may want to scale it down or up. Currently our deployment is running 3 pods. Let's reduce the number to 2 pods.

Exercise: Scale deployment jen-nginx to 2 pods.

(Answer: 
1. `kubectl -n class scale deployment jen-nginx --replicas=2`
2. Edit `jen-nginx-deploy.yaml` file and set the field `replicas` to 2, save and run `kubectl apply -f jen-nginx-deploy.yaml`)

## ConfigMaps

[TODO]

## Secrets

[TODO]

