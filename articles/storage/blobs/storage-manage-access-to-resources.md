---
title: Azure Blob depolamada kapsayıcılar ve Bloblar için genel okuma erişimini etkinleştirme | Microsoft Docs
description: Kapsayıcıları ve Blobları anonim erişim için nasıl kullanılabilir yapacağınızı ve programlı olarak nasıl erişebileceğini öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/30/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.openlocfilehash: 6293fc84969c4e246c05da4482f76142263db230
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68985553"
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Kapsayıcılara ve blob’lara anonim okuma erişimini yönetme

Azure Blob depolama alanındaki bir kapsayıcıya ve bloblarına anonim, genel okuma erişimi sağlayabilirsiniz. Bunu yaptığınızda, hesap anahtarınızı paylaşmadan ve paylaşılan erişim imzası (SAS) gerekmeden bu kaynaklara salt okuma erişimi verebilirsiniz.

Genel okuma erişimi, belirli Blobların anonim okuma erişimi için her zaman kullanılabilir olmasını istediğiniz senaryolar için idealdir. Daha ayrıntılı denetim için, paylaşılan erişim imzası oluşturabilirsiniz. Paylaşılan erişim imzaları, belirli bir süre boyunca farklı izinler kullanarak kısıtlı erişim sağlamanıza olanak tanır. Paylaşılan erişim imzaları oluşturma hakkında daha fazla bilgi için bkz. [Azure depolama 'da paylaşılan erişim imzaları (SAS) kullanma](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Kapsayıcılar ve bloblara anonim kullanıcılara izin verme

Varsayılan olarak, bir kapsayıcıya ve içindeki bloblara yalnızca uygun izinlere sahip olan bir kullanıcı tarafından erişilebilir. Anonim kullanıcılara bir kapsayıcıya ve bloblarına okuma erişimi sağlamak için kapsayıcı genel erişim düzeyini ayarlayabilirsiniz. Bir kapsayıcıya genel erişim verdiğinizde, anonim kullanıcılar, isteği yetkilendirmeden genel olarak erişilebilen bir kapsayıcı içindeki Blobları okuyabilir.

Aşağıdaki izinlerle bir kapsayıcı yapılandırabilirsiniz:

* **Genel okuma erişimi yok:** Kapsayıcıya ve bloblarına yalnızca depolama hesabı sahibi tarafından erişilebilir. Bu, tüm yeni kapsayıcılar için varsayılandır.
* **Yalnızca Bloblar için genel okuma erişimi:** Kapsayıcı içindeki Bloblar anonim istek tarafından okunabilir, ancak kapsayıcı verileri kullanılamıyor. Anonim istemciler kapsayıcı içindeki Blobları numaralandıramaz.
* **Kapsayıcı ve Blobları için genel okuma erişimi:** Tüm kapsayıcı ve blob verileri, anonim istek tarafından okunabilir. İstemciler kapsayıcı içindeki Blobları anonim istek ile numaralandırabilirler, ancak depolama hesabındaki kapsayıcıları numaralandıramaz.

Kapsayıcı izinlerini ayarlamak için aşağıdakileri kullanabilirsiniz:

* [Azure portal](https://portal.azure.com)
* [Azure PowerShell](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Azure CLI](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* Program aracılığıyla, depolama istemci kitaplıklarından birini veya REST API kullanarak

### <a name="set-container-public-access-level-in-the-azure-portal"></a>Azure portal kapsayıcı genel erişim düzeyini ayarlama

[Azure Portal](https://portal.azure.com), bir veya daha fazla kapsayıcı için genel erişim düzeyini güncelleştirebilirsiniz:

1. Azure portalda depolama hesabınıza gidin.
1. Menü dikey penceresinde **BLOB hizmeti** altında Bloblar 'ı seçin.
1. Ortak erişim düzeyini ayarlamak istediğiniz kapsayıcıları seçin.
1. Genel erişim ayarlarını göstermek için **erişim düzeyini Değiştir** düğmesini kullanın.
1. **Genel erişim düzeyi** açılan menüsünde istenen genel erişim düzeyini seçin ve değişikliği seçili kapsayıcılara uygulamak için Tamam düğmesine tıklayın.

Aşağıdaki ekran görüntüsünde, seçili kapsayıcılar için genel erişim düzeyinin nasıl değiştirileceği gösterilmektedir.

![Portalda genel erişim düzeyinin nasıl ayarlanacağını gösteren ekran görüntüsü](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

> [!NOTE]
> Tek bir blob için genel erişim düzeyini değiştiremezsiniz. Genel erişim düzeyi yalnızca kapsayıcı düzeyinde ayarlanır.

### <a name="set-container-public-access-level-with-net"></a>.NET ile kapsayıcı genel erişim düzeyini ayarlama

.NET için depolama Istemci kitaplığı C# ile bir kapsayıcı için izinleri ayarlamak için, önce **GetPermissions** metodunu çağırarak kapsayıcının mevcut izinlerini alın. Ardından, **GetPermissions** yöntemi tarafından döndürülen **blobcontainerpermissions** nesnesi için **publicAccess** özelliğini ayarlayın. Son olarak, güncelleştirilmiş izinlerle **SetPermissions** yöntemini çağırın.

Aşağıdaki örnek, kapsayıcının izinlerini tam genel okuma erişimi olarak ayarlar. Yalnızca Bloblar için genel okuma erişimi izinleri ayarlamak için, **publicAccess** özelliğini **Blobcontainerpublicaccesstype. blob**olarak ayarlayın. Anonim kullanıcılar için tüm izinleri kaldırmak için, özelliği **Blobcontainerpublicaccesstype. off**olarak ayarlayın.

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a>Kapsayıcılar ve bloblara anonim erişim

Kapsayıcılara ve bloblara erişen bir istemci, kimlik bilgileri gerektirmeyen oluşturucuları kullanabilir. Aşağıdaki örneklerde, kapsayıcılara ve bloblara anonim olarak başvurmak için birkaç farklı yol gösterilmektedir.

### <a name="create-an-anonymous-client-object"></a>Anonim istemci nesnesi oluşturma

Hesap için BLOB depolama uç noktası sağlayarak anonim erişim için yeni bir hizmet istemci nesnesi oluşturabilirsiniz. Ancak, bu hesapta anonim erişim için kullanılabilen bir kapsayıcının adını da bilmeniz gerekir.

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob storage endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a>Kapsayıcıya anonim olarak başvuru

Anonim olarak kullanılabilen bir kapsayıcının URL 'SI varsa, kapsayıcıyı doğrudan başvuruda bulunmak için kullanabilirsiniz.

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in the container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a>Bir Blobun anonim olarak başvur

Anonim erişim için kullanılabilen bir Blobun URL 'SI varsa, bu URL 'YI kullanarak blob 'a doğrudan başvurabilirsiniz:

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-to-anonymous-users"></a>Anonim kullanıcıların kullanabildiği Özellikler

Aşağıdaki tabloda, bir kapsayıcı genel erişim için yapılandırıldığında hangi işlemlerin adsız olarak çağrılabilecek gösterilmektedir.

| REST Işlemi | Kapsayıcıya genel okuma erişimi | Yalnızca bloblara genel okuma erişimi |
| --- | --- | --- |
| Kapsayıcıları Listele | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Kapsayıcı oluşturma | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Kapsayıcı özelliklerini al | İzin verilen anonim istekler | Yalnızca yetkili istekler |
| Kapsayıcı meta verilerini al | İzin verilen anonim istekler | Yalnızca yetkili istekler |
| Kapsayıcı meta verilerini ayarla | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Kapsayıcı ACL 'sini al | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Kapsayıcı ACL 'sini ayarla | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Kapsayıcıyı Sil | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Blobları Listele | İzin verilen anonim istekler | Yalnızca yetkili istekler |
| Blobu koy | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Blob al | İzin verilen anonim istekler | İzin verilen anonim istekler |
| Blob özelliklerini al | İzin verilen anonim istekler | İzin verilen anonim istekler |
| Blob özelliklerini ayarla | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Blob Meta Verilerini al | İzin verilen anonim istekler | İzin verilen anonim istekler |
| Blob meta verilerini ayarla | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Yerleştirme bloğu | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Engelleme listesini al (yalnızca taahhüt blokları) | İzin verilen anonim istekler | İzin verilen anonim istekler |
| Engelleme listesini al (yalnızca kaydedilmemiş bloklar veya tüm bloklar) | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Öbek listesini yerleştirme | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| BLOB silme | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Kopya blob'u | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Anlık görüntü blobu | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Kira blobu | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Yerleştirme sayfası | Yalnızca yetkili istekler | Yalnızca yetkili istekler |
| Sayfa aralıklarını al | İzin verilen anonim istekler | İzin verilen anonim istekler |
| Ekleme Blobu | Yalnızca yetkili istekler | Yalnızca yetkili istekler |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure depolama hizmetleri için yetkilendirme](https://docs.microsoft.com/rest/api/storageservices/authorization-for-the-azure-storage-services)
* [Paylaşılan erişim imzaları (SAS) kullanma](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)