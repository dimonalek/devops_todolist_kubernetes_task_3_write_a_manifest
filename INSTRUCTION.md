# ToDo App Kubernetes Instructions

## Build and push image

Replace your Docker Hub username and run:

```bash
docker build -t <yourname>/todoapp:3.0.0 .
docker login
docker push <yourname>/todoapp:3.0.0
```

Then update the image in `.infrastructure/todoapp-pod.yml` if needed.

## Apply manifests

Run the following commands from the project root:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/busybox.yml
```

Check resources:

```bash
kubectl get namespaces
kubectl get pods -n todoapp
```

## Test ToDo app using port-forward

Forward local port 8000 to the pod:

```bash
kubectl port-forward -n todoapp pod/todoapp 8000:8000
```

In another terminal, test endpoints:

```bash
curl http://127.0.0.1:8000/api/readiness/
curl http://127.0.0.1:8000/api/liveness/
```

## Test ToDo app using busyboxplus:curl container

Get the ToDo pod IP and run curl commands from inside the busybox pod:

```bash
kubectl get pod todoapp -n todoapp -o wide
kubectl exec -n todoapp busybox-curl -- curl -s http://<TODOAPP_POD_IP>:8000/api/readiness/
kubectl exec -n todoapp busybox-curl -- curl -s http://<TODOAPP_POD_IP>:8000/api/liveness/
```
