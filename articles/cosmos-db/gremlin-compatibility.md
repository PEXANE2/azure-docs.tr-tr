---
title: Inkerpop özellikleriyle Gremlin uyumluluğu Azure Cosmos DB
description: Başvuru belgeleri grafik Altyapısı uyumluluk sorunları
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: sngun
ms.openlocfilehash: 581bc813ca27067b1f27ab9866a45df3084dbbcc
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75644741"
---
# <a name="azure-cosmos-db-gremlin-compatibility"></a>Azure Cosmos DB Gremlin uyumluluğu
Azure Cosmos DB Graph Engine, [Apache TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps) geçiş adımları belirtimini yakından izler, ancak farklılıklar vardır.

## <a name="behavior-differences"></a>Davranış farklılıkları

* Azure Cosmos DB Graph Engine, ınkerpop Gremlin 'ın derinliği ilk kez olacak şekilde, ***ilk*** çapraz geçişi çalıştırır. Bu davranış, Cosmos DB benzer şekilde yatay ölçeklenebilir sistemde daha iyi performans elde eder. 

## <a name="unsupported-features"></a>Desteklenmeyen özellikler

* ***[Gremlin Bytecode](https://tinkerpop.apache.org/docs/current/tutorials/gremlin-language-variants/)*** graf dolaşma işlemlerine yönelik programlama dilinden bağımsız bir belirtimdir. Cosmos DB Graph henüz desteklemiyor. `GremlinClient.SubmitAsync()` kullanın ve çapraz geçişi metin dizesi olarak geçirin.

* ***`property(set, 'xyz', 1)`*** kümesi kardinalitesi bugün desteklenmiyor. Bunun yerine `property(list, 'xyz', 1)` kullanın. Daha fazla bilgi için bkz. [ınkerpop Ile köşe özellikleri](http://tinkerpop.apache.org/docs/current/reference/#vertex-properties).

* ***`atch()`*** , bildirime dayalı model eşleştirmeyi kullanarak grafiklerin sorgulanmasına olanak tanır. Bu özellik kullanılamıyor.

* Köşelerin veya kenarlardaki ***Özellikler olarak nesneler*** desteklenmez. Özellikler yalnızca temel türler veya diziler olabilir.

* ***Dizi özelliklerine göre sıralama*** desteklenmez `order().by(<array property>)`. Yalnızca temel türlere göre sıralama desteklenir.

* ***Ilkel olmayan JSON türleri*** desteklenmez. `string`, `number`veya `true`/`false` türlerini kullanın. `null` değerleri desteklenmez. 

* ***GraphSONv3*** seri hale getirici Şu anda desteklenmiyor. Bağlantı yapılandırmasındaki `GraphSONv2` serileştirici, okuyucu ve yazıcı sınıfları kullanın.

* **Lambda ifadeleri ve işlevleri** Şu anda desteklenmemektedir. Buna `.map{<expression>}`, `.by{<expression>}`ve `.filter{<expression>}` işlevleri dahildir. Daha fazla bilgi edinmek ve Gremlin adımlarını kullanarak bunları yeniden yazmayı öğrenmek için [Lambdalar hakkında bir nota](http://tinkerpop.apache.org/docs/current/reference/#a-note-on-lambdas)bakın.

* Sistemin dağıtılmış doğası nedeniyle ***işlemler*** desteklenmiyor.  Gremlin hesabında uygun tutarlılık modelini "kendi yazınızla okumak" üzere yapılandırın ve çakışan yazmaları çözümlemek için iyimser eşzamanlılık kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* Geri bildirim paylaşmak ve takımınızın sizin için önemli olan özelliklere odaklanmasına yardımcı olmak için [Cosmos DB Kullanıcı sesi](https://feedback.azure.com/forums/263030-azure-cosmos-db) sayfasını ziyaret edin.
