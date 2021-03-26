- [1. Storage clients packages](#1-storage-clients-packages)
- [2. Introduction](#2-introduction)
  - [2.1. Blobs](#21-blobs)
    - [2.1.1. Packages changes](#211-packages-changes)
    - [2.1.2. Packages dependencies to add](#212-packages-dependencies-to-add)
    - [2.1.2. Packages dependencies to remove](#212-packages-dependencies-to-remove)
    - [2.1.3. Code changes](#213-code-changes)
      - [2.1.3.1. Usings](#2131-usings)
      - [2.1.3.1. Classes](#2131-classes)
      - [2.1.3.2. Projects and solutions to apply](#2132-projects-and-solutions-to-apply)
  - [2.2. Queues](#22-queues)
    - [2.2.1. Packages changes](#221-packages-changes)
    - [2.2.2. Code changes](#222-code-changes)
      - [2.2.2.1. Classes](#2221-classes)
      - [2.2.2.2. Projects and solutions to apply](#2222-projects-and-solutions-to-apply)
  - [2.3. Web jobs](#23-web-jobs)
    - [2.3.1. Packages changes](#231-packages-changes)
    - [2.3.2. Code changes](#232-code-changes)
      - [2.3.2.1. Classes](#2321-classes)
      - [2.3.2.2. Projects and solutions to apply](#2322-projects-and-solutions-to-apply)

# 1. Storage clients packages

# 2. Introduction

## 2.1. Blobs

### 2.1.1. Packages changes

| Current packages (v11 for .NET) | Packages to install (v12 for .NET) |
| ------------------------------- | ---------------------------------- |
| Microsoft.Azure.Storage.Blob    | Azure.Storage.Blobs                |
| Microsoft.Azure.Storage.Common  | Azure.Storage.Common               |

### 2.1.2. Packages dependencies to add

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

### 2.1.2. Packages dependencies to remove

As concecuense of upgrate client packages related to blob storage, the following packages will be deprecated and should be removed from dependencies:

- Microsoft.Azure.Storage.Common
- Microsoft.Azure.Storage.Blob

### 2.1.3. Code changes

How you can see inside official azure blob documentation, the code related
to client libraries has three main diferences: libraries, usings and client
classes to connect, next sections will show this situation with more detail

https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-dotnet

https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-dotnet-legacy

#### 2.1.3.1. Usings

| Usings to add with v11 for .NET | Usings to add with v12 for .NET |
| ------------------------------- | ------------------------------- |
| Microsoft.Azure.Storage         | Azure.Storage.Blobs             |
| Microsoft.Azure.Storage.Blob    | Azure.Storage.Blobs.Models      |

#### 2.1.3.1. Classes

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

#### 2.1.3.2. Projects and solutions to apply

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

## 2.2. Queues

### 2.2.1. Packages changes

### 2.2.2. Code changes

#### 2.2.2.1. Classes

#### 2.2.2.2. Projects and solutions to apply

## 2.3. Web jobs

### 2.3.1. Packages changes

### 2.3.2. Code changes

#### 2.3.2.1. Classes

#### 2.3.2.2. Projects and solutions to apply
