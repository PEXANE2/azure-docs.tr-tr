---
title: Inkerpop özellikleriyle Gremlin uyumluluğu Azure Cosmos DB
description: Başvuru belgeleri grafik Altyapısı uyumluluk sorunları
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: sngun
ms.openlocfilehash: 989a033a843b861c34dc9dbdbced50399f8e5cd7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81449893"
---
# <a name="azure-cosmos-db-gremlin-compatibility"></a>Azure Cosmos DB Gremlin uyumluluğu
Azure Cosmos DB Graph Engine, [Apache TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps) geçiş adımları belirtimini yakından izler, ancak farklılıklar vardır.

## <a name="behavior-differences"></a>Davranış farklılıkları

* Azure Cosmos DB Graph Engine, ınkerpop Gremlin 'ın derinliği ilk kez olacak şekilde, ***ilk*** çapraz geçişi çalıştırır. Bu davranış, Cosmos DB benzer şekilde yatay ölçeklenebilir sistemde daha iyi performans elde eder. 

## <a name="unsupported-features"></a>Desteklenmeyen özellikler

* ***[Gremlin Bytecode](https://tinkerpop.apache.org/docs/current/tutorials/gremlin-language-variants/)*** graf dolaşma işlemlerine yönelik programlama dilinden bağımsız bir belirtimdir. Cosmos DB Graph henüz desteklemiyor. Geçişi `GremlinClient.SubmitAsync()` bir metin dizesi olarak kullanın ve geçirin.

* ***`property(set, 'xyz', 1)`*** kardinalite kümesi bugün desteklenmiyor. Bunun yerine `property(list, 'xyz', 1)` kullanın. Daha fazla bilgi için bkz. [ınkerpop Ile köşe özellikleri](http://tinkerpop.apache.org/docs/current/reference/#vertex-properties).

* ***`atch()`*** bildirim temelli model eşleştirmeyi kullanarak grafiklerin sorgulanmasına izin verir. Bu özellik kullanılamıyor.

* Köşelerin veya kenarlardaki ***Özellikler olarak nesneler*** desteklenmez. Özellikler yalnızca temel türler veya diziler olabilir.

* ***Dizi özelliklerine*** `order().by(<array property>)` göre sıralama desteklenmez. Yalnızca temel türlere göre sıralama desteklenir.

* ***Ilkel olmayan JSON türleri*** desteklenmez. , `string` `number` `true` Veya / türlerini `false` kullanın. `null`değerler desteklenmez. 

* ***GraphSONv3*** seri hale getirici Şu anda desteklenmiyor. Bağlantı `GraphSONv2` yapılandırmasındaki serileştirici, okuyucu ve yazıcı sınıflarını kullanın. Azure Cosmos DB Gremlin API tarafından döndürülen sonuçlar GraphSON biçimiyle aynı biçimde değil. 

* **Lambda ifadeleri ve işlevleri** Şu anda desteklenmemektedir. Bu,, `.map{<expression>}`ve `.filter{<expression>}` işlevlerini `.by{<expression>}`içerir. Daha fazla bilgi edinmek ve Gremlin adımlarını kullanarak bunları yeniden yazmayı öğrenmek için [Lambdalar hakkında bir nota](http://tinkerpop.apache.org/docs/current/reference/#a-note-on-lambdas)bakın.

* Sistemin dağıtılmış doğası nedeniyle ***işlemler*** desteklenmiyor.  Gremlin hesabında uygun tutarlılık modelini "kendi yazınızla okumak" üzere yapılandırın ve çakışan yazmaları çözümlemek için iyimser eşzamanlılık kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* Geri bildirim paylaşmak ve takımınızın sizin için önemli olan özelliklere odaklanmasına yardımcı olmak için [Cosmos DB Kullanıcı sesi](https://feedback.azure.com/forums/263030-azure-cosmos-db) sayfasını ziyaret edin.
