# 🗒️ Kubernetes Security Checklist

## Pre-Assessment

* [ ] **Gather Kubernetes version:**&#x20;

Use `kubectl version` to check the running version of Kubernetes and then reference the [CVE ](https://cve.mitre.org/)(Common Vulnerabilities and Exposures) database to identify any known vulnerabilities associated with that version.



## Cluster Configuration and Management

* [ ] **Audit RBAC policies:**&#x20;

Use tools like `rbac-tool` or&#x20;

```
kubectl get roles,clusterroles,rolebindings,clusterrolebindings --all-namespaces 
```

to dump and analyze RBAC settings for overly permissive configurations.

#### 3. Node and Pod Security

* **Inspect security contexts:** Use a command like `kubectl get pods --all-namespaces -o=jsonpath='{.items[?(@.spec.securityContext.privileged==true)].metadata.name}'` to find pods running with privileged security contexts, which could be a security risk.

#### 4. Authentication and Authorization

* **Evaluate service accounts:** Check for service accounts with unnecessary permissions by inspecting their linked roles and cluster roles using `kubectl get serviceaccounts -o yaml` and then analyzing their permissions.

#### 5. Workload Security

* **Review deployed applications for common vulnerabilities:** Use a vulnerability scanner such as Trivy against container images to identify known vulnerabilities in the applications deployed within the cluster.

#### 6. Network Exposure and Services

* **Enumerate exposed services:** Use a tool like Kube-hunter to scan for exposed services and ingress resources that might be accessible from outside the cluster without proper authorization.

#### 7. Storage Security

* **Inspect PersistentVolumes (PVs):** Check the configuration of PVs for public access or misconfigurations using `kubectl get pv -o yaml` and review the `accessModes` and `persistentVolumeReclaimPolicy`.

#### 8. Etcd Security

* **Ensure etcd encryption:** Verify if etcd data is encrypted at rest by checking the Kubernetes API server manifest for the `--encryption-provider-config` flag, indicating that encryption is enabled for resources stored in etcd.

#### 9. CI/CD and DevOps Practices

* **Assess the CI/CD pipeline security:** Review the pipeline configurations for exposed secrets or lack of security steps like SAST (Static Application Security Testing) by inspecting the pipeline definition files (e.g., `.gitlab-ci.yml`, `Jenkinsfile`).

#### 10. Logging and Monitoring

* **Verify logging configurations:** Ensure that audit logging is enabled and properly configured on the Kubernetes API server with `--audit-log-path` and `--audit-policy-file` flags, which specify the log file path and the audit policy file, respectively.
