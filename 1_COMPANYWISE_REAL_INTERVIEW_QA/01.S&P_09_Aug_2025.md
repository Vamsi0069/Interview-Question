
### **Jenkins:**

**1. How your build gets passed from UAT to PRD?**

* Through **pipeline promotion** using **`input` step** for manual approval or by using a **multi-branch pipeline** with stages (UAT → Staging → Prod).
* Artifacts built in UAT are stored in **Nexus/Artifactory** and reused in PRD to avoid rebuilding.

**2. How authentication is managed in Jenkins?**

* By **Jenkins own user database**, **LDAP**, **Active Directory**, or **OAuth tokens** (e.g., GitHub).
* Role-Based Access Control (RBAC) using plugins like **Role Strategy**.

**3. How to install custom plugins in Jenkins?**

* Manage Jenkins → Manage Plugins → Advanced → Upload `.hpi` or `.jpi` plugin file.

**4. Pipeline script with multiple stages taking a lot of time, how to reduce deployment time?**

* Use **parallel stages**, **shared libraries**, and **docker agents** to avoid environment setup for each stage.
* Cache dependencies (e.g., Maven, npm).

**5. How to execute stage parallel in same env?**

```groovy
stage('Parallel Tasks') {
  parallel {
    stage('Task 1') { steps { sh 'echo Task1' } }
    stage('Task 2') { steps { sh 'echo Task2' } }
  }
}
```

**6. Freestyle job modified, how to know who modified it?**

* Enable **Job Config History Plugin** or check `config.xml` history in Jenkins home.

**7. How to execute Jenkins job remotely?**

* Use **Jenkins API** or **`buildWithParameters`** endpoint with an API token:
  `curl -X POST http://user:token@jenkins/job/job_name/build`

**8. What is the use of post-build actions?**

* To trigger actions **after build success/failure** (e.g., email notifications, archiving artifacts, triggering downstream jobs).

**9. Can we trigger other jobs on 2 different instances after 1 job completion?**

* Yes, via **Parameterized Trigger Plugin**, **webhooks**, or **Jenkins API**.

**10. Where to store pipeline scripts?**

* Best practice: **`Jenkinsfile` in source code repository** (SCM).
* Alternatively, store in Jenkins shared libraries.

**11. Errors in Jenkins you faced?**

 **OutOfMemoryError**,
 **workspace locked**, 
 **plugin compatibility issues**, 
 **failed pipeline due to missing environment variables**.

**12. Automatic rollback approach for pipeline deployment?**

* Maintain a **stable artifact version** and in pipeline failure, run a **rollback stage** deploying previous artifact.
* Use **blue-green** or **canary** strategies.



### **Docker:**

**13. How to map a port to container?**

* `docker run -p host_port:container_port image`

**14. How to create image from running container?**

* `docker commit <container_id> new_image_name`

**15. Docker networks?**

* Types: **bridge**, **host**, **none**, **overlay**, **macvlan**.

**16. Can I create custom network in Docker?**

* Yes: `docker network create mynet`

**17. Can I assign a network to a running container?**

* Yes: `docker network connect mynet container_name`

**18. Host network in Docker?**

* Uses **host's network stack directly** (no NAT). Use `--network host`.

**19. Running container mounted to host dir — will data persist?**

* Yes, **data persists** on host directory even after container stops or is removed.

**20. What happens when you run `docker build` and `docker run`?**

* `docker build`: Creates image using Dockerfile.
* `docker run`: Creates and starts container from image.

**21. 2 containers communication?**

* Connect them to **same bridge or overlay network**.

**22. App on network A, DB on B — can they communicate?**

* No, unless you **connect networks or containers** using `docker network connect`.

**23. Create Docker image for K8s?**

* Write Dockerfile → `docker build -t app:1.0 .` → push to registry → reference in K8s deployment YAML.

**24. Health check in Docker?**

* In Dockerfile:
  `HEALTHCHECK CMD curl -f http://localhost/ || exit 1`


### **Kubernetes:**

**25. What is ReplicaSet?**

* Ensures a **specified number of pod replicas** are running at all times.

**26. How to do deployment in K8s?**

* `kubectl apply -f deployment.yaml`

**27. Stateful in K8s?**

* **StatefulSet** manages pods with **persistent storage, stable identities**.

**28. Liveness & Readiness probes?**

* **Liveness**: Checks if container is alive.
* **Readiness**: Checks if container is ready to accept traffic.

**29. Helm charts help?**

* **Package manager** for K8s — simplifies deployment with templates.

**30. How to scale K8s app?**

* `kubectl scale deployment app --replicas=5`

**31. Challenging issue in K8s?**

* Example: **Pods stuck in `CrashLoopBackOff`**, **network policy issues**, **node resource exhaustion**.



### **Ansible:**

**32. 4 hosts execution fails on 3rd & 4th, what happens?**

* By default, **playbook fails** for failed hosts. Use `ignore_errors: true` to continue.

**33. Vars in Ansible?**

* Variables defined in **playbooks, inventory, group\_vars, host\_vars**.

**34. Diff between play & task?**

* **Play:** Mapping of hosts to tasks.
* **Task:** Single action executed on host.

**35. Conditional statements in Ansible?**

* Use `when:` e.g.,

  ```yaml
  - name: Install package
    yum: name=httpd
    when: ansible_os_family == "RedHat"
  ```

**36. Facts in Ansible?**

* **System info** collected automatically (e.g., IP, OS).

**37. Debug a failing Ansible task?**

* Use `-vvv` or `debug:` module.



### **Linux:**

**38. System is slow — how to check?**

* Use `top`, `htop`, `iostat`, `vmstat`.

**39. Check connection host1 to host2?**

* `ping`, `telnet <host> <port>`, `nc -zv host port`.

**40. Connectivity to a port?**

* `telnet host port` or `nc -z host port`.

**41. iptables in Linux?**

* Firewall rules management: `iptables -L`.

**42. HTTP error codes?**

* 200 OK, 404 Not Found, 500 Server Error, 302 Redirect.

**43. Shell script run last day of every month?**

* Cron job:
  `59 23 28-31 * * [ "$(date +\%d -d tomorrow)" == "01" ] && /path/script.sh`

**44. Memory usage in Linux?**

* `free -m`, `top`, `vmstat`.

**45. Running ports in Linux?**

* `netstat -tuln` or `ss -tuln`.

**46. Check if service running?**

* `systemctl status <service>` or `ps aux | grep <service>`.



### **Prometheus:**

**47. Shell script metric data to Prometheus?**

* Expose metrics in **Prometheus text format** and use **node\_exporter textfile collector** or push to **Pushgateway**.
