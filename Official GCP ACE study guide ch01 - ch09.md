# Official GCP ACE study guide ch01 - ch09

## Table of content

1. Overview of Google Cloud Platform
2. Google Cloud Computing Services
3. Projects, Service Accounts, and Billing
4. Introduction to Computing in Google Cloud
5. Computing with Compute Engine VMs
6. Managing Virtual Machines
7. Computing with Kubernetes
8. Managing Kubernetes Clusters
9. Computing with App Engine

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
- Data Flow – batch and stream processing
- Data Proc – managed Hadoop
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

### 4.5. Cloud Functions

Cloud Functions is a serverless computing platform designed to run single-purpose pieces of code in response to events in the GCP environment. It provides the blue between otherwise independent services. Well suited to short-running, event-based processing.

Execution of one function is independent of all others, that is Cloud functions may be running multiple instances at one time. You do not have to do anything to prevent conflicts b/w the two instances, they are independent.

## 5. Computing with Compute Engine VMs

### 5.1. VM configuration Details console

1. Create/Select a project
2. Create/Select billing account

The name of a VM is primarily for your use. GCP uses other identifiers internally to manage VMs.
After you specify the region/zone, GCP can determine the VMs available in that zone.

Minimal set of information in gcloud for setting up a VM: name, type (default n1-standard1), region, and zone (info can be taken from project defaults)

In Identity and API section you can specify a service account for the VM and set the scope of API access. If you want the process running on this VM to use only some APIs, you can use the options to limit the VMs' access to specific APIs.

- Management TAB: labels, description, Deletion protection, startup script, metadata, preemptibility, automatic restart, maintenance
- Security: Secure Boot, virtual Trusted Platform Module, Integrity Monitoring, block project-wide SSH keys
- Disks: Boot disk deletion rule, encryption + additional disks
- Networking: Network interface information + network tags
- Sole Tenancy

### 5.2. VM configuration Details Cloud SDK

`gcloud init` -> authenticate -> select project

`gcloud compute instances create instance-1 instance2`

If you do not specify additional parameters, gcloud will use your information from your default project.

```shell
# to view project information

gcloud compute project-info describe
```

- `--zone`
- `--boot-disk-size`
- `--boot-disk-type`
- `--labels`
- `--machine-type`
- `--preemptible`

### 5.3. VM management

`gcloud compute instances stop INSTANCE-NAME`

While your VM is running you can monitor CPU, disk, and network load by viewing the monitoring page in the VM instance details

### 5.4. Cost of VMs

VMs are charged for a minimum of 1 minute of use. VMS are billed in 1second increments, the cost is based on machine type, Google offers discounts for sustained usage.

## 6. Managing Virtual Machines

### 6.1. start, stop, delete an instance

Reset = restart a VM. The properties of VM will not change, but data in memory will be lost.

### 6.2. Viewing VMs

Filtering based on:

- Name
- Internal IP
- External IP
- Status
- Zone
- Network
- Deletion protection
- Member of a managed instance group
- Member of unmanaged instance group

### 6.3. GPUs

GPUs are used for math-intensive applications like visualizations or machine learning.  GPUs perform math calculations and allow some work to be off-loaded from the CPU to GPU. To add a GPU to an instance, you must:

- start an instance in which GPU libraries have been installed.
- Verify that the instance will run in a zone that has GPUs available.

To add GPU:

- Machine type -> Customize -> Select Nr of GPUs/GPU type

Restrictions:

- CPU must be compatible with the GPU
- GPUs cannot be attached to shared memory machines
- You must set the instance to terminate during maintenance

### 6.4. Snapshots

Snapshots are copies of the persistent disk. Snapshots are useful as backups and for copying data to another instance

1st snapshot = full copy of a disk. 2nd snapshot = diff.

FLUSH data from memory before snapshot.

To work with snapshots you must be assigned the Compute Storage Admin role.

When creating a snapshot you can specify name, labels, description.

### 6.5. Images

Images are copies of disk content to create VMs, as they save OS and related configuration.

You can create Images from:

- Disk
- Snapshot
- Cloud Storage File
- Another Image (image can be also from a different project)

When creating an image you can specify a name, labels, description, and Family (Family allows to group images, when family for the image is selected the latest image is used).

You can delete or depreciate old images, which marks the image no longer supported and allows you to specify replacement images.

### 6.6. gcloud global flags

- `--account`
- `--configuration`
- `--flatten`
- `--format`
- `--help`
- `--project`
- `--quiet`
- `--verbosity`

### 6.7. gcloud starting instances

`gcloud compute instances start INSTANCE_NAMES`

- `--async` displays information about the start operation aka –verbose in Linux
- `--zone`, specifies zones

`gcloud compute instances stop INSTANCE_NAMES`

`gcloud compute instances delete INSTANCE_NAMES`

- `--zone`, specifies zones
- `--delete-disks=(ALL,BOOT,DATA)`
- `--keep-disks=(ALL,BOOT,DATA)`

`gcloud compute instances list`

`gcloud compute instances describe`

### 6.8. gcloud disk -> snapshot

Create a snapshot:

`gcloud compute disks snapshot DISK_NAME --snapshot-names=SNAPSHOT_NAME`

List snapshots:

`gcloud compute snapshots list`

Describe snapshots:

`gcloud compute snapshots describe SNAPSHOT_NAME`

### 6.9. gcloud snapshot -> disk

Create a disk from snapshot:

`gcloud compute disks create DISK_NAME  --source-snapshot=SNAPSHOT_NAME`

- `--size=100`
- `--type=pd-standard`

### 6.10. images

`gcloud compute images create IMAGE_NAMES`

- `--source-disk`
- `--source-image`
- `--source-image-family`
- `--source-snapshot`
- `--source-uri`
- `--description`
- `--labels`

`gcloud compute images delete IMAGE_NAMES`

To export the image to Cloud Storage:

`gcloud compute images export --destination-uri=DESTINATION_URI --image IMAGE_NAME`

### 6.11. Creating and removing instance groups

- Instance groups = groups of VMs with the same configuration that are managed as a single entity. Instance groups can contain instances in a single zone or across a region.
- Managed instance groups  = are created using an instance template which is a specification of a VM configuration, including machine type, boot disks image, zone, labels, and other properties of an instance. Managed instance groups can automatically scale the number of instances in a group and be used with load balancing to distribute workloads across the instance group.
- Unmanaged instance groups   = different configurations within different VMs within the group.

`gcloud compute instance-templates create INSTANCE_TEMPLATE_NAME`

- `--source-instance`, specify an existing VM as the source of the instance template.

`gcloud compute instance-templates delete INSTANCE_TEMPLATE_NAME`

`gcloud compute instance-templates list`

`gcloud compute instance-groups managed create INSTANCE_GROUP_NAME --size=SIZE –template=TEMPLATE`

- `--size`
- `--template`

`gcloud compute instance-groups managed delete INSTANCE_GROUP_NAME`

`gcloud compute instance-groups managed list`

`gcloud compute instance-groups managed list-instances`

## 7. Computing with Kubernetes

### 7.1. Intro to containers

Containers offer a highly portable, lightweight means of distributing and scaling your application or workloads without replicating the guest OS. They can start/stop much faster (usually in seconds) and use fewer resources.

### 7.2. Introduction to Kubernetes Engine

Kubernetes – a containers orchestration system created and open sources by Google.

Kubernetes Engine – GCP's managed Kubernetes service. With this service, GCP customers can create and maintain their Kubernetes cluster without having to manage the Kubernetes platform.

Kubernetes runs containers on a cluster of virtual machines. It determines where to run containers, monitors the health of containers, and manages the full lifecycle of VM instances. This collection of tasks is called containers orchestration.

It may sound as if a Kubernetes cluster is similar to an instance group and there are some similarities:

- Both are sets of VMs that can be managed as a group.
- Instance group has some monitoring and can restart instances that fail, but Kubernetes has much more flexibility with regards to maintaining a cluster of servers.

Differences:

- Instance groups are much more restricted. All VMs generally run the same image in an instance group. That is not the case with Kubernetes.
- Also instance groups have no mechanism to support the deployment of containers

### 7.3. Kubernetes cluster architecture

Kubernetes architecture consists of several objects and a set of controllers.

Kubernetes cluster consists of cluster master and one or more nodes, which are the workers of the cluster.

The cluster master (ctrl plane) controls the cluster. The cluster master manages services provided by Kubernetes, such as Kubernetes API, controllers, and schedulers. All interactions with clusters are done through the master using the Kubernetes API. The cluster master issues the command that acts on a node. Users can interact with a cluster using the *kubectl* command.

Nodes execute the workloads run on the cluster. Nodes are VMs that run containers configured to run an application. The nodes run an agent called *kubelet*, which is the service that communicates with the cluster master. These VMs run specialized OS optimized to run containers.

Kubernetes organizes processes into workloads.

### 7.4. Kubernetes objects

- Pods
- Services
- Volumes
- Namespaces

#### 7.4.1. Pods

Pods are a single instance of a running process in a cluster. Pods contain at least one container. Pods use shared networking and storage across containers. Each pod gets a unique IP address and a set of ports. Containers connect to a port. Multiple containers in a pod connect to different ports and can talk to each other on localhost. A pod allows its containers to behave as if they are running on an isolated VM, sharing common storage, IP address, and asset of pods.

Pods are created in groups. Replicas are copies of a pod and constitute a group of pods that are managed as a unit.

Pods support autoscaling. Pods are considered ephemeral – they are expected to terminate. The mechanism that manages to scale and health monitoring is known as a controller.

Pods are similar to managed instance groups. A key difference is that pods are for executing applications in containers and may be placed in various nodes in the cluster, but MIG all execute the same application code on each of the nodes.

### 7.4.2. Services

Since pods are ephemeral and can be terminated by the controller, other services that depend on the pods, should not be coupled to particular pods. So Kubernetes provides a level of indirection b/w application running on pods and other applications that call them. It is called a service.

- ClusterIP. Exposes a service that is only accessible from within the cluster.
- NodePort. Exposes a service via a static port on each node’s IP.
- LoadBalancer. Exposes the service via the cloud provider’s load balancer.

### 7.4.3. ReplicaSet

Controller, that ensures s the correct number of identical pods are running. Also used to update and delete pods.

### 7.4.4. Deployment

Deployments are sets of identical pods. The pods all run the same application because they are created using the same pod template is a definition of how to run a pod aka pod specification.

### 7.5. Deploying Kubernetes Cluster + Deployment

- Create Cluster (control plane + nodes)
- Create Deployment
- Create services

1. Enable Kubernetes Engine API
2. Kubernetes Engine -> Clusters -> Create Cluster -> Standard Cluster -> Create
3. Kubernetes Engine -> Create Deployment -> Deploy
4. View generated YAML file

Command line for creating cluster:
`gcloud container clusters create CLUSTER_NAME –num-nodes=3 –region=us-central1`

Command line for creating deployment:
`kubectl run ch07-app-deploy –image=ch07-app –port=8080`

gcloud and kubectl have different command syntaxes:

- kubectl specify a verb and then a resource: `kubectl scale deployment …`
- gcloud specifies a resource before the verb: `gcloud container clusters create …`

### 7.6. Monitoring

When creating a cluster be sure to enable Stackdriver monitoring and logging by selecting advanced options in the create cluster form in the cloud console. Under additional features, choose Enable Stackdriver Logging Service and Enable Stackdriver Monitoring Service.

Select Stackdriver -> create a Workspace (support up to 100 projects) -> Monitor GCP Resources

## 8. Managing Kubernetes Clusters

### 8.1. Viewing the status of Kubernetes Clusters using Cloud Console

Kubernetes Engine -> Clusters -> Click on the name of the cluster to view:

- Details + Addons + Permissions (shows APIs) + Details of Node Pools (separate instance groups running in a Kubernetes cluster)
- Storage (persistent storage + storage classes)
- Nodes: list of nodes or  VMs running in the cluster
  - If you click on the node you will see CPU utilization, memory consumption, and disk IO + list of pods
    - If you click on the pod you will see the pod details (CPU utilization, memory consumption, and disk IO) + events + logs + status + containers
      - If you click on containers you will see containers information

To list clusters the names and basic information of all clusters:

`gcloud container clusters list`

To describe a particular cluster:

`gcloud container clusters describe CLUSTER_NAME --zone ZONE`

To list information about the nodes and pods you use the kubectl command.

Configure kubeconfig file:

`gcloud container clusters get_credentials CLUSTER_NAME --zone ZONE`

List/describe nodes:

`kubectl get nodes`

`kubectl describe nodes`

List/describe pods:

`kubectl get pods`

`kubectl describe pods`

### 8.2. Adding/ modifying. Removing nodes

KE -> Clusters -> Edit -> size = xxx

To resize the default node pool of an existing cluster, run:

`gcloud container clusters resize CLUSTER_NAME --size=5`

To enable autoscaling:

`gcloud container clusters update CLUSTER_NAME --enable-autoscaling`

### 8.3. Adding/ modifying. Removing pods

It is considered best practice not to manipulate pods directly. K8s will maintain the nr of pods specified for a deployment. If you want to change the nr of pods, you should change the deployment configuration.

Kubernetes Engine -> Workloads -> Select Deployment -> Actions -> Scale -> Nr of Replicas, or select autoscaling

To list deployments:

`kubectl get deployment`

To scale deployment:

`kubectl scale deployment DEPLOYMENT_NAME --replicas 5`

To add autoscale:

`Kubectl autoscale deployment DEPLOYMENT_NAME --min 1 --max 10 --cpu-percent 80`

### 8.4. Adding/modifying/removing services

Services are added through deployments.

Kubernetes Engine ->  Workloads -> click of deployment to view services -> Click on Services to view details and delete

To list services:

`kubectl get services`

To add services:

`kubectl run hello-server --image=IMAGE_PATH --port 8080`

To expose services:

`kubectl expose deployment hello-server –type=”LoadBalancer”`

To delete services:

`kubectl delete service hello-server`

### 8.5. Viewing the image repository

Container Registry -> Click on the Image name to see the version -> Click on a version to see the details

To list images:

`gcloud container images list`

`gcloud container images describe IMAGE_NAME`

## 9. Computing with App Engine

### 9.1. App Engine Components

App engine Standard applications consist of the following components:

- Application
- Service
- Version (source code (main .py)+ configuration file(conf.yaml))
- Instance

Each project can have an App Engine application. All resources associated with an App Engine app are created in the region specified when the app is created.

Apps have at least one Service, which is the code executed in the App Engine environment. Because multiple versions of an application’s codebase can exist, App Engine supports the versioning of the apps.
A service can have multiple versions, and these are usually slightly different, with newer versions having new features, bug fixes, and other changes.
Then a version executes it creates an instance of the app.

Services are typically structured to perform a single function with complex applications made up of multiple services known as microservices:

- One microservice may handle API requests for data access,
- while another microservice performs authentication and
- the third records data for billing purposes

Services are defined by their source code and configuration file. The combination of those files constitutes a version of the app.

### 9.2. Deploy App engine App

`gcloud components install app-engine-python`
`git clone https://github.com/GoogleCloudPlatform/python-docs-samples`
`cd python-docs-samples`

To deploy the app to App Engine:

`gcloud app deploy [DEPLOYABLES …]`

- if `[DEPLOYABLES …]` = app.yaml, then no need to provide it.
- `--version` - can specify the version nr
- `--project` - deploy to a specific project
- `--no-promote`, no traffic routed

To view the application in the browser:

`gcloud app browse`

To view application details in the console:

- Console -> App Engine:
  - Services – to view apps
  - Versions – To view deployed versions + migrate traffic
  - Instances – To view instance performance details to understand the load on the application

To stop application:

`gcloud app versions stop v1`

To disablement entire application in Console:

Console -> App Engine -> Settings -> Disable app

### 9.3. Scaling App Engine Applications

Instances are created to execute an application on an App Engine managed server. App Engine can automatically add/remove instances as needed based on load. When instances are scaled based on load, they are called dynamic instances. These dynamic instances help optimize your costs by shutting down when demand is low.

To specify scaling add a section to app.yaml

- automatic_scaling:
  - target_cpu_utilization – Max CPU
  - target_throughput_utilization – max nr of concurrent requests in %
  - max_concurrent_requests – max nr of concurrent requests
  - max_instances
  - min_instances
  - max_pending_latency max time a request will wait in queue, before new instance
  - min_pending_latency
- basic_scaling:
  - idle_timeout - when instance should be removed
  - max_instances
- manual_Scaling:
  - instances

### 9.4. Splitting traffic b/w App Engine versions

If you have more than one version of an application running, you can split traffic between the versions. App Engine provides three ways to split traffic: IP Address, HTTP cookie, and by random selection:

- IP address a client is always routed to the same split, as long as the IP address does not change.
- HTTP cookie – when you want to assign users to versions
- Random selection – when you want to evenly distribute the workload

`gcloud app services ser-traffic serv1 --splits v1=.4 v2=.6`

- `--splits`, to divide percentages of traffic b/w versions
- `--migrate`, App Engine should migrate traffic from the previous version
- `--split-by`=(IP, cookie,random)

You can also migrate traffic from the console. Navigate to the versions page and select the migrate command.
