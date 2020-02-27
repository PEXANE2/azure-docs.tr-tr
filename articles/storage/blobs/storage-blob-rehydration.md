---
title: Arşiv katmanından blob verilerini yeniden doldurma
description: Verilere erişebilmek için bloblarınızı arşiv depolamadan yeniden doldurma.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 11/14/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: 0a7012d9daa808933a51ac05862a8a9aa4cfcf77
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77614792"
---
# <a name="rehydrate-blob-data-from-the-archive-tier"></a>Arşiv katmanından blob verilerini yeniden doldurma

Blob, arşiv erişim katmanında olduğunda çevrimdışı olarak kabul edilir ve okunamaz veya değiştirilemez. Blob meta verileri çevrimiçi ve kullanılabilir durumda kalır ve bu da blobu ve özelliklerini listelemenize olanak sağlar. Blob verilerini okuma ve değiştirme yalnızca sık erişimli veya seyrek erişimli gibi çevrimiçi katmanlarla kullanılabilir. Arşiv erişim katmanında depolanan verileri almak ve erişmek için iki seçenek vardır.

1. [Arşivlenmiş bir blob 'u çevrimiçi katmana yeniden](#rehydrate-an-archived-blob-to-an-online-tier) doldurma- [BLOB katmanını ayarla](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) işlemini kullanarak katmanını değiştirerek sık veya seyrek erişimli bir arşiv blobu.
2. [Arşivlenmiş bir blobu çevrimiçi katmana kopyalama](#copy-an-archived-blob-to-an-online-tier) - [BLOB kopyalama](https://docs.microsoft.com/rest/api/storageservices/copy-blob) işlemini kullanarak arşiv blobunun yeni bir kopyasını oluşturun. Farklı bir blob adı ve sık erişimli veya seyrek erişimli hedef katmanı belirtin.

 Katmanlar hakkında daha fazla bilgi için bkz. [Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md).

## <a name="rehydrate-an-archived-blob-to-an-online-tier"></a>Arşivlenmiş bir Blobun çevrimiçi bir katmana yeniden doldurma

[!INCLUDE [storage-blob-rehydration](../../../includes/storage-blob-rehydrate-include.md)]

## <a name="copy-an-archived-blob-to-an-online-tier"></a>Arşivlenmiş bir blobu çevrimiçi katmana kopyalama

Arşiv blobundan yeniden doldurma yapmak istemiyorsanız, bir [blobu kopyalama](https://docs.microsoft.com/rest/api/storageservices/copy-blob) işlemi yapabilirsiniz. İş üzerinde çalışmanız için çevrimiçi sık erişimli veya seyrek erişimli katmanda yeni bir blob oluşturulduğunda, özgün Blobun arşiv 'de değiştirilmemiş olarak kalır. Blobu kopyalama işleminde, blob kopyanızın oluşturulmasını istediğiniz önceliği belirtmek için isteğe bağlı *x-MS-rehibulunan öncelik* özelliğini standart veya yüksek (Önizleme) olarak da ayarlayabilirsiniz.

Arşiv blob 'ları yalnızca aynı depolama hesabı içindeki çevrimiçi hedef katmanlara kopyalanabilir. Bir arşiv blobunun başka bir arşiv blobuna kopyalanması desteklenmez.

Bir Blobun arşivden kopyalanması, seçili olan yeniden doldurma önceliğine bağlı olarak tamamlanması saatler sürebilir. Arka planda **kopyalama blobu** işlemi, seçili hedef katmanda yeni bir çevrimiçi blob oluşturmak için Arşiv kaynak blobunu okur. Blob 'ları listelediğinizde yeni blob görünebilir, ancak kaynak arşiv blobundan okuma tamamlanana ve veriler yeni çevrimiçi hedef bloba yazıldıktan sonra veriler kullanılamaz. Yeni blob bağımsız bir kopya olarak ve üzerinde yapılan herhangi bir değişiklik veya silme, kaynak Arşivi blob 'unu etkilemez.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Blob 'ların arşiv dışına, sık veya seyrek erişimli katmanlara yeniden doldurma işlemi, okuma işlemleri ve veri alımı olarak ücretlendirilir. Yüksek öncelikli (Önizleme) kullanımı, standart önceliğe kıyasla daha yüksek işlem ve veri alımı maliyetlerine sahiptir. Yüksek öncelikli yeniden doldurma işlemi faturanızda ayrı bir satır öğesi olarak gösterilir. Birkaç gigabaytlık bir arşiv blobu döndürmek için yüksek öncelikli bir istek 5 saatten fazla sürerse, yüksek öncelikli alma ücreti üzerinden ücretlendirilmezsiniz. Ancak, yeniden doldurma diğer isteklere göre önceliklendirildiğinden standart alma ücretleri yine de geçerlidir.

Blobların arşivden sık veya seyrek erişimli katmanlara kopyalanması, okuma işlemleri ve veri alımı olarak ücretlendirilir. Yeni blob kopyasının oluşturulması için bir yazma işlemi ücretlendirilir. Kaynak blobu arşiv katmanında değiştirilmemiş olarak kaldığı için, erken silme ücretleri bir çevrimiçi bloba kopyalanırken uygulanmaz. Seçilirse, yüksek öncelikli alma ücretleri uygulanır.

Arşiv katmanındaki Bloblar en az 180 gün önce depolanmalıdır. Arşivlenmiş blob 'ları 180 günden önce silmek veya yeniden doldurma işlemi erken silme ücretleri oluşturacak.

> [!NOTE]
> Blok Blobları ve veri yeniden doldurma fiyatlandırması hakkında daha fazla bilgi için bkz. [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/). Giden veri aktarımı ücretleri hakkında daha fazla bilgi için bkz. [veri aktarımları fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="quickstart-scenarios"></a>Hızlı Başlangıç senaryoları

### <a name="rehydrate-an-archive-blob-to-an-online-tier"></a>Bir arşiv blobunu çevrimiçi katmana yeniden doldurma
# <a name="portal"></a>[Portal](#tab/azure-portal)
1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Azure portal, **tüm kaynakları**arayıp seçin.

1. Depolama hesabınızı seçin.

1. Kapsayıcınızı seçin ve ardından blobu seçin.

1. **BLOB özellikleri**' nde **Katmanı Değiştir**' i seçin.

1. **Sık** veya **seyrek erişimli erişim** katmanını seçin. 

1. **Standart** veya **yüksek**bir yeniden doldurma önceliği seçin.

1. Alt kısımdaki **Kaydet** ' i seçin.

![Depolama hesabı katmanını değiştirme](media/storage-tiers/blob-access-tier.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Bir arşiv Blobun blob katmanını değiştirmek için aşağıdaki PowerShell betiği kullanılabilir. `$rgName` değişkeni, kaynak grubu adınızla başlatılmalıdır. `$accountName` değişkeni, depolama hesabı adınızla başlatılmalıdır. `$containerName` değişkeni, kapsayıcı adınızla başlatılmalıdır. `$blobName` değişkeni, blob adınızla başlatılmalıdır. 
```powershell
#Initialize the following with your resource group, storage account, container, and blob names
$rgName = ""
$accountName = ""
$containerName = ""
$blobName = ""

#Select the storage account and get the context
$storageAccount =Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

#Select the blob from a container
$blobs = Get-AzStorageBlob -Container $containerName -Blob $blobName -Context $context

#Change the blob’s access tier to Hot using Standard priority rehydrate
$blob.ICloudBlob.SetStandardBlobTier("Hot", “Standard”)
```
---

### <a name="copy-an-archive-blob-to-a-new-blob-with-an-online-tier"></a>Bir arşiv blobunu çevrimiçi bir katman ile yeni bir bloba kopyalama
Aşağıdaki PowerShell betiği, aynı depolama hesabı içindeki yeni bir bloba arşiv blobu kopyalamak için kullanılabilir. `$rgName` değişkeni, kaynak grubu adınızla başlatılmalıdır. `$accountName` değişkeni, depolama hesabı adınızla başlatılmalıdır. `$srcContainerName` ve `$destContainerName` değişkenleri kapsayıcı adlarınızla başlatılmalıdır. `$srcBlobName` ve `$destBlobName` değişkenleri blob adlarınızla başlatılmalıdır. 
```powershell
#Initialize the following with your resource group, storage account, container, and blob names
$rgName = ""
$accountName = ""
$srcContainerName = ""
$destContainerName = ""
$srcBlobName = ""
$destBlobName = ""

#Select the storage account and get the context
$storageAccount =Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

#Copy source blob to a new destination blob with access tier hot using standard rehydrate priority
Start-AzStorageBlobCopy -SrcContainer $srcContainerName -SrcBlob $srcBlobName -DestContainer $destContainerName -DestBlob $destBlobName -StandardBlobTier Hot -RehydratePriority Standard -Context $ctx
```

## <a name="next-steps"></a>Sonraki Adımlar

* [BLOB depolama katmanları hakkında bilgi edinin](storage-blob-storage-tiers.md)
* [Blob depolama ve GPv2 hesaplarında bölgeye göre sık erişimli, seyrek erişimli ve arşiv fiyatlarını denetleme](https://azure.microsoft.com/pricing/details/storage/)
* [Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)
* [Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)
