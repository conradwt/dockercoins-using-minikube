# Getting Started With Kubernetes and Container Orchestration Notes

The purpose of this example is to provide instructions for running the Dockercoins sample app using Minikube.

## Software Requirements

- Docker For Mac 4.19.0 or newer

- Minikube 1.30.1 or newer

## Create Cluster

```zsh
minikube start --profile=dockercoins --nodes=3 --kubernetes-version=v1.27.2
```

Note: Servers represent the control plan nodes and agents represents the worker nodes. For additional information, please read [here](https://rancher.com/docs/k3s/latest/en/architecture).

## Create Necessary Environment Variables

```zsh
export REGISTRY=dockercoins
export TAG=v0.1
```

## Create Redis Deployment

```zsh
kubectl create deployment redis --image=redis
```

## Create Other Deployments

```zsh
for SERVICE in hasher rng webui worker; do
  kubectl create deployment $SERVICE --image=$REGISTRY/$SERVICE:$TAG
done
```

## Create Redis, Rng, And Hasher Services Using ClusterIP Type

```zsh
kubectl expose deployment redis --port 6379
kubectl expose deployment rng --port 80
kubectl expose deployment hasher --port 80
```

## Create Webui Service Using NodePort Type

```zsh
kubectl create service nodeport webui --node-port=30080 --tcp=8082:80
```

## Navigate To Webui Service In The Browser

```zsh
open http://localhost:8082
```

## Scaling The Worker Service

```zsh
kubectl scale deploy/worker --replicas=10
```

## Teardown Cluster

```zsh
minikube delete --profile=dockercoins
```

## References

- https://minikube.sigs.k8s.io/docs

- https://training.play-with-kubernetes.com/kubernetes-workshop

## Support

Bug reports and feature requests can be filed here:

- [File Bug Reports and Features](https://github.com/conradwt/dockercoins-using-minikube/issues)

## License

Dockercoins Using Minikube is released under the [MIT license](./LICENSE.md).

## Copyright

copyright:: (c) Copyright 2020 - 2023 Conrad Taylor. All Rights Reserved.
