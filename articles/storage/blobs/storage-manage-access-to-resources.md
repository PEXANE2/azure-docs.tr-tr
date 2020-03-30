---
title: Kapsayıcılar ve lekeler için genel okuma erişimini yönetme
titleSuffix: Azure Storage
description: Kapsayıcıları ve blob'ları anonim erişim için nasıl kullanabileceğizi ve bunları programlı olarak nasıl erişirecek olarak öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.openlocfilehash: 4d9a54c220861b19d67b07998e609ee72897446a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255489"
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Kapsayıcılara ve blob’lara anonim okuma erişimini yönetme

Azure Blob depolamada bir kapsayıcıya ve bloblarına anonim, genel okuma erişimini etkinleştirebilirsiniz. Bunu yaparak hesap anahtarınızı paylaşmadan ve paylaşılan erişim imzasına (SAS) gerek kalmadan bu kaynaklara salt okuma erişimi verebilirsiniz.

Herkese açık okuma erişimi, belirli lekelerin her zaman anonim okuma erişimi için kullanılabilir olmasını istediğiniz senaryolar için en iyisidir. Daha ayrıntılı denetim için paylaşılan bir erişim imzası oluşturabilirsiniz. Paylaşılan erişim imzaları, belirli bir süre için farklı izinleri kullanarak kısıtlı erişim sağlamanıza olanak tanır. Paylaşılan erişim imzaları oluşturma hakkında daha fazla bilgi için [bkz.](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Anonim kullanıcılara kapsayıcılara ve bloblara izin verme

Varsayılan olarak kapsayıcıya ve içindeki bloblara yalnızca uygun izinlerin verilmiş olduğu kullanıcılar erişebilir. Anonim kullanıcılara kapsayıcı ve blobları üzerinde okuma erişimi vermek için, kapsayıcıya genel erişim düzeyini ayarlayabilirsiniz. Kapsayıcıya genel erişim verdiğinizde anonim kullanıcılar isteği yetkilendirmeden genel olarak erişilebilen kapsayıcının içindeki blobları okuyabilir.

Bir kapsayıcıyı aşağıdaki izinlerle yapılandırabilirsiniz:

- **Herkese açık okuma erişimi yok:** Konteyner ve lekeleri yalnızca depolama hesabı sahibi tarafından erişilebilir. Bu, tüm yeni kapsayıcılar için varsayılan dır.
- **Yalnızca blobs için genel okuma erişimi:** Kapsayıcıiçindeki lekeler anonim istekle okunabilir, ancak kapsayıcı verileri kullanılamaz. Anonim istemciler konteyner içindeki lekeleri sayısalolarak taslayamaz.
- **Konteyner ve lekeleri için genel okuma erişimi:** Tüm konteyner ve blob verileri anonim istek le okunabilir. İstemciler, anonim istekle kapsayıcıiçindeki lekeleri sayabilir, ancak depolama hesabı içindeki kapsayıcıları sayamaz.

### <a name="set-container-public-access-level-in-the-azure-portal"></a>Azure portalında kapsayıcı genel erişim düzeyini ayarlama

Azure [portalından,](https://portal.azure.com)bir veya daha fazla kapsayıcı için genel erişim düzeyini güncelleştirebilirsiniz:

1. Azure portalındaki depolama hesabınıza genel bakışa gidin.
1. Menü bıçak **blob hizmeti** altında, **Blobs**seçin.
1. Ortak erişim düzeyini ayarlamak istediğiniz kapsayıcıları seçin.
1. Genel **erişim** ayarlarını görüntülemek için erişim düzeyini değiştir düğmesini kullanın.
1. **Genel erişim düzeyi** açılır tarafından istenen ortak erişim düzeyini seçin ve değişikliği seçili kapsayıcılara uygulamak için Tamam düğmesini tıklatın.

Aşağıdaki ekran görüntüsü, seçili kapsayıcılar için genel erişim düzeyinin nasıl değiştirilebildiğini gösterir.

![Portalda genel erişim düzeyinin nasıl ayarlanını gösteren ekran görüntüsü](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

> [!NOTE]
> Tek bir blob için ortak erişim düzeyini değiştiremezsiniz. Genel erişim düzeyi yalnızca kapsayıcı düzeyinde ayarlanır.

### <a name="set-container-public-access-level-with-net"></a>.NET ile kapsayıcı genel erişim düzeyini ayarlama

.NET için Azure Depolama istemci kitaplığını kullanan bir kapsayıcıiçin izin ayarlamak için, önce aşağıdaki yöntemlerden birini arayarak kapsayıcının varolan izinlerini alın:

- [Getpermissions](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getpermissions)
- [GetPermissionsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getpermissionsasync)

Ardından, **GetPermissions** yöntemiyle döndürülen [BlobContainerPermissions](/dotnet/api/microsoft.azure.storage.blob.blobcontainerpermissions) nesnesi üzerindeki **PublicAccess** özelliğini ayarlayın.

Son olarak, kapsayıcının izinlerini güncelleştirmek için aşağıdaki yöntemlerden birini arayın:

- [Setpermissions](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setpermissions)
- [SetPermissionsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setpermissionsasync)

Aşağıdaki örnek, kapsayıcının tam genel okuma erişimi için izinlerini ayarlar. Yalnızca blobs için herkese açık okuma erişimi için izinler ayarlamak için **PublicAccess** özelliğini **BlobContainerPublicAccessType.Blob**olarak ayarlayın. Anonim kullanıcılar için tüm izinleri kaldırmak için özelliği **BlobContainerPublicAccessType.Off**olarak ayarlayın.

```csharp
private static async Task SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    await container.SetPermissionsAsync(permissions);

    Console.WriteLine("Container {0} - permissions set to {1}", container.Name, permissions.PublicAccess);
}
```

## <a name="access-containers-and-blobs-anonymously"></a>Kapsayıcılara ve lekelere anonim olarak erişin

Kapsayıcılara ve blob'lara anonim olarak erişen bir istemci, kimlik bilgileri gerektirmeyen oluşturucular kullanabilir. Aşağıdaki örnekler, kapsayıcılara ve lekelere anonim olarak başvurmanın birkaç farklı yolunu göstermektedir.

### <a name="create-an-anonymous-client-object"></a>Anonim bir istemci nesnesi oluşturma

Hesap için Blob depolama bitiş noktasını sağlayarak anonim erişim için yeni bir hizmet istemcisi nesnesi oluşturabilirsiniz. Ancak, bu hesaptaki ve anonim erişim için kullanılabilen bir kapsayıcının adını da bilmeniz gerekir.

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob storage endpoint for your account.
    CloudBlobClient blobClient = new CloudBlobClient(
        new Uri(@"https://storagesamples.blob.core.windows.net"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. 
    // Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a>Bir kapsayıcıya anonim olarak başvurun

Url'niz anonim olarak kullanılabilen bir kapsayıcının URL'sine sahipseniz, doğrudan kapsayıcıya başvurmak için kullanabilirsiniz.

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(
        new Uri(@"https://storagesamples.blob.core.windows.net/sample-container"));

    // List blobs in the container.
    // Note this is only possible when the container supports full public read access.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a>Bir blob'u anonim olarak referans

Anonim erişim için kullanılabilen bir blob'un URL'sine sahipseniz, bu URL'yi kullanarak doğrudan blob'a başvuruda bulunabilirsiniz:

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(
        new Uri(@"https://storagesamples.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", FileMode.Create);
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Depolama'ya erişim yetkisi](../common/storage-auth.md)
- [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni verme](../common/storage-sas-overview.md)
- [Blob Hizmeti REST API'si](/rest/api/storageservices/blob-service-rest-api)
