# 🗒️ Kubernetes Security Checklist

## Pre-Assessment

* [ ] **Gather Kubernetes version:**&#x20;

Use `kubectl version` to check the running version of Kubernetes and then reference the [CVE ](https://cve.mitre.org/)(Common Vulnerabilities and Exposures) database to identify any known vulnerabilities associated with that version.

## Cluster Configuration and Management

* [ ] **Audit RBAC policies:**&#x20;

Use tools like `rbac-tool` or `kubectl get roles,clusterroles,rolebindings,clusterrolebindings --all-namespaces` to dump and analyze RBAC settings for overly permissive configurations.

## Node and Pod Security

* [ ] **Inspect security contexts:**&#x20;

Use a command like `kubectl get pods --all-namespaces -o=jsonpath='{.items[?(@.spec.securityContext.privileged==true)].metadata.name}'` to find pods running with privileged security contexts, which could be a security risk.

## Authentication and Authorization

* [ ] **Evaluate service accounts:**&#x20;

Check for service accounts with unnecessary permissions by inspecting their linked roles and cluster roles using `kubectl get serviceaccounts -o yaml` and then analyzing their permissions.

## Workload Security

* [ ] **Review deployed applications for common vulnerabilities:**&#x20;

Use a vulnerability scanner such as `Trivy` against container images to identify known vulnerabilities in the applications deployed within the cluster

## Network Exposure and Services

* [ ] **Enumerate exposed services:**&#x20;

Use a tool like `Kube-hunter` to scan for exposed services and ingress resources that might be accessible from outside the cluster without proper authorization.

## Storage Security

* [ ] **Inspect Persistent Volumes (PVs):**

&#x20;Check the configuration of PVs for public access or misconfigurations using `kubectl get pv -o yaml` and review the `accessModes` and `persistentVolumeReclaimPolicy`.

## `Etcd` Security

* [ ] **Ensure `etcd`encryption:**&#x20;

Verify if etcd data is encrypted at rest by checking the Kubernetes API server manifest for the `--encryption-provider-config` flag, indicating that encryption is enabled for resources stored in etcd.

## CI/CD and DevOps Practices

* [ ] **Assess the CI/CD pipeline security:**&#x20;

Review the pipeline configurations for exposed secrets or lack of security steps like SAST (Static Application Security Testing) by inspecting the pipeline definition files (e.g., `.gitlab-ci.yml`, `Jenkinsfile`).

## Logging and Monitoring

* [ ] **Verify logging configurations:**&#x20;

Ensure that audit logging is enabled and properly configured on the Kubernetes API server with `--audit-log-path` and `--audit-policy-file` flags, which specify the log file path and the audit policy file, respectively.
