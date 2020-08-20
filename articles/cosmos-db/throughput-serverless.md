---
title: Azure Cosmos DB sağlanan aktarım hızı ve sunucusuz arasında seçim yapma
description: İş yükünüz için sağlanan aktarım hızı ve sunucusuz arasından seçim yapma hakkında bilgi edinin.
author: ThomasWeiss
ms.author: thweiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2020
ms.openlocfilehash: 0adb346a693beaa905438cfdc1249c1646c28811
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608866"
---
# <a name="how-to-choose-between-provisioned-throughput-and-serverless"></a>Sağlanan aktarım hızı ve sunucusuz arasından seçim yapma

Azure Cosmos DB iki farklı kapasite modunda kullanılabilir: [sağlanan aktarım hızı](set-throughput.md) ve [sunucusuz](serverless.md). Her iki modda da tam olarak aynı veritabanı işlemlerini gerçekleştirebilirsiniz, ancak bu işlemler için faturalandırıldığınız şekilde tamamen farklıdır. Aşağıdaki videoda, bu modlar arasındaki temel farklılıklar ve farklı iş yükleri türlerine nasıl uydukları açıklanmaktadır:

> [!VIDEO https://www.youtube.com/embed/CgYQo6uHyt0]

> [!NOTE]
> Sunucusuz Şu anda yalnızca Azure Cosmos DB Çekirdek (SQL) API 'sinde destekleniyor.

## <a name="detailed-comparison"></a>Ayrıntılı karşılaştırma

| Ölçütler | Sağlanan aktarım hızı | Sunucusuz |
| --- | --- | --- |
| Durum | Genel kullanıma sunuldu | Önizlemede |
| En uygun | Tahmin edilebilir performans gerektiren görev açısından kritik iş yükleri | Hafif trafik içeren küçük ve orta düzeyde kritik olmayan iş yükleri |
| Nasıl çalışır? | Kapsayıcılarınızın her biri için, saniye başına [Istek birimi](request-units.md) cinsinden ifade edilen bazı üretilen iş miktarı sağlayabilirsiniz. Her saniye, veritabanı işlemleriniz için bu Istek birimi miktarı kullanılabilir. Sağlanan üretilen iş, el ile veya otomatik [ölçeklendirme](provision-throughput-autoscale.md)ile ayarlanabilir. | Veritabanı işlemlerinizi herhangi bir kapasite sağlamak zorunda kalmadan kapsayıcılarınıza karşı çalıştırırsınız. |
| Hesap başına sınırlamalar | Maksimum Azure bölgesi sayısı: sınırsız | Maksimum Azure bölgesi sayısı: 1 |
| Kapsayıcı başına sınırlamalar | En yüksek aktarım hızı: sınırsız<br>Maksimum depolama alanı: sınırsız | En yüksek aktarım hızı: 5.000 RU/sn<br>Maksimum depolama alanı: 50 GB |
| Performans | SLA kapsamındaki% 99,99 ile% 99,999 kullanılabilirlik<br>SLA tarafından kapsanan nokta okuma ve yazma işlemleri için 10 ms gecikme <<br>SLA kapsamındaki% 99,99 garanti edilen aktarım hızı | SLA kapsamındaki% 99,9 ile% 99,99 kullanılabilirlik<br><, SLO tarafından kapsanan yazma işlemleri için nokta okuma ve < 30 MS için 10 ms gecikme süresi<br>SLO tarafından kapsanan %95 burkararlılığı |
| Faturalama modeli | Faturalandırma, ne kadar RUs tüketildiğine bakılmaksızın, sağlanan RU/s için saat başına yapılır. | Faturalama, veritabanı işlemlerinizin kullandığı RUs miktarı için saatlik olarak gerçekleştirilir. |

> [!IMPORTANT]
> Sunucusuz kısıtlamaların bazıları sunucusuz genel kullanıma sunulduğunda kaldırılabilir veya kaldırılabilir. **your feedback** Bize ulaşın ve sunucusuz deneyiminiz hakkında daha fazla bilgi verin: [azurecosmosdbserverless@service.microsoft.com](mailto:azurecosmosdbserverless@service.microsoft.com) .

## <a name="burstability-and-expected-consumption"></a>Burkararlılığı ve beklenen tüketim

Bazı durumlarda, belirli bir iş yükü için sağlanan aktarım hızı veya sunucusuz seçilmelidir. Bu kararya yardımcı olması için şunları tahmin edebilirsiniz:

- İş yükünüz, bir saniye içinde kullanmanız gerekebilecek en fazla RUs miktarı olan **buristikülebilirlik** gereksinimidir
- Genel olarak **beklenen**, bir ayda kullanabileceğiniz Toplam ru sayısı (Bu, [burada](plan-manage-costs.md#estimating-serverless-costs)gösterilen tablonun yardımıyla tahmin edebilirsiniz)

İş yükünüz, saniyede 5.000 RU 'dan patlama gerektiriyorsa, sunucusuz kapsayıcılar bu sınırın üzerinde veri bloğu yaptığından sağlanan aktarım hızı seçilmelidir. Aksi takdirde, beklenen tüketiminiz temelinde her iki modun maliyetini karşılaştırabilirsiniz.

**Örnek 1**: bir iş yükünün en fazla 10.000 ru/sn 'ye patlaması ve bir ayda toplam 20.000.000 ru kullanması beklenir.

- Yalnızca sağlanan aktarım hızı modu, 10.000 RU/sn aktarım hızını sunabilir

**Örnek 2**: bir iş yükünün en fazla 500 ru/sn 'ye patlaması ve bir ayda toplam 20.000.000 ru kullanması beklenir.

- Sağlanan aktarım hızı modunda, aylık 500 RU/sn ile bir kapsayıcı sağlayacaksınız: $0,008 * 5 * 730 = **$29,20**
- Sunucusuz modda tüketilen RUs için ödeme yaparsınız: $0,25 * 20 = **$5,00**

**Örnek 3**: iş yükünün en fazla 500 ru/sn 'ye patlaması ve bir ayda toplam 250.000.000 ru kullanması beklenir.

- Sağlanan aktarım hızı modunda, aylık 500 RU/sn ile bir kapsayıcı sağlayacaksınız: $0,008 * 5 * 730 = **$29,20**
- Sunucusuz modda tüketilen ru: $0,25 * 250 = **$62,50** için ödeme yaparsınız.

(Bu örnekler, iki mod ile aynı olan depolama maliyeti için hesap değildir)

> [!NOTE]
> Önceki örnekte gösterilen maliyetler yalnızca tanıtım amaçlıdır. En son fiyatlandırma bilgileri için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB üretilen iş sağlama](set-throughput.md) hakkında daha fazla bilgi edinin
- [Azure Cosmos DB sunucusuz](serverless.md) hakkında daha fazla bilgi edinin
- [Istek birimi](request-units.md) kavramıyla ilgili bilgi edinin
