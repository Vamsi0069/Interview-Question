
### 1. What is Kubernetes?

Answer:
Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. It helps manage clusters of containers in a reliable and scalable way.



### 2. Write a Kubernetes Deployment manifest to create an Nginx pod with 3 replicas.

Answer:

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



### 3. Write a Kubernetes ConfigMap YAML to store database configuration with keys `DB_HOST=db.example.com` and `DB_PORT=5432`.

Answer:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: db-config
data:
  DB_HOST: db.example.com
  DB_PORT: "5432"
```



### 4. What is HPA (Horizontal Pod Autoscaler)?

Answer:
HPA automatically scales the number of pod replicas in a deployment or replica set based on observed CPU utilization or other custom metrics.



### 5. How do you scale up the replicas in a deployment using a command?

Answer:

```bash
kubectl scale deployment nginx-deployment --replicas=5
```



### 6. How to copy files from the host to a container using a command?

Answer:

```bash
kubectl cp /path/on/host <pod-name>:/path/in/container
```



### 7. How to restart a pod without stopping the deployment?

Answer:

```bash
kubectl rollout restart deployment <deployment-name>
```



### 8. How do pods communicate with each other?

Answer:
Pods communicate using internal IPs assigned by the Kubernetes network. Services provide stable DNS names and load balancing to enable communication between pods.



### 9. What are the security best practices for a Kubernetes cluster?

Answer:

* Use RBAC to control access
* Run containers as non-root
* Use Network Policies
* Store sensitive data in Secrets
* Use pod security standards
* Enable auditing and logging
* Keep components up-to-date



### 10. How do you ensure the application is highly available and scalable?

Answer:

* Use Deployments with multiple replicas
* Distribute pods across multiple nodes
* Use HPA to autoscale based on load
* Configure readiness and liveness probes
* Use LoadBalancers or Ingress Controllers



### 11. How do you troubleshoot a pod in a Pending state?

Answer:

* Run:

  ```bash
  kubectl describe pod <pod-name>
  ```
* Check for:

  * Resource limits on nodes
  * Taints and tolerations
  * PVC binding issues
  * NodeSelector/affinity misconfigurations



### 12. How do you create a Kubernetes cluster? Explain the steps.

Answer:

* Using Minikube (for local testing):

  ```bash
  minikube start
  ```
* Using `kubeadm` (for production):

  1. Install Docker, kubelet, kubeadm, kubectl
  2. Run `kubeadm init` on the master node
  3. Set up kubeconfig
  4. Install a CNI plugin like Calico
  5. Join worker nodes using the `kubeadm join` command



### 13. What critical issues have you faced with a Kubernetes cluster?

Answer:

* Pods stuck in Pending due to lack of resources
* CrashLoopBackOff due to bad image or config
* Network issues due to missing CNI plugin
* HPA not working due to missing metrics server
* Disk pressure evicting pods unexpectedly



### 14. What is an indentation error in a YAML file?

Answer:
Indentation errors happen when YAML syntax is not properly aligned. YAML uses spaces (not tabs) and expects consistent indentation. Misaligned sections cause parsing failures in Kubernetes.



### 15. How do you enter into a running pod or container in Kubernetes and Docker?

Answer:

* Kubernetes:

  ```bash
  kubectl exec -it <pod-name> -- /bin/bash
  ```

* Docker:

  ```bash
  docker exec -it <container-id> /bin/bash
  ```




####  Managing secrets?

* Use `kubectl create secret` or `Secrets` YAML.
* For encryption: enable secret encryption at rest + use tools like HashiCorp Vault.

####  Why ArgoCD?

* GitOps tool for declarative deployment.
* Auto-syncs cluster state with Git repo.

####  Using kubectl logs?

```bash
kubectl logs <pod-name> [-c container-name]  
```

####  Prometheus + Grafana Setup?

* Use `kube-prometheus-stack` Helm chart.
* Prometheus scrapes metrics; Grafana visualizes.

####  ReplicaSet vs Deployment?

* Deployment = ReplicaSet + rollout strategy.
* Use Deployment for updates and scaling.

####  Canary vs Rolling?

* Canary: Route traffic partially to new version.
* Rolling: Replace pods gradually.

####  Blue-Green Deployment?

* Run 2 identical environments (blue & green).
* Switch traffic to green when ready.

####  StatefulSet vs DaemonSet?

* StatefulSet: stable identities, persistent storage.
* DaemonSet: one pod per node (e.g., logs, monitoring).

####  Attach PV to Deployment?

* Yes, using `PersistentVolumeClaim` in pod spec.

####  Init vs Sidecar containers?

* Init: Runs before app starts (e.g., setup).
* Sidecar: Runs alongside (e.g., logging agent).

####  Create PV?

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /data
```

## Kubernetes (EKS) Troubleshooting

####   You are managing EKS and all pods go offline.Where do you check logs?
kubectl get pods --all-namespaces → Check pod status

kubectl logs <pod-name> → View pod logs

kubectl describe node/pod → Check for scheduling or node issues

CloudWatch Logs → Centralized logging (if integrated)

Check Cluster Autoscaler and node group health

VPC CNI Plugin logs (for network-level issues)

####   Someone accidentally deleted IPs in EKS — where can you trace it in AWS?
Use CloudTrail: Filter logs for DeleteNetworkInterface, DetachNetworkInterface, or eks.amazonaws.com activities.

Look for source IP, IAM user, and time of action.

Also check VPC → ENI logs and EC2 Network Interfaces.

####   What can you check when the API server is down, but the EC2 nodes are running?
kubectl is not responding → Check if the EKS control plane is healthy

Go to EKS Console → Cluster status

Check VPC endpoint reachability or if the VPC/subnet/network ACLs allow control plane communication

Check for IAM role misconfigurations

Use CloudWatch Logs or aws eks describe-cluster to troubleshoot


####   Can you describe a recent issue you troubleshooted in Kubernetes?

Example: Pod crash-looping due to incorrect image tag → checked logs with kubectl logs, fixed the image in deployment YAML.

####   What Kubernetes services are you using in your project and what is their purpose?

ClusterIP: Internal communication

NodePort: External access via static port

LoadBalancer: Public access via cloud provider's LB

Ingress: Route HTTP/HTTPS traffic with path-based routing

####   Can multiple pods run under a single deployment in Kubernetes?

No

####  A Deployment manages a set of replicas of the same Pod template

####  If you need different containers, use a Pod with multiple containers or multiple Deployments.
