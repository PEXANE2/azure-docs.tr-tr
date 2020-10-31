---
title: Azure Cosmos DB 'de çok bölgeli dağıtımlar için maliyeti iyileştirin
description: Bu makalede Azure Cosmos DB içinde çok bölgeli dağıtımların maliyetlerinin nasıl yönetileceği açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2020
ms.openlocfilehash: 010ca40f4f3aacd6353aecd150e944672cc09066
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93097524"
---
# <a name="optimize-multi-region-cost-in-azure-cosmos-db"></a>Azure Cosmos DB'de birden çok bölgenin maliyetini iyileştirme
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos hesabınıza istediğiniz zaman bölge ekleyebilir veya kaldırabilirsiniz. Çeşitli Azure Cosmos veritabanları ve kapsayıcıları için yapılandırdığınız aktarım hızı, hesabınızla ilişkili her bölgede ayrılır. Saat başına sağlanan aktarım hızı, Azure Cosmos hesabınız için tüm veritabanları ve kapsayıcılar genelinde yapılandırılan RU/sn 'nin toplamı `T` ve veritabanı hesabınızla Ilişkili Azure bölgelerinin sayısı ise, `N` belirli bir saat Için Cosmos hesabınız için sağlanan toplam üretilen iş hacmi değerine eşittir `T x N RU/s` .

Tek yazma bölgesinde sağlanan aktarım hızı saniyede 100 RU ile saat başına 0,008 ABD doları, birden fazla yazılabilir bölgede sağlanan aktarım hızı saniyede 100 RU ile saat başına 0,016 ABD doları ile ücretlendirilir. Daha fazla bilgi için bkz. Azure Cosmos DB [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/).

## <a name="costs-for-multiple-write-regions"></a>Birden çok yazma bölgesinin maliyeti

Çok bölgeli bir yazma sisteminde, yazma işlemleri için net kullanılabilir RUs, `N` `N` yazma bölgesi sayısı olan zamanları artırır. Tek bölge yazmalarından farklı olarak, her bölge artık yazılabilir ve çakışma çözümünü destekler. Maliyet Planlama noktasından, `M` dünyanın her yerindeki yazma işlemini gerçekleştirmek için `RUs` bir kapsayıcıya veya veritabanı düzeyine M sağlamanız gerekir. Daha sonra istediğiniz kadar bölge ekleyebilir ve bunları dünya genelindeki yazma işlemleri için yazma işlemleri için kullanabilirsiniz `M` .

### <a name="example"></a>Örnek

Batı ABD, tek bölgeli yazma işlemleri için yapılandırılmış bir kapsayıcınızı kabul edin, bu arada üretilen iş 10.000 RU/sn ile sağlanır ve bu ay 1 TB veri depolar. Aynı depolama ve aktarım hızı ile Doğu ABD bir bölge ekleyin ve uygulamanızdan her iki bölgedeki kapsayıcılara yazmak istediğinizi varsayalım. Aylık toplam faturanız (31 gün varsayılarak), şu şekildedir:

|**Öğe**|**Kullanım (aylık)**|**Hız**|**Aylık maliyet**|
|----|----|----|----|
|Batı ABD (tek yazma bölgelerinde) kapsayıcı için üretilen iş faturası |10.000 RU/s * 24 saat * 31 gün |$0,008/saat başına 100 RU/sn |$584,06 |
|2 bölgede kapsayıcı için üretilen iş faturası-Batı ABD & Doğu ABD (birden fazla yazma bölgesi) |2 * 10.000 RU/s * 24 saat * 31 gün|$0,016/saat başına 100 RU/sn |$2.336,26 |
|Batı ABD kapsayıcı için depolama faturası |1 TB (veya 1.024 GB) |$0,25/GB |$256 |
|2 bölge için depolama faturası-Batı ABD & Doğu ABD |2 * 1 TB (veya 3.072 GB) |$0,25/GB |$768 |

## <a name="improve-throughput-utilization-on-a-per-region-basis"></a>Bölge bazında üretilen iş kullanımını geliştirme

Örneğin, bir veya daha fazla kullanılan okuma bölgesi olan bir veya daha fazla kullanım alanı için bir veya daha fazla okuma bölgesi kullanıyorsanız, oku-Region ' ı kullanarak bir veya daha fazla okuma bölgesinde değişiklik akışını kullanarak en fazla İlk olarak yazma bölgesinde sağlanan üretilen işi (ru) iyileştirdiğinizden emin olmanız gerekir. Çok büyük sorgular bile, hatta kullanımı zorda zor olabilir. Genel olarak, Bölgelerinizdeki tüketilen üretilen işi izleyin ve okuma ve yazma aktarım hızını ölçeklendirmek için isteğe bağlı bölge ekleyin veya kaldırın. böylece, aynı bölgede dağıtılan tüm uygulamalar için gecikme süresini anladığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra, aşağıdaki makalelerle Azure Cosmos DB maliyet iyileştirmesi hakkında daha fazla bilgi edinebilirsiniz:

* [Geliştirme ve test Için iyileştirme](optimize-dev-test.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB Faturanızı Anlama](understand-your-bill.md) hakkında daha fazla bilgi edinin
* [Verimlilik maliyetini iyileştirme](optimize-cost-throughput.md) hakkında daha fazla bilgi edinin
* [Depolama maliyetini iyileştirme](optimize-cost-storage.md) hakkında daha fazla bilgi edinin
* [Okuma ve yazma maliyetlerini iyileştirme](optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin
* [Sorguların maliyetini En Iyi duruma getirme](./optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin