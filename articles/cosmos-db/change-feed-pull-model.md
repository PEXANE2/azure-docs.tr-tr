---
title: Değişiklik akışı çekme modeli
description: Değişiklik akışını ve çekme modeli ve değişiklik akışı Işlemcisi arasındaki farkları okumak için Azure Cosmos DB değişiklik akışı çekme modelini nasıl kullanacağınızı öğrenin
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/19/2020
ms.reviewer: sngun
ms.openlocfilehash: c47016d0b82a4e4ed084f5d82394d91fd2b46be1
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83697721"
---
# <a name="change-feed-pull-model-in-azure-cosmos-db"></a>Azure Cosmos DB akış çekme modelini değiştirme

Değişiklik akışı çekme modeliyle birlikte Azure Cosmos DB değişiklik akışını kendi hızınızda kullanabilirsiniz. Değişiklik [akışı işlemcisi](change-feed-processor.md)ile zaten yaptığınız gibi, değişiklik akışı çekme modelini kullanarak değişikliklerin birden fazla değişiklik akışı tüketicisinden işlenmesini paralel hale getirmek.

> [!NOTE]
> Değişiklik akışı çekme modeli, şu anda yalnızca [Azure Cosmos DB .NET SDK sürümünde önizlemededir](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.9.0-preview) . Önizleme henüz diğer SDK sürümleri için kullanılamaz.

## <a name="consuming-an-entire-containers-changes"></a>Kapsayıcının tüm değişikliklerinin kullanılması

`FeedIterator`Çekme modelini kullanarak değişiklik akışını işlemek için bir oluşturabilirsiniz. İlk olarak bir oluşturduğunuzda, `FeedIterator` içinde isteğe bağlı olarak belirtebilirsiniz `StartTime` `ChangeFeedRequestOptions` . Belirtilmediğinde, belirtilmemişse `StartTime` geçerli saat olur.

`FeedIterator`İki şekilde sunulur. Aşağıdaki örnek varlık nesneleri döndüren örneklere ek olarak, destek ile yanıtı da elde edebilirsiniz `Stream` . Akışlar, verileri ilk seri durumdan çıkmadan, istemci kaynakları üzerinde kaydederek okuyabilmeniz için izin verir.

İşte `FeedIterator` , bu durumda bir nesnesi olan varlık nesnelerini döndüren bir örnek `User` :

```csharp
FeedIterator<User> iteratorWithPOCOS = container.GetChangeFeedIterator<User>();
```

Aşağıda şunu döndüren bir örneği elde etmek için bir örnek verilmiştir `FeedIterator` `Stream` :

```csharp
FeedIterator iteratorWithStreams = container.GetChangeFeedStreamIterator();
```

Bir kullanarak `FeedIterator` , kapsayıcının tüm değişiklik akışını kendi hızınızda kolayca işleyebilirsiniz. İşte bir örnek:

```csharp
FeedIterator<User> iteratorForTheEntireContainer= container.GetChangeFeedIterator<User>();

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

Bazı durumlarda, yalnızca belirli bir bölüm anahtarının değişikliklerini işlemek isteyebilirsiniz. `FeedIterator`Belirli bir bölüm anahtarı için bir elde edebilir ve değişiklikleri tüm kapsayıcı için kullanabileceğiniz şekilde işleyebilirsiniz.

```csharp
FeedIterator<User> iteratorForThePartitionKey = container.GetChangeFeedIterator<User>(new PartitionKey("myPartitionKeyValueToRead"));

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

[Değişiklik akışı işlemcisinde](change-feed-processor.md), çalışma otomatik olarak birden çok tüketiciye yayılır. Değişiklik akışı Çekme modelinde, `FeedRange` değişiklik akışı işlemesini paralel hale getirmek için kullanabilirsiniz. Bir `FeedRange` bölüm anahtarı değerleri aralığını temsil eder.

Bu, Kapsayıcınız için aralıkların bir listesinin nasıl alınacağını gösteren bir örnek aşağıda verilmiştir:

```csharp
IReadOnlyList<FeedRange> ranges = await container.GetFeedRangesAsync();
```

Kapsayıcınız için FeedRanges listesini aldığınızda, `FeedRange` [fiziksel bölüm](partition-data.md#physical-partitions)başına bir tane elde edersiniz.

Bir kullanarak `FeedRange` , `FeedIterator` birden fazla makinede veya iş parçacığında değişiklik akışı işlemesini paralel hale getirmek için bir oluşturabilirsiniz. Tüm kapsayıcının tek bir için nasıl alınacağını gösteren önceki örneğin aksine, `FeedIterator` `FeedRange` değişiklik akışını paralel olarak işleyebilen birden fazla Fede elde etmek için kullanabilirsiniz.

FeedRanges kullanmak istediğiniz durumda, FeedRanges alıp bu makinelere dağıtan bir Orchestrator işleminiz olması gerekir. Bu dağıtım şu olabilir:

* `FeedRange.ToJsonString`Bu dize değerini kullanarak ve dağıtarak. Tüketiciler bu değeri şu şekilde kullanabilir`FeedRange.FromJsonString`
* Dağıtım sürecde ise, `FeedRange` nesne başvurusunu geçirerek.

Aşağıda, paralel olarak okunan iki kuramsal ayrı makine kullanarak kapsayıcının değişiklik akışından nasıl okunacağını gösteren bir örnek verilmiştir:

Makine 1:

```csharp
FeedIterator<User> iteratorA = container.GetChangeFeedIterator<User>(ranges[0], new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});
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

`FeedIterator`Devamlılık belirteci oluşturarak sitenizin konumunu kaydedebilirsiniz. Devamlılık belirteci, FeedIterator 'ın son işlenen değişikliklerinin izlenmesini tutan bir dize değeridir. Bu, `FeedIterator` daha sonra bu noktada sürdürülmesine izin verir. Aşağıdaki kod, kapsayıcı oluşturulduktan sonra değişiklik akışını okuyacaktır. Daha fazla değişiklik yoksa, değişiklik akışı tüketiminin daha sonra devam edebilmesi için bir devamlılık belirteci kalıcı hale gelir.

```csharp
FeedIterator<User> iterator = container.GetChangeFeedIterator<User>(ranges[0], new ChangeFeedRequestOptions{StartTime = DateTime.MinValue});

string continuation = null;

while (iterator.HasMoreResults)
{
   FeedResponse<User> users = await iterator.ReadNextAsync();
   continuation = users.ContinuationToken;

   foreach (User user in users)
    {
        Console.WriteLine($"Detected change for user with id {user.id}");
    }
}

// Some time later
FeedIterator<User> iteratorThatResumesFromLastPoint = container.GetChangeFeedIterator<User>(continuation);
```

Cosmos kapsayıcısı hala mevcut olduğu sürece FeedIterator 'ın devamlılık belirtecinin süresi dolmaz.

## <a name="comparing-with-change-feed-processor"></a>Değişiklik akışı işlemcisi ile karşılaştırma

Pek çok senaryo değişiklik akışını [değişiklik akışı işlemcisini](change-feed-processor.md) veya çekme modelini kullanarak işleyebilir. Çekme modelinin devamlılık belirteçleri ve değişiklik akışı işlemcisinin kira kapsayıcısı, değişiklik akışındaki son işlenen öğe (veya öğe toplu işi) için hem "yer işaretleri" dir.
Ancak, devamlılık belirteçlerini bir kira kapsayıcısına dönüştüremezsiniz (veya tersi).

Bu senaryolarda çekme modelini kullanmayı göz önünde bulundurmanız gerekir:

- Belirli bir bölüm anahtarından değişiklikler okunuyor
- İstemcinizin işlenmek üzere değişiklikleri alacağı hızların denetlenmesi
- Değişiklik akışında var olan verileri bir kerelik okuma işlemi yapma (örneğin, veri geçişi yapmak için)

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