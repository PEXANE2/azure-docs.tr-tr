---
title: Parametreli URL 'lerle Azure Time Series Insights özel görünümleri paylaşma | Microsoft Docs
description: Bu makalede, bir müşteri görünümünün kolayca paylaşılabilmesi için Azure Time Series Insights parametreli URL 'Lerin nasıl geliştirileceği açıklanır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.topic: conceptual
ms.workload: big-data
ms.date: 10/03/2019
ms.custom: seodec18
ms.openlocfilehash: cad57e3e7e52ec291819110bab9d8d79f51e5a2f
ms.sourcegitcommit: 7868d1c40f6feb1abcafbffcddca952438a3472d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71958168"
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Parametreli URL kullanarak özel bir görünüm paylaşma

Time Series Insights Explorer 'da özel bir görünüm paylaşmak için programlı bir şekilde özel görünümün parametreli bir URL 'SI oluşturabilirsiniz.

Time Series Insights Gezgini, deneyimdeki görünümleri doğrudan URL 'den belirtmek için URL sorgu parametrelerini destekler. Örneğin, yalnızca URL 'YI kullanarak bir hedef ortam, bir arama koşulu ve istediğiniz zaman aralığını belirtebilirsiniz. Kullanıcı özelleştirilmiş URL 'yi seçtiğinde, arabirim Time Series Insights portalında doğrudan bu varlığa bir bağlantı sağlar. Veri erişim ilkeleri uygulanır.

> [!TIP]
> * Ücretsiz [Time Series Insights tanıtımı](https://insights.timeseries.azure.com/samples)' nı görüntüleyin.
> * Eşlik eden [Time Series Insights gezgin](./time-series-insights-explorer.md) belgelerini okuyun.

## <a name="environment-id"></a>Ortam KIMLIĞI

@No__t-0 parametresi, hedef ortam KIMLIĞINI belirtir. Bu, veri erişim FQDN 'sinin bir bileşenidir ve Azure portal ortama genel bakış ' ın sağ üst köşesinde bulabilirsiniz. @No__t,-0 ' dan önce gelen her şey.

Örnek ortam KIMLIĞI parametresi `?environmentId=10000000-0000-0000-0000-100000000108` ' dır.

## <a name="time"></a>Zaman

Parametreli bir URL ile mutlak veya göreli zaman değerleri belirtebilirsiniz.

### <a name="absolute-time-values"></a>Mutlak zaman değerleri

Mutlak zaman değerleri için `from=<integer>` ve `to=<integer>` parametrelerini kullanın.

* `from=<integer>`, arama yayılması için başlangıç zamanının JavaScript milisaniyelik değerindeki bir değerdir.
* `to=<integer>`, arama yayılması için bitiş saatinin JavaScript milisaniyesi cinsinden bir değerdir.

Bir tarih için JavaScript milisaniyesini belirlemek için bkz. [dönem & Unix zaman damgası Dönüştürücüsü](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html).

### <a name="relative-time-values"></a>Göreli zaman değerleri

Göreli bir zaman değeri için `relativeMillis=<value>` kullanın; burada *değer* , arka uçtaki en son verilerden JavaScript milisaniyedir.

Örneğin, `&relativeMillis=3600000` en son 60 dakikalık verileri görüntüler.

Kabul edilen değerler Time Series Insights Explorer **hızlı zaman** menüsüne karşılık gelir ve şunları içerir:

* `1800000` (son 30 dakika)
* `3600000` (son 60 dakika)
* `10800000` (son 3 saat)
* `21600000` (son 6 saat)
* `43200000` (son 12 saat)
* `86400000` (son 24 saat)
* `604800000` (son 7 gün)
* `2592000000` (son 30 saat)

### <a name="optional-parameters"></a>İsteğe bağlı parametreler

@No__t-0 parametresi, bir Time Series Insights görünümünün koşullarını belirtir:

| Parametre | URL öğesi | Açıklama |
| --- | --- | --- |
| **ada** | `\<string>` | *Terimin*adı. |
| **Bölünmüş** | `\<string>` | *Bölünecek*sütun adı. |
| **measureName** | `\<string>` | *Ölçünün*sütun adı. |
| **koşulunda** | `\<string>` | Sunucu tarafı filtreleme için *WHERE* yan tümcesi. |
| **useSum** | `true` | Ölçmenize yönelik Sum kullanımını belirten isteğe bağlı bir parametre. </br>  @No__t-0 seçili ölçü ise, varsayılan olarak sayı seçilidir.  </br>  @No__t-0 seçili değilse, varsayılan olarak ortalama seçilidir. |

* @No__t-0 anahtar-değer çifti grafikte yığınlama imkanı sunar.
* @No__t-0 anahtar-değer çifti, isteğe bağlı bir parametre içindeki terimler genelinde aynı Y ekseni ölçeğini sunar.  
* @No__t-0, grafiğin daha ayrıntılı veya daha yumuşak, daha toplanmış bir görünümünü sağlamak için Aralık kaydırıcısını ayarlamanıza olanak sağlar.  
* @No__t-0 parametresi, grafiğin saat dilimini UTC 'ye bir uzaklığa göre görüntülenecek şekilde ayarlamanıza olanak sağlar.

| Çift (ler) | Açıklama |
| --- | --- |
| `multiChartStack=false` | `true` varsayılan olarak etkindir, bu nedenle `false` ' i yığına geçirin. |
| `multiChartStack=false&multiChartSameScale=true` | Aynı Y ekseni ölçeğini şartlar genelinde kullanmak için yığınlama etkinleştirilmelidir.  Varsayılan olarak @no__t 0 ' dır, bu nedenle ' true ' geçirilmesi bu işlevselliği sunar. |
| `timeBucketUnit=<Unit>&timeBucketSize=<integer>` | Birimler = gün, saat, dakika, saniye, milisaniye.  Birimi her zaman büyük harfe Dönüştür. </br> TimeBucketSize için istenen tamsayıyı geçirerek birim sayısını tanımlayın.  Bu şekilde 7 güne kadar düzgünleştirmek istediğinizi göz önünde bulabilirsiniz.  |
| `timezoneOffset=-<integer>` | Tamsayı her zaman milisaniyedir. </br> Bu işlevsellik, Time Series Insights Explorer 'da etkinleştirildiğimiz miktardan biraz farklı olduğundan, yerel (tarayıcı saati) veya UTC 'yi seçmenizi sağlar. |

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

[![Time Series Insights Gezgini koşulları](media/parameterized-url/url1.png)](media/parameterized-url/url1.png#lightbox)

Tam Görünüm (grafik dahil):

[![Grafik görünümü](media/parameterized-url/url2.png)](media/parameterized-url/url2.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

* [ C#Kullanarak verileri sorgulamayı ](time-series-insights-query-data-csharp.md)öğrenin.

* [Time Series Insights Gezgini](./time-series-insights-explorer.md)hakkında bilgi edinin.
