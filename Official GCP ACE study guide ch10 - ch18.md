# Official GCP ACE study guide ch10 - ch18

## Table of content

10. Computing with Cloud Functions
11. Planning Storage in the Cloud
12. Deploying Storage in Google Cloud Platform
13. Loading Data into Storage
14. Networking in the Cloud: VPC and VPN

## 10. Computing with Cloud Functions

### 10.1. Intro to cloud functions

Cloud Functions is a serverless compute service, provided by GCP.

- Cloud function is similar to App Engine, in that they are both serverless.
- A primary difference, though is that App Engine supports multiple services organized into a single application, while Cloud functions support individual services that are managed and operate independently of other services.

- Cloud Function = single-purpose function
- App Engine = multifunctional application

Example 1: your department may upload a daily data extract from a database, which is then loaded into an enterprise data warehouse. If the data extract files are loaded into Cloud Storage, then you could use a function to perform preprocessing, such as verifying the file is in the right format and meets other business rules. If the data passes checks, a message is written to Pub/Sub topic, which is read by the data warehouse load process. Cloud Functions allows developers to decouple the initial data quality check from the rest of the extraction, transformation, and load process.

Example2:  functions can be used to automate the OCR process. 1. Function determines if a file is searchable. 2) If y, writes the location into pub/sub.

### 10.2. Events, Triggers and Functions

Events are a particular action that happens in Google Cloud, such as a file is uploaded to Cloud Storage or a message (called a topic) is written to a Pub/Sub message queue. Currently, Cloud Functions support events in five categories:

- Cloud Storage – including uploading, deleting, archiving data, and metadata update
- Cloud Pub/Sub – event for publishing a message
- HTTP – POST, GET, etc
- Firebase – actions executed in Firebase database
- Stackdriver Logging – set up a function to respond to change in Stackdriver Logging

For each of the events that can occur you define a trigger – which is a way of responding to an event.

Triggers have an associated function. The function is passed arguments with data about the event. The functions execute in response to the event.

### 10.3. Runtime environment

Functions run in their environments. Each time a function is invoked it is run in a separate instance from all other invocations.

### 10.4. Deploying  a Cloud function for Cloud Storage

Cloud Console -> Cloud functions -> Create:

- Function Name
- Memory allocation – the amount of memory that will be available for the function 128 MB to 8 GB
- Timeout: 60
- Runtime: Py, JS, Go, Java, .NET, Ruby, PHP
- Trigger
- Event Type
- Source Code
- Runtime

To create a cloud function:

`gcloud functions deploy FUNCTION_NAME --runtime RUNTIME_VERSION --trigger-resource CLOUD_STORAGE_BUCKET --trigger` google.storage.object.(finalize, delete, archive, metadataUpdate)

To delete a cloud function:

`gcloud functions delete FUNCTION_NAME`

### 10.5. Deploying  a Cloud function for Pub/Sub

To create a cloud function:

`gcloud functions deploy FUNCTION_NAME --runtime RUNTIME_VERSION --trigger-topic PUB_SUB_TOPIC`

## 11. Planning Storage in the Cloud

### 11.1. Cache

- A cache is an in-memory data store designed to provide applications with sub-millisecond access to data.
  - low latency
  - limited in size, volatile
Memorystore – a GCP-managed open-source Redis service with 1GB to 300 GB memory. Can be used with applications running in Compute, App Engine, and K8s.

### 11.2. Persistent Storage

Persistent Storage is durable block storage. Persistent disks can be attached to VM in GCE and GKE. Since persistent disks are block storage devices, you can create file systems on these devices.

Persistent disks are not directly attached to physical servers hosting your VMs but are network accessible.

Persistent disks exist independently of VMs, local attached SSDs are not:

- VMs can have locally attached SSDs, but the data on those drives is lost, when the VM is terminated.
- The data on persistent disks continues to exist after VMs are shut down and terminated.

Persistent disks are available in SSD and hard disk drive (HDD) configurations:

- SSDs are used when high throughput is important.
- HHDs have longer latencies but cost less, so HHDs are a good option for storing large amounts of data and performing batch operations.
- Persistent disks can be mounted on multiple VMs to provide multi-reader storage
- The size of persistent disks can be increased while mounted to a VM

A persistent disk can be created blank or from an image or snapshot:

- Use the **image** option if you want to create  a persistent **boot disk**
- Use a **snapshot** if you want to create a replica of **data disk**

### 11.3. Object Storage

- Cache - for storing small amounts of data with sub-millisecond latency
- Persistent disk – to store up to 64TB on a single disk and provide up to hundreds of IOPS for r/w operations
- Object Storage – to store large volumes of data up to exabytes and share it widely.

Object storage system means that files stored in the systems are treated as atomic units, you cannot operate on part of the file. Cloud storage is well suited for storing large volumes of data w/o requiring any consistent data structure. You can store different types of data in the bucket, which is the logical unit of organization in Cloud Storage.

Object storage does not provide a filesystem. Buckets are like directories, in that they help organize objects into groups. Buckets are not true directories that support features such as subdirectories.

Multiregional and regional objects can be changed to nearline or coldline. Nearline can be changed only to coldline. Coldline cannot be changed.

### 11.4. SQL

Relational databases like Cloud SQL, Cloud Spanner support database transactions. A transaction is a set of operations that are guaranteed to succeed or fail in its entirety – there is no chance that some operations are executed and others are not.

Cloud SQL is used for databases that do not need to scale horizontally – by adding additional servers to the cluster. Cloud SQL scale vertically – by adding more memory and CPU to servers.

- Cloud SQL – web apps, Business intelligence, and eCommerce
- Cloud Spanner – global supply chains and financial services applications
- BigQuery – a managed analytics service - a service designed for data warehouse and analytical applications

### 11.5. NoSQL

NoSQL databases do not use the relational model and do not require a fixed structure or schema. When no fixed schema is required, developers have the option to store different attributes in different records.

- Cloud Firestore –uses a document data model. Cloud Firestore is designed for storing, sync, and querying data across distributed applications. Cloud Firestore supports transactions and provides multiregional replication.
- Cloud Bigtable – uses a wide-column data model. Wide-column databases store tables that can have a wide number of columns. Not all rows need to use all the columns. Consistent low-millisecond latency. Designed for petabyte-scale databases, IoT data, analytics processing, data science applications. Designed for applications with high data volumes, and high-velocity ingest of data. Time-series, IoT, Financial, etc.

## 12. Deploying Storage in Google Cloud Platform

### 12.1. MySQL

Connect to instance:

`gcloud sql connect [INSTANCE_NAME] --user=[USERNAME]`

it is a good practice not to specify passwords in the command line.

In the MySQL command-line environment, you use MySQL commands not gcloud commands:

```sql
CREATE DATABASE database_name;
USE database_name;
CREATE TABLE books …;
INSERT INTO books …;
SELECT * FROM books;
```

### 12.2. MySQL backup

Cloud SQL enables both on-demand and automatic backups.

- Cloud SQL -> click in instance -> backups -> create backup

Create on-demand backup:

`gcloud sql backups create --instance [INSTANCE_NAME] --async`

create scheduled backup:

`gcloud sql instances patch [INSTANCE_NAME] --backup-start-time [HH:MM]`

### 12.3. Datastore

You add data to the Datastore database using the Entities option in the Datastore section of the console. The entity data structure is analogous to the schema in relational databases. You create an entity by clicking Create Entity and filling the form that appears. After creating entities you can query the document database using GQL (Graph Query Language), a query language similar to SQL.

### 12.4. Datastore backup

1. Create a storage bucket to keep the backup
2. `gcloud datastore export --namespaces=’(default)’  gs://[BUCKET_NAME]`
3. `gcloud datastore import gs://exampleBucket/exampleExport/exampleFileName.overall_export_metadata`

### 12.5. BigQuery cost estimation

BigQuery is a fully managed service, so Google takes care of backups and other basic administrative tasks.

1. BigQuery provides an estimate in Query Editor lower right corner of how much data will be scanned.
2. `bq query [SQL_QUERY] --location=[LOCATION] --use_legacy_sql=false --dry_run`
3. You can use this number with the Pricing Calculator to estimate the costs.

### 12.6. BigQuery Jobs

Jobs in BigQuery are processes used to load, export, copy, or query data. Jobs are automatically created when you start any of these operations.

- BigQuery -> Job History
- Issue the bq show command with the -j flag and a job ID.
- `bq show -j my-project-1234:US.bquijob_123x456_123y123z123c`

### 12.7. Cloud Spanner

Create Instance & Database:

- Cloud Spanner -> Create Instance -> Create Database (using SQL data definition language)

Add data:

- Table Details -> Data -> Insert

### 12.8. Cloud Pub/Sub

There are 2 tasks required to deploy Pub/sub message queues:

- creating a topic – a structure where apps can send messages. Pub/Sub receives the message and keeps it then until they are read by an application.
- creating a subscription – apps read messages using subscription.

Create a topic:

- Pub/Sub -> Create a Topic -> [TOPIC_NAME] -> create
- `gcloud pubsub topics create [TOPIC_NAME]`

Create a subscription:

- Topic name -> three dots -> New Subscription -> [SUBSCRIPTION NAME] +delivery type=(push, pull) -> create
- `gcloud pubsub subscription create [SUBSCRIPTION_NAME] --topic [TOPIC_NAME]`

### 12.9. Bigtable

Bigtable -> Create Instance

To create a table you have to use the command line

```shell
- gcloud components update
- gcloud components install cbt
- echo instance = ace-exam-bigtable >> ~/.cbtrc
- cbt createtable [TABLE_NAME]
```

list tables:

cbt ls

Add column family:

`cbt createfamily [TABLE_NAME] [COLUMN_FAMILY_NAME]`

Add value to the cell:

`cbt set [TABLE_NAME] row1 [COLUMN_FAMILY_NAME]:col1=[CELL_VALUE]`

Display contents of the table:

`cbt read [TABLE_NAME]`

### 12.10. Dataproc

Google Dataproc is Google’s managed Apache Spark and Apache Hadoop services.

- Apache Spark = data analysis and machine learning
- Apache Hadoop = well suited to batch big data applications

Dataproc -> Create Cluster -> Specify:

- Name
- Region, zone
- Cluster mode
- Master node
- Worker nodes (including local SSDs)

`gcloud dataproc clusters create [CLUSTER_NAME] --zone [ZONE]`

Dataproc -> Jobs -> Specify:

- Job Type = Spark, PySpark, SparkR, Hive, Spark SQL, Pig, Hadoop

`gcloud dataproc jobs submit spark --cluster [CLUSTER_NAME] --jar [JAR_FILE]`

### 12.11. Cloud Storage

Change object storage class:

`gsutil rewrite -s [STORAGE_CLASS] gs://[PATH_TO_OBJECT]`

Move or rename object b/w buckets:

`gsutil mv gs://[BUCKET]/[OBJECT] gs://[BUCKET]/[OBJECT]`

## 13. Loading Data into Storage

### 13.1. Cloud Storage

`gsutil cp [LOCAL_OBJECT_LOCATION] gs://[BUCKET_NAME]/`

### 13.1. Cloud SQL

Cloud SQL -> Instance Details -> Import/Export -> CSV or SQL

### 13.2. Cloud SQL command line

Find service account for Cloud SQL:

`gcloud sql instances describe [INSTANCE_NAME]`

Add Cloud SQL write permission to bucket:

`gsutil acl ch -u [SERVICE_ACCOUNT_ADDRESS]:W gs://[BUCKET_NAME]`

Export:

`gcloud sql export sql [INSTANCE_NAME] gs:// [BUCKET_NAME]/[FILE_NAME –database=[DATABASE_NAME]`

### 13.3. Cloud Datastore

Only through the command line. The same process as creating backups in the previous chapter. The export process will create a folder and the folder will contain a metadata file and a folder containing the exported data.

### 13.4. BigQuery

Export formats: CSV, json, and AVRO

`bq extract –destination_format=[FORMAT] [DATASET].[TABLE] gs://[BUCKET]/[FILENAME]`

Import formats: CSV, JSON, Parquet, PRC, and Cloud Datastore Backup

`bq load –source_format=[FORMAT] [DATASET].[TABLE] gs://[BUCKET]/[FILENAME]`

### 13.5. Cloud Spanner

Cloud Spanner -> Instance Details -> Import/Export using Dataflow solution-> CSV or Avro

### 13.6. Cloud Bigtable

Cloud Bigtable does not have an Export and Import option in the GCP Console or gcloud. You have  2 options – using a Java application or using the HBase interface to execute HBase commands.

### 13.7. Dataproc

Cloud Dataproc does have import/export commands to save/restore cluster configuration data.

```shell
gcloud components install beta
gcloud dataproc clusters export [CLUSTER_NAME] --destination=[PATH_TO_FILE]
gcloud dataproc clusters import [PATH_TO_FILE]
```

### 13.8. Pub/Sub

`gcloud pubsub topics publish [TOPICS_NAME] --message [MESSAGE]`
`gcloud pubsub subscription pull  [SUBSCRIPTION_NAME] --auto-ack`

## 14. Networking in the Cloud: VPC and VPN

### 14.1. Into to VPC

VPCs are software versions of physical networks that link resources in a project. GCP automatically creates VPC when you create a project. You can create additional VPCs and modify the VPC created by GCP.

VPCs are global resources, so they are not tied to a specific region or zone. VPC contains subnets which are regional resources. Subnets have a range of IP addresses associated with them. Subnets provide private internal addresses. Resources use IP addresses to communicate with each other and with Google APIs.

In addition, you can create **shared VPC** within an organization. The shared VPC is hosted in a common project.  Users can create resources in the shared VPC.

You can also use **VPC peering** for interproject connectivity, even if an organization is not defined.

### 14.2. Creating a VPC from Console

- VPC Network -> [NAME]+[DESCRIPTION]
- Subnets -> Automatic / Custom -> Private Google access & Flow logs
- Firewall rules -> Dynamic Routing mode -> DNS server policy
  - Regional routing will have Google Cloud Routers learn routes within a region.
  - Global routing will enable Google Cloud Routers to learn routes on all subnetworks in the VPC.

### 14.3. Creating VPC with gcloud

`gcloud compute networks create [NETWORK_NAME] --subnet-mode=auto`

`gcloud compute networks create [NETWORK_NAME] --subnet-mode=custom`
`gcloud compute networks subnets create [SUBNET_NAME] --network=[NETWORK_NAME] --region=[REGION] --range=[CIDR_RANGE]`

Classless interdomain routing (CIDR) allows network admins to define networks with the number of addresses that they need.

CIDR address consists of 2 sets of numbers a network address for identifying a subnet and a host identifier.

Network addresses:

- 10.0.0.0
- 172.16.0.0
- 192.168.0.0

Host identifier (/29, /28, etc.) indicates how many bits of an IP address (32 bits) to allocate to the network mask, which determines which addresses are within the block of the addresses and which are not. /29 identifies that 29 bits are used for specifying a network and 3 bits are used to specify the host address. With 3 bits you can create up to 8 addresses.

### 14.4. Creating a shared VPC using gcloud

Assign Shared VPC Admin role to org or folder level:

`gcloud organization add-iam-policy binding [ORG_ID] --member=’user:[EMAIL_ADDRESS]’ --role=’roles/compute.xpnAdmin’`

Enable shared VPC:

`gcloud compute shared-vpc enable [HOST_PROJECT_ID]`

Add projects to shared VPC:

`gcloud compute shared-vpc associate project add [SERVICE_PROJECT_ID] --host-project=[HOST_PROJECT_ID]`

### 14.5. VPC Peering

Alternatively, VPC peering can be used for interproject traffic, when an organization does not exist. VPC peering implemented using the gcloud compute networks peering create command.

`gcloud compute networks peering create [NAME] --network=[NETWORK] --peer-network=[PEER_NETWORK] --auto-create-routes`

### 14.6. Deploy VM with a Custom Network

`gcloud compute instances create[INSTANCE_NAME] --subnet=[SUBNET] --zone=[ZONE]`

### 14.7. Firewalls

Rules with the highest priority are applied first. Any rule with lower priority that matches is not applied. Priority is specified by an integer from 0 (highest) to 65535 (lowest).

A VPC starts with 2 implied rules:

- Allows egress to all destinations with priority 65535
- Denies ingress from any source with priority 65535

When VPC is automatically created, it is created with 4 default rules:

- Allow SSH 22 with priority 65534
- Allow RDP 3389 with priority 65534
- Allow ICMP with priority 65534
- Allow ingress from the same network with priority 65534

### 14.8. Firewall in gcloud

`gcloud compute firewall-rules create [NAME] --network=[NETWORK_NAME] --allow tcp:20000-25000`

### 14.9. VPN

1. Hybrid Connectivity -> VPN -> Create VPN Connection
2. Reserve a static IP address
3. Add Tunnel – to configure another network endpoint in the VPN. 3 routing options:
    a. Dynamic (BGP) + Add Cloud Router + ASN number
    b. Route-based
    c. Policy-based

`gcloud compute target-vpn-gateways`
`gcloud compute forwarding-rule`
`gcloud compute vpn-tunnels`
