# nginx-examples-kube
Some container examples for a kubernetes workshop

# Version 1.0

Docker image with hardcoded .htpasswd file to be used as test locally only. nginx default.conf set up for basic authentication.

# Ways to start it with Kubernetes

## Declarative

First create a namespace for you to work on:

`kubectl create namespace class`

Then create a deployment from the deployment manifest file `jen-nginx-deploy.yaml`:

`kubectl -n class apply -f jen-nginx-deploy.yaml`

Create a service for the pod above:

`kubectl -n class apply -f jen-nginx-svc.yaml`

Port forward to access service locally:

`kubectl port-forward service/jen-nginx 8080:80`

Go on your browser in the address 127.0.0.1:8080

You will be prompted for a username and password:

User: jenny

Password: adifficultpass




# Shortcut (to be deprecated)
Create deployment:

`kubectl run kubectl run jen-nginx --image=jen-nginx:1.0`

Expose deployment:

`kubectl expose deploy jen-nginx --port=80`

