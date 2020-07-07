---
title: Sorgusu alınan telemetri verileri
description: Bu makalede, alınan Telemetri verilerinin nasıl sorgulanmakta olduğu açıklanır.
author: sunasing
ms.topic: article
ms.date: 03/11/2020
ms.author: sunasing
ms.openlocfilehash: f717903b3f953e04c793092c86802f2006de7e82
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80349812"
---
# <a name="query-ingested-telemetry-data"></a>Sorgusu alınan telemetri verileri

Bu makalede, Azure Farmınts 'den alınan algılayıcı verilerinin nasıl sorgulanmakta olduğu açıklanır.

Cihazlar ve algılayıcılar gibi Nesnelerin İnterneti (IoT) kaynaklarından veri almak, Farmınts 'de yaygın bir senaryodur. Cihazlar ve sensörler için meta veriler oluşturur ve ardından geçmiş verileri, kurallı bir biçimde Farmto 'Lar için kullanırsınız. Bir algılayıcı verileri Farmrets veri hub 'ında kullanılabilir olduktan sonra, eyleme dönüştürülebilir içgörüler veya yapı modelleri oluşturmak için aynı şekilde sorgu yapabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleye devam etmeden önce, IoT cihazlarınızdan farmınts ve giriş algılayıcı telemetri verilerini, bu makaleye yüklediğinizden emin olun.

Algılayıcı telemetri verilerini almak için alma [Geçmiş telemetri verilerini](ingest-historical-telemetry-data-in-azure-farmbeats.md) ziyaret edin

Devam etmeden önce, API 'Leri kullanarak alınan Telemetriyi sorgulamadığınızda, Farmcts REST API 'Lerini de öğrendiğinizden emin olmanız gerekir. Farmtts API 'Leri hakkında daha fazla bilgi için bkz. [FARMTEMPTS REST API 'leri](rest-api-in-azure-farmbeats.md). **Farmrets Datahub uç noktanıza API istekleri yapabildiğinizden emin olun**.

## <a name="query-ingested-sensor-telemetry-data"></a>Sorgu alınan algılayıcı telemetri verileri

Farmtts 'den telemetri verilerine erişmek ve bunları sorgulamak için iki yol vardır:

- API ve
- Time Series Insights (TSI).

### <a name="query-using-rest-api"></a>REST API kullanarak sorgulama

Farmınts REST API 'Lerini kullanarak alınan algılayıcı telemetri verilerini sorgulamak için adımları izleyin:

1. İlgilendiğiniz algılayıcıyı belirler. Bunu,/sensör API 'sinde bir GET isteği yaparak yapabilirsiniz.

> [!NOTE]
> İlgilendiğiniz algılayıcı nesnesinin **kimliği** ve **Sensormodelıd** .

2. Adım 1 ' de belirtildiği gibi **Sensormodelıd** için/SensorModel API 'sinde Get/{ID} yapın. "Algılayıcı modeli", sensörden alınan telemetri hakkında tüm meta verileri ve ayrıntıları içerir. Örneğin, algılayıcı **modeli** nesnesi Içindeki **algılayıcı ölçüsü** , sensörlerin hangi ölçülerde ve hangi türlerde ve birimlerde olduğuna ilişkin ayrıntıları içerir. Örneğin,

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
Algılayıcı modeli için GET/{id} çağrısından gelen yanıtı bir yere unutmayın.

3. /Telemetri API 'sinde aşağıdaki giriş yüküne sahip bir POST çağrısı yapın

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
4. /Telemetri API 'sindeki yanıt şuna benzer şekilde görünür:

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
Yukarıdaki örnek yanıtta, sorgulanan algılayıcı telemetrisi, iki zaman damgasına ilişkin verileri, ölçü adı ("moist_soil_last") ve bildirilen Telemetriyi iki zaman damgasına göre verir. Bildirilen değerlerin türünü ve birimini yorumlamak için ilişkili algılayıcı modeline (adım 2 ' de açıklandığı gibi) başvurmanız gerekecektir.

### <a name="query-using-azure-time-series-insights-tsi"></a>Azure Time Series Insights kullanarak sorgulama (TSI)

Farmınts, IoT ölçeğinde verileri almak, depolamak, sorgulamak ve görselleştirmek için [Azure Time Series Insights (TSI)](https://azure.microsoft.com/services/time-series-insights/) kullanır. Bu, zaman serisi için en iyi duruma getirilmiş ve en iyileştirilmiş veri.

Telemetri verileri bir EventHub üzerinde alınır ve sonra işlenir ve Farmtts kaynak grubu içindeki bir TSI ortamına gönderilir. Veriler daha sonra doğrudan TSI 'dan sorgulanabilir. Daha fazla bilgi için bkz. [TSI belgeleri](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer)

TSI üzerinde verileri görselleştirmek için adımları izleyin:

1. **Azure Portal**  >  **farmrets Datahub kaynak grubu** ' na gidin > **Time Series Insights** ortamı (TSİ-xxxx) > **veri erişim ilkeleri**' ni seçin. Okuyucu veya katkıda bulunan erişimi olan kullanıcı ekleyin.
2. **Time Series Insights** ortamının (TSİ-xxxx) **genel bakış** sayfasına gidin ve **Time Series Insights Explorer URL 'sini**seçin. Artık alınan telemetrisini görselleştirebileceksiniz.

Telemetriyi depolamanın, sorgulayan ve görselleştirmenin yanı sıra, TSI Power BI panosu ile tümleştirmeyi de sağlar. Daha fazla bilgi için [buraya]( https://docs.microsoft.com/azure/time-series-insights/how-to-connect-power-bi) bakın

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Farmtts örneğinden gelen algılayıcı verilerini sorguladınız. Şimdi, gruplar için [haritalar oluşturmayı](generate-maps-in-azure-farmbeats.md#generate-maps) öğrenin.
