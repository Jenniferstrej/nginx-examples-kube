# nginx-examples-kube
Some container examples for a kubernetes workshop


# Version 1.1

Basic Welcome to nginx page. No authentication required.

# Ways to start it with Kubernetes

## Declarative

Create/Update a deployment from the deployment manifest file `jen-nginx-deploy.yaml`:

`kubectl -n class apply -f jen-nginx-deploy.yaml`

Create a service for the pod above:

`kubectl -n class apply -f jen-nginx-svc.yaml`

Port forward to access service locally:

`kubectl port-forward service/jen-nginx 8080:80`

Go on your browser in the address 127.0.0.1:8080


