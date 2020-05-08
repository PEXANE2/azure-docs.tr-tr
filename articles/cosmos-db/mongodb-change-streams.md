---
title: MongoDB için Azure Cosmos DB API 'sindeki akışları değiştirme
description: Verilerinize yapılan değişiklikleri almak için Azure Cosmos DB, MongoDB için API 'sindeki değişiklik akışlarını nasıl kullanacağınızı öğrenin.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: tisande
ms.openlocfilehash: 7a6060448175530ada5ba95ceda470056a7be002
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872148"
---
# <a name="change-streams-in-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API 'sindeki akışları değiştirme

Azure Cosmos DB, MongoDB için API 'sinde bulunan [akış](change-feed.md) desteğini değiştirme, akışları değiştirme API 'si kullanılarak kullanılabilir. Uygulamalarınız, değişiklik akışları API 'sini kullanarak koleksiyonda veya tek bir parçadaki öğelerde yapılan değişiklikleri alabilir. Daha sonra sonuçlara göre daha fazla işlem yapabilirsiniz. Koleksiyondaki öğelerde yapılan değişiklikler, değişiklik zamanının sırasına göre yakalanır ve sıralama düzeni parça anahtarı başına garanti edilir.

> [!NOTE]
> Değişiklik akışlarını kullanmak için, Azure Cosmos DB MongoDB için API 'sinin veya sonraki bir sürümünün 3,6 sürümünü içeren hesabı oluşturun. Değişiklik akışı örneklerini önceki bir sürüme karşı çalıştırırsanız, `Unrecognized pipeline stage name: $changeStream` hatayı görebilirsiniz.

## <a name="current-limitations"></a>Geçerli sınırlamalar

Değişiklik akışları kullanılırken aşağıdaki sınırlamalar geçerlidir:

* `operationType` Ve `updateDescription` özellikleri çıkış belgesinde henüz desteklenmiyor.
* `insert`, `update`Ve `replace` işlemler türleri şu anda destekleniyor. 
* Silme işlemi veya diğer olaylar henüz desteklenmiyor.

Bu sınırlamalar nedeniyle, önceki örneklerde gösterildiği gibi $match Stage, $project Stage ve fullDocument seçenekleri gereklidir.

Azure Cosmos DB SQL API 'sindeki değişiklik akışının aksine, değişiklik akışlarını kullanmak için ayrı bir [değişiklik akışı Işlemci kitaplığı](change-feed-processor.md) veya bir kiralama kapsayıcısı gereksinimi yoktur. Şu anda değişiklik akışlarını işlemek için [Azure işlevleri tetikleyicilerinin](change-feed-functions.md) desteği yoktur.

## <a name="error-handling"></a>Hata işleme

Değişiklik akışları kullanılırken aşağıdaki hata kodları ve iletileri desteklenir:

* **Http hata kodu 16500** -değişiklik akışı kısıtlandığından boş bir sayfa döndürülür.

* **NamespaceNotFound (OperationType geçersiz kıl)** -değişiklik akışını varolmayan koleksiyonda çalıştırırsanız veya koleksiyon bırakıldıysanız bir `NamespaceNotFound` hata döndürülür. `operationType` Özelliği, çıkış belgesinde `operationType Invalidate` hata yerine döndürülmediğinden `NamespaceNotFound` hata döndürülür.

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, koleksiyondaki tüm öğelerde değişiklik akışlarının nasıl alınacağını gösterir. Bu örnek, öğeleri yerleştirildiğinde, güncelleştirilirken veya değiştirildiğinde izlemek için bir imleç oluşturur. Değişiklik `$match` akışlarını almak `$project` için aşama, `fullDocument` aşama ve seçenek gereklidir. Değişiklik akışlarını kullanarak silme işlemleri için izleme şu anda desteklenmiyor. Geçici bir çözüm olarak, silinmekte olan öğelere bir yumuşak işaret ekleyebilirsiniz. Örneğin, "Deleted" adlı öğeye bir öznitelik ekleyebilirsiniz. Öğeyi silmek istediğinizde, ' Deleted ' olarak `true` ayarlayabilir ve öğe ÜZERINDE bir TTL ayarlayabilirsiniz. "Deleted" güncelleştirmesi bir güncelleştirme `true` olduğundan, bu değişiklik akış değişikliğini görünür hale görünecektir.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

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
# <a name="c"></a>[, #](#tab/csharp)

```csharp
var pipeline = new EmptyPipelineDefinition<ChangeStreamDocument<BsonDocument>>()
    .Match(change => change.OperationType == ChangeStreamOperationType.Insert || change.OperationType == ChangeStreamOperationType.Update || change.OperationType == ChangeStreamOperationType.Replace)
    .AppendStage<ChangeStreamDocument<BsonDocument>, ChangeStreamDocument<BsonDocument>, BsonDocument>(
    "{ $project: { '_id': 1, 'fullDocument': 1, 'ns': 1, 'documentKey': 1 }}");

var options = new ChangeStreamOptions{
        FullDocument = ChangeStreamFullDocumentOption.UpdateLookup
    };

var enumerator = coll.Watch(pipeline, options).ToEnumerable().GetEnumerator();

while (enumerator.MoveNext()){
        Console.WriteLine(enumerator.Current);
    }

enumerator.Dispose();
```

## <a name="changes-within-a-single-shard"></a>Tek parça içindeki değişiklikler

Aşağıdaki örnek, tek bir parça içindeki öğelerde yapılan değişikliklerin nasıl alınacağını gösterir. Bu örnek, "a" ve parça anahtar değeri "1" değerine eşit olan öğelerin konumunu alır. Farklı parçalardan paralel olarak değişiklik okuyan farklı istemcilerin olması mümkündür.

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

## <a name="next-steps"></a>Sonraki adımlar

* [MongoDB için Azure Cosmos DB API 'sindeki verilerin süresi dolduğunda otomatik olarak yaşam süresi kullanma](mongodb-time-to-live.md)
* [MongoDB için Azure Cosmos DB API 'sinde dizin oluşturma](mongodb-indexing.md)
