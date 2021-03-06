# Getting Started With Kubeadm.

## Step 1 - Initialise Master.

### Kubeadm Init command.

```
master $ kubeadm init --token=102952.1a7dd4cc8d1f4cc5 --kubernetes-version v1.8.0
[kubeadm] WARNING: kubeadm is in beta, please do not use it for production clusters.
[init] Using Kubernetes version: v1.8.0
[init] Using Authorization modes: [Node RBAC][preflight] Running pre-flight checks
[preflight] Starting the kubelet service[kubeadm] WARNING: starting in 1.8, tokens expire after 24 hours by default (if you require a non-expiring token use --token-ttl 0)[certificates] Generated ca certificate and key.
[certificates] Generated apiserver certificate and key.
[certificates] apiserver serving cert is signed for DNS names [master kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 172.17.0.148][certificates] Generated apiserver-kubelet-client certificate and key.
[certificates] Generated sa key and public key.[certificates] Generated front-proxy-ca certificate and key.
[certificates] Generated front-proxy-client certificate and key.
[certificates] Valid certificates and keys now exist in "/etc/kubernetes/pki"
[kubeconfig] Wrote KubeConfig file to disk: "admin.conf"
[kubeconfig] Wrote KubeConfig file to disk: "kubelet.conf"
[kubeconfig] Wrote KubeConfig file to disk: "controller-manager.conf"
[kubeconfig] Wrote KubeConfig file to disk: "scheduler.conf"
[controlplane] Wrote Static Pod manifest for component kube-apiserver to "/etc/kubernetes/manifests/kube-apiserver.yaml"
[controlplane] Wrote Static Pod manifest for component kube-controller-manager to "/etc/kubernetes/manifests/kube-controller-manager.yaml"
[controlplane] Wrote Static Pod manifest for component kube-scheduler to "/etc/kubernetes/manifests/kube-scheduler.yaml"
[etcd] Wrote Static Pod manifest for a local etcd instance to "/etc/kubernetes/manifests/etcd.yaml"
[init] Waiting for the kubelet to boot up the control plane as Static Pods from directory "/etc/kubernetes/manifests"
[init] This often takes around a minute; or longer if the control plane images have to be pulled.
[apiclient] All control plane components are healthy after 28.502422 seconds
[uploadconfig] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[markmaster] Will mark node master as master by adding a label and a taint
[markmaster] Master master tainted and labelled with key/value: node-role.kubernetes.io/master=""
[bootstraptoken] Using token: 102952.1a7dd4cc8d1f4cc5
[bootstraptoken] Configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstraptoken] Configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstraptoken] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[addons] Applied essential addon: kube-dns
[addons] Applied essential addon: kube-proxy

Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run (as a regular user):

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  http://kubernetes.io/docs/admin/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join --token 102952.1a7dd4cc8d1f4cc5 172.17.0.148:6443 --discovery-token-ca-cert-hash sha256:a2108586e5280061ed8bcc90ff1e2178e349687322ac9aa5ca13617b60a6602c
```

## Step 2 - Join Cluster.

### Kubeadm Token command.

```
master $ kubeadm token list
TOKEN                     TTL       EXPIRES                USAGES                   DESCRIPTION                      EXTRA GROUPS
102952.1a7dd4cc8d1f4cc5   23h       2018-02-08T14:24:47Z   authentication,signing   The default bootstrap token generated by 'kubeadm init'.   system:bootstrappers:kubeadm:default-node-token
```

### Kubeadm Join command.

```
node01 $ kubeadm join --token=102952.1a7dd4cc8d1f4cc5 172.17.0.148:6443
[kubeadm] WARNING: kubeadm is in beta, please do not use it for production clusters.
[preflight] Running pre-flight checks
[preflight] Starting the kubelet service
[validation] WARNING: using token-based discovery without DiscoveryTokenCACertHashes can be unsafe (see https://kubernetes.io/docs/admin/kubeadm/#kubeadm-join).
[validation] WARNING: Pass --discovery-token-unsafe-skip-ca-verification to disable this warning. This warning will become an error in Kubernetes 1.9.
[discovery] Trying to connect to API Server "172.17.0.148:6443"
[discovery] Created cluster-info discovery client, requesting info from "https://172.17.0.148:6443"
[discovery] Cluster info signature and contents are valid and no TLS pinning was specified, will use API Server "172.17.0.148:6443"
[discovery] Successfully established connection with API Server "172.17.0.148:6443"
[bootstrap] Detected server version: v1.8.0
[bootstrap] The server supports the Certificates API (certificates.k8s.io/v1beta1)

Node join complete:
* Certificate signing request sent to master and response
  received.
* Kubelet informed of new secure connection details.

Run 'kubectl get nodes' on the master to see this machine join.
```

## Step 3 - View Nodes.

### Copy required files in master to home folder.

```
master $ sudo cp /etc/kubernetes/admin.conf $HOME/
```

```
master $ sudo chown $(id -u):$(id -g) $HOME/admin.conf
```

```
master $ export KUBECONFIG=$HOME/admin.conf
```

###  Kubectl Get command.

```
master $ kubectl get nodes
NAME      STATUS     ROLES     AGE       VERSION
master    NotReady   master    10m       v1.8.0
node01    NotReady   <none>    3m        v1.8.5
```

## Step 4 - Deploy Container Networking Interface (CNI).

### Weave definition file.

```
master $ cat /opt/weave-kube
apiVersion: v1kind: List
items:
  - apiVersion: v1
    kind: ServiceAccount    metadata:
      name: weave-net      labels:
        name: weave-net
      namespace: kube-system
  - apiVersion: rbac.authorization.k8s.io/v1beta1
    kind: ClusterRole
    metadata:
      name: weave-net
      labels:
        name: weave-net
    rules:
      - apiGroups:
          - ''
        resources:
          - pods
          - namespaces
          - nodes
        verbs:
          - get
          - list
          - watch
      - apiGroups:
          - extensions
        resources:
          - networkpolicies
        verbs:
          - get
          - list
          - watch
  - apiVersion: rbac.authorization.k8s.io/v1beta1
    kind: ClusterRoleBinding
    metadata:
      name: weave-net
      labels:
        name: weave-net
    roleRef:
      kind: ClusterRole
      name: weave-net
      apiGroup: rbac.authorization.k8s.io
    subjects:
      - kind: ServiceAccount
        name: weave-net
        namespace: kube-system
  - apiVersion: extensions/v1beta1
    kind: DaemonSet
    metadata:
      name: weave-net
      labels:
        name: weave-net
      namespace: kube-system
    spec:
      template:
        metadata:
          labels:
            name: weave-net
        spec:
          containers:
            - name: weave
              command:
                - /home/weave/launch.sh
              env:
                - name: HOSTNAME
                  valueFrom:
                    fieldRef:
                      apiVersion: v1
                      fieldPath: spec.nodeName
              image: 'weaveworks/weave-kube:2.0.4'
              livenessProbe:
                httpGet:
                  host: 127.0.0.1
                  path: /status
                  port: 6784
                initialDelaySeconds: 30
              resources:
                requests:
                  cpu: 10m
              securityContext:
                privileged: true
              volumeMounts:
                - name: weavedb
                  mountPath: /weavedb
                - name: cni-bin
                  mountPath: /host/opt
                - name: cni-bin2
                  mountPath: /host/home
                - name: cni-conf
                  mountPath: /host/etc
                - name: dbus
                  mountPath: /host/var/lib/dbus
                - name: lib-modules
                  mountPath: /lib/modules
                - name: xtables-lock
                  mountPath: /run/xtables.lock
                  readOnly: false
            - name: weave-npc
              env:
                - name: HOSTNAME
                  valueFrom:
                    fieldRef:
                      apiVersion: v1
                      fieldPath: spec.nodeName
              image: 'weaveworks/weave-npc:2.0.4'
              resources:
                requests:
                  cpu: 10m
              securityContext:
                privileged: true
              volumeMounts:
                - name: xtables-lock
                  mountPath: /run/xtables.lock
                  readOnly: false
          hostNetwork: true
          hostPID: true
          restartPolicy: Always
          securityContext:
            seLinuxOptions: {}
          serviceAccountName: weave-net
          tolerations:
            - effect: NoSchedule
              operator: Exists
          volumes:
            - name: weavedb
              hostPath:
                path: /var/lib/weave
            - name: cni-bin
              hostPath:
                path: /opt
            - name: cni-bin2
              hostPath:
                path: /home
            - name: cni-conf
              hostPath:
                path: /etc
            - name: dbus
              hostPath:
                path: /var/lib/dbus
            - name: lib-modules
              hostPath:
                path: /lib/modules
            - name: xtables-lock
              hostPath:
                path: /run/xtables.lock
      updateStrategy:
        type: RollingUpdate
```

### Kubectl Apply command.

```
master $ kubectl apply -f /opt/weave-kube
serviceaccount "weave-net" created
clusterrole "weave-net" created
clusterrolebinding "weave-net" created
daemonset "weave-net" created
```

### KubeCtl Get command.

```
master $ kubectl get pod -n kube-system
NAME                             READY     STATUS    RESTARTS   AGE
etcd-master                      1/1       Running   0          7m
kube-apiserver-master            1/1       Running   0          7m
kube-controller-manager-master   1/1       Running   0          7m
kube-dns-545bc4bfd4-dzbtk        3/3       Running   0          14m
kube-proxy-6qbvk                 1/1       Running   0          14m
kube-proxy-ljt6f                 1/1       Running   0          7m
kube-scheduler-master            1/1       Running   0          7m
weave-net-d9nj2                  2/2       Running   0          42s
weave-net-kd949                  2/2       Running   0          42s
```

## Step 5 - Deploy Pod.

### KubeCtl Run command.

```
master $ kubectl run http --image=katacoda/docker-http-server:latest --replicas=1
deployment "http" created
```


### Docker Ps command.

```
node01 $ docker ps | grep docker-http-server
6beae43e54c7        katacoda/docker-http-server@sha256:76dc8a47fd019f80f2a3163aba789faf55b41b2fb06397653610c754cb12d3ee                          "/app"                   6 seconds ago       Up 5 seconds                            k8s_http_http-9f4ff8f47-5tk9z_default_bb6ade30-0c15-11e8-8fb8-0242ac110029_0
```

## Step 7 - Deploy Dashboard.

### Dashboard definition file.

```
master $ cat dashboard.yaml
# Copyright 2015 Google Inc. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Configuration to deploy release version of the Dashboard UI compatible with
# Kubernetes 1.6 (RBAC enabled).
#
# Example usage: kubectl create -f <this_file>

apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: kubernetes-dashboard
  labels:
    k8s-app: kubernetes-dashboard
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: kubernetes-dashboard
  namespace: kube-system
---
kind: Deployment
apiVersion: extensions/v1beta1
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kube-system
spec:
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      k8s-app: kubernetes-dashboard
  template:
    metadata:
      labels:
        k8s-app: kubernetes-dashboard
    spec:
      containers:
      - name: kubernetes-dashboard
        image: gcr.io/google_containers/kubernetes-dashboard-amd64:v1.6.0
        ports:
        - containerPort: 9090
          protocol: TCP
        args:
          # Uncomment the following line to manually specify Kubernetes API server Host
          # If not specified, Dashboard will attempt to auto discover the API server and connect
          # to it. Uncomment only if the default does not work.
          # - --apiserver-host=http://my-address:port
        livenessProbe:
          httpGet:
            path: /
            port: 9090
          initialDelaySeconds: 30
          timeoutSeconds: 30
      serviceAccountName: kubernetes-dashboard
      # Comment the following tolerations if Dashboard must not be deployed on master
      tolerations:
      - key: node-role.kubernetes.io/master
        effect: NoSchedule
---
kind: Service
apiVersion: v1
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kube-system
spec:
  type: NodePort
  ports:
  - port: 9090
    nodePort: 30000
  selector:
    k8s-app: kubernetes-dashboard
```

### Kubectl Apply command.

```
master $ kubectl apply -f dashboard.yaml
```

### Kubectl Get command.

```
master $ kubectl get pods -n kube-system
NAME                                   READY     STATUS    RESTARTS   AGE
etcd-master                            1/1       Running   0          9m
kube-apiserver-master                  1/1       Running   0          9m
kube-controller-manager-master         1/1       Running   0          9m
kube-dns-545bc4bfd4-k4kx8              3/3       Running   0          10m
kube-proxy-8tzh9                       1/1       Running   0          9m
kube-proxy-tbcg6                       1/1       Running   0          10m
kube-scheduler-master                  1/1       Running   0          9m
kubernetes-dashboard-b88d56789-tlmfw   1/1       Running   0          58s
weave-net-pl2kr                        2/2       Running   0          8m
weave-net-s2dnf                        2/2       Running   0          8m
```
