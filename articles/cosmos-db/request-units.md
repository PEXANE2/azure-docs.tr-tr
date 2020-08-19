---
title: Azure Cosmos DB bir verimlilik ve performans para birimi olarak istek birimleri
description: Azure Cosmos DB içinde Istek birimi gereksinimlerini belirtme ve tahmin etme hakkında bilgi edinin
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2020
ms.openlocfilehash: 6831cb3f39c25eb69d16300156f456980cf57fa0
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88604812"
---
# <a name="request-units-in-azure-cosmos-db"></a>Azure Cosmos DB'de İstek birimleri

Azure Cosmos DB SQL, MongoDB, Cassandra, Gremlin ve Table gibi birçok API 'yi destekler. Her API 'nin kendi veritabanı işlemleri kümesi vardır. Bu işlemler, karmaşık sorgulardan okuma ve yazma işlemleri için basit noktadan aralığıdır. Her veritabanı işlemi, işlemin karmaşıklığına göre sistem kaynaklarını tüketir.

Tüm veritabanı işlemlerinin maliyeti Azure Cosmos DB normalleştirilerek, *Istek birimleri* (veya ru) tarafından ifade edilir. , Azure Cosmos DB tarafından desteklenen veritabanı işlemlerini gerçekleştirmek için gereken CPU, ıOPS ve bellek gibi sistem kaynaklarını soyutlayan bir performans para birimi olarak RUs 'yi düşünebilirsiniz.

Bir nokta alma maliyeti (yani, KIMLIK ve bölüm anahtarı değeri ile tek bir öğe getirme) 1 KB 'lik öğe için 1 Istek birimi (veya 1 RU) olur. Diğer tüm veritabanı işlemlerine RU'lar kullanılarak benzer şekilde maliyet atanır. Azure Cosmos kapsayıcınızla etkileşim kurmak için hangi API'yi kullanırsanız kullanın maliyetler her zaman RU cinsinden ölçülür. Veritabanı işleminin bir yazma, işaret okuma veya sorgu olup olmadığı, maliyetler her zaman RUs cinsinden ölçülür.

Aşağıdaki resimde üst düzey RU yaklaşımı gösterilir:

:::image type="content" source="./media/request-units/request-units.png" alt-text="Veritabanı işlemleri Istek birimlerini tüketir" border="false":::

Kapasiteyi yönetmek ve planlamak için, Azure Cosmos DB belirli bir veri kümesi üzerinde belirli bir veritabanı işleminde RU sayısının belirleyici olmasını sağlar. Herhangi bir veritabanı işleminde tüketilen RU sayısını izlemek için yanıt üst bilgisini inceleyebilirsiniz. RU ücretlerini ve uygulamanızın aktarım hızı gereksinimlerini [etkileyen faktörleri](request-units.md#request-unit-considerations) anladığınızda, uygulama maliyetinizi etkin bir şekilde çalıştırabilirsiniz.

Kullanmakta olduğunuz Azure Cosmos hesabının türü, kullanılan RUs 'in ücretlendirildiği yöntemi belirler:

- [Sağlanan aktarım hızı](set-throughput.md) modunda, saniye başına 100 ru 'lik artışlarla, uygulamanız için ru sayısını saniye başına temin edersiniz. Uygulamanız için sağlanan aktarım hızını ölçeklendirmek için, her zaman, ru sayısını 100 ru 'lik artışlarla artırabilir veya azaltabilirsiniz. Değişikliklerinizi program aracılığıyla veya Azure portalını kullanarak yapabilirsiniz. Sağladığınız saniye başına ru miktarı için saatlik olarak faturalandırılırsınız. Aktarım hızını iki ayrı granuya temin edebilirsiniz:
  - **Kapsayıcılar**: daha fazla bilgi için bkz. [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md).
  - **Veritabanları**: daha fazla bilgi için bkz. [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md).
- [Sunucusuz](serverless.md) modda, Azure Cosmos hesabınızda kaynak oluştururken herhangi bir aktarım hızı sağlamanız gerekmez. Faturalama döneminizin sonunda, veritabanı işlemleriniz tarafından tüketilen Istek birimi miktarına göre faturalandırılırsınız.

## <a name="request-unit-considerations"></a>İstek Birimi ile ilgili dikkate alınacak noktalar

İş yükünüz tarafından tüketilen RUs sayısını tahmin ederken aşağıdaki faktörleri göz önünde bulundurun:

* **Öğe boyutu**: Öğenin boyutu arttıkça öğeyi okumak veya yazmak için tüketilen RU sayısı da artar.

* **Öğeyi dizine alma**: Varsayılan olarak her öğe otomatik olarak dizine alınır. Kapsayıcıdaki öğelerinizden bazılarını dizine almamayı seçerseniz daha az RU tüketilir.

* **Öğe özelliği sayısı**: Tüm özelliklerde varsayılan olarak dizine alma seçeneğinin açık olduğunu kabul edersek, öğenin özellik sayısı arttıkça öğeyi yazmak için tüketilen RU'ların sayısı da artar.

* **Dizinli özellikler**: Her kapsayıcıda bir dizin kuralı, varsayılan olarak hangi özelliklerin dizinli olduğunu belirler. Yazma işlemlerinin RU tüketimini azaltmak için dizinli özelliklerin sayısını sınırlayın.

* **Veri tutarlılığı**: güçlü ve sınırlanmış stalet tutarlılık düzeyleri, diğer gevşek tutarlılık seviyelerine kıyasla okuma işlemleri gerçekleştirirken yaklaşık iki kat daha fazla tüketir.

* **Okuma türü**: nokta okuma maliyeti, sorgulardan önemli ölçüde daha az.

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
* [Azure Cosmos DB sunucusuz](serverless.md)hakkında daha fazla bilgi edinin.
* [Mantıksal bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
* [Sağlanan aktarım hızını genel olarak ölçeklendirme](scaling-throughput.md)hakkında daha fazla bilgi edinin.
* [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md)hakkında bilgi edinin.
* [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md)hakkında bilgi edinin.
* [Bir işlem için istek birimi ücreti bulmayı](find-request-unit-charge.md)öğrenin.
* [Azure Cosmos DB ' de sağlanan üretilen iş maliyetini en uygun hale getirmeyi](optimize-cost-throughput.md)öğrenin.
* [Azure Cosmos DB ' de okuma ve yazma maliyetini en iyileştirmeden](optimize-cost-reads-writes.md)öğrenin.
* [Sorgu maliyetinin Azure Cosmos DB nasıl iyileştirileceği](optimize-cost-queries.md)hakkında bilgi edinin.
* [Verimlilik izlemek için ölçümleri nasıl kullanacağınızı](use-metrics.md)öğrenin.
