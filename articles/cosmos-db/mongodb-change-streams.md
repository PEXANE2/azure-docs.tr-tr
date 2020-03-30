---
title: MongoDB için Azure Cosmos DB'nin API'sındaki akışları değiştirme
description: Verilerinizde yapılan değişiklikleri almak için MongoDB için Azure Cosmos DB'nin API'sini nasıl kullanacağınızı öğrenin.
author: srchi
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 11/16/2019
ms.author: srchi
ms.openlocfilehash: ec1ec1a8a80953f8988355341ee7128bd29b982d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77467786"
---
# <a name="change-streams-in-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB'nin API'sındaki akışları değiştirme

Azure Cosmos DB'nin MongoDB için API'sinde akış desteğini [değiştir,](change-feed.md) değişiklik akışları API'sini kullanarak kullanılabilir. Değişiklik akışları API'sini kullanarak, uygulamalarınız koleksiyonda veya öğelerde yapılan değişiklikleri tek bir parça halinde alabilir. Daha sonra sonuçlara dayalı olarak daha fazla işlem yapabilirsiniz. Koleksiyondaki öğelerdeki değişiklikler, değişiklik zamanları sırasına göre yakalanır ve parça anahtarı başına sıralama sırası garanti edilir.

> [!NOTE]
> Değişiklik akışlarını kullanmak için, Azure Cosmos DB'nin MongoDB için API sürümünün 3.6 sürümü veya daha sonraki bir sürümüyle hesabı oluşturun. Değişiklik akışı örneklerini önceki bir sürüme göre `Unrecognized pipeline stage name: $changeStream` çalıştırıyorsanız, hatayı görebilirsiniz. 

Aşağıdaki örnek, koleksiyondaki tüm öğelerde değişiklik akışlarının nasıl alınış göstereceğini gösterir. Bu örnek, öğeleri eklendiğinde, güncelleştirildiğinde veya değiştirildiğinde izlemek için bir imleç oluşturur. Değişiklik akışlarını almak için $match aşaması, $project aşaması ve fullDocument seçeneği gereklidir. Değişiklik akışlarını kullanarak işlemleri silme yi izlemek şu anda desteklenmemektedir. Geçici çözüm olarak, silinen öğelere yumuşak bir işaretçi ekleyebilirsiniz. Örneğin, "silinmiş" adlı öğeye bir öznitelik ekleyebilir ve "true" olarak ayarlayabilir ve öğeye bir TTL ayarlayabilirsiniz, böylece öğeyi otomatik olarak silebilir ve izleyebilir.

```javascript
var cursor = db.coll.watch(
    [
        { $match: { "operationType": { $in: ["insert", "update", "replace"] } } },
        { $project: { "_id": 1, "fullDocument": 1, "ns": 1, "documentKey": 1 } }
    ],
    { fullDocument: "updateLookup" });

while (!cursor.isExhausted()) {
    if (cursor.hasNext()) {
        printjson(cursor.next());
    }
}
```

Aşağıdaki örnek, tek bir parçadaki öğelerde değişiklik nasıl alınış gösterilmektedir. Bu örnek, "a" ile eşit sabit anahtar değerine sahip öğelerin değişikliklerini ve "1"e eşit parça anahtar değerini alır.

```javascript
var cursor = db.coll.watch(
    [
        { 
            $match: { 
                $and: [
                    { "fullDocument.a": 1 }, 
                    { "operationType": { $in: ["insert", "update", "replace"] } }
                ]
            }
        },
        { $project: { "_id": 1, "fullDocument": 1, "ns": 1, "documentKey": 1} }
    ],
    { fullDocument: "updateLookup" });

```

## <a name="current-limitations"></a>Geçerli sınırlamalar

Değişiklik akışları kullanırken aşağıdaki sınırlamalar geçerlidir:

* Ve `operationType` `updateDescription` özellikleri henüz çıktı belgesinde desteklenmez.
* `insert`, `update`, ve `replace` işlem türleri şu anda desteklenir. Silme işlemi veya diğer olaylar henüz desteklenmez.

Bu sınırlamalar nedeniyle, önceki örneklerde gösterildiği gibi $match aşaması, $project aşaması ve fullDocument seçenekleri gereklidir.

## <a name="error-handling"></a>Hata işleme

Değişiklik akışları kullanırken aşağıdaki hata kodları ve iletiler desteklenir:

* **HTTP hata kodu 429** - Değişiklik akışı daraltıldığında, boş bir sayfa döndürür.

* **NamespaceNotFound (OperationType Geçersiz Kılma)** - Var olmayan koleksiyonda değişiklik akışını çalıştırırsanız veya `NamespaceNotFound` koleksiyon bırakılırsa, bir hata döndürülür. `operationType` Özellik çıktı belgesinde döndürülemediğinden, `operationType Invalidate` hata yerine `NamespaceNotFound` hata döndürülür.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'nin MongoDB api'sinde verilerin otomatik olarak süresi dolmak için yaşamak için zamanı kullanın](mongodb-time-to-live.md)
* [MongoDB için Azure Cosmos DB'nin API'sinde dizin oluşturma](mongodb-indexing.md)
