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
kubectl logs --tail=0 <pod>
```

## Publisher

Find out public queue socket in cluster for building NATS URL. Since `nats_url` is configured in the config file, we have to override it using the appropriate environment variable.

```bash
cluster_ip=$(minikube ip)
node_port=$(kubectl get services/eventhandler-queue -o go-template='{{(index .spec.ports 0).nodePort}}')
nats_url="nats://$cluster_ip:$node_port"
```

### Native binary

```bash
# git clone, go build etc.
export NATS_URL=$nats_url
export RECIPIENT=<subscriber pod>
./eventhandler publish --payload='{"check_name": "check_foo"}'
```

### Docker "binary"

Build (against your _local_ Docker daemon).

```bash
# once
for role in base publisher; do
    docker build -t eventhandler/$role docker/$role
done
```

And run. CLI args following the container name are passed through to the publisher.

```bash
docker run -e NATS_URL=$nats_url eventhandler/publisher --payload='{"check_name": "check_foo"}'
```
