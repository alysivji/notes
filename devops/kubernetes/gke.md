# Google Kubernetes Engine

## Create Docker Image

```console
docker build -t myapp .
docker tag myapp gcr.io/netes-sandbox/myapp:blue
docker push gcr.io/netes-sandbox/myapp:blue
```

## Set up GKE

```console
gcloud config set project netes-sandbox
gcloud container clusters get-credentials cluster-1 --zone=us-central1-c
````

## Networking

```console
k port-forward nginx 8080:80
```
