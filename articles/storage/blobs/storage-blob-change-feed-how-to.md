---
title: Azure Blob Depolama 'da işlem akışı akışı (Önizleme) | Microsoft Dokümanlar
description: Bir .NET istemci uygulamasında besleme günlüklerini nasıl işleyeceğinizi öğrenin
author: normesta
ms.author: normesta
ms.date: 11/04/2019
ms.topic: article
ms.service: storage
ms.subservice: blobs
ms.reviewer: sadodd
ms.openlocfilehash: 75995eeb3f8255cb4c60d5be267f9c343edfea89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74111862"
---
# <a name="process-change-feed-in-azure-blob-storage-preview"></a>Azure Blob Depolama 'da akış değişikliği (Önizleme)

Değişiklik akışı, depolama hesabınızdaki blob'larda ve blob meta verilerinde meydana gelen tüm değişikliklerin işlem günlüklerini sağlar. Bu makalede, blob change feed işlemci kitaplığını kullanarak değişiklik akışı kayıtlarını nasıl okuyabileceğinizi gösterir.

Değişiklik akışı hakkında daha fazla bilgi edinmek için [Azure Blob Depolama 'da (Önizleme) akış değiştir'e](storage-blob-change-feed.md)bakın.

> [!NOTE]
> Değişiklik akışı genel önizlemede dir ve **westcentralus** ve **westus2** bölgelerinde kullanılabilir. Bilinen sorunlar ve sınırlamalarla birlikte bu özellik hakkında daha fazla bilgi edinmek için [Azure Blob Depolama'da özet akışı desteğini değiştir'e](storage-blob-change-feed.md)bakın. Akış işlemcikitaplığını değiştir, şimdi ile bu kitaplığın genel kullanıma sunulduğu zaman arasında değişebilir.

## <a name="get-the-blob-change-feed-processor-library"></a>Blob değiştirme besleme işlemcikitaplığını alın

1. Visual Studio'da URL'yi `https://azuresdkartifacts.blob.core.windows.net/azuresdkpartnerdrops/index.json` NuGet paket kaynaklarınıza ekleyin. 

   Nasıl yapılacağını öğrenmek için [paket kaynaklarına](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources)bakın.

2. NuGet Paket Yöneticisi'nde **Microsoft.Azure.Storage.Changefeed** paketini bulun ve projenize yükleyin. 

   Nasıl yapılacağını öğrenmek için [bkz.](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#find-and-install-a-package)

## <a name="connect-to-the-storage-account"></a>Depolama hesabına bağlanma

[CloudStorageAccount.TryParse](/dotnet/api/microsoft.azure.storage.cloudstorageaccount.tryparse) yöntemini arayarak bağlantı dizesini ayrıştın. 

Ardından, [CloudStorageAccount.CreateCloudBlobClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.blobaccountextensions.createcloudblobclient) yöntemini arayarak depolama hesabınızda Blob Depolama'yı temsil eden bir nesne oluşturun.

```cs
public bool GetBlobClient(ref CloudBlobClient cloudBlobClient, string storageConnectionString)
{
    if (CloudStorageAccount.TryParse
        (storageConnectionString, out CloudStorageAccount storageAccount))
        {
            cloudBlobClient = storageAccount.CreateCloudBlobClient();

            return true;
        }
        else
        {
            return false;
        }
    }
}
```

## <a name="initialize-the-change-feed"></a>Değişiklik akışını başlatma

Kod dosyanızın üst bölümüne aşağıdaki ifadeleri kullanarak ekleyin. 

```csharp
using Avro.Generic;
using ChangeFeedClient;
```

Ardından, **GetContainerReference** yöntemini arayarak **ChangeFeed** sınıfının bir örneğini oluşturun. Değişiklik besleme kapsayıcısı adına geçirin.

```csharp
public async Task<ChangeFeed> GetChangeFeed(CloudBlobClient cloudBlobClient)
{
    CloudBlobContainer changeFeedContainer =
        cloudBlobClient.GetContainerReference("$blobchangefeed");

    ChangeFeed changeFeed = new ChangeFeed(changeFeedContainer);
    await changeFeed.InitializeAsync();

    return changeFeed;
}
```

## <a name="reading-records"></a>Kayıtları okuma

> [!NOTE]
> Değişiklik akışı, depolama hesabınızda değişmez ve salt okunur bir varlıktır. Herhangi bir sayıda uygulama, değişiklik akışını aynı anda ve bağımsız olarak kendi kolaylıkları yla okuyabilir ve işleyebilir. Bir uygulama bunları okuduğunda kayıtlar değişiklik akışından kaldırılmaz. Her tüketen okuyucunun okuma veya yineleme durumu bağımsızdır ve yalnızca uygulamanız tarafından korunur.

Kayıtları okumanın en basit yolu **ChangeFeedReader** sınıfının bir örneğini oluşturmaktır. 

Bu örnek, değişiklik akışındaki tüm kayıtları yineler ve sonra her kayıttan birkaç değeri konsola yazdırır. 
 
```csharp
public async Task ProcessRecords(ChangeFeed changeFeed)
{
    ChangeFeedReader processor = await changeFeed.CreateChangeFeedReaderAsync();

    ChangeFeedRecord currentRecord = null;
    do
    {
        currentRecord = await processor.GetNextItemAsync();

        if (currentRecord != null)
        {
            string subject = currentRecord.record["subject"].ToString();
            string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
            string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

            Console.WriteLine("Subject: " + subject + "\n" +
                "Event Type: " + eventType + "\n" +
                "Api: " + api);
        }

    } while (currentRecord != null);
}
```

## <a name="resuming-reading-records-from-a-saved-position"></a>Kaydedilen konumdan okuma kayıtlarını devam landırma

Değişiklik akışınızda okuma konumunuz kaydedin ve ileriki bir zamanda kayıtları yeniden kaydetmeyi seçebilirsiniz. **ChangeFeedReader.SerializeState()** yöntemini kullanarak istediğiniz zaman değişiklik akışının yineleme durumunu kaydedebilirsiniz. Durum bir **dizedir** ve uygulamanız bu durumu uygulamanızın tasarımına göre kaydedebilir (Örneğin: bir veritabanına veya dosyaya).

```csharp
    string currentReadState = processor.SerializeState();
```

**CreateChangeFeedReaderFromPointerAsync** yöntemini kullanarak **ChangeFeedReader'ı** oluşturarak son durumdan gelen kayıtları yineleyerek devam edebilirsiniz.

```csharp
public async Task ProcessRecordsFromLastPosition(ChangeFeed changeFeed, string lastReadState)
{
    ChangeFeedReader processor = await changeFeed.CreateChangeFeedReaderFromPointerAsync(lastReadState);

    ChangeFeedRecord currentRecord = null;
    do
    {
        currentRecord = await processor.GetNextItemAsync();

        if (currentRecord != null)
        {
            string subject = currentRecord.record["subject"].ToString();
            string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
            string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

            Console.WriteLine("Subject: " + subject + "\n" +
                "Event Type: " + eventType + "\n" +
                "Api: " + api);
        }

    } while (currentRecord != null);
}

```

## <a name="stream-processing-of-records"></a>Kayıtların akış işleme

Akış kayıtlarını geldikleri gibi işlemeyi seçebilirsiniz. [Teknik Özelliklere](storage-blob-change-feed.md#specifications)Bakın.

```csharp
public async Task ProcessRecordsStream(ChangeFeed changeFeed, int waitTimeMs)
{
    ChangeFeedReader processor = await changeFeed.CreateChangeFeedReaderAsync();

    ChangeFeedRecord currentRecord = null;
    while (true)
    {
        do
        {
            currentRecord = await processor.GetNextItemAsync();

            if (currentRecord != null)
            {
                string subject = currentRecord.record["subject"].ToString();
                string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
                string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

                Console.WriteLine("Subject: " + subject + "\n" +
                    "Event Type: " + eventType + "\n" +
                    "Api: " + api);
            }

        } while (currentRecord != null);

        await Task.Delay(waitTimeMs);
    }
}
```

## <a name="reading-records-within-a-time-range"></a>Zaman aralığında kayıtları okuma

Değişiklik akışı, değişiklik olay saatini temel alınarak saatlik bölümler halinde düzenlenir. [Teknik Özelliklere](storage-blob-change-feed.md#specifications)Bakın. Belirli bir zaman aralığına giren değişiklik akışı bölümlerinden kayıtları okuyabilirsiniz.

Bu örnek, tüm kesimlerin başlangıç saatlerini alır. Daha sonra, başlangıç saati son sarf segmentinin zamanının ötesinde veya istenilen aralığın bitiş zamanının ötesine geçene kadar bu listeyi yineler. 

### <a name="selecting-segments-for-a-time-range"></a>Bir zaman aralığı için segmentleri seçme

```csharp
public async Task<List<DateTimeOffset>> GetChangeFeedSegmentRefsForTimeRange
    (ChangeFeed changeFeed, DateTimeOffset startTime, DateTimeOffset endTime)
{
    List<DateTimeOffset> result = new List<DateTimeOffset>();

    DateTimeOffset stAdj = startTime.AddMinutes(-15);
    DateTimeOffset enAdj = endTime.AddMinutes(15);

    DateTimeOffset lastConsumable = (DateTimeOffset)changeFeed.LastConsumable;

    List<DateTimeOffset> segments = 
        (await changeFeed.ListAvailableSegmentTimesAsync()).ToList();

    foreach (var segmentStart in segments)
    {
        if (lastConsumable.CompareTo(segmentStart) < 0)
        {
            break;
        }

        if (enAdj.CompareTo(segmentStart) < 0)
        {
            break;
        }

        DateTimeOffset segmentEnd = segmentStart.AddMinutes(60);

        bool overlaps = stAdj.CompareTo(segmentEnd) < 0 && 
            segmentStart.CompareTo(enAdj) < 0;

        if (overlaps)
        {
            result.Add(segmentStart);
        }
    }

    return result;
}
```

### <a name="reading-records-in-a-segment"></a>Bir segmentteki kayıtları okuma

Tek tek segmentlerden veya segment aralıklarından kayıtları okuyabilirsiniz.

```csharp
public async Task ProcessRecordsInSegment(ChangeFeed changeFeed, DateTimeOffset segmentOffset)
{
    ChangeFeedSegment segment = new ChangeFeedSegment(segmentOffset, changeFeed);
    await segment.InitializeAsync();

    ChangeFeedSegmentReader processor = await segment.CreateChangeFeedSegmentReaderAsync();

    ChangeFeedRecord currentRecord = null;
    do
    {
        currentRecord = await processor.GetNextItemAsync();

        if (currentRecord != null)
        {
            string subject = currentRecord.record["subject"].ToString();
            string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
            string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

            Console.WriteLine("Subject: " + subject + "\n" +
                "Event Type: " + eventType + "\n" +
                "Api: " + api);
        }

    } while (currentRecord != null);
}
```

## <a name="read-records-starting-from-a-time"></a>Bir andan itibaren kayıtları okuma

Değişiklik akışı kayıtlarını başlangıç segmentinden sonuna kadar okuyabilirsiniz. Bir zaman aralığında kayıtları okumaya benzer şekilde, segmentleri listeleyebilir ve yeniden yinelemek için bir segment seçebilirsiniz.

Bu örnek, işlemek için ilk segmentin [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?view=netframework-4.8) alır.

```csharp
public async Task<DateTimeOffset> GetChangeFeedSegmentRefAfterTime
    (ChangeFeed changeFeed, DateTimeOffset timestamp)
{
    DateTimeOffset result = new DateTimeOffset();

    DateTimeOffset lastConsumable = (DateTimeOffset)changeFeed.LastConsumable;
    DateTimeOffset lastConsumableEnd = lastConsumable.AddMinutes(60);

    DateTimeOffset timestampAdj = timestamp.AddMinutes(-15);

    if (lastConsumableEnd.CompareTo(timestampAdj) < 0)
    {
        return result;
    }

    List<DateTimeOffset> segments = (await changeFeed.ListAvailableSegmentTimesAsync()).ToList();
    foreach (var segmentStart in segments)
    {
        DateTimeOffset segmentEnd = segmentStart.AddMinutes(60);
        if (timestampAdj.CompareTo(segmentEnd) <= 0)
        {
            result = segmentStart;
            break;
        }
    }

    return result;
}
```

Bu örnek, başlangıç segmentinin [DateTimeOffset'inden](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?view=netframework-4.8) başlayarak akış kayıtlarını değiştirir.

```csharp
public async Task ProcessRecordsStartingFromSegment(ChangeFeed changeFeed, DateTimeOffset segmentStart)
{
    TimeSpan waitTime = new TimeSpan(60 * 1000);

    ChangeFeedSegment segment = new ChangeFeedSegment(segmentStart, changeFeed);

    await segment.InitializeAsync();

    while (true)
    {
        while (!await IsSegmentConsumableAsync(changeFeed, segment))
        {
            await Task.Delay(waitTime);
        }

        ChangeFeedSegmentReader reader = await segment.CreateChangeFeedSegmentReaderAsync();

        do
        {
            await reader.CheckForFinalizationAsync();

            ChangeFeedRecord currentItem = null;
            do
            {
                currentItem = await reader.GetNextItemAsync();
                if (currentItem != null)
                {
                    string subject = currentItem.record["subject"].ToString();
                    string eventType = ((GenericEnum)currentItem.record["eventType"]).Value;
                    string api = ((GenericEnum)((GenericRecord)currentItem.record["data"])["api"]).Value;

                    Console.WriteLine("Subject: " + subject + "\n" +
                        "Event Type: " + eventType + "\n" +
                        "Api: " + api);
                }
            } while (currentItem != null);

            if (segment.timeWindowStatus != ChangefeedSegmentStatus.Finalized)
            {
                await Task.Delay(waitTime);
            }
        } while (segment.timeWindowStatus != ChangefeedSegmentStatus.Finalized);

        segment = await segment.GetNextSegmentAsync(); // TODO: What if next window doesn't yet exist?
        await segment.InitializeAsync(); // Should update status, shard list.
    }
}

private async Task<bool> IsSegmentConsumableAsync(ChangeFeed changeFeed, ChangeFeedSegment segment)
{
    if (changeFeed.LastConsumable >= segment.startTime)
    {
        return true;
    }
    await changeFeed.InitializeAsync();
    return changeFeed.LastConsumable >= segment.startTime;
}
```

>[!TIP]
> Bir segmentbir veya daha fazla *chunkFilePath*değişiklik besleme günlükleri olabilir. Birden fazla *chunkFilePath* durumunda sistem, yayımlama iş kısmını yönetmek için kayıtları dahili olarak birden çok parçaya betkimiştir. Segmentin her bir bölümünün birbirini dışlayan lekeler için değişiklikler içereceği ve siparişi ihlal etmeden bağımsız olarak işlenebileceği garanti edilir. Senaryonuz için en verimli se, **changefeedSegmentShardReader** sınıfını kullanarak parça düzeyindeki kayıtları yineleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Özet akışı günlüklerini değiştirme hakkında daha fazla bilgi edinin. Bkz. [Azure Blob Depolama'da akış değiştir (Önizleme)](storage-blob-change-feed.md)
