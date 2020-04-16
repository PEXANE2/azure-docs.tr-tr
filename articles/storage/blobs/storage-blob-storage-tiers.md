---
title: Blobs için sıcak, serin ve arşiv erişim katmanları - Azure Depolama
description: Azure depolama hesapları için sıcak, havalı ve arşiv erişim katmanları.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/23/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: clausjor
ms.openlocfilehash: c803d489b70cda6910865f6096d21c2021c4ae3a
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393699"
---
# <a name="azure-blob-storage-hot-cool-and-archive-access-tiers"></a>Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları

Azure depolama, blob nesne verilerini en uygun maliyetli şekilde depolamanıza olanak tanıyan farklı erişim katmanları sunar. Kullanılabilir erişim katmanları şunlardır:

- **Sıcak** - Sık erişilen verileri depolamak için optimize edilmiştir.
- **Cool** - Seyrek erişilen ve en az 30 gün boyunca depolanan verileri depolamak için optimize edilmiştir.
- **Arşiv** - Esnek gecikme gereksinimleriyle (saat sırasına göre) en az 180 gün boyunca nadiren erişilen ve depolanan verileri depolamak için optimize edilmiştir.

Aşağıdaki hususlar farklı erişim katmanları için geçerlidir:

- Hesap düzeyinde yalnızca sıcak ve havalı erişim katmanları ayarlanabilir. Arşiv erişim katmanı hesap düzeyinde kullanılamaz.
- Sıcak, serin ve arşiv katmanları yükleme sırasında veya yükleme den sonra blob düzeyinde ayarlanabilir.
- Serin erişim katmanındaki veriler biraz daha düşük kullanılabilirliği tolere edebilir, ancak yine de yüksek dayanıklılık, alma gecikmesi ve sıcak verilere benzer veri elde etme özellikleri gerektirir. Serin veriler için, biraz daha düşük kullanılabilirlik hizmet düzeyi sözleşmesi (SLA) ve sıcak verilere kıyasla daha yüksek erişim maliyetleri, daha düşük depolama maliyetleri için kabul edilebilir dengelerdir.
- Arşiv depolama verileri çevrimdışı olarak saklar ve en düşük depolama maliyetlerinin değil, aynı zamanda en yüksek veri rehydrate ve erişim maliyetlerini sunar.

Bulutta depolanan veriler üstel bir hızda büyür. Artan depolama ihtiyaçlarınızın maliyetlerini yönetmek için, maliyetleri optimize etmek amacıyla erişim sıklığı ve planlanan elde tutma dönemi gibi özniteliklere bağlı olarak verilerinizi düzenlemek yararlıdır. Bulutta depolanan veriler, kullanım ömrü boyunca nasıl oluşturulduğuna, işlendiğine ve erişildiğine bağlı olarak farklı olabilir. Bazı veriler ve yaşam süresi boyunca aktif şekilde erişilebilir ve değiştirilebilir. Bazı verilere, veriler eskidikçe önemli ölçüde azalan erişimle, yaşam sürelerinin başlarında sık erişilebilir. Bazı veriler bulutta boşta kalır ve depolandıktan sonra nadiren erişilir.

Bu veri erişim senaryolarının her biri, belirli bir erişim deseni için en iyi duruma getirilmiş farklı bir erişim katmanından yararlanır. Azure Blob depolama, sıcak, havalı ve arşiv erişim katmanlarıyla, ayrı fiyatlandırma modelleri ne olursa o kadar farklı erişim katmanları na yönelik bu gereksinimi giderir.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="storage-accounts-that-support-tiering"></a>Katman ayarlamayı destekleyen depolama hesapları

Sıcak, serin ve arşiv arasındaki nesne depolama verileri yalnızca Blob depolama ve Genel Amaç v2 (GPv2) hesaplarında desteklenir. Genel Amaçlı v1 (GPv1) hesapları katmanlamayı desteklemez. Müşteriler, Azure portalı aracılığıyla mevcut GPv1 veya Blob depolama hesaplarını Kolayca GPv2 hesaplarına dönüştürebilir. GPv2, blobs, dosyalar ve kuyruklar için yeni fiyatlandırma ve özellikler sağlar. Bazı özellikler ve fiyat indirimleri sadece GPv2 hesaplarında sunulmaktadır. Fiyatlandırmayı kapsamlı bir şekilde gözden geçirdikten sonra GPv2 hesaplarını kullanarak değerlendirin. Bazı iş yükleri GPv2'de GPv1'den daha pahalı olabilir. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

Blob depolama ve GPv2 **hesapları, Access Tier** özniteliğini hesap düzeyinde ortaya çıkarır. Bu öznitelik, nesne düzeyinde açık ayarlanmış olmayan herhangi bir blob için varsayılan erişim katmanını belirtmenize olanak tanır. Nesne düzeyinde katman kümesi olan nesneler için hesap katmanı geçerli olmaz. Arşiv katmanı yalnızca nesne düzeyinde uygulanabilir. İstediğiniz zaman bu erişim katmanları arasında geçiş yapabilirsiniz.

## <a name="hot-access-tier"></a>Sık erişim katmanı

Sıcak erişim katmanı, serin ve arşiv katmanlarından daha yüksek depolama maliyetine, ancak en düşük erişim maliyetlerine sahiptir. Sıcak erişim katmanı için örnek kullanım senaryoları şunlardır:

- Etkin kullanımda olan veya sık sık erişilmesi beklenen (okunan ve yazılan) veriler.
- İşleme ve serin erişim katmanına nihai geçiş için aşamalı veriler.

## <a name="cool-access-tier"></a>Seyrek erişim katmanı

Serin erişim katmanı, sıcak depolamaya kıyasla daha düşük depolama maliyetlerine ve daha yüksek erişim maliyetlerine sahiptir. Bu katman, seyrek erişim katmanında en az 30 gün boyunca kalacak verilere yöneliktir. Serin erişim katmanı için örnek kullanım senaryoları şunlardır:

- Kısa süreli yedekleme ve olağanüstü durum kurtarma veri kümeleri.
- Artık sık görüntülenmeyen ancak erişildiğinde hemen kullanılabilir olması beklenen eski medya içeriği.
- Gelecekte işlenmek üzere daha fazla veri toplanırken uygun maliyetli olarak depolanması gereken büyük veri kümeleri. (*Örneğin*, bilimsel verilerin uzun süreli depolanması, üretim tesisinden alınan ham telemetri verileri)

## <a name="archive-access-tier"></a>Arşiv erişim katmanı

Arşiv erişim katmanı en düşük depolama maliyetine sahiptir. Ancak sıcak ve serin katmanlara göre daha yüksek veri alma maliyetlerine sahiptir. Arşiv katmanındaki verilerin alınması birkaç saat sürebilir. Veriler en az 180 gün arşiv katmanında kalmalı veya erken silme ücretine tabi olmalıdır.

Bir blob arşiv depolama da olsa, blob verileri çevrimdışı ve okunamıyor, üzerine yazılamaz veya değiştirilemez. Arşivdeki bir lekeyi okumak veya indirmek için, önce bir çevrimiçi katmana yeniden su vermelisiniz. Arşiv deposunda bir lekenin anlık görüntülerini alamazsınız. Ancak, blob meta verileri çevrimiçi ve kullanılabilir kalır, blob ve özelliklerini listelemek için izin. Arşivdeki lekeler için tek geçerli işlemler GetBlobProperties, GetBlobMetadata, ListBlobs, SetBlobTier, CopyBlob ve DeleteBlob'dır. Daha fazla bilgi edinmek için [arşiv katmanından rehydrate blob verileri](storage-blob-rehydration.md) bakın.

Arşiv erişim katmanı için örnek kullanım senaryoları şunlardır:

- Uzun vadeli yedekleme, ikincil yedekleme ve arşiv veri kümeleri
- Son kullanılabilir biçime işlendikten sonra bile özgün (ham) veriler korunmalıdır.
- Uzun süre depolanması gereken ve çok ender erişilecek uyumluluk ve arşiv verileri.

## <a name="account-level-tiering"></a>Hesap düzeyinde katmanlama

Her üç erişim katmanındaki blob'lar aynı hesap içinde bir arada bulunabilir. Açıkça atanmış bir katmanı olmayan herhangi bir blob, katmanı hesap erişim katmanı ayarından çıkar. Erişim katmanı hesaptan geliyorsa, **Access Tier Çıkarılan** blob özelliğinin "true" olarak ayarlanmış olduğunu ve **Access Tier** blob özelliğinin hesap katmanıyla eşleştiğini görürsünüz. Azure portalında, _erişim katmanı çıkarılan_ özellik blob erişim katmanıyla **Hot (çıkarılan)** veya **Cool (çıkarılan)** olarak görüntülenir.

Hesap erişim katmanını değiştirmek, hesapta açık bir katman kümesi olmayan depolanan tüm _erişim katmanı çıkarılan_ nesneler için geçerlidir. Hesap katmanını sıcaktan soğuğa doğru asalederseniz, yalnızca GPv2 hesaplarında belirli bir katman olmadan tüm blob'lar için yazma işlemleri (10.000 başına) için ücretlendirilirsiniz. Blob depolama hesaplarındaki bu değişiklik için herhangi bir ücret alınmaz. Blob depolama veya GPv2 hesaplarında serinden sıcaka geçiş yaptığınızda hem okuma işlemleri (10.000 kişi başına) hem de veri alma (GB başına) için ücretlendirilirsiniz.

## <a name="blob-level-tiering"></a>Blob düzeyinde katman ayarlama

Blob düzeyinde katmanlama, Blob Veya [Blok Listesi Koy](/rest/api/storageservices/put-block-list) işlemlerini kullanarak seçtiğiniz erişim katmanına veri yüklemenize ve Set [Blob](/rest/api/storageservices/put-blob) [Tier](/rest/api/storageservices/set-blob-tier) işlemi veya Yaşam [Döngüsü yönetimi](#blob-lifecycle-management) özelliğini kullanarak verilerinizin katmanını nesne düzeyinde değiştirmenize olanak tanır. Gerekli erişim katmanınıza veri yükleyebilir ve ardından kullanım desenleri değiştikçe, hesaplar arasında veri taşımak zorunda kalmadan, sıcak, serin veya arşiv katmanları arasındaki blob erişim katmanını kolayca değiştirebilirsiniz. Tüm katman değiştirme istekleri hemen gerçekleşir ve sıcak ve serin arasındaki katman değişiklikleri anında gerçekleşir. Ancak, arşivden bir damla rehydrating birkaç saat sürebilir.

Son blob katmanı değişikliğinin zamanı, **Erişim Katmanı Değişim Zamanı** blob özelliği aracılığıyla gösterilir. Sıcak veya serin katmanda bir blob üzerine yazarken, yeni oluşturulan blob, yeni blob erişim katmanı açıkça oluşturulma üzerinde ayarlanmıyorsa, üzerine yazılmış blob katmanını devralır. Bir blob arşiv katmanındaysa, üzerine yazılamaz, bu nedenle bu senaryoda aynı blob'u yüklemek izin verilmez. 

> [!NOTE]
> Arşiv depolama ve blob düzeyinde katman ayarlama, yalnızca blok bloblarını destekler. Ayrıca, anlık görüntüleri olan bir blok blob katmanını da şu anda değiştiremezsiniz.

### <a name="blob-lifecycle-management"></a>Blob yaşam döngüsü yönetimi

Blob Depolama yaşam döngüsü yönetimi, verilerinizi en iyi erişim katmanına dönüştürmek ve yaşam döngüsünün sonunda verileri nizi sona erdirmek için kullanabileceğiniz zengin, kural tabanlı bir ilke sunar. Daha fazla bilgi edinmek için [Azure Blob depolama yaşam döngüsünü yönet'e](storage-lifecycle-management-concepts.md) bakın.  

> [!NOTE]
> Blob Katmanı Kümesi veya Azure [Blob](/rest/api/storageservices/set-blob-tier) Depolama yaşam döngüsü yönetimi kullanılarak şu anda bir blok blob depolama hesabında (Premium performans) depolanan veriler şu anda sıcak, serin veya arşive katmanlandırılamaz.
> Verileri taşımak için, URL [API'den Blok Taşı'nı](/rest/api/storageservices/put-block-from-url) veya bu API'yi destekleyen AzCopy sürümünü kullanarak blob'ları blok blob depolama hesabından farklı bir hesaptaki sıcak erişim katmanına senkronize olarak kopyalamanız gerekir.
> *URL'den Blok La* Engelleme, sunucudaki verileri eşzamanlı olarak kopyalar, yani tüm veriler özgün sunucu konumundan hedef konuma taşındığında arama tamamlanır.

### <a name="blob-level-tiering-billing"></a>Blob düzeyinde katman ayarlama faturalandırması

Bir blob yüklendiğinde veya sıcak, serin veya arşiv katmanına taşındığında, katman değişikliğinden hemen sonra ilgili oranda ücretlendirilir.

Bir blob daha serin bir katmana (sıcak >, sıcak >arşivine veya cool->arşivine taşındığında), işlem, yazma işleminin (10.000 başına) ve hedef katmanın veri yazma (GB başına) ücretlerinin geçerli olduğu hedef katmana yazma işlemi olarak faturalandırılır.

Bir blob daha sıcak bir katmana taşındığında (arşiv >serin, arşiv >sıcak veya soğuk >sıcak), işlem, okuma işleminin (10.000 başına) ve kaynak katmanın veri alma (GB başına) ücretlerinin geçerli olduğu kaynak katmandan okundu olarak faturalandırılır. Seyrek erişim veya arşiv katmanı dışına taşınmış tüm bloblar için erken silme ücretleri de uygulanabilir. [Arşivden gelen verilerin yeniden sulandırılması](storage-blob-rehydration.md) zaman alır ve veriler çevrimiçi olarak geri yüklenene ve blob katmanı sıcak veya serin olarak değişene kadar arşiv fiyatları ndan ücretlendirilir. Aşağıdaki tabloda katman değişikliklerinin nasıl faturalandırıldığu özetlenmiştir:

| | **Yazma Ücretleri (İşlem + Erişim)** | **Ücretleri Okuma (İşlem + Erişim)**
| ---- | ----- | ----- |
| **SetBlobTier Yönü** | sıcak->serin,<br> sıcak >arşivi,<br> cool->arşivi | arşiv >serin,<br> arşiv->sıcak,<br> serin >sıcak

### <a name="cool-and-archive-early-deletion"></a>Seyrek erişimli ve arşiv erken silme

Serin katmana taşınan herhangi bir leke (yalnızca GPv2 hesapları) 30 günlük serin bir erken silme dönemine tabidir. Arşiv katmanına taşınan herhangi bir blob, 180 günlük bir arşiv erken silme dönemine tabidir. Bu ücret eşit olarak bölünür. Örneğin, bir blob arşive taşınır ve 45 gün sonra silinir veya sıcak katmana taşınırsa, bu blob'u arşivde depolamak için 135 (180 eksi 45) güne eşdeğer bir erken silme ücreti alırsınız.

Erişim katmanı nda değişiklik yoksa, **son değiştirilen**blob özelliğini kullanarak erken silme işlemini hesaplayabilirsiniz. Aksi takdirde, erişim katmanı en son değiştirildiğinde blob özelliğini görüntüleyerek serinlemek veya arşivlemek için kullanabilirsiniz: **erişim katmanı değiştirme zamanı.** Blob özellikleri hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/storageservices/get-blob-properties)

## <a name="comparing-block-blob-storage-options"></a>Blok blob depolama seçeneklerini karşılaştırma

Aşağıdaki tablo, birinci sınıf performans bloğu blob depolama ve sıcak, serin ve arşiv erişim katmanları bir karşılaştırma gösterir.

|                                           | **Üstün performans**   | **Sıcak katman** | **Serin katman**       | **Arşiv katmanı**  |
| ----------------------------------------- | ------------------------- | ------------ | ------------------- | ----------------- |
| **Kullanılabilirlik**                          | %99,9                     | %99,9        | %99                 | Çevrimdışı           |
| **Kullanılabilirlik** <br> **(RA-GRS okumaları)**  | Yok                       | %99,99       | %99,9               | Çevrimdışı           |
| **Kullanım ücretleri**                         | Daha yüksek depolama maliyetleri, daha düşük erişim ve işlem maliyeti | Daha yüksek depolama maliyetleri, daha düşük erişim ve işlem maliyetleri | Daha düşük depolama maliyetleri, daha yüksek erişim ve işlem maliyetleri | En düşük depolama maliyetleri, en yüksek erişim ve işlem maliyetleri |
| **En düşük nesne boyutu**                   | Yok                       | Yok          | Yok                 | Yok               |
| **En az depolama süresi**              | Yok                       | Yok          | 30 gün<sup>1</sup> | 180 gün
| **Gecikme süresi** <br> **(İlk bayta kalan süre)** | Tek basamaklı milisaniyeler | milisaniye | milisaniye        | saat<sup>2</sup> |

<sup>1</sup> GPv2 hesaplarındaki serin katmandaki nesnelerin en az 30 günlük saklama süresi vardır. Blob depolama hesapları, serin katman için minimum saklama süresine sahip değildir.

<sup>2</sup> Arşiv Depolama şu anda farklı alma gecikmeleri sunan 2 rehydrate öncelikleri, Yüksek ve Standart destekler. Daha fazla bilgi için [arşiv katmanından Rehydrate blob verilerine](storage-blob-rehydration.md)bakın.

> [!NOTE]
> Blob depolama hesapları, genel amaçlı v2 depolama hesaplarıyla aynı performans ve ölçeklenebilirlik hedeflerini destekler. Daha fazla bilgi için [Blob depolama için Ölçeklenebilirlik ve performans hedeflerine](scalability-targets.md)bakın.

## <a name="quickstart-scenarios"></a>Hızlı Başlangıç senaryoları

Bu bölümde, aşağıdaki senaryolar Azure portalı ve powershell kullanılarak gösterilmiştir:

- GPv2 veya Blob depolama hesabının varsayılan hesap erişim katmanını değiştirme.
- GPv2 veya Blob depolama hesabında blobun katmanını değiştirme.

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>GPv2 veya Blob depolama hesabının varsayılan hesap erişim katmanını değiştirme

# <a name="portal"></a>[Portal](#tab/azure-portal)
1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Azure portalında **Tüm Kaynakları**arayın ve seçin.

1. Depolama hesabınızı seçin.

1. **Ayarlar'da,** hesap yapılandırmasını görüntülemek ve değiştirmek için **Yapılandırma'yı** seçin.

1. İhtiyaçlarınız için doğru erişim katmanını seçin: **Erişim katmanını** **Cool** veya **Hot**olarak ayarlayın.

1. En üstte **Kaydet'i** tıklatın.

![Depolama hesabı katmanını değiştirme](media/storage-tiers/account-tier.png)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
Aşağıdaki PowerShell komut dosyası hesap katmanını değiştirmek için kullanılabilir. Değişken, `$rgName` kaynak grup adınız ile başharfe alınmalıdır. Değişken, `$accountName` depolama hesabı adınız ile başharfe bürünmelidir. 
```powershell
#Initialize the following with your resource group and storage account names
$rgName = ""
$accountName = ""

#Change the storage account tier to hot
Set-AzStorageAccount -ResourceGroupName $rgName -Name $accountName -AccessTier Hot
```
---

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>GPv2 veya Blob depolama hesabındaki bir lekenin katmanını değiştirme
# <a name="portal"></a>[Portal](#tab/azure-portal)
1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Azure portalında **Tüm Kaynakları**arayın ve seçin.

1. Depolama hesabınızı seçin.

1. Kabınızı seçin ve sonra blob seçin.

1. **Blob özelliklerinde,** **katmanı değiştir'i**seçin.

1. **Sıcak,** **Cool**veya **Arşiv** erişim katmanını seçin. Blob şu anda arşivde ve bir online katmana rehydrate istiyorsanız, ayrıca **Standart** veya **Yüksek**Bir Rehydrate Öncelik seçebilirsiniz.

1. Alttaki **Kaydet'i** seçin.

![Depolama hesabı katmanını değiştirme](media/storage-tiers/blob-access-tier.png)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
Aşağıdaki PowerShell komut dosyası blob katmanını değiştirmek için kullanılabilir. Değişken, `$rgName` kaynak grup adınız ile başharfe alınmalıdır. Değişken, `$accountName` depolama hesabı adınız ile başharfe bürünmelidir. Değişken, `$containerName` kapsayıcı adınız ile başharfe alınmalıdır. Değişken, `$blobName` blob adınız ile başharfe alınmalıdır. 
```powershell
#Initialize the following with your resource group, storage account, container, and blob names
$rgName = ""
$accountName = ""
$containerName = ""
$blobName == ""

#Select the storage account and get the context
$storageAccount =Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

#Select the blob from a container
$blob = Get-AzStorageBlob -Container $containerName -Blob $blobName -Context $ctx

#Change the blob’s access tier to archive
$blob.ICloudBlob.SetStandardBlobTier("Archive")
```
---

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Tüm depolama hesapları, her blob katmanına bağlı olarak Blob depolamayı engelle için bir fiyatlandırma modeli kullanır. Aşağıdaki faturalandırma hususlarını unutmayın:

- **Depolama maliyetleri**: Depolanan veri miktarına ek olarak, veri depolamanın maliyeti erişim katmanına bağlı olarak değişir. Katmanın erişim sıklığı düştükçe gigabayt başına ücret de azalır.
- **Veri erişimi maliyetleri**: Katmanın erişimi sıklığı düştükçe veri erişimi ücretleri artar. Serin ve arşiv erişim katmanındaki veriler için okumalar için gigabayt başına veri erişim ücreti ücretlendirilirsiniz.
- **İşlem maliyetleri**: Katman soğuttukça artan tüm katmanlar için işlem başına ücret vardır.
- **Coğrafi Çoğaltma veri aktarımı maliyetleri**: Bu ücret, GRS ve RA-GRS dahil olmak üzere yalnızca coğrafi çoğaltma yapılandırılmış hesaplara uygulanır. Coğrafi çoğaltma veri aktarımı gigabayt başına ücret doğurur.
- **Giden veri aktarımı maliyetleri**: Giden veri aktarımları (bir Azure bölgesinin dışına aktarılan veriler), genel amaçlı depolama hesapları ile tutarlı şekilde gigabayt başına esaslı olarak bant genişliği kullanımı için fatura doğurur.
- **Erişim katmanının değiştirilmesi**: Hesap erişim katmanının değiştirilmesi, hesapta açık bir katman kümesi olmayan _erişim katmanı çıkarılan_ blob'lar için katman değişikliği ücretlerine neden olur. Tek bir blob için erişim katmanını değiştirme hakkında bilgi için [Blob düzeyinde katmanlama faturalandırmasına](#blob-level-tiering-billing)bakın.

> [!NOTE]
> Blok blob'ları için fiyatlandırma hakkında daha fazla bilgi için [Azure Depolama Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/) sayfasına bakın. Giden veri aktarımı ücretlerine ilişkin daha fazla bilgi için [Veri Aktarımları Fiyatlandırma Bilgileri](https://azure.microsoft.com/pricing/details/data-transfers/) sayfasına bakın.

## <a name="faq"></a>SSS

**Verilerimi katmanlamak istersem Blob depolama veya GPv2 hesaplarını kullanmalı mıyım?**

Katman ayarlama için Blob depolama hesapları yerine GPv2 kullanmanızı öneririz. GPv2, Blob depolama hesaplarının desteklediği tüm özelliklerin yanı sıra başka birçoğunu da destekler. Blob depolama ile GPv2 ücretleri neredeyse aynıdır, ancak bazı yeni özellikler ve fiyat indirimleri yalnızca GPv2 hesaplarında kullanılabilir. GPv1 hesapları katmanlamayı desteklemez.

GPv1 ve GPv2 hesapları arasındaki fiyat yapısı farklıdır ve müşteriler GPv2 hesaplarını kullanmaya karar vermeden önce her ikisini de dikkatle değerlendirmelidir. Mevcut bir Blob depolama veya GPv1 hesabını tek tıklamada basit bir işlemle kolayca GPv2’ye dönüştürebilirsiniz. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

**Nesneleri üç (sıcak, serin ve arşiv) erişim katmanında da aynı hesapta saklayabilir miyim?**

Evet. Hesap düzeyinde ayarlanan **Erişim Katmanı** özniteliği, açık bir ayar katmanı olmadan bu hesaptaki tüm nesneler için geçerli olan varsayılan hesap katmanıdır. Blob düzeyinde katmanlama, hesaptaki erişim katmanı ayarı ne olursa olsun, erişim düzeyini nesne düzeyinde ayarlamanıza olanak tanır. Üç erişim katmanından herhangi birinde (sıcak, serin veya arşiv) lekeler aynı hesap içinde bulunabilir.

**Blob veya GPv2 depolama hesabımın varsayılan erişim katmanını değiştirebilir miyim?**

Evet, depolama hesabında **Access katmanı** özniteliğini ayarlayarak varsayılan hesap katmanını değiştirebilirsiniz. Hesap katmanını değiştirmek, hesapta depolanan açık bir katman kümesi olmayan tüm nesneler için geçerlidir (örneğin, **Sıcak (çıkarılan)** veya **Cool (çıkarılan)**). Yalnızca GPv2 hesaplarında belirli bir katman olmadan tüm bloblar için hesap katmanını sıcaktan soğuğa (10.000'de) yapıştırmak ve hem okuma işlemleri (10.000'de) hem de Blob depolama ve GPv2 hesaplarındaki tüm lekeler için veri alma (GB başına) ücretleri ne de cool'dan sıcak ait lere yapıştırMak.

**Varsayılan hesap erişim katmanımı arşiv olarak ayarlayabilir miyim?**

Hayır. Varsayılan hesap erişim katmanı olarak yalnızca sıcak ve havalı erişim katmanları ayarlanabilir. Arşiv yalnızca nesne düzeyinde ayarlanabilir. Blob yüklemede, varsayılan hesap katmanından bağımsız olarak, seçtiğiniz erişim katmanının sıcak, havalı veya arşiv li olmasını belirtirsiniz. Bu işlevsellik, blob depolamada veri oluşturduğunuz andan itibaren maliyet tasarrufu gerçekleştirmek için verileri doğrudan arşiv katmanına yazmanıza olanak tanır.

**Sıcak, serin ve arşiv erişim katmanları hangi bölgelerde kullanılabilir?**

Sıcak ve serin erişim katmanları ve blob düzeyinde katmanlama tüm bölgelerde mevcuttur. Arşiv depolama, başlangıçta yalnızca seçilmiş bölgelerde kullanılabilir. Tam liste için bkz. [Bölgelere göre kullanılabilir Azure ürünleri](https://azure.microsoft.com/regions/services/).

**Serin erişim katmanındaki lekeler sıcak erişim katmanındakilerden farklı mı?**

Sıcak erişim katmanındaki lekeler, GPv1, GPv2 ve Blob depolama hesaplarındaki lekelerle aynı gecikme ye sahiptir. Serin erişim katmanındaki lekeler, GPv1, GPv2 ve Blob depolama hesaplarındaki lekelere benzer bir gecikme alamına (milisaniye cinsinden) sahiptir. Arşiv erişim katmanındaki blob'ların GPv1, GPv2 ve Blob depolama hesaplarında birkaç saatlik gecikme savardır.

Serin erişim katmanındaki blob'lar, sıcak erişim katmanında depolanan lekelerden biraz daha düşük kullanılabilirlik hizmet düzeyine (SLA) sahiptir. Daha fazla bilgi için bkz. [depolama SLA’sı](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).

**İşlemler sık erişimli, seyrek erişimli ve arşiv katmanları arasında aynı mıdır?**

Sık erişimli ve seyrek erişimli arasında tüm işlemleri %100 tutarlıdır. GetBlobProperties, GetBlobMetadata, ListBlobs, SetBlobTier ve DeleteBlob dahil olmak üzere tüm geçerli arşiv işlemleri sıcak ve serin %100 uyumludur. Blob verileri, arşiv katmanında rehydrated'e kadar okunamıyor veya değiştirilemez; yalnızca blob meta veri okuma işlemleri arşivde yken desteklenir.

**Blobu arşiv katmanından sık erişimli veya seyrek erişimli katmana yeniden doldururken işlemin ne zaman tamamlandığını nasıl bilebilirim?**

Rehidrasyon sırasında, **Arşiv Durumu** özniteliğini yoklamak ve katman değişikliğinin ne zaman tamamlanagerektiğini onaylamak için get blob özelliklerini kullanabilirsiniz. Durum, hedef katmana göre "rehydrate-pending-to-hot" veya "rehydrate-pending-to-cool" olabilir. Tamamlandıktan sonra arşiv durumu özelliği kaldırılır ve **Erişim Katmanı** blob özelliği sık veya seyrek erişimli bu yeni katmanı gösterir. Daha fazla bilgi edinmek için [arşiv katmanından rehydrate blob verileri](storage-blob-rehydration.md) bakın.

**Blobun katmanını ayarladıktan sonra buna uygun fiyattan fatura almaya ne zaman başlarım?**

Her blob her zaman **blob'un Access Tier** özelliğinde belirtilen katmana göre faturalandırılır. Bir blob için yeni bir çevrimiçi katman ayarladığınızda, **Access Tier** özelliği tüm geçişler için yeni katmanı hemen yansıtır. Ancak, çevrimdışı arşiv katmanından sıcak veya serin bir katmana bir damlaremileme birkaç saat sürebilir. Bu durumda, rehidrasyon tamamlanana kadar arşiv ücretleriyle faturalandırılırsınız ve bu noktada **Access Tier** özelliği yeni katmanı yansıtır. Bir kez online katmana rehydrated, sıcak veya serin oranda bu blob için fatura konum.

**Bir blob'u serin veya arşiv katmanından çıkarırken veya çıkarırken erken silme ücretine tabi olup olmadığımı nasıl belirleyebilirim?**

Sırasıyla 30 ve 180 günden önce seyrek erişimli (yalnızca GPv2 hesapları) ya da arşiv katmanından silinen veya dışarı taşınan tüm bloblar eşit dağıtılmış bir erken silme ücreti ödenmesini gerektirir. Son katman değişikliğinin damgasını sağlayan **Access Tier Change Time** blob özelliğini denetleyerek bir blob'un serin veya arşiv katmanında ne kadar süre olduğunu belirleyebilirsiniz. Blob'un katmanı hiç değiştirilmediyse, **Son Değiştirilen** blob özelliğini kontrol edebilirsiniz. Daha fazla bilgi için [Cool'a bakın ve erken silme işlemini arşivleyin.](#cool-and-archive-early-deletion)

**Hangi Azure araçları ve SDK’lar blob düzeyinde katman ayarlamayı ve arşiv depolamayı destekliyor?**

Azure portalı, PowerShell ve CLI araçları ile .NET, Java, Python ve Node.js istemci kitaplıklarının tümü blob düzeyinde katman ayarlamayı ve arşiv depolamayı destekler.  

**Sık, seyrek ve arşiv katmanlarına ne kadar veri depolayabilirim?**

Veri depolama, diğer sınırlarla birlikte erişim katmanı başına değil, hesap düzeyinde ayarlanır. Limitinizin tümünü tek bir katmanda veya üç katmanda da kullanmayı seçebilirsiniz. Daha fazla bilgi [için, standart depolama hesapları için Ölçeklenebilirlik ve performans hedeflerine](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)bakın.

## <a name="next-steps"></a>Sonraki adımlar

GPv2 ve Blob depolama hesaplarında sıcak, serin ve arşivleme

- [Bölgeye göre sık, seyrek ve arşiv kullanılabilirliğini denetleme](https://azure.microsoft.com/regions/#services)
- [Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)
- [Kabarcık verilerini arşiv katmanından rehydrating hakkında bilgi edinin](storage-blob-rehydration.md)
- [Premium performansın uygulamanızdan fayda sağp alamayacağını belirleyin](storage-blob-performance-tiers.md)
- [Azure Depolama ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](../common/storage-enable-and-view-metrics.md)
- [Blob depolama ve GPv2 hesaplarında bölgeye göre sık erişimli, seyrek erişimli ve arşiv fiyatlarını denetleme](https://azure.microsoft.com/pricing/details/storage/)
- [Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)
