---
title: Azure Cosmos DB 'de çok bölgeli dağıtımlar için maliyeti iyileştirin
description: Bu makalede Azure Cosmos DB içinde çok bölgeli dağıtımların maliyetlerinin nasıl yönetileceği açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/31/2019
ms.openlocfilehash: e0a24b52c12bce6a8e016a926dfa64a1e36a7cc6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "72753313"
---
# <a name="optimize-multi-region-cost-in-azure-cosmos-db"></a>Azure Cosmos DB'de birden çok bölgenin maliyetini iyileştirme

Azure Cosmos hesabınıza istediğiniz zaman bölge ekleyebilir veya kaldırabilirsiniz. Çeşitli Azure Cosmos veritabanları ve kapsayıcıları için yapılandırdığınız aktarım hızı, hesabınızla ilişkili her bölgede ayrılır. Saat başına sağlanan aktarım hızı, Azure Cosmos hesabınız için tüm veritabanları ve kapsayıcılar genelinde yapılandırılan RU/sn 'nin toplamı `T` ve veritabanı hesabınızla Ilişkili Azure bölgelerinin sayısı ise, `N` belirli bir saat Için Cosmos hesabınız için sağlanan toplam üretilen iş hacmi şuna eşittir:

1. `T x N RU/s`Azure Cosmos hesabınız tek bir yazma bölgesiyle yapılandırıldıysa. 

1. `T x (N+1) RU/s`Azure Cosmos hesabınız, yazma işlemleri yapabilen tüm bölgelerle yapılandırılmışsa. 

Tek yazma bölgesinde sağlanan aktarım hızı saniyede 100 RU ile saat başına 0,008 ABD doları, birden fazla yazılabilir bölgede sağlanan aktarım hızı saniyede 100 RU ile saat başına 0,016 ABD doları ile ücretlendirilir. Daha fazla bilgi için bkz. Azure Cosmos DB [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/).

## <a name="costs-for-multiple-write-regions"></a>Birden çok yazma bölgesinin maliyeti

Çoklu ana sistemde, yazma işlemleri için net kullanılabilir ru, `N` `N` yazma bölgesi sayısı olan zamanları artırır. Tek bölge yazmalarından farklı olarak, her bölge artık yazılabilir ve çakışma çözümünü desteklemelidir. Yazarlar için iş yükü miktarı artmıştır. Maliyet Planlama noktasından, `M` dünyanın her yerindeki yazma işlemini gerçekleştirmek için `RUs` bir kapsayıcıya veya veritabanı düzeyine M sağlamanız gerekir. Daha sonra istediğiniz kadar bölge ekleyebilir ve bunları dünya genelindeki yazma işlemleri için yazma işlemleri için kullanabilirsiniz `M` . 

### <a name="example"></a>Örnek

Batı ABD, aktarım hızı 10.000 RU/sn ile sağlanan ve bu ay 1 TB veri depolayan bir kapsayıcınızı düşünün. Her biri aynı depolama ve aktarım hızı ile Doğu ABD, Kuzey Avrupa ve Doğu Asya üç bölge eklediğinizi ve küresel olarak dağıtılan uygulamanızdan dört bölgedeki kapsayıcılara yazmak istediğinizi varsayalım. Aylık toplam faturanız (31 gün varsayılarak), şu şekildedir:

|**Öğe**|**Kullanım (aylık)**|**Fiyat**|**Aylık maliyet**|
|----|----|----|----|
|Batı ABD kapsayıcı için üretilen iş faturası (birden fazla yazma bölgesi) |10.000 RU/s * 24 * 31 |$0,016/saat başına 100 RU/sn |$1.190,40 |
|3 ek bölge için üretilen iş faturası Doğu ABD, Kuzey Avrupa ve Doğu Asya (birden fazla yazma bölgesi) |(3 + 1) * 10.000 RU/s * 24 * 31 |$0,016/saat başına 100 RU/sn |$4.761,60 |
|Batı ABD kapsayıcı için depolama faturası |1 TB (veya 1.024 GB) |$0,25/GB |$256 |
|3 ek bölge için depolama faturası Doğu ABD, Kuzey Avrupa ve Doğu Asya |3 * 1 TB (veya 3.072 GB) |$0,25/GB |$768 |
|**Toplam**|||**$6.976** |

## <a name="improve-throughput-utilization-on-a-per-region-basis"></a>Bölge bazında üretilen iş kullanımını geliştirme

Kullanım dışı bir veya daha fazla veya daha fazla bölge (örneğin, bir veya daha fazla), verimlilik kullanımını geliştirmek için aşağıdaki adımları uygulayabilirsiniz:  

1. İlk olarak yazma bölgesinde sağlanan üretilen işi (ru) iyileştirdiğinizden emin olun ve ardından oku-Bölgesi ' nden değişiklik akışını kullanarak RUs 'in okuma bölgelerinde en fazla kullanımını yapın. 

2. Birden çok yazma bölgesi okuma ve yazma işlemleri, Azure Cosmos hesabıyla ilişkili tüm bölgelerde ölçeklendirilebilir. 

3. Bölgelerinizdeki etkinliği izleyin ve okuma ve yazma aktarım hızını ölçeklendirmek için isteğe bağlı olarak bölge ekleyebilir ve kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra, aşağıdaki makalelerle Azure Cosmos DB maliyet iyileştirmesi hakkında daha fazla bilgi edinebilirsiniz:

* [Geliştirme ve test Için iyileştirme](optimize-dev-test.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB Faturanızı Anlama](understand-your-bill.md) hakkında daha fazla bilgi edinin
* [Verimlilik maliyetini iyileştirme](optimize-cost-throughput.md) hakkında daha fazla bilgi edinin
* [Depolama maliyetini iyileştirme](optimize-cost-storage.md) hakkında daha fazla bilgi edinin
* [Okuma ve yazma maliyetlerini iyileştirme](optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin
* [Sorguların maliyetini En Iyi duruma getirme](optimize-cost-queries.md) hakkında daha fazla bilgi edinin

