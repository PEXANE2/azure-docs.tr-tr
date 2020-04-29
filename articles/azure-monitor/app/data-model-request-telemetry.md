---
title: İstek telemetrisi için veri modeli-Azure Application Insights
description: İstek telemetrisi için veri modeli Application Insights
ms.topic: conceptual
ms.date: 01/07/2019
ms.reviewer: sergkanz
ms.openlocfilehash: d8a28063bf6780c3cace4ead81e289779b95eb9a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77671911"
---
# <a name="request-telemetry-application-insights-data-model"></a>İstek telemetrisi: Application Insights veri modeli

İstek telemetrisi öğesi ( [Application Insights](../../azure-monitor/app/app-insights-overview.md)), uygulamanıza yönelik bir dış istek tarafından tetiklenen mantıksal yürütme dizisini temsil eder. Her istek yürütmesi benzersiz `ID` ve `url` tüm yürütme parametrelerini içerecek şekilde tanımlanır. İstekleri mantıksal `name` olarak gruplandırabilir ve bu isteğin `source` sayısını tanımlayabilirsiniz. Kod yürütme, `success` veya `fail` ile sonuçlanabilir. `duration` Başarılı ve başarısız yürütmeler, tarafından `resultCode`daha fazla gruplandırılabilir. Zarf düzeyinde tanımlanan istek telemetrisi için başlangıç zamanı.

İstek telemetrisi, özel `properties` ve `measurements`kullanan standart genişletilebilirlik modelini destekler.

## <a name="name"></a>Adı

İsteğin adı, isteği işlemek için alınan kod yolunu temsil eder. İsteklerin daha iyi gruplanmasına izin vermek için düşük kardinalite değeri. HTTP istekleri için, gerçek `GET /values/{id}` `id` değer olmadan gıbı http yöntemini ve URL yol şablonunu temsil eder.

Application Insights Web SDK, "olduğu gibi" istek adını harf büyük harfe göre gönderir. Kullanıcı arabirimindeki gruplandırma, büyük/küçük harfe `GET /Home/Index` duyarlıdır; bu nedenle `GET /home/INDEX` , genellikle aynı denetleyici ve eylem yürütmeye neden olur. Bunun nedeni, genel olarak URL 'lerin [büyük/küçük harfe duyarlıdır](https://www.w3.org/TR/WD-html40-970708/htmlweb.html). Büyük harfle yazılan URL 'ler için tümünün `404` olup olmadığını görmek isteyebilirsiniz. İstek adı koleksiyonu hakkında daha fazla bilgi için ASP.NET Web SDK tarafından [blog gönderisine](https://apmtips.com/blog/2015/02/23/request-name-and-url/)erişebilirsiniz.

Maksimum Uzunluk: 1024 karakter

## <a name="id"></a>Kimlik

İstek çağrısı örneğinin tanımlayıcısı. İstek ve diğer telemetri öğeleri arasındaki bağıntı için kullanılır. KIMLIK, genel olarak benzersiz olmalıdır. Daha fazla bilgi için bkz. [bağıntı](../../azure-monitor/app/correlation.md) sayfası.

Maksimum Uzunluk: 128 karakter

## <a name="url"></a>Url

Tüm sorgu dizesi parametreleriyle birlikte istek URL 'SI.

Maksimum Uzunluk: 2048 karakter

## <a name="source"></a>Kaynak

İsteğin kaynağı. Örnekanahtarın, arayanın veya IP adresinin izleme anahtarıdır. Daha fazla bilgi için bkz. [bağıntı](../../azure-monitor/app/correlation.md) sayfası.

Maksimum Uzunluk: 1024 karakter

## <a name="duration"></a>Süre

Şu biçimdeki istek süresi: `DD.HH:MM:SS.MMMMMM`. Pozitif ve günlerden `1000` küçük olmalıdır. İstek telemetrisi, başlangıcı ve bitişi olan işlemi temsil ettiğinden bu alan gereklidir.

## <a name="response-code"></a>Yanıt kodu

İstek yürütmenin sonucu. HTTP istekleri için HTTP durum kodu. Diğer istek türleri `HRESULT` için değer veya özel durum türü olabilir.

Maksimum Uzunluk: 1024 karakter

## <a name="success"></a>Başarılı

Başarılı veya başarısız çağrı göstergesi. Bu alan gereklidir. Açıkça olarak `false` ayarlanmamışsa, istek başarılı olarak kabul edilir. İşlem özel durum tarafından `false` kesintiye uğrarsa veya hata sonucu kodu döndürdüğünden, bu değeri olarak ayarlayın.

Web uygulamaları için, yanıt kodu değerinden `400` küçük veya buna eşit olduğunda isteği başarılı olarak tanımlayın Application Insights. `401` Ancak, bu varsayılan eşlemenin uygulamanın anlam ile eşleşmemesi durumunda durumlar vardır. Yanıt kodu `404` , normal akışın bir parçası olabilen "kayıt yok" anlamına gelebilir. Aynı zamanda bozuk bir bağlantı olduğunu gösteriyor olabilir. Bozuk bağlantılar için daha gelişmiş mantık da uygulayabilirsiniz. Hatalı bağlantıları, yalnızca bu bağlantılar aynı sitede bulunduğunda, URL başvuran ' i çözümleyerek hatalara göre işaretleyebilirsiniz. Ya da şirketin mobil uygulamasından erişildiğinde bunları hatalara göre işaretleyin. Benzer `301` şekilde `302` , yeniden yönlendirmeyi desteklemeyen istemciden erişildiğinde hata olduğunu gösterir.

Kısmen kabul edilen `206` içerik, genel bir isteğin bir başarısızlığını gösterebilir. Örneğin, Application Insights uç noktası tek bir istek olarak telemetri öğelerini toplu olarak alır. Toplu işteki `206` bazı öğelerin başarıyla işlenmediği zaman döndürür. Artış oranı, `206` Araştırılması gereken bir sorunu gösterir. Benzer mantık, başarıyı `207` ayrı yanıt kodlarının en kötü olabileceği çok durum için geçerlidir.

İstek sonucu kodu ve durum kodu hakkında daha fazla bilgi için [blog gönderisine](https://apmtips.com/blog/2016/12/03/request-success-and-response-code/)erişebilirsiniz.

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Özel istek telemetrisi yazma](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest)
- Application Insights türleri ve veri modeli için [veri modeli](data-model.md) 'ne bakın.
- ASP.NET Core uygulamayı Application Insights ile [yapılandırmayı](../../azure-monitor/app/asp-net.md) öğrenin.
- Application Insights tarafından desteklenen [platformları](../../azure-monitor/app/platforms.md) inceleyin.
