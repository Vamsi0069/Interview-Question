🔹 Can you describe a recent issue you troubleshooted in Kubernetes?

Example: Pod crash-looping due to incorrect image tag → checked logs with kubectl logs, fixed the image in deployment YAML.

🔹 What Kubernetes services are you using in your project and what is their purpose?

ClusterIP: Internal communication

NodePort: External access via static port

LoadBalancer: Public access via cloud provider's LB

Ingress: Route HTTP/HTTPS traffic with path-based routing

🔹 Can multiple pods run under a single deployment in Kubernetes?

No. A Deployment manages a set of replicas of the same Pod template. If you need different containers, use a Pod with multiple containers or multiple Deployments.
