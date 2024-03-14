# 🗒️ Kubernetes Security Checklist

## Pre-Assessment

* [ ] **Gather Kubernetes version details:**

```bash
kubectl version --short
```

Cross-reference the obtained version information with the [CVE ](https://cve.mitre.org/)database to identify any vulnerabilities specific to your Kubernetes version.

## Cluster Configuration and Management

* [ ] **Audit RBAC policies:**&#x20;

Use tools like `rbac-tool` or `kubectl get roles,clusterroles,rolebindings,clusterrolebindings --all-namespaces` to dump and analyze RBAC settings for overly permissive configurations.

* [ ] **Audit RBAC policies using `audit2rbac`:**&#x20;

`audit2rbac` is a tool that can create RBAC policies based on Kubernetes audit logs, providing insight into what permissions are being used. This can highlight overly permissive or unused roles and bindings:

<pre class="language-bash"><code class="lang-bash"><strong>kubectl logs kube-apiserver-&#x3C;pod_name> -n kube-system > audit.log
</strong>audit2rbac --filename audit.log
</code></pre>

## Node and Pod Security

* [ ] **Check for containers running as root:**&#x20;

Identify pods running as root, which can be a major security risk, using a command like:

```bash
kubectl get pods -o jsonpath='{.items[?(@.spec.containers[*].securityContext.runAsUser==0)].metadata.name}'
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

## Network Exposure and Services

* [ ] **Enumerate exposed services:**&#x20;

Kube-hunter is an open-source tool designed to hunt for security weaknesses in Kubernetes clusters. It can be used to perform a wide range of tests against your clusters:

```bash
kube-hunter --remote <your_cluster_API_endpoint>
```

## Storage Security

* [ ] **Inspect Persistent Volumes (PVs):**

&#x20;Check the configuration of PVs for public access or misconfigurations using&#x20;

```
kubectl get pv -o yaml
```

* [ ] Review `accessmodes`

Review the access modes of PVCs to ensure they are not overly permissive:

```bash
kubectl pvc --all-namespaces -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.accessModes}{"\n"}{end}'
```

## `Etcd` Security

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

## CI/CD and DevOps Practices

* [ ] **Assess the CI/CD pipeline security:**&#x20;

Review the pipeline configurations for exposed secrets or lack of security steps like SAST (Static Application Security Testing) by inspecting the pipeline definition files (e.g., `.gitlab-ci.yml`, `Jenkinsfile`).

* [ ] **Audit CI/CD pipeline for exposed secrets:**&#x20;

Manually review configuration files or use automated tools to detect hard-coded secrets. GitHub's secret scanning feature or tools like `truffleHog` can be utilized to search through the repository history for exposed secrets.

## Logging and Monitoring

* [ ] **Verify logging configurations:**&#x20;

Ensure that audit logging is enabled and properly configured on the Kubernetes API server with `--audit-log-path` and `--audit-policy-file` flags, which specify the log file path and the audit policy file, respectively.

Can also be done use the following command

```
ps aux | grep kube-apiserver | grep audit-log-path
```
