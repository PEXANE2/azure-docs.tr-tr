---
title: Azure Blok Blobu depolama performansı katmanları-Azure depolama
description: Azure Blob depolama için performans katmanları.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 10/02/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: clausjor
ms.openlocfilehash: e0d746f1b01784bc383c12543936f06dae66ca09
ms.sourcegitcommit: f7f70c9bd6c2253860e346245d6e2d8a85e8a91b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73063252"
---
# <a name="azure-block-blob-storage-performance-tiers"></a>Azure Blok Blobu depolama performansı katmanları

Kuruluşlar, performansa duyarlı, bulutta yerel uygulamalar dağıtdığından, farklı performans düzeylerinde uygun maliyetli veri depolama seçeneklerine sahip olmak önemlidir.

Azure Blok Blobu depolama iki farklı performans katmanı sunar:

- Premium: yüksek işlem hızları ve tek basamaklı tutarlı depolama gecikmesi için iyileştirildi
- Standart: yüksek kapasite ve yüksek aktarım hızı için iyileştirilmiş

Farklı performans katmanları için aşağıdaki noktalar geçerlidir:

- Standart performans tüm [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/services/?products=storage)kullanılabilir. Premium performans, [seçme bölgelerinde](https://azure.microsoft.com/global-infrastructure/services/?products=storage)kullanılabilir.
- Premium performans, bu iş yüklerinin [toplam depolama maliyetini düşürmenize](https://azure.microsoft.com/blog/reducing-overall-storage-costs-with-azure-premium-blob-storage/) yardımcı olmak için yüksek işlem ücretleri olan uygulamalar için iyileştirilmiş fiyatlandırma sağlar.
- Blok Blobları için Premium performansı almak üzere BlockBlobStorage hesap türünü kullanmanız gerekir.
- Genel Amaçlı v1, Genel Amaçlı v2 ve BLOB depolama hesapları ile standart performans kullanılabilir.
- Premium ve standart performans, [yüksek verimlilik blok bloblarını](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/)destekler. Yüksek aktarım hızı blok blob 'ları 256 KiB 'den büyük olan Premium performans için kullanılabilir. Yüksek aktarım hızı blok blob 'ları, 4 MIB put bloğundan veya put blob boyutundan büyük bir standart performans için kullanılabilir.
- Premium performans Şu anda yalnızca yerel olarak yedekli depolama (LRS) ile kullanılabilir.

## <a name="premium-performance"></a>Premium performans

Premium performans bloğu blob depolaması, verilerin yüksek performanslı donanımlar aracılığıyla kullanılabilmesini sağlar. Veriler, düşük gecikme süresi için iyileştirilmiş katı hal sürücülerinde (SSD) depolanır. SSD 'Ler geleneksel sabit sürücülerle karşılaştırıldığında daha yüksek aktarım hızı sağlar.

Premium performans bloğu blob depolaması, hızlı ve tutarlı yanıt süreleri gerektiren iş yükleri için idealdir. Çok sayıda küçük işlem gerçekleştiren iş yükleri için idealdir.

## <a name="standard-performance"></a>Standart performans

Standart performans, verileri en düşük maliyetli şekilde depolamak için farklı [erişim katmanlarını](storage-blob-storage-tiers.md) destekler. Büyük veri kümelerinde yüksek kapasite ve yüksek aktarım hızı için en iyi duruma getirilmiştir.

## <a name="blob-lifecycle-management"></a>Blob yaşam döngüsü yönetimi

Blob Storage yaşam döngüsü yönetimi, zengin, kural tabanlı bir ilke sunar:

- Premium-yaşam döngüsünün sonundaki verilerin süresi doluyor
- Standart geçiş verileri en iyi erişim katmanına ve yaşam döngüsünün sonunda verileri sona ermez

Daha fazla bilgi için bkz. [Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md).

Premium Blok Blobu depolama hesabında depolanan veriler, sık erişimli, seyrek erişimli ve arşiv katmanları arasında taşınamaz. Ancak, blob 'ları bir blok BLOB depolama hesabından *farklı* bir hesaptaki sık erişimli erişim katmanına kopyalayabilirsiniz. Verileri farklı bir hesaba kopyalamak için URL API 'si veya [AzCopy Ile v10 arasındaki](../common/storage-use-azcopy-v10.md) [öğesinden put bloğunu](/rest/api/storageservices/put-block-from-url) kullanın. **URL API 'Den put bloğu** , sunucudaki verileri eşzamanlı olarak kopyalar. Çağrı yalnızca tüm veriler özgün sunucu konumundan hedef konuma taşındıktan sonra tamamlanır.

## <a name="next-steps"></a>Sonraki adımlar

GPv2 ve BLOB depolama hesaplarında sık erişimli, seyrek erişimli ve arşivi değerlendirin

- [Bölgeye göre sık, seyrek ve arşiv kullanılabilirliğini denetleme](https://azure.microsoft.com/regions/#services)
- [Azure Blob depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)
- [Blob verilerini arşiv katmanından yeniden doldurma hakkında bilgi edinin](storage-blob-rehydration.md)
- [Azure Depolama ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](../common/storage-enable-and-view-metrics.md)
- [Blob depolama ve GPv2 hesaplarında bölgeye göre sık erişimli, seyrek erişimli ve arşiv fiyatlarını denetleme](https://azure.microsoft.com/pricing/details/storage/)
- [Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)
