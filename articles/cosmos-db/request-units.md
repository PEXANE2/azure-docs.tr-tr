---
title: Azure Cosmos DB'de İstek Birimleri ve iş ortası
description: Azure Cosmos DB'de İstek Birimi gereksinimlerini nasıl belirtin ve tahmin edebilirsiniz hakkında bilgi edinin
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.openlocfilehash: a0058bf309e0ff4fbe687731d676e907d1c3fd82
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246597"
---
# <a name="request-units-in-azure-cosmos-db"></a>Azure Cosmos DB'de İstek Birimleri

Azure Cosmos DB ile sağladığınız aktarım hızı ve tükettiğiniz depolama için saat temelinde ödeme yaparsınız. Azure Cosmos veritabanında her zaman yeterli sistem kaynaklarının kullanılabildiğinden emin olmak için aktarım hızının sağlanması gerekir. [Azure Cosmos DB SLa'ları](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)karşılamak veya aşmak için yeterli kaynağa ihtiyacınız vardır.

Azure Cosmos DB, SQL, MongoDB, Cassandra, Gremlin ve Table gibi birçok API'yi destekler. Her API'nin kendi veritabanı işlemleri kümesi vardır. Bu işlemler basit nokta okuma ve yazmadan karmaşık sorgulara kadar değişir. Her veritabanı işlemi, işlemin karmaşıklığına bağlı olarak sistem kaynaklarını tüketir. 

Tüm veritabanı işlemlerinin maliyeti Azure Cosmos DB tarafından normale döndürülur ve *İstek Birimleri* (veya Kısaca RUs) tarafından ifade edilir. Saniye başına RU sayısını, aktarım hızının para birimi olarak düşünebilirsiniz. Saniye başına RU sayısı hızı temel alan bir para birimidir. Azure Cosmos DB tarafından desteklenen veritabanı işlemlerini gerçekleştirmek için gereken CPU, IOPS ve bellek gibi sistem kaynaklarının soyutlamasını sağlar. 

1 KB öğeyi okumanın maliyeti 1 İstek Birimidir (veya 1 RU). Her 1 GB veriyi depolamak için en az 10 RU/s gerekir. Diğer tüm veritabanı işlemlerine RU'lar kullanılarak benzer şekilde maliyet atanır. Azure Cosmos kapsayıcınızla etkileşim kurmak için hangi API'yi kullanırsanız kullanın maliyetler her zaman RU cinsinden ölçülür. Veritabanı işleminin yazma, okuma veya sorgulama olması fark etmez; maliyetler her zaman RU cinsinden ölçülür.

Aşağıdaki resimde üst düzey RU yaklaşımı gösterilir:

![Veritabanı işlemleri İstek Birimlerini tüketir](./media/request-units/request-units.png)

Kapasiteyi yönetmek ve planlamak için, Azure Cosmos DB belirli bir veri kümesi üzerinde belirli bir veritabanı işleminde RU sayısının belirleyici olmasını sağlar. Herhangi bir veritabanı işleminde tüketilen RU sayısını izlemek için yanıt üst bilgisini inceleyebilirsiniz. [RU ücretlerini](request-units.md#request-unit-considerations) ve uygulamanızın iş girdi gereksinimini etkileyen faktörleri anladığınızda, uygulamanızın maliyetini etkin bir şekilde çalıştırabilirsiniz.

Uygulamanız için RU sayısını saniyede 100 RU'luk artışlarla saniye temelinde sağlarsınız. Uygulamanıza sağlanan aktarım hızını ölçeklendirmek için istediğiniz zaman RU sayısını artırabilir veya azaltabilirsiniz. 100 RU'luk artışlar veya düşüşlerle ölçeklendirme yapabilirsiniz. Değişikliklerinizi program aracılığıyla veya Azure portalını kullanarak yapabilirsiniz. Saat temelinde faturalandırılırsınız.

İki ayrı ayrı ayrıayrıda iş elde edebilirsiniz: 

* **Kapsayıcılar**: Daha fazla bilgi için Azure [Cosmos kapsayıcısı üzerinde Sağlama iş ortası](how-to-provision-container-throughput.md)konusuna bakın.
* **Veritabanları**: Daha fazla bilgi için [Azure Cosmos veritabanında Sağlama veri tabanına](how-to-provision-database-throughput.md)bakın.

## <a name="request-unit-considerations"></a>İstek Birimi ile ilgili dikkate alınacak noktalar

Saniyede sağlanacak RU sayısını tahmin ederken aşağıdaki faktörleri dikkate alın:

* **Madde boyutu**: Bir öğenin boyutu arttıkça, öğeyi okumak veya yazmak için tüketilen RUS sayısı da artar.

* **Madde dizini :** Varsayılan olarak, her öğe otomatik olarak dizine eklenir. Kapsayıcıdaki öğelerinizden bazılarını dizine almamayı seçerseniz daha az RU tüketilir.

* **Madde özellik sayısı**: Varsayılan dizin oluşturmanın tüm özelliklerde olduğunu varsayarsak, madde özelliği sayısı arttıkça madde yazmak için tüketilen RUS sayısı artar.

* **Dizinlenmiş özellikler**: Her kapsayıcıdaki dizin ilkesi, varsayılan olarak hangi özelliklerin dizine eksendedildiğini belirler. Yazma işlemlerinin RU tüketimini azaltmak için dizinli özelliklerin sayısını sınırlayın.

* **Veri tutarlılığı**: Güçlü ve sınırlı bayatlık tutarlılık düzeyleri, diğer rahat tutarlılık düzeylerine kıyasla okuma işlemleri gerçekleştirirken yaklaşık iki kat daha fazla RUs tüketir.

* **Sorgu desenleri**: Bir sorgunun karmaşıklığı, bir işlem için kaç RUs tüketilir etkiler. Sorgu işlemlerinin maliyetini etkileyen faktörler şunlardır: 
    
    - Sorgu sonuçlarının sayısı
    - Koşulların sayısı
    - Koşulların yapısı
    - Kullanıcı tanımlı işlevlerin sayısı
    - Kaynak verilerin boyutu
    - Sonuç kümesinin boyutu
    - Yansıtmalar

  Azure Cosmos DB, yinelenen yürütmelerde aynı veriler üzerinde çalıştırılan aynı sorgunun her zaman aynı sayıda RU'ya mal olacağını garanti eder.

* **Komut dosyası kullanımı**: Sorgularda olduğu gibi, depolanan yordamlar ve tetikleyiciler, gerçekleştirilen işlemlerin karmaşıklığına bağlı olarak RUS tüketir. Uygulamanızı geliştirirken, her işlemin ne kadar RU kapasitesi tükettiğini daha iyi anlamak için [istek ücreti üst bilgisini](optimize-cost-queries.md#evaluate-request-unit-charge-for-a-query) inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos kapsayıcıları ve veritabanlarında iş ortası sağlama](set-throughput.md)hakkında daha fazla bilgi edinin.
* [Mantıksal bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
* Sağlanan iş buzun nasıl [ölçeklendirileceä](scaling-throughput.md)i hakkında daha fazla bilgi edinin.
* [Azure Cosmos kapsayıcısına nasıl iş veriş sağlamayı](how-to-provision-container-throughput.md)öğrenin.
* [Azure Cosmos veritabanında iş ortası sağlama](how-to-provision-database-throughput.md)yı öğrenin.
* Bir işlem için istek birimi ücretini nasıl [bulacağınızı](find-request-unit-charge.md)öğrenin.
* [Azure Cosmos DB'de sağlanan iş maliyetinin](optimize-cost-throughput.md)nasıl optimize edilebildiğini öğrenin.
* [Azure Cosmos DB'de okuma ve yazma maliyetini](optimize-cost-reads-writes.md)nasıl optimize edebilirsiniz öğrenin.
* [Azure Cosmos DB'de sorgu maliyetini](optimize-cost-queries.md)nasıl optimize edebilirsiniz öğrenin.
* [İş bilgililiği izlemek için ölçümleri](use-metrics.md)nasıl kullanacağınızı öğrenin.
