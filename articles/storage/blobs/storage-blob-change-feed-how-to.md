---
title: Azure Blob depolama 'da değişiklik akışını işleme (Önizleme) | Microsoft Docs
description: .NET istemci uygulamasında değişiklik akışı günlüklerini işlemeyi öğrenin
author: normesta
ms.author: normesta
ms.date: 06/18/2020
ms.topic: article
ms.service: storage
ms.subservice: blobs
ms.reviewer: sadodd
ms.openlocfilehash: dedf1174e00f5bb75822fb720a592af86121ec2d
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88691437"
---
# <a name="process-change-feed-in-azure-blob-storage-preview"></a>Azure Blob depolama 'da değişiklik akışını işleme (Önizleme)

Değişiklik akışı, bloblarda oluşan tüm değişikliklerin işlem günlükleri ve Depolama hesabınızdaki blob meta verileri sağlar. Bu makalede blob değişiklik akışı işlemci kitaplığını kullanarak değişiklik akışı kayıtlarının nasıl okunacağı gösterilmektedir.

Değişiklik akışı hakkında daha fazla bilgi edinmek için bkz. [Azure Blob depolama (Önizleme) içinde akışı değiştirme](storage-blob-change-feed.md).

> [!NOTE]
> Değişiklik akışı genel önizlemededir ve **westcentralus** ve **westus2** bölgelerinde kullanılabilir. Bu özellik hakkında bilinen sorunlar ve sınırlamalar hakkında daha fazla bilgi edinmek için bkz. [Azure Blob depolamada akış desteğini değiştirme](storage-blob-change-feed.md). Değişiklik akışı işlemci kitaplığı, şu anda ve bu kitaplık genel kullanıma sunulduğunda değişiklik gösterebilir.

## <a name="get-the-blob-change-feed-processor-library"></a>Blob değişiklik akışı işlemci kitaplığını al

1. Bir komut penceresi açın (örneğin: Windows PowerShell).
2. Proje dizininizden **Azure. Storage. Bloblar. changefeed** NuGet paketini yükler.

```console
dotnet add package Azure.Storage.Blobs.ChangeFeed --source https://azuresdkartifacts.blob.core.windows.net/azure-sdk-for-net/index.json --version 12.0.0-dev.20200604.2
```
## <a name="read-records"></a>Kayıtları oku

> [!NOTE]
> Değişiklik akışı, depolama hesabınızda sabit ve salt okunurdur bir varlıktır. Herhangi bir sayıda uygulama değişiklik akışını eşzamanlı ve bağımsız olarak kendi kolaylığını okuyabilir ve işleyebilir. Bir uygulama bunları okurken değişiklik akışından kayıt kaldırılmaz. Her bir tüketen okuyucunun okuma veya yineleme durumu yalnızca uygulamanız tarafından bağımsızdır ve korunur.

Bu örnek, değişiklik akışındaki tüm kayıtlar boyunca yinelenir, bunları bir listeye ekler ve sonra bu listeyi çağırana döndürür.
 
```csharp
public async Task<List<BlobChangeFeedEvent>> ChangeFeedAsync(string connectionString)
{
    // Get a new blob service client.
    BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);

    // Get a new change feed client.
    BlobChangeFeedClient changeFeedClient = blobServiceClient.GetChangeFeedClient();

    List<BlobChangeFeedEvent> changeFeedEvents = new List<BlobChangeFeedEvent>();

    // Get all the events in the change feed. 
    await foreach (BlobChangeFeedEvent changeFeedEvent in changeFeedClient.GetChangesAsync())
    {
        changeFeedEvents.Add(changeFeedEvent);
    }

    return changeFeedEvents;
}
```

Bu örnek, konsola listedeki her kayıttan birkaç değeri yazdırır. 

```csharp
public void showEventData(List<BlobChangeFeedEvent> changeFeedEvents)
{
    foreach (BlobChangeFeedEvent changeFeedEvent in changeFeedEvents)
    {
        string subject = changeFeedEvent.Subject;
        string eventType = changeFeedEvent.EventType.ToString();
        string api = changeFeedEvent.EventData.Api;

        Console.WriteLine("Subject: " + subject + "\n" +
        "Event Type: " + eventType + "\n" +
        "Api: " + api);
    }
}
```

## <a name="resume-reading-records-from-a-saved-position"></a>Kaydedilmiş bir konumdan kayıtları okumayı sürdürür

Okuma konumunuzu değişiklik akışına kaydetmeyi ve ardından gelecekte kayıtlar arasında yineleme işlemini sürdürmeyi seçebilirsiniz. Değişiklik akışı imlecini alarak okuma konumunu kaydedebilirsiniz. İmleç bir **dizedir** ve uygulamanız bu dizeyi uygulamanızın tasarımı için anlamlı olacak şekilde kaydedebilir (örneğin: bir dosyaya veya veritabanına).

Bu örnek, değişiklik akışındaki tüm kayıtlar boyunca yinelenir, bunları bir listeye ekler ve imleci kaydeder. Liste ve imleç çağırana döndürülür. 

```csharp
public async Task<(string, List<BlobChangeFeedEvent>)> ChangeFeedResumeWithCursorAsync
    (string connectionString,  string cursor)
{
    // Get a new blob service client.
    BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);

    // Get a new change feed client.
    BlobChangeFeedClient changeFeedClient = blobServiceClient.GetChangeFeedClient();
    List<BlobChangeFeedEvent> changeFeedEvents = new List<BlobChangeFeedEvent>();

    IAsyncEnumerator<Page<BlobChangeFeedEvent>> enumerator = changeFeedClient
        .GetChangesAsync(continuation: cursor)
        .AsPages(pageSizeHint: 10)
        .GetAsyncEnumerator();

    await enumerator.MoveNextAsync();

    foreach (BlobChangeFeedEvent changeFeedEvent in enumerator.Current.Values)
    {
    
        changeFeedEvents.Add(changeFeedEvent);             
    }
    
    // Update the change feed cursor.  The cursor is not required to get each page of events,
    // it is intended to be saved and used to resume iterating at a later date.
    cursor = enumerator.Current.ContinuationToken;
    return (cursor, changeFeedEvents);
}
```

## <a name="stream-processing-of-records"></a>Kayıtların akışını işleme

Değişiklik akışı kayıtlarını geldikçe işlemeyi tercih edebilirsiniz. Bkz. [Özellikler](storage-blob-change-feed.md#specifications). Her saat için değişiklikleri yoklamenizi öneririz.

Bu örnek, değişiklikleri düzenli aralıklarla yoklar.  Değişiklik kayıtları varsa, bu kod bu kayıtları işler ve değişiklik akışı imlecini kaydeder. Bu işlem durdurulur ve sonra yeniden başlatılırsa, uygulama son kaldığınız yerden kayıtları işlemeye başlamak için imleci kullanabilir. Bu örnek, imleci yerel bir uygulama yapılandırma dosyasına kaydeder, ancak uygulamanız onu senaryonuz için en anlamlı hale getiren herhangi bir biçimde kaydedebilir. 

```csharp
public async Task ChangeFeedStreamAsync
    (string connectionString, int waitTimeMs, string cursor)
{
    // Get a new blob service client.
    BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);

    // Get a new change feed client.
    BlobChangeFeedClient changeFeedClient = blobServiceClient.GetChangeFeedClient();

    while (true)
    {
        IAsyncEnumerator<Page<BlobChangeFeedEvent>> enumerator = changeFeedClient
        .GetChangesAsync(continuation: cursor).AsPages().GetAsyncEnumerator();

        while (true) 
        {
            var result = await enumerator.MoveNextAsync();

            if (result)
            {
                foreach (BlobChangeFeedEvent changeFeedEvent in enumerator.Current.Values)
                {
                    string subject = changeFeedEvent.Subject;
                    string eventType = changeFeedEvent.EventType.ToString();
                    string api = changeFeedEvent.EventData.Api;

                    Console.WriteLine("Subject: " + subject + "\n" +
                        "Event Type: " + eventType + "\n" +
                        "Api: " + api);
                }
            
                // helper method to save cursor. 
                SaveCursor(enumerator.Current.ContinuationToken);
            }
            else
            {
                break;
            }

        }
        await Task.Delay(waitTimeMs);
    }

}

public void SaveCursor(string cursor)
{
    System.Configuration.Configuration config = 
        ConfigurationManager.OpenExeConfiguration
        (ConfigurationUserLevel.None);

    config.AppSettings.Settings.Clear();
    config.AppSettings.Settings.Add("Cursor", cursor);
    config.Save(ConfigurationSaveMode.Modified);
}
```

## <a name="reading-records-within-a-time-range"></a>Bir zaman aralığı içinde kayıtları okuma

Belirli bir zaman aralığı içinde kalan kayıtları okuyabilirsiniz. Bu örnek, değişiklik akışındaki 3:00 Mart 2 2017 ve 2:00, 7 2019 Ekim, bir listeye ekleyen ve bu listeyi çağırana döndüren tüm kayıtlar boyunca yinelenir.

### <a name="selecting-segments-for-a-time-range"></a>Bir zaman aralığı için segmentleri seçme

```csharp
public async Task<List<BlobChangeFeedEvent>> ChangeFeedBetweenDatesAsync(string connectionString)
{
    // Get a new blob service client.
    BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);

    // Get a new change feed client.
    BlobChangeFeedClient changeFeedClient = blobServiceClient.GetChangeFeedClient();
    List<BlobChangeFeedEvent> changeFeedEvents = new List<BlobChangeFeedEvent>();

    // Create the start and end time.  The change feed client will round start time down to
    // the nearest hour, and round endTime up to the next hour if you provide DateTimeOffsets
    // with minutes and seconds.
    DateTimeOffset startTime = new DateTimeOffset(2017, 3, 2, 15, 0, 0, TimeSpan.Zero);
    DateTimeOffset endTime = new DateTimeOffset(2020, 10, 7, 2, 0, 0, TimeSpan.Zero);

    // You can also provide just a start or end time.
    await foreach (BlobChangeFeedEvent changeFeedEvent in changeFeedClient.GetChangesAsync(
        start: startTime,
        end: endTime))
    {
        changeFeedEvents.Add(changeFeedEvent);
    }

    return changeFeedEvents;
}
```

Sağladığınız başlangıç saati en yakın saate yuvarlanır ve bitiş saati en yakın saate yuvarlanır. Kullanıcıların başlangıç zamanından önce ve bitiş zamanından önce gerçekleşen olayları görebileceği olasıdır. Başlangıç ve bitiş zamanı arasında oluşan bazı olayların görünmeyeceği de mümkündür. Bunun nedeni, olayların başlangıç saatinden önceki bir saat boyunca veya bitiş saatinden sonraki saat boyunca kaydedilebildiği bir durum olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Değişiklik akışı günlükleri hakkında daha fazla bilgi edinin. Bkz. [Azure Blob depolamada akışı değiştirme (Önizleme)](storage-blob-change-feed.md)
