---
title: "Kubernetes 101"
date: 2026-01-29
draft: false
weight: 9
summary: "Run and scale containers in production with Kubernetes — pods, deployments, services, and your first kubectl commands."
description: "A beginner's introduction to Kubernetes container orchestration."
tags: ["kubernetes", "containers", "orchestration"]
categories: ["Containers"]
series: ["DevOps Roadmap"]
ShowToc: true
TocOpen: true
---

One Docker container is easy. But what happens when you have 50 of them, across many machines, that need to stay healthy, scale up under load, and recover from crashes? That's the job of **Kubernetes** (often shortened to **K8s**).

## What Kubernetes does for you

- **Scheduling** — decides which machine runs each container.
- **Self-healing** — restarts containers that crash.
- **Scaling** — adds or removes copies based on demand.
- **Load balancing** — spreads traffic across your containers.
- **Rollouts & rollbacks** — ship updates safely.

## Practice locally first

You don't need a cloud cluster to learn. Install [Minikube](https://minikube.sigs.k8s.io/docs/start/) or [kind](https://kind.sigs.k8s.io/) to run Kubernetes on your laptop, plus `kubectl`, the command-line tool.

```bash
minikube start
kubectl version --client
```

## The key building blocks

| Object | What it is |
|--------|-----------|
| **Pod** | The smallest unit — wraps one (or a few) containers |
| **Deployment** | Manages a set of identical pods and their updates |
| **Service** | A stable network endpoint to reach your pods |
| **Namespace** | A way to group and isolate resources |

A useful mental model: a **Deployment** keeps the right number of **Pods** running, and a **Service** gives them a fixed address.

## Your first deployment

Create `deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

Apply it:

```bash
kubectl apply -f deployment.yaml
kubectl get pods        # you should see 3 pods
kubectl get deployments
```

## Exposing it with a Service

Create `service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
```

```bash
kubectl apply -f service.yaml
minikube service nginx-service   # opens it in your browser
```

## Essential kubectl commands

```bash
kubectl get pods                 # list pods
kubectl get all                  # list everything
kubectl describe pod <name>      # detailed info
kubectl logs <pod>               # view logs
kubectl exec -it <pod> -- bash   # shell into a pod
kubectl delete -f deployment.yaml
```

## Scaling and self-healing in action

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

Now try deleting a pod, Kubernetes immediately creates a replacement to maintain the desired count. That's self-healing:

```bash
kubectl delete pod <pod-name>
kubectl get pods   # a new one is already spinning up
```

## Rolling updates

Change the image version and Kubernetes updates pods gradually, with zero downtime:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.25
kubectl rollout status deployment/nginx-deployment
kubectl rollout undo deployment/nginx-deployment   # roll back if needed
```

## Don't be intimidated

Kubernetes is huge, and nobody learns it all at once. Master pods, deployments, and services first. Everything else builds on those three.

## Practice challenge 🏋️

1. Deploy the Nginx example above with 3 replicas.
2. Scale it to 6.
3. Delete a pod and watch Kubernetes recreate it.
4. Clean up with `kubectl delete`.

## What's next?

Your apps are running and scaling. The final piece: knowing what's actually happening inside them.

👉 Next up: [Monitoring with Prometheus & Grafana](/posts/monitoring-prometheus-grafana/)
