---
title: Azure Time Series Insights Preview verilerini sorgulama | Microsoft Docs
description: Azure Time Series Insights Preview verilerini sorgulama.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 10/07/2019
ms.custom: seodec18
ms.openlocfilehash: 0b2f89a846747eaf78c60077372b48802506731e
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72553375"
---
# <a name="data-querying"></a>Veri sorgulama

Azure Time Series Insights önizleme, genel yüzey API 'Leri aracılığıyla ortamda depolanan olaylar ve meta veriler üzerinde veri sorgulama imkanı sunar. Bu API 'Ler [Time Series Insights önizleme Gezgininde](./time-series-insights-update-explorer.md)de kullanılır.

Time Series Insights ' de üç birincil API kategorisi mevcuttur:

* **Ortam API 'leri**: Time Series Insights ortamının sorgularını etkinleştirilir. Sorgu örnekleri, çağıranın erişimi olan ortamların ve ortam meta verilerinin listesidir.

* **Zaman serisi modeli-sorgu (TSM-Q) API 'leri**: zaman serisi modelinin ortam bölümünde depolanan meta verilerde oluşturma, okuma, güncelleştirme ve SILME (CRUD) işlemlerine izin vermez. Örnekler, türler ve Hiyerarşiler örnektir.

* **Zaman serisi sorgusu (TSQ) API 'leri**: kaynak sağlayıcıdan kaydedildiğinden olay verilerinin alınmasına izin vermez. Bu API 'Ler, zaman serisi verilerinde hesaplamalar dönüştürmek, birleştirmek ve hesaplamalar gerçekleştirmek için işlemler gerçekleştirebilir.

[Zaman serisi ifadesi (TSX) dili](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx) güçlü bir dördüncü kategorisidir. Gelişmiş hesaplamanın birleşimini etkinleştirmek için zaman serisi modellerini kullanır.

## <a name="azure-time-series-insights-preview-core-apis"></a>Azure Time Series Insights Preview Core API 'Leri

Aşağıdaki temel API 'Ler desteklenir.

[![Time serisi sorgusuna genel bakış](media/v2-update-tsq/tsq.png)](media/v2-update-tsq/tsq.png#lightbox)

## <a name="environment-apis"></a>Ortam API 'Leri

Aşağıdaki ortam API 'Leri kullanılabilir:

* [Ortam API 'Si al](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environments-api): çağıranın erişim yetkisine sahip olduğu ortamların listesini döndürür.
* [Ortam kullanılabilirliği API 'si](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environment-availability-api): `$ts` olay zaman damgasına olay sayısı dağılımını döndürür. Bu API, varsa olay sayısını döndürerek zaman damgasında herhangi bir olay olup olmadığını belirlemenize yardımcı olur.
* [Olay şeması API 'Si al](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-event-schema-api): belirli bir arama yayılması için olay şeması meta verilerini döndürür. Bu API, belirtilen arama alanı için şemada bulunan tüm meta verileri ve özellikleri almaya yardımcı olur.

## <a name="time-series-model-query-tsm-q-apis"></a>Zaman serisi modeli-sorgu (TSD-Q) API 'Leri

Aşağıdaki zaman serisi modeli sorgu API 'Leri kullanılabilir:

* [Model ayarları API 'si](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api): varsayılan tür ve ortamın model adı üzerinde Get ve Patch seçeneklerini sunar.
* [Türler API 'si](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api): zaman serisi türlerinde ve bunlarla ilişkili değişkenlerde CRUD 'yi mümkün.
* [Hiyerarşiler API 'si](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api): zaman serisi hiyerarşilerinde ve bunlarla ilişkili alan yollarında CRUD 'yi mümkün.
* [Örnek API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api): zaman serisi örneklerine ve bunlarla ilişkili örnek alanlarına CRUD 'yi sunar.

## <a name="time-series-query-tsq-apis"></a>Zaman serisi sorgusu (TSQ) API 'Leri

Aşağıdaki zaman serisi sorgu API 'Leri kullanılabilir:

* [Olayları al API](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-events-api): kaynak sağlayıcıdan Time Series Insights kaydedildiğinden Time Series Insights verilerinin olayları sorgulamasını ve alınmasını mümkün.

* [Seri API 'Si al](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-series-api): hatta, hattaki verileri kullanarak yakalanan olaylardan Time Series Insights verilerinin sorgu ve alınmasına izin vermez. Döndürülen değerler modelde tanımlanan veya satır içi olarak verilen değişkenlere göre yapılır.

    >[!NOTE]
    > Toplama yan tümcesi, bir modelde belirtilse veya satır içi sağlanmış olsa bile yok sayılır.

  Get Series API 'SI her Aralık için her değişken için bir zaman serisi değeri döndürür. Bir zaman serisi değeri, Time Series Insights bir sorgudan çıkış JSON için kullanılan bir biçimdir. Döndürülen değerler, zaman serisi KIMLIĞI ve belirtilen değişken kümesi temel alınarak hesaplanır.

* [Toplama serisi API 'si](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#aggregate-series-api): kaydedilen verileri örnekleyerek ve toplayarak yakalanan olaylardan Time Series Insights verilerinin sorgu ve alınmasına izin vermez.

  Toplama serisi API 'SI her Aralık için her değişken için bir zaman serisi değeri döndürür. Değerler, zaman serisi KIMLIĞINI ve sağlanmış olan değişken kümesini temel alır. Toplama serisi API 'SI, zaman serisi modelinde depolanan veya toplu olarak veya örnek veriler için satır içi olarak belirtilen değişkenleri kullanarak azalmaya erişir.

  Desteklenen toplam türleri: `Min`, `Max`, `Sum`, `Count`, `Average`

## <a name="next-steps"></a>Sonraki adımlar

- Azure Time Series Insights önizlemede [depolama ve](./time-series-insights-update-storage-ingress.md) giriş hakkında daha fazla bilgi edinin.

- Time Series Insights Preview [veri modelleme](./time-series-insights-update-tsm.md) makalesini okuyun.

- [Zaman SERISI kimliği seçerken en iyi uygulamaları](./time-series-insights-update-how-to-id.md)keşfetme.
