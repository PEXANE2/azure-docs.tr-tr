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
ms.date: 10/22/2019
ms.custom: seodec18
ms.openlocfilehash: f8a50e062d2dac1f30f8b745f351570262daac53
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2019
ms.locfileid: "72990886"
---
# <a name="shape-events-with-azure-time-series-insights-preview"></a>Azure Time Series Insights Preview ile şekil olayları

Bu makale, Azure Time Series Insights Preview sorgularınızın verimliliğini en üst düzeye çıkarmak için JSON dosyanızı şekillendirmenize yardımcı olur.

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

## <a name="example"></a>Örnek

Aşağıdaki örnek, iki veya daha fazla cihazın ölçüm veya sinyal gönderen bir senaryoya dayalıdır. Ölçümler veya sinyaller *Flow ücreti*, *altyapı yağ baskısı*, *sıcaklık*ve *nem*olabilir.

Örnekte, dış dizinin ortak boyut değerlerinin paylaşılan bir bölümünü içerdiği tek bir Azure IoT Hub iletisi vardır. Dış dizi, iletinin verimliliğini artırmak için zaman serisi örnek verilerini kullanır. 

Zaman serisi örneği, cihaz meta verilerini içerir. Bu meta veriler her olayla değişmez, ancak veri analizi için yararlı özellikler sağlıyor. Kablo üzerinden gönderilen baytlara kaydetmek ve iletiyi daha verimli hale getirmek için ortak boyut değerlerini toplu olarak oluşturmayı ve zaman serisi örnek meta verilerini kullanmayı düşünün.

### <a name="example-json-payload"></a>Örnek JSON yükü

```JSON
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 1.0172575712203979,
                "Engine Oil Pressure psi ": 34.7
            },
            {
                "Flow Rate ft3/s": 2.445906400680542,
                "Engine Oil Pressure psi ": 49.2
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

### <a name="time-series-instance"></a>Zaman serisi örneği 
> [!NOTE]
> Zaman serisi KIMLIĞI *DeviceID*'dir.

```JSON
{
    "timeSeriesId": [
      "FXXX"
    ],
    "typeId": "17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds": [
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description": null,
    "instanceFields": {
      "L1": "REVOLT SIMULATOR",
      "L2": "Battery System",
    }
  },
  {
    "timeSeriesId": [
      "FYYY"
    ],
    "typeId": "17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds": [
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description": null,
    "instanceFields": {
      "L1": "COMMON SIMULATOR",
      "L2": "Battery System",
    }
  },
```

Time Series Insights önizlemesi, sorgu süresi boyunca bir tabloyu (düzleştirme sonrasında) birleştirir. Tablo, **türü**gibi ek sütunlar içerir. Aşağıdaki örnek, telemetri verilerinizi nasıl [şekillendirebileceğinizi](./time-series-insights-send-events.md#supported-json-shapes) göstermektedir.

| deviceId  | Tür | L1 | L2 | timestamp | ilerindeki. Akış oranı ft3/sn | ilerindeki. Motor yağ basıncı psi |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | SIMÜLATÖR | Pil sistemi | 2018-01-17T01:17:00Z |   1.0172575712203979 |    34,7 |
| `FXXX` | Default_Type | SIMÜLATÖR |   Pil sistemi |    2018-01-17T01:17:00Z | 2.445906400680542 |  49,2 |
| `FYYY` | LINE_DATA ORTAK | SIMÜLATÖR |    Pil sistemi |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22,2 |

Yukarıdaki örnekte aşağıdaki noktalara göz önünde yer verilmiştir:

* Statik özellikler, ağ üzerinden gönderilen verileri iyileştirmek için Time Series Insights önizlemede depolanır.
* Time Series Insights Önizleme verileri, örnekte tanımlanan zaman serisi KIMLIĞIYLE sorgu zamanına birleştirilir.
* İki iç içe geçme katmanı kullanılır. Bu sayı Time Series Insights önizlemenin desteklediği en büyük sayıdır. Derin iç içe dizilerden kaçınmak önemlidir.
* Birkaç ölçü olduğundan, bunlar aynı nesne içinde ayrı özellikler olarak gönderilirler. Örnekte, **serisi. Akış hızı PSI**, **serisi. Motor yağ basıncı psi**ve **serisi. Flow ücreti ft3/s** benzersiz sütunlardır.

>[!IMPORTANT]
> Örnek alanları telemetri ile depolanmaz. Bunlar, zaman serisi modelinde meta verilerle birlikte depolanır.
> Yukarıdaki tablo, sorgu görünümünü temsil eder.

## <a name="next-steps"></a>Sonraki adımlar

- Bu yönergeleri uygulamaya koymak için bkz. [Azure Time Series Insights Preview sorgu sözdizimi](./time-series-insights-query-data-csharp.md). Veri erişimi için Time Series Insights Preview REST API sorgu sözdizimi hakkında daha fazla bilgi edineceksiniz.
- Desteklenen JSON şekilleri hakkında bilgi edinmek için bkz. [desteklenen JSON şekilleri](./time-series-insights-send-events.md#supported-json-shapes).
