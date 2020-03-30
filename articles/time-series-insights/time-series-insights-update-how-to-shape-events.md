---
title: Şekil olayları - Azure Time Series Öngörüleri | Microsoft Dokümanlar
description: Azure Zaman İstatistikleri Önizleme'de sorgu için en iyi uygulamalar ve olayları nasıl şekillendiririz hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650932"
---
# <a name="shape-events-with-azure-time-series-insights-preview"></a>Azure Time Series Öngörüleri Önizlemesi ile olayları şekillendirme

Bu makalede, Azure Zaman Serisi Öngörüleri'nde yutulmak üzere JSON yüklerinizi şekillendirmek ve Önizleme sorgularınızın verimliliğini en üst düzeye çıkarmak için en iyi uygulamalar tanımlanır.

## <a name="best-practices"></a>En iyi uygulamalar

Zaman Serisi Öngörüleri Önizleme ortamınıza olayları nasıl gönderdiğinizi dikkatlice göz önünde bulundurmanız en iyisidir. 

Genel en iyi uygulamalar şunlardır:

* Verileri ağ üzerinden olabildiğince verimli bir şekilde gönderin.
* Verilerinizi, senaryonuz için daha uygun bir şekilde toplamanıza yardımcı olacak şekilde depolayın.

En iyi sorgu performansı için, aşağıdaki başparmak kurallarına uyun:

* Gereksiz özellikler göndermeyin. Zaman Serisi Öngörüler Kullanıma göre önizleme faturaları. Yalnızca sorguladığınız verileri depolamak ve işlemek en iyisidir.
* Statik veriler için örnek alanlarını kullanın. Bu uygulama ağ üzerinden statik veri göndermemeye yardımcı olur. Zaman Serisi Modeli'nin bir bileşeni olan örnek alanlar, genellikle kullanılabilir Time Series Insights hizmetinde referans verileri gibi çalışır. Örnek alanlar hakkında daha fazla bilgi edinmek için [Time Series Model'i](./time-series-insights-update-tsm.md)okuyun.
* Boyut özelliklerini iki veya daha fazla olay arasında paylaşın. Bu uygulama, ağ üzerinden daha verimli veri göndermenize yardımcı olur.
* Derin dizi yuvalama kullanmayın. Zaman Serisi Öngörüleri Önizleme, nesneleri içeren en fazla iki iç içe dizi düzeylerini destekler. Zaman Serisi Öngörüleri Önizleme, iletideki dizileri özellik değeri çiftleri ile birden çok olaya dönüştürür.
* Tüm veya çoğu olay için yalnızca birkaç önlem varsa, bu ölçüleri aynı nesne içinde ayrı özellikler olarak göndermek daha iyidir. Bunları ayrı olarak göndermek, olay sayısını azaltır ve daha az olayın işlenmesi gerektiğinden sorgu performansını artırabilir.

## <a name="column-flattening"></a>Sütun düzleme

Yutma sırasında, iç içe nesneler içeren yükler, sütun adının bir delineator ile tek bir değer olması için düzleştirilmiş olacaktır.

* Örneğin, aşağıdaki iç içe json:

   ```JSON
   "data": {
        "flow": 1.0172575712203979,
   },
   ```

   Olur: `data_flow` düzleştirilmiş zaman.

> [!IMPORTANT]
> * Azure Zaman Serisi Öngörüleri Önizleme, sütun demi için alt çizgi`_`() kullanır.
> * Dönemleri ()`.`kullanan Genel Kullanılabilirlik'ten farkı not edin.

Daha karmaşık senaryolar aşağıda gösterilmiştir.

#### <a name="example-1"></a>Örnek 1:

Aşağıdaki senaryoda ölçümleri (sinyalleri) gönderen iki (veya daha fazla) cihaz vardır: *Akış Hızı,* *Motor Yağı Basıncı,* *Sıcaklık*ve *Nem.*

Dış dizinin ortak boyut değerlerinin paylaşılan bir bölümünü içerdiği tek bir Azure IoT Hub iletisi gönderilir (iletide bulunan iki aygıt girişine dikkat edin).

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

**Takeaways:**

* Örnek, JSON'un iletinin verimliliğini artırmak için [Zaman Serisi Örnek](./time-series-insights-update-tsm.md#time-series-model-instances) verilerini kullanan bir dış dizivardır. Zaman Serisi Örnekleri aygıt meta verilerinin değişme olasılığı olmasa da, genellikle veri çözümlemesi için yararlı özellikler sağlar.

* JSON, iki veya daha fazla iletiyi (her cihazdan bir tane) zaman içinde bant genişliğinden tasarruf eden tek bir yükte birleştirir.

* Her aygıt için tek tek seri veri noktaları, her aygıt için sürekli olarak güncelleştirme akışı gereksinimini azaltarak tek bir **seri** özniteliği olarak birleştirilir.

> [!TIP]
> Veri göndermek ve telemetriyi daha verimli hale getirmek için, ortak boyut değerlerini ve Zaman Serisi Örneği meta verilerini tek bir JSON yükünde toplu hale getirmeyi düşünün.

#### <a name="time-series-instance"></a>Zaman Serisi Örneği 

JSON'unuzu daha iyi şekillendirmek için [Time Series Instance'ı](./time-series-insights-update-tsm.md#time-series-model-instances) nasıl kullanacağınıza daha yakından bakalım. 

> [!NOTE]
> Aşağıdaki [Zaman Serisi kimlikleri](./time-series-insights-update-how-to-id.md) *deviceId'lerdir.*

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

Zaman Serisi Öngörüleri Önizleme, sorgu süresi boyunca bir tabloya (düzleme den sonra) katılır. Tablo, **Türü**gibi ek sütunlar içerir.

| deviceId  | Tür | L1 | L2 | timestamp | series_Flow Oranı ft3/s | series_Engine Yağ Basıncı psi |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | Simülatörü | Pil Sistemi | 2018-01-17T01:17:00Z |   1.0172575712203979 |    34.7 |
| `FXXX` | Default_Type | Simülatörü |   Pil Sistemi |    2018-01-17T01:17:00Z | 2.445906400680542 |  49.2 |
| `FYYY` | LINE_DATA ORTAK | Simülatörü |    Pil Sistemi |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22.2 |

> [!NOTE]
>  Önceki [tablo, Zaman Serisi Önizleme Gezgini'ndeki](./time-series-insights-update-explorer.md)sorgu görünümünü temsil eder.

**Takeaways:**

* Önceki örnekte, statik özellikler ağ üzerinden gönderilen verileri optimize etmek için Zaman Serisi Öngörüleri Önizleme'de depolanır.
* Zaman Serisi Öngörüleri Önizleme verileri, sorgu sırasında, örnekte tanımlanan Zaman Serisi Kimliği aracılığıyla birleştirilir.
* İki yuvalama katmanı kullanılır. Bu sayı, Time Series Insights Preview'un desteklediği en büyük sayıdır. İç içe gelen dizilerden kaçınmak çok önemlidir.
* Birkaç önlem olduğundan, aynı nesne içinde ayrı özellikler olarak gönderilirler. Örnekte, **series_Flow Oranı psi**, **series_Engine Yağ Basıncı psi**, ve **series_Flow Oranı ft3/s** benzersiz sütunlar vardır.

>[!IMPORTANT]
> Örnek alanlar telemetri ile depolanır. Zaman Serisi Modeli'nde meta verilerle birlikte depolanırlar.

#### <a name="example-2"></a>Örnek 2:

Aşağıdaki JSON düşünün:

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

Yukarıdaki örnekte, basık `data["flow"]` özellik `data_flow` özelliği ile bir adlandırma çarpışma sunacak.

Bu durumda, *en son* özellik değeri öncekinin üzerine yazar. 

> [!TIP]
> Daha fazla yardım için Time Series Insights ekibiyle iletişime geçin!

> [!WARNING] 
> * Düzleştirme veya başka bir mekanizma nedeniyle yinelenen özelliklerin aynı (tekil) olay yükünde bulunduğu durumlarda, önceki değerlerin üzerinde aşırı yazılması yla en son > özellik değeri depolanır.
> * Birleştirilmiş olaylar dizisi birbirini geçersiz kılmaz.

## <a name="next-steps"></a>Sonraki adımlar

* Bu yönergeleri uygulamaya koymak için [Azure Zaman Serisi Öngörüleri Önizleme sorgu sözdizimini](./time-series-insights-query-data-csharp.md)okuyun. Veri erişimi için Zaman Serisi Öngörüleri [Önizleme REST API'nin](https://docs.microsoft.com/rest/api/time-series-insights/preview) sorgu sözdizimi hakkında daha fazla bilgi edineceksiniz.

* [Nasıl Zaman Serisi Modeli ile](./time-series-insights-update-how-to-tsm.md)JSON en iyi uygulamaları birleştirin.
