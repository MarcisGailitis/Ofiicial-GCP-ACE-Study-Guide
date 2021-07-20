# Official GCP ACE study guide ch10 - ch18

## Table of content

10. Computing with Cloud Functions
11. Planning Storage in the Cloud

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

For each of the events that can occur you define a trigger – which a way of responding to an event.

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
