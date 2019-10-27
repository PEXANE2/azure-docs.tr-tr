---
title: Azure IoT Central verilerinizi dışarı aktarın | Microsoft Docs
description: Azure IoT Central uygulamanızdan Azure Event Hubs, Azure Service Bus ve Azure Blob depolama 'ya veri aktarma
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 10/15/2019
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: bc78f11d4f61c46e2ad4f7143fe8b3af6762b0b4
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72950424"
---
# <a name="export-your-azure-iot-central-datapreview-features"></a>Azure IoT Central verilerinizi dışarı aktarma (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

*Bu konu, Yöneticiler için geçerlidir.*

Bu makalede verilerinizi Azure Event Hubs, Azure Service Bus veya Azure Blob depolama örneklerine aktarmak için Azure IoT Central sürekli veri dışa aktarma özelliğinin nasıl kullanılacağı açıklanır. Veriler JSON biçiminde verilir ve Telemetriyi, cihaz bilgilerini ve cihaz şablonu bilgilerini içerebilir. İçin, bu verileri kullan:

- Sıcak yol öngörüleri ve analizi. Bu seçenek, Azure Stream Analytics özel kuralların tetiklenmesi, Azure Logic Apps özel iş akışlarını tetiklenmesi veya dönüştürülecek Azure Işlevleri aracılığıyla geçirilmesidir.
- Microsoft Power BI 'de Azure Machine Learning veya uzun süreli eğilim analizinde eğitim modelleri gibi soğuk yol analizi.

> [!Note]
> Sürekli veri dışarı aktarmayı açtığınızda, yalnızca o andan itibaren verileri alırsınız. Şu anda, sürekli veri dışa aktarma kapalı olduğunda veriler bir saat için alınamaz. Daha fazla geçmiş verileri sürdürmek için sürekli veri dışa aktarmayı erken açın.

## <a name="prerequisites"></a>Önkoşullar

IoT Central uygulamanızda yönetici olmanız gerekir

## <a name="set-up-export-destination"></a>Dışarı aktarma hedefini ayarla

Sürekli veri dışa aktarmayı yapılandırmadan önce dışa aktarma hedefinin mevcut olması gerekir.

### <a name="create-event-hubs-namespace"></a>Event Hubs ad alanı oluştur

Uygulamasına dışarı aktarmak için mevcut bir Event Hubs ad alanınız yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir Event Hubs ad alanı](https://ms.portal.azure.com/#create/Microsoft.EventHub)oluşturun. [Azure Event Hubs docs](../../event-hubs/event-hubs-create.md)'ta daha fazla bilgi edinebilirsiniz.

2. Bir abonelik seçin. Verileri, Kullandıkça Öde IoT Central uygulamanızla aynı abonelikte olmayan diğer aboneliklere aktarabilirsiniz. Bu durumda bir bağlantı dizesi kullanarak bağlanırsınız.

3. Event Hubs ad alanında bir olay hub 'ı oluşturun. Ad alanına gidin ve bir olay hub 'ı örneği oluşturmak için en üstteki **+ Olay Hub 'ını** seçin.

### <a name="create-service-bus-namespace"></a>Service Bus ad alanı oluştur

Uygulamasına dışarı aktarmak için mevcut bir Service Bus ad alanınız yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir Service Bus ad alanı](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5)oluşturun. [Azure Service Bus docs](../../service-bus-messaging/service-bus-create-namespace-portal.md)' de daha fazla bilgi edinebilirsiniz.
2. Bir abonelik seçin. Verileri, Kullandıkça Öde IoT Central uygulamanızla aynı abonelikte olmayan diğer aboneliklere aktarabilirsiniz. Bu durumda bir bağlantı dizesi kullanarak bağlanırsınız.

3. Service Bus ad alanına gidin ve en üstteki **+ kuyruk** veya **+ Konu** başlığını seçerek dışarı aktarılacak bir kuyruk veya konu oluşturun.

Dışarı aktarma hedefi olarak Service Bus seçtiğinizde, kuyruklar ve konuların oturumları veya yinelenen algılama özelliği etkin olmalıdır. Bu seçeneklerden biri etkinleştirilirse, bazı iletiler kuyruğunuza veya konuya ulaşmayacaktır.

### <a name="create-storage-account"></a>Depolama hesabı oluştur

' A dışa aktarılacak mevcut bir Azure depolama hesabınız yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir depolama hesabı](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)oluşturun. Yeni [Azure Blob depolama hesapları](https://aka.ms/blobdocscreatestorageaccount) veya [Azure Data Lake Storage v2 depolama hesapları](../../storage/blobs/data-lake-storage-quickstart-create-account.md)oluşturma hakkında daha fazla bilgi edinebilirsiniz.

    - Verileri bir Azure Data Lake Storage v2 depolama hesabına aktarmayı seçerseniz, **Hesap türü**olarak **blobstorage** ' ı seçmeniz gerekir.
    - Kullandıkça Öde IoT Central uygulamanız için olandan farklı aboneliklerdeki depolama hesaplarına veri aktarabilirsiniz. Bu durumda bir bağlantı dizesi kullanarak bağlanacaksınız.

2. Depolama hesabınızda bir kapsayıcı oluşturun. Depolama hesabınıza gidin. **BLOB hizmeti**altında **bloblara gözatamıyorum**' ı seçin. Yeni bir kapsayıcı oluşturmak için üst kısımdaki **+ kapsayıcı** ' yı seçin.

## <a name="set-up-continuous-data-export"></a>Sürekli veri dışarı aktarma ayarlama

Verilerin dışarı aktarılacağı bir hedef olduğuna göre, sürekli veri dışa aktarma ayarlamak için aşağıdaki adımları izleyin.

1. IoT Central uygulamanızda oturum açın.

2. Sol bölmede **veri dışa aktar**' ı seçin.

    > [!Note]
    > Sol bölmede veri dışa aktarma görmüyorsanız, uygulamanızda yönetici değilsiniz demektir. Verilerin dışarı aktarılmasını ayarlamak için bir yöneticiye danışın.

3. Sağ üst köşedeki **+ Yeni** düğmesini seçin. Dışarı aktarmanın hedefi olarak **azure Event Hubs**, **Azure Service Bus**veya **Azure Blob depolamadan** birini seçin. Uygulama başına en fazla dışarı aktarma sayısı beştir.

    ![Yeni sürekli veri dışa aktarma oluştur](media/howto-export-data-pnp/export-new2.png)

4. Aşağı açılan liste kutusunda **Event Hubs ad**alanınızı, **Service Bus ad alanını**, **depolama hesabı ad alanını**seçin veya **bir bağlantı dizesi girin**.

    - Depolama hesapları, Event Hubs ad alanları ve Service Bus ad alanlarını IoT Central uygulamanızla aynı abonelikte görürsünüz. Bu aboneliğin dışında bir hedefe aktarmak istiyorsanız, **bir bağlantı dizesi girin** ' i seçin ve 5. adıma bakın.
    - Yedi günlük deneme uygulamaları için sürekli veri vermeyi yapılandırmanın tek yolu bir bağlantı dizesidir. Yedi günlük deneme uygulamalarının ilişkili bir Azure aboneliği yoktur.

    ![Yeni Olay Hub 'ı oluştur](media/howto-export-data-pnp/export-eh.png)

5. Seçim **Bir bağlantı dizesi girin**' i seçerseniz, Bağlantı dizenizi yapıştırmanız için yeni bir kutu belirir. İçin bağlantı dizesini almak için:
    - Event Hubs veya Service Bus, Azure portal ad alanına gidin.
        - **Ayarlar**altında, **paylaşılan erişim ilkeleri** ' ni seçin.
        - Varsayılan **RootManageSharedAccessKey** seçin veya yeni bir tane oluşturun
        - Birincil veya ikincil bağlantı dizesini kopyalayın
    - Depolama hesabı, Azure portal depolama hesabına gidin:
        - **Ayarlar**altında **erişim anahtarları** ' nı seçin.
        - KEY1 bağlantı dizesini veya key2 bağlantı dizesini kopyalayın

6. Aşağı açılan liste kutusundan bir olay hub 'ı, kuyruğu, konuyu veya kapsayıcıyı seçin.

7. **Dışarı aktarılacak veriler**' in altında, türü **üzerine**ayarlayarak dışarı aktarılacak veri türlerini seçin.

8. Sürekli veri dışa aktarmayı açmak için, **veri dışa aktarma** geçişi 'nin **Açık**olduğundan emin olun. **Kaydet**’i seçin.

9. Birkaç dakika sonra verileriniz seçtiğiniz hedefte görüntülenir.

## <a name="data-format"></a>Veri biçimi

Veriler, Olay Hub 'ınıza veya Service Bus kuyruğuna veya konuya neredeyse gerçek zamanlı olarak verilir.

Veriler, son içe aktarma dosyasından bu yana değişiklik kümesini içeren her bir dosyayla, depolama hesabınıza dakikada bir kez verilir. Verilen veriler JSON biçiminde üç klasöre yerleştirilir. Depolama hesabınızdaki varsayılan yollar şunlardır:

- Telemetri: _{Container}/{app-id}/telemetry/{yyyy}/{mm}/{dd}/{hh}/{mm}/{filename}_
- Cihazlar: _{Container}/{app-id}/Devices/{yyyy}/{mm}/{dd}/{ss}/{mm}/{filename}_
- Cihaz şablonları: _{Container}/{App-ID}/devicetemplates/{yyyy}/{mm}/{dd}/{ss}/{mm}/{filename}_

Dosyaya giderek ve **blobu Düzenle** sekmesini seçerek Azure Portal dışarıya aktarılmış dosyalara gidebilirsiniz.

İçe aktarılmış telemetri verileri, yalnızca telemetri değerlerinin kendileri değil, cihazlarınızın IoT Central gönderildiği iletinin tamamını içerir. İçe aktarılmış cihazlar verileri tüm cihazların özelliklerine ve meta verilerine yapılan değişiklikleri içerir ve bu cihaz şablonları tüm cihaz şablonlarında değişiklikler içerir. İçe aktarılmış veriler gövde özelliğinde bulunur ve JSON biçimindedir.

### <a name="telemetry"></a>Telemetri

IoT Central bir cihazdan ileti aldıktan sonra yeni bir ileti hızlı bir şekilde verilir.

- Event Hubs ve Service Bus içindeki her bir dışarıya gönderilen ileti, cihazın, JSON biçiminde Body özelliğinde gönderildiği tam iletiyi içerir.
- Blob Storage 'daki dışarıya aktarılmış dosyalar, blob depolamaya [IoT Hub ileti yönlendirmesi tarafından içeri](../../iot-hub/iot-hub-csharp-csharp-process-d2c.md) aktarılmış ileti dosyalarıyla aynı biçimi kullanır. Cihazlarınızın `contentType: application/JSON` ve `contentEncoding:utf-8` (veya `utf-16`, `utf-32`) sahip iletiler göndermesini sağlayın. Bir örnek için [IoT Hub belgelerine](../../iot-hub/iot-hub-devguide-routing-query-syntax.md#message-routing-query-based-on-message-body) bakın.

Telemetriyi gönderen cihazlar cihaz kimlikleri tarafından temsil edilir (aşağıdaki bölümlere bakın). Cihazların adlarını almak için, cihaz verilerini dışarı aktarın ve cihaz iletisinin **DeviceID** 'Siyle eşleşen **connectiondeviceıd** 'yi kullanarak her iletiyi ilişkilendirin.

Aşağıdaki örnek, Olay Hub 'ında veya Service Bus kuyruğu veya konusunda alınan telemetri verileriyle ilgili bir ileti gösterir.

```json
{
  "body":{
    "temp":67.96099945281145,
    "humid":58.51139305465015,
    "pm25":36.91162432340187
  },
  "annotations":{
    "iothub-connection-device-id":"<deviceId>",
    "iothub-connection-auth-method":"{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "iothub-connection-auth-generation-id":"<generationId>",
    "iothub-enqueuedtime":1539381029965,
    "iothub-message-source":"Telemetry",
    "x-opt-sequence-number":25325,
    "x-opt-offset":"<offset>",
    "x-opt-enqueued-time":1539381030200
  },
  "sequenceNumber":25325,
  "enqueuedTimeUtc":"2018-10-12T21:50:30.200Z",
  "offset":"<offset>",
  "properties":{
    "content_type":"application/json",
    "content_encoding":"utf-8"
  }
}
```

Aşağıdaki örnekte, blob depolamada JSON biçimindeki bir kayıt gösterilmektedir:

```json
{
  "EnqueuedTimeUtc":"2019-09-26T17:46:09.8870000Z",
  "Properties":{

  },
  "SystemProperties":{
    "connectionDeviceId":"<deviceid>",
    "connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "connectionDeviceGenerationId":"637051167384630591",
    "contentType":"application/json",
    "contentEncoding":"utf-8",
    "enqueuedTime":"2019-09-26T17:46:09.8870000Z"
  },
  "Body":{
    "temp":49.91322758395974,
    "humid":49.61214852573155,
    "pm25":25.87332214661367
  }
}
```

### <a name="devices"></a>Cihazlar

Cihaz verileri içeren iletiler, her birkaç dakikada bir olay hub 'ınıza veya Service Bus kuyruğuna veya konuya gönderilir. Blob depolamaya dakikada bir kez yeni bir anlık görüntü yazılır. Her ileti veya anlık görüntü aşağıdakiler hakkındaki verileri içerir:

- Eklenen yeni cihazlar
- Değiştirilen özellik değerleri olan cihazlar

Anlık görüntüdeki her ileti veya kayıt, son dışarıya alınan iletiden bu yana bir cihazdaki bir veya daha fazla değişikliği temsil eder. Bilgiler şunları içerir:

- IoT Central cihaz `@id`
- Cihazın `name`
- [cihaz sağlama hizmeti](https://aka.ms/iotcentraldocsdps) 'nden `deviceId`
- Cihaz şablonu bilgileri
- Özellik değerleri

Son Batch, bu yana silinen cihazlar tarafından silindi. Şu anda, silinen cihazlar için, aktarılmış iletilerde hiçbir gösterge yok.

Her cihazın ait olduğu cihaz şablonu, bir cihaz şablonu KIMLIĞI ile temsil edilir. Cihaz şablonunun adını almak için, cihaz şablonu verilerini de dışarı aktardığınızdan emin olun.

Aşağıdaki örnek, Olay Hub 'ındaki veya Service Bus kuyruğu veya konusunun cihaz verileri hakkında bir ileti gösterir:

```json
{
  "body":{
    "@id":"<id>",
    "@type":"Device",
    "displayName":"Airbox - 266d30aedn5",
    "data":{
      "$cloudProperties":{
        "Color":"blue"
      },
      "EnvironmentalSensor":{
        "thsensormodel":{
          "reported":{
            "value":"A1",
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        },
        "pm25sensormodel":{
          "reported":{
            "value":"P1",
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        }
      },
      "urn_azureiot_DeviceManagement_DeviceInformation":{
        "totalStorage":{
          "reported":{
            "value":3088.1959855710156,
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        },
        "totalMemory":{
          "reported":{
            "value":16005.703586477555,
            "$lastUpdatedTimestamp":"2019-10-02T18:14:49.3820326Z"
          }
        }
      }
    },
    "instanceOf":"<templateId>",
    "deviceId":"<deviceId>",
    "simulated":true
  },
  "annotations":{
    "iotcentral-message-source":"devices",
    "x-opt-partition-key":"<partitionKey>",
    "x-opt-sequence-number":39740,
    "x-opt-offset":"<offset>",
    "x-opt-enqueued-time":1539274959654
  },
  "partitionKey":"<partitionKey>",
  "sequenceNumber":39740,
  "enqueuedTimeUtc":"2019-10-02T18:14:49.3820326Z",
  "offset":"<offset>"
}
```

İçe aktarılmış dosyalar kayıt başına tek bir satır içerir. Aşağıdaki örnekte JSON biçimindeki bir kayıt gösterilmektedir.

```json
{
  "@id":"<id-value>",
  "@type":"Device",
  "displayName":"Airbox",
  "data":{
    "$cloudProperties":{
        "Color":"blue"
    },
    "EnvironmentalSensor":{
      "thsensormodel":{
        "reported":{
          "value":"Neque quia et voluptatem veritatis assumenda consequuntur quod.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "pm25sensormodel":{
        "reported":{
          "value":"Aut alias odio.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    },
    "urn_azureiot_DeviceManagement_DeviceInformation":{
      "totalStorage":{
        "reported":{
          "value":27900.9730905171,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "totalMemory":{
        "reported":{
          "value":4667.82916715811,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    }
  },
  "instanceOf":"<template-id>",
  "deviceId":"<device-id>",
  "simulated":true
}
```

### <a name="device-templates"></a>Cihaz şablonları

Cihaz şablonu verilerini içeren iletiler, her birkaç dakikada bir olay hub 'ınıza veya Service Bus kuyruğuna veya konuya gönderilir. Blob depolamaya dakika başına yeni bir anlık görüntü yazılır. Bu nedenle, birkaç dakikada bir ileti toplu işi ile ilgili veriler hakkında bilgi alındığında:

Cihaz şablonu verilerini içeren iletiler, her birkaç dakikada bir olay hub 'ınıza veya Service Bus kuyruğuna veya konuya gönderilir. Blob depolamaya dakikada bir kez yeni bir anlık görüntü yazılır. Her ileti veya anlık görüntü aşağıdakiler hakkındaki verileri içerir:

- Eklenen veya sürümlü yeni cihaz şablonları
- Değiştirilen yetenek modelleri, bulut özellikleri, geçersiz kılmalar ve başlangıç değerleri içeren cihaz şablonları

Her ileti veya anlık görüntü kaydı, son içe aktarma iletisinden bu yana cihaz şablonunda yapılan bir veya daha fazla değişikliği temsil eder. Her ileti veya kayıtta gönderilen bilgiler şunları içerir:

- cihaz şablonunun `@id`
- cihaz şablonunun `name`
- cihaz şablonunun `version`
- Cihaz, `interfaces`ve telemetri, Özellikler ve komut tanımlarını dahil `capabilityModel`.
- `cloudProperties` tanımları
- `capabilityModel` satır içi olarak geçersiz kılmalar ve başlangıç değerleri

Son Batch 'in aktarılmasından bu yana cihaz şablonları silindi. Şu anda, Silinen cihaz şablonları için, aktarılmış iletilerde hiçbir gösterge yok.

Aşağıdaki örnekte, Olay Hub 'ında veya Service Bus kuyruğu veya konusunun bir cihaz şablonu iletisi gösterilmektedir:

```json
{
  "body":{
    "@id":"<template-id>",
    "@type":"DeviceModelDefinition",
    "displayName":"Airbox",
    "capabilityModel":{
      "@id":"<id>",
      "@type":"CapabilityModel",
      "implements":[
        {
          "@id":"<id>",
          "@type":"InterfaceInstance",
          "name":"EnvironmentalSensor",
          "schema":{
            "@id":"<id>",
            "@type":"Interface",
            "comment":"Requires temperature and humidity sensors.",
            "description":"Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
            "displayName":"Environmental Sensor",
            "contents":[
              {
                "@id":"<id>",
                "@type":"Telemetry",
                "description":"Current temperature on the device",
                "displayName":"Temperature",
                "name":"temp",
                "schema":"double",
                "unit":"Units/Temperature/celsius",
                "valueDetail":{
                  "@id":"<id>",
                  "@type":"ValueDetail/NumberValueDetail",
                  "minValue":{
                    "@value":"50"
                  }
                },
                "visualizationDetail":{
                  "@id":"<id>",
                  "@type":"VisualizationDetail"
                }
              },
              {
                "@id":"<id>",
                "@type":"Telemetry",
                "description":"Current humidity on the device",
                "displayName":"Humidity",
                "name":"humid",
                "schema":"integer"
              },
              {
                "@id":"<id>",
                "@type":"Telemetry",
                "description":"Current PM2.5 on the device",
                "displayName":"PM2.5",
                "name":"pm25",
                "schema":"integer"
              },
              {
                "@id":"<id>",
                "@type":"Property",
                "description":"T&H Sensor Model Name",
                "displayName":"T&H Sensor Model",
                "name":"thsensormodel",
                "schema":"string"
              },
              {
                "@id":"<id>",
                "@type":"Property",
                "description":"PM2.5 Sensor Model Name",
                "displayName":"PM2.5 Sensor Model",
                "name":"pm25sensormodel",
                "schema":"string"
              }
            ]
          }
        },
        {
          "@id":"<id>",
          "@type":"InterfaceInstance",
          "name":"urn_azureiot_DeviceManagement_DeviceInformation",
          "schema":{
            "@id":"<id>",
            "@type":"Interface",
            "displayName":"Device information",
            "contents":[
              {
                "@id":"<id>",
                "@type":"Property",
                "comment":"Total available storage on the device in kilobytes. Ex. 20480000 kilobytes.",
                "displayName":"Total storage",
                "name":"totalStorage",
                "displayUnit":"kilobytes",
                "schema":"long"
              },
              {
                "@id":"<id>",
                "@type":"Property",
                "comment":"Total available memory on the device in kilobytes. Ex. 256000 kilobytes.",
                "displayName":"Total memory",
                "name":"totalMemory",
                "displayUnit":"kilobytes",
                "schema":"long"
              }
            ]
          }
        }
      ],
      "displayName":"AAEONAirbox52"
    },
    "solutionModel":{
      "@id":"<id>",
      "@type":"SolutionModel",
      "cloudProperties":[
        {
          "@id":"<id>",
          "@type":"CloudProperty",
          "displayName":"Color",
          "name":"Color",
          "schema":"string",
          "valueDetail":{
            "@id":"<id>",
            "@type":"ValueDetail/StringValueDetail"
          },
          "visualizationDetail":{
            "@id":"<id>",
            "@type":"VisualizationDetail"
          }
        }
      ]
    },
    "annotations":{
      "iotcentral-message-source":"deviceTemplates",
      "x-opt-partition-key":"<partitionKey>",
      "x-opt-sequence-number":25315,
      "x-opt-offset":"<offset>",
      "x-opt-enqueued-time":1539274985085
    },
    "partitionKey":"<partitionKey>",
    "sequenceNumber":25315,
    "enqueuedTimeUtc":"2019-10-02T16:23:05.085Z",
    "offset":"<offset>"
  }
}
```

İçe aktarılmış dosyalar kayıt başına tek bir satır içerir. Aşağıdaki örnekte JSON biçimindeki bir kayıt gösterilmektedir.

```json
{
  "@id":"<template-id>",
  "@type":"DeviceModelDefinition",
  "displayName":"Airbox",
  "capabilityModel":{
    "@id":"<id>",
    "@type":"CapabilityModel",
    "implements":[
      {
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"EnvironmentalSensor",
        "schema":{
          "@id":"<id>",
          "@type":"Interface",
          "comment":"Requires temperature and humidity sensors.",
          "description":"Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
          "displayName":"Environmental Sensor",
          "contents":[
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current temperature on the device",
              "displayName":"Temperature",
              "name":"temp",
              "schema":"double",
              "unit":"Units/Temperature/celsius",
              "valueDetail":{
                "@id":"<id>",
                "@type":"ValueDetail/NumberValueDetail",
                "minValue":{
                  "@value":"50"
                }
              },
              "visualizationDetail":{
                "@id":"<id>",
                "@type":"VisualizationDetail"
              }
            },
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current humidity on the device",
              "displayName":"Humidity",
              "name":"humid",
              "schema":"integer"
            },
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current PM2.5 on the device",
              "displayName":"PM2.5",
              "name":"pm25",
              "schema":"integer"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "description":"T&H Sensor Model Name",
              "displayName":"T&H Sensor Model",
              "name":"thsensormodel",
              "schema":"string"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "description":"PM2.5 Sensor Model Name",
              "displayName":"PM2.5 Sensor Model",
              "name":"pm25sensormodel",
              "schema":"string"
            }
          ]
        }
      },
      {
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"urn_azureiot_DeviceManagement_DeviceInformation",
        "schema":{
          "@id":"<id>",
          "@type":"Interface",
          "displayName":"Device information",
          "contents":[
            {
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available storage on the device in kilobytes. Ex. 20480000 kilobytes.",
              "displayName":"Total storage",
              "name":"totalStorage",
              "displayUnit":"kilobytes",
              "schema":"long"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available memory on the device in kilobytes. Ex. 256000 kilobytes.",
              "displayName":"Total memory",
              "name":"totalMemory",
              "displayUnit":"kilobytes",
              "schema":"long"
            }
          ]
        }
      }
    ],
    "displayName":"AAEONAirbox52"
  },
  "solutionModel":{
    "@id":"<id>",
    "@type":"SolutionModel",
    "cloudProperties":[
      {
        "@id":"<id>",
        "@type":"CloudProperty",
        "displayName":"Color",
        "name":"Color",
        "schema":"string",
        "valueDetail":{
          "@id":"<id>",
          "@type":"ValueDetail/StringValueDetail"
        },
        "visualizationDetail":{
          "@id":"<id>",
          "@type":"VisualizationDetail"
        }
      }
    ]
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Artık verilerinizi Azure Event Hubs ve Azure Service Bus dışarı aktarmayı öğrenmiş olduğunuza göre, bir sonraki adımla devam edin:

> [!div class="nextstepaction"]
> [Azure Işlevleri 'ni tetikleme](howto-trigger-azure-functions.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
