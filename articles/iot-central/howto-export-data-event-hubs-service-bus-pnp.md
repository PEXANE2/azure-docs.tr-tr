---
title: Verilerinizi Azure Event Hubs 'ye aktarın ve Azure Service Bus | Microsoft Docs
description: Azure IoT Central uygulamanızdan Azure Event Hubs ve Azure Service Bus verileri dışarı aktarma
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/09/2019
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: fed9c924274cb66671e233a7dc6d431d81e0dbfb
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973196"
---
# <a name="export-your-data-in-azure-iot-central-preview-features"></a>Verilerinizi Azure IoT Central dışarı aktarma (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

*Bu konu, Yöneticiler için geçerlidir.*

Bu makalede verilerinizi kendi **azure Event Hubs**ve **Azure Service Bus** örneklerine aktarmak için Azure IoT Central sürekli veri dışa aktarma özelliğinin nasıl kullanılacağı açıklanır. Sıcak yol öngörüleri ve analizi için **telemetri**, **cihaz**ve **cihaz şablonlarını** kendi hedefinize dışarı aktarabilirsiniz. Bu, Azure Stream Analytics özel kuralların tetiklenmesi, Azure Logic Apps özel iş akışlarını tetiklenmesi veya dönüştürülecek Azure Işlevleri aracılığıyla geçirilmesidir.

> [!Note]
> Bir kez daha, sürekli veri dışarı aktarmayı açtığınızda, yalnızca o andan itibaren verileri alırsınız. Şu anda, sürekli veri dışa aktarma kapalı olduğunda veriler bir saat için alınamaz. Daha fazla geçmiş verileri sürdürmek için sürekli veri dışa aktarmayı erken açın.

## <a name="prerequisites"></a>Önkoşullar

- IoT Central uygulamanızda yönetici olmanız gerekir

## <a name="set-up-export-destination"></a>Dışarı aktarma hedefini ayarla

Uygulamasına dışarı aktarmak için mevcut bir Event Hubs/Service Bus yoksa, oluşturmak için aşağıdaki adımları izleyin:

### <a name="create-event-hubs-namespace"></a>Event Hubs ad alanı oluştur

1. [Azure Portal yeni bir Event Hubs ad alanı](https://ms.portal.azure.com/#create/Microsoft.EventHub)oluşturun. [Azure Event Hubs docs](https://docs.microsoft.com/azure/event-hubs/event-hubs-create)'ta daha fazla bilgi edinebilirsiniz.
2. Bir abonelik seçin. 

    > [!Note] 
    > Artık verileri, Kullandıkça Öde IoT Central uygulamanızla **aynı olmayan** diğer aboneliklerle dışarı aktarabilirsiniz. Bu durumda bir bağlantı dizesi kullanarak bağlanacaksınız.
3. Event Hubs ad alanında bir olay hub 'ı oluşturun. Ad alanına gidin ve bir olay hub 'ı örneği oluşturmak için en üstteki **+ Olay Hub 'ını** seçin.

### <a name="create-service-bus-namespace"></a>Service Bus ad alanı oluştur

1. [Azure Portal yeni bir Service Bus ad alanı](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5) oluşturun. [Azure Service Bus docs](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)' de daha fazla bilgi edinebilirsiniz.
2. Bir abonelik seçin. 

    > [!Note] 
    > Artık verileri, Kullandıkça Öde IoT Central uygulamanızla **aynı olmayan** diğer aboneliklerle dışarı aktarabilirsiniz. Bu durumda bir bağlantı dizesi kullanarak bağlanacaksınız.

3. Service Bus ad alanına gidin ve en üstteki **+ kuyruk** veya **+ Konu** başlığını seçerek dışarı aktarılacak bir kuyruk veya konu oluşturun.


## <a name="set-up-continuous-data-export"></a>Sürekli veri dışarı aktarma ayarlama

Verilerin dışarı aktarılacağı Event Hubs/Service Bus hedefine sahip olduğunuza göre, sürekli veri dışa aktarma ayarlamak için aşağıdaki adımları izleyin. 

1. IoT Central uygulamanızda oturum açın.

2. Sol taraftaki menüden **veri dışarı aktarma**' yı seçin.

    > [!Note]
    > Sol menüde veri dışa aktarma görmüyorsanız, uygulamanızda yönetici değilsiniz demektir. Verilerin dışarı aktarılmasını ayarlamak için bir yöneticiye danışın.

3. Sağ üst köşedeki **+ Yeni** düğmesini seçin. Dışarı aktarmanın hedefi olarak **Azure Event Hubs** veya **Azure Service Bus** birini seçin. 

    > [!NOTE] 
    > Uygulama başına en fazla dışarı aktarma sayısı beştir. 

    ![Yeni sürekli veri dışa aktarma oluştur](media/howto-export-data-pnp/export-new2.png)

4. Aşağı açılan liste kutusunda **Event Hubs ad alanı/Service Bus ad alanını**seçin. Ayrıca, listede **bir bağlantı dizesi girerek**son seçeneği de seçebilirsiniz. 

    > [!NOTE] 
    > Depolama hesapları/Event Hubs ad alanları/Service Bus ad alanlarını **IoT Central uygulamanız ile aynı abonelikte**görürsünüz. Bu aboneliğin dışında bir hedefe aktarmak istiyorsanız, **bir bağlantı dizesi girin** ' i seçin ve 5. adıma bakın.

    > [!NOTE] 
    > 7 günlük deneme uygulamaları için sürekli veri vermeyi yapılandırmanın tek yolu bir bağlantı dizesidir. Bunun nedeni 7 günlük deneme uygulamalarının ilişkili bir Azure aboneliğine sahip olmaması olabilir.

    ![Yeni CDE Olay Hub 'ı oluştur](media/howto-export-data-pnp/export-eh.png)

5. Seçim **Bir bağlantı dizesi girin**' i seçerseniz, Bağlantı dizenizi yapıştırmanız için yeni bir kutu belirir. İçin bağlantı dizesini almak için:
    - Event Hubs veya Service Bus, Azure portal ad alanına gidin.
        - **Ayarlar**altında, **paylaşılan erişim ilkeleri** ' ni seçin.
        - Varsayılan **RootManageSharedAccessKey** seçin veya yeni bir tane oluşturun
        - Birincil veya ikincil bağlantı dizesini kopyalayın

6. Açılan liste kutusundan bir olay hub 'ı/kuyruğu veya konu başlığını seçin.

7. **Dışarı aktarılacak veriler**' in altında, türü **üzerine**ayarlayarak dışarı aktarılacak her bir veri türünü belirtin.

8. Sürekli veri dışa aktarmayı açmak için, **veri dışa aktarma** geçişi 'nin **Açık**olduğundan emin olun. **Kaydet**’i seçin.

9. Birkaç dakika sonra verileriniz seçtiğiniz hedefte görüntülenir.


## <a name="data-format"></a>Veri biçimi

Telemetri, cihaz ve cihaz şablonları verileri, Olay Hub 'ınıza veya Service Bus kuyruğuna veya konuya neredeyse gerçek zamanlı olarak verilir. İçe aktarılmış telemetri verileri, yalnızca telemetri değerlerinin kendileri değil, cihazlarınızın IoT Central gönderildiği iletinin tamamını içerir. İçe aktarılmış cihazlar verileri tüm cihazların özelliklerine ve meta verilerine yapılan değişiklikleri içerir ve bu cihaz şablonları tüm cihaz şablonlarında değişiklikler içerir. "Body" özelliği olan ve JSON biçiminde olan veriler.

> [!NOTE]
> Dışarı aktarma hedefi olarak bir Service Bus seçerken, kuyruklar ve konuların **oturumları veya yinelenen algılama özelliği etkin olmalıdır**. Bu seçeneklerden biri etkinleştirilirse, bazı iletiler kuyruğunuza veya konuya ulaşmayacaktır.

### <a name="telemetry"></a>Telemetri

IoT Central bir cihazdan ileti aldıktan sonra yeni bir ileti hızlı bir şekilde verilir. Event Hubs ve Service Bus içindeki her bir dışarıya gönderilen ileti, cihazın JSON biçimindeki "Body" özelliğinde gönderildiği tam iletiyi içerir. 

> [!NOTE]
> Telemetriyi gönderen cihazlar cihaz kimlikleri tarafından temsil edilir (aşağıdaki bölümlere bakın). Cihazların adlarını almak için, cihaz verilerini dışa aktarın ve cihaz iletisinin **DeviceID** 'Siyle eşleşen **connectiondeviceıd** 'yi kullanarak her bir ileti ile bağıntı kurun.

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

### <a name="devices"></a>Cihazlar

Cihaz verileri içeren iletiler, her birkaç dakikada bir olay hub 'ınıza veya Service Bus kuyruğuna veya konuya gönderilir. Bu, birkaç dakika içinde, bir dizi iletinin bir toplu iş hakkında verilerle birlikte geldiğini
- Eklenen yeni cihazlar
- Değiştirilen özellik değerleri olan cihazlar

Her ileti, son dışarıya alınan iletiden bu yana bir cihazdaki bir veya daha fazla değişikliği temsil eder. Her iletide gönderilecek bilgiler şunları içerir:
- IoT Central cihazın `@id`
- @no__t-cihazın 0
- [cihaz sağlama hizmeti](https://aka.ms/iotcentraldocsdps) 'nden 0 @no__t
- Cihaz şablonu bilgileri
- Özellik değerleri

> [!NOTE]
> Son Batch, bu yana silinen cihazlar tarafından silindi. Şu anda, silinen cihazlar için, aktarılmış iletilerde hiçbir gösterge yok.
>
> Her cihazın ait olduğu cihaz şablonu, bir cihaz şablonu KIMLIĞI ile temsil edilir. Cihaz şablonunun adını almak için, cihaz şablonu verilerini de dışarı aktardığınızdan emin olun.

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

### <a name="device-templates"></a>Cihaz şablonları

Cihaz şablonu verilerini içeren iletiler, her birkaç dakikada bir olay hub 'ınıza veya Service Bus kuyruğuna veya konuya gönderilir. Bu, birkaç dakika içinde, bir dizi iletinin bir toplu iş hakkında verilerle birlikte geldiğini
- Eklenen veya sürümlü yeni cihaz şablonları
- Değiştirilen Capabilitymodellerini, cloudProperties, geçersiz kılmalar ve başlangıç değerlerini içeren cihaz şablonları

Her ileti, son dışarıya alınan iletiden bu yana bir cihaz şablonunda yapılan bir veya daha fazla değişikliği temsil eder. Her iletide gönderilecek bilgiler şunları içerir:
- cihaz şablonunun `@id`
- cihaz şablonunun `name`
- cihaz şablonunun `version`
- @No__t-0 ' ı `interfaces` ' i ve telemetri, Özellikler ve komut tanımlarını içeren cihaz
- `cloudProperties` tanımları
- @No__t-0 ile satır içi olarak geçersiz kılmalar ve başlangıç değerleri

> [!NOTE]
> Son Batch 'in aktarılmasından bu yana cihaz şablonları silindi. Şu anda, Silinen cihaz şablonları için, aktarılmış iletilerde hiçbir gösterge yok.

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

## <a name="next-steps"></a>Sonraki adımlar

Artık verilerinizi Azure Event Hubs ve Azure Service Bus dışarı aktarmayı öğrenmiş olduğunuza göre, bir sonraki adımla devam edin:

> [!div class="nextstepaction"]
> [Azure Işlevleri 'ni tetikleme](howto-trigger-azure-functions.md)
