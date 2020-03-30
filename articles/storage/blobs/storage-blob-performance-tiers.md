---
title: Blob depolama performans katmanlarını engelleyin — Azure Depolama
description: Azure blok blob depolama için premium ve standart performans katmanları arasındaki farkı tartışır.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 11/12/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: clausjor
ms.openlocfilehash: ff82986b27d038c536872b07e1308b0d48fadaef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74270220"
---
# <a name="performance-tiers-for-block-blob-storage"></a>Blok blob depolama için performans katmanları

Şirketler performansa duyarlı bulut tabanlı uygulamaları dağıttıkça, farklı performans düzeylerinde uygun maliyetli veri depolama seçeneklerine sahip olmak önemlidir.

Azure blok blob depolama iki farklı performans katmanı sunar:

- **Premium**: yüksek işlem oranları ve tek haneli tutarlı depolama gecikmesi için optimize edilsin
- **Standart**: yüksek kapasite ve yüksek üretim için optimize edin

Aşağıdaki hususlar farklı performans katmanları için geçerlidir:

| Alan |Standart performans  |Üstün performans  |
|---------|---------|---------|
|Bölge kullanılabilirliği     |   Tüm bölgeler      | [Belirli bölgelerde](https://azure.microsoft.com/global-infrastructure/services/?products=storage)       |
|Desteklenen [depolama hesabı türleri](../common/storage-account-overview.md#types-of-storage-accounts)     |     Genel amaçlı v2, BlobStorage, Genel amaçlı v1    |    BlockBlobDepolama     |
|[Yüksek iş artışı blok lekelerini](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/) destekler     |    Evet, 4 MiB PutBlock veya PutBlob boyutundan daha büyük     |    Evet, 256 KiB PutBlock veya PutBlob boyutlarından daha fazla    |
|Yedeklilik     |     Bkz. [Depolama Hesabı Türleri](../common/storage-account-overview.md#types-of-storage-accounts)   |  Şu anda yalnızca yerel olarak yedekli depolamayı (LRS) ve bölge redudant depolamayı (ZRS) destekler<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>1</sup></div>     |

<div id="zone-redundant-storage"><sup>1.1.2</sup> Bölge yedekli depolama (ZRS) premium performans bloğu blob depolama hesapları için belirli bölgelerde kullanılabilir.</div>

Maliyetle ilgili olarak, premium performans, bu iş yükleri için [toplam depolama maliyetini düşürmeye](https://azure.microsoft.com/blog/reducing-overall-storage-costs-with-azure-premium-blob-storage/) yardımcı olmak için yüksek işlem oranlarına sahip uygulamalar için optimize edilmiş fiyatlandırma sağlar.

## <a name="premium-performance"></a>Üstün performans

Üstün performans bloğu blob depolama, yüksek performanslı donanım aracılığıyla verileri kullanılabilir hale getirir. Veriler, düşük gecikme süremi için optimize edilmiş katı hal sürücülerinde (SSD'ler) depolanır. SSD'ler geleneksel sabit disklere göre daha yüksek iş elde sağlar.

Üstün performans depolama, hızlı ve tutarlı yanıt süreleri gerektiren iş yükleri için idealdir. Birçok küçük işlem gerçekleştiren iş yükleri için en iyisidir. Örnek iş yükleri şunlardır:

- **Etkileşimli iş yükleri.** Bu iş yükleri, e-ticaret ve haritalama uygulamaları gibi anlık güncelleştirmeler ve kullanıcı geri bildirimleri gerektirir. Örneğin, bir e-ticaret uygulamasında, daha az sıklıkta görüntülenen öğeler önbelleğe alınmaz. Ancak, talep üzerine anında müşteriye görüntülenmelidir.

- **Analitik**. Bir IoT senaryosunda, çok sayıda küçük yazma işlemi her saniye buluta itilebilir. Büyük miktarda veri alınabilir, analiz amacıyla toplanabilir ve hemen silinebilir. Premium blok blob depolamanın yüksek yutma özellikleri, bu tür bir iş yükü için verimli olmasını sağlar.

- **Yapay zeka/makine öğrenimi (AI/ML)**. AI/ML, görsel, konuşma ve metin gibi farklı veri türlerinin tüketimi ve işlenmesiyle ilgilenir. Bu yüksek performanslı iş yükü türü, veri analizi için hızlı yanıt ve verimli işlem süreleri gerektiren büyük miktarda veriyle ilgilenir.

- **Veri dönüşümü.** Verilerin sürekli düzenlenmesi, değiştirilmesi ve dönüştürülmesini gerektiren işlemler anında güncelleştirmeler gerektirir. Doğru veri gösterimi için, bu verilerin tüketicileri bu değişikliklerin hemen yansıtTığını görmelidir.

## <a name="standard-performance"></a>Standart performans

Standart performans, verileri en uygun maliyetli şekilde depolamak için farklı [erişim katmanlarını](storage-blob-storage-tiers.md) destekler. Büyük veri kümelerinde yüksek kapasite ve yüksek iş için optimize edin.

- **Yedekleme ve olağanüstü durum kurtarma veri kümeleri.** Standart performans depolama, hem kısa hem de uzun vadeli olağanüstü durum kurtarma veri kümeleri, ikincil yedeklemeler ve uyumluluk veri arşivleme için mükemmel bir kullanım örneği haline getirerek uygun maliyetli katmanlar sunar.

- **Medya içeriği**. İlk oluşturulduğunda ve depolandığında resimlere ve videolara genellikle sık sık erişilir, ancak bu içerik türü yaşlandıkça daha az kullanılır. Standart performans depolama, medya içeriği ihtiyaçları için uygun katmanlar sunar. 

- **Toplu veri işleme**. Bu tür iş yükleri, tutarlı düşük gecikme alanı yerine uygun maliyetli yüksek iş çıkışlı depolama gerektirdiğinden standart depolama için uygundur. Büyük, ham veri kümeleri işleme için aşamalı ve sonunda soğutucu katmanları na geçirilir.

## <a name="migrate-from-standard-to-premium"></a>Standarttan premium'a geçiş

Varolan standart performans depolama hesabını, üstün performansla bir blok blob depolama hesabına dönüştüremezsiniz. Premium performans depolama hesabına geçiş yapmak için bir BlockBlobStorage hesabı oluşturmanız ve verileri yeni hesaba geçirmeniz gerekir. Daha fazla bilgi için [blockblobstorage hesabı oluştur'a](storage-blob-create-account-block-blob.md)bakın.

Depolama hesapları arasındaki lekeleri kopyalamak için [AzCopy](../common/storage-use-azcopy-blobs.md) komut satırı aracının en son sürümünü kullanabilirsiniz. Azure Veri Fabrikası gibi diğer araçlar da veri hareketi ve dönüşümü için kullanılabilir.

## <a name="blob-lifecycle-management"></a>Blob yaşam döngüsü yönetimi

Blob depolama yaşam döngüsü yönetimi zengin, kural tabanlı bir ilke sunar:

- **Premium**: Yaşam döngüsünün sonunda ki verileri sona erdirin.
- **Standart**: Verileri en iyi erişim katmanına geçirin ve yaşam döngüsünün sonunda son kullanma verileri.

Daha fazla bilgi için Azure [Blob depolama yaşam döngüsünü yönet'e](storage-lifecycle-management-concepts.md)bakın.

Premium blok blob depolama hesabında depolanan verileri sıcak, serin ve arşiv katmanları arasında taşıyamazsınız. Ancak, blob'ları bir blok blob depolama hesabından *farklı* bir hesaptaki sıcak erişim katmanına kopyalayabilirsiniz. Verileri farklı bir hesaba kopyalamak için URL API'den Blok [Taşı](/rest/api/storageservices/put-block-from-url) veya [AzCopy v10'u](../common/storage-use-azcopy-v10.md)kullanın. **URL API'den BlokLa'** yı eşit olarak sunucuda kopyalar. Arama, yalnızca tüm veriler özgün sunucu konumundan hedef konuma taşındıktan sonra tamamlanır.

## <a name="next-steps"></a>Sonraki adımlar

GPv2 ve Blob depolama hesaplarında sıcak, serin ve arşivleme yi değerlendirin.

- [Kabarcık verilerini arşiv katmanından rehydrating hakkında bilgi edinin](storage-blob-rehydration.md)
- [Azure Depolama ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](../common/storage-enable-and-view-metrics.md)
- [Blob depolama ve GPv2 hesaplarında bölgeye göre sık erişimli, seyrek erişimli ve arşiv fiyatlarını denetleme](https://azure.microsoft.com/pricing/details/storage/)
- [Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)
