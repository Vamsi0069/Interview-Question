###  *1. Application Downtime*

* *Issue*: Site or service becomes unavailable.
* *Cause*: Server crash, bad deployment, DNS failure, etc.
* *Fix*:

  * Check health checks (/health, /status).
  * Use load balancer failover.
  * Roll back deployment quickly using CI/CD.



###  *2. Disk Space Full*

* *Issue*: App crashes or services stop.
* *Cause*: Logs, images, backups filling /var, /tmp, or /.
* *Fix*:

  * Clear old logs: sudo journalctl --vacuum-time=3d
  * Clean Docker: docker system prune
  * Setup alerts for disk usage.



###  *3. High CPU or Memory Usage*

* *Issue*: Server becomes unresponsive.
* *Cause*: Infinite loops, memory leaks, heavy DB queries.
* *Fix*:

  * top / htop to identify process.
  * Restart or scale up/down pods (Kubernetes).
  * Use monitoring tools: CloudWatch, Prometheus.



###  *4. Expired SSL Certificate*

* *Issue*: HTTPS stops working, users see security warnings.
* *Fix*:

  * Renew using Let's Encrypt: certbot renew
  * Automate renewal with cron.
  * Use Route53 + ACM in AWS for auto-renew.



###  *5. CI/CD Pipeline Failures*

* *Issue*: Code changes aren’t deployed.
* *Cause*: Git conflicts, failed tests, broken scripts.
* *Fix*:

  * Review pipeline logs in Jenkins/GitHub Actions/GitLab.
  * Fix test or script errors.
  * Re-trigger build after fixing.



###  *6. DNS Propagation Delays*

* *Issue*: Users can't access site after migration.
* *Cause*: DNS TTL too high or misconfigured records.
* *Fix*:

  * Use low TTL during migration.
  * Verify with nslookup, dig.



###  *7. Misconfigured Load Balancer*

* *Issue*: Traffic not routing properly.
* *Fix*:

  * Check health check config.
  * Review target group settings.
  * Confirm correct listener rules.



###  *8. IAM Misconfiguration (AWS/Azure)*

* *Issue*: “Access Denied” errors for developers or automation.
* *Fix*:

  * Review IAM policies.
  * Use least privilege principle.
  * Use sts decode-authorization-message (AWS) to debug.



###  *9. Docker Container CrashLoop*

* *Issue*: Pod/Container restarts continuously.
* *Fix*:

  * Check logs: docker logs <container> or kubectl logs.
  * Check image tag, health checks, volume paths.
  * Add proper readiness & liveness probes.



###  *10. Monitoring Alert Storms*

* *Issue*: 100s of alerts flood after small incident.
* *Fix*:

  * Implement alert grouping (like in Prometheus Alertmanager).
  * Use thresholds with time windows.
  * Tune noise-making alerts.


### 11. *Server Crash or Kernel Panic*

* *Cause*: High load, faulty drivers, hardware issues.
* *Action*:

  * Analyze logs (/var/log/syslog, dmesg).
  * Use monitoring alerts to auto-reboot or failover.



### 12. *Database Connection Timeout*

* *Cause*: Too many connections, slow queries, network issues.
* *Fix*:

  * Optimize queries & indexing.
  * Tune DB connection pool.
  * Use RDS/Aurora metrics & auto-scaling.



### 13. *Secrets/Keys Exposed in Code*

* *Issue*: API keys or passwords pushed to GitHub.
* *Fix*:

  * Revoke and rotate secrets.
  * Use tools like AWS Secrets Manager, HashiCorp Vault.
  * Scan commits using git-secrets or TruffleHog.



### 14. *Incorrect Infrastructure Provisioning (Terraform mistakes)*

* *Cause*: Wrong instance type, public IP enabled, missing tags.
* *Fix*:

  * Review Terraform plan before apply.
  * Use terraform state to inspect issues.
  * Maintain modules and version control.



### 15. *Container Image Bloat*

* *Issue*: Build takes too long, huge images (>1GB).
* *Fix*:

  * Use multi-stage builds.
  * Use alpine or slim base images.
  * Clean up unnecessary files in Dockerfile.



### 16. *Improper Load Balancer Routing*

* *Issue*: Requests hitting wrong targets.
* *Fix*:

  * Verify listener rules, health checks.
  * Ensure target groups are healthy.
  * Use AWS ELB/ALB access logs for debugging.



### 17. *Kubernetes Pod CrashLoopBackOff*

* *Cause*: App crashes on start due to config/env issues.
* *Fix*:

  * Run kubectl logs <pod> and kubectl describe pod.
  * Use readiness/liveness probes correctly.
  * Check resource limits (CPU/Memory).



### 18. *High Latency in Services*

* *Cause*: Network hops, unoptimized backend, throttling.
* *Fix*:

  * Use tracing (OpenTelemetry, AWS X-Ray, Jaeger).
  * Monitor with Prometheus + Grafana.
  * Optimize API response time and DB calls.



### 19. *CI/CD Pipeline Hangs or Is Too Slow*

* *Cause*: Long tests, large artifacts, misconfigured agents.
* *Fix*:

  * Use caching (e.g., Docker layer cache, NPM cache).
  * Split pipelines into stages (build/test/deploy).
  * Parallelize jobs and use runners wisely.



### 20. *Incorrect Auto-scaling Behavior*

* *Issue*: Pods/instances not scaling when needed.
* *Cause*: Metrics not collected properly or thresholds too high.
* *Fix*:

  * Use proper CPU/memory requests & limits.
  * Verify HPA (Horizontal Pod Autoscaler) setup.
  * Ensure CloudWatch/Prometheus metrics are accurate.



### 21. *Data Loss During Deployment*

* *Cause*: Volume deletion, wrong rm -rf, or dropped DB.
* *Fix*:

  * Always use backups (EBS snapshots, S3 versioning).
  * Practice disaster recovery (RTO, RPO).
  * Avoid destructive scripts in CI/CD.



### 22. *Log Flooding or No Logs*

* *Issue*: Either too many logs or missing logs.
* *Fix*:

  * Use centralized logging (ELK, EFK, CloudWatch Logs).
  * Implement log rotation.
  * Adjust log levels in prod (info/warn vs debug).



### 23. *DNS Misconfiguration*

* *Issue*: Domain not resolving.
* *Fix*:

  * Check with dig, nslookup.
  * Validate Route53 or DNS provider settings.
  * Check CNAME, A records, TTL, etc.



### 24. *Time Sync Issues*

* *Issue*: JWT expiry mismatch, logs out of order.
* *Fix*:

  * Sync all servers with NTP.
  * Use tools like Chrony or ntpd.



### 25. *Wrong Artifact Deployed*

* *Cause*: Tag mismatch, stale cache, build errors.
* *Fix*:

  * Use Git SHA or version in artifacts.
  * Lock dependencies in package-lock.json or requirements.txt.



### 26. *Insecure Security Group/Firewall Rules*

* *Issue*: Ports like 22, 3306 open to all.
* *Fix*:

  * Use least privilege and restrict to known IPs.
  * Regularly audit using tools like AWS Trusted Advisor.



### 27. *Storage Volume Not Mounting (EBS/EFS)*

* *Issue*: App fails due to missing persistent volume.
* *Fix*:

  * Check if volume is attached and mounted.
  * Review volume permissions and mount path in Kubernetes.



### 28. *IAM Role/Policy Misassignment*

* *Issue*: Lambda/S3/EC2 can't access services.
* *Fix*:

  * Verify sts:AssumeRole policies.
  * Use IAM policy simulator to debug.
