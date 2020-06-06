---
title: .NET 'te blob anlık görüntüsü oluşturma ve yönetme
titleSuffix: Azure Storage
description: Blob verilerini belirli bir anda yedeklemek için Blobun salt okunurdur bir anlık görüntü oluşturmayı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 04/02/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: be3d06d8ea493ab7c246ace5c49d5e5bc0036108
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84463544"
---
# <a name="create-and-manage-a-blob-snapshot-in-net"></a>.NET 'te blob anlık görüntüsü oluşturma ve yönetme

Anlık görüntü, bir Blobun zaman içinde alınmış bir salt okunurdur. Bu makalede, [.net Için Azure Storage istemci kitaplığı](/dotnet/api/overview/azure/storage?view=azure-dotnet)'nı kullanarak blob anlık görüntülerinin oluşturulması ve yönetilmesi gösterilmektedir.

Azure depolama 'daki blob anlık görüntüleri hakkında daha fazla bilgi için bkz. [.net 'te blob anlık görüntüsü oluşturma ve yönetme](snapshots-overview.md).

## <a name="create-a-snapshot"></a>Anlık görüntü oluşturma

# <a name="net-version-12x"></a>[.NET sürüm 12. x](#tab/v12)

.NET için Azure depolama istemci kitaplığı 'nın 12. x sürümünü kullanarak bir blok blobunun anlık görüntüsünü oluşturmak için aşağıdaki yöntemlerden birini kullanın:

- [CreateSnapshot](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.createsnapshot)
- [Createanlık görüntüsünün Tasync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.createsnapshotasync)

Aşağıdaki kod örneği, 12. x sürümüne sahip bir anlık görüntü oluşturmayı gösterir. İstekleri hizmete yetkilendirmek için Azure AD kimlik bilgilerinizi kullanmak üzere [Azure. Identity](https://www.nuget.org/packages/azure.identity) kitaplığına bir başvuru ekleyin.

```csharp
private static async Task CreateBlockBlobSnapshot(string accountName, string containerName, string blobName, Stream data)
{
    const string blobServiceEndpointSuffix = ".blob.core.windows.net";
    Uri containerUri = new Uri("https://" + accountName + blobServiceEndpointSuffix + "/" + containerName);

    // Get a container client object and create the container.
    BlobContainerClient containerClient = new BlobContainerClient(containerUri,
        new DefaultAzureCredential());
    await containerClient.CreateIfNotExistsAsync();

    // Get a blob client object.
    BlobClient blobClient = containerClient.GetBlobClient(blobName);

    try
    {
        // Upload text to create a block blob.
        await blobClient.UploadAsync(data);

        // Add blob metadata.
        IDictionary<string, string> metadata = new Dictionary<string, string>
        {
            { "ApproxBlobCreatedDate", DateTime.UtcNow.ToString() },
            { "FileType", "text" }
        };
        await blobClient.SetMetadataAsync(metadata);

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // You can specify metadata at the time that the snapshot is created.
        // If no metadata is specified, then the blob's metadata is copied to the snapshot.
        await blobClient.CreateSnapshotAsync();
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

# <a name="net-version-11x"></a>[.NET sürüm 11. x](#tab/v11)

.NET için Azure depolama istemci kitaplığı 'nın 11. x sürümünü kullanarak bir blok blobunun anlık görüntüsünü oluşturmak için aşağıdaki yöntemlerden birini kullanın:

- [CreateSnapshot](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshot)
- [Createanlık görüntüsünün Tasync](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshotasync)

Aşağıdaki kod örneği, 11. x sürümüne sahip bir anlık görüntünün nasıl oluşturulacağını göstermektedir. Bu örnek, bir anlık görüntü oluşturulduğunda diğer meta verileri belirtir.

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // You can specify metadata at the time that the snapshot is created.
        // If no metadata is specified, then the blob's metadata is copied to the snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
        Console.WriteLine(snapshot.SnapshotQualifiedStorageUri.PrimaryUri);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

---

## <a name="delete-snapshots"></a>Anlık görüntüleri Sil

Bir blobu silmek için, önce söz konusu Blobun anlık görüntülerini silmeniz gerekir. Bir anlık görüntüyü tek tek silebilir veya kaynak blobu silindiğinde tüm anlık görüntülerin silineceğini belirtebilirsiniz. Anlık görüntülere sahip olan bir blobu silmeye çalışırsanız hata oluşur.

# <a name="net-version-12x"></a>[.NET sürüm 12. x](#tab/v12)

.NET için Azure depolama istemci kitaplığı 'nın 12. x sürümünü kullanarak bir blobu ve anlık görüntülerini silmek için aşağıdaki yöntemlerden birini kullanın ve [Deletesnapshot Tsoption](/dotnet/api/azure.storage.blobs.models.deletesnapshotsoption) sabit listesini ekleyin:

- [Sil](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.delete)
- [DeleteAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.deleteasync)
- [Deleteıfexists](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.deleteifexists)
- [Deleteıfexistsasync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.deleteifexistsasync)

Aşağıdaki kod örneği, .NET 'teki bir Blobun ve anlık görüntülerinin nasıl silineceğini ve burada `blobClient` [blobclient](/dotnet/api/azure.storage.blobs.blobclient)türünde bir nesne olduğunu gösterir:

```csharp
await blobClient.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, default);
```

# <a name="net-version-11x"></a>[.NET sürüm 11. x](#tab/v11)

.NET için Azure depolama istemci kitaplığı 'nın 11. x sürümünü kullanarak bir blobu ve anlık görüntülerini silmek için aşağıdaki blob silme yöntemlerinden birini kullanın ve [Deletesnapshot Tsoption](/dotnet/api/microsoft.azure.storage.blob.deletesnapshotsoption) sabit listesini ekleyin:

- [Sil](/dotnet/api/microsoft.azure.storage.blob.cloudblob.delete)
- [DeleteAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteasync)
- [Deleteıfexists](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexists)
- [Deleteıfexistsasync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexistsasync)

Aşağıdaki kod örneği, .NET 'teki bir Blobun ve anlık görüntülerinin nasıl silineceğini gösterir; burada, `blockBlob` [CloudBlockBlob] [dotnet_CloudBlockBlob] türünde bir nesnedir:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

---

## <a name="next-steps"></a>Sonraki adımlar

- [Blob anlık görüntüleri](snapshots-overview.md)
- [Blob sürümleri (Önizleme)](versioning-overview.md)
- [Bloblar için geçici silme](storage-blob-soft-delete.md)