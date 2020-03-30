---
title: Yutulan telemetri verilerini sorgulama
description: Bu makalede, sindirilen telemetri verilerinin nasıl sorgulanılacaösin.
author: sunasing
ms.topic: article
ms.date: 03/11/2020
ms.author: sunasing
ms.openlocfilehash: f717903b3f953e04c793092c86802f2006de7e82
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349812"
---
# <a name="query-ingested-telemetry-data"></a>Yutulan telemetri verilerini sorgulama

Bu makalede, Azure FarmBeats'ten alınan sensör verilerinin nasıl sorgulanılan bir şekilde sorgulanış ları açıklanmaktadır.

Cihazlar ve sensörler gibi Nesnelerin İnterneti (IoT) kaynaklarından veri yutmak FarmBeats'te yaygın bir senaryodur. Aygıtlar ve sensörler için meta veriler oluşturur sunuz ve tarihi verileri farmbeats'e kanonik biçimde sindirirsiniz. FarmBeats Datahub'da sensör verileri kullanılabilir hale geldikten sonra, işlem yapılabilir öngörüler oluşturmak veya modeller oluşturmak için aynı sorguyı sorgulayabiliriz.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleye devam etmeden önce FarmBeats yüklü ve FarmBeats için IoT cihazlardan sensör telemetri verileri yutulan emin olun.

Sensör telemetri verilerini almak [için, geçmiş telemetri verilerini](ingest-historical-telemetry-data-in-azure-farmbeats.md)

Devam etmeden önce, API'leri kullanarak sindirilmiş telemetriyi sorguladığınız için FarmBeats REST API'lerine aşina olduğunuzdan da emin olmanız gerekir. FarmBeats API'leri hakkında daha fazla bilgi için [FarmBeats REST API'lerine](rest-api-in-azure-farmbeats.md)bakın. **FarmBeats Datahub bitiş noktanıza API isteklerinde bulunabildiğinizden emin olun.**

## <a name="query-ingested-sensor-telemetry-data"></a>Yutulan sensör telemetri verilerini sorgulama

FarmBeats'ten telemetri verilerine erişmenin ve sorgulamanın iki yolu vardır:

- API ve
- Zaman Serisi Öngörüleri (TSI).

### <a name="query-using-rest-api"></a>REST API kullanarak sorgu

FarmBeats REST API'lerini kullanarak yutulan sensör telemetri verilerini sorgulamak için aşağıdaki adımları izleyin:

1. İlgilendiğiniz sensörü tanımlayın. Bunu /Sensor API'de GET isteği nde bulunarak yapabilirsiniz.

> [!NOTE]
> İlgilenen sensör nesnesinin **kimliği** ve **sensörüModelId.**

2. Adım 1'de belirtildiği gibi **SensorModelId** için /SensorModel API'de GET/{id} yapın. "Sensör Modeli" sensöründen yutulan telemetri ile ilgili tüm meta verilere ve ayrıntılara sahiptir. Örneğin, **Sensör Modeli** nesnesi içindeki **Sensör Ölçümü,** sensörün hangi önlemlerin gönderildiği ve hangi tür ve birimlerde gönderildiği hakkında ayrıntılara sahiptir. Örneğin,

  ```json
  {
      "name": "moist_soil_last <name of the sensor measure - this is what we will receive as part of the queried telemetry data>",
      "dataType": "Double <Data Type - eg. Double>",
      "type": "SoilMoisture <Type of measure eg. temperature, soil moisture etc.>",
      "unit": "Percentage <Unit of measure eg. Celsius, Percentage etc.>",
      "aggregationType": "None <either of None, Average, Maximum, Minimum, StandardDeviation>",
      "description": "<Description of the measure>"
  }
  ```
Sensör Modeli için GET/{id} çağrısından yanıta dikkat edin.

3. /Telemetri API'de aşağıdaki giriş yüküyle POST araması yapın

  ```json
  {
    "sensorId": "<id of the sensor as noted in step 1>",
    "searchSpan": {
      "from": "<desired start timestamp in ISO 8601 format; default is UTC>",
      "to": "<desired end timestamp in ISO 8601 format; default is UTC>"
    },
    "filter": {
      "tsx": "string"
    },
    "projectedProperties": [
      {
        "additionalProp1": "string",
        "additionalProp2": "string",
        "additionalProp3": "string"
      }
    ]
  }
  ```
4. /Telemetri API'sinin yanıtı şuna benzer:

  ```json
  {
    "timestamps": [
      "2020-XX-XXT07:30:00Z",
      "2020-XX-XXT07:45:00Z"
    ],
    "properties": [
      {
        "values": [
          "<id of the sensor>",
          "<id of the sensor>"
        ],
        "name": "Id",
        "type": "String"
      },
      {
        "values": [
          2.1,
          2.2
        ],
        "name": "moist_soil_last <name of the SensorMeasure as defined in the SensorModel object>",
        "type": "Double <Data Type of the value - eg. Double>"
      }
    ]
  }
  ```
Yukarıdaki örnek yanıtta, sorgulanmış sensör telemetrisi, iki zaman damgasında bildirilen telemetrinin değerleri ("moist_soil_last") ve ölçü adı ile birlikte iki zaman damgası için veri verir. Bildirilen değerlerin türünü ve birimini yorumlamak için ilişkili Sensör Modeline (adım 2'de açıklandığı şekilde) başvurmanız gerekir.

### <a name="query-using-azure-time-series-insights-tsi"></a>Azure Zaman Serisi Öngörülerini Kullanarak Sorgula (TSI)

FarmBeats, IoT ölçeğinde verileri yüksek düzeyde bağlamsallaştırılan ve zaman serisi için optimize edilmiş verileri yutmak, depolamak, sorgulamak ve görselleştirmek için [Azure Zaman Serisi Öngörülerinden (TSI)](https://azure.microsoft.com/services/time-series-insights/) yararlanır.

Telemetri verileri bir EventHub'dan alınır ve farmbeats kaynak grubu içinde bir TSI ortamına işlenir ve itilir. Veriler daha sonra doğrudan TSI'den sorgulanabilir. Daha fazla bilgi için [TSI belgelerine](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer) bakın

TSI'deki verileri görselleştirmek için aşağıdaki adımları izleyin:

1. Azure **Portal** > **FarmBeats DataHub kaynak grubuna** gidin > **Zaman Serisi Öngörüler** ortamını (tsi-xxxx) > Veri Erişim **İlkeleri'ni**seçin. Reader veya Contributor erişimi olan kullanıcı ekleyin.
2. **Time Series Insights** ortamının **Genel Bakış** sayfasına (tsi-xxxx) gidin ve Time **Series Insights Explorer URL'sini**seçin. Artık yutulan telemetriyi görselleştirebileceksiniz.

TSI, telemetrinin depolanması, sorgulanması ve görselleştirilmesinin yanı sıra Power BI panosuna tümleştirme yi de sağlar. Daha fazla bilgi için [buraya]( https://docs.microsoft.com/azure/time-series-insights/how-to-connect-power-bi) bakın

## <a name="next-steps"></a>Sonraki adımlar

Azure FarmBeats örneğinizden gelen sensör verilerini şimdi sorguladınız. Şimdi, çiftlikleriniz için haritaları nasıl [oluşturacağınızı](generate-maps-in-azure-farmbeats.md#generate-maps) öğrenin.
