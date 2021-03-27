- [1. Storage clients packages](#1-storage-clients-packages)
- [2. Introduction](#2-introduction)
- [3. Blobs](#3-blobs)
  - [3.1. Packages changes](#31-packages-changes)
  - [3.2. Packages dependencies to add](#32-packages-dependencies-to-add)
  - [3.3. Packages dependencies to remove](#33-packages-dependencies-to-remove)
  - [3.4. Code changes](#34-code-changes)
    - [3.4.1. Usings](#341-usings)
    - [3.4.2. Classes](#342-classes)
  - [3.5. Projects and solutions to apply](#35-projects-and-solutions-to-apply)
- [4. Queues](#4-queues)
  - [4.1. Packages changes](#41-packages-changes)
  - [4.2. Packages dependencies to add](#42-packages-dependencies-to-add)
  - [4.3. Code changes](#43-code-changes)
    - [4.3.1. usings](#431-usings)
    - [4.3.2. Classes](#432-classes)
  - [4.4. Projects and solutions to apply](#44-projects-and-solutions-to-apply)
  - [4.5. Web jobs](#45-web-jobs)
    - [4.5.1. Packages changes](#451-packages-changes)
    - [4.5.2. Code changes](#452-code-changes)
      - [4.5.2.1. Classes](#4521-classes)
      - [4.5.2.2. Projects and solutions to apply](#4522-projects-and-solutions-to-apply)

# 1. Storage clients packages

# 2. Introduction

# 3. Blobs

## 3.1. Packages changes

| Current packages (v11 for .NET) | Packages to install (v12 for .NET) |
| ------------------------------- | ---------------------------------- |
| Microsoft.Azure.Storage.Blob    | Azure.Storage.Blobs                |
| Microsoft.Azure.Storage.Common  | Azure.Storage.Common               |

## 3.2. Packages dependencies to add

In this case **_Azure.Storage.Blobs_** will be main package to install but all of its dependencies packages should be included inside the unique repository as concecuence the follow table contains all required packages in relation to Azure.Storage.Blobs.

- Azure.Core
- Azure.Storage.Blobs
- **Azure.Storage.Common**
- Microsoft.Bcl.AsyncInterfaces
- System.Buffers
- System.Diagnostics.DiagnosticSource
- System.Memory
- System.Numerics.Vectors
- System.Runtime.CompilerServices.Unsafe
- System.Text.Encodings.Web
- **System.Text.Json**
- System.Threading.Tasks.Extensions
- System.ValueTuple

## 3.3. Packages dependencies to remove

As concecuense of upgrate client packages related to blob storage, the following packages will be deprecated and should be removed from dependencies:

- Microsoft.Azure.Storage.Common
- Microsoft.Azure.Storage.Blob

## 3.4. Code changes

How you can see inside official azure blob documentation, the code related
to client libraries has three main diferences: libraries, usings and client
classes to connect, next sections will show this situation with more detail

https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-dotnet

https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-dotnet-legacy

### 3.4.1. Usings

| Usings to add with v11 for .NET | Usings to add with v12 for .NET |
| ------------------------------- | ------------------------------- |
| Microsoft.Azure.Storage         | Azure.Storage.Blobs             |
| Microsoft.Azure.Storage.Blob    | Azure.Storage.Blobs.Models      |

### 3.4.2. Classes

The next table contains main changes related to clases for blob storage

| Usings to add with v11 for .NET | Usings to add with v12 for .NET |
| ------------------------------- | ------------------------------- |
| CloudBlobClient                 | BlobServiceClient               |
| CloudBlobContainer              | BlobContainerClient             |

As example you can see the next to code's segments for conecting and using
blobs with v11 for .NET or with v12 for .NET:

Code fragment for create a blob container with v11 .Net

```
// use conection string to create an storageAccount
CloudStorageAccount storageAccount;
if (CloudStorageAccount.TryParse(storageConnectionString, out storageAccount))
{
    // Create the CloudBlobClient that represents the
    // Blob storage endpoint for the storage account.
    CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

    // Create a container called 'quickstartblobs' and
    // append a GUID value to it to make the name unique.
    CloudBlobContainer cloudBlobContainer =
    cloudBlobClient.GetContainerReference("quickstartblobs" +
        Guid.NewGuid().ToString());
await cloudBlobContainer.CreateAsync();
}
```

Code fragment for create a blob container with v12 .Net:

```
BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);

// Create the container and return a container client object
BlobContainerClient containerClient = await blobServiceClient.CreateBlobContainerAsync(containerName);
```

## 3.5. Projects and solutions to apply

Based on what has been seen in previous sections, the following table
contains a list with proyects and clases when the all software solucions
will be affected:

| Project                         | class                             |
| ------------------------------- | --------------------------------- |
| Microsoft.AEO.Services          | StorageBackendConfigurationGetter |
| Microsoft.AEO.Services          | HttpStorageMediaHandler           |
| Microsoft.AEO.Services          | BaseStorageService                |
| Microsoft.AEO.Services          | StorageService                    |
| Microsoft.AEO.CmdLets           | StorageHelper                     |
| Microsoft.AEO.CosmosToBlob      | Program                           |
| Microsoft.AEO.EmailSenderDirect | BlobService                       |
| Microsoft.AEO.EmailValidator    | Microsoft.AEO.EmailValidator      |

# 4. Queues

## 4.1. Packages changes

| Current packages (v11 for .NET)      | Packages to install (v12 for .NET)        |
| ------------------------------------ | ----------------------------------------- |
| Microsoft.Azure.Storage.Queue        | Azure.Storage.Queues                      |
| Microsoft.Azure.ConfigurationManager | system.Configuration.ConfigurationManager |

## 4.2. Packages dependencies to add

In Queues case, the oficial documentation refers to Azure.Storage.Queues and as
System.Configuration.ConfigurationManager main packages to add, but main packages
and dependencies need be included inside feed repositori, then the follow list
shows all dependencies related to this packages:

dependencies list for **_Azure.Storage.Queues_**:

- Azure.Core
- Azure.Storage.Common
- Microsoft.Bcl.AsyncInterfaces
- System.Buffers
- System.Diagnostics.DiagnosticSource
- System.Memory
- System.Memory.Data
- System.Numerics.Vectors
- System.Runtime.CompilerServices.Unsafe
- System.Text.Encodings.Web
- System.Text.Json
- System.Threading.Tasks.Extensions
- System.ValueTuple

dependencies list for **_System.Configuration.ConfigurationManager_**:

- System.Security.Principal.Windows
- System.Security.AccessControl
- System.Security.Permissions

## 4.3. Code changes

### 4.3.1. usings

| Usings to add with v11 for .NET | Usings to add with v12 for .NET |
| ------------------------------- | ------------------------------- |
| Microsoft.Azure                 | Azure.Storage.Queues            |
| Microsoft.Azure.Storage         | Azure.Storage.Queues.Models     |
| Microsoft.Azure.Storage.Queue   |                                 |

### 4.3.2. Classes

The next table contains main changes related to clases for queue's client code.

| Usings to add with v11 for .NET | Usings to add with v12 for .NET |
| ------------------------------- | ------------------------------- |
| CloudQueueClient                | queueClient                     |
| CloudQueue                      |                                 |

As example you can see the next to code's segments for conecting and using
queues with v11 for .NET or with v12 for .NET:

Code fragment for create a queue with v11 .Net

```
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference(queueName);

// Create the queue if it doesn't already exist
queue.CreateIfNotExists();
```

Code fragment for create a queue with v12 .Net:

```
string connectionString = ConfigurationManager.AppSettings["StorageConnectionString"];

// Instantiate a QueueClient which will be used to create and manipulate the queue
QueueClient queueClient = new QueueClient(connectionString, queueName);

// Create the queue
queueClient.CreateIfNotExists();
```

As you can notice in the code above shows that in .net v12 is enough with
the **_QueueClient_** to use the queue storage, as consequense the required
code is less than the .net v12 but need be modified after with the nuget packages upgrade

## 4.4. Projects and solutions to apply

| Project                             | class                      |
| ----------------------------------- | -------------------------- |
| Microsoft.AEO.Web                   | MediatorModule             |
| Microsoft.AEO.Web                   | ServicesModule             |
| Microsoft.AEO.Jobs.EmailProcessor   | ExperimentModule           |
| Microsoft.AEO.Jobs.MessageProcessor | PendingCommandHandler      |
| Microsoft.AEO.Jobs.MessageProcessor | RetryCommandHandler        |
| Microsoft.AEO.Jobs.MessageProcessor | JobModule                  |
| Microsoft.AEO.Jobs.QueueDepth       | JobModule                  |
| Microsoft.AEO.Jobs.StoppingMonitor  | RemoveBackendQueuesHandler |
| Microsoft.AEO.Jobs.StoppingMonitor  | JobModule                  |
| Microsoft.AEO.Jobs.Web              | MediatorModule             |
| Microsoft.AEO.Jobs.Web              | ServicesModule             |
| Microsoft.AEO.Jobs.Nurturer.Web     | MediatorModule             |
| Microsoft.AEO.Jobs.Nurturer.Web     | ServiceModule              |
| Microsoft.AEO.Helpers               | QueuedMediator             |
| Microsoft.AEO.Jobs.CommonModules    | StorageModule              |
| Microsoft.AEO.Services              | MonitoringService          |
| Microsoft.AEO.Services              | QueueSummaryModule         |
| Microsoft.AEO.Services              | QueueSummaryService        |

## 4.5. Web jobs

### 4.5.1. Packages changes

Microsoft.Azure.WebJobs.Core

Dependencies for Microsoft.Azure.WebJobs.Core:

- System.ComponentModel.Annotations
- System.Diagnostics.TraceSource
- Microsoft.Azure.WebJobs.Core
- System.Diagnostics.TraceSource

Dependencies for Microsoft.Azure.WebJobs:

- Microsoft.Bcl.AsyncInterfaces.1.0.0
- Microsoft.Extensions.Configuration.2.1.1
- Microsoft.Extensions.Configuration.Abstractions.2.1.1
- Microsoft.Extensions.Configuration.Binder.2.1.1
- Microsoft.Extensions.Configuration.EnvironmentVariables.2.1.0
- Microsoft.Extensions.Configuration.FileExtensions.2.1.0
- Microsoft.Extensions.Configuration.Json.2.1.0
- Microsoft.Extensions.DependencyInjection.2.1.0
- Microsoft.Extensions.DependencyInjection.Abstractions.2.1.1
- Microsoft.Extensions.FileProviders.Abstractions.2.1.0
- Microsoft.Extensions.FileProviders.Physical.2.1.0
- Microsoft.Extensions.FileSystemGlobbing.2.1.0
- Microsoft.Extensions.Hosting.2.1.0
- Microsoft.Extensions.Hosting.Abstractions.2.1.0
- Microsoft.Extensions.Logging.2.1.1
- Microsoft.Extensions.Logging.Abstractions.2.1.1
- Microsoft.Extensions.Logging.Configuration.2.1.0
- Microsoft.Extensions.Options.2.1.1
- Microsoft.Extensions.Options.ConfigurationExtensions.2.1.0
- Microsoft.Extensions.Primitives.2.1.1
- System.Buffers.4.5.0
- System.Memory.4.5.3
- System.Memory.Data.1.0.1
- System.Numerics.Vectors.4.5.0
- System.Runtime.CompilerServices.Unsafe.4.6.0
- System.Text.Encodings.Web.4.6.0
- System.Text.Json.4.6.0
- System.Threading.Tasks.Dataflow.4.8.0
- System.Threading.Tasks.Extensions.4.5.2
- System.ValueTuple.4.5.0
- Microsoft.Extensions.Configuration.Abstractions
- Microsoft.Extensions.FileSystemGlobbin
- Microsoft.Extensions.Configuration.Binder
- Microsoft.Extensions.Configuration
- Microsoft.Extensions.Configuration.Json
- Microsoft.Extensions.Hosting.Abstractions
- Microsoft.Extensions.FileProviders.Physical
- Microsoft.Extensions.Configuration.EnvironmentVariables
- System.Memory.Data

### 4.5.2. Code changes

#### 4.5.2.1. Classes

#### 4.5.2.2. Projects and solutions to apply
