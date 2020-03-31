---
title: Dizinleyici yürütmeyi zamanlama
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama dizinleyicilerini içeriği düzenli aralıklarla veya belirli zamanlarda dizine ekecek şekilde zamanlayın.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 72326413d463d449d339b1f3fd241ba2c27b4b6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74112953"
---
# <a name="how-to-schedule-indexers-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da dizin oluşturma zamanlama

Bir dizin oluşturucu normalde oluşturulduktan hemen sonra bir kez çalışır. Portal, REST API veya .NET SDK'yı kullanarak isteğe bağlı olarak yeniden çalıştırabilirsiniz. Ayrıca, bir dizin leyiciyi zamanlamada düzenli aralıklarla çalışacak şekilde yapılandırabilirsiniz.

Dizinleyici zamanlamasının yararlı olduğu bazı durumlar:

* Kaynak veriler zaman içinde değişecektir ve Azure Bilişsel Arama dizinleyicilerin değiştirilen verileri otomatik olarak işlemesini istiyorsunuz.
* Dizin birden çok veri kaynağından doldurulur ve dizin leyicilerin çakışmaları azaltmak için farklı zamanlarda çalışmasını sağlamak istersiniz.
* Kaynak veriler çok büyük tür ve dizin leyici işlemeyi zamana yaymak istiyorsunuz. Büyük hacimli verileri dizine alma hakkında daha fazla bilgi için Azure [Bilişsel Arama'da büyük veri kümelerini nasıl dizine ekleyeceklerine](search-howto-large-index.md)bakın.

Zamanlayıcı, Azure Bilişsel Arama'nın yerleşik bir özelliğidir. Arama dizinleyicilerini denetlemek için harici bir zamanlayıcı kullanamazsınız.

## <a name="define-schedule-properties"></a>Zamanlama özelliklerini tanımlama

Dizinleyici zamanlamasının iki özelliği vardır:
* **Zamanlanan**dizinleyici yürütmeler arasındaki zaman miktarını tanımlayan aralık. İzin verilen en küçük aralık 5 dakika, en büyüğü ise 24 saattir.
* Dizinleyicinin ilk kez çalıştırılması gerektiğini gösteren **Başlangıç Saati (UTC)**

Dizin oluşturucuyu ilk oluştururken veya dizin oluşturucunun özelliklerini daha sonra güncelleştirerek bir zamanlama belirtebilirsiniz. Dizin leyici zamanlamaları [portal](#portal), [REST API](#restApi)veya [.NET SDK](#dotNetSdk)kullanılarak ayarlanabilir.

Bir dizin dizininin yalnızca bir yürütülmesi aynı anda çalıştırılabilir. Bir sonraki yürütme zamanlandığında bir dizin leyici zaten çalışıyorsa, bu yürütme bir sonraki zamanlanan saate ertelenir.

Bunu daha somut hale getirmek için bir örnek düşünelim. Dizin oluşturma takvimini saatlik **aralık** ve 1 Haziran 2019 **başlangıç saati** ile 8:00:00 UTC olarak yapılandırdığımızı varsayalım. Dizin leyici çalıştırın bir saatten uzun sürdüğünde neler olabileceği aşağıda açıklanabilir:

* İlk dizinleyici uygulaması 1 Haziran 2019 civarında 08:00'de UTC'de başlar. Bu yürütmenin 20 dakika (veya 1 saatten az bir süre) sürdüğünü varsayalım.
* İkinci infaz 1 Haziran 2019 civarında 09:00 UTC'de başlayacaktır. Bu yürütmenin 70 dakika sürdüğünü ve UTC saat 10:10'a kadar tamamlanmayacağını varsayalım.
* Üçüncü yürütme 10:00 UTC'de başlayacak, ancak o zaman önceki yürütme hala çalışıyor. Bu zamanlanmış yürütme sonra atlanır. Dizinleyicinin bir sonraki yürütmesi UTC saat 11:00'e kadar başlamaz.

> [!NOTE]
> Bir dizin leyici belirli bir zamanlamaya ayarlanır, ancak her çalıştığında aynı belgede tekrar tekrar başarısız olursa, dizinleyici başarılı bir şekilde ilerleme kaydedene kadar daha az sıklıkta (en az 24 saatte bir en fazla) çalışmaya başlar Tekrar.  Dizinleyicinin belirli bir noktada sıkışıp kalmasına neden olan sorunu düzelttettiğinizi düşünüyorsanız, dizinleyicinin isteğe bağlı çalışmasını gerçekleştirebilirsiniz ve bu başarıyla ilerleme kaydederse, dizinleyici yine ayarlanan zamanlama aralığına geri döner.

<a name="portal"></a>

## <a name="schedule-in-the-portal"></a>Portalda zamanlama

Portaldaki Veri Alma sihirbazı, oluşturma zamanında bir dizinleyicinin zamanlamasını tanımlamanızı sağlar. Varsayılan Zamanlama ayarı **Saatliktir,** bu da dizin oluşturucunun oluşturulduktan sonra bir kez çalıştığı ve daha sonra her saat tekrar çalıştığı anlamına gelir.

Dizinleyicinin otomatik olarak yeniden çalışmasını istemiyorsanız Zamanlama ayarını **Bir Kez** veya günde bir kez çalışacak **Günlük** olarak değiştirebilirsiniz. Farklı bir aralık veya belirli bir gelecekteki Başlangıç Saati belirtmek istiyorsanız özel **olarak** ayarlayın.

Zamanlamayı **Özel**olarak ayarladığınızda, alanlar **Aralığı** ve Başlangıç **Saatini (UTC)** belirtmenize izin vermek için görünür. İzin verilen en kısa zaman aralığı 5 dakika, en uzun süre 1440 dakikadır (24 saat).

   ![İçe Aktarma Verisi sihirbazında dizin oluşturma çizelgesi ni ayarlama](media/search-howto-schedule-indexers/schedule-import-data.png "İçe Aktarma Verisi sihirbazında dizin oluşturma çizelgesi ni ayarlama")

Dizin oluşturucu oluşturulduktan sonra, dizin oluşturucunun Edit panelini kullanarak zamanlama ayarlarını değiştirebilirsiniz. Zamanlama alanları, Veri Alma sihirbazıyla aynıdır.

   ![Dizinleyici Edit panelinde zamanlamayı ayarlama](media/search-howto-schedule-indexers/schedule-edit.png "Dizinleyici Edit panelinde zamanlamayı ayarlama")

<a name="restApi"></a>

## <a name="schedule-using-rest-apis"></a>REST API'lerini kullanarak zamanlama

REST API'sini kullanarak bir dizinleyicinin zamanlamasını tanımlayabilirsiniz. Bunu yapmak için, dizin oluşturup güncellerken **zamanlama** özelliğini ekleyin. Aşağıdaki örnek, varolan bir dizinleyiciyi güncelleştirmek için put isteğini gösterir:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2019-05-06
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

**Aralık** parametresi gereklidir. Aralık, iki ardışık dizinleyici yürütmenin başlangıcı arasındaki süreyi ifade eder. İzin verilen en küçük aralık 5 dakikadır; en uzunu bir gündür. XSD "dayTimeDuration" değeri [(ISO 8601 süre](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) değerinin sınırlı bir alt kümesi) olarak biçimlendirilmelidir. Bunun için desen: `P(nD)(T(nH)(nM))`. Örnekler: `PT15M` her 15 `PT2H` dakikada bir, her 2 saat için.

İsteğe bağlı **başlangıç Zamanı,** zamanlanmış yürütmelerin ne zaman başlaması gerektiğini gösterir. Atlanırsa, geçerli UTC saati kullanılır. Bu süre geçmişte olabilir, bu durumda ilk yürütme dizinleyici özgün **başlangıç Zamanından**beri sürekli çalışıyormuş gibi zamanlanır.

Ayrıca, Run Indexer çağrısını kullanarak istediğiniz zaman isteğe bağlı bir dizinleyici çalıştırabilirsiniz. Dizin leyicileri çalıştırma ve dizin oluşturma zamanlamaları hakkında daha fazla bilgi için, REST API Başvurusu'nda [Run Indexer,](https://docs.microsoft.com/rest/api/searchservice/run-indexer) [Get Indexer](https://docs.microsoft.com/rest/api/searchservice/get-indexer)ve [Update Indexer'a](https://docs.microsoft.com/rest/api/searchservice/update-indexer) bakın.

<a name="dotNetSdk"></a>

## <a name="schedule-using-the-net-sdk"></a>.NET SDK'yı kullanarak zamanlama

Azure Bilişsel Arama .NET SDK'yı kullanarak bir dizin leyicinin zamanlamasını tanımlayabilirsiniz. Bunu yapmak için, bir Dizin Oluşturup güncellerken **zamanlama** özelliğini ekleyin.

Aşağıdaki C# örneği, önceden tanımlanmış bir veri kaynağı ve dizin kullanarak bir dizin oluşturup, 30 dakika dan itibaren her gün bir kez çalışacak şekilde zamanlamasını ayarlar:

```
    Indexer indexer = new Indexer(
        name: "azure-sql-indexer",
        dataSourceName: dataSource.Name,
        targetIndexName: index.Name,
        schedule: new IndexingSchedule(
                        TimeSpan.FromDays(1), 
                        new DateTimeOffset(DateTime.UtcNow.AddMinutes(30))
                    )
        );
    await searchService.Indexers.CreateOrUpdateAsync(indexer);
```
**Zamanlama** parametresi atlanırsa, dizin oluşturucu oluşturulduktan hemen sonra yalnızca bir kez çalışır.

**Başlangıç Zamanı** parametresi geçmişte bir zaman olarak ayarlanabilir. Bu durumda, ilk yürütme dizinleyici verilen **başlangıç Zamanından**beri sürekli çalışıyormuş gibi zamanlanır.

Zamanlama, [IndexingSchedule](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingschedule?view=azure-dotnet) sınıfı kullanılarak tanımlanır. **IndexingSchedule** oluşturucu, **TimeSpan** nesnesi kullanılarak belirtilen bir **aralık** parametresi gerektirir. İzin verilen en küçük aralık değeri 5 dakika, en büyüğü 24 saattir. **DateTimeOffset** nesnesi olarak belirtilen ikinci **başlangıç Zamanı** parametresi isteğe bağlıdır.

.NET SDK, [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient) sınıfını ve **IIndexersOperations** arabiriminden yöntemler uygulayan [Indexers](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.indexers) özelliğini kullanarak dizin oluşturmayı denetlemenize olanak tanır. 

[Run](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexersoperationsextensions.run), [RunAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexersoperationsextensions.runasync)veya [RunWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations.runwithhttpmessagesasync) yöntemlerinden birini kullanarak istediğiniz zaman isteğe bağlı bir dizinleyici çalıştırabilirsiniz.

Dizin oluşturma, güncelleştirme ve çalıştırma hakkında daha fazla bilgi için [IIindexersOperations'a](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations?view=azure-dotnet)bakın.
