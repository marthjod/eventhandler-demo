# eventhandler-demo

## Queue and subscriber(s)

Build against minikube cluster Docker daemon.

```bash
eval $(minikube docker-env)
# once
for role in queue base subscriber; do
    docker build -t eventhandler/$role docker/$role
done
```

Run inside the minikube cluster.

```bash
kubectl create -f cluster.yaml
# or, later,
kubectl replace --force -f cluster.yaml
```

Inspect the components' behavior with, e.g.

```bash
kubectl logs <subscriber-pod>
```

## Publisher

Build (against your _local_ Docker daemon).

```bash
# once
for role in base publisher; do
    docker build -t eventhandler/$role docker/$role
done
```

Acquire public socket for queue in cluster.

```bash
cluster_ip=$(minikube ip)
node_port=$(kubectl get services/eventhandler-queue -o go-template='{{(index .spec.ports 0).nodePort}}')
nats_url="nats://$cluster_ip:$node_port"
```

And run. CLI args following the container name are passed through to the publisher.

```bash
docker run -e NATS_URL=$nats_url eventhandler/publisher --payload='{}'
```