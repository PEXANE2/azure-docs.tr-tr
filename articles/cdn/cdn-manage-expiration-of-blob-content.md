---
title: Azure Blob depolama alanının sona ermesini yönetme
titleSuffix: Azure Content Delivery Network
description: Azure CDN önbelleğe alma da damlalar için canlı zamanı denetleme seçenekleri hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 02/1/2018
ms.author: mazha
ms.openlocfilehash: f28282a802e4b38fadc05c7090fa2a2af154de54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74083151"
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-cdn"></a>Azure Blob depolama alanının sona erme sürelerini Azure CDN'de yönetme
> [!div class="op_single_selector"]
> * [Azure web içeriği](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob depolama](cdn-manage-expiration-of-blob-content.md)
> 
> 

Azure Depolama'daki [Blob depolama hizmeti,](../storage/common/storage-introduction.md#blob-storage) Azure İçerik Dağıtım Ağı (CDN) ile tümleşik birkaç Azure tabanlı kaynaktan biridir. Herkese açık olarak erişilebilen tüm blob içeriği, kullanıma açık (TTL) süresi dolana kadar Azure CDN'de önbelleğe alınabilir. TTL, başlangıç sunucusundan HTTP yanıtındaki `Cache-Control` üstbilgi tarafından belirlenir. Bu makalede, `Cache-Control` üstbilgiazure Depolama bir blob üzerinde ayarlayabilirsiniz çeşitli yolları açıklanır.

CDN önbelleğe alma kuralları ayarlayarak Azure portalından önbellek ayarlarını da denetleyebilirsiniz. Önbelleğe alma kuralı oluşturur ve önbelleğe alma davranışını **Geçersiz Kılma** veya **Atlama önbelleğini**ayarlarsanız, bu makalede tartışılan kaynak sağlanan önbelleğe alma ayarları yoksayılır. Genel önbelleğe alma kavramları hakkında bilgi [için](cdn-how-caching-works.md)bkz.

> [!TIP]
> Bir blob üzerinde Hiçbir TTL ayarlamak için seçebilirsiniz. Bu durumda, Azure portalında önbelleğe alma kuralları ayarlamadığınız sürece Azure CDN varsayılan yedi günlük ttl'yi otomatik olarak uygular. Bu varsayılan TTL yalnızca genel web teslim optimizasyonları için geçerlidir. Büyük dosya optimizasyonları için varsayılan TTL bir gündür ve ortam akışı optimizasyonları için varsayılan TTL bir yıldır.
> 
> Azure CDN'nin blob'lara ve diğer dosyalara erişimi hızlandırmak için nasıl çalıştığı hakkında daha fazla bilgi için Azure [İçerik Teslim Ağı'na Genel Bakış bölümüne](cdn-overview.md)bakın.
> 
> Azure Blob depolama alanı hakkında daha fazla bilgi için [blob depolamaya giriş 'e](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)bakın.
 

## <a name="setting-cache-control-headers-by-using-cdn-caching-rules"></a>CDN önbelleğe alma kurallarını kullanarak Önbellek Denetimi üstleri ayarlama
Blob üstbilgisini `Cache-Control` ayarlamak için tercih edilen yöntem, Azure portalında önbelleğe alma kurallarını kullanmaktır. CDN önbelleğe alma kuralları hakkında daha fazla bilgi için [önbelleğe alma kurallarıyla Azure CDN önbelleğe alma davranışını denetle'](cdn-caching-rules.md)ye bakın.

> [!NOTE] 
> Önbelleğe alma kuralları yalnızca **Akamai profillerinden** Verizon ve Azure **CDN Standard'dan Azure CDN Standardı** için kullanılabilir. **Verizon profillerinden Azure CDN Premium** için, benzer işlevler için **Yönet** portalındaki [Azure CDN kuralları altyapısını](cdn-rules-engine.md) kullanmanız gerekir.

**CDN önbelleğe alma kuralları sayfasına gitmek için:**

1. Azure portalında bir CDN profili seçin ve ardından blob için bitiş noktasını seçin.

2. Ayarların altındaki sol bölmede **Önbelleğe alma kuralları**’nı seçin.

   ![CDN önbelleğe alma kuralları düğmesi](./media/cdn-manage-expiration-of-blob-content/cdn-caching-rules-btn.png)

   **Önbelleğe alma kuralları** sayfası görüntülenir.

   ![CDN önbelleğe alma sayfası](./media/cdn-manage-expiration-of-blob-content/cdn-caching-page.png)


**Bir Blob depolama hizmetinin Önbellek denetimi üstbilgisini genel önbelleğe alma kurallarını kullanarak ayarlamak için:**

1. **Genel önbelleğe alma kuralları**altında, **sorgu dizeleri yoksaymak** için **Sorgu dize önbelleğe alma davranışı** ayarlayın ve **Önbelleğe Alma davranışını** **geçersiz kılmak**için ayarlayın.
      
2. **Önbellek son kullanma süresi için** **Saniye** kutusuna 3600 veya **Saat** kutusuna 1 girin. 

   ![CDN genel önbelleğe alma kuralları örneği](./media/cdn-manage-expiration-of-blob-content/cdn-global-caching-rules-example.png)

   Bu genel önbelleğe alma kuralı bir saatlik önbellek süresi ayarlar ve bitiş noktasına kadar tüm istekleri etkiler. Bitiş noktası `Cache-Control` tarafından `Expires` belirtilen başlangıç sunucusu tarafından gönderilen herhangi bir veya HTTP üstbilgisini geçersiz kılar.   

3. **Kaydet'i**seçin.
 
**Özel önbelleğe alma kurallarını kullanarak blob dosyasının Önbellek Denetimi üstbilgisini ayarlamak için:**

1. **Özel önbelleğe alma kuralları altında,** iki maç koşulu oluşturun:

     A. İlk eşleşme koşulu **için, Match koşulunu** **Yol'a** ayarlayın ve Maç `/blobcontainer1/*` **değeri**için girin. **Önbelleğe Alma davranışını** **Geçersiz kılmaya** ayarlayın ve **Saat** kutusuna 4 girin.

    B. İkinci eşleşme koşulu için **Maç koşulunu** `/blobcontainer1/blob1.txt` **Yol'a** ayarlayın ve Match **değeri**için girin. **Önbelleğe Alma davranışını** **Geçersiz kılmaya** ayarlayın ve **Saat** kutusuna 2 girin.

    ![CDN özel önbelleğe alma kuralları örneği](./media/cdn-manage-expiration-of-blob-content/cdn-custom-caching-rules-example.png)

    İlk özel önbelleğe alma kuralı, bitiş noktanızda belirtilen başlangıç `/blobcontainer1` sunucusundaki klasördeki herhangi bir blob dosyası için dört saatlik bir önbellek süresi ayarlar. İkinci kural yalnızca `blob1.txt` blob dosyası için ilk kuralı geçersiz kılar ve bunun için iki saatlik bir önbellek süresi ayarlar.

2. **Kaydet'i**seçin.


## <a name="setting-cache-control-headers-by-using-azure-powershell"></a>Azure PowerShell'i kullanarak Önbellek Denetimi üsterlerini ayarlama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure PowerShell,](/powershell/azure/overview) Azure hizmetlerinizi yönetmenin en hızlı ve en güçlü yollarından biridir. Blob `Get-AzStorageBlob` bir referans almak için cmdlet kullanın, `.ICloudBlob.Properties.CacheControl` sonra özelliği ayarlayın. 

Örnek:

```powershell
# Create a storage context
$context = New-AzStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> [CdN profillerinizi ve uç noktalarınızı yönetmek](cdn-manage-powershell.md)için PowerShell'i de kullanabilirsiniz.
> 
>

## <a name="setting-cache-control-headers-by-using-net"></a>.NET kullanarak Önbellek Denetimi üstleri ayarlama
.NET kodunu kullanarak `Cache-Control` bir blob üstbilgisini belirtmek için [CloudBlob.Properties.CacheControl](/dotnet/api/microsoft.azure.storage.blob.blobproperties.cachecontrol) özelliğini ayarlamak [için .NET için Azure Depolama İstemci Kitaplığını](../storage/blobs/storage-dotnet-how-to-use-blobs.md) kullanın.

Örnek:

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference to the container
        CloudBlobContainer <container name> = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob <blob name> = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> [.NET için Azure Blob Depolama Örnekleri'nde](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)daha fazla .NET kod örneği bulunmaktadır.
> 

## <a name="setting-cache-control-headers-by-using-other-methods"></a>Önbellek denetimi üsterlerini diğer yöntemleri kullanarak ayarlama

### <a name="azure-storage-explorer"></a>Azure Storage Gezgini
[Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) *ile, Önbellek* özelliği gibi özellikler de dahil olmak üzere blob depolama kaynaklarınızı görüntüleyebilir ve edebilirsiniz. 

Azure Depolama Gezgini ile bir blob'un *Önbellek* özelliğini güncelleştirmek için:
   1. Bir blob seçin ve ardından bağlam menüsünden **Özellikler'i** seçin. 
   2. *CacheControl* özelliğine gidin.
   3. Bir değer girin, ardından **Kaydet'i**seçin.


![Azure Depolama Gezgini özellikleri](./media/cdn-manage-expiration-of-blob-content/cdn-storage-explorer-properties.png)

### <a name="azure-command-line-interface"></a>Azure Komut Satırı Arabirimi
Azure [Komut Satırı Arabirimi](https://docs.microsoft.com/cli/azure) (CLI) ile Komut satırından Azure blob kaynaklarını yönetebilirsiniz. Azure CLI ile bir blob yüklediğinizde önbellek denetimi üstbilgisini ayarlamak için, anahtarı `-p` kullanarak *önbellek Denetimi* özelliğini ayarlayın. Aşağıdaki örnekte TTL'nin nasıl bir saate (3600 saniye) ayarlanınması gösterilmektedir:
  
```azurecli
azure storage blob upload -c <connectionstring> -p cacheControl="max-age=3600" .\<blob name> <container name> <blob name>
```

### <a name="azure-storage-services-rest-api"></a>Azure depolama hizmetleri REST API
Bir istekte aşağıdaki işlemleri kullanarak *x-ms-blob-cache denetimi* özelliğini açıkça ayarlamak için [Azure depolama hizmetleri REST API'yi](/rest/api/storageservices/) kullanabilirsiniz:
  
   - [İkili Büyük Nesne Koyma](/rest/api/storageservices/Put-Blob)
   - [Blok Listesi Koy](/rest/api/storageservices/Put-Block-List)
   - [Blob Özelliklerini Ayarla](/rest/api/storageservices/Set-Blob-Properties)

## <a name="testing-the-cache-control-header"></a>Önbellek Denetimi üstbilgisini sına
Lekelerinizin TTL ayarlarını kolayca doğrulayabilirsiniz. Tarayıcınızın geliştirici [araçlarıyla,](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)blob'unuzun `Cache-Control` yanıt üstbilgisini içerdiğini test edin. Yanıt üstbilgilerini incelemek için [Wget,](https://www.gnu.org/software/wget/) [Postman](https://www.getpostman.com/)veya [Fiddler](https://www.telerik.com/fiddler) gibi bir araç da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure CDN'de Bulut Hizmeti içeriğinin sona ermesini nasıl yönetebilirsiniz öğrenin](cdn-manage-expiration-of-cloud-service-content.md)
* [Önbelleğe alma kavramları hakkında bilgi edinin](cdn-how-caching-works.md)

