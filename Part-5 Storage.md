
# 1. Why Storage?

Containers are **ephemeral** by nature.

This means whenever a Pod is restarted, rescheduled or deleted, anything stored inside the container filesystem is lost.

Example:

```
Pod A
│
├── application.jar
├── temp.txt
└── report.pdf
```

If Pod A gets deleted,

```
Pod B
```

is created with a fresh filesystem.

Hence, any important data should never be stored inside a container.

---

# 2. Ephemeral Storage

Ephemeral Storage exists only for the lifetime of a Pod.

Examples:

- Temporary files
- Cache
- Session data
- Intermediate processing files

Suitable for:

- Stateless applications
- Temporary processing

Not suitable for:

- Databases
- Uploaded documents
- Payment records
- Images
- Persistent logs

---

# 3. Persistent Storage

Persistent Storage survives Pod failures.

Even if the Pod is deleted,

the storage remains.

New Pods can mount the same storage.

Example:

```
Pod
   │
   ▼
Persistent Volume
```

---

# 4. Persistent Volume (PV)

A Persistent Volume is storage created and managed by the Infrastructure / Platform team.

Examples:

- NFS
- SAN
- NAS
- AWS EBS
- Azure Disk
- Google Persistent Disk

Application developers normally **do not create PVs**.

Responsibilities of Infra Team:

- Create storage
- Configure storage
- Manage lifecycle
- Allocate capacity

---

# 5. Persistent Volume Claim (PVC)

Applications never consume a PV directly.

Instead they request storage through a PVC.

Flow:

```
Application

↓

Deployment

↓

PVC

↓

PV

↓

Storage
```

PVC acts as a request for storage.

---

# 6. StorageClass

StorageClass automates storage provisioning.

Instead of creating PV manually,

developer simply requests

```
5Gi

StorageClass=fast-storage
```

Kubernetes automatically creates the PV.

Benefits:

- No manual storage creation
- Dynamic provisioning
- Simplified deployments

---

# 7. Static Provisioning

Flow

```
Infra Team

↓

Creates PV

↓

Developer creates PVC

↓

PVC binds to PV
```

Used in many enterprise environments.

---

# 8. Dynamic Provisioning

Flow

```
Developer

↓

Creates PVC

↓

StorageClass

↓

Automatic PV creation
```

Much simpler.

Recommended.

---

# 9. Access Modes

### ReadWriteOnce (RWO)

One node can mount the volume.

Commonly used for databases.

---

### ReadOnlyMany (ROX)

Many nodes can read.

No writing.

---

### ReadWriteMany (RWX)

Many Pods can read and write simultaneously.

Useful for shared storage.

---

# 10. Volume Mounts

A PVC alone is not enough.

Deployment must mount it.

Example

```
Deployment

↓

volumeMount

↓

/data

↓

PVC

↓

PV
```

Now application reads

```
/data
```

instead of container filesystem.

---

# 11. WordPress Demo

Trainer demonstrated

```
WordPress

↓

PVC

↓

PV

↓

Storage
```

WordPress uploads remain even if Pod restarts.

---

# 12. MySQL Demo

Similarly,

```
MySQL

↓

PVC

↓

PV
```

Database files remain persistent.

Without PVC,

database would become empty after Pod recreation.

---

# 13. Stateful Applications

Examples

- MySQL
- PostgreSQL
- MongoDB
- Kafka
- Elasticsearch

Characteristics

- Persistent data
- Stable storage
- Stable identity

Usually deployed using StatefulSets.

---

# 14. StatefulSets

Purpose

Provide

- Stable Pod names
- Stable storage
- Ordered deployment
- Ordered shutdown

Example

```
mysql-0

mysql-1

mysql-2
```

Each Pod gets its own PVC.

---

# 15. StatefulSet vs Deployment

Deployment

- Stateless
- Pods interchangeable
- No fixed identity

Examples

- Spring Boot APIs
- Payment Receiver
- REST APIs

---

StatefulSet

- Persistent storage
- Stable hostname
- Dedicated PVC
- Ordered startup

Examples

- Database
- Kafka
- ZooKeeper

---

# 16. Storage Best Practices

Never store

- Images
- PDFs
- Logs
- Database files

inside container filesystem.

Always use

- PV
- PVC
- External Storage

---

# 17. Logging Discussion (Project Mapping)

During project discussion,

Receiver application attempted

```
/logs
```

Result

```
Could not create directory /logs
```

Reason

OpenShift containers run with restricted permissions.

Root filesystem is not intended for persistent application storage.

Trainer recommendation and project discussion:

Application

↓

stdout

↓

OpenShift

↓

/var/log/pods

↓

Splunk

Instead of

```
Application

↓

/logs/payment.log
```

---

# 18. Can PVC be used for Logs?

Yes.

Architecture

```
Application

↓

/logs

↓

Volume Mount

↓

PVC

↓

PV
```

Useful only when

- Compliance requires file logs
- Third-party software requires log files

Otherwise

Console logging is recommended.

---

# 19. Dynamic Storage in OpenShift

OpenShift commonly uses StorageClasses.

Developer creates

```
PVC
```

Platform creates

```
PV
```

Application only mounts

```
/data
```

Developer usually does not know physical storage location.

---

# 20. Best Practices

✔ Use Deployments for stateless applications.

✔ Use StatefulSets for databases.

✔ Never depend on container filesystem.

✔ Mount PVCs for persistent data.

✔ Prefer StorageClasses.

✔ Keep business data outside Pods.

✔ Use console logging unless file logging is mandatory.

---

# Trainer Nuggets 💡

- Pods are temporary.
- Storage is permanent.
- Applications should never assume Pod lifetime.
- PVC is consumed by applications.
- PV is managed by infrastructure.
- Stateful applications require StatefulSets.
- WordPress and MySQL were demonstrated using PVCs.

---

# Commands

```bash
# Storage

oc get pv

oc get pvc

oc describe pvc <name>

oc describe pv <name>

# Apply

oc apply -f pvc.yaml

oc apply -f deployment.yaml

# Delete

oc delete pvc <name>
```

---

# BofA Mapping

Receiver Application

```
Receiver

↓

Deployment

↓

Pod
```

Currently

- Stateless
- Uses console logging
- Logs forwarded to Splunk
- No PVC currently used

Possible future use of PVC

- Log files (if compliance requires)
- File uploads
- Generated reports

Current recommendation discussed with Parvinder

```
Application

↓

stdout

↓

OpenShift

↓

/var/log/pods

↓

Splunk
```

instead of storing logs inside the container.

---

# Key Takeaways

- Containers should remain stateless wherever possible.
- Persistent data belongs in PVs.
- Applications request storage through PVCs.
- StorageClasses enable dynamic provisioning.
- StatefulSets are designed for applications requiring stable storage and identity.
- Console logging is preferred for cloud-native applications; PVC-backed log directories are only needed for specific requirements.
