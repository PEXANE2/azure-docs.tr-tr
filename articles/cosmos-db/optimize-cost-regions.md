---
title: Azure Cosmos DB'deki çok bölgeli dağıtımlar için maliyeti optimize edin
description: Bu makalede, Azure Cosmos DB'deki çok bölgeli dağıtımmaliyetlerinin nasıl yönetilen olduğu açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/31/2019
ms.openlocfilehash: e0a24b52c12bce6a8e016a926dfa64a1e36a7cc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72753313"
---
# <a name="optimize-multi-region-cost-in-azure-cosmos-db"></a>Azure Cosmos DB'de birden çok bölgenin maliyetini iyileştirme

Azure Cosmos hesabınıza istediğiniz zaman bölgeler ekleyebilir ve kaldırabilirsiniz. Çeşitli Azure Cosmos veritabanları ve kapsayıcılar için yapılandırdığınız iş, hesabınızla ilişkili her bölgede ayrılmıştır. Saatte sağlanan iş ortası, Azure Cosmos hesabınız için tüm veritabanları ve kapsayıcılar arasında yapılandırılan `T` RU/s toplamı ise ve veritabanı `N`hesabınızla ilişkili Azure bölgelerinin sayısı ise , belirli bir saat için Cosmos hesabınız için sağlanan toplam iş ortası eşittir:

1. `T x N RU/s`Azure Cosmos hesabınız tek bir yazma bölgesiyle yapılandırılırsa. 

1. `T x (N+1) RU/s`Azure Cosmos hesabınız, yazma işlemlerini işleyebilen tüm bölgelerle yapılandırılırsa. 

Tek yazma bölgesinde sağlanan aktarım hızı saniyede 100 RU ile saat başına 0,008 ABD doları, birden fazla yazılabilir bölgede sağlanan aktarım hızı saniyede 100 RU ile saat başına 0,016 ABD doları ile ücretlendirilir. Daha fazla bilgi için Azure Cosmos DB [Fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/)bakın.

## <a name="costs-for-multiple-write-regions"></a>Birden çok yazma bölgesi için maliyetler

Çok analı sistemde, yazma işlemleri için kullanılabilir `N` net `N` RUs yazma bölgelerinin sayısı nın olduğu süreleri artırır. Tek bir bölge yazmanın aksine, her bölge artık yazılabilir ve çakışma çözümünü desteklemelidir. Yazarlar için iş yükü miktarı artmıştır. Maliyet planlama açısından, dünya çapında `M` ru /s yazma ları gerçekleştirmek için, `RUs` bir konteyner veya veritabanı düzeyinde M sağlamanız gerekir. Daha sonra istediğiniz kadar bölge ekleyebilir ve bunları dünya `M` çapında ki RU değer yazılarını gerçekleştirmek için yazmak için kullanabilirsiniz. 

### <a name="example"></a>Örnek

Batı ABD'de 10K RU/s ile sağlanan bir konteyner var düşünün ve bu ay veri 1 TB depolar. Doğu ABD, Kuzey Avrupa ve Doğu Asya olmak üzere üç bölge eklediğinizi varsayalım, her biri aynı depolama ve üretim emeğe sahip ve küresel olarak dağıtılan uygulamanızdan dört bölgede de kapsayıcılara yazma olanağı istiyorsunuz. Bir ay daki toplam aylık faturanız (31 gün varsayarak) aşağıdaki gibidir:

|**Öğe**|**Kullanım (aylık)**|**Fiyat**|**Aylık Maliyet**|
|----|----|----|----|
|Batı ABD'de konteyner için iş mesuliyet faturası (birden çok yazma bölgesi) |10K RU/s * 24 * 31 |$0.016 /saat başına 100 RU/s |1.190,40 $ |
|Doğu ABD, Kuzey Avrupa ve Doğu Asya (birden fazla yazma bölgesi) |(3 + 1) * 10K RU/s * 24 * 31 |$0.016 /saat başına 100 RU/s |4.761,60 TL |
|Batı ABD'de konteyner için depolama faturası |1 TB (veya 1.024 GB) |$0.25/TR |256 DOLAR |
|Doğu ABD, Kuzey Avrupa ve Doğu Asya - 3 ek bölge için depolama faturası |3 * 1 TB (veya 3.072 GB) |$0.25/TR |768 DOLAR |
|**Toplam**|||**6.976 dolar** |

## <a name="improve-throughput-utilization-on-a-per-region-basis"></a>Bölge bazında iş ortası kullanımını geliştirin

Örneğin, az kullanılan veya aşırı kullanılan bölgeler gibi verimsiz kullanımınız varsa, iş ortası kullanımını iyileştirmek için aşağıdaki adımları atabilirsiniz:  

1. Önce yazma bölgesinde sağlanan iş akışını (RUS) optimize ettiğinizden ve daha sonra okuma bölgesinden gelen değişiklik akışını vb. kullanarak okuma bölgelerindeKI RUs'lardan maksimum şekilde kullandığınızdan emin olun. 

2. Birden çok yazma bölgesi okunur ve yazılabilir, Azure Cosmos hesabıyla ilişkili tüm bölgeler arasında ölçeklenebilir. 

3. Bölgelerinizdeki etkinliği izleyin ve okuma ve yazma işlerinizi ölçeklendirmek için talep üzerine bölgeler ekleyip kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra aşağıdaki makalelerle Azure Cosmos DB'de maliyet optimizasyonu hakkında daha fazla bilgi edinebilirsiniz:

* [Geliştirme ve test için Optimizasyon](optimize-dev-test.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB faturanızı anlama](understand-your-bill.md) hakkında daha fazla bilgi edinin
* [İş memat maliyetini optimize etme](optimize-cost-throughput.md) hakkında daha fazla bilgi edinin
* [Depolama maliyetini optimize etme](optimize-cost-storage.md) hakkında daha fazla bilgi edinin
* [Okuma ve yazma maliyetini optimize etme](optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin
* [Sorguların maliyetini optimize etme](optimize-cost-queries.md) hakkında daha fazla bilgi edinin

