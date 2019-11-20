---
title: MongoDB için Azure Cosmos DB API 'sindeki akışları değiştirme
description: Verilerinize yapılan değişiklikleri almak için MongoDB için akış değiştirme n Azure Cosmos DB API 'sini nasıl kullanacağınızı öğrenin.
author: srchi
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 11/16/2019
ms.author: srchi
ms.openlocfilehash: fbfce1c107fcf4b6f7d0b5f590a8ddfa64e69190
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74184732"
---
# <a name="change-streams-in-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API 'sindeki akışları değiştirme

Azure Cosmos DB, MongoDB için API 'sinde bulunan [akış](change-feed.md) desteğini değiştirme, akışları değiştirme API 'si kullanılarak kullanılabilir. Uygulamalarınız, değişiklik akışları API 'sini kullanarak koleksiyonda veya tek bir parçadaki öğelerde yapılan değişiklikleri alabilir. Daha sonra sonuçlara göre daha fazla işlem yapabilirsiniz. Koleksiyondaki öğelerde yapılan değişiklikler, değişiklik zamanının sırasına göre yakalanır ve sıralama düzeni parça anahtarı başına garanti edilir.

Aşağıdaki örnek, koleksiyondaki tüm öğelerde değişiklik akışlarının nasıl alınacağını gösterir. Bu örnek, öğeleri yerleştirildiğinde, güncelleştirilirken veya değiştirildiğinde izlemek için bir imleç oluşturur. Değişiklik akışlarını almak için $match aşama, $project aşama ve fullDocument seçeneği gereklidir. Değişiklik akışlarını kullanarak silme işlemleri için izleme şu anda desteklenmiyor. Geçici bir çözüm olarak, silinmekte olan öğelere bir yumuşak işaret ekleyebilirsiniz. Örneğin, "Deleted" adlı öğeye bir öznitelik ekleyebilir ve bunu "true" olarak ayarlayabilir ve onu izlemek için öğesini otomatik olarak silip öğe üzerinde bir TTL ayarlayabilirsiniz.

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

Aşağıdaki örnek, tek bir parçadaki öğelerde yapılan değişikliklerin nasıl alınacağını gösterir. Bu örnek, "a" ve parça anahtar değeri "1" değerine eşit olan öğelerin konumunu alır.

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

Değişiklik akışları kullanılırken aşağıdaki sınırlamalar geçerlidir:

* `operationType` ve `updateDescription` özellikleri çıkış belgesinde henüz desteklenmiyor.
* `insert`, `update`ve `replace` işlemleri türleri şu anda destekleniyor. Silme işlemi veya diğer olaylar henüz desteklenmiyor.

Bu sınırlamalar nedeniyle, önceki örneklerde gösterildiği gibi $match Stage, $project Stage ve fullDocument seçenekleri gereklidir.

## <a name="error-handling"></a>Hata işleme

Değişiklik akışları kullanılırken aşağıdaki hata kodları ve iletileri desteklenir:

* **Http hata kodu 429** -değişiklik akışı kısıtlandığından boş bir sayfa döndürülür.

* **NamespaceNotFound (OperationType geçersiz kıl)** -değişiklik akışını varolmayan koleksiyonda çalıştırırsanız veya koleksiyon bırakıldıysanız bir `NamespaceNotFound` hatası döndürülür. `operationType` özelliği çıktı belgesinde döndürülmediği için `operationType Invalidate` hata yerine `NamespaceNotFound` hatası döndürülür.

## <a name="next-steps"></a>Sonraki adımlar

* [MongoDB için Azure Cosmos DB API 'sindeki verilerin süresi dolduğunda otomatik olarak yaşam süresi kullanma](mongodb-time-to-live.md)
* [MongoDB için Azure Cosmos DB API 'sinde dizin oluşturma](mongodb-indexing.md)