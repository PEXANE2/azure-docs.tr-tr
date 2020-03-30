---
title: İstek telemetrisi için veri modeli - Azure Uygulama Öngörüleri
description: İstek telemetrisi için Uygulama Öngörüleri veri modeli
ms.topic: conceptual
ms.date: 01/07/2019
ms.reviewer: sergkanz
ms.openlocfilehash: d8a28063bf6780c3cace4ead81e289779b95eb9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671911"
---
# <a name="request-telemetry-application-insights-data-model"></a>İstek telemetrisi: Application Insights veri modeli

İstek telemetri öğesi [(Uygulama Öngörüleri'nde),](../../azure-monitor/app/app-insights-overview.md)uygulamanız için harici bir istek tarafından tetiklenen mantıksal yürütme sırasını temsil eder. Her istek yürütme benzersiz `ID` `url` ve tüm yürütme parametreleri içeren tarafından tanımlanır. İstekleri mantıksal `name` olarak gruplayabilir `source` ve bu isteğin tanımlanmasını sağlayabilirsiniz. Kod yürütme neden `success` `fail` olabilir veya `duration`ve belirli bir . Hem başarı hem de başarısızlık yürütmeleri `resultCode`daha da . Zarf düzeyinde tanımlanan istek telemetrisi için başlangıç zamanı.

İstek telemetrisi, standart genişletilebilirlik `properties` `measurements`modelini özel ve .

## <a name="name"></a>Adı

İsteğin adı, isteği işlemek için alınan kod yolunu temsil eder. İsteklerin daha iyi gruplanmasına izin vermek için düşük önem düzeyi değeri. HTTP istekleri için gerçek `GET /values/{id}` `id` değeri olmadan http yöntemi ve URL yolu şablonu temsil eder.

Application Insights web SDK, mektup davası yla ilgili olarak talep adı "olduğu gibi" gönderir. UI'de gruplandırma, büyük/küçük harf duyarlıdır, bu nedenle `GET /Home/Index` genellikle aynı denetleyici ve eylem yürütmeyle sonuçlanmış olsalar `GET /home/INDEX` bile ayrı olarak sayılır. Bunun nedeni, genel olarak url'lerin [büyük/küçük harf duyarlı](https://www.w3.org/TR/WD-html40-970708/htmlweb.html)olmasıdır. Büyük harfle yazılan `404` url'ler için tüm bunların olup olmadığını görmek isteyebilirsiniz. [Blog gönderisinde](https://apmtips.com/blog/2015/02/23/request-name-and-url/)Web SDK ASP.NET tarafından istek adı toplama hakkında daha fazla bilgi edinebilirsiniz.

Maksimum uzunluk: 1024 karakter

## <a name="id"></a>Kimlik

İstek arama örneğinin tanımlayıcısı. İstek ve diğer telemetri öğeleri arasındaki korelasyon için kullanılır. Kimlik genel olarak benzersiz olmalıdır. Daha fazla bilgi için [korelasyon](../../azure-monitor/app/correlation.md) sayfasına bakın.

Maksimum uzunluk: 128 karakter

## <a name="url"></a>Url

Tüm sorgu dize parametreleri ile URL isteyin.

Maksimum uzunluk: 2048 karakter

## <a name="source"></a>Kaynak

İsteğin kaynağı. Örnekler, arayanın enstrümantasyon anahtarı veya arayanın ip adresidir. Daha fazla bilgi için [korelasyon](../../azure-monitor/app/correlation.md) sayfasına bakın.

Maksimum uzunluk: 1024 karakter

## <a name="duration"></a>Süre

Formatta istek `DD.HH:MM:SS.MMMMMM`süresi: . Olumlu ve birkaç `1000` günden az olmalı. İstek telemetrisi baş ve son ile işlemi temsil olarak bu alan gereklidir.

## <a name="response-code"></a>Yanıt kodu

Bir istek yürütme sonucu. HTTP istekleri için HTTP durum kodu. Diğer istek `HRESULT` türleri için değer veya özel durum türü olabilir.

Maksimum uzunluk: 1024 karakter

## <a name="success"></a>Başarılı

Başarılı veya başarısız aramanın göstergesi. Bu alan gereklidir. Açıkça ayarlanmadığında `false` - bir istek başarılı olarak kabul edilir. Bu değeri, `false` işlemin özel durum veya döndürülen hata sonuç kodu tarafından kesintiye uğrayıp kesilmeyecek şekilde ayarlayın.

Web uygulamaları için Application Insights, yanıt kodu `400` `401`'ndan küçük veya eşit olduğunda bir isteği başarılı olarak tanımlar. Ancak, bu varsayılan eşlemenin uygulamanın anlambilimiyle eşleşmediği durumlar vardır. Yanıt `404` kodu, normal akışın bir parçası olabilecek "kayıt yok"u gösterebilir. Ayrıca kırık bir bağlantı gösterebilir. Kırık bağlantılar için, daha gelişmiş mantık bile uygulayabilirsiniz. Bozuk bağlantıları yalnızca bu bağlantılar aynı sitede bulunduğunda url referrer'ı analiz ederek hata olarak işaretleyebilirsiniz. Veya şirketin mobil uygulamasından erişildiğinde bunları hata olarak işaretleyin. Benzer `301` şekilde `302` ve yeniden yönlendirmeyi desteklemeyen istemciden erişildiğinde başarısızlığı gösterir.

Kısmen kabul edilen `206` içerik, genel bir isteğin başarısız olduğunu gösterebilir. Örneğin, Application Insights bitiş noktası tek bir istek olarak bir dizi telemetri öğesi alır. Toplu `206` işteki bazı öğeler başarıyla işlenmediğinde döndürür. Artan hızı `206` araştırılması gereken bir sorunu gösterir. Benzer mantık, `207` başarının ayrı yanıt kodlarının en kötüsü olabileceği Çoklu Durum için de geçerlidir.

[Blog gönderisinde](https://apmtips.com/blog/2016/12/03/request-success-and-response-code/)istek sonuç kodu ve durum kodu hakkında daha fazla bilgi edinebilirsiniz.

## <a name="custom-properties"></a>Özel özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Özel istek telemetrisi yazın](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest)
- Uygulama Öngörüleri türleri ve veri modeli için [veri modeline](data-model.md) bakın.
- Application Insights ile ASP.NET Core uygulamasını nasıl [yapılandırıştırmayı](../../azure-monitor/app/asp-net.md) öğrenin.
- Application Insights tarafından desteklenen [platformlara](../../azure-monitor/app/platforms.md) göz atın.
