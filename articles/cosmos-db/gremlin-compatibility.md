---
title: TinkerPop özellikleriyle Azure Cosmos DB Gremlin uyumluluğu
description: Referans belgeleri Grafik motoru uyumluluk sorunları
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: sngun
ms.openlocfilehash: 989a033a843b861c34dc9dbdbced50399f8e5cd7
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81449893"
---
# <a name="azure-cosmos-db-gremlin-compatibility"></a>Azure Cosmos DB Gremlin uyumluluğu
Azure Cosmos DB Graph [motoru, Apache TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps) geçiş adımlarını yakından takip eder, ancak farklılıklar vardır.

## <a name="behavior-differences"></a>Davranış farklılıkları

* Azure Cosmos DB Graph motoru ***geniş-ilk*** geçiş çalışırken TinkerPop Gremlin önce dir. Bu davranış, Cosmos DB gibi yatay ölçeklenebilir sistemde daha iyi performans sağlar. 

## <a name="unsupported-features"></a>Desteklenmeyen özellikler

* ***[Gremlin Bytecode](https://tinkerpop.apache.org/docs/current/tutorials/gremlin-language-variants/)*** graf dolaşma işlemlerine yönelik programlama dilinden bağımsız bir belirtimdir. Cosmos DB Graph henüz desteklemiyor. Geçişi metin dizesi olarak kullanın `GremlinClient.SubmitAsync()` ve geçirin.

* ***`property(set, 'xyz', 1)`*** set kardinallik bugün desteklenmez. Bunun yerine `property(list, 'xyz', 1)` kullanın. Daha fazla bilgi için [TinkerPop ile Vertex özelliklerine](http://tinkerpop.apache.org/docs/current/reference/#vertex-properties)bakın.

* ***`atch()`*** açıklayıcı desen eşleştirme kullanarak grafik sorgulama sağlar. Bu özellik kullanılamıyor.

* Tepe veya kenarlarındaki ***özellikteki nesneler*** desteklenmez. Özellikler yalnızca temel türler veya diziler olabilir.

* ***Dizi özelliklerine*** `order().by(<array property>)` göre sıralama desteklenmez. Yalnızca temel türlere göre sıralama desteklenir.

* ***İlkel olmayan JSON türleri*** desteklenmez. Kullanın `string` `number`, `true` / `false` veya türleri. `null`değerler desteklenmez. 

* ***GraphSONv3*** serializer şu anda desteklenmiyor. Bağlantı `GraphSONv2` yapılandırmasında Serializer, Reader ve Writer sınıflarını kullanın. Azure Cosmos DB Gremlin API tarafından döndürülen sonuçlar GraphSON biçimiyle aynı biçime sahip değildir. 

* **Lambda ifadeleri ve işlevleri** şu anda desteklenmiyor. `.map{<expression>}`Bu, `.by{<expression>}`, ve `.filter{<expression>}` işlevleri içerir. Daha fazla bilgi edinmek ve Gremlin adımlarını kullanarak bunları nasıl yeniden yazacağız öğrenmek için [Lambdas'a Bir Not](http://tinkerpop.apache.org/docs/current/reference/#a-note-on-lambdas)bakın.

* ***Hareketler,*** sistemin dağıtılmış yapısı nedeniyle desteklenmez.  Gremlin hesabındauygun tutarlılık modelini "kendi yazılarınızı okumak" için yapılandırın ve çakışan yazıları çözmek için iyimser eşzamanlılık kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* Geri bildirim paylaşmak ve ekibin sizin için önemli olan özelliklere odaklanmasına yardımcı olmak için [Cosmos DB kullanıcı ses](https://feedback.azure.com/forums/263030-azure-cosmos-db) sayfasını ziyaret edin.
