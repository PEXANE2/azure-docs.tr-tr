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
ms.date: 10/18/2019
ms.custom: seodec18
ms.openlocfilehash: e862cd604263c5125259344baf2e990ad56a3291
ms.sourcegitcommit: 9a4296c56beca63430fcc8f92e453b2ab068cc62
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2019
ms.locfileid: "72675851"
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

## <a name="time"></a>Zaman

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

* `1800000` (son 30 dakika)
* `3600000` (son 60 dakika)
* `10800000` (son 3 saat)
* `21600000` (son 6 saat)
* `43200000` (son 12 saat)
* `86400000` (son 24 saat)
* `604800000` (son 7 gün)
* `2592000000` (son 30 saat)

### <a name="optional-parameters"></a>İsteğe bağlı parametreler

@No__t_0 parametresi bir Time Series Insights görünümünün koşullarını belirtir:

| Parametre | URL öğesi | Açıklama |
| --- | --- | --- |
| **ada** | `\<string>` | *Dönem* adı. |
| **Bölünmüş** | `\<string>` | *Bölme ölçütü* sütunun adı. |
| **measureName** | `\<string>` | *Ölçü* sütununun adı. |
| **koşulunda** | `\<string>` | Sunucu tarafı filtrelemesi için *where* yan tümcesi. |
| **useSum** | `true` | Ölçmenize yönelik Sum kullanımını belirten isteğe bağlı bir parametre. </br>  @No__t_0 seçili ölçü ise, varsayılan olarak sayı seçilidir.  </br>  @No__t_0 seçili değilse, varsayılan olarak ortalama seçilidir. |

* @No__t_0 anahtar-değer çifti grafikte yığınlama imkanı sunar.
* @No__t_0 anahtar-değer çifti, isteğe bağlı bir parametre içindeki terimleri kapsayan aynı Y ekseni ölçeğini sunar.  
* @No__t_0, grafiğin daha ayrıntılı veya daha yumuşak, daha toplanmış bir görünümünü sağlamak için Aralık kaydırıcısını ayarlamanıza olanak sağlar.  
* @No__t_0 parametresi, grafiğin saat dilimini UTC 'ye bir uzaklığa göre görüntülenecek şekilde ayarlamanıza olanak sağlar.

| Çift (ler) | Açıklama |
| --- | --- |
| `multiChartStack=false` | `true` varsayılan olarak etkinleştirilmiştir `false` yığına geçirin. |
| `multiChartStack=false&multiChartSameScale=true` | Terimler arasında aynı Y ekseni ölçeğini kullanmak için yığın oluşturmanın etkinleştirilmesi gerekir.  Varsayılan olarak `false`, bu nedenle ' true ' geçirilmesi bu işlevselliği sunar. |
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

[![Time Series Insights Gezgini koşulları](media/parameterized-url/url1.png)](media/parameterized-url/url1.png#lightbox)

Tam Görünüm (grafik dahil):

[![Chart görünümü](media/parameterized-url/url2.png)](media/parameterized-url/url2.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

* [ C#Kullanarak verileri sorgulamayı ](time-series-insights-query-data-csharp.md)öğrenin.

* [Time Series Insights Gezgini](./time-series-insights-explorer.md)hakkında bilgi edinin.
