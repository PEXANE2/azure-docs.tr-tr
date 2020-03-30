---
title: Önizlemede veri sorgulama - Azure Time Series Öngörüleri | Microsoft Dokümanlar
description: Azure Zaman Serisi Öngörüleri Önizlemesinde veri sorgu kavramları ve HTTP REST API genel bakışı.
author: shreyasharmamsft
ms.author: shresha
manager: dpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 03/25/2020
ms.custom: seodec18
ms.openlocfilehash: 23094ec71dac5780def10e16b90de0b818ef3c68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80284900"
---
# <a name="data-querying-in-azure-time-series-insights-preview"></a>Azure Time Series Öngörüleri Önizlemesinde veri sorgulama

Azure Time Series Öngörüleri, genel yüzey API'leri aracılığıyla ortamda depolanan olaylar ve meta veriler hakkında veri sorgulamaolanağı sağlar. Bu API'ler aynı zamanda [Time Series Insights Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-explorer)tarafından da kullanılmaktadır.

Time Series Insights'ta üç ana API kategorisi mevcuttur:

* **Çevre API'leri**: Bu API'ler Time Series Insights ortamının kendisinde sorguyapılmasını sağlar. Bunlar, arayanın erişebildiği ortamların listesini toplamak ve meta verileri çevrelemek için kullanılabilir.
* **Zaman Serisi Model-Sorgu (TSM-Q) API'leri**: Ortamın Zaman Serisi Modeli'nde depolanan meta veriler üzerinde (CRUD) oluşturma, okuma, güncelleme ve silme (CRUD) işlemlerinin oluşturulmasını sağlar. Bunlar, örneklere, türlere ve hiyerarşilere erişmek ve bunları erişmek için kullanılabilir.
* **Zaman Serisi Sorgusu (TSQ) API'leri**: Kaynak sağlayıcıdan kaydedildiği gibi telemetri veya olay verilerinin alınmasını sağlar ve gelişmiş skaler ve toplama işlevleri ni kullanarak veriler üzerinde performant hesaplamaları ve toplamaları sağlar.

Time Series Insights hesaplamaları ifade etmek için zengin bir dize tabanlı ifade dili, [Time Series Expression (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax)kullanır.

## <a name="azure-time-series-insights-core-apis"></a>Azure Time Series Öngörüleri çekirdek API'leri

Aşağıdaki temel API'ler desteklenir.

[![Zaman Serisi Sorgusuna genel bakış](media/v2-update-tsq/tsq.png)](media/v2-update-tsq/tsq.png#lightbox)

## <a name="environment-apis"></a>Çevre API'leri

* [Ortamlar API'si alın](https://docs.microsoft.com/rest/api/time-series-insights/management/environments/get): Arayanın erişmeye yetkili olduğu ortamların listesini verir.
* [Ortamlar Kullanılabilirlik API alın](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/getavailability): Olay zaman `$ts`damgası üzerinde olay sayısı dağıtımını verir. Bu API, varsa zaman aralıklarına bölünen olayların sayısını döndürerek ortamda herhangi bir olay olup olmadığını belirlemeye yardımcı olur.
* [Olay Şeması API'sini Alın](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/geteventschema): Belirli bir arama aralığı için olay şema meta verilerini verir. Bu API, verilen arama aralığı için şemada bulunan tüm meta verileri ve özellikleri nalınmasına yardımcı olur.

## <a name="time-series-model-query-tsm-q-apis"></a>Zaman Serisi Model-Sorgu (TSM-Q) API'leri

Bu API'lerin çoğu, birden çok Zaman Serisi Modeli varlıklarında toplu CRUD işlemlerini etkinleştirmek için toplu yürütme işlemini destekler:

* [Model Ayarları API](https://docs.microsoft.com/rest/api/time-series-insights/preview#model-settings-api): Varsayılan türde ve ortamın model adı üzerinde *GET* ve *PATCH'i* etkinleştirir.
* [Tip API](https://docs.microsoft.com/rest/api/time-series-insights/preview#types-api): Zaman Serisi türleri ve ilişkili değişkenler üzerinde CRUD sağlar.
* [Hiyerarşiler API](https://docs.microsoft.com/rest/api/time-series-insights/preview#hierarchies-api): Zaman Serisi hiyerarşileri ve ilişkili alan yollarında CRUD sağlar.
* [Örnekler API](https://docs.microsoft.com/rest/api/time-series-insights/preview#instances-api): Zaman Serisi örneklerinde ve ilişkili örnek alanlarında CRUD'u etkinleştirir. Ayrıca, Örnekler API aşağıdaki işlemleri destekler:
  * [Arama](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/search): Örnek özniteliklerine göre zaman serisi örneklerinde aramada isabetlerin kısmi bir listesini alır.
  * [Öner](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/suggest): Örnek özniteliklerine dayalı olarak zaman serisi örneklerini arar ve aramada isabetlerin kısmi bir listesini önerir.

## <a name="time-series-query-tsq-apis"></a>Zaman Serisi Sorgusu (TSQ) API'leri

Bu API'ler Time Series Insights'taki çok katmanlı depolama çözümümüzde her iki mağazada da mevcuttur. Sorgu URL parametreleri, sorgunun yürütülmesi gereken [mağaza türünü](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#uri-parameters) belirtmek için kullanılır:

* [Etkinlik API'sini Edinin](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getevents): Kaynak sağlayıcıdan Time Series Öngörüleri'nde kaydedildikleri sırada ham olayların ve ilişkili olay zaman damgalarının sorgulanmasını ve alınmasını sağlar. Bu API, belirli bir Zaman Serisi kimliği ve arama süresi için ham olayların alınmasına izin verir. Bu API, seçili girişiçin tam yanıt veri kümesini almak için pagination destekler. 

* [Seri API'yi Al](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getseries): Ham olaylar üzerinde değişkenler tarafından tanımlanan hesaplamaları uygulayarak, işlem yapılan değerlerin ve ilişkili olay zaman damgalarının sorgulanmasını ve alınmasını sağlar. Bu değişkenler Zaman Serisi Modeli'nde tanımlanabilir veya sorguda satır içinde sağlanabilir. Bu API, seçili girişiçin tam yanıt veri kümesini almak için pagination destekler. 

* [Toplam Seri API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries): Ham olaylar üzerinde değişkenler tarafından tanımlanan hesaplamalar uygulayarak, toplanan değerlerin ve ilişkili aralık zaman damgalarının sorgulanmasını ve alınmasını sağlar. Bu değişkenler Zaman Serisi Modeli'nde tanımlanabilir veya sorguda satır içinde sağlanabilir. Bu API, seçili girişiçin tam yanıt veri kümesini almak için pagination destekler. 
  
  Belirli bir arama aralığı ve aralığı için, bu API, Bir Zaman Serisi Kimliği için aralık başına değişken başına toplu yanıt verir. Yanıt veri kümesindeki aralık sayısı, dönem onayonaylarını (Unix çağından bu yana geçen milisaniye sayısı - 1 Ocak 1970) ve onayonaylarının sorguda belirtilen aralık aralığı boyutuna bölünmesiyle hesaplanır.

  Yanıt kümesinde döndürülen zaman damgaları, aralıktaki örneklenmiş olayların değil, sol aralık sınırlarıdır. 

## <a name="next-steps"></a>Sonraki adımlar

- [Zaman Serisi Modeli'nde](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-tsm)tanımlanabilecek farklı değişkenler hakkında daha fazla bilgi edinin.
- [Zaman Serisi Öngörüler Explorer'dan](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-explorer)verileri nasıl sorgulayabilirsiniz hakkında daha fazla bilgi edinin.
