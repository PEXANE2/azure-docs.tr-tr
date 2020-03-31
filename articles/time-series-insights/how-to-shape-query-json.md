---
title: JSON - Azure Time Series Insights sorgularını şekillendirmek için en iyi uygulamalar | Microsoft Dokümanlar
description: JSON'u şekillendirerek Azure Time Series Öngörüleri sorgu verimliliğinizi nasıl artıreceğinizi öğrenin.
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.service: time-series-insights
ms.topic: article
ms.date: 02/03/2020
ms.custom: seodec18
ms.openlocfilehash: 8232fd4c2a1e17800c96854b4ba7298e57ed84b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76989901"
---
# <a name="shape-json-to-maximize-query-performance"></a>Sorgu performansını en üst düzeye çıkarmak için JSON'u şekillendirin 

Bu makalede, Azure Zaman Serisi Öngörüler sorgularınızın verimliliğini en üst düzeye çıkarmak için JSON'un nasıl şekillendirileceklerine ilişkin kılavuzlar sağlanmaktadır.

## <a name="video"></a>Video

### <a name="learn-best-practices-for-shaping-json-to-meet-your-storage-needsbr"></a>Depolama ihtiyaçlarınızı karşılayacak şekilde JSON'u şekillendirmek için en iyi uygulamaları öğrenin.</br>

> [!VIDEO https://www.youtube.com/embed/b2BD5hwbg5I]

## <a name="best-practices"></a>En iyi uygulamalar

Zaman Serisi Öngörüleri'ne olayları nasıl gönderdiğinizi düşünün. Yani, her zaman:

1. Verileri ağ üzerinden olabildiğince verimli bir şekilde gönderin.
1. Senaryonuza uygun toplamaları gerçekleştirebilmeniz için verilerinizin bir şekilde depolandığınızdan emin olun.
1. Zaman Serisi Öngörüleri'nin maksimum özellik sınırlarına ulaşmadığınızdan emin olun:
   - S1 ortamları için 600 özellik (sütun).
   - S2 ortamları için 800 özellik (sütun).

> [!TIP]
> Azure Time Series Öngörüleri Önizlemesinde [sınırları ve planlamayı](time-series-insights-update-plan.md) gözden geçirin.

Aşağıdaki kılavuz, mümkün olan en iyi sorgu performansını sağlamaya yardımcı olur:

1. Etiket kimliği gibi dinamik özellikleri özellik adı olarak kullanmayın. Bu kullanım, maksimum özellik sınırına ulaşmaya katkıda bulunur.
1. Gereksiz özellikler göndermeyin. Bir sorgu özelliği gerekli değilse, göndermemek en iyisidir. Bu şekilde depolama sınırlamalarını önlersiniz.
1. Ağ üzerinden statik veri göndermemek için [başvuru verilerini](time-series-insights-add-reference-data-set.md) kullanın.
1. Ağ üzerinden daha verimli veri göndermek için boyut özelliklerini birden çok olay arasında paylaşın.
1. Derin dizi yuvalama kullanmayın. Zaman Serisi Öngörüleri, nesneleri içeren en fazla iki iç içe dizi düzeyidestekler. Zaman Serisi Öngörüleri, iletideki dizileri özellik değeri çiftleri ile birden çok olaya dönüştürür.
1. Tüm veya çoğu olay için yalnızca birkaç önlem varsa, bu ölçüleri aynı nesne içinde ayrı özellikler olarak göndermek daha iyidir. Bunları ayrı olarak göndermek, olay sayısını azaltır ve daha az olayın işlenmesi gerektiğinden sorgu performansını artırabilir. Çeşitli önlemler olduğunda, bunları tek bir özellikte değer olarak göndermek, maksimum özellik sınırına ulaşma olasılığını en aza indirir.

## <a name="example-overview"></a>Örnek genel bakış

Aşağıdaki iki örnek, önceki önerileri vurgulamak için olayların nasıl gönderilebildiğini gösterir. Her örnekten sonra, önerilerin nasıl uygulandığını gözden geçirebilirsiniz.

Örnekler, birden çok aygıtın ölçüm veya sinyal gönderdiği bir senaryoya dayanır. Ölçümler veya sinyaller Akış Hızı, Motor Yağ Basıncı, Sıcaklık ve Nem olabilir. İlk örnekte, tüm aygıtlar arasında birkaç ölçüm vardır. İkinci örnekte birçok aygıt vardır ve her aygıt birçok benzersiz ölçüm gönderir.

## <a name="scenario-one-only-a-few-measurements-exist"></a>Senaryo bir: Yalnızca birkaç ölçüm var

> [!TIP]
> Her ölçüm veya sinyali ayrı bir özellik veya sütun olarak göndermenizi öneririz.

Aşağıdaki örnekte, dış dizinin ortak boyut değerlerinin paylaşılan bir bölümünü içerdiği tek bir Azure IoT Hub iletisi vardır. Dış dizi, iletinin verimliliğini artırmak için başvuru verilerini kullanır. Başvuru verileri, her olayla birlikte değişmeyen aygıt meta verilerini içerir, ancak veri çözümlemesi için yararlı özellikler sağlar. Ortak boyut değerlerinin toplu hale ilmesi ve referans verilerinin çalıştırıldığı, kablo üzerinden gönderilen baytlarda tasarruf sağlar ve bu da iletiyi daha verimli hale getirir.

Azure bulutuna gönderildiğinde JSON'a seri hale getirilmiş bir [IoT Aygıt İletisi nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.message?view=azure-dotnet) kullanarak Time Series Insights GA ortamınıza gönderilen aşağıdaki JSON yükünü göz önünde bulundurun:


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

* Anahtar özelliği **deviceId**olan başvuru veri tablosu:

   | deviceId | Messageıd | deviceLocation |
   | --- | --- | --- |
   | FXXX | HAT\_VERİlerİ | AB |
   | FYYY | HAT\_VERİlerİ | ABD |

* Zaman Serisi Öngörüler etkinlik tablosu, düzleme sonrası:

   | deviceId | Messageıd | deviceLocation | timestamp | Serisi. Akış Hızı ft3/s | Serisi. Motor Yağı Basınç psi |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | HAT\_VERİlerİ | AB | 2018-01-17T01:17:00Z | 1.0172575712203979 | 34.7 |
   | FXXX | HAT\_VERİlerİ | AB | 2018-01-17T01:17:00Z | 2.445906400680542 | 49.2 |
   | FYYY | HAT\_VERİlerİ | ABD | 2018-01-17T01:18:00Z | 0.58015072345733643 | 22.2 |

> [!NOTE]
> - **DeviceId** sütunu, filodaki çeşitli aygıtlar için sütun üstbilgisi olarak hizmet verir. **DeviceId** değerinin kendi özellik adını niçin diğer beş sütunla birlikte 595 (S1 ortamları için) veya 795 (S2 ortamları için) ile sınırlandırır.
> - Gereksiz özelliklerden (örneğin, yapı ve model bilgileri) kaçınılır. Özellikler gelecekte sorgulanamayacağından, bunları ortadan kaldırmak daha iyi ağ ve depolama verimliliği sağlar.
> - Referans veriler ağ üzerinden aktarılan bayt sayısını azaltmak için kullanılır. İki öznitelikleri **messageId** ve **deviceLocation** anahtar özelliği **deviceId**kullanılarak birleştirilir. Bu veriler giriş zamanında telemetri verileriyle birleştirilir ve daha sonra sorgulama için Time Series Insights'ta saklanır.
> - Time Series Insights tarafından desteklenen en fazla iç içe geçme miktarı olan iki iç içe geçme katmanı kullanılır. İç içe gelen dizilerden kaçınmak çok önemlidir.
> - Ölçüler aynı nesne içinde ayrı özellikler olarak gönderilir, çünkü birkaç ölçü vardır. İşte, **seri. Akış Hızı psi** ve **serisi. Motor Yağı Basınç ft3/s** benzersiz sütunlar vardır.

## <a name="scenario-two-several-measures-exist"></a>Senaryo iki: Çeşitli önlemler var

> [!TIP]
> Ölçümleri "tür", "birim" ve "değer" tuples olarak göndermenizi öneririz.

Örnek JSON yükü:

```JSON
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 1.0172575712203979
            },
            {
                "tagId": "oilPressure",
                "value": 49.2
            },
            {
                "tagId": "pumpRate",
                "value": 2.445906400680542
            },
            {
                "tagId": "oilPressure",
                "value": 34.7
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 0.58015072345733643
            },
            {
                "tagId": "oilPressure",
                "value": 22.2
            }
        ]
    }
]
```

* Anahtar özelliklere sahip referans veri tablosu **deviceId** ve **series.tagId**:

   | deviceId | dizi.tagId | Messageıd | deviceLocation | type | unit |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | HAT\_VERİlerİ | AB | Akış Hızı | ft3/s |
   | FXXX | oilPressure | HAT\_VERİlerİ | AB | Motor Yağ Basıncı | psi |
   | FYYY | pumpRate | HAT\_VERİlerİ | ABD | Akış Hızı | ft3/s |
   | FYYY | oilPressure | HAT\_VERİlerİ | ABD | Motor Yağ Basıncı | psi |

* Zaman Serisi Öngörüler etkinlik tablosu, düzleme sonrası:

   | deviceId | dizi.tagId | Messageıd | deviceLocation | type | unit | timestamp | series.value |
   | --- | --- | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | HAT\_VERİlerİ | AB | Akış Hızı | ft3/s | 2018-01-17T01:17:00Z | 1.0172575712203979 | 
   | FXXX | oilPressure | HAT\_VERİlerİ | AB | Motor Yağ Basıncı | psi | 2018-01-17T01:17:00Z | 34.7 |
   | FXXX | pumpRate | HAT\_VERİlerİ | AB | Akış Hızı | ft3/s | 2018-01-17T01:17:00Z | 2.445906400680542 | 
   | FXXX | oilPressure | HAT\_VERİlerİ | AB | Motor Yağ Basıncı | psi | 2018-01-17T01:17:00Z | 49.2 |
   | FYYY | pumpRate | HAT\_VERİlerİ | ABD | Akış Hızı | ft3/s | 2018-01-17T01:18:00Z | 0.58015072345733643 |
   | FYYY | oilPressure | HAT\_VERİlerİ | ABD | Motor Yağ Basıncı | psi | 2018-01-17T01:18:00Z | 22.2 |

> [!NOTE]
> - DeviceId **deviceId** ve **series.tagId** sütunları, filodaki çeşitli aygıtlar ve etiketler için sütun başlıkları olarak hizmet verir. Her birinin kendi özniteliği olarak kullanılması sorguyu diğer altı sütunla birlikte 594 (S1 ortamları için) veya 794 (S2 ortamları için) toplam aygıtlarıyla sınırlar.
> - İlk örnekte belirtilen nedenden dolayı gereksiz özelliklerden kaçınıldı.
> - Referans verileri **deviceId**tanıtarak ağ üzerinden aktarılan bayt sayısını azaltmak için kullanılır , **messageId** ve **deviceLocation**benzersiz çifti için kullanılır. Kompozit anahtar **series.tagId** **türü** ve **birim**benzersiz çifti için kullanılır. Bileşik anahtar **deviceId** ve **series.tagId** çiftinin dört değere başvurmak için kullanılmasını sağlar: **messageId, deviceLocation, type** ve **unit**. Bu veriler giriş zamanında telemetri verileriile birleştirilir. Daha sonra sorgulama için Time Series Insights'ta saklanır.
> - İlk örnekte belirtilen nedenden dolayı iki iç içe geçme katmanı kullanılır.

### <a name="for-both-scenarios"></a>Her iki senaryo için de

Çok sayıda olası değere sahip bir özellik için, her değer için yeni bir sütun oluşturmak yerine tek bir sütun içinde farklı değerler olarak göndermek en iyisidir. Önceki iki örnekten:

  - İlk örnekte, birkaç özelliğin birkaç değeri vardır, bu nedenle her biri ayrı bir özellik yapmak uygundur.
  - İkinci örnekte, ölçüler tek tek özellikler olarak belirtilmemiştir. Bunun yerine, ortak bir seri özelliği altında bir dizi değer veya ölçüler dir. Yeni anahtar **tagId** gönderilir, düzleştirilmiş tabloda yeni sütun **series.tagId** oluşturur. Yeni özellikler **türü** ve **birimi,** özellik sınırına ulaşılamasın diye başvuru verileri kullanılarak oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

- [Buluta IoT Hub aygıt iletileri](../iot-hub/iot-hub-devguide-messages-construct.md)gönderme hakkında daha fazla bilgi edinin.

- Zaman Serisi Öngörüleri veri erişimi REST API'nin sorgu sözdizimi hakkında daha fazla bilgi edinmek için [Azure Zaman Serisi Öngörüleri sorgu sözdizimini](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-syntax) okuyun.

- [Olayları nasıl şekillendireceklerini](./time-series-insights-send-events.md)öğrenin.
