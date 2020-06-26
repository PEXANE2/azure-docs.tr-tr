---
title: Uygulama verilerine güvenli erişim
titleSuffix: Azure Storage
description: Bulutta uygulamanızın veri güvenliğini sağlamak için SAS belirteçlerini, şifrelemeyi ve HTTPS’yi kullanın.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.topic: tutorial
ms.date: 06/10/2020
ms.author: tamram
ms.reviewer: ozgun
ms.custom: mvc
ms.openlocfilehash: b5ca24a68b271c08ea7cd4196d5b8659eb0262d2
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85367385"
---
# <a name="secure-access-to-application-data"></a>Uygulama verilerine güvenli erişim

Bu öğretici, bir serinin üçüncü bölümüdür. Depolama hesabına erişim güvenliğinin nasıl sağlanacağını öğrenirsiniz. 

Serinin üçüncü bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Küçük resimlere erişmek için SAS belirteçlerini kullanma
> * Sunucu tarafı şifrelemesini açma
> * Yalnızca HTTPS taşımasını etkinleştirme

[Azure blob depolama](../common/storage-introduction.md#blob-storage), uygulamalara ilişkin dosyaları depolamak için sağlam bir hizmet sağlar. Bu öğretici, bir web uygulamasından depolama hesabınıza erişim güvenliğinin nasıl sağlanacağını göstermek için [önceki konuyu][previous-tutorial] genişletir. İşiniz bittiğinde görüntüler şifrelenir ve web uygulaması, küçük resimlere erişmek için güvenli SAS belirteçlerini kullanır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için önceki şu Depolama öğreticisini tamamlamış olmanız gerekir: [Karşıya yüklenen görüntüleri yeniden boyutlandırmayı Event Grid kullanarak otomatikleştirme][previous-tutorial].

## <a name="set-container-public-access"></a>Kapsayıcı genel erişimini ayarlama

Öğretici serisinin bu kısmında, küçük resimlere erişmek için SAS belirteçleri kullanılır. Bu adımda, *thumbnails* kapsayıcısının genel erişimini `off` olarak ayarlarsınız.

```bash
blobStorageAccount="<blob_storage_account>"

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
    --account-name $blobStorageAccount --query [0].value --output tsv) 

az storage container set-permission \
    --account-name $blobStorageAccount \
    --account-key $blobStorageAccountKey \
    --name thumbnails \
    --public-access off
```

```powershell
$blobStorageAccount="<blob_storage_account>"

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup `
    --account-name $blobStorageAccount --query [0].value --output tsv) 

az storage container set-permission `
    --account-name $blobStorageAccount `
    --account-key $blobStorageAccountKey `
    --name thumbnails `
    --public-access off
```

## <a name="configure-sas-tokens-for-thumbnails"></a>Küçük resimler için SAS belirteçlerini yapılandırma

Bu öğretici serisinin birinci kısmında web uygulaması, bir genel kapsayıcıdaki görüntüleri gösteriyordu. Serinin bu bölümünde, küçük resim görüntülerini almak için paylaşılan erişim imzaları (SAS) belirteçlerini kullanırsınız. SAS belirteçleri; IP, protokol, zaman aralığı veya izin verilen haklar temelinde bir kapsayıcıya ya da bloba kısıtlı erişim sağlamanıza olanak verir. SAS hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../common/storage-sas-overview.md).

Bu örnekte kaynak kod deposu, güncelleştirilmiş bir kod örneği içeren `sasTokens` dalını kullanır. [az webapp deployment source delete](/cli/azure/webapp/deployment/source) komutuyla mevcut GitHub dağıtımını silin. Sonra [az webapp deployment source config](/cli/azure/webapp/deployment/source) komutuyla web uygulamasına GitHub dağıtımını yapılandırın.

Aşağıdaki komutta `<web-app>`, web uygulamanızın adıdır.

```bash
az webapp deployment source delete --name <web-app> --resource-group myResourceGroup

az webapp deployment source config --name <web_app> \
    --resource-group myResourceGroup --branch sasTokens --manual-integration \
    --repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
```

```powershell
az webapp deployment source delete --name <web-app> --resource-group myResourceGroup

az webapp deployment source config --name <web_app> `
    --resource-group myResourceGroup --branch sasTokens --manual-integration `
    --repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
```

Deponun `sasTokens` dalı, `StorageHelper.cs` dosyasını güncelleştirir. `GetThumbNailUrls` görevini, aşağıdaki kod örneğiyle değiştirir. Güncelleştirilmiş görev, SAS belirtecinin başlangıç zamanını, süre sonu zamanını ve izinlerini belirtmek için bir [Blobsasbuilder](/dotnet/api/azure.storage.sas.blobsasbuilder) kullanarak küçük resim URL 'lerini alır. Dağıtıldıktan sonra web uygulaması artık bir SAS belirteci kullanarak URL ile küçük resimleri alır. Aşağıdaki örnekte güncelleştirilmiş görev gösterilmektedir:

```csharp
public static async Task<List<string>> GetThumbNailUrls(AzureStorageConfig _storageConfig)
{
    List<string> thumbnailUrls = new List<string>();

    // Create a URI to the storage account
    Uri accountUri = new Uri("https://" + _storageConfig.AccountName + ".blob.core.windows.net/");

    // Create BlobServiceClient from the account URI
    BlobServiceClient blobServiceClient = new BlobServiceClient(accountUri);

    // Get reference to the container
    BlobContainerClient container = blobServiceClient.GetBlobContainerClient(_storageConfig.ThumbnailContainer);

    if (container.Exists())
    {
        // Set the expiration time and permissions for the container.
        // In this case, the start time is specified as a few 
        // minutes in the past, to mitigate clock skew.
        // The shared access signature will be valid immediately.
        BlobSasBuilder sas = new BlobSasBuilder
        {
            Resource = "c",
            BlobContainerName = _storageConfig.ThumbnailContainer,
            StartsOn = DateTimeOffset.UtcNow.AddMinutes(-5),
            ExpiresOn = DateTimeOffset.UtcNow.AddHours(1)
        };

        sas.SetPermissions(BlobContainerSasPermissions.All);

        // Create StorageSharedKeyCredentials object by reading
        // the values from the configuration (appsettings.json)
        StorageSharedKeyCredential storageCredential =
            new StorageSharedKeyCredential(_storageConfig.AccountName, _storageConfig.AccountKey);

        // Create a SAS URI to the storage account
        UriBuilder sasUri = new UriBuilder(accountUri);
        sasUri.Query = sas.ToSasQueryParameters(storageCredential).ToString();

        foreach (BlobItem blob in container.GetBlobs())
        {
            // Create the URI using the SAS query token.
            string sasBlobUri = container.Uri + "/" +
                                blob.Name + sasUri.Query;

            //Return the URI string for the container, including the SAS token.
            thumbnailUrls.Add(sasBlobUri);
        }
    }
    return await Task.FromResult(thumbnailUrls);
}
```

Yukarıdaki görevde, aşağıdaki sınıflar, özellikler ve yöntemler kullanılır:

| Sınıf | Özellikler | Yöntemler |
|-------|------------|---------|
|[StorageSharedKeyCredential](/dotnet/api/azure.storage.storagesharedkeycredential) |  |  |
|[BlobServiceClient](/dotnet/api/azure.storage.blobs.blobserviceclient) |  |[GetBlobContainerClient](/dotnet/api/azure.storage.blobs.blobserviceclient.getblobcontainerclient) |
|[BlobContainerClient](/dotnet/api/azure.storage.blobs.blobcontainerclient) | [Kullanılmamışsa](/dotnet/api/azure.storage.blobs.blobcontainerclient.uri) |[Var](/dotnet/api/azure.storage.blobs.blobcontainerclient.exists) <br> [GetBlob 'Lar](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobs) |
|[BlobSasBuilder](/dotnet/api/azure.storage.sas.blobsasbuilder) |  | [SetPermissions](/dotnet/api/azure.storage.sas.blobsasbuilder.setpermissions) <br> [ToSasQueryParameters](/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) |
|[BlobItem](/dotnet/api/azure.storage.blobs.models.blobitem) | [Adı](/dotnet/api/azure.storage.blobs.models.blobitem.name) |  |
|[UriBuilder](/dotnet/api/system.uribuilder) | [Sorgu](/dotnet/api/system.uribuilder.query) |  |
|[Liste](/dotnet/api/system.collections.generic.list-1) | | [Ekle](/dotnet/api/system.collections.generic.list-1.add) |

## <a name="azure-storage-encryption"></a>Azure Depolama şifrelemesi

[Azure depolama şifrelemesi](../common/storage-service-encryption.md) , bekleyen verileri şifreleyerek ve şifreleme ve şifre çözme işlemlerini gerçekleştirerek verilerinizi korumanıza ve korumanıza yardımcı olur. Verilerin tamamı, mevcut en güçlü blok şifreleme özelliklerinden biri olan 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) ile şifrelenir.

Microsoft 'un şifreleme anahtarlarını yönetmesine veya Azure Key Vault ile müşteri tarafından yönetilen anahtarlarla kendi anahtarlarınızı getirebileceğini seçebilirsiniz. Daha fazla bilgi için bkz. [Azure depolama şifrelemesini yönetmek için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanma](../common/encryption-customer-managed-keys.md).

Azure depolama şifrelemesi, tüm performans katmanlarında (Standart ve Premium) verileri, tüm dağıtım modellerini (Azure Resource Manager ve klasik) ve tüm Azure depolama hizmetlerini (blob, kuyruk, tablo ve dosya) otomatik olarak şifreler.

## <a name="enable-https-only"></a>Yalnızca HTTPS'yi etkinleştirme

Depolama hesabına gelen ve depolama hesabından giden isteklerin güvenli olduğundan emin olmak için istekleri yalnızca HTTPS ile sınırlayabilirsiniz. [az storage account update](/cli/azure/storage/account) komutunu kullanarak depolama hesabı için gerekli protokolü güncelleştirin.

```azurecli-interactive
az storage account update --resource-group myresourcegroup --name <storage-account-name> --https-only true
```

`HTTP` protokolünü kullanarak `curl` kullanan bağlantıyı test edin.

```azurecli-interactive
curl http://<storage-account-name>.blob.core.windows.net/<container>/<blob-name> -I
```

Artık güvenli aktarım gerekli olduğundan şu iletiyi alırsınız:

```
HTTP/1.1 400 The account being accessed does not support http.
```

## <a name="next-steps"></a>Sonraki adımlar

Serinin üçüncü kısmında, aşağıda örnekleri verilen eylemlerle birlikte depolama hesabına erişim güvenliğinin nasıl sağlanacağını öğrendiniz:

> [!div class="checklist"]
> * Küçük resimlere erişmek için SAS belirteçlerini kullanma
> * Sunucu tarafı şifrelemesini açma
> * Yalnızca HTTPS taşımasını etkinleştirme

Bulut depolama uygulamasının nasıl izleneceğini ve sorunlarının nasıl giderileceğini öğrenmek için serinin dördüncü kısmına ilerleyin.

> [!div class="nextstepaction"]
> [Uygulama bulut uygulama depolamasını izleme ve sorunlarını giderme](storage-monitor-troubleshoot-storage-application.md)

[previous-tutorial]: ../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json
