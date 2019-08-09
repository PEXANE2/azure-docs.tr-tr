---
title: Parametreli URL'lerle Azure Time Series Insights özel görünümlerini paylaşma | Microsoft Docs
description: Bu makalede, özelleştirilmiş görünümün kolayca paylaşılabilmesi için Azure Time Series Insights'ta parametreli URL'lerin nasıl geliştirileceği açıklanır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.topic: conceptual
ms.workload: big-data
ms.date: 08/08/2019
ms.custom: seodec18
ms.openlocfilehash: ce8c74e5c194dbd971ecb65659c4fc8a7150146d
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68882914"
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Parametreli URL'yi kullanarak özel görünümü paylaşma

Time Series Insights Explorer 'da özel bir görünüm paylaşmak için programlı bir şekilde özel görünümün parametreli bir URL 'SI oluşturabilirsiniz.

Time Series Insights Gezgini, deneyimdeki görünümleri doğrudan URL 'den belirtmek için URL sorgu parametrelerini destekler. Örneğin, yalnızca URL'yi kullanarak hedef ortamı, arama koşulunu ve istenen zaman aralığını belirtebilirsiniz. Kullanıcı özelleştirilmiş URL 'yi seçtiğinde, arabirim Time Series Insights portalında doğrudan bu varlığa bir bağlantı sağlar. Veri erişimi ilkeleri uygulanır.

> [!TIP]
> * Ücretsiz [Time Series Insights tanıtımı](https://insights.timeseries.azure.com/samples)' nı görüntüleyin.
> * Eşlik eden [Time Series Insights gezgin](./time-series-insights-explorer.md) belgelerini okuyun.

## <a name="environment-id"></a>Ortam Kimliği

`environmentId=<guid>` parametresi hedef ortam kimliğini belirtir. Bu, veri erişim FQDN 'sinin bir bileşenidir ve Azure portal ortama genel bakış ' ın sağ üst köşesinde bulabilirsiniz. `env.timeseries.azure.com` bölümünden önce gelen her şey, bu değeri oluşturur.

Örnek bir ortam kimliği parametresi olarak `?environmentId=10000000-0000-0000-0000-100000000108` verilebilir.

## <a name="time"></a>Time

Parametreli URL ile mutlak veya göreli zaman değerleri belirtebilirsiniz.

### <a name="absolute-time-values"></a>Mutlak zaman değerleri

Mutlak zaman değerleri için, `from=<integer>` ve `to=<integer>` parametrelerini kullanın.

* `from=<integer>`, arama aralığının başlangıç zamanını gösteren JavaScript milisaniyesi cinsinden bir değerdir.
* `to=<integer>`, arama aralığının bitiş zamanını gösteren JavaScript milisaniyesi cinsinden bir değerdir.

Tarihe ilişkin JavaScript milisaniyesini belirlemek için bkz. [Epoch ve Unix Zaman Damgası Dönüştürücüsü](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html).

### <a name="relative-time-values"></a>Göreli zaman değerleri

Göreli bir zaman değeri için `relativeMillis=<value>` kullanın; burada *değer*, arka uçtaki en son verilerden gelen JavaScript milisaniyesi cinsinden bir değerdir.

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

`timeSeriesDefinitions=<collection of term objects>` Parametresi Time Series Insights görünümün koşullarını belirtir:

| Parametre | URL öğesi | Açıklama |
| --- | --- | --- |
| **name** | `\<string>` | *Dönem* adı. |
| **Bölünmüş** | `\<string>` | *Bölme ölçütü* sütunun adı. |
| **measureName** | `\<string>` | *Ölçü* sütununun adı. |
| **koşulunda** | `\<string>` | Sunucu tarafı filtrelemesi için *where* yan tümcesi. |
| **useSum** | `true` | Ölçmenize yönelik Sum kullanımını belirten isteğe bağlı bir parametre. </br>  Seçili ölçü ise, varsayılan olarak sayım seçilidir. `Events`  </br>  `Events` Seçili değilse, varsayılan olarak ortalama seçilidir. |

* `multiChartStack=<true/false>` Anahtar-değer çifti grafikte yığınlama imkanı sunar.
* `multiChartSameScale=<true/false>` Anahtar-değer çifti, isteğe bağlı bir parametre içindeki terimleri kapsayan aynı Y ekseni ölçeğini sunar.  
* , `timeBucketUnit=<Unit>&timeBucketSize=<integer>` Grafiğin daha ayrıntılı veya daha yumuşak, daha toplanmış bir görünümünü sağlamak için Aralık kaydırıcısını ayarlamanıza olanak sağlar.  
* `timezoneOffset=<integer>` Parametresi, grafiğin saat dilimini UTC 'ye bir uzaklığa göre görüntülenecek şekilde ayarlamanıza olanak sağlar.

| Çift (ler) | Açıklama |
| --- | --- |
| `multiChartStack=false` | `true`Varsayılan olarak etkin olduğundan yığına geçirin `false` . |
| `multiChartStack=false&multiChartSameScale=true` | Terimler arasında aynı Y ekseni ölçeğini kullanmak için yığın oluşturmanın etkinleştirilmesi gerekir.  Bu, varsayılan olarak ' true ' geçirilmesi bu işlevselliği sunar. `false` |
| `timeBucketUnit=<Unit>&timeBucketSize=<integer>` | Birimler = gün, saat, dakika, saniye, milisaniye.  Her zaman birimin ilk harfini büyük yapın. </br> timeBucketSize için istediğiniz tamsayıyı geçererek birim sayısını tanımlayın.  En fazla 7 güne kadar düzleştirebilirsiniz.  |
| `timezoneOffset=-<integer>` | Bu tamsayı her zaman milisaniye cinsindendir. </br> Bu işlevsellik, Time Series Insights Explorer 'da etkinleştirildiğimiz miktardan biraz farklı olduğundan, yerel (tarayıcı saati) veya UTC 'yi seçmenizi sağlar. |

### <a name="examples"></a>Örnekler

Bir Time Series Insights ortamına URL parametresi olarak zaman serisi tanımları eklemek için, şunu ekleyin:

```plaintext
&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},
{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

İçin örnek zaman serisi tanımlarını kullanın:

* Ortam KIMLIĞI
* Son 60 dakikalık veriler
* İsteğe bağlı parametreleri oluşturan koşullar (F1PressureID, F2TempStation ve F3VibrationPL)

Bir görünüm için aşağıdaki parametreli URL 'YI oluşturabilirsiniz:

```plaintext
https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

> [!TIP]
> [URL 'yi kullanarak](https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}])gezgin Live 'a bakın.

Yukarıdaki URL Time Series Insights Gezgin görünümünü tanımlar ve oluşturur:

[![Time Series Insights Explorer koşulları](media/parameterized-url/url1.png)](media/parameterized-url/url1.png#lightbox)

Tam Görünüm (grafik dahil):

[![Grafik görünümü](media/parameterized-url/url2.png)](media/parameterized-url/url2.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

* [ C#Kullanarak verileri sorgulamayı ](time-series-insights-query-data-csharp.md)öğrenin.

* [Time Series Insights Gezgini](./time-series-insights-explorer.md)hakkında bilgi edinin.
