---
title: Blob 'lar için sık erişimli, seyrek erişimli ve arşiv erişim katmanları-Azure depolama
description: Azure depolama hesapları için sık erişimli, seyrek erişimli ve arşiv erişim katmanları.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/23/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: clausjor
ms.openlocfilehash: 48c6d6ed60045d906fcb711bd07ab492b6bbf488
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69543671"
---
# <a name="azure-blob-storage-hot-cool-and-archive-access-tiers"></a>Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları

Azure depolama, blob nesne verilerini en düşük maliyetli biçimde depolamanıza olanak sağlayan farklı erişim katmanları sunar. Kullanılabilir erişim katmanları şunları içerir:

- Sık erişilen verileri depolamak için sık iyileştirilmiş.
- Seyrek erişimli ve en az 30 gün boyunca depolanan verileri depolamak için en iyi duruma getirilmiştir.
- **Arşiv** -seyrek erişimli ve esnek gecikme süresi gereksinimleriyle en az 180 gün boyunca depolanan verileri depolamak için en iyi duruma getirilmiştir (saat sırasına göre).

Farklı erişim katmanları için aşağıdaki noktalar geçerlidir:

- Hesap düzeyinde yalnızca sık ve seyrek erişimli erişim katmanları ayarlanabilir. Arşiv erişim katmanı, hesap düzeyinde kullanılamaz.
- Sık erişimli, seyrek erişimli ve arşiv katmanları blob düzeyinde ayarlanabilir.
- Seyrek Erişimli erişim katmanındaki veriler biraz daha düşük kullanılabilirliğe sahip olabilir, ancak yine de sık erişimli verilerle benzer yüksek dayanıklılık, alma gecikmesi ve verimlilik özellikleri gerektirir. Seyrek Erişimli veriler için, hafif bir kullanılabilirlik hizmet düzeyi sözleşmesi (SLA) ve sık erişimli verilerle karşılaştırıldığında daha yüksek erişim maliyetleri, daha düşük depolama maliyetleri için kabul edilebilir.
- Arşiv depolama, verileri çevrimdışı olarak depolar ve en düşük depolama maliyetlerini sağlar, ayrıca en yüksek veri yeniden doldurma ve erişim maliyetlerine sahiptir.

Bulutta depolanan veriler üstel bir hızda artar. Artan depolama ihtiyaçlarınızın maliyetlerini yönetmek için, maliyetleri optimize etmek amacıyla erişim sıklığı ve planlanan elde tutma dönemi gibi özniteliklere bağlı olarak verilerinizi düzenlemek yararlıdır. Bulutta depolanan veriler, nasıl oluşturulduğu, işlendiği ve süresinin ömrü boyunca erişilme açısından farklı olabilir. Bazı veriler ve yaşam süresi boyunca aktif şekilde erişilebilir ve değiştirilebilir. Bazı verilere, veriler eskidikçe önemli ölçüde azalan erişimle, yaşam sürelerinin başlarında sık erişilebilir. Bazı veriler bulutta boş kalır ve oluşturulduktan sonra, daha seyrek erişimli olur.

Bu veri erişim senaryolarının her biri, belirli bir erişim deseninin en iyi duruma getirilmiş farklı bir erişim katmanından faydalanır. Sık erişimli, seyrek erişimli ve arşiv erişim katmanlarında Azure Blob depolama, ayrı fiyatlandırma modelleriyle farklı erişim katmanları için bu gereksinimi ortadan gideriyor.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="storage-accounts-that-support-tiering"></a>Katman ayarlamayı destekleyen depolama hesapları

Sık erişimli, seyrek erişimli veya arşiv 'e nesne depolama verileri katmanlama yalnızca BLOB depolama ve Genel Amaçlı v2 (GPv2) hesaplarında desteklenir. Genel Amaçlı v1 (GPv1) hesaplar katman ayarlamayı desteklemez. Bununla birlikte, müşteriler Azure portalında basit bir tek tıklama işlemiyle var olan GPv1 veya Blob depolama hesaplarını GPv2 hesaplarına kolayca dönüştürebilir. GPv2; Bloblar, dosyalar ve kuyruklar için yeni bir fiyatlandırma yapısı ve ayrıca çeşitli yeni depolama özelliklerine erişim sağlar. Ayrıca, bazı yeni özelliklere geçmek ve fiyat indirimleri yalnızca GPv2 hesaplarda sunulur. Bu nedenle, müşteriler, bazı iş yükleri GPv1 'den GPv2 üzerinde daha pahalı olabildikleri için, yeni fiyatlandırmayı büyük ölçüde gözden geçirdikten sonra GPv2 hesaplarını kullanmayı değerlendirmelidir. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

BLOB depolama ve GPv2 hesapları, **erişim katmanı** özniteliğini hesap düzeyinde kullanıma sunar. Bu öznitelik, varsayılan erişim katmanını, nesne düzeyinde ayarlanmış açık bir katmanı olmayan depolama hesabındaki tüm Bloblar için sık veya seyrek erişimli olarak belirtmenizi sağlar. Katmanı nesne düzeyinde ayarlanan nesnelerde hesap katmanı geçerli olmaz. Arşiv katmanı yalnızca nesne düzeyinde uygulanabilir. Bu erişim katmanları arasında istediğiniz zaman geçiş yapabilirsiniz.

## <a name="premium-performance-block-blob-storage"></a>Premium performans bloğu blob depolaması

Premium performans bloğu blob depolaması, sık erişilen verilerin yüksek performanslı donanımlar aracılığıyla kullanılabilmesini sağlar. Bu performans katmanındaki veriler, düşük ve tutarlı gecikme süresi için iyileştirilmiş katı hal sürücülerinde (SSD) depolanır. SSD 'Ler geleneksel sabit sürücülere kıyasla daha yüksek işlem hızları ve verimlilik sağlar.

Premium performans bloğu blob depolaması, hızlı ve tutarlı yanıt süreleri gerektiren iş yükleri için idealdir. Telemetri verilerini, mesajlaşma ve veri dönüştürmeyi yakalama gibi birçok küçük işlem gerçekleştiren iş yükleri için idealdir. Etkileşimli video düzenlemesi, statik Web içeriği ve çevrimiçi işlemler gibi son kullanıcıları içeren veriler de iyi adaylardır.

Premium performans bloğu BLOB depolama alanı yalnızca blok BLOB depolama hesabı türü aracılığıyla kullanılabilir ve şu anda sık erişimli, seyrek erişimli veya arşiv erişim katmanlarında katmanlamayı desteklememektedir.

## <a name="hot-access-tier"></a>Sık erişim katmanı

Sık erişimli erişim katmanı seyrek erişimli ve arşiv katmanlarından daha yüksek depolama maliyetlerine sahiptir, ancak en düşük erişim ücretleri vardır. Sık erişim katmanı için örnek kullanım senaryoları şunları içerir:

- Etkin kullanımda olan veya erişilmesi beklenen veriler (okuma ve yazma) sıklıkla.
- Seyrek Erişimli katmana işleme ve son geçiş için hazırlanan veriler.

## <a name="cool-access-tier"></a>Seyrek erişim katmanı

Seyrek Erişimli erişim katmanının depolama maliyetleri ve daha yüksek erişim maliyetleri, sık erişimli depolama ile karşılaştırıldığında daha düşüktür. Bu katman, seyrek erişim katmanında en az 30 gün boyunca kalacak verilere yöneliktir. Seyrek Erişimli erişim katmanı için örnek kullanım senaryoları şunları içerir:

- Kısa süreli yedekleme ve olağanüstü durum kurtarma veri kümeleri.
- Artık sık görüntülenmeyen ancak erişildiğinde hemen kullanılabilir olması beklenen eski medya içeriği.
- Gelecekte işlenmek üzere daha fazla veri toplanırken uygun maliyetli olarak depolanması gereken büyük veri kümeleri. (*Örneğin*, bilimsel verilerin uzun süreli depolanması, üretim tesisinden alınan ham telemetri verileri)

## <a name="archive-access-tier"></a>Arşiv erişim katmanı

Arşiv erişim katmanı, sık ve seyrek katmanlarla karşılaştırıldığında en düşük depolama maliyetine ve daha yüksek veri alma maliyetine sahiptir. Bu katman, birkaç saatlik alma gecikmesinden etkilenmeyecek ve arşiv katmanında en az 180 gün kalacak verilere yöneliktir.

Blob arşiv depolarken blob verileri çevrimdışı olur ve okunamaz, kopyalanabilir, üzerine yazılamaz veya değiştirilemez. Arşiv depolamadaki bir Blobun anlık görüntülerini alamazsınız. Ancak, blob meta verileri çevrimiçi ve kullanılabilir durumda kalır ve bu da blobu ve özelliklerini listelemenize olanak tanır. Arşivdeki Bloblar için, tek geçerli işlemler GetBlobProperties, GetBlobMetadata, Listbloblar, SetBlobTier ve DeleteBlob.

Arşiv erişim katmanı için örnek kullanım senaryoları şunları içerir:

- Uzun vadeli yedekleme, ikincil yedekleme ve arşiv veri kümeleri
- Son kullanılabilir biçime işlendikten sonra bile özgün (ham) veriler korunmalıdır. (*Örneğin*, kodlama diğer biçimlere dönüştürüldükten sonra ham medya dosyaları)
- Uzun süre depolanması gereken ve çok ender erişilecek uyumluluk ve arşiv verileri. (*Örneğin*, güvenlik kamerası çekimi, eski X ışınlar/MRA sağlık kuruluşları, ses kayıtları ve finans hizmetleri için müşteri çağrılarının dökümünü

### <a name="blob-rehydration"></a>Blob yeniden doldurma

[!INCLUDE [storage-blob-rehydrate-include](../../../includes/storage-blob-rehydrate-include.md)]
Daha fazla bilgi edinmek için [Arşiv katmanından blob verilerini yeniden doldurma](storage-blob-rehydration.md) bölümüne bakın.  

## <a name="account-level-tiering"></a>Hesap düzeyi katmanlama

Her üç erişim katmanında blob 'lar aynı hesap içinde bulunabilir. Açıkça atanan bir katmanı olmayan tüm bloblar katmanı hesap erişim katmanının ayarını çıkarır. Erişim katmanı hesaptan çıkarsanamıyor, **erişim katmanı tarafından çıkarılan** blob özelliği "true" olarak ayarlanmış ve BLOB **erişim katmanı** blobu özelliği hesap katmanıyla eşleşir. Azure portal, _erişim katmanının çıkartılan_ özelliği, blob erişim katmanı ile **sık erişimli (çıkarılabilen)** veya seyrek erişimli **(çıkarılan)** olarak görüntülenir.

Hesap erişim katmanını değiştirmek, bir açık katman kümesi olmayan hesapta depolanan tüm _erişim katmanı_ nesneleri için geçerlidir. Hesap katmanını sık erişilenden seyrek erişilene değiştirirseniz yalnızca GPv2 hesaplarında ayarlanmış katmanı olmayan tüm bloblar için yazma işlemleri (10.000 işlem başına) ücretlendirilir. BLOB depolama hesaplarında bu değişiklik için ücret alınmaz. Seyrek Erişimli bir BLOB depolama veya GPv2 hesaplarında sık erişimli olarak geçiş yaparsanız, hem okuma işlemleri (10.000 başına) hem de veri alımı (GB başına) için ücretlendirilirsiniz.

## <a name="blob-level-tiering"></a>Blob düzeyinde katman ayarlama

Blob düzeyinde katman ayarlama, [Blob Katmanını Ayarlama](/rest/api/storageservices/set-blob-tier) adlı tek bir işlem kullanarak nesne düzeyinde verilerinizin katmanını değiştirmenize olanak verir. Bir blob’un erişim katmanını, kullanım şekli değiştikçe verileri hesapları arasında taşımaya gerek kalmadan sık erişimli, seyrek erişimli veya arşiv katmanları arasında kolayca değiştirebilirsiniz. Tüm katman değişiklikleri anında gerçekleşir. Ancak, arşivden bir blobu yeniden doldurma birkaç saat sürebilir.

Son blob katmanı değişikliğinin zamanı, **Erişim Katmanı Değişim Zamanı** blob özelliği aracılığıyla gösterilir. Blob arşiv katmanındaysa üzerine yazılamaz, bu nedenle aynı Blobun karşıya yüklemeye Bu senaryoda izin verilmez. Bir Blobun, sık erişimli veya seyrek erişimli bir katmanda üzerine yazabilir, bu durumda yeni blob üzerine yazılmış olan Blobun katmanını devralır.

> [!NOTE]
> Arşiv depolama ve blob düzeyinde katman ayarlama, yalnızca blok bloblarını destekler. Ayrıca, anlık görüntülerine sahip bir blok blobunun katmanını değiştiremezsiniz.

### <a name="blob-lifecycle-management"></a>Blob yaşam döngüsü yönetimi

Blob Storage yaşam döngüsü yönetimi, verilerinizi en iyi erişim katmanına geçirmeye ve yaşam döngüsünün sonunda verilerin süresini dolabilmeniz için kullanabileceğiniz zengin, kural tabanlı bir ilke sunar. Daha fazla bilgi edinmek için bkz. [Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md) .  

> [!NOTE]
> Blok Blobu depolama hesabında (Premium performans) depolanan veriler şu anda [BLOB katmanını ayarla](/rest/api/storageservices/set-blob-tier) veya Azure Blob depolama yaşam döngüsü yönetimi kullanılarak sık erişimli, seyrek erişimli veya arşiv olarak katmanlanamaz.
> Verileri taşımak için, Blok Blob depolama hesabındaki Blobları, [URL API 'Sinden put bloğu](/rest/api/storageservices/put-block-from-url) veya bu API 'yi destekleyen bir AzCopy sürümü kullanarak farklı bir hesapta bulunan sık erişimli erişim katmanına kopyalamanız gerekir.
> *URL API 'Den yerleştirme bloğu* , sunucudaki verileri eşzamanlı olarak kopyalar, yani çağrı yalnızca tüm veriler özgün sunucu konumundan hedef konuma taşındıktan sonra tamamlanır.

### <a name="blob-level-tiering-billing"></a>Blob düzeyinde katman ayarlama faturalandırması

Bir blob daha soğuk bir katmana taşındığında (sık erişimli > seyrek erişimli, > sık erişimli arşiv veya seyrek erişimli > Arşivi), bu işlem hedef katmana yazma işlemi olarak faturalandırılır ve hedef katmanın yazma işlemi (10.000 başına) ile veri yazma (GB başına) ücretleri uygulanır.

Bir blob bir çarpıtma katmanına taşındığında (Arşiv-> seyrek erişimli, Arşiv > sık erişimli > veya seyrek erişimli), bu işlem kaynak katmandan okuma olarak faturalandırılır ve kaynak katmanın okuma işlemi (10.000 başına) ile veri alma (GB başına) ücretleri uygulanır. Seyrek erişim veya arşiv katmanı dışına taşınmış tüm bloblar için erken silme ücretleri de uygulanabilir. Aşağıdaki tabloda, katman değişikliklerinin nasıl faturalandırıldığını özetlenmektedir.

| | **Yazma ücretleri (Işlem + erişim)** | **Okuma ücretleri (Işlem + erişim)**
| ---- | ----- | ----- |
| **SetBlobTier yönü** | sık erişimli > seyrek erişimli,<br> sık erişimli > Arşivi,<br> Seyrek Erişimli > Arşivi | Arşiv-> seyrek erişimli,<br> Arşiv-> sık erişimli,<br> Seyrek Erişimli > sık erişimli

### <a name="cool-and-archive-early-deletion"></a>Seyrek erişimli ve arşiv erken silme

GB başına aylık ücrete ek olarak seyrek erişimli katmana taşınan tüm bloblar (yalnızca GPv2 hesapları) 30 günlük seyrek erişim erken silme süresine tabidir ve arşiv katmanına taşınan tüm bloblar da 180 günlük arşiv erken silme süresine tabidir. Bu ücret eşit olarak bölünür. Örneğin, bir blob arşive taşınır ve ardından silinir veya 45 gün sonra sık erişimli katmana taşınırsa sizden o blobu arşivde 135 (180 eksi 45) gün depolamaya eşdeğer bir erken silme ücreti istenir.

Bir erişim katmanı değişikliği yoksa, **son değiştirilme**olan blob özelliğini kullanarak erken silme işlemini hesaplayabilirsiniz. Aksi takdirde, erişim katmanının en son seyrek erişimli veya arşiv olarak değiştirildiği zaman, blob özelliğini görüntüleyerek kullanabilirsiniz: **erişim katmanı değiştirme zamanı**. Blob özellikleri hakkında daha fazla bilgi için bkz. [BLOB özelliklerini al](https://docs.microsoft.com/rest/api/storageservices/get-blob-properties).

## <a name="comparing-block-blob-storage-options"></a>Blok Blobu depolama seçeneklerini karşılaştırma

Aşağıdaki tabloda, Premium performans bloğu blob depolaması ve sık erişimli, seyrek erişimli ve arşiv erişim katmanları karşılaştırması gösterilmektedir.

|                                           | **Premium performans**   | **Etkin katman** | **Cool katmanı**       | **Arşiv katmanı**  |
| ----------------------------------------- | ------------------------- | ------------ | ------------------- | ----------------- |
| **Kullanılabilirlik**                          | %99,9                     | %99,9        | %99                 | Çevrimdışı           |
| **Kullanılabilirlik** <br> **(RA-GRS okumaları)**  | Yok                       | %99,99       | %99,9               | Çevrimdışı           |
| **Kullanım ücretleri**                         | Daha yüksek depolama maliyetleri, daha düşük erişim ve işlem maliyeti | Daha yüksek depolama maliyetleri, daha düşük erişim ve işlem maliyetleri | Daha düşük depolama maliyetleri, daha yüksek erişim ve işlem maliyetleri | En düşük depolama maliyetleri, en yüksek erişim ve işlem maliyetleri |
| **En düşük nesne boyutu**                   | Yok                       | Yok          | Yok                 | Yok               |
| **En az depolama süresi**              | Yok                       | Yok          | 30 gün<sup>1</sup> | 180 gün
| **Gecikme süresi** <br> **(İlk bayta kadar süre)** | Tek basamaklı milisaniye | milisaniye | milisaniye        | Saat<sup>2</sup> |

GPv2 hesaplarındaki seyrek katmandaki <sup>1</sup> nesne en az 30 günlük saklama süresine sahiptir. BLOB depolama hesaplarının Cool katmanı için en düşük saklama süresi yoktur.

<sup>2</sup> arşiv depolama Şu anda, farklı alma gecikmeleri sunan 2 yeniden doldurma önceliklerini, yüksek ve standart 'ı desteklemektedir. Daha fazla bilgi için bkz. [BLOB yeniden doldurma](#blob-rehydration).

> [!NOTE]
> BLOB depolama hesapları, genel amaçlı v2 depolama hesaplarıyla aynı performans ve ölçeklenebilirlik hedeflerini destekler. Daha fazla bilgi için bkz. [Azure Storage ölçeklenebilirlik ve performans hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="quickstart-scenarios"></a>Hızlı Başlangıç senaryoları

Bu bölümde Azure portalı kullanarak aşağıdaki senaryolar gösterilmektedir:

- GPv2 veya Blob depolama hesabının varsayılan hesap erişim katmanını değiştirme.
- GPv2 veya Blob depolama hesabında blobun katmanını değiştirme.

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>GPv2 veya Blob depolama hesabının varsayılan hesap erişim katmanını değiştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Depolama hesabınıza gitmek için Tüm Kaynaklar’ı ve ardından depolama hesabınızı seçin.

1. Ayarlar dikey penceresinde **Yapılandırma**’ya tıklayarak hesap yapılandırmasını görüntüleyin ve/veya değiştirin.

1. Gereksinimleriniz için doğru erişim katmanını seçin: **Erişim katmanını seyrek erişimli** veya **sık**erişimli olarak ayarlayın.

1. Dikey pencerenin en üstündeki **Kaydet**'e tıklayın.

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>GPv2 veya blob depolama hesabındaki bir Blobun katmanını değiştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Depolama hesabınızdaki blobunuza gitmek için Tüm Kaynaklar’ı seçin, depolama hesabınızı seçin, kapsayıcınızı seçin ve ardından blobunuzu seçin.

1. **BLOB özellikleri** dikey penceresinde katmanı **Değiştir** düğmesini seçerek katman dikey penceresini açın.

1. **Sık**erişimli, seyrekerişimli veya **Arşiv** erişim katmanını seçin. Blobun Şu anda arşivde varsa ve çevrimiçi bir katmana yeniden dönmek istiyorsanız, **Standart** veya **yüksek**bir yeniden doldurma önceliği de seçebilirsiniz.

1. Dikey pencerenin alt kısmındaki **Tamam**’a tıklayın.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Tüm depolama hesapları, Blok Blobu depolama için her Blobun katmanını temel alan bir fiyatlandırma modeli kullanır. Aşağıdaki faturalandırma konularını göz önünde bulundurun:

- **Depolama maliyetleri**: Depolanan veri miktarına ek olarak, veri depolama maliyeti erişim katmanına bağlı olarak değişir. Katmanın erişim sıklığı düştükçe gigabayt başına ücret de azalır.
- **Veri erişim maliyetleri**: Katman daha soğuk aldığından veri erişimi ücretleri artar. Seyrek erişimli ve arşiv erişim katmanındaki veriler için, okuma için gigabayt başına veri erişim ücreti üzerinden ücretlendirilirsiniz.
- **İşlem maliyetleri**: Katman daha soğuk aldığından artan tüm katmanlar için işlem başına ücret uygulanır.
- **Coğrafi çoğaltma veri aktarımı maliyetleri**: Bu ücret yalnızca GRS ve RA-GRS dahil olmak üzere, coğrafi çoğaltma yapılandırılmış hesaplara uygulanır. Coğrafi çoğaltma veri aktarımı gigabayt başına ücret doğurur.
- **Giden veri aktarımı maliyetleri**: Giden veri aktarımları (bir Azure bölgesinden aktarılan veriler), genel amaçlı depolama hesaplarıyla tutarlı, gigabayt başına, bant genişliği kullanımı için faturalandırılır.
- **Erişim katmanını değiştirme**: Hesap erişim katmanını değiştirmek, açık bir katman kümesi olmayan hesapta depolanan _erişim katmanı çıkarılan_ blob 'ların katman değişikliği ücretlerine neden olur. Tek bir Blobun erişim katmanını değiştirme hakkında daha fazla bilgi için lütfen [BLOB düzeyi katmanlama faturalama](#blob-level-tiering-billing)bölümüne bakın.

> [!NOTE]
> Blok Blobları için fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/) sayfası. Giden veri aktarımı ücretlerine ilişkin daha fazla bilgi için [Veri Aktarımları Fiyatlandırma Bilgileri](https://azure.microsoft.com/pricing/details/data-transfers/) sayfasına bakın.

## <a name="faq"></a>SSS

**Verilerime katman ayarlamak istediğimde Blob depolamayı mı yoksa GPv2 hesaplarını mı kullanmalıyım?**

Katman ayarlama için Blob depolama hesapları yerine GPv2 kullanmanızı öneririz. GPv2, Blob depolama hesaplarının desteklediği tüm özelliklerin yanı sıra başka birçoğunu da destekler. Blob depolama ile GPv2 ücretleri neredeyse aynıdır, ancak bazı yeni özellikler ve fiyat indirimleri yalnızca GPv2 hesaplarında kullanılabilir. GPv1 hesapları katman ayarlamayı desteklemez.

GPv1 ve GPv2 hesapları arasındaki fiyat yapısı farklıdır ve müşteriler GPv2 hesaplarını kullanmaya karar vermeden önce her ikisini de dikkatle değerlendirmelidir. Mevcut bir Blob depolama veya GPv1 hesabını tek tıklamada basit bir işlemle kolayca GPv2’ye dönüştürebilirsiniz. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

**Aynı hesapta üç (sık erişimli, seyrek erişimli ve arşiv) erişim katmanlarında nesneleri depolayabilirim miyim?**

Evet. Hesap düzeyinde ayarlanan **erişim katmanı** özniteliği, bu hesaptaki tüm nesneler için açık bir küme katmanı olmadan uygulanan varsayılan hesap katmandır. Ancak blob düzeyinde katman ayarlama, hesabın erişim katmanı ayarından bağımsız olarak nesne düzeyinde erişim katmanını açık olarak ayarlamanıza olanak tanır. Aynı hesapta üç erişim katmanının (sık erişimli, seyrek erişimli veya Arşiv) hiçbirinde blob 'lar bulunabilir.

**Blob veya GPv2 depolama hesabımın varsayılan erişim katmanını değiştirebilir miyim?**

Evet, depolama hesabındaki **erişim katmanı** özniteliğini ayarlayarak varsayılan hesap katmanını değiştirebilirsiniz. Hesap katmanını değiştirmek, hesapta depolanan ve açık katman kümesi olmayan (örneğin, **sık erişimli (çıkarılan)** veya seyrek erişimli **(çıkarılabilen)** tüm nesneler için geçerlidir. Hesap katmanını, yalnızca GPv2 hesaplarında ayarlanmış bir katman olmadan tüm Bloblar için sık erişimli ve seyrek erişimli bir yazma işlemi (10.000 başına) olarak değiştirme ve seyrek erişimli 'ten sık erişimli olarak değiştirme işlemi, BLOB depolama alanındaki tüm Bloblar için hem okuma işlemleri (10.000 başına) hem de veri alma (GB başına) ücretleri ve GPv2 hesapları.

**Varsayılan hesap erişim katmanımı arşiv olarak ayarlayabilir miyim?**

Hayır. Yalnızca sık ve seyrek erişimli katmanlar varsayılan hesap erişim katmanı olarak ayarlanabilir. Arşiv yalnızca nesne düzeyinde ayarlanabilir.

**Hangi bölgelerde kullanılabilir sık erişimli, seyrek erişimli ve arşiv erişim katmanları vardır?**

Blob düzeyi katmanlama ile birlikte sık ve seyrek erişimli katmanlar, tüm bölgelerde kullanılabilir. Arşiv depolama, başlangıçta yalnızca seçilmiş bölgelerde kullanılabilir. Tam liste için bkz. [Bölgelere göre kullanılabilir Azure ürünleri](https://azure.microsoft.com/regions/services/).

**Seyrek Erişimli erişim katmanındaki Bloblar, sık erişimli erişim katmanından farklı şekilde davranır mi?**

Sık erişimli erişim katmanındaki Bloblar, GPv1, GPv2 ve BLOB depolama hesaplarındaki bloblarla aynı gecikme süresine sahiptir. Seyrek Erişimli erişim katmanındaki Bloblar, GPv1, GPv2 ve BLOB depolama hesaplarındaki bloblarla benzer gecikme süresine (milisaniye olarak) sahiptir. Arşiv erişim katmanındaki Bloblar, GPv1, GPv2 ve BLOB depolama hesaplarında birkaç saatlik gecikme süresine sahiptir.

Seyrek Erişimli erişim katmanındaki Bloblar, sık erişimli erişim katmanında depolanan bloblara göre biraz daha düşük kullanılabilirlik hizmet düzeyine (SLA) sahiptir. Daha fazla bilgi için bkz. [depolama SLA’sı](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).

**İşlemler sık erişimli, seyrek erişimli ve arşiv katmanları arasında aynı mıdır?**

Sık erişimli ve seyrek erişimli arasında tüm işlemleri %100 tutarlıdır. GetBlobProperties, GetBlobMetadata, Listbloblar, SetBlobTier ve DeleteBlob dahil tüm geçerli arşiv işlemleri, sık erişimli ve seyrek erişimli% 100 tutarlıdır. Blob verileri, yeniden doldurma yapılıncaya kadar arşiv katmanında okunamaz veya değiştirilemez; Arşiv sırasında yalnızca blob meta veri okuma işlemleri desteklenir.

**Blobu arşiv katmanından sık erişimli veya seyrek erişimli katmana yeniden doldururken işlemin ne zaman tamamlandığını nasıl bilebilirim?**

Yeniden doldurma işlemi sırasında, katman değişikliğinin ne zaman tamamlandığını onaylamak üzere **Arşiv Durumu** özniteliğini yoklamak için blob özelliklerini alma işlemini kullanabilirsiniz. Durum, hedef katmana göre "rehydrate-pending-to-hot" veya "rehydrate-pending-to-cool" olabilir. Tamamlandıktan sonra arşiv durumu özelliği kaldırılır ve **Erişim Katmanı** blob özelliği sık veya seyrek erişimli bu yeni katmanı gösterir.  

**Blobun katmanını ayarladıktan sonra buna uygun fiyattan fatura almaya ne zaman başlarım?**

Her blob her zaman blob 'un **erişim katmanı** özelliği tarafından belirtilen katmana göre faturalandırılır. Blob için yeni bir katman ayarladığınızda, **Access Tier** özelliği tüm geçişler için yeni katmanı hemen yansıtır. Ancak arşiv katmanındaki bir blobun sık veya seyrek erişim katmanına alınması birkaç saat sürebilir. Bu durumda, yeniden doldurma tamamlanana kadar arşiv fiyatları üzerinden faturalandırılırsınız, bu noktada **erişim katmanı** özelliği yeni katmanı yansıtır. Bu noktada, bu blob için sık veya seyrek hızda faturalandırılırsınız.

**Seyrek erişimli veya arşiv katmanındaki bir blobu silerken veya dışarı taşırken erken silme ücreti ödeyip ödemeyeceğimi nasıl anlarım?**

Sırasıyla 30 ve 180 günden önce seyrek erişimli (yalnızca GPv2 hesapları) ya da arşiv katmanından silinen veya dışarı taşınan tüm bloblar eşit dağıtılmış bir erken silme ücreti ödenmesini gerektirir. Bir Blobun, son katman değişikliğinin bir damgasını sağlayan **erişim katmanı değiştirme zamanı** blobu özelliğini denetleyerek seyrek erişimli veya arşiv katmanında ne kadar süreyle olduğunu belirleyebilirsiniz. Blobun katmanı hiçbir şekilde değiştirilmediyse, **son değiştirilen** blob özelliğini kontrol edebilirsiniz. Daha fazla bilgi için bkz. Seyrek Erişimli [ve arşiv erken silme](#cool-and-archive-early-deletion).

**Hangi Azure araçları ve SDK’lar blob düzeyinde katman ayarlamayı ve arşiv depolamayı destekliyor?**

Azure portalı, PowerShell ve CLI araçları ile .NET, Java, Python ve Node.js istemci kitaplıklarının tümü blob düzeyinde katman ayarlamayı ve arşiv depolamayı destekler.  

**Sık, seyrek ve arşiv katmanlarına ne kadar veri depolayabilirim?**

Diğer limitlerle birlikte veri depolama alanı, erişim katmanına göre değil, hesap düzeyinde ayarlanır. Bu nedenle, tüm sınırınızı tek bir katmanda veya üç katmanın tümünde kullanmayı tercih edebilirsiniz. Daha fazla bilgi için bkz. [Azure Storage ölçeklenebilirlik ve performans hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar

### <a name="evaluate-hot-cool-and-archive-in-gpv2-and-blob-storage-accounts"></a>GPv2 ve BLOB depolama hesaplarında sık erişimli, seyrek erişimli ve arşivi değerlendirin

[Bölgeye göre sık, seyrek ve arşiv kullanılabilirliğini denetleme](https://azure.microsoft.com/regions/#services)

[Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)

[Blob verilerini arşiv katmanından yeniden doldurma hakkında bilgi edinin](storage-blob-rehydration.md)

[Azure Depolama ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](../common/storage-enable-and-view-metrics.md)

[Blob depolama ve GPv2 hesaplarında bölgeye göre sık erişimli, seyrek erişimli ve arşiv fiyatlarını denetleme](https://azure.microsoft.com/pricing/details/storage/)

[Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)
