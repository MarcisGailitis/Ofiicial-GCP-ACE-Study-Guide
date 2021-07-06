# Official GCP ACE study guide

## Table of content

1. Overview of Google Cloud Platform
2. Google Cloud Computing Services
3. Projects, Service Accounts, and Billing
4. Introduction to Computing in Google Cloud
5. Computing with Compute Engine VMs

## 1. Overview of Google Cloud Platform

### 1.1. GCP

To use GCP you first have to create an account with a cloud provider and provide a billing account.

A zone is considered a single failure domain, which means that if all instances in your applications are running in a zone and there is a failure, then all instances of your application will be inaccessible. Zones are data center-like resources, but they may be comprised of one or more closely coupled data centers.

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

### 2.1. Compute Engine

VMs are programs that emulate physical servers and provide CPU, memory, storage, and other services. VMs run on a low-level service called a hypervisor. GCP uses a security-hardened version of the Kernel Virtual Machine (KVM) hypervisor.

### 2.2. k8s

Kubernetes Engine is designed to allow users to easily run containerized (lightweight VMs) applications on a cluster of servers.

### 2.3. App Engine

Manages the underlying computing and network infrastructure.

- Standard - Sandbox env.
- Flexible - Docker containers

### 2.4. Cloud Functions

FaaS

### 2.5. Storage

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

### 2.6. Networking

VPC - Virtual Private Cloud can span the globe without relying on the public Internet. Traffic from any server on a VPC can be securely routed through the Google global network to any other point on the network.

### 2.7. Cloud Load Balancing

Using single multicast address.

### 2.8. Cloud Armor

- DDOS
- Cross-site scripting
- SQL injection

### 2.9. Cloud CDN

Content Delivery Networks enable low-latency response by caching content on a set of endpoints across the globe.

### 2.10. Cloud Interconnect

Set of services for connecting your existing network to the Google network. Interconnect & peering.

### 2.11. Cloud DNS

Also provides for private zones, that allows you to create custom names for your VMs.

### 2.12. Dev Tools

- SDK + plugins
- Container Registry
- Cloud Build
- Cloud Source Repo

### 2.13. Monitoring

Stackdriver – logging, monitoring, error reporting, trace, debug, profile

### 2.14. Specialized services

- Apigee
- Data Prep – explore + prepare
- Data flow – batch and stream processing
- Data proc – managed Hadoop
- Machine Learning

## 3. Projects, Service Accounts, and Billing

### 3.1. Organization

G-Suite Domains and Cloud Identity accounts map to GCP Organization.

1. A single cloud identity is associated with at most one organization.
2. Cloud identities have super admins.
3. Those super admins assign the role of Organization Administrator IAM role to users, who will administrate the organization. Users with Organization Administrator role are responsible to:
3.1 define the structure or resource hierarchy
3.2 defining identity access management policies over the resource hierarchy
3.3 delegating other management roles to other users
4. GCP will automatically grant Project Creator and Billing Account Creator roles to all roles in the domain.

### 3.2. Project

Anyone with `resourcemanager.projects.create` IAM permission can create a project. Projects are the lowest-level structure in the resource hierarchy.

### 3.3. Organization Policies

Organization policy service control access to an organization's resources.
The Organization Policy Service complements the IAM service. One way to think of this difference is that IAM specifies WHO can do things and Organization Policy Service specifies WHAT can be done.
IAM lets you assign permissions, so users and roles can perform a specific operation in the cloud.
Organization Policy Service lets you specify limits on the ways resources can be used:

- Allow a specific set of values
- Deny a specific set of values
- Deny a value and all its child values
- Allow all allowed
- Deny all values

### 3.4. Policy inheritance

For example, you might have a specific policy. You could attach it to an organization. All folders and projects below will inherit that policy. Since policies are inherited and cannot be disabled or overridden by objects lower in the hierarchy, this is an effective way to apply a policy across all organization resources.

### 3.5. Roles in GCP

A role is a collection of permissions. Roles are granted to users by binding a user to a role.

Human/Service -> Identity (email) <-roles

### 3.6. Granting roles to identities

Permissions cannot be granted to users. Permissions can only be assigned to roles.
Roles are then assigned to users.

### 3.7. Service account

Identities are usually associated with individual users. Sometimes it is helpful to have applications or VMs act on behalf of a user or perform operations that the user does not have permissions to perform.

For example, an application needs access to a database, but you do not want to allow users of the application to access the database directly. A service account can be created that has access to the database. That service account can be assigned to the application, so the application can execute queries on behalf of users w/o having to grant database access to those users.

The service account can be resource and identity:

- Permissions -> Role -> Service account <- Identity (users)

- User managed service account (up to 100 per project)
- Google-managed service account.

Service accounts can be managed as a group of accounts at the project level or an individual service account level. For example, if you grant `iam.serviceAccountUser` to a user in a specific project, then the user can manage all service accounts in the project. If you prefer to limit users to manage only a specific service account, you could grant `iam.serviceAccountUser` for a specific Service account.

### 3.8 Billing Accounts

Billing acc. store information about how to pay charges for resources used. A billing account is associated with one or more projects.

Billing account roles:

- Billing Account Creator - the only role, which can create a billing account!!!
- Billing Account Administrator - Authorized to see and manage all aspects of billing accounts
- Billing Account User - Can associate projects with billing accounts
- Billing Account Viewer - Can view information about billing accounts

## 4. Introduction to Computing in Google Cloud

### 4.1. Virtual Machine Custom Images

You can create a custom image from a boot disk or by starting with another image:

1. VM instances -> Create Instance -> choose an image -> create the VM
2. SSH, make changes
3. Snapshots -> Create snapshot -> Specify parameters -> Create
4. Use the image you created from the snapshot

### 4.2. Roles for Compute Engine

- Compute Engine Admin
- Compute Engine Network Admin
- Compute Engine Security Admin
- Compute Engine Viewer

### 4.3. App Engine

With App Engine you focus on your application and not on the VMs that run the application. You have little need to configure and control the underlying OS or storage system.

### 4.4. K8s

K8s allows you to do the following:

- Create clusters of Compute Engine VMs that run the Kubernetes orchestration software
- Deploy containerized applications in groups called pods. Containers within a single pod share storage and network
- Administer the cluster. A pod is a logically single unit for providing a service.
- Specify policies, like autoscaling
- Monitor cluster health

K8s provides the following functions:

- Load balancing
- Automatic scaling
- Automatic upgrades of cluster software
- Node monitoring + health
- Logging
- Support for node pools

K8s overhead:

- 2% - 25% of memory
- 0.25% - 6% of CPU

A K8s deployment is a group of running identical pods. These identical pods are referred to as replicas. Deployment can be in 3 states:

- Progressing
- Completed
- Failed

K8s Engine is a good choice for a large-scale application that requires high availability and high reliability.

### Cloud Functions

Cloud Functions is a serverless computing platform designed to run single-purpose pieces of code in response to events in the GCP environment. It provides the blue between otherwise independent services. Well suited to short-running, event-based processing.

Execution of one function is independent of all others, that is Cloud functions may be running multiple instances at one time. You do not have to do anything to prevent conflicts b/w the two instances, they are independent.
