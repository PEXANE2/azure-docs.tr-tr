---
title: Parametreli URL 'lerle özel görünümleri paylaşma-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights ' de özelleştirilmiş gezgin görünümlerini kolayca paylaşmak için parametreli URL 'Ler oluşturmayı öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.topic: conceptual
ms.workload: big-data
ms.date: 04/15/2020
ms.custom: seodec18
ms.openlocfilehash: 10616c8003d9bbbe42cb70bd1bac4193044907c0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81416989"
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Parametreli URL'yi kullanarak özel görünümü paylaşma

Time Series Insights Explorer 'da özel bir görünüm paylaşmak için programlı bir şekilde özel görünümün parametreli bir URL 'SI oluşturabilirsiniz.

Time Series Insights Gezgini, deneyimdeki görünümleri doğrudan URL 'den belirtmek için URL sorgu parametrelerini destekler. Örneğin, yalnızca URL'yi kullanarak hedef ortamı, arama koşulunu ve istenen zaman aralığını belirtebilirsiniz. Kullanıcı özelleştirilmiş URL 'yi seçtiğinde, arabirim Time Series Insights portalında doğrudan bu varlığa bir bağlantı sağlar. Veri erişimi ilkeleri uygulanır.

> [!TIP]
> * Ücretsiz [Time Series Insights tanıtımı](https://insights.timeseries.azure.com/samples)' nı görüntüleyin.
> * Eşlik eden [Time Series Insights gezgin](./time-series-insights-explorer.md) belgelerini okuyun.

## <a name="environment-id"></a>Ortam Kimliği

`environmentId=<guid>` parametresi hedef ortam kimliğini belirtir. Bu, veri erişim FQDN 'sinin bir bileşenidir ve Azure portal ortama genel bakış ' ın sağ üst köşesinde bulabilirsiniz. Bundan önce gelen her şey `env.timeseries.azure.com` .

Örnek bir ortam kimliği parametresi olarak `?environmentId=10000000-0000-0000-0000-100000000108` verilebilir.

## <a name="time"></a>Saat

Parametreli URL ile mutlak veya göreli zaman değerleri belirtebilirsiniz.

### <a name="absolute-time-values"></a>Mutlak zaman değerleri

Mutlak zaman değerleri için, `from=<integer>` ve `to=<integer>` parametrelerini kullanın.

* `from=<integer>`, arama aralığının başlangıç zamanını gösteren JavaScript milisaniyesi cinsinden bir değerdir.
* `to=<integer>`, arama aralığının bitiş zamanını gösteren JavaScript milisaniyesi cinsinden bir değerdir.

> [!TIP]
> Tarihleri JavaScript milisaniyeye kolayca çevirmek için, [dönem & Unix zaman damgası dönüştürücüsünü](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html)deneyin.

### <a name="relative-time-values"></a>Göreli zaman değerleri

Göreli bir zaman değeri için, `relativeMillis=<value>` *değeri* API 'den alınan en son zaman damgasından JavaScript milisaniyedir ' i kullanın.

Örneğin, `&relativeMillis=3600000` en 60 dakikanın verilerini görüntüler.

Kabul edilen değerler Time Series Insights Explorer **hızlı zaman** menüsüne karşılık gelir ve şunları içerir:

* `1800000`(Son 30 dakika)
* `3600000`(Son 60 dakika)
* `10800000`(Son 3 saat)
* `21600000`(Son 6 saat)
* `43200000`(Son 12 saat)
* `86400000`(Son 24 saat)
* `604800000`(Son 7 gün)
* `2592000000`(Son 30 saat)

### <a name="optional-parameters"></a>İsteğe bağlı parametreler

`timeSeriesDefinitions=<collection of term objects>`Parametresi bir Time Series Insights görünümünde görünecek koşul koşullarını belirtir:

| Parametre | URL öğesi | Description |
| --- | --- | --- |
| **ada** | `\<string>` | *Dönem* adı. |
| **Bölünmüş** | `\<string>` | *Bölme ölçütü* sütunun adı. |
| **measureName** | `\<string>` | *Ölçü* sütununun adı. |
| **koşulunda** | `\<string>` | Sunucu tarafı filtrelemesi için *where* yan tümcesi. |
| **useSum** | `true` | Ölçmenize yönelik Sum kullanımını belirten isteğe bağlı bir parametre. |

> [!NOTE]
> `Events`Seçili **usesum** ölçüsünde, varsayılan olarak sayı seçilidir.  
> `Events`Seçili değilse, varsayılan olarak ortalama seçilidir. |

* `multiChartStack=<true/false>`Anahtar-değer çifti grafikte yığınlama imkanı sunar.
* `multiChartSameScale=<true/false>`Anahtar-değer çifti, isteğe bağlı bir parametre içindeki terimleri kapsayan aynı Y ekseni ölçeğini sunar.  
* , `timeBucketUnit=<Unit>&timeBucketSize=<integer>` Grafiğin daha ayrıntılı veya daha yumuşak, daha toplanmış bir görünümünü sağlamak için Aralık kaydırıcısını ayarlamanıza olanak sağlar.  
* `timezoneOffset=<integer>`Parametresi, grafiğin saat DILIMINI UTC 'ye bir uzaklığa göre görüntülenecek şekilde ayarlamanıza olanak sağlar.

| Çift (ler) | Description |
| --- | --- |
| `multiChartStack=false` | `true`Varsayılan olarak etkin olduğundan yığına geçirin `false` . |
| `multiChartStack=false&multiChartSameScale=true` | Terimler arasında aynı Y ekseni ölçeğini kullanmak için yığın oluşturmanın etkinleştirilmesi gerekir.  `false`Bu, varsayılan olarak, geçirme `true` Bu işlevselliği sunar. |
| `timeBucketUnit=<Unit>&timeBucketSize=<integer>` | Birimler = `days` , `hours` , `minutes` , `seconds` , `milliseconds` .  Her zaman birimin ilk harfini büyük yapın. </br> **TimeBucketSize**için istenen tamsayıyı geçirerek birim sayısını tanımlayın.  |
| `timezoneOffset=-<integer>` | Bu tamsayı her zaman milisaniye cinsindendir. |

> [!NOTE]
> **timeBucketUnit** değerleri 7 güne kadar düzgünleştirilir.
> **timezonespan** değeri UTC veya yerel saat değil.

### <a name="examples"></a>Örnekler

Bir Time Series Insights ortamına URL parametresi olarak zaman serisi tanımları eklemek için, şunu ekleyin:

```URL parameter
&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},
{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

İçin örnek zaman serisi tanımlarını kullanın:

* Ortam KIMLIĞI
* Son 60 dakikalık veriler
* İsteğe bağlı parametreleri oluşturan koşullar (**F1PressureID**, **F2TempStation**ve **F3VibrationPL**)

Bir görünüm için aşağıdaki parametreli URL 'YI oluşturabilirsiniz:

```URL
https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

[![Time Series Insights Explorer parametreli URL 'SI](media/parameterized-url/share-parameterized-url.png)](media/parameterized-url/share-parameterized-url.png#lightbox)

> [!TIP]
> Yukarıdaki [URL örneğini kullanarak](https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]) gezgin Live ' a bakın.

Yukarıdaki URL tanımlar ve parametreli Time Series Insights Gezgin görünümünü görüntüler. 

* Parametreli koşullar.

  [![Time Series Insights Explorer parametreli koşullar.](media/parameterized-url/share-parameterized-url-predicates.png)](media/parameterized-url/share-parameterized-url-predicates.png#lightbox)

* Paylaşılan tam grafik görünümü.

  [![Paylaşılan tam grafik görünümü.](media/parameterized-url/share-parameterized-url-full-chart.png)](media/parameterized-url/share-parameterized-url-full-chart.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

* [C# kullanarak verileri sorgulamayı](time-series-insights-query-data-csharp.md)öğrenin.

* [Time Series Insights Gezgini](./time-series-insights-explorer.md)hakkında bilgi edinin.
