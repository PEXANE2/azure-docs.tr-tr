---
title: Şekil olayları-Azure Time Series Insights | Microsoft Docs
description: En iyi uygulamalar hakkında bilgi edinin ve Azure saat öngörüleri önizlemesinde sorgulama için olayların nasıl şekillendireceğinizi öğrenin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/24/2020
ms.custom: seodec18
ms.openlocfilehash: 99a2f32c3f76d7fec475c9b299f7208b4db29cfe
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77650932"
---
# <a name="shape-events-with-azure-time-series-insights-preview"></a>Azure Time Series Insights Preview ile şekil olayları

Bu makalede, Azure Time Series Insights için JSON yüklerinizi şekillendirip önizleme sorgularının verimliliğini en üst düzeye çıkarmak için en iyi yöntemler tanımlanmaktadır.

## <a name="best-practices"></a>En iyi uygulamalar

Time Series Insights önizleme ortamınıza nasıl olay gönderileceğini dikkatle düşünmek en iyisidir. 

Genel en iyi uygulamalar şunlardır:

* Verileri ağ üzerinden mümkün olduğunca verimli bir şekilde gönderin.
* Verilerinizi, senaryonuza yönelik uygun şekilde daha fazla bilgi toplamanıza yardımcı olacak şekilde depolayın.

En iyi sorgu performansı için aşağıdaki kaydırma kurallarına uyar:

* Gereksiz özellikler göndermeyin. Time Series Insights, kullanıma göre faturaları önizleyin. Yalnızca Sorgulayabileceğiniz verileri depolamak ve işlemek en iyisidir.
* Statik veriler için örnek alanlarını kullanın. Bu uygulama, ağ üzerinden statik veri göndermekten kaçınmaya yardımcı olur. Zaman serisi modelinin bir bileşeni olan örnek alanları, genel kullanıma sunulan Time Series Insights hizmetindeki başvuru verileri gibi çalışır. Örnek alanları hakkında daha fazla bilgi edinmek için [zaman serisi modelini](./time-series-insights-update-tsm.md)okuyun.
* Boyut özelliklerini iki veya daha fazla olay arasında paylaşabilirsiniz. Bu uygulama, verileri ağ üzerinden daha verimli bir şekilde göndermenize yardımcı olur.
* Derin dizi iç içe geçme kullanmayın. Time Series Insights önizleme, nesneleri içeren iç içe geçmiş dizilerin en fazla iki düzeyini destekler. Önizleme Time Series Insights, iletilerde bulunan dizileri Özellik değeri çiftleriyle birden çok olaya düzleştirir.
* Tüm olaylar için yalnızca birkaç ölçü mevcutsa, bu ölçüleri aynı nesne içinde ayrı özellikler olarak göndermek daha iyidir. Bunları ayrı olarak göndermek olayların sayısını azaltır ve daha az olay işlenmesi gerektiğinden sorgu performansını iyileştirebilirler.

## <a name="column-flattening"></a>Sütun düzleştirme

Alma sırasında, iç içe geçmiş nesneler içeren yük, sütun adının bir ayırıcıya sahip tek bir değer olması için düzleştirilmez.

* Örneğin, aşağıdaki iç içe geçmiş JSON:

   ```JSON
   "data": {
        "flow": 1.0172575712203979,
   },
   ```

   Düzleştirildiğinde `data_flow` :.

> [!IMPORTANT]
> * Azure Time Series Insights önizleme, sütun kaldırma`_`için alt çizgi () kullanır.
> * Bunun yerine, dönemleri (`.`) kullanan genel kullanıma ait farkı dikkate alın.

Daha karmaşık senaryolar aşağıda gösterilmiştir.

#### <a name="example-1"></a>Örnek 1:

Aşağıdaki senaryoda ölçümleri (sinyalleri) Gönderen iki (veya daha fazla) cihaz vardır: *akış hızı*, *altyapı yağ baskısı*, *sıcaklık*ve *nem*.

Dış dizinin ortak boyut değerlerinin paylaşılan bir bölümünü içerdiği, tek bir Azure IoT Hub iletisi gönderilir (iletide bulunan iki cihaz girişini aklınızda bulunur).

```JSON
[
  {
    "deviceId":"FXXX",
    "timestamp":"2018-01-17T01:17:00Z",
    "series":[
      {
        "Flow Rate ft3/s":1.0172575712203979,
        "Engine Oil Pressure psi ":34.7
      },
      {
        "Flow Rate ft3/s":2.445906400680542,
        "Engine Oil Pressure psi ":49.2
      }
    ]
  },
  {
    "deviceId":"FYYY",
    "timestamp":"2018-01-17T01:18:00Z",
    "series":[
      {
        "Flow Rate ft3/s":0.58015072345733643,
        "Engine Oil Pressure psi ":22.2
      }
    ]
  }
]
```

**Dersler**

* Örnek JSON, iletinin verimliliğini artırmak için [zaman serisi örnek](./time-series-insights-update-tsm.md#time-series-model-instances) verilerini kullanan bir dış dizi içerir. Zaman serisi örnekleri cihaz meta verilerinin değişmeme olasılığı olsa da, genellikle veri analizi için yararlı özellikler sağlar.

* JSON, iki veya daha fazla iletiyi (her cihazdan biri) zaman içinde bant genişliğine göre tek bir yük ile birleştirir.

* Her bir cihaza yönelik ayrı seriler veri noktaları, her bir cihaz için sürekli olarak güncelleştirmeleri akışa almak gereksinimini azaltan tek bir **seri** özniteliğinde birleştirilir.

> [!TIP]
> Veri göndermek ve Telemetriyi daha verimli hale getirmek için gereken ileti sayısını azaltmak için, ortak boyut değerlerini ve zaman serisi örnek meta verilerini tek bir JSON yükünde toplu olarak toplu hale getirin.

#### <a name="time-series-instance"></a>Zaman serisi örneği 

Şimdi de JSON 'nizi daha iyi şekilde şekillendirmek için [zaman serisi örneğini](./time-series-insights-update-tsm.md#time-series-model-instances) kullanma bölümüne daha yakından bakalım. 

> [!NOTE]
> Aşağıdaki [zaman serisi kimlikleri](./time-series-insights-update-how-to-id.md) *deviceds*' dir.

```JSON
[
  {
    "timeSeriesId":[
      "FXXX"
    ],
    "typeId":"17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds":[
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description":null,
    "instanceFields":{
      "L1":"REVOLT SIMULATOR",
      "L2":"Battery System"
    }
  },
  {
    "timeSeriesId":[
      "FYYY"
    ],
    "typeId":"17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds":[
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description":null,
    "instanceFields":{
      "L1":"COMMON SIMULATOR",
      "L2":"Battery System"
    }
  }
]
```

Time Series Insights önizlemesi, sorgu süresi boyunca bir tabloyu (düzleştirme sonrasında) birleştirir. Tablo, **türü**gibi ek sütunlar içerir.

| deviceId  | Tür | L1 | L2 | timestamp | series_Flow Rate ft3/sn | series_Engine yağ basıncı psi |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | SIMÜLATÖR | Pil sistemi | 2018-01-17T01:17:00Z |   1.0172575712203979 |    34,7 |
| `FXXX` | Default_Type | SIMÜLATÖR |   Pil sistemi |    2018-01-17T01:17:00Z | 2.445906400680542 |  49,2 |
| `FYYY` | LINE_DATA ORTAK | SIMÜLATÖR |    Pil sistemi |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22,2 |

> [!NOTE]
>  Yukarıdaki tablo, [zaman serisi önizleme Gezgininde](./time-series-insights-update-explorer.md)sorgu görünümünü temsil eder.

**Dersler**

* Yukarıdaki örnekte, statik özellikler, ağ üzerinden gönderilen verileri iyileştirmek için Time Series Insights önizlemede depolanır.
* Time Series Insights Önizleme verileri, örnekte tanımlanan zaman serisi KIMLIĞIYLE sorgu zamanına birleştirilir.
* İki iç içe geçme katmanı kullanılır. Bu sayı Time Series Insights önizlemenin desteklediği en büyük sayıdır. Derin iç içe dizilerden kaçınmak önemlidir.
* Birkaç ölçü olduğundan, bunlar aynı nesne içinde ayrı özellikler olarak gönderilirler. Örnekte, **Series_Flow değerlendirin**, **yağ basıncı Series_Engine**ve **series_Flow Rate ft3/s** benzersiz sütunlardır.

>[!IMPORTANT]
> Örnek alanları telemetri ile depolanmaz. Bunlar, zaman serisi modelinde meta verilerle birlikte depolanır.

#### <a name="example-2"></a>Örnek 2:

Aşağıdaki JSON 'ı göz önünde bulundurun:

```JSON
{
  "deviceId": "FXXX",
  "timestamp": "2019-01-18T01:17:00Z",
  "data": {
        "flow": 1.0172575712203979,
    },
  "data_flow" : 1.76435072345733643
}
```

Yukarıdaki örnekte, düzleştirilmiş `data["flow"]` özellik `data_flow` özelliği ile bir adlandırma çarpışması sunmalıdır.

Bu durumda, *en son* Özellik değeri önceki bir değerin üzerine yazılır. 

> [!TIP]
> Daha fazla yardım için Time Series Insights ekibine başvurun!

> [!WARNING] 
> * Düzleştirme veya başka bir mekanizma nedeniyle aynı (tekil) olay yükünde yinelenen özelliklerin mevcut olduğu durumlarda, en son > Özellik değeri, önceki değerlerin yazıldığı şekilde depolanır.
> * Birleşik olayların serisi bir diğerinin üzerine yazılmaz.

## <a name="next-steps"></a>Sonraki adımlar

* Bu yönergeleri uygulamaya koymak için [Azure Time Series Insights Preview sorgu söz dizimini](./time-series-insights-query-data-csharp.md)okuyun. Veri erişimi için Time Series Insights [Preview REST API](https://docs.microsoft.com/rest/api/time-series-insights/preview) sorgu sözdizimi hakkında daha fazla bilgi edineceksiniz.

* JSON en iyi yöntemlerini, [zaman serisi modeli](./time-series-insights-update-how-to-tsm.md)ile birleştirin.
