---
title: MongoDB için Azure Cosmos DB'nin API'sındaki akışları değiştirme
description: Verilerinizde yapılan değişiklikleri almak için Azure Cosmos DB'nin MongoDB api'sinde değişiklik akışlarını nasıl kullanacağınızı öğrenin.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: tisande
ms.openlocfilehash: ecfa98241f74aac43a827b645a6ed877624d643d
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437818"
---
# <a name="change-streams-in-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB'nin API'sındaki akışları değiştirme

Azure Cosmos DB'nin MongoDB için API'sinde akış desteğini [değiştir,](change-feed.md) değişiklik akışları API'sini kullanarak kullanılabilir. Değişiklik akışları API'sini kullanarak, uygulamalarınız koleksiyonda veya öğelerde yapılan değişiklikleri tek bir parça halinde alabilir. Daha sonra sonuçlara dayalı olarak daha fazla işlem yapabilirsiniz. Koleksiyondaki öğelerdeki değişiklikler, değişiklik zamanları sırasına göre yakalanır ve parça anahtarı başına sıralama sırası garanti edilir.

> [!NOTE]
> Değişiklik akışlarını kullanmak için, Azure Cosmos DB'nin MongoDB için API sürümünün 3.6 sürümü veya daha sonraki bir sürümüyle hesabı oluşturun. Değişiklik akışı örneklerini önceki bir sürüme göre `Unrecognized pipeline stage name: $changeStream` çalıştırıyorsanız, hatayı görebilirsiniz.

## <a name="current-limitations"></a>Geçerli sınırlamalar

Değişiklik akışları kullanırken aşağıdaki sınırlamalar geçerlidir:

* Ve `operationType` `updateDescription` özellikleri henüz çıktı belgesinde desteklenmez.
* `insert`, `update`, ve `replace` işlem türleri şu anda desteklenir. Silme işlemi veya diğer olaylar henüz desteklenmez.

Bu sınırlamalar nedeniyle, önceki örneklerde gösterildiği gibi $match aşaması, $project aşaması ve fullDocument seçenekleri gereklidir.

Azure Cosmos DB'nin SQL API'sindeki değişiklik akışının aksine, değişiklik akışlarını veya kiralama kapsayıcısına ihtiyaç duyan ayrı bir [Değişiklik Akışı İşlemci Kitaplığı](change-feed-processor.md) yoktur. Değişiklik akışlarını işlemek için [Azure İşlevleri tetikleyicileri](change-feed-functions.md) için şu anda destek yok.

## <a name="error-handling"></a>Hata işleme

Değişiklik akışları kullanırken aşağıdaki hata kodları ve iletiler desteklenir:

* **HTTP hata kodu 16500** - Değişiklik akışı daraltıldığında, boş bir sayfa döndürür.

* **NamespaceNotFound (OperationType Geçersiz Kılma)** - Var olmayan koleksiyonda değişiklik akışını çalıştırırsanız veya `NamespaceNotFound` koleksiyon bırakılırsa, bir hata döndürülür. `operationType` Özellik çıktı belgesinde döndürülemediğinden, `operationType Invalidate` hata yerine `NamespaceNotFound` hata döndürülür.

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, koleksiyondaki tüm öğelerde değişiklik akışlarının nasıl alınış göstereceğini gösterir. Bu örnek, öğeleri eklendiğinde, güncelleştirildiğinde veya değiştirildiğinde izlemek için bir imleç oluşturur. Değişiklik `$match` akışlarını almak için aşama, `$project` aşama ve `fullDocument` seçenek gereklidir. Değişiklik akışlarını kullanarak işlemleri silme yi izlemek şu anda desteklenmemektedir. Geçici çözüm olarak, silinen öğelere yumuşak bir işaretçi ekleyebilirsiniz. Örneğin, "silinmiş" adlı öğeye bir öznitelik ekleyebilirsiniz. Öğeyi silmek istediğinizde, "silinmiş" öğeye `true` ayarlayabilir ve öğeye bir TTL ayarlayabilirsiniz. "Silindi"yi güncelleştirmeye `true` güncelleştirmek bir güncelleştirme olduğundan, bu değişiklik değişiklik akışında görünür.

### <a name="javascript"></a>Javascript:

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

### <a name="c"></a>C#:

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

## <a name="changes-within-a-single-shard"></a>Tek bir parça içindeki değişiklikler

Aşağıdaki örnek, tek bir parça içindeki öğelerde nasıl değişiklik alınış gösteriş yapılacağını gösterir. Bu örnek, "a" ile eşit sabit anahtar değerine sahip öğelerin değişikliklerini ve "1"e eşit parça anahtar değerini alır. Farklı istemcilerin farklı kırıklardan gelen değişiklikleri paralel olarak okuması mümkündür.

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

* [Azure Cosmos DB'nin MongoDB api'sinde verilerin otomatik olarak süresi dolmak için yaşamak için zamanı kullanın](mongodb-time-to-live.md)
* [MongoDB için Azure Cosmos DB'nin API'sinde dizin oluşturma](mongodb-indexing.md)
