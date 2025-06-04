
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
