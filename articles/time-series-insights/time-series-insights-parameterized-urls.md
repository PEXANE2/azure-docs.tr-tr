---
title: Parametreli URL'lerle özel görünümleri paylaşın - Azure Time Series Insights | Microsoft Dokümanlar
description: Azure Time Series Öngörüleri'nde özelleştirilmiş explorer görünümlerini kolayca paylaşmak için parametreli URL'ler oluşturmayı öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.topic: conceptual
ms.workload: big-data
ms.date: 02/04/2020
ms.custom: seodec18
ms.openlocfilehash: 9dfe499a7d6084a23fd71ab98db472befe71fc04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024373"
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Parametreli URL'yi kullanarak özel görünümü paylaşma

Zaman Serisi Öngörüler Gezgini'nde özel bir görünümü paylaşmak için, programlı bir şekilde özel görünümün parametreli URL'sini oluşturabilirsiniz.

Zaman Serisi Öngörüler Gezgini, deneyimdeki görünümleri doğrudan URL'den belirtmek için URL sorgu parametrelerini destekler. Örneğin, yalnızca URL'yi kullanarak hedef ortamı, arama koşulunu ve istenen zaman aralığını belirtebilirsiniz. Bir kullanıcı özelleştirilmiş URL'yi seçtiğinde, arabirim, Time Series Insights portalında doğrudan o varlığa bir bağlantı sağlar. Veri erişimi ilkeleri uygulanır.

> [!TIP]
> * [Ücretsiz Time Series Insights demosu](https://insights.timeseries.azure.com/samples)görüntüleyin.
> * Eşlik eden [Time Series Insights Explorer belgelerini](./time-series-insights-explorer.md) okuyun.

## <a name="environment-id"></a>Ortam Kimliği

`environmentId=<guid>` parametresi hedef ortam kimliğini belirtir. Veri erişimi FQDN'nin bir bileşenidir ve azure portalında ortama genel bakışın sağ üst köşesinde bulabilirsiniz. `env.timeseries.azure.com` bölümünden önce gelen her şey, bu değeri oluşturur.

Örnek bir ortam kimliği parametresi olarak `?environmentId=10000000-0000-0000-0000-100000000108` verilebilir.

## <a name="time"></a>Zaman

Parametreli URL ile mutlak veya göreli zaman değerleri belirtebilirsiniz.

### <a name="absolute-time-values"></a>Mutlak zaman değerleri

Mutlak zaman değerleri için, `from=<integer>` ve `to=<integer>` parametrelerini kullanın.

* `from=<integer>`, arama aralığının başlangıç zamanını gösteren JavaScript milisaniyesi cinsinden bir değerdir.
* `to=<integer>`, arama aralığının bitiş zamanını gösteren JavaScript milisaniyesi cinsinden bir değerdir.

> [!TIP]
> Tarihleri JavaScript milisaniyelerine kolayca çevirmek [için, Epoch & Unix Timestamp Converter'ı](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html)deneyin.

### <a name="relative-time-values"></a>Göreli zaman değerleri

Göreceli bir zaman değeri `relativeMillis=<value>`için, *değerin* API'den alınan en son zaman damgasından JavaScript milisaniyesinde olduğu yeri kullanın.

Örneğin, `&relativeMillis=3600000` en 60 dakikanın verilerini görüntüler.

Kabul edilen değerler Zaman Serisi Öngörüleri explorer **hızlı zaman** menüsüne karşılık gelir ve şunları içerir:

* `1800000`(Son 30 dakika)
* `3600000`(Son 60 dakika)
* `10800000`(Son 3 Saat)
* `21600000`(Son 6 Saat)
* `43200000`(Son 12 Saat)
* `86400000`(Son 24 Saat)
* `604800000`(Son 7 Gün)
* `2592000000`(Son 30 Saat)

### <a name="optional-parameters"></a>İsteğe bağlı parametreler

Parametre, `timeSeriesDefinitions=<collection of term objects>` Time Series Insights görünümünde görünecek yüklem terimlerini belirtir:

| Parametre | URL Öğesi | Açıklama |
| --- | --- | --- |
| **Adı** | `\<string>` | *Dönem* adı. |
| **splitBy** | `\<string>` | *Bölme ölçütü* sütunun adı. |
| **measureName** | `\<string>` | *Ölçü* sütununun adı. |
| **Yüklemi** | `\<string>` | Sunucu tarafı filtrelemesi için *where* yan tümcesi. |
| **useSum** | `true` | Ölçünüz için toplam kullanarak belirten isteğe bağlı bir parametre. |

> [!NOTE]
> Seçili `Events` **useSum** ölçüsü ise, varsayılan olarak sayım seçilir.  
> `Events` Seçili değilse, ortalama varsayılan olarak seçilir. |

* Anahtar `multiChartStack=<true/false>` değeri çifti grafikte istifleme sağlar.
* Anahtar `multiChartSameScale=<true/false>` değer çifti isteğe bağlı bir parametre içinde terimler arasında aynı Y ekseni ölçeğini sağlar.  
* Grafik `timeBucketUnit=<Unit>&timeBucketSize=<integer>` daha ayrıntılı veya daha düzgün, daha toplu bir görünüm sağlamak için aralık kaydırıcısını ayarlamanızı sağlar.  
* Parametre, `timezoneOffset=<integer>` grafiğin UTC'ye ofset olarak görüntülenmesi için saat dilimini ayarlamanızı sağlar.

| Çift(ler) | Açıklama |
| --- | --- |
| `multiChartStack=false` | `true`varsayılan olarak etkindir, bu nedenle yığına geçin. `false` |
| `multiChartStack=false&multiChartSameScale=true` | Terimler arasında aynı Y ekseni ölçeğini kullanmak için yığın oluşturmanın etkinleştirilmesi gerekir.  Varsayılan olarak, `false` bu nedenle `true` geçmek bu işlevselliği sağlar. |
| `timeBucketUnit=<Unit>&timeBucketSize=<integer>` | Birimler `days`= `hours` `minutes`, `seconds` `milliseconds`, , .  Her zaman birimin ilk harfini büyük yapın. </br> TimeBucketSize için istenilen sondayı geçirerek birim sayısını **tanımlayın.**  |
| `timezoneOffset=-<integer>` | Bu tamsayı her zaman milisaniye cinsindendir. |

> [!NOTE]
> **timeBucketUnit** değerleri 7 güne kadar düzeltilebilir.
> **timezoneOffset** değerleri ne UTC ne de yerel saattir.

### <a name="examples"></a>Örnekler

Zaman Serisi Öngörüleri ortamına URL parametresi olarak zaman serisi tanımları eklemek için aşağıdakileri ekleyin:

```URL parameter
&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},
{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Aşağıdakiler için örnek zaman serisi tanımlarını kullanın:

* Ortam kimliği
* Verilerin son 60 dakikası
* İsteğe bağlı parametreleri oluşturan terimler **(F1PressureID**, **F2TempStation**ve **F3VibrationPL)**

Görünüm için aşağıdaki parametreli URL'yi oluşturabilirsiniz:

```URL
https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

[![Zaman Serisi Öngörüler explorer parametreli URL](media/parameterized-url/share-parameterized-url.png)](media/parameterized-url/share-parameterized-url.png#lightbox)

> [!TIP]
> Yukarıdaki URL [örneğini kullanarak Explorer'ı](https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]) canlı olarak görün.

Yukarıdaki URL, parametrelendirilmiş Zaman Serisi Öngörüleri explorer görünümünü açıklar ve görüntüler. 

* Parametreli yüklemler.

  [![Zaman Serisi Öngörüler explorer parametreli yüklemler.](media/parameterized-url/share-parameterized-url-predicates.png)](media/parameterized-url/share-parameterized-url-predicates.png#lightbox)

* Paylaşılan tam grafik görünümü.

  [![Paylaşılan tam grafik görünümü.](media/parameterized-url/share-parameterized-url-full-chart.png)](media/parameterized-url/share-parameterized-url-full-chart.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

* C# kullanarak verileri nasıl [sorgulayarak sorgulamayı](time-series-insights-query-data-csharp.md)öğrenin.

* [Zaman Serisi Öngörüler Explorer](./time-series-insights-explorer.md)hakkında bilgi edinin.
