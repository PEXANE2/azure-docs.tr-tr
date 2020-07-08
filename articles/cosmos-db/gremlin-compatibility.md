---
title: Inkerpop özellikleriyle Gremlin uyumluluğu Azure Cosmos DB
description: Başvuru belgeleri grafik Altyapısı uyumluluk sorunları
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: sngun
ms.openlocfilehash: 1db7937cb574ce62986f25e0bfa688dc54b5c606
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84700608"
---
# <a name="azure-cosmos-db-gremlin-compatibility"></a>Azure Cosmos DB Gremlin uyumluluğu
Azure Cosmos DB Graph Engine, [Apache TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps) geçiş adımları belirtimini yakından izler, ancak uygulamada Azure Cosmos DB özgü farklılıklar vardır. Desteklenen Gremlin adımlarının listesini öğrenmek için bkz. [GREMLIN API tel protokolü desteği](gremlin-support.md) makalesi.

## <a name="behavior-differences"></a>Davranış farklılıkları

* Azure Cosmos DB Graph Engine, ınkerpop Gremlin 'ın derinliği ilk kez olacak şekilde, ***ilk*** çapraz geçişi çalıştırır. Bu davranış, Cosmos DB benzer şekilde yatay ölçeklenebilir sistemde daha iyi performans elde eder. 

## <a name="unsupported-features"></a>Desteklenmeyen özellikler

* ***[Gremlin Bytecode](https://tinkerpop.apache.org/docs/current/tutorials/gremlin-language-variants/)*** graf dolaşma işlemlerine yönelik programlama dilinden bağımsız bir belirtimdir. Cosmos DB Graph henüz desteklemiyor. `GremlinClient.SubmitAsync()`Geçişi bir metin dizesi olarak kullanın ve geçirin.

* ***`property(set, 'xyz', 1)`*** kardinalite kümesi bugün desteklenmiyor. Bunun yerine `property(list, 'xyz', 1)` kullanın. Daha fazla bilgi için bkz. [ınkerpop Ile köşe özellikleri](http://tinkerpop.apache.org/docs/current/reference/#vertex-properties).

* *** `match()` Adım*** Şu anda kullanılamıyor. Bu adım bildirime dayalı sorgulama özellikleri sağlar.

* Köşelerin veya kenarlardaki ***Özellikler olarak nesneler*** desteklenmez. Özellikler yalnızca temel türler veya diziler olabilir.

* ***Dizi özelliklerine*** `order().by(<array property>)` göre sıralama desteklenmez. Yalnızca temel türlere göre sıralama desteklenir.

* ***Ilkel olmayan JSON türleri*** desteklenmez. `string`, `number` Veya türlerini kullanın `true` / `false` . `null`değerler desteklenmez. 

* ***GraphSONv3*** seri hale getirici Şu anda desteklenmiyor. `GraphSONv2`Bağlantı yapılandırmasındaki serileştirici, okuyucu ve yazıcı sınıflarını kullanın. Azure Cosmos DB Gremlin API tarafından döndürülen sonuçlar GraphSON biçimiyle aynı biçimde değil. 

* **Lambda ifadeleri ve işlevleri** Şu anda desteklenmemektedir. Bu,, `.map{<expression>}` `.by{<expression>}` ve `.filter{<expression>}` işlevlerini içerir. Daha fazla bilgi edinmek ve Gremlin adımlarını kullanarak bunları yeniden yazmayı öğrenmek için [Lambdalar hakkında bir nota](http://tinkerpop.apache.org/docs/current/reference/#a-note-on-lambdas)bakın.

* Sistemin dağıtılmış doğası nedeniyle ***işlemler*** desteklenmiyor.  Gremlin hesabında uygun tutarlılık modelini "kendi yazınızla okumak" üzere yapılandırın ve çakışan yazmaları çözümlemek için iyimser eşzamanlılık kullanın.

## <a name="known-limitations"></a>Bilinen sınırlamalar

* **Orta çapraz geçiş `.V()` adımlarıyla Gremlin sorguları için dizin kullanımı**: Şu anda, `.V()` bir geçiş geçişinin yalnızca ilk çağrısı, bu dizine iliştirilmiş tüm filtre ve koşullara çözüm sağlamak için dizini kullanır. Sonraki çağrılar dizine danışmayacak, bu da sorgunun gecikmesini ve maliyetini artırabilir.
    
    Varsayılan dizin oluşturma varsayılırsa, adımla başlayan tipik bir okunan Gremlin sorgusu, `.V()` `.has()` `.where()` sorgu maliyetini ve performansını iyileştirmek için veya gibi ilgili filtreleme adımlarında parametreleri kullanacaktır. Örneğin:

    ```java
    g.V().has('category', 'A')
    ```

    Ancak, `.V()` Gremlin sorgusuna birden fazla adım eklendiğinde, sorgu verilerinin çözümlenmesi en iyi durumda olmayabilir. Örnek olarak aşağıdaki sorguyu gerçekleştirin:

    ```java
    g.V().has('category', 'A').as('a').V().has('category', 'B').as('b').select('a', 'b')
    ```

    Bu sorgu, adlandırılan özelliklerine göre iki köşe grubu döndürür `category` . Bu durumda, yalnızca ilk çağrı, `g.V().has('category', 'A')` özelliklerinin değerlerine göre köşeleri çözümlemek için dizini kullanır.

    Bu sorguya yönelik bir geçici çözüm, ve gibi subtraversal adımlarını kullanmaktır `.map()` `union()` . Bu, aşağıda belirtilen şekilde belirlenir:

    ```java
    // Query workaround using .map()
    g.V().has('category', 'A').as('a').map(__.V().has('category', 'B')).as('b').select('a','b')

    // Query workaround using .union()
    g.V().has('category', 'A').fold().union(unfold(), __.V().has('category', 'B'))
    ```

    [Gremlin `executionProfile()` Step] (Graf-Execution-profile.MD) kullanarak sorguların performansını gözden geçirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Geri bildirim paylaşmak ve takımınızın sizin için önemli olan özelliklere odaklanmasına yardımcı olmak için [Cosmos DB Kullanıcı sesi](https://feedback.azure.com/forums/263030-azure-cosmos-db) sayfasını ziyaret edin.
