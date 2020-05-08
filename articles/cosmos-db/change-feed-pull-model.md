---
title: Akış çekme modelini Değiştir
description: Değişiklik akışını ve çekme modeli ve değişiklik akışı Işlemcisi arasındaki farkları okumak için Azure Cosmos DB değişiklik akışı çekme modelini nasıl kullanacağınızı öğrenin
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/06/2020
ms.reviewer: sngun
ms.openlocfilehash: 2854e3d92462ced3958afd1cf1e7e99d7e9892f6
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82984687"
---
# <a name="change-feed-pull-model-in-azure-cosmos-db"></a>Azure Cosmos DB akış çekme modelini değiştirme

Değişiklik akışı çekme modeliyle birlikte Azure Cosmos DB değişiklik akışını kendi hızınızda kullanabilirsiniz. Değişiklik [akışı işlemcisi](change-feed-processor.md)ile zaten yaptığınız gibi, değişiklik akışı çekme modelini kullanarak değişikliklerin birden fazla değişiklik akışı tüketicisinden işlenmesini paralel hale getirmek.

> [!NOTE]
> Değişiklik akışı çekme modeli, şu anda yalnızca [Azure Cosmos DB .NET SDK sürümünde önizlemededir](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.9.0-preview) . Önizleme henüz diğer SDK sürümleri için kullanılamaz.

## <a name="consuming-an-entire-containers-changes"></a>Kapsayıcının tüm değişikliklerinin kullanılması

Çekme modelini kullanarak değişiklik `FeedIterator` akışını işlemek için bir oluşturabilirsiniz. İlk olarak bir `FeedIterator`oluşturduğunuzda, içinde isteğe bağlı `StartTime` olarak belirtebilirsiniz. `ChangeFeedRequestOptions` Belirtilmediğinde, belirtilmemişse geçerli saat `StartTime` olur.

İki `FeedIterator` şekilde sunulur. Aşağıdaki örnek varlık nesneleri döndüren örneklere ek olarak, destek ile `Stream` yanıtı da elde edebilirsiniz. Akışlar, verileri ilk seri durumdan çıkmadan, istemci kaynakları üzerinde kaydederek okuyabilmeniz için izin verir.

İşte, bu durumda bir `FeedIterator` `User` nesnesi olan varlık nesnelerini döndüren bir örnek:

```csharp
FeedIterator<User> iteratorWithPOCOS = container.GetChangeFeedIterator<User>();
```

Aşağıda şunu döndüren bir `FeedIterator` örneği elde etmek için bir örnek `Stream`verilmiştir:

```csharp
FeedIterator iteratorWithStreams = container.GetChangeFeedStreamIterator();
```

Bir `FeedIterator`kullanarak, kapsayıcının tüm değişiklik akışını kendi hızınızda kolayca işleyebilirsiniz. Bir örneği aşağıda verilmiştir:

```csharp
FeedIterator<User> iteratorForTheEntireContainer= container.GetChangeFeedIterator(new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});

while (iteratorForTheEntireContainer.HasMoreResults)
{
   FeedResponse<User> users = await iteratorForTheEntireContainer.ReadNextAsync();

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}
```

## <a name="consuming-a-partition-keys-changes"></a>Bölüm anahtarının değişikliklerini kullanma

Bazı durumlarda, yalnızca belirli bir bölüm anahtarının değişikliklerini işlemek isteyebilirsiniz. Belirli bir bölüm anahtarı `FeedIterator` için bir elde edebilir ve değişiklikleri tüm kapsayıcı için kullanabileceğiniz şekilde işleyebilirsiniz:

```csharp
FeedIterator<User> iteratorForThePartitionKey = container.GetChangeFeedIterator(new PartitionKey("myPartitionKeyValueToRead"), new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});

while (iteratorForThePartitionKey.HasMoreResults)
{
   FeedResponse<User> users = await iteratorForThePartitionKey.ReadNextAsync();

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}
```

## <a name="using-feedrange-for-parallelization"></a>Paralelleştirme için FeedRange kullanma

[Değişiklik akışı işlemcisinde](change-feed-processor.md), çalışma otomatik olarak birden çok tüketiciye yayılır. Değişiklik akışı Çekme modelinde, değişiklik akışı işlemesini paralel hale getirmek `FeedRange` için kullanabilirsiniz. Bir `FeedRange` bölüm anahtarı değerleri aralığını temsil eder.

Bu, Kapsayıcınız için aralıkların bir listesinin nasıl alınacağını gösteren bir örnek aşağıda verilmiştir:

```csharp
IReadOnlyList<FeedRange> ranges = await container.GetFeedRangesAsync();
```

Kapsayıcınız için FeedRanges listesini aldığınızda, `FeedRange` [fiziksel bölüm](partition-data.md#physical-partitions)başına bir tane elde edersiniz.

Bir `FeedRange`kullanarak, birden fazla makinede veya iş `FeedIterator` parçacığında değişiklik akışı işlemesini paralel hale getirmek için bir oluşturabilirsiniz. Tüm kapsayıcının tek `FeedIterator` bir için nasıl alınacağını gösteren önceki örneğin aksine, değişiklik akışını paralel olarak işleyebilen birden fazla `FeedRange` Fede elde etmek için kullanabilirsiniz.

FeedRanges kullanmak istediğiniz durumda, FeedRanges alıp bu makinelere dağıtan bir Orchestrator işleminiz olması gerekir. Bu dağıtım şu olabilir:

* Bu `FeedRange.ToJsonString` dize değerini kullanarak ve dağıtarak. Tüketiciler bu değeri şu şekilde kullanabilir`FeedRange.FromJsonString`
* Dağıtım sürecde ise, `FeedRange` nesne başvurusunu geçirerek.

Aşağıda, paralel olarak okunan iki kuramsal ayrı makine kullanarak kapsayıcının değişiklik akışından nasıl okunacağını gösteren bir örnek verilmiştir:

Makine 1:

```csharp
FeedIterator<User> iteratorA = container.GetChangeFeedIterator<Person>(ranges[0], new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});
while (iteratorA.HasMoreResults)
{
   FeedResponse<User> users = await iteratorA.ReadNextAsync();

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}
```

Makine 2:

```csharp
FeedIterator<User> iteratorB = container.GetChangeFeedIterator<User>(ranges[1], new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});
while (iteratorB.HasMoreResults)
{
   FeedResponse<User> users = await iteratorB.ReadNextAsync();

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}
```

## <a name="saving-continuation-tokens"></a>Devamlılık belirteçleri kaydediliyor

Devamlılık belirteci oluşturarak sitenizin `FeedIterator` konumunu kaydedebilirsiniz. Devamlılık belirteci, FeedIterator 'ın son işlenen değişikliklerinin izlenmesini tutan bir dize değeridir. Bu, `FeedIterator` daha sonra bu noktada sürdürülmesine izin verir. Aşağıdaki kod, kapsayıcı oluşturulduktan sonra değişiklik akışını okuyacaktır. Daha fazla değişiklik yoksa, değişiklik akışı tüketiminin daha sonra devam edebilmesi için bir devamlılık belirteci kalıcı hale gelir.

```csharp
FeedIterator<User> iterator = container.GetChangeFeedIterator<User>(ranges[0], new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});

string continuation = null;

while (iterator.HasMoreResults)
{
   FeedResponse<User> users = await iterator.ReadNextAsync();
   continuation = orders.ContinuationToken;

   foreach (User user in Users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}

// Some time later
FeedIterator<User> iteratorThatResumesFromLastPoint = container.GetChangeFeedIterator<User>(continuation);
```

## <a name="comparing-with-change-feed-processor"></a>Değişiklik akışı işlemcisi ile karşılaştırma

Pek çok senaryo değişiklik akışını [değişiklik akışı işlemcisini](change-feed-processor.md) veya çekme modelini kullanarak işleyebilir. Çekme modelinin devamlılık belirteçleri ve değişiklik akışı işlemcisinin kira kapsayıcısı, değişiklik akışındaki son işlenen öğe (veya öğe toplu işi) için hem "yer işaretleri" dir.
Ancak, devamlılık belirteçlerini bir kira kapsayıcısına dönüştüremezsiniz (veya tersi).

Bu senaryolarda çekme modelini kullanmayı göz önünde bulundurmanız gerekir:

- Değişiklik akışında var olan verileri tek seferlik okumak istiyorsunuz
- Yalnızca belirli bir bölüm anahtarından değişiklikleri okumak istiyorsunuz
- Bir gönderme modeli istemezsiniz ve değişiklik akışını kendi hızınızda kullanmak isteyeceksiniz

Aşağıda, değişiklik akışı işlemcisi ve çekme modeli arasındaki bazı önemli farklılıklar verilmiştir:

|  | Değişiklik akışı işlemcisi| Çekme modeli |
| --- | --- | --- |
| Değişiklik akışındaki geçerli noktayı takip tutma | Kira (Azure Cosmos DB kapsayıcısında depolanır) | Devamlılık belirteci (bellekte depolanan veya el ile kalıcı) |
| Geçmiş değişiklikleri yeniden oynatma yeteneği | Evet, gönderim modeliyle | Evet, çekme modeliyle|
| Gelecekteki değişiklikler için yoklama | Kullanıcı tarafından belirtilen değişiklikleri otomatik olarak denetler`WithPollInterval` | El ile |
| Tüm kapsayıcılardan değişiklikleri işle | Evet ve aynı kapsayıcıdan birden çok iş parçacığı/makine tarafından otomatik olarak paralelleştirildi| Evet ve Feedtoken kullanarak manuel olarak paralelleştirildi |
| Yalnızca tek bir bölüm anahtarından değişiklikleri işle | Desteklenmiyor | Yes|
| Destek düzeyi | Genel kullanıma sunuldu | Önizleme |

## <a name="next-steps"></a>Sonraki adımlar

* [Değişiklik akışına genel bakış](change-feed.md)
* [Değişiklik akışı işlemcisini kullanma](change-feed-processor.md)
* [Azure İşlevleri'ni tetikleme](change-feed-functions.md)