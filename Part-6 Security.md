
# 1. Why Security is Important?

Applications deployed in an enterprise environment must ensure:

- Authentication
- Authorization
- Data Confidentiality
- Data Integrity
- Secure Communication

OpenShift is designed with **security by default**, making it more secure than a vanilla Kubernetes cluster.

---

# 2. OpenShift Security Model

Unlike Docker containers that often run as the root user, OpenShift follows the principle of **least privilege**.

Characteristics:

- Containers do not run as root.
- Every Pod gets a randomly assigned User ID (UID).
- Applications should not depend on a fixed UID.
- Root filesystem is generally read-only or restricted.

This prevents applications from making unauthorized changes to the host system.

---

# 3. Why Applications Should Not Run as Root?

Running containers as root increases security risks.

Potential issues:

- Unauthorized file access
- Host compromise
- Privilege escalation
- Data leakage

OpenShift prevents this by assigning non-root UIDs.

---

# 4. Security Context Constraints (SCC)

OpenShift uses **Security Context Constraints (SCC)** to control what a Pod is allowed to do.

SCC defines:

- Whether the container can run as root
- Allowed Linux capabilities
- Volume permissions
- Filesystem access
- Privileged mode

SCC provides an additional layer of security on top of Kubernetes.

---

# 5. Service Accounts

Applications should not use personal user credentials.

Instead, Pods use **Service Accounts**.

Purpose:

- Authenticate Pods
- Access Kubernetes/OpenShift APIs
- Access cluster resources securely

Each Pod is associated with a Service Account.

---

# 6. Role-Based Access Control (RBAC)

RBAC determines **who can perform which actions**.

Examples:

Developer

- View Pods
- Deploy applications
- View Logs

Administrator

- Create Projects
- Manage Nodes
- Install Operators
- Configure Cluster

RBAC follows the principle of **least privilege**.

Users should receive only the permissions necessary to perform their work.

---

# 7. Secrets

Applications often require sensitive information such as:

- Database passwords
- API Keys
- TLS Certificates
- MQ Certificates
- OAuth Tokens

These should **never** be stored inside source code.

OpenShift stores such information as **Secrets**.

---

# 8. Secret Types

Common Secret examples:

- Username / Password
- TLS Certificates
- SSH Keys
- Docker Registry Credentials
- Generic Key-Value Secrets

Applications consume Secrets either as:

- Environment Variables
- Mounted Volumes

---

# 9. ConfigMaps vs Secrets

## ConfigMap

Stores non-sensitive configuration.

Examples:

- URLs
- Feature Flags
- Timeouts
- Application Properties

---

## Secret

Stores sensitive information.

Examples:

- Passwords
- Certificates
- Tokens
- Keys

Never place confidential information inside ConfigMaps.

---

# 10. TLS (Transport Layer Security)

TLS encrypts communication between client and server.

Without TLS:

```
Browser

↓

HTTP

↓

Plain Text
```

Anyone on the network could potentially intercept the traffic.

With TLS:

```
Browser

↓

HTTPS

↓

Encrypted Communication
```

---

# 11. TLS Termination

Trainer discussed **Edge Termination**.

Flow:

```
Browser

↓

HTTPS

↓

Reverse Proxy / Router

↓

HTTP

↓

Application
```

The OpenShift Router (or enterprise reverse proxy) decrypts HTTPS traffic before forwarding it internally.

Benefits:

- Easier certificate management
- Reduced application complexity
- Centralized security

---

# 12. Reverse Proxy

Purpose:

- SSL/TLS termination
- Request forwarding
- Load balancing
- Security
- Routing

Applications remain focused on business logic while networking concerns are handled by the platform.

---

# 13. Certificates

Certificates establish trust between systems.

Examples:

- HTTPS Certificates
- MQ Certificates
- Client Certificates

Applications may need certificates to establish secure communication with external systems.

---

# 14. MQ Certificates (Project Mapping)

In your Receiver project,

MQ communication required certificates.

Flow:

```
Receiver Application

↓

MQ Certificate

↓

Secure Connection

↓

Message Queue
```

Certificates were added as Kubernetes/OpenShift Secrets rather than being embedded into the application.

---

# 15. Secret Mounting

Secrets can be mounted into Pods as files.

Example:

```
Secret

↓

Volume

↓

/etc/certs

↓

Application
```

Application reads the certificate directly from the mounted location.

---

# 16. Environment Variable Injection

Secrets may also be injected as environment variables.

Example:

```
Secret

↓

Environment Variable

↓

Application
```

Useful for passwords and API keys.

---

# 17. Image Security

Only trusted container images should be deployed.

Best Practices:

- Use enterprise registries
- Avoid unverified public images
- Scan images for vulnerabilities
- Keep base images updated

---

# 18. Principle of Least Privilege

Every component should receive only the permissions it needs.

Examples:

Developer

✔ Deploy application

✘ Delete Cluster

Database Application

✔ Read Database

✘ Access Kubernetes API

This minimizes the impact of accidental or malicious actions.

---

# 19. Security Best Practices

✔ Never hardcode passwords.

✔ Store credentials in Secrets.

✔ Use ConfigMaps only for non-sensitive configuration.

✔ Use HTTPS for external communication.

✔ Avoid running containers as root.

✔ Grant minimum required permissions.

✔ Use trusted container images.

✔ Rotate certificates periodically.

---

# Trainer Nuggets 💡

- OpenShift is secure by default.
- Containers run with non-root UIDs.
- Secrets should never be stored in source code.
- ConfigMaps are for configuration, not credentials.
- TLS should terminate at the reverse proxy.
- RBAC implements least privilege.

---

# Useful Commands

```bash
# Secrets

oc get secrets

oc describe secret <secret-name>

# ConfigMaps

oc get configmap

oc describe configmap <configmap-name>

# Service Accounts

oc get sa

# SCC

oc get scc

# RBAC

oc get roles

oc get rolebindings
```

---

# BofA Mapping

Examples from your project:

### MQ Certificates

```
Receiver

↓

Secret

↓

Mounted Certificate

↓

Secure MQ Connection
```

---

### Application Configuration

```
Receiver

↓

ConfigMap

↓

Application Properties
```

---

### Authentication

```
Developer

↓

OpenShift Login

↓

RBAC

↓

Project Access
```

---

### Logging

Even application logs should not expose:

- Passwords
- Account Numbers
- Tokens
- Certificates

Sensitive information should always be masked before being written to the console.

---

# Key Takeaways

- OpenShift follows a secure-by-default approach.
- Containers run as non-root users.
- SCC restricts container capabilities.
- RBAC enforces least privilege.
- Secrets securely store sensitive data.
- ConfigMaps store non-sensitive configuration.
- TLS encrypts communication.
- Reverse Proxies handle TLS termination.
- MQ certificates should be managed using Secrets rather than hardcoded into the application.
