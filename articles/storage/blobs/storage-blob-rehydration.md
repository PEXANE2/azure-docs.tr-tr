---
title: Arşiv katmanından rehydrate blob verileri
description: Verilere erişebilmeniz için lekelerinizi arşiv depolamasından yeniden sulendirin.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 11/14/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: 0a7012d9daa808933a51ac05862a8a9aa4cfcf77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77614792"
---
# <a name="rehydrate-blob-data-from-the-archive-tier"></a>Arşiv katmanından rehydrate blob verileri

Bir blob arşiv erişim katmanında yken çevrimdışı olarak kabul edilir ve okunamıyor veya değiştirilemez. Blob meta verileri çevrimiçi ve kullanılabilir durumda kalır ve bu da blob'u ve özelliklerini listelemenize olanak sağlar. Blob verilerini okumak ve değiştirmek yalnızca sıcak veya havalı gibi çevrimiçi katmanlarla kullanılabilir. Arşiv erişim katmanında depolanan verileri almak ve bunlara erişmek için iki seçenek vardır.

1. [Arşivlenmiş bir lekeyi çevrimiçi bir katmana rehydrate](#rehydrate-an-archived-blob-to-an-online-tier) - [Set Blob Tier](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) işlemini kullanarak katmanını değiştirerek arşiv lekesini sıcak veya soğumaya rehydrate.
2. [Arşivlenmiş bir lekeyi çevrimiçi katmana kopyalayın](#copy-an-archived-blob-to-an-online-tier) - [Kopya Blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob) işlemini kullanarak arşiv lekesinin yeni bir kopyasını oluşturun. Farklı bir blob adı ve sıcak veya serin bir hedef katmanı belirtin.

 Katmanlar hakkında daha fazla bilgi için [Azure Blob depolama alanına bakın: sıcak, serin ve arşiv erişim katmanları.](storage-blob-storage-tiers.md)

## <a name="rehydrate-an-archived-blob-to-an-online-tier"></a>Arşivlenmiş bir lekeyi çevrimiçi katmana rehydrate

[!INCLUDE [storage-blob-rehydration](../../../includes/storage-blob-rehydrate-include.md)]

## <a name="copy-an-archived-blob-to-an-online-tier"></a>Arşivlenmiş blobu çevrimiçi katmana kopyalama

Arşiv lekenizi yeniden sulamak istemiyorsanız, [Bir Kopya Blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob) işlemi yapmayı seçebilirsiniz. Üzerinde çalışabileceğiniz çevrimiçi sıcak veya serin katmanda yeni bir leke oluşturulurken orijinal lekeniz arşivde değiştirilmemiş olarak kalır. Copy Blob işleminde, blob kopyanızın oluşturulmasını istediğiniz önceliği belirtmek için isteğe bağlı *x-ms-rehydrate öncelikli* özelliğistandart veya Yüksek (önizleme) olarak da ayarlayabilirsiniz.

Arşiv lekeleri yalnızca aynı depolama hesabı içindeki çevrimiçi hedef katmanlara kopyalanabilir. Bir arşiv blob'unu başka bir arşiv blob'una kopyalamak desteklenmez.

Arşivden bir leke kopyalama seçilen rehydrate önceliğine bağlı olarak tamamlanması saatler sürebilir. Arka planda, **Copy Blob** işlemi, seçili hedef katmanda yeni bir çevrimiçi blob oluşturmak için arşiv kaynağı nızı okur. Yeni blob, lekeleri listelediğinizde görünür olabilir, ancak kaynak arşiv blob'dan okunan ve veriler yeni çevrimiçi hedef blob'a yazılana kadar veriler kullanılamaz. Yeni blob bağımsız bir kopya olarak ve herhangi bir değişiklik veya silme kaynak arşiv blob etkilemez.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Arşivdeki lekeleri sıcak veya serin katmanlara rehydrating okuma işlemleri ve veri alma olarak ücretlendirilir. Yüksek öncelikli (önizleme) kullanmak, standart önceliğe kıyasla daha yüksek işlem ve veri alma maliyetlerine sahiptir. Yüksek öncelikli rehidrasyon faturanızda ayrı bir satır öğesi olarak ortaya çıkar. Birkaç gigabaytlık arşiv blob'u iade etmek için yüksek öncelikli bir istek 5 saatten fazla sürerse, yüksek öncelikli alma oranı ücretlendirilmez. Ancak, rehidrasyon diğer isteklere göre öncelik olarak standart alma oranları hala geçerlidir.

Arşivdeki lekelerin arşivden sıcak veya serin katmanlara kopyalanması okuma işlemleri ve veri alma olarak ücretlendirilir. Yeni blob kopyasının oluşturulması için bir yazma işlemi ücretlendirilir. Kaynak blob arşiv katmanında değiştirilmemiş kalır, çünkü bir çevrimiçi blob kopyaladığınızda erken silme ücretleri geçerli değildir. Seçilirse yüksek öncelikli alma ücretleri uygulanır.

Arşiv katmanındaki lekeler en az 180 gün saklanmalıdır. Arşivlenmiş lekelerin 180 günden önce silinmesi veya yeniden sulanması erken silme ücretlerine neden olur.

> [!NOTE]
> Blok blob'ları ve veri yeniden hidrasyonu için fiyatlandırma hakkında daha fazla bilgi için Azure [Depolama Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/storage/blobs/)bakın. Giden veri aktarım ücretleri hakkında daha fazla bilgi için veri [aktarımları Fiyatlandırma Ayrıntıları'na](https://azure.microsoft.com/pricing/details/data-transfers/)bakın.

## <a name="quickstart-scenarios"></a>Hızlı Başlangıç senaryoları

### <a name="rehydrate-an-archive-blob-to-an-online-tier"></a>Bir arşiv lekesini çevrimiçi katmana rehydrate
# <a name="portal"></a>[Portal](#tab/azure-portal)
1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Azure portalında **Tüm Kaynakları**arayın ve seçin.

1. Depolama hesabınızı seçin.

1. Kabınızı seçin ve sonra blob seçin.

1. **Blob özelliklerinde,** **katmanı değiştir'i**seçin.

1. **Sıcak** veya **Cool** erişim katmanını seçin. 

1. **Standart** veya **Yüksek**Bir Rehydrate Önceliği seçin.

1. Alttaki **Kaydet'i** seçin.

![Depolama hesabı katmanını değiştirme](media/storage-tiers/blob-access-tier.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Aşağıdaki PowerShell komut dosyası, bir arşiv blob'unun blob katmanını değiştirmek için kullanılabilir. Değişken, `$rgName` kaynak grup adınız ile başharfe alınmalıdır. Değişken, `$accountName` depolama hesabı adınız ile başharfe bürünmelidir. Değişken, `$containerName` kapsayıcı adınız ile başharfe alınmalıdır. Değişken, `$blobName` blob adınız ile başharfe alınmalıdır. 
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

### <a name="copy-an-archive-blob-to-a-new-blob-with-an-online-tier"></a>Çevrimiçi katmanla yeni bir blob'a arşiv blob'u kopyalama
Aşağıdaki PowerShell komut dosyası, aynı depolama hesabı içindeki yeni bir blob'a arşiv blob'u kopyalamak için kullanılabilir. Değişken, `$rgName` kaynak grup adınız ile başharfe alınmalıdır. Değişken, `$accountName` depolama hesabı adınız ile başharfe bürünmelidir. Ve `$srcContainerName` `$destContainerName` değişkenler kapsayıcı adlarınızla başharfe bilmelidir. Ve `$srcBlobName` `$destBlobName` değişkenler blob adlarınızla başharfe alınmalıdır. 
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

* [Blob Depolama Katmanları hakkında bilgi edinin](storage-blob-storage-tiers.md)
* [Blob depolama ve GPv2 hesaplarında bölgeye göre sık erişimli, seyrek erişimli ve arşiv fiyatlarını denetleme](https://azure.microsoft.com/pricing/details/storage/)
* [Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)
* [Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)
