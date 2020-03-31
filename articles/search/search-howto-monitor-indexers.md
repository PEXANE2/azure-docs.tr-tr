---
title: Dizin leyici durumunu ve sonuçlarını izleme
titleSuffix: Azure Cognitive Search
description: REST API veya .NET SDK'yı kullanarak Azure portalındaki Azure Bilişsel Arama dizinleyicilerinin durumunu, ilerlemesini ve sonuçlarını izleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 699b5a4e5a7f10c883667ca5030dd971855467f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74112982"
---
# <a name="how-to-monitor-azure-cognitive-search-indexer-status-and-results"></a>Azure Bilişsel Arama dizinleyici durumu ve sonuçları nasıl izlenir?

Azure Bilişsel Arama, her dizin leyicinin geçerli ve geçmiş çalıştırmaları hakkında durum ve izleme bilgileri sağlar.

Dizinleyici izleme istediğiniz zaman yararlıdır:

* Devam eden bir çalışma sırasında bir dizin leyicinin ilerlemesini izleyin.
* Devam eden veya önceki dizinleyici çalışmasının sonuçlarını gözden geçirin.
* Üst düzey dizinleyici hatalarını ve tek tek belgelerin dizine konmayla ilgili hataları veya uyarıları tanımlayın.

## <a name="get-status-and-history"></a>Durum ve geçmiş alın

Dizin leyici izleme bilgilerine aşağıdakiler dahil olmak üzere çeşitli şekillerde erişebilirsiniz:

* [Azure portalında](#portal)
* REST [API'sini](#restapi) kullanma
* [.NET SDK'yı](#dotnetsdk) kullanma

Kullanılabilir dizinleyici izleme bilgileri aşağıdakilerin tümünü içerir (ancak veri biçimleri kullanılan erişim yöntemine göre farklılık gösterir):

* Dizinleyicinin kendisi hakkında durum bilgileri
* Dizin leyicinin durumu, başlangıç ve bitiş saatleri ve ayrıntılı hatalar ve uyarılar dahil olmak üzere en son çalışması hakkında bilgi.
* Geçmiş dizinleyici çalışır listesi ve durumları, sonuçları, hataları ve uyarıları.

Büyük hacimli verileri işleyen dizinleyicilerin çalıştırılaması uzun sürebilir. Örneğin, milyonlarca kaynak belgeyi işleyen dizinleyiciler 24 saat boyunca çalıştırılabilir ve hemen yeniden başlatılabilir. Yüksek hacimli dizinleyicilerin durumu her zaman portalda **Devam Ediyor** diyebilir. Bir dizin leyici çalışırken bile, devam eden ilerleme ve önceki çalıştırmalar hakkında ayrıntılar kullanılabilir.

<a name="portal"></a>

## <a name="monitor-using-the-portal"></a>Portalı kullanarak izleme

Arama hizmeti Genel Bakış sayfanızda **Dizinleyiciler** listesinde tüm dizin leyicilerinizin geçerli durumunu görebilirsiniz.

   ![Dizin leyiciler listesi](media/search-monitor-indexers/indexers-list.png "Dizin leyiciler listesi")

Bir dizin leyici yürütüldüğünde, listedeki durum **Devam Eden'i**gösterir ve **Dokümanlar Başarılı** değeri şimdiye kadar işlenen belge sayısını gösterir. Portalın dizinleyici durum değerlerini ve belge sayımlarını güncelleştirmesi birkaç dakika sürebilir.

En son çalışması başarılı olan bir dizinleyici **Başarıyı**gösterir. Tek tek belgelerde hata olsa bile, dizinleyicinin **Max başarısız öğeleri** ayarından daha az olan dizin leyici çalıştırı başarılı olabilir.

En son çalıştırma bir hatayla sona erdiyse, durum **Başarısız'ı**gösterir. **Sıfırlama** durumu, dizinleyicinin değişiklik izleme durumunun sıfırlandığını belirtir.

Dizinleyicinin geçerli ve son çalıştırmaları hakkında daha fazla ayrıntı görmek için listedeki bir dizin leyiciyi tıklatın.

   ![Dizinleyici özeti ve yürütme geçmişi](media/search-monitor-indexers/indexer-summary.png "Dizinleyici özeti ve yürütme geçmişi")

**Dizin leyici özet** grafiği, en son çalıştırmalarında işlenen belge sayısının grafiğini görüntüler.

**Yürütme ayrıntıları** listesi en son yürütme sonuçlarının 50'sini gösterir.

Bu çalıştırmayla ilgili ayrıntıları görmek için listedeki yürütme sonucunu tıklatın. Bu, başlangıç ve bitiş saatlerini ve oluşan hataları ve uyarıları içerir.

   ![Dizinleyici yürütme ayrıntıları](media/search-monitor-indexers/indexer-execution.png "Dizinleyici yürütme ayrıntıları")

Çalışma sırasında belgeye özgü sorunlar varsa, bunlar Hatalar ve Uyarılar alanlarında listelenir.

   ![Hataları olan dizinleyici ayrıntıları](media/search-monitor-indexers/indexer-execution-error.png "Hataları olan dizinleyici ayrıntıları")

Uyarılar bazı dizin leyici türleri ile yaygındır ve her zaman bir sorun göstermez. Örneğin, bilişsel hizmetleri kullanan dizinleyiciler, görüntü veya PDF dosyaları işlenebilir herhangi bir metin içermediğinde uyarıları bildirebilir.

Dizin oluşturma yıkıntıcı hatalarını ve uyarıları araştırma hakkında daha fazla bilgi için Azure [Bilişsel Arama'da sık karşılaşılan sorun giderme dizin oluşturma konusuna](search-indexer-troubleshooting.md)bakın.

<a name="restapi"></a>

## <a name="monitor-using-rest-apis"></a>REST API'lerini kullanarak monitör

[Dizinleyici Durum Al komutunu](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status)kullanarak bir dizin dizininin durumunu ve yürütme geçmişini alabilirsiniz:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2019-05-06
    api-key: [Search service admin key]

Yanıt, genel dizinleyici durumunu, son (veya devam eden) dizinleyici çağrısını ve son dizinleyici çağrılarının geçmişini içerir.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2018-11-26T03:37:18.853Z",
            "endTime":"2018-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2018-11-26T03:37:18.853Z",
            "endTime":"2018-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Yürütme geçmişi, ters kronolojik sırada sıralanmış en son 50 çalıştırmayı içerir (en son önce).

İki farklı durum değeri olduğunu unutmayın. En üst düzey durum dizinleyicinin kendisi içindir. **Çalışan** dizinleyici durumu, dizinleyicinin doğru şekilde ayarlanıp çalıştırılabilmek için kullanılabilir olduğu, ancak şu anda çalışmakta olmadığı anlamına gelir.

Dizinleyicinin her çalışması, belirli yürütmenin devam ettiğini **(çalışıyor)** veya zaten bir **başarı**, **geçici Başarısızlık**veya kalıcı **Başarısızlık** durumuyla tamamlanıp tamamlanmadığını gösteren kendi durumu vardır. 

Bir dizin leyici, değişiklik izleme durumunu yenilemek için sıfırlandığında, **sıfırlama** durumuyla ayrı bir yürütme geçmişi girişi eklenir.

Durum kodları ve dizinleyici izleme verileri hakkında daha fazla bilgi için [GetIndexerStatus](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status)'a bakın.

<a name="dotnetsdk"></a>

## <a name="monitor-using-the-net-sdk"></a>.NET SDK'yı kullanarak monitör

Azure Bilişsel Arama .NET SDK'yı kullanarak bir dizin leyicinin zamanlamasını tanımlayabilirsiniz. Bunu yapmak için, bir Dizin Oluşturup güncellerken **zamanlama** özelliğini ekleyin.

Aşağıdaki C# örneği, bir dizin leyicinin durumu ve konsola en son (veya devam eden) çalışmasının sonuçları hakkında bilgi yazar.

```csharp
static void CheckIndexerStatus(Indexer indexer, SearchServiceClient searchService)
{
    try
    {
        IndexerExecutionInfo execInfo = searchService.Indexers.GetStatus(indexer.Name);

        Console.WriteLine("Indexer has run {0} times.", execInfo.ExecutionHistory.Count);
        Console.WriteLine("Indexer Status: " + execInfo.Status.ToString());

        IndexerExecutionResult result = execInfo.LastResult;

        Console.WriteLine("Latest run");
        Console.WriteLine("  Run Status: {0}", result.Status.ToString());
        Console.WriteLine("  Total Documents: {0}, Failed: {1}", result.ItemCount, result.FailedItemCount);

        TimeSpan elapsed = result.EndTime.Value - result.StartTime.Value;
        Console.WriteLine("  StartTime: {0:T}, EndTime: {1:T}, Elapsed: {2:t}", result.StartTime.Value, result.EndTime.Value, elapsed);

        string errorMsg = (result.ErrorMessage == null) ? "none" : result.ErrorMessage;
        Console.WriteLine("  ErrorMessage: {0}", errorMsg);
        Console.WriteLine("  Document Errors: {0}, Warnings: {1}\n", result.Errors.Count, result.Warnings.Count);
    }
    catch (Exception e)
    {
        // Handle exception
    }
}
```

Konsoldaki çıkış şuna benzer:

    Indexer has run 18 times.
    Indexer Status: Running
    Latest run
      Run Status: Success
      Total Documents: 7, Failed: 0
      StartTime: 10:02:46 PM, EndTime: 10:02:47 PM, Elapsed: 00:00:01.0990000
      ErrorMessage: none
      Document Errors: 0, Warnings: 0

İki farklı durum değeri olduğunu unutmayın. Üst düzey durum dizinleyicinin durumudur. **Çalışan** dizinleyici durumu, dizinleyicinin doğru şekilde ayarlanıp yürütme için kullanılabilir olduğu, ancak şu anda yürütüldettiği anlamına gelmez.

Dizinleyicinin her çalışması, belirli bir yürütmenin devam edip etmediği **(Çalışan)** veya başarı **veya** **Geçici Hata** durumuyla zaten tamamlanmış olup olmadığı konusunda da kendi durumu vardır. 

Bir dizin leyici, değişiklik izleme durumunu yenilemek için sıfırlandığında, **sıfırlama** durumuyla ayrı bir geçmiş girişi eklenir.

Durum kodları ve dizinleyici izleme bilgileri hakkında daha fazla bilgi için, REST API'deki [GetIndexerStatus'a](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status) bakın.

Belgelere özgü hatalar veya uyarılar la ilgili ayrıntılar listeler `IndexerExecutionResult.Errors` ve `IndexerExecutionResult.Warnings`.

Dizinleyicileri izlemek için kullanılan .NET SDK sınıfları hakkında [IndexerExecutionResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutionresult?view=azure-dotnet)daha fazla bilgi için [bkz.](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutioninfo?view=azure-dotnet)
