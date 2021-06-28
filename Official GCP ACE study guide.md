# Official GCP ACE study guide

## Table of content

1. Overview of Google Cloud Platform
2. Google Cloud Computing Services

## 1. Overview of Google Cloud Platform

### 1.1. GCP

To use GCP you first have to create an account with a cloud provider and provide a billing account.

A zone is considered a single failure domain, which means that if all instances in your applications are running in a zone and there is a failure, then all instances of your application will be inaccessible.

### 1.2. Compute

VMs are a basic unit uz computing resources. You have full access to the VM.

### 1.3. Kubernetes

Many users would rather focus on their applications and not the tasks needed to keep a cluster of servers up and running. Autoscaling + health checks + monitoring, networking & security mgmt.

### 1.4. Serverless

Allows developers to run their code in a computing env. that does not require setting up a VM or K8s.

### 1.5. Storage

- Object storage - Cloud Storage
- File Storage - Cloud Filestore
- Block Storage - Persistent/Ephemeral disks
- Caches - in-memory storage

- Object Storage is a system that manages the use of storage in terms of objects or blobs. Files are not stores in a file system. Each object is addressable via an URL.
- Files storage services provide a hierarchical storage system for files, based on the Network File System (NFS) storage system.
- Block Storage is commonly used in ephemeral and persistent disks attached to VMs. With a block storage system, you can install file systems on top of block storage, or you can run apps that access blocks directly, like some relational DBs. Common block size 4KB to 8KB or more.
- caches are in-memory data stores that maintain fast access to data. 1) Memory is more expensive, 2) cache is volatile and 3) can get out of sync.

### 1.6. Storage speeds

Retrieve 1 MB of data | latency | x times slower
--- | --- | ---
in-memory cache | 250 microseconds |
persistent SSD | 1 000 microseconds/ 1 millisecond | 4x
persistent HDD | 20 millisecond | 20x
Object storage | | It takes longer to retrieve data from object storage than to retrieve it from block storage.

### 1.7. Networking

Peering is a general term for linking distinct networks.

### 1.8. Specialized services

Are:

- serverless
- provide a specific function
- provide API
- are changed based on the usage

### 1.9. OpEXp vs CapEx

Since cloud providers have many customers, the variation in demand of any of the customers has little effect on the overall use of their resources.

## 2. Google Cloud Computing Services

IaaS -> K8s -> PaaS

### Compute Engine

VMs are programs that emulate physical servers and provide CPU, memory, storage, and other services. VMs run on a low-level service called a hypervisor. GCP uses a security-hardened version of the Kernel Virtual Machine (KVM) hypervisor.

### k8s

Kubernetes Engine is designed to allow users to easily run containerized (lightweight VMs) applications on a cluster of servers.

### App Engine

Manages the underlying computing and network infrastructure.

- Standard - Sandbox env.
- Flexible - Docker containers

### Cloud Functions

FaaS

### Storage

- Object:
  - Cloud Storage - buckets -> Objects with URL access. Cloud Storage is not part of a VM in the way an attached persistent disk is.
- Block:
  - Persistent disks are storage services that are attached to VMs in Compute Engine or Kubernetes Engine.
- File:
  - To have access to a file system housed on network-attached storage.
- SQL
  - Cloud SQL - GCPs managed relational database service for MySQL or PostgreSQL
  - Cloud Spanner - globally distributed relational database that combines benefits or relational database and ability to scale horizontally.
- NoSQL
  - Cloud Firestore - NoSQL document database
  - Cloud Bigtable - wide-column data model for low latency read/write operations
- In-memory cache:
  - Cloud Memorystore - managed Redis

### Networking

VPC - Virtual Private Cloud can span the globe without relying on the public Internet. Traffic from any server on a VPC can be securely routed through the Google global network to any other point on the network.