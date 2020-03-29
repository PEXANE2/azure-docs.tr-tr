---
title: Azure Hizmet Kumaşında GüvenilirConcurrentQueue
description: ReliableConcurrentQueue paralel enqueues ve dequeues sağlayan bir yüksek iş bölümü sırasıdır.
ms.topic: conceptual
ms.date: 5/1/2017
ms.openlocfilehash: a7115db8259fde0e87e53557ecef730f8e82d2fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75462731"
---
# <a name="introduction-to-reliableconcurrentqueue-in-azure-service-fabric"></a>Azure Hizmet Kumaşında GüvenilirConcurrentQueue'e Giriş
Güvenilir Eşzamanlı Sıra, enqueue ve dequeue işlemleri için yüksek eşzamanlılık özellikleri bir eşzamanlı, işlem sel ve çoğaltılmış bir kuyruktır. [Güvenilir Kuyruk](https://msdn.microsoft.com/library/azure/dn971527.aspx) tarafından sağlanan sıkı FIFO siparişini gevşeterek yüksek iş ortası ve düşük gecikme süresini sağlamak üzere tasarlanmıştır ve bunun yerine en iyi performansla sipariş sağlar.

## <a name="apis"></a>API'ler

|Eşzamanlı Sıra                |Güvenilir Eşzamanlı Kuyruk                                         |
|--------------------------------|------------------------------------------------------------------|
| void Enqueue(T öğesi)           | Görev EnqueueAsync(ITransaction tx, T öğesi)                       |
| bool TryDequeue(çıkış T sonucu)  | Görev< KoşulluDeğer < T > > TryDequeueAsync(ITransaction tx)  |
| int Sayısı()                    | uzun Kont()                                                     |

## <a name="comparison-with-reliable-queue"></a>Güvenilir [Sıra](https://msdn.microsoft.com/library/azure/dn971527.aspx) ile Karşılaştırma

Güvenilir Eşzamanlı [Kuyruk, Güvenilir Sıraya](https://msdn.microsoft.com/library/azure/dn971527.aspx)alternatif olarak sunulur. Fifo'nun garanti altına alınması eşzamanlı lıkla bir denge gerektirdiğinden, sıkı FIFO siparişinin gerekli olmadığı durumlarda kullanılmalıdır.  [Güvenilir Kuyruk,](https://msdn.microsoft.com/library/azure/dn971527.aspx) FIFO siparişini zorlamak için kilitleri kullanır, en fazla bir işlemin sıraya girmesine izin verilir ve en fazla bir işlemin aynı anda sıradan çıkmasına izin verilir. Buna karşılık, Güvenilir Eşzamanlı Kuyruk sipariş kısıtlamasını rahatlatır ve herhangi bir sayıda eşzamanlı işlemin sıralarını ve dequeue işlemlerini birbirine bırakmasına izin verir. En iyi efor lu sıralama sağlanır, ancak Güvenilir Eşzamanlı Sırada iki değerin göreli sıralanması asla garanti edilemez.

Güvenilir Eşzamanlı Sıra, enqueue ve/veya dequeues gerçekleştiren birden çok eşzamanlı işlem olduğunda [Güvenilir Kuyruk'tan](https://msdn.microsoft.com/library/azure/dn971527.aspx) daha yüksek iş ve daha düşük gecikme süremi sağlar.

ReliableConcurrentQueue için örnek kullanım örneği [İleti Sırası](https://en.wikipedia.org/wiki/Message_queue) senaryosudur. Bu senaryoda, bir veya daha fazla ileti üreticisi kuyruğa öğe oluşturur ve ekler ve bir veya daha fazla ileti tüketicisi iletileri kuyruktan çeker ve işler. Birden çok üretici ve tüketici, sırayı işlemek için eşzamanlı hareketleri kullanarak bağımsız olarak çalışabilir.

## <a name="usage-guidelines"></a>Kullanım Yönergeleri
* Kuyruk, kuyruktaki öğelerin düşük bekletme süresine sahip olmasını bekler. Diğer bir deyişle, öğeler uzun süre kuyrukta kalmaz.
* Kuyruk sıkı FIFO siparişini garanti etmez.
* Sıra kendi yazılarını okumaz. Bir öğe bir hareket içinde sıralanmışsa, aynı işlem içinde bir dequeuer tarafından görünür olmayacaktır.
* Dequeues birbirinden izole edilmez. *A* maddesi *txnA*işleminde dequeued ise , *txnA* taahhüt edilmese bile, *A* öğesi eşzamanlı bir işlem *txnB*görünür olmaz.  *TxnA* iptal edilirse, *A* hemen *txnB'ye* görünür hale gelir.
* *TryPeekAsync* davranışı bir *TryDequeueAsync* kullanarak ve daha sonra işlem iptal ederek uygulanabilir. Bu davranışın bir örneği Programlama Desenleri bölümünde bulunabilir.
* Sayım işlem dışıdır. Kuyruktaki öğelerin sayısı hakkında bir fikir almak için kullanılabilir, ancak bir noktada-in-time temsil eder ve güvenilemez.
* Sistem üzerinde performans etkisi yaratabilecek uzun süren işlemlerden kaçınmak için, işlem etkinken sıradan silinen maddelerde pahalı işlemler gerçekleştirilmemelidir.

## <a name="code-snippets"></a>Kod Parçacıkları
Bize birkaç kod parçacıkları ve beklenen çıktıları bakalım. Özel durum işleme bu bölümde yoksayılır.

### <a name="instantiation"></a>Örnek oluşturma
Güvenilir Eşzamanlı Sıra örneği oluşturmak, diğer Güvenilir Koleksiyon'a benzer.

```csharp
IReliableConcurrentQueue<int> queue = await this.StateManager.GetOrAddAsync<IReliableConcurrentQueue<int>>("myQueue");
```

### <a name="enqueueasync"></a>EnqueueAsync
Burada EnqueueAsync ve beklenen çıktıları takip kullanmak için birkaç kod parçacıkları vardır.

- *Örnek 1: Tek SıraLı Görev*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

Görevin başarıyla tamamlandığını ve sırayı değiştiren eşzamanlı hareketler olmadığını varsayalım. Kullanıcı, sıranın aşağıdaki siparişlerden herhangi birinde öğeleri içermesini bekleyebilir:

> 10, 20
> 
> 20, 10


- *Örnek 2: Paralel Enqueue Görev*

```
// Parallel Task 1
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}

// Parallel Task 2
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 30, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 40, cancellationToken);

    await txn.CommitAsync();
}
```

Görevlerin başarıyla tamamlandığını, görevlerin paralel olarak çalıştırıldığını ve sırayı değiştiren başka eşzamanlı hareketler olmadığını varsayalım. Kuyruktaki öğelerin sırası hakkında hiçbir çıkarım yapılamaz. Bu kod parçacığı için, öğeler 4'ün herhangi birinde görünebilir! olası siparişler.  Kuyruk öğeleri özgün (enqueued) sırada tutmaya çalışır, ancak eşzamanlı işlemler veya hatalar nedeniyle bunları yeniden sipariş etmek zorunda kalabilir.


### <a name="dequeueasync"></a>DequeueAsync
TryDequeueAsync'i kullanmak için beklenen çıktıları izleyen birkaç kod parçacıkları aşağıda veda edebilirsiniz. Sıranın zaten kuyrukta aşağıdaki öğelerle doldurulmuş olduğunu varsayalım:
> 10, 20, 30, 40, 50, 60

- *Örnek 1: Tek Dequeue Görev*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

Görevin başarıyla tamamlandığını ve sırayı değiştiren eşzamanlı hareketler olmadığını varsayalım. Kuyruktaki öğelerin sırası hakkında herhangi bir çıkarım yapılamadığından, öğelerin üçünün herhangi bir sırada nisbeten silinebilir. Kuyruk öğeleri özgün (enqueued) sırada tutmaya çalışır, ancak eşzamanlı işlemler veya hatalar nedeniyle bunları yeniden sipariş etmek zorunda kalabilir.  

- *Örnek 2: Paralel Dequeue Görev*

```
// Parallel Task 1
List<int> dequeue1;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;

    await txn.CommitAsync();
}

// Parallel Task 2
List<int> dequeue2;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;

    await txn.CommitAsync();
}
```

Görevlerin başarıyla tamamlandığını, görevlerin paralel olarak çalıştırıldığını ve sırayı değiştiren başka eşzamanlı hareketler olmadığını varsayalım. Kuyruktaki öğelerin sırası hakkında herhangi bir çıkarım yapılamadığından, *dequeue1* ve *dequeue2* listelerinin her biri herhangi bir sırada iki öğe içerir.

Aynı öğe her iki listede de *görünmez.* Bu nedenle, dequeue1 *varsa 10*, *30*, o zaman dequeue2 *olurdu 20*, *40*.

- *Örnek 3: İşlem İptal ibaresi ile Sırayı Dequeue*

Uçuş içi dequeue'lerle yapılan bir hareketi iptal etmek, öğeleri sıranın başına geri koyar. Öğelerin sıranın başına geri konma sırası garanti edilmez. Aşağıdaki koda bakalım:

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort the transaction
    await txn.AbortAsync();
}
```
Öğelerin aşağıdaki sırada sırayla çözülttürün olduğunu varsayalım:
> 10, 20

Hareketi iptal ettiğimizde, maddeler aşağıdaki siparişlerden herhangi birinde sıranın başına geri eklenir:
> 10, 20
> 
> 20, 10

Aynı işlem başarıyla *Taahhüt*edilmedi tüm durumlarda için geçerlidir.

## <a name="programming-patterns"></a>Programlama Desenleri
Bu bölümde, ReliableConcurrentQueue'i kullanmada yararlı olabilecek birkaç programlama deleline bakalım.

### <a name="batch-dequeues"></a>Toplu Dequeues
Önerilen programlama deseni, tüketici görevinin aynı anda bir dequeue gerçekleştirmek yerine dequeue'lerini toplu olarak işlemesi içindir. Kullanıcı, her toplu iş veya toplu iş boyutu arasındaki gecikmeleri azaltmayı seçebilir. Aşağıdaki kod snippet bu programlama modelini gösterir. Bu örnekte, işlem işlem yapıldıktan sonra yapılır, bu nedenle işlem sırasında bir hata oluşursa, işlenmemiş maddeler işlenmeden kaybolur.  Alternatif olarak, işleme işlemin kapsamı içinde yapılabilir, ancak performans üzerinde olumsuz bir etkisi olabilir ve zaten işlenmiş maddelerin işlenmesini gerektirir.

```
int batchSize = 5;
long delayMs = 100;

while(!cancellationToken.IsCancellationRequested)
{
    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        for(int i = 0; i < batchSize; ++i)
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if (ret.HasValue)
            {
                // If an item was dequeued, add to the buffer for processing
                processItems.Add(ret.Value);
            }
            else
            {
                // else break the for loop
                break;
            }
        }

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }

    int delayFactor = batchSize - processItems.Count;
    await Task.Delay(TimeSpan.FromMilliseconds(delayMs * delayFactor), cancellationToken);
}
```

### <a name="best-effort-notification-based-processing"></a>En İyi Çaba Bildirim Tabanlı İşleme
Başka bir ilginç programlama deseni Kont API kullanır. Burada, kuyruk için en iyi çaba bildirim tabanlı işleme uygulayabilirsiniz. Sıra Sayısı, bir sırayı veya sırayı çözme görevini daraltmak için kullanılabilir.  Önceki örnekte olduğu gibi, işlem işlemin hareketin dışında gerçekleştiğinden, işleme sırasında bir hata oluşursa işlenmemiş maddelerin kaybolabileceğini unutmayın.

```
int threshold = 5;
long delayMs = 1000;

while(!cancellationToken.IsCancellationRequested)
{
    while (this.Queue.Count < threshold)
    {
        cancellationToken.ThrowIfCancellationRequested();

        // If the queue does not have the threshold number of items, delay the task and check again
        await Task.Delay(TimeSpan.FromMilliseconds(delayMs), cancellationToken);
    }

    // If there are approximately threshold number of items, try and process the queue

    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if (ret.HasValue)
            {
                // If an item was dequeued, add to the buffer for processing
                processItems.Add(ret.Value);
            }
        } while (processItems.Count < threshold && ret.HasValue);

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
}
```

### <a name="best-effort-drain"></a>En İyi Çaba Giderme
Veri yapısının eşzamanlı yapısı nedeniyle sıranın boşalması garanti edilemez.  Kuyruktaki hiçbir kullanıcı işlemi uçuş sırasında olmasa bile, TryDequeueAsync'e yapılan belirli bir çağrının daha önce sıralanmış ve işlenmiş bir öğeyi döndürmemesi mümkündür.  Enqueued öğe *sonunda* dequeue görünür hale garanti edilir, ancak bant dışı bir iletişim mekanizması olmadan, bağımsız bir tüketici tüm üreticiler durduruldu ve yeni enqueue işlemleri izin verilmiş olsa bile kuyruk sabit bir duruma ulaştığını bilemez. Bu nedenle, drenaj işlemi aşağıda uygulandığı gibi en iyi çabadır.

Kullanıcı, diğer tüm üretici ve tüketici görevlerini durdurmalı ve sırayı boşaltmaya çalışmadan önce uçuş içi işlemlerin işlemesini veya iptal etmesini beklemelidir.  Kullanıcı kuyrukta beklenen öğe sayısını biliyorsa, tüm öğelerin sıraya girildiğini bildiren bir bildirim ayarlayabilir.

```
int numItemsDequeued;
int batchSize = 5;

ConditionalValue ret;

do
{
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if(ret.HasValue)
            {
                // Buffer the dequeues
                processItems.Add(ret.Value);
            }
        } while (ret.HasValue && processItems.Count < batchSize);

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
} while (ret.HasValue);
```

### <a name="peek"></a>Göz At
ReliableConcurrentQueue *TryPeekAsync* api sağlamaz. Kullanıcılar bir *TryDequeueAsync* kullanarak ve daha sonra işlem iptal ederek peek semantik alabilirsiniz. Bu örnekte, dequeues yalnızca öğenin değeri *10'dan*büyükse işlenir.

```
using (var txn = this.StateManager.CreateTransaction())
{
    ConditionalValue ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);
    bool valueProcessed = false;

    if (ret.HasValue)
    {
        if (ret.Value > 10)
        {
            // Process the item
            Console.WriteLine("Value : " + ret.Value);
            valueProcessed = true;
        }
    }

    if (valueProcessed)
    {
        await txn.CommitAsync();    
    }
    else
    {
        await txn.AbortAsync();
    }
}
```

## <a name="must-read"></a>Mutlaka Okunmalı
* [Güvenilir Hizmetler hızlı başlat](service-fabric-reliable-services-quick-start.md)
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [Güvenilir Hizmetler bildirimleri](service-fabric-reliable-services-notifications.md)
* [Güvenilir Hizmetler Yedekleme ve Geri Yükleme (Olağanüstü Durum Kurtarma)](service-fabric-reliable-services-backup-restore.md)
* [Güvenilir Devlet Yöneticisi Yapılandırması](service-fabric-reliable-services-configuration.md)
* [Hizmet Kumaş Web API Hizmetleri ile Başlarken](service-fabric-reliable-services-communication-webapi.md)
* [Güvenilir Hizmet Programlama Modelinin Gelişmiş Kullanımı](service-fabric-reliable-services-advanced-usage.md)
* [Güvenilir Koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
