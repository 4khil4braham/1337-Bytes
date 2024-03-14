# 🗒️ Kubernetes Security Checklist

## Pre-Assessment

* [ ] **Gather Kubernetes version details:**

```bash
kubectl version --short
```

Cross-reference the obtained version information with the [CVE ](https://cve.mitre.org/)database to identify any vulnerabilities specific to your Kubernetes version.

* [ ] **Check for Expired Certificates:**&#x20;

Kubernetes clusters use certificates for various components and services. Expired certificates can lead to service outages and vulnerabilities. Use `openssl` to check the expiration of a certificate:

```bash
openssl x509 -noout -dates -in /etc/kubernetes/pki/apiserver.crt
```

## Cluster Configuration and Management

* [ ] **Audit RBAC policies:**&#x20;

Use tools like `rbac-tool` or `kubectl get roles,clusterroles,rolebindings,clusterrolebindings --all-namespaces` to dump and analyze RBAC settings for overly permissive configurations.

* [ ] **Audit RBAC policies using `audit2rbac`:**&#x20;

`audit2rbac` is a tool that can create RBAC policies based on Kubernetes audit logs, providing insight into what permissions are being used. This can highlight overly permissive or unused roles and bindings:

<pre class="language-bash"><code class="lang-bash"><strong>kubectl logs kube-apiserver-&#x3C;pod_name> -n kube-system > audit.log
</strong>audit2rbac --filename audit.log
</code></pre>

## Kubernetes API Server

* [ ] **Analyze API Server Auditing Settings:**&#x20;

Auditing settings are crucial for tracking unauthorized access attempts and changes within the cluster. Check the audit policy file (`audit-policy.yaml`) used by the API server to ensure it's logging at the desired level (e.g., `RequestResponse` for all requests and responses).

```yaml
yamlCopy code# Example audit-policy.yaml snippet
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
  - level: RequestResponse
    resources:
    - group: ""
      resources: ["pods"]  
```

* [ ] **Check if the API server allows anonymous access:**

```bash
curl https://<api-server-ip>:<port>/api --header "Authorization: Bearer wrong"
```

If you get a successful response rather than a `401 Unauthorized`, anonymous access is enabled.

## Node and Pod Security

* [ ] **Check for containers running as `root`:**&#x20;

Identify pods running as root, which can be a major security risk, using a command like:

```bash
kubectl get pods -o jsonpath='{.items[?(@.spec.containers[*].securityContext.runAsUser==0)].metadata.name}'
```

* [ ] **Check for `AllowPrivilegeEscalation`:**&#x20;

Preventing privilege escalation within containers is critical. Use the following command to find pods allowing privilege escalation:

```bash
kubectl get pods -A -o json | jq '.items[] | select(.spec.containers[].securityContext.allowPrivilegeEscalation == true) | .metadata.name'
```

## Authentication and Authorization

* [ ] **Evaluate service accounts:**&#x20;

Check for service accounts with unnecessary permissions by inspecting their linked roles and cluster roles using `kubectl get serviceaccounts -o yaml` and then analyzing their permissions.

* [ ] **Test Service Account token access:**&#x20;

You can attempt to use a service account's token to access the Kubernetes API and test what level of access is granted. This requires accessing a pod and using the mounted service account token found at `/var/run/secrets/kubernetes.io/serviceaccount/token`:

```bash
TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
curl --insecure --header "Authorization: Bearer $TOKEN" https://kubernetes.default.sv
```

## Workload Security

* [ ] **Review deployed applications for common vulnerabilities:**&#x20;

```bash
trivy image <your_image_name>
```

Trivy is a comprehensive vulnerability scanner for container images, filesystems, and packages. It can help identify known vulnerabilities in the container images used in your cluster.

* [ ] **Use Kube-bench for CIS Benchmark Tests:**&#x20;

`kube-bench` runs checks against Kubernetes clusters to ensure they are deployed securely by checking against the CIS Kubernetes Benchmark.

```bash
kube-bench run
```

## Network Exposure and Services

* [ ] **Enumerate exposed services:**&#x20;

Kube-hunter is an open-source tool designed to hunt for security weaknesses in Kubernetes clusters. It can be used to perform a wide range of tests against your clusters:

```bash
kube-hunter --remote <your_cluster_API_endpoint>
```

* [ ] **Inspect Ingress Objects for TLS Configuration:**

&#x20;Ensuring that Ingress objects are correctly configured with TLS can prevent traffic interception. List all Ingress objects and review their TLS settings:

```bash
kubectl get ingress -A -o yaml | less
```

## Storage Security

* [ ] **Inspect Persistent Volumes (PVs):**

&#x20;Check the configuration of PVs for public access or misconfigurations using&#x20;

```
kubectl get pv -o yaml
```

* [ ] Review `accessmodes`

```bash
kubectl pvc --all-namespaces -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.accessModes}{"\n"}{end}'
```

* [ ] **Review StorageClass for Default Encryption:**&#x20;

```bash
kubectl get storageclass -o yaml
```

## `etcd` security

* [ ] **Ensure `etcd`encryption:**&#x20;

Verify if `etcd` data is encrypted at rest by checking the Kubernetes API server manifest for the `--encryption-provider-config` flag, indicating that encryption is enabled for resources stored in `etcd`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  ...
spec:
  containers:
  - command:
    - kube-apiserver
    ...
    - --encryption-provider-config=/etc/kubernetes/pki/encryption-config.yaml

```

* [ ] **Check for Etcd Access Controls:**&#x20;

Direct access to `etcd` is a significant risk. Ensure that access is restricted and encrypted. If you have access, you can check the `etcd` member list to understand its configuration:

```bash
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/path/to/ca.pem --cert=/path
```

* #### Etcd Exposed to the Internet

An exposed etcd server can be a gold mine for attackers, providing access to all state and secrets managed by Kubernetes.

Scan for open etcd ports using nmap or similar tools:

```bash
nmap -p 2379,2380 <cluster-IP-range>
```

## CI/CD and DevOps Practices

* [ ] **Assess the CI/CD pipeline security:**&#x20;

Review the pipeline configurations for exposed secrets or lack of security steps like SAST (Static Application Security Testing) by inspecting the pipeline definition files (e.g., `.gitlab-ci.yml`, `Jenkinsfile`).

* [ ] **Audit CI/CD pipeline for exposed secrets:**&#x20;

Manually review configuration files or use automated tools to detect hard-coded secrets. GitHub's secret scanning feature or tools like `truffleHog` can be utilized to search through the repository history for exposed secrets.

* [ ] **Static Analysis of Infrastructure as Code (IaC):**&#x20;

Tools like `checkov` or `terraform-compliance` can be used to perform static analysis on IaC (e.g., Terraform, CloudFormation) to identify misconfigurations.

```bash
bashCopy codecheckov -d /path/to/terraform/code
```

## Logging and Monitoring

* [ ] **Verify logging configurations:**&#x20;

Ensure that audit logging is enabled and properly configured on the Kubernetes API server with `--audit-log-path` and `--audit-policy-file` flags specify the log file path and the audit policy file, respectively.

This can also be done using the following command

```
ps aux | grep kube-apiserver | grep audit-log-path
```
