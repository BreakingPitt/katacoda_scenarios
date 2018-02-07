# Launch Single Node Kubernetes Cluster.

## Step 1 - Start Minikube.

### Minikube Version command.

```
$ minikube version
minikube version: v0.17.1-katacoda
```

### Minikube Start command.

```
$ minikube start
Starting local Kubernetes cluster...
```

## Step 2 - Cluster Info.

### KubeCtl Cluster Info command.

```
$ kubectl cluster-info
Kubernetes master is running at http://host01:8080
heapster is running at http://host01:8080/api/v1/namespaces/kube-system/services/heapster/proxy
kubernetes-dashboard is running at http://host01:8080/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy
monitoring-grafana is running at http://host01:8080/api/v1/namespaces/kube-system/services/monitoring-grafana/proxy
monitoring-influxdb is running at http://host01:8080/api/v1/namespaces/kube-system/services/monitoring-influxdb/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

### KubeCtl Get Nodes command.

```
$ kubectl get nodes
NAME      STATUS    ROLES     AGE       VERSION
host01    Ready     <none>    2m        v1.5.2
```

## Step 3 - Deploy Containers.

### KubeCtl Run command.
```
$ kubectl run first-deployment --image=katacoda/docker-httpserver --port=80
deployment "first-deployment" created
```

### KubeCtl Get command.

```
$ kubectl get pods
NAME                               READY     STATUS             RESTARTS   AGE
first-deployment-1614400005-9n1gl   0/1       ImagePullBackOff   0          25s
```

### KubeCtl Expose command.

```
$ kubectl expose deployment first-deployment --port=80 --type=NodePort
service "fist-deployment" exposed
```

### KubeCtl Get command.

```
$ export PORT=$(kubectl get svc first-deployment -o go-template='{{range.spec.ports}}{{if .nodePort}}{{.nodePort}}{{"\n"}}{{end}}{{end}}')
```

```
$ curl host01:$PORT
<h1>This request was processed by host: first-deployment-3374828310-vhcb9</h1
```

## Step 4 - Dashboard

### Curl UI fom terminal.

```
$ curl https://2886795290-8080-frugo01.environments.katacoda.com/ui/
<a href="/api/v1/proxy/namespaces/kube-system/services/kubernetes-dashboard">Temporary Redirect</a>.
```
