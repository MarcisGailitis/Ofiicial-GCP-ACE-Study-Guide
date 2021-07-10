# Official GCP ACE study guide ch10 - ch18

## Table of content

10. Computing with Cloud Functions

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

Events are a particular action that happens in Google Cloud, such as file is uploaded to Cloud Storage or a message (called topic) is written to a Pub/Sub message queue. Currently, Cloud Functions support events in five categories:

- Cloud Storage – including uploading, deleting, archiving data, and metadata update
- Cloud Pub/Sub – event for publishing a message
- HTTP – POST, GET, etc
- Firebase – actions taken in Firebase database 
- Stackdriver Logging – set up a function to respond to change in Stackdriver Logging

For each of the events that can occur you define a trigger – which a way of responding to an event.

Triggers have an associated function. The function is passed arguments with data about the event. The functions execute in response to the event.

### 10.3. Runtime environment

Functions run in their own environments. Each time a function is invoked it is run in a separate instance from all other invocations.

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

### Deploying  a Cloud function for Pub/Sub

To create a cloud function:

`gcloud functions deploy FUNCTION_NAME --runtime RUNTIME_VERSION --trigger-topic PUB_SUB_TOPIC`
