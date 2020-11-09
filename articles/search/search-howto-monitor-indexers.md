---
title: Dizin Oluşturucu durumunu ve sonuçlarını izleme
titleSuffix: Azure Cognitive Search
description: REST API veya .NET SDK kullanarak Azure portal Azure Bilişsel Arama dizin oluşturucularının durumunu, ilerlemesini ve sonuçlarını izleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/12/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 0107dfb24ddad2a5b0f9f0ab12d2fe701466e385
ms.sourcegitcommit: 65d518d1ccdbb7b7e1b1de1c387c382edf037850
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94372838"
---
# <a name="how-to-monitor-azure-cognitive-search-indexer-status-and-results"></a>Azure Bilişsel Arama Dizin Oluşturucu durumunu ve sonuçlarını izleme

Azure Bilişsel Arama her dizin oluşturucunun geçerli ve geçmiş çalıştırmaları hakkında durum ve izleme bilgileri sağlar.

Şunları yapmak istediğinizde Dizin Oluşturucu izleme yararlı olur:

* Bir dizin oluşturucunun ilerlemesini devam eden bir çalıştırma sırasında izleyin.
* Devam eden veya önceki Dizin Oluşturucu çalıştırmasının sonuçlarını gözden geçirin.
* En üst düzey Dizin Oluşturucu hatalarını ve dizin oluşturulan tek belgelerle ilgili hataları veya uyarıları belirler.

## <a name="get-status-and-history"></a>Durum ve geçmişi al

Dizin Oluşturucu izleme bilgilerine aşağıdakiler dahil olmak üzere çeşitli yollarla erişebilirsiniz:

* [Azure Portal](#portal)
* [REST API](#restapi) kullanma
* [.NET SDK 'yı](#dotnetsdk) kullanma

Kullanılabilir Dizin Oluşturucu izleme bilgileri aşağıdakilerin tümünü içerir (ancak veri biçimleri kullanılan erişim yöntemine göre farklılık gösterir):

* Dizin oluşturucunun kendisi ile ilgili durum bilgileri
* Dizin oluşturucunun durum, başlangıç ve bitiş zamanları, ayrıntılı hatalar ve uyarılar dahil en son çalışması hakkında bilgiler.
* Geçmiş dizin oluşturucunun bir listesi, durumları, sonuçları, hataları ve uyarıları çalışır.

Büyük hacimler veri işleyen Dizin oluşturucular çalıştırmak uzun sürebilir. Örneğin, milyonlarca kaynak belgeyi işleyen Dizin oluşturucular 24 saat boyunca çalıştırılabilir ve hemen hemen yeniden başlatılabilir. Yüksek hacimli dizin oluşturucularının durumu her zaman portalda **devam** edebilir. Bir Dizin Oluşturucu çalışırken bile devam eden ilerleme ve önceki çalıştırmalar hakkında ayrıntılar bulunur.

<a name="portal"></a>

## <a name="monitor-using-the-portal"></a>Portalı kullanarak izleme

Tüm Dizin oluşturucularınızın geçerli durumunu, arama hizmeti genel bakış sayfanızdaki **Dizin oluşturucular** listesinde görebilirsiniz.

   ![Dizin oluşturucular listesi](media/search-monitor-indexers/indexers-list.png "Dizin oluşturucular listesi")

Bir Dizin Oluşturucu yürütüldüğü zaman, listedeki durum **devam ediyor** ' i gösterir ve **docs başarılı** değeri, şimdiye kadar işlenen belgelerin sayısını gösterir. Portalın Dizin Oluşturucu durum değerlerini ve belge sayılarını güncelleştirmesi birkaç dakika sürebilir.

En son çalıştırma başarılı olan bir Dizin Oluşturucu **başarılı** gösterir. Hata sayısı, dizin oluşturucunun **en fazla başarısız öğe** ayarından daha küçükse, tek tek belgelerde hata olsa bile Dizin Oluşturucu çalıştırması başarılı olabilir.

En son çalıştırma bir hatayla bitdiyse, durum **başarısız** ' ı gösterir. **Reset** durumu, dizin oluşturucunun değişiklik izleme durumunun sıfırlandığı anlamına gelir.

Dizin oluşturucunun geçerli ve son çalıştırmaları hakkında daha fazla ayrıntı görmek için listedeki bir dizin oluşturucuya tıklayın.

   ![Dizin Oluşturucu Özeti ve yürütme geçmişi](media/search-monitor-indexers/indexer-summary.png "Dizin Oluşturucu Özeti ve yürütme geçmişi")

**Dizin Oluşturucu Özet** grafiğinde, en son çalıştırmalarından işlenen belgelerin sayısını görüntüleyen bir grafik görüntülenir.

**Yürütme ayrıntıları** listesinde en son yürütme sonuçlarının 50 ' i gösterilir.

Bu çalıştıra ilişkin ayrıntıları görmek için listede bir yürütme sonucuna tıklayın. Bu, başlangıç ve bitiş zamanlarını ve oluşan hata ve uyarıları içerir.

   ![Dizin Oluşturucu yürütme ayrıntıları](media/search-monitor-indexers/indexer-execution.png "Dizin Oluşturucu yürütme ayrıntıları")

Çalıştırma sırasında belgeye özgü sorunlar varsa, bunlar hatalar ve uyarılar alanlarında listelenecektir.

   ![Dizin Oluşturucu ayrıntıları hatalarla](media/search-monitor-indexers/indexer-execution-error.png "Dizin Oluşturucu ayrıntıları hatalarla")

Uyarılar bazı Dizin oluşturucular türleriyle ortaktır ve her zaman bir sorun göstermez. Örneğin bilişsel hizmetler kullanan Dizin oluşturucular, görüntü veya PDF dosyaları işlemek için herhangi bir metin içermiyorsa uyarıları rapor edebilir.

Dizin Oluşturucu hatalarını ve uyarılarını araştırma hakkında daha fazla bilgi için bkz. [Azure bilişsel arama 'da ortak Dizin Oluşturucu sorunlarını giderme](search-indexer-troubleshooting.md).

<a name="restapi"></a>

## <a name="monitor-using-rest-apis"></a>REST API 'Leri kullanarak izleme

Dizin Oluşturucu [durumunu Al komutunu](/rest/api/searchservice/get-indexer-status)kullanarak bir dizin oluşturucunun durum ve yürütme geçmişini alabilirsiniz:

```http
GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2020-06-30
api-key: [Search service admin key]
```

Yanıt genel Dizin Oluşturucu durumunu, son (veya sürmekte olan) Dizin Oluşturucu çağrısını ve son Dizin Oluşturucu etkinleştirmeleri geçmişini içerir.

```output
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
```

Yürütme geçmişi, geriye doğru kronolojik sıraya (en son ilk) göre sıralanan en güncel 50 çalıştırmaya kadar içerir.

İki farklı durum değeri olduğunu aklınızda edin. En üst düzey durum, dizin oluşturucunun kendisi içindir. Öğesinin Dizin Oluşturucu durumu, dizin oluşturucunun doğru şekilde ayarlandığı ve çalıştırılabileceği, ancak şu anda çalıştığı **anlamına gelir.**

Dizin oluşturucunun her çalışmasının Ayrıca, belirli yürütmenin devam eden ( **çalışıyor** ) veya **başarılı** , **geçişli bir hata** veya **kalıcı bir hata** durumuyla tamamlanmış olup olmadığını belirten kendi durumu vardır. 

Bir Dizin Oluşturucu değişiklik izleme durumunu yenilemek üzere sıfırlandığında, **sıfırlama** durumuyla ayrı bir yürütme geçmişi girişi eklenir.

Durum kodları ve Dizin Oluşturucu izleme verileri hakkında daha fazla bilgi için bkz. [GetIndexerStatus](/rest/api/searchservice/get-indexer-status).

<a name="dotnetsdk"></a>

## <a name="monitor-using-the-net-sdk"></a>.NET SDK 'Yı kullanarak izleme

Aşağıdaki C# örneği, Azure Bilişsel Arama .NET SDK 'sını kullanarak bir dizin oluşturucunun durumu ve konsolunda en son (veya devam eden) sonuçları hakkında bilgi yazar.

```csharp
static void CheckIndexerStatus(SearchIndexerClient indexerClient, SearchIndexer indexer)
{
    try
    {
        string indexerName = "hotels-sql-idxr";
        SearchIndexerStatus execInfo = indexerClient.GetIndexerStatus(indexerName);

        Console.WriteLine("Indexer has run {0} times.", execInfo.ExecutionHistory.Count);
        Console.WriteLine("Indexer Status: " + execInfo.Status.ToString());

        IndexerExecutionResult result = execInfo.LastResult;

        Console.WriteLine("Latest run");
        Console.WriteLine("Run Status: {0}", result.Status.ToString());
        Console.WriteLine("Total Documents: {0}, Failed: {1}", result.ItemCount, result.FailedItemCount);

        TimeSpan elapsed = result.EndTime.Value - result.StartTime.Value;
        Console.WriteLine("StartTime: {0:T}, EndTime: {1:T}, Elapsed: {2:t}", result.StartTime.Value, result.EndTime.Value, elapsed);

        string errorMsg = (result.ErrorMessage == null) ? "none" : result.ErrorMessage;
        Console.WriteLine("ErrorMessage: {0}", errorMsg);
        Console.WriteLine(" Document Errors: {0}, Warnings: {1}\n", result.Errors.Count, result.Warnings.Count);
    }
    catch (Exception e)
    {
        // Handle exception
    }
}
```

Konsolundaki çıktı şuna benzer şekilde görünür:

```output
Indexer has run 18 times.
Indexer Status: Running
Latest run
  Run Status: Success
  Total Documents: 7, Failed: 0
  StartTime: 11:29:31 PM, EndTime: 11:29:31 PM, Elapsed: 00:00:00.2560000
  ErrorMessage: none
  Document Errors: 0, Warnings: 0
```

İki farklı durum değeri olduğunu aklınızda edin. Üst düzey durum, dizin oluşturucunun durumunun kendisidir. Öğesinin Dizin Oluşturucu durumu, dizin oluşturucunun doğru şekilde ayarlandığı ve yürütme için kullanılabilir olduğu, ancak şu anda yürütülmekte olduğu **anlamına gelir.**

Dizin oluşturucunun her çalışmasının Ayrıca, belirli yürütmenin devam eden ( **çalışıyor** ) veya **başarılı** veya **geçişli** bir durum ile zaten tamamlanmış olup olmadığı için kendi durumu vardır. 

Bir Dizin Oluşturucu değişiklik izleme durumunu yenilemek üzere sıfırlandığında, **sıfırlama** durumuyla ayrı bir geçmiş girişi eklenir.

## <a name="next-steps"></a>Sonraki adımlar

Durum kodları ve Dizin Oluşturucu izleme bilgileri hakkında daha fazla ayrıntı için aşağıdaki API başvurusuna başvurun:

* [GetIndexerStatus (REST API)](/rest/api/searchservice/get-indexer-status)
* [IndexerStatus](/dotnet/api/azure.search.documents.indexes.models.indexerstatus)
* [IndexerExecutionStatus](/dotnet/api/azure.search.documents.indexes.models.indexerexecutionstatus)
* [IndexerExecutionResult](/dotnet/api/azure.search.documents.indexes.models.indexerexecutionresult)