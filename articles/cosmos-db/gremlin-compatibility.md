---
title: Inkerpop özellikleriyle Gremlin uyumluluğu Azure Cosmos DB
description: Başvuru belgeleri grafik Altyapısı uyumluluk sorunları
author: olignat
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: olignat
ms.openlocfilehash: d58d1ea50251c3568161ce008db2672ff0fd6372
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70911090"
---
# <a name="azure-cosmos-db-gremlin-compatibility"></a>Azure Cosmos DB Gremlin uyumluluğu
Azure Cosmos DB Graph Engine, [Apache TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps) geçiş adımları belirtimini yakından izler, ancak farklılıklar vardır.

## <a name="behavior-differences"></a>Davranış farklılıkları

* Azure Cosmos DB Graph Engine, ınkerpop Gremlin 'ın derinliği ilk kez olacak şekilde, ***ilk*** çapraz geçişi çalıştırır. Bu davranış, Cosmos DB benzer şekilde yatay ölçeklenebilir sistemde daha iyi performans elde eder. 

## <a name="unsupported-features"></a>Desteklenmeyen özellikler

* ***[Gremlin Bytecode](http://tinkerpop.apache.org/docs/current/tutorials/gremlin-language-variants/)*** graf dolaşma işlemlerine yönelik programlama dilinden bağımsız bir belirtimdir. Cosmos DB Graph henüz desteklemiyor. Geçişi ```GremlinClient.SubmitAsync()``` bir metin dizesi olarak kullanın ve geçirin.

* ***```property(set, 'xyz', 1)```*** kardinalite kümesi bugün desteklenmiyor. Bunun yerine ```property(list, 'xyz', 1)``` kullanın.

* ***```match()```*** bildirim temelli model eşleştirmeyi kullanarak grafiklerin sorgulanmasına izin verir. Bu özellik kullanılamıyor.

* Köşelerin veya kenarlardaki ***Özellikler olarak nesneler*** desteklenmez. Özellikler yalnızca temel türler veya diziler olabilir.

* ***Dizi özelliklerine göre sıralama*** ```.order().by(<array property>)``` desteklenmez. Yalnızca temel türlere göre sıralama desteklenir.

* ***Ilkel olmayan JSON türleri*** desteklenmez. , ```string``` ```number```Veya türlerinikullanın/ . ```true``` ```false``` ```null```değerler desteklenmez. 

* ***GraphSONv3*** seri hale getirici bugün kullanılamıyor.

* Sistemin dağıtılmış doğası nedeniyle ***işlemler*** desteklenmiyor.  Gremlin hesabında uygun tutarlılık modelini "kendi yazınızla okumak" üzere yapılandırın ve çakışan yazmaları çözümlemek için iyimser eşzamanlılık kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* Geri bildirim paylaşmak ve takımınızın sizin için önemli olan özelliklere odaklanmasına yardımcı olmak için [Cosmos DB Kullanıcı sesi](https://feedback.azure.com/forums/263030-azure-cosmos-db) sayfasını ziyaret edin.
