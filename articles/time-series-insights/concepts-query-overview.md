---
title: Veri sorgulama-Azure Time Series Insights Gen2 | Microsoft Docs
description: Veri sorgulama kavramları ve REST API genel bakış Azure Time Series Insights Gen2.
author: shreyasharmamsft
ms.author: shresha
manager: dpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 08/31/2020
ms.custom: seodec18
ms.openlocfilehash: 0fb50e01e86c00ac863352118eb36f7470251435
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89489162"
---
# <a name="querying-data-from-azure-time-series-insights-gen2"></a>Azure Time Series Insights Gen2 'dan veri sorgulama

Azure Time Series Insights Gen2, genel yüzey API 'Leri aracılığıyla ortamda depolanan olaylar ve meta veriler üzerinde veri sorgulama imkanı sunar. Bu API 'Ler [Azure TIME SERIES INSIGHTS TSI Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-explorer)tarafından da kullanılır.

Azure Time Series Insights Gen2 ' de üç birincil API kategorisi mevcuttur:

* **Ortam API 'leri**: Bu apı 'Ler Azure Time Series Insights Gen2 ortamındaki sorguları etkinleştirir. Bunlar, arayanın erişiminin ve ortam meta verilerinde erişebileceği ortamların listesini toplamak için kullanılabilir.
* **Zaman serisi modeli-sorgu (TSM-Q) API 'leri**: ortamın zaman serisi modelinde depolanan meta verilerde oluşturma, okuma, güncelleştirme ve SILME (CRUD) işlemlerine izin vermez. Bunlar örneklere, türlere ve hiyerarşilere erişmek ve bunları düzenlemek için kullanılabilir.
* **Zaman serisi sorgusu (TSQ) API 'leri**: kaynak sağlayıcıdan kaydedildiğinden telemetri veya olay verilerinin alınmasına izin verebilir ve gelişmiş skaler ve toplama işlevlerini kullanarak verilerde performans hesaplamaları ve toplamaları etkinleştirilir.

Azure Time Series Insights Gen2, [zaman serisi değişkenlerinde](./concepts-variables.md)hesaplamaları ifade etmek için zengin bir dize tabanlı ifade dili, [zaman serisi ifadesi (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax)kullanır.

## <a name="azure-time-series-insights-gen2-apis-overview"></a>Azure Time Series Insights Gen2 API 'Lerine genel bakış

Aşağıdaki temel API 'Ler desteklenir.

[![Zaman serisi sorgusuna genel bakış](media/v2-update-tsq/tsq.png)](media/v2-update-tsq/tsq.png#lightbox)

## <a name="environment-apis"></a>Ortam API 'Leri

* [Ortamları al API 'si](https://docs.microsoft.com/rest/api/time-series-insights/management(gen1/gen2)/accesspolicies/listbyenvironment): çağıranın erişim yetkisine sahip olduğu ortamların listesini döndürür.
* [Ortamları al kullanılabilirlik API 'si](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/getavailability): olay zaman damgasına olay sayısı dağılımını döndürür `$ts` . Bu API, varsa, varsa zaman aralıklarıyla oluşan olay sayısını döndürerek ortamda herhangi bir olay olup olmadığını belirlemenize yardımcı olur.
* [Olay şeması API 'Si al](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/geteventschema): belirli bir arama yayılması için olay şeması meta verilerini döndürür. Bu API, belirtilen arama alanı için şemada bulunan tüm meta verileri ve özellikleri almaya yardımcı olur.

## <a name="time-series-model-query-tsm-q-apis"></a>Zaman serisi modeli-sorgu (TSD-Q) API 'Leri

Bu API 'lerin çoğu, birden çok zaman serisi model varlığı üzerinde toplu CRUD işlemlerini etkinleştirmek için toplu yürütme işlemini destekler:

* [Model ayarları API 'si](https://docs.microsoft.com/rest/api/time-series-insights/reference-model-apis): varsayılan tür ve ortamın model adı üzerinde *Get* ve *Patch* seçeneklerini sunar.
* [Türler API 'si](https://docs.microsoft.com/rest/api/time-series-insights/reference-model-apis#types-api): zaman serisi türlerinde ve bunlarla ilişkili değişkenlerde CRUD 'yi mümkün.
* [Hiyerarşiler API 'si](https://docs.microsoft.com/rest/api/time-series-insights/reference-model-apis#hierarchies-api): zaman serisi hiyerarşilerinde ve bunlarla ilişkili alan yollarında CRUD 'yi mümkün.
* [Örnek API](https://docs.microsoft.com/rest/api/time-series-insights/reference-model-apis#instances-api): zaman serisi örneklerine ve bunlarla ilişkili örnek alanlarına CRUD 'yi sunar. Ayrıca, örnekleri API 'SI aşağıdaki işlemleri destekler:
  * [Arama](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances/search): örnek özniteliklerine göre zaman serisi örnekleri aramasında bir isabetlerin kısmi listesini alır.
  * [Öneri](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances/suggest):, örnek özniteliklerine göre zaman serisi örnekleri aramasında arama sırasında oluşan kısmi bir liste arar ve bu noktaları önerir.

## <a name="time-series-query-tsq-apis"></a>Zaman serisi sorgusu (TSQ) API 'Leri

Bu API 'Ler, çok katmanlı depolama çözümümüzde (normal ve soğuk) her iki mağaza üzerinde de kullanılabilir. Sorgu URL 'SI parametreleri sorgunun yürütülmesi gereken [Depo türünü](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#uri-parameters) belirtmek için kullanılır:

* [Olayları alma API 'si](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#getevents): ham olayların ve ilişkili olay zaman damgalarının, kaynak sağlayıcıdan Azure Time Series Insights Gen2 içinde kaydedildikleri şekilde sorgu ve alma imkanı sunar. Bu API, belirli bir zaman serisi KIMLIĞI ve arama kapsamı için ham olayların alınmasına izin verir. Bu API, seçilen giriş için tüm yanıt veri kümesini almak üzere sayfalandırmayı destekler.

  > [!IMPORTANT]

  > * [JSON düzleştirme ve kaçış kuralları üzerinde yapılan değişikliklerin](https://docs.microsoft.com/azure/time-series-insights/ingestion-rules-update)bir parçası olarak, diziler **dinamik** tür olarak depolanır. Bu tür olarak depolanan yük özelliklerine **yalnızca olayları al API 'si aracılığıyla erişilebilir**.

* [SERI API al](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#getseries): ham olaylarda değişkenler tarafından tanımlanan hesaplamalar uygulanarak, hesaplanan değerlerin ve ilişkili olay zaman damgalarının sorgu ve alınmasına izin vermez. Bu değişkenler, zaman serisi modelinde tanımlanabilir ya da sorguda satır içi olarak sağlanmış olabilir. Bu API, seçilen giriş için tüm yanıt veri kümesini almak üzere sayfalandırmayı destekler.

* [Toplama serisi API 'si](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#aggregateseries): ham olaylarda değişkenler tarafından tanımlanan hesaplamalar uygulanarak toplanmış değerlerin ve ilişkili Aralık zaman damgalarının sorgu ve alınmasına izin vermez. Bu değişkenler, zaman serisi modelinde tanımlanabilir ya da sorguda satır içi olarak sağlanmış olabilir. Bu API, seçilen giriş için tüm yanıt veri kümesini almak üzere sayfalandırmayı destekler.
  
  Belirtilen arama kapsamı ve aralığı için bu API, bir zaman serisi KIMLIĞI için değişken başına toplam bir yanıt döndürür. Yanıt veri kümesindeki aralıkların sayısı, zaman aralığı (UNIX dönemi-1 Ocak 1970 ' den beri geçen milisaniye sayısı) ve onay işaretlerini sorguda belirtilen Aralık aralığı boyutuna bölerek hesaplanır.

  Yanıt kümesinde döndürülen zaman damgaları, aralıktan örneklenmiş olayları değil, sol Aralık sınırlardır.

## <a name="next-steps"></a>Sonraki adımlar

* [Zaman serisi modelinde](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-tsm)tanımlanabilen farklı değişkenler hakkında daha fazla bilgi edinin.
* [Azure Time Series Insights Gezgini](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-explorer)'nden verileri sorgulama hakkında daha fazla bilgi edinin.
