---
title: Azure Cosmos DB'de okuma ve yazma maliyetini optimize etme
description: Bu makalede, veriler üzerinde okuma ve yazma işlemleri gerçekleştirirken Azure Cosmos DB maliyetlerinin nasıl azaltılacak açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: 725876594a7e7c5f3b3a02802f487dc5fdfb64dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535944"
---
# <a name="optimize-reads-and-writes-cost-in-azure-cosmos-db"></a>Azure Cosmos DB'de maliyeti en iyi duruma getirin ve yazar

Bu makalede, Azure Cosmos DB'den veri okumak ve yazmak için gereken maliyetin nasıl hesaplandığı açıklanmaktadır. Okuma işlemleri öğeleri alma işlemleri ve yazma işlemleri öğeleri ekleme, değiştirme, silme ve yükseltme içerir.  

## <a name="cost-of-reads-and-writes"></a>Okuma ve yazma maliyeti

Azure Cosmos DB, sağlanan bir iş ortası modelini kullanarak, iş sonu ve gecikme sonu açısından öngörülebilir performansı garanti eder. Sağlanan iş başı, [istek birimleri](request-units.md) açısından saniyede veya RU/s olarak temsil edilir. İstek Birimi (RU), cpu, bellek, IO, vb. gibi bir isteği gerçekleştirmek için gereken bilgi işlem kaynakları üzerinde mantıksal bir soyutlamadır. Sağlanan iş sonu (RUs) bir kenara bırakılır ve öngörülebilir iş sonu ve gecikme saðlamak için konteynerveya veritabanınıza adanmýr. Sağlanan iş ortası, Azure Cosmos DB'nin öngörülebilir ve tutarlı performans, garantili düşük gecikme sonu ve her ölçekte yüksek kullanılabilirlik sağlamasına olanak tanır. İstek birimleri, bir uygulamanın kaç kaynağa ihtiyacı olduğu yla ilgili mantığı basitleştiren normalleştirilmiş para birimini temsil eder. 

İstek birimlerini okuma ve yazma arasında ayırt etmeyi düşünmenize gerek yoktur. İstek birimlerinin birleşik para birimi modeli, hem okuma hem de yazma için aynı verim kapasitesini birbirinin yerine kullanmak için verimlilik oluşturur. Aşağıdaki tablo, 1 KB ve 100 KB boyutundaki maddeler için RU/s açısından okuma ve yazma maliyetini gösterir.

|**Öğe Boyutu**  |**Bir okuma maliyeti** |**Bir yazma maliyeti**|
|---------|---------|---------|
|1 KB |1 RU |5 RUs |
|100 KB |10 RU |50 Rus |

Boyutu 1 KB olan bir öğeyi okuma bir RU maliyeti. 1-KB olan bir öğeyazma beş RUs maliyeti. Varsayılan oturum [tutarlılık düzeyini](consistency-levels.md)kullanırken okuma ve yazma maliyetleri uygulanabilir.  RUs'lar etrafında göz önünde bulundurulması gereken hususlar şunlardır: madde boyutu, özellik sayısı, veri tutarlılığı, dizine eklenmiş özellikler, dizin oluşturma ve sorgu desenleri.

## <a name="optimize-the-cost-of-writes-and-reads"></a>Yazma ve okuma maliyetini optimize edin

Yazma işlemlerini gerçekleştirirken, saniyede gereken yazma sayısını desteklemek için yeterli kapasite sağlamanız gerekir. SDK, portal, CLI yazılarını gerçekleştirmeden önce kullanarak verilen iş buzun artabilir ve yazmalar tamamlandıktan sonra elde edilen iş buzun azaltılmasını sağlayabilirsiniz. Yazma dönemi için iş kaynağınız, verilen veriler için gereken minimum iş kaynağının yanı sıra başka iş yükü çalışmadığını varsayarak ekle iş yükü için gereken iş yükü dür. 

Diğer iş yüklerini aynı anda çalıştırıyorsanız(örneğin, sorgula/oku/güncelle/sil) bu işlemler için gereken ek istek birimlerini de eklemeniz gerekir. Yazma işlemleri fiyat sınırlıysa, Azure Cosmos DB SDK'ları kullanarak yeniden deneme/geri tepme ilkesini özelleştirebilirsiniz. Örneğin, küçük bir istek oranı oranı sınırlı olana kadar yükü artırabilirsiniz. Oran sınırı oluşursa, istemci uygulaması belirtilen yeniden deneme aralığı için fiyat sınırlayıcı istekleri ni geri almalıdır. Yazmaları yeniden denemeden önce, yeniden denemeler arasında en az miktarda zaman aralığına sahip olmalısınız. Yeniden deneme ilkesi desteği SQL .NET, Java, Node.js ve Python SDK'larına ve .NET Core SDK'larının desteklenen tüm sürümlerine dahildir. 

Ayrıca Azure Cosmos DB'ye veri toplu olarak ekleyebilir veya desteklenen herhangi bir kaynak veri deposundan [Azure Veri Fabrikası'nı](../data-factory/connector-azure-cosmos-db.md)kullanarak verileri Azure Cosmos DB'ye kopyalayabilirsiniz. Azure Veri Fabrikası, veri yazarken en iyi performansı sağlamak için Azure Cosmos DB Toplu API ile yerel olarak tümleşir.

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra aşağıdaki makalelerle Azure Cosmos DB'de maliyet optimizasyonu hakkında daha fazla bilgi edinebilirsiniz:

* [Geliştirme ve test için Optimizasyon](optimize-dev-test.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB faturanızı anlama](understand-your-bill.md) hakkında daha fazla bilgi edinin
* [İş memat maliyetini optimize etme](optimize-cost-throughput.md) hakkında daha fazla bilgi edinin
* [Depolama maliyetini optimize etme](optimize-cost-storage.md) hakkında daha fazla bilgi edinin
* [Sorguların maliyetini optimize etme](optimize-cost-queries.md) hakkında daha fazla bilgi edinin
* [Çok bölgeli Azure Cosmos hesaplarının maliyetini optimize etme](optimize-cost-regions.md) hakkında daha fazla bilgi edinin
