---
title: Azure Cosmos DB 'de istek birimleri ve aktarım hızı
description: Azure Cosmos DB içinde Istek birimi gereksinimlerini belirtme ve tahmin etme hakkında bilgi edinin
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.openlocfilehash: 3378b442c80758c4a5dca02b33f92ba3823f33ed
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85113660"
---
# <a name="request-units-in-azure-cosmos-db"></a>Azure Cosmos DB'de İstek birimleri

Azure Cosmos DB ile sağladığınız aktarım hızı ve tükettiğiniz depolama için saat temelinde ödeme yaparsınız. Azure Cosmos veritabanında her zaman yeterli sistem kaynaklarının kullanılabildiğinden emin olmak için aktarım hızının sağlanması gerekir. [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)'larını karşılamak veya aşmak için yeterli kaynak gereklidir.

Azure Cosmos DB SQL, MongoDB, Cassandra, Gremlin ve Table gibi birçok API 'yi destekler. Her API 'nin kendi veritabanı işlemleri kümesi vardır. Bu işlemler, karmaşık sorgulardan okuma ve yazma işlemleri için basit noktadan aralığıdır. Her veritabanı işlemi, işlemin karmaşıklığına göre sistem kaynaklarını tüketir. 

Tüm veritabanı işlemlerinin maliyeti Azure Cosmos DB normalleştirilerek, *Istek birimleri* (veya ru) tarafından ifade edilir. Saniye başına RU sayısını, aktarım hızının para birimi olarak düşünebilirsiniz. Saniye başına RU sayısı hızı temel alan bir para birimidir. Azure Cosmos DB tarafından desteklenen veritabanı işlemlerini gerçekleştirmek için gereken CPU, IOPS ve bellek gibi sistem kaynaklarının soyutlamasını sağlar. 

1 KB öğeyi okumanın maliyeti 1 İstek Birimidir (veya 1 RU). Her 1 GB veri depolamak için en az 10 RU/sn gereklidir. Diğer tüm veritabanı işlemlerine RU'lar kullanılarak benzer şekilde maliyet atanır. Azure Cosmos kapsayıcınızla etkileşim kurmak için hangi API'yi kullanırsanız kullanın maliyetler her zaman RU cinsinden ölçülür. Veritabanı işleminin yazma, okuma veya sorgulama olması fark etmez; maliyetler her zaman RU cinsinden ölçülür.

Aşağıdaki resimde üst düzey RU yaklaşımı gösterilir:

:::image type="content" source="./media/request-units/request-units.png" alt-text="Veritabanı işlemleri Istek birimlerini tüketir" border="false":::

Kapasiteyi yönetmek ve planlamak için, Azure Cosmos DB belirli bir veri kümesi üzerinde belirli bir veritabanı işleminde RU sayısının belirleyici olmasını sağlar. Herhangi bir veritabanı işleminde tüketilen RU sayısını izlemek için yanıt üst bilgisini inceleyebilirsiniz. RU ücretlerini ve uygulamanızın aktarım hızı gereksinimlerini [etkileyen faktörleri](request-units.md#request-unit-considerations) anladığınızda, uygulama maliyetinizi etkin bir şekilde çalıştırabilirsiniz.

Uygulamanız için RU sayısını saniyede 100 RU'luk artışlarla saniye temelinde sağlarsınız. Uygulamanıza sağlanan aktarım hızını ölçeklendirmek için istediğiniz zaman RU sayısını artırabilir veya azaltabilirsiniz. 100 RU'luk artışlar veya düşüşlerle ölçeklendirme yapabilirsiniz. Değişikliklerinizi program aracılığıyla veya Azure portalını kullanarak yapabilirsiniz. Saat temelinde faturalandırılırsınız.

Aktarım hızını iki ayrı granuya temin edebilirsiniz: 

* **Kapsayıcılar**: daha fazla bilgi için bkz. [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md).
* **Veritabanları**: daha fazla bilgi için bkz. [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md).

## <a name="request-unit-considerations"></a>İstek Birimi ile ilgili dikkate alınacak noktalar

Saniyede sağlanacak RU sayısını tahmin ederken aşağıdaki faktörleri dikkate alın:

* **Öğe boyutu**: bir öğenin boyutu arttıkça, öğeyi okumak veya yazmak Için tüketilen Rus sayısı da artar.

* **Öğe dizin oluşturma**: varsayılan olarak, her öğe otomatik olarak dizinlenir. Kapsayıcıdaki öğelerinizden bazılarını dizine almamayı seçerseniz daha az RU tüketilir.

* **Öğe özelliği sayısı**: varsayılan dizin oluşturmanın tüm özelliklerde olduğu varsayıldığında, öğe yazmak Için tüketilen Rus sayısı, öğe özelliği sayısı arttıkça artar.

* **Dizinli Özellikler**: her kapsayıcıda bir dizin ilkesi, varsayılan olarak hangi özelliklerin dizine alınacağını belirler. Yazma işlemlerinin RU tüketimini azaltmak için dizinli özelliklerin sayısını sınırlayın.

* **Veri tutarlılığı**: güçlü ve sınırlanmış stalet tutarlılık düzeyleri, diğer gevşek tutarlılık seviyelerine kıyasla okuma işlemleri gerçekleştirirken yaklaşık iki kat daha fazla tüketir.

* **Sorgu desenleri**: bir sorgunun karmaşıklığı, bir işlem için kaç ru tüketildiğini etkiler. Sorgu işlemlerinin maliyetini etkileyen faktörler şunlardır: 
    
    - Sorgu sonuçlarının sayısı
    - Koşulların sayısı
    - Koşulların yapısı
    - Kullanıcı tanımlı işlevlerin sayısı
    - Kaynak verilerin boyutu
    - Sonuç kümesinin boyutu
    - Yansıtmalar

  Azure Cosmos DB, yinelenen yürütmelerde aynı veriler üzerinde çalıştırılan aynı sorgunun her zaman aynı sayıda RU'ya mal olacağını garanti eder.

* **Betik kullanımı**: sorgularda olduğu gibi, saklı yordamlar ve Tetikleyiciler, gerçekleştirilen işlemlerin karmaşıklığına göre Rus kullanır. Uygulamanızı geliştirirken, her işlemin ne kadar RU kapasitesi tükettiğini daha iyi anlamak için [istek ücreti üst bilgisini](optimize-cost-queries.md#evaluate-request-unit-charge-for-a-query) inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos kapsayıcılarında ve veritabanlarında işleme sağlama](set-throughput.md)hakkında daha fazla bilgi edinin.
* [Mantıksal bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
* [Sağlanan aktarım hızını genel olarak ölçeklendirme](scaling-throughput.md)hakkında daha fazla bilgi edinin.
* [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md)hakkında bilgi edinin.
* [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md)hakkında bilgi edinin.
* [Bir işlem için istek birimi ücreti bulmayı](find-request-unit-charge.md)öğrenin.
* [Azure Cosmos DB ' de sağlanan üretilen iş maliyetini en uygun hale getirmeyi](optimize-cost-throughput.md)öğrenin.
* [Azure Cosmos DB ' de okuma ve yazma maliyetini en iyileştirmeden](optimize-cost-reads-writes.md)öğrenin.
* [Sorgu maliyetinin Azure Cosmos DB nasıl iyileştirileceği](optimize-cost-queries.md)hakkında bilgi edinin.
* [Verimlilik izlemek için ölçümleri nasıl kullanacağınızı](use-metrics.md)öğrenin.
