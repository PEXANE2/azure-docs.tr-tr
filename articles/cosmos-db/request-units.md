---
title: Azure Cosmos DB 'de istek birimleri ve aktarım hızı
description: Azure Cosmos DB içinde Istek birimi gereksinimlerini belirtme ve tahmin etme hakkında bilgi edinin
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/24/2020
ms.openlocfilehash: f1f203d17de9fb0fc9fe8bb0f6de80fe2b93ba8b
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87327812"
---
# <a name="request-units-in-azure-cosmos-db"></a>Azure Cosmos DB'de İstek birimleri

Azure Cosmos DB ile sağladığınız aktarım hızı ve tükettiğiniz depolama için saat temelinde ödeme yaparsınız. Azure Cosmos veritabanında her zaman yeterli sistem kaynaklarının kullanılabildiğinden emin olmak için aktarım hızının sağlanması gerekir. [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)'larını karşılamak veya aşmak için yeterli kaynak gereklidir.

Azure Cosmos DB SQL, MongoDB, Cassandra, Gremlin ve Table gibi birçok API 'yi destekler. Her API 'nin kendi veritabanı işlemleri kümesi vardır. Bu işlemler, karmaşık sorgulardan okuma ve yazma işlemleri için basit noktadan aralığıdır. Her veritabanı işlemi, işlemin karmaşıklığına göre sistem kaynaklarını tüketir.

Tüm veritabanı işlemlerinin maliyeti Azure Cosmos DB normalleştirilerek, *Istek birimleri* (veya ru) tarafından ifade edilir. Saniye başına RU sayısını, aktarım hızının para birimi olarak düşünebilirsiniz. Saniye başına RU sayısı hızı temel alan bir para birimidir. Azure Cosmos DB tarafından desteklenen veritabanı işlemlerini gerçekleştirmek için gereken CPU, IOPS ve bellek gibi sistem kaynaklarının soyutlamasını sağlar. Her 1 GB veri depolamak için en az 10 RU/sn gereklidir.

1 KB 'lik bir öğe için okuma noktası yapılacak ücret 1 Istek birimidir (veya 1 RU). Diğer tüm veritabanı işlemlerine RU'lar kullanılarak benzer şekilde maliyet atanır. Azure Cosmos kapsayıcınızla etkileşim kurmak için hangi API'yi kullanırsanız kullanın maliyetler her zaman RU cinsinden ölçülür. Veritabanı işleminin bir yazma, işaret okuma veya sorgu olup olmadığı, maliyetler her zaman RUs cinsinden ölçülür.

Aşağıdaki resimde üst düzey RU yaklaşımı gösterilir:

:::image type="content" source="./media/request-units/request-units.png" alt-text="Veritabanı işlemleri Istek birimlerini tüketir" border="false":::

Kapasiteyi yönetmek ve planlamak için, Azure Cosmos DB belirli bir veri kümesi üzerinde belirli bir veritabanı işleminde RU sayısının belirleyici olmasını sağlar. Herhangi bir veritabanı işleminde tüketilen RU sayısını izlemek için yanıt üst bilgisini inceleyebilirsiniz. RU ücretlerini ve uygulamanızın aktarım hızı gereksinimlerini [etkileyen faktörleri](request-units.md#request-unit-considerations) anladığınızda, uygulama maliyetinizi etkin bir şekilde çalıştırabilirsiniz.

Uygulamanız için RU sayısını saniyede 100 RU'luk artışlarla saniye temelinde sağlarsınız. Uygulamanıza sağlanan aktarım hızını ölçeklendirmek için istediğiniz zaman RU sayısını artırabilir veya azaltabilirsiniz. 100 RU'luk artışlar veya düşüşlerle ölçeklendirme yapabilirsiniz. Değişikliklerinizi program aracılığıyla veya Azure portalını kullanarak yapabilirsiniz. Saat temelinde faturalandırılırsınız.

Aktarım hızını iki ayrı granuya temin edebilirsiniz:

* **Kapsayıcılar**: daha fazla bilgi için bkz. [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md).
* **Veritabanları**: daha fazla bilgi için bkz. [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md).

## <a name="request-unit-considerations"></a>İstek Birimi ile ilgili dikkate alınacak noktalar

Saniyede sağlanacak RU sayısını tahmin ederken aşağıdaki faktörleri dikkate alın:

* **Öğe boyutu**: Öğenin boyutu arttıkça öğeyi okumak veya yazmak için tüketilen RU sayısı da artar.

* **Öğeyi dizine alma**: Varsayılan olarak her öğe otomatik olarak dizine alınır. Kapsayıcıdaki öğelerinizden bazılarını dizine almamayı seçerseniz daha az RU tüketilir.

* **Öğe özelliği sayısı**: Tüm özelliklerde varsayılan olarak dizine alma seçeneğinin açık olduğunu kabul edersek, öğenin özellik sayısı arttıkça öğeyi yazmak için tüketilen RU'ların sayısı da artar.

* **Dizinli özellikler**: Her kapsayıcıda bir dizin kuralı, varsayılan olarak hangi özelliklerin dizinli olduğunu belirler. Yazma işlemlerinin RU tüketimini azaltmak için dizinli özelliklerin sayısını sınırlayın.

* **Veri tutarlılığı**: güçlü ve sınırlanmış stalet tutarlılık düzeyleri, diğer gevşek tutarlılık seviyelerine kıyasla okuma işlemleri gerçekleştirirken yaklaşık iki kat daha fazla tüketir.

* **Okuma türü**: nokta okuma maliyeti, sorgulardan önemli ölçüde daha az ru.

* **Sorgu kalıpları**: Sorgunun karmaşıklığı, bir işlem için tüketilen RU sayısını etkiler. Sorgu işlemlerinin maliyetini etkileyen faktörler şunlardır: 
    
    - Sorgu sonuçlarının sayısı
    - Koşulların sayısı
    - Koşulların yapısı
    - Kullanıcı tanımlı işlevlerin sayısı
    - Kaynak verilerin boyutu
    - Sonuç kümesinin boyutu
    - Projeksiyonlar

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
