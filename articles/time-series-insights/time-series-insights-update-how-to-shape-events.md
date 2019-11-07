---
title: Azure Time Series Insights Preview ile şekil olayları | Microsoft Docs
description: Azure Time Series Insights Preview ile olayları nasıl şekillendirebileceğinizi anlayın.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 10/31/2019
ms.custom: seodec18
ms.openlocfilehash: 8b9dd10a4017d821794af037e502c784b10cd62f
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73585271"
---
# <a name="shape-events-with-azure-time-series-insights-preview"></a>Azure Time Series Insights Preview ile şekil olayları

Bu makale, JSON dosyanızı alma için şekillendirmenize ve Azure Time Series Insights Preview sorgularınızın verimliliğini en üst düzeye çıkarmaya yardımcı olur.

## <a name="best-practices"></a>En iyi uygulamalar

Time Series Insights önizlemeye nasıl olay gönderileceğini düşünün. Yani, her zaman şunları yapmalısınız:

* Verileri ağ üzerinden mümkün olduğunca verimli bir şekilde gönderin.
* Verilerinizi, senaryonuza yönelik uygun şekilde daha fazla bilgi toplamanıza yardımcı olacak şekilde depolayın.

En iyi sorgu performansı için aşağıdakileri yapın:

* Gereksiz özellikler göndermeyin. Time Series Insights önizlemesi kullanımınıza sunuldu. Sorgulayabileceğiniz verilerin depolanması ve işlenmesi en iyisidir.
* Statik veriler için örnek alanlarını kullanın. Bu uygulama, ağ üzerinden statik veri göndermekten kaçınmanıza yardımcı olur. Zaman serisi modelinin bir bileşeni olan örnek alanları, genel kullanıma sunulan Time Series Insights hizmetindeki başvuru verileri gibi çalışır. Örnek alanları hakkında daha fazla bilgi için bkz. [zaman serisi modeli](./time-series-insights-update-tsm.md).
* Boyut özelliklerini iki veya daha fazla olay arasında paylaşabilirsiniz. Bu uygulama, verileri ağ üzerinden daha verimli bir şekilde göndermenize yardımcı olur.
* Derin dizi iç içe geçme kullanmayın. Time Series Insights önizleme, nesneleri içeren iç içe geçmiş dizilerin en fazla iki düzeyini destekler. Önizleme Time Series Insights, iletilerde bulunan dizileri Özellik değeri çiftleriyle birden çok olaya düzleştirir.
* Tüm olaylar için yalnızca birkaç ölçü mevcutsa, bu ölçüleri aynı nesne içinde ayrı özellikler olarak göndermek daha iyidir. Bunları ayrı olarak göndermek olayların sayısını azaltır ve daha az olay işlenmesi gerektiğinden sorgu performansını iyileştirebilirler.

Alma sırasında, iç içe geçme içeren yük, sütun adı bir ayırıcıya sahip tek bir değer olacak şekilde düzleştirilir. Time Series Insights önizlemesi, kaldırma için alt çizgi kullanır. Bu, ürünün kullanıldığı dönem için GA sürümünden bir değişiklik olduğunu unutmayın. Önizleme sırasında, aşağıdaki ikinci örnekte gösterilen düzleştirme etrafında bir desteklenmediği uyarısıyla vardır.

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, iki veya daha fazla cihazın ölçüm veya sinyal gönderen bir senaryoya dayalıdır. Ölçümler veya sinyaller *Flow ücreti*, *altyapı yağ baskısı*, *sıcaklık*ve *nem*olabilir.

Örnekte, dış dizinin ortak boyut değerlerinin paylaşılan bir bölümünü içerdiği tek bir Azure IoT Hub iletisi vardır. Dış dizi, iletinin verimliliğini artırmak için zaman serisi örnek verilerini kullanır. 

Zaman serisi örneği, cihaz meta verilerini içerir. Bu meta veriler her olayla değişmez, ancak veri analizi için yararlı özellikler sağlıyor. Kablo üzerinden gönderilen baytlara kaydetmek ve iletiyi daha verimli hale getirmek için ortak boyut değerlerini toplu olarak oluşturmayı ve zaman serisi örnek meta verilerini kullanmayı düşünün.

### <a name="example-1"></a>Örnek 1:

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

### <a name="time-series-instance"></a>Zaman serisi örneği 

> [!NOTE]
> Zaman serisi KIMLIĞI *DeviceID*'dir.

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

Time Series Insights önizlemesi, sorgu süresi boyunca bir tabloyu (düzleştirme sonrasında) birleştirir. Tablo, **türü**gibi ek sütunlar içerir. Aşağıdaki örnek, telemetri verilerinizi nasıl [şekillendirebileceğinizi](./time-series-insights-send-events.md#supported-json-shapes) göstermektedir.

| deviceId  | Tür | L1 | L2 | timestamp | series_Flow oranı ft3/sn | series_Engine petrol basıncı psi |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | SIMÜLATÖR | Pil sistemi | 2018-01-17T01:17:00Z |   1.0172575712203979 |    34,7 |
| `FXXX` | Default_Type | SIMÜLATÖR |   Pil sistemi |    2018-01-17T01:17:00Z | 2.445906400680542 |  49,2 |
| `FYYY` | LINE_DATA ORTAK | SIMÜLATÖR |    Pil sistemi |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22,2 |

Yukarıdaki örnekte aşağıdaki noktalara göz önünde yer verilmiştir:

* Statik özellikler, ağ üzerinden gönderilen verileri iyileştirmek için Time Series Insights önizlemede depolanır.
* Time Series Insights Önizleme verileri, örnekte tanımlanan zaman serisi KIMLIĞIYLE sorgu zamanına birleştirilir.
* İki iç içe geçme katmanı kullanılır. Bu sayı Time Series Insights önizlemenin desteklediği en büyük sayıdır. Derin iç içe dizilerden kaçınmak önemlidir.
* Birkaç ölçü olduğundan, bunlar aynı nesne içinde ayrı özellikler olarak gönderilirler. Örnekte, **Series_Flow Rate psi**, **series_Engine yağ basıncı psi**ve **series_Flow Rate ft3/s** benzersiz sütunlardır.

>[!IMPORTANT]
> Örnek alanları telemetri ile depolanmaz. Bunlar, zaman serisi modelinde meta verilerle birlikte depolanır.
> Yukarıdaki tablo, sorgu görünümünü temsil eder.

### <a name="example-2"></a>Örnek 2:

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
Yukarıdaki örnekte, düzleştirilmiş `data_flow` özelliği `data_flow` özelliği ile bir adlandırma çarpışması sunmalıdır. Bu durumda, *en son* Özellik değeri önceki bir değerin üzerine yazılır. Bu davranış iş senaryolarınız için bir zorluk sunmuşsa, lütfen TSI ekibine başvurun.

> [!WARNING] 
> Düzleştirme veya başka bir mekanizma nedeniyle aynı olay yükünde yinelenen özelliklerin bulunduğu durumlarda, en son özellik değeri, önceki değerlerin üzerine yazılır.


## <a name="next-steps"></a>Sonraki adımlar

- Bu yönergeleri uygulamaya koymak için bkz. [Azure Time Series Insights Preview sorgu sözdizimi](./time-series-insights-query-data-csharp.md). Veri erişimi için Time Series Insights Preview REST API sorgu sözdizimi hakkında daha fazla bilgi edineceksiniz.
- Desteklenen JSON şekilleri hakkında bilgi edinmek için bkz. [desteklenen JSON şekilleri](./time-series-insights-send-events.md#supported-json-shapes).
