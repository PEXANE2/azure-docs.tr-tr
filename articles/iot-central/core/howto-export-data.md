---
title: Azure IoT Central verilerinizi dışarı aktarın | Microsoft Docs
description: Azure IoT Central uygulamanızdan Azure Event Hubs, Azure Service Bus ve Azure Blob depolama 'ya veri aktarma
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 01/30/2019
ms.topic: conceptual
ms.service: iot-central
manager: corywink
ms.openlocfilehash: 058fe9aea87879fe85dcbc6dcb864fd841fcb049
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026802"
---
# <a name="export-your-azure-iot-central-data"></a>Azure IoT Central verilerinizi dışarı aktarın



*Bu konu, Yöneticiler için geçerlidir.*

Bu makalede verilerinizi **azure Event Hubs**, **Azure Service Bus**veya **Azure Blob depolama** örneklerine aktarmak için Azure IoT Central sürekli veri dışa aktarma özelliğinin nasıl kullanılacağı açıklanır. Veriler JSON biçiminde verilir ve Telemetriyi, cihaz bilgilerini ve cihaz şablonu bilgilerini içerebilir. İçin, bu verileri kullan:

- Sıcak yol öngörüleri ve analizi. Bu seçenek, Azure Stream Analytics özel kuralların tetiklenmesi, Azure Logic Apps özel iş akışlarını tetiklenmesi veya dönüştürülecek Azure Işlevleri aracılığıyla geçirilmesidir.
- Microsoft Power BI 'de Azure Machine Learning veya uzun süreli eğilim analizinde eğitim modelleri gibi soğuk yol analizi.

> [!Note]
> Sürekli veri dışarı aktarmayı açtığınızda, yalnızca o andan itibaren verileri alırsınız. Şu anda, sürekli veri dışa aktarma kapalı olduğunda veriler bir saat için alınamaz. Daha fazla geçmiş verileri sürdürmek için sürekli veri dışa aktarmayı erken açın.

## <a name="prerequisites"></a>Ön koşullar

IoT Central uygulamanızda yönetici olmanız veya veri dışa aktarma izinlerinizin olması gerekir.

## <a name="set-up-export-destination"></a>Dışarı aktarma hedefini ayarla

Sürekli veri dışa aktarmayı yapılandırmadan önce dışa aktarma hedefinin mevcut olması gerekir.

### <a name="create-event-hubs-namespace"></a>Event Hubs ad alanı oluştur

Uygulamasına dışarı aktarmak için mevcut bir Event Hubs ad alanınız yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir Event Hubs ad alanı](https://ms.portal.azure.com/#create/Microsoft.EventHub)oluşturun. [Azure Event Hubs docs](../../event-hubs/event-hubs-create.md)'ta daha fazla bilgi edinebilirsiniz.

2. Bir abonelik seçin. IoT Central uygulamanızla aynı abonelikte olmayan diğer aboneliklerdeki verileri dışarı aktarabilirsiniz. Bu durumda bir bağlantı dizesi kullanarak bağlanırsınız.

3. Event Hubs ad alanında bir olay hub 'ı oluşturun. Ad alanına gidin ve bir olay hub 'ı örneği oluşturmak için en üstteki **+ Olay Hub 'ını** seçin.

### <a name="create-service-bus-namespace"></a>Service Bus ad alanı oluştur

Uygulamasına dışarı aktarmak için mevcut bir Service Bus ad alanınız yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir Service Bus ad alanı](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5)oluşturun. [Azure Service Bus docs](../../service-bus-messaging/service-bus-create-namespace-portal.md)' de daha fazla bilgi edinebilirsiniz.
2. Bir abonelik seçin. IoT Central uygulamanızla aynı abonelikte olmayan diğer aboneliklerdeki verileri dışarı aktarabilirsiniz. Bu durumda bir bağlantı dizesi kullanarak bağlanırsınız.

3. Service Bus ad alanına gidin ve en üstteki **+ kuyruk** veya **+ Konu** başlığını seçerek dışarı aktarılacak bir kuyruk veya konu oluşturun.

Dışarı aktarma hedefi olarak Service Bus seçtiğinizde, kuyruklar ve konuların oturumları veya yinelenen algılama özelliği etkin olmalıdır. Bu seçeneklerden biri etkinleştirilirse, bazı iletiler kuyruğunuza veya konuya ulaşmayacaktır.

### <a name="create-storage-account"></a>Depolama hesabı oluştur

' A dışa aktarılacak mevcut bir Azure depolama hesabınız yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir depolama hesabı](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)oluşturun. Yeni [Azure Blob depolama hesapları](https://aka.ms/blobdocscreatestorageaccount) veya [Azure Data Lake Storage v2 depolama hesapları](../../storage/blobs/data-lake-storage-quickstart-create-account.md)oluşturma hakkında daha fazla bilgi edinebilirsiniz.

    - Verileri bir Azure Data Lake Storage v2 depolama hesabına aktarmayı seçerseniz, **Hesap türü**olarak **blobstorage** ' ı seçmeniz gerekir.
    - IoT Central uygulamanız için olandan farklı aboneliklerdeki depolama hesaplarına veri aktarabilirsiniz. Bu durumda bir bağlantı dizesi kullanarak bağlanacaksınız.

2. Depolama hesabınızda bir kapsayıcı oluşturun. Depolama hesabınıza gidin. **BLOB hizmeti**altında **bloblara gözatamıyorum**' ı seçin. Yeni bir kapsayıcı oluşturmak için üst kısımdaki **+ kapsayıcı** ' yı seçin.

## <a name="set-up-continuous-data-export"></a>Sürekli veri dışarı aktarma ayarlama

Verilerin dışarı aktarılacağı bir hedef olduğuna göre, sürekli veri dışa aktarma ayarlamak için aşağıdaki adımları izleyin.

1. IoT Central uygulamanızda oturum açın.

2. Sol bölmede **veri dışa aktar**' ı seçin.

    > [!Note]
    > Sol bölmede veri dışa aktarma 'yı görmüyorsanız, uygulamanızda veri dışarı aktarma yapılandırma izniniz yok demektir. Verilerin dışarı aktarılmasını ayarlamak için bir yöneticiye danışın.

3. Sağ üst köşedeki **+ Yeni** düğmesini seçin. Dışarı aktarmanın hedefi olarak **azure Event Hubs**, **Azure Service Bus**veya **Azure Blob depolamadan** birini seçin. Uygulama başına en fazla dışarı aktarma sayısı beştir.

    ![Yeni sürekli veri dışa aktarma oluştur](media/howto-export-data/new-export-definition.png)

4. Aşağı açılan liste kutusunda **Event Hubs ad**alanınızı, **Service Bus ad alanını**, **depolama hesabı ad alanını**seçin veya **bir bağlantı dizesi girin**.

    - Depolama hesapları, Event Hubs ad alanları ve Service Bus ad alanlarını IoT Central uygulamanızla aynı abonelikte görürsünüz. Bu aboneliğin dışında bir hedefe aktarmak istiyorsanız, **bir bağlantı dizesi girin** ' i seçin ve 5. adıma bakın.
    - Ücretsiz fiyatlandırma planı kullanılarak oluşturulan uygulamalar için sürekli veri vermeyi yapılandırmanın tek yolu bir bağlantı dizesidir. Ücretsiz fiyatlandırma planındaki uygulamalar ilişkili bir Azure aboneliğine sahip değildir.

    ![Yeni Olay Hub 'ı oluştur](media/howto-export-data/export-event-hub.png)

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

8. Sürekli veri dışa aktarmayı açmak için **etkin** geçiş özelliğinin **Açık**olduğundan emin olun. **Kaydet**’i seçin.

9. Birkaç dakika sonra verileriniz seçtiğiniz hedefte görüntülenir.

## <a name="export-contents-and-format"></a>İçeriği ve biçimi dışarı aktar

İçe aktarılmış telemetri verileri, yalnızca telemetri değerlerinin kendileri değil, cihazlarınızın IoT Central gönderildiği iletinin tamamını içerir. İçe aktarılmış cihazlar verileri tüm cihazların özelliklerine ve meta verilerine yapılan değişiklikleri içerir ve bu cihaz şablonları tüm cihaz şablonlarında değişiklikler içerir.

Event Hubs ve Service Bus için veriler neredeyse gerçek zamanlı olarak verilir. Veriler gövde özelliğinde bulunur ve JSON biçimindedir (örnekler için aşağıya bakın).

BLOB depolama için, veriler dakikada bir kez, son içe aktarma dosyasından bu yana yapılan değişiklikleri içeren her bir dosyayla birlikte verilir. Verilen veriler JSON biçiminde üç klasöre yerleştirilir. Depolama hesabınızdaki varsayılan yollar şunlardır:

- Telemetri: _{Container}/{app-id}/telemetry/{yyyy}/{mm}/{dd}/{hh}/{mm}/{filename}_
- Cihazlar: _{Container}/{app-id}/Devices/{yyyy}/{mm}/{dd}/{ss}/{mm}/{filename}_
- Cihaz şablonları: _{Container}/{App-ID}/devicetemplates/{yyyy}/{mm}/{dd}/{ss}/{mm}/{filename}_

Dosyaya giderek ve **blobu Düzenle** sekmesini seçerek Azure Portal dışarıya aktarılmış dosyalara gidebilirsiniz.


## <a name="telemetry"></a>Telemetri

Event Hubs ve Service Bus için yeni bir ileti bir cihazdan ileti aldıktan IoT Central sonra hızlı bir şekilde verilir ve her bir dışarıya ileti, bir cihazın JSON biçiminde gövde özelliğinde gönderildiği tam iletiyi içerir.

BLOB depolama için, iletiler toplu ve dakikada bir kez verilir. İçeri aktarılmış dosyalar, blob depolamaya [IoT Hub ileti yönlendirme tarafından içeri](../../iot-hub/tutorial-routing.md) aktarılmış ileti dosyalarıyla aynı biçimi kullanır. 

> [!NOTE]
> BLOB depolama için, cihazlarınızın `contentType: application/JSON` ve `contentEncoding:utf-8` (veya `utf-16`, `utf-32`) sahip iletiler göndermesini sağlayın. Bir örnek için [IoT Hub belgelerine](../../iot-hub/iot-hub-devguide-routing-query-syntax.md#message-routing-query-based-on-message-body) bakın.

Telemetriyi Gönderen cihaz, cihaz KIMLIĞI tarafından temsil edilir (aşağıdaki bölümlere bakın). Cihazların adlarını almak için, cihaz verilerini dışarı aktarın ve cihaz iletisinin **DeviceID** 'Siyle eşleşen **connectiondeviceıd** 'yi kullanarak her iletiyi ilişkilendirin.

Bu, bir olay hub 'ında veya Service Bus kuyruğu veya konusunun aldığı örnek bir iletidir.

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

Bu, blob depolamaya aktarılmış bir örnek kaydıdır:

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

## <a name="devices"></a>Cihazlar

Anlık görüntüdeki her ileti veya kayıt, bir cihazdaki bir veya daha fazla değişikliği, son dışarıya alınan iletiden bu yana cihaz ve bulut özelliklerini temsil eder. Buna aşağıdakiler dahildir:

- IoT Central cihaz `id`
- Cihazın `displayName`
- `instanceOf` cihaz şablonu kimliği
- `simulated` bayrak, cihaz sanal bir cihaz ise doğru
- `provisioned` bayrak, cihaz sağlandıysa doğru
- `approved` bayrak, cihaz veri göndermek üzere onaylanmışsa doğru
- Özellik değerleri
- Cihaz ve bulut özellikleri değerlerini dahil `properties`

Silinen cihazlar aktarılmaz. Şu anda, silinen cihazlar için, aktarılmış iletilerde hiçbir gösterge yok.

Event Hubs ve Service Bus için, cihaz verilerini içeren iletiler, IoT Central ' de göründüğü gibi, neredeyse gerçek zamanlı olarak olay hub 'ınıza veya Service Bus kuyruğuna veya konusuna gönderilir. 

BLOB depolama için, en son yazılan son bir dakikada bir kez aktarıldığından, tüm değişiklikleri içeren yeni bir anlık görüntü

Bu, Olay Hub 'ında veya Service Bus kuyruğu veya konusunun cihaz ve özellik verileri hakkında örnek bir iletidir:

```json
{
  "body":{
    "id": "<device Id>",
    "etag": "<etag>",
    "displayName": "Sensor 1",
    "instanceOf": "<device template Id>",
    "simulated": false,
    "provisioned": true,
    "approved": true,
    "properties": {
        "sensorComponent": {
            "setTemp": "30",
            "fwVersion": "2.0.1",
            "status": { "first": "first", "second": "second" },
            "$metadata": {
                "setTemp": {
                    "desiredValue": "30",
                    "desiredVersion": 3,
                    "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                    "ackVersion": 3
                },
                "fwVersion": { "ackVersion": 3 },
                "status": {
                    "desiredValue": {
                        "first": "first",
                        "second": "second"
                    },
                    "desiredVersion": 2,
                    "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                    "ackVersion": 2
                }
            },
            
        }
    },
    "installDate": { "installDate": "2020-02-01" }
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
  "enqueuedTimeUtc":"2020-02-01T18:14:49.3820326Z",
  "offset":"<offset>"
}
```

Bu, blob depolamada cihazları ve özellik verilerini içeren örnek bir anlık görüntüdür. İçe aktarılmış dosyalar kayıt başına tek bir satır içerir.

```json
{
  "id": "<device Id>",
  "etag": "<etag>",
  "displayName": "Sensor 1",
  "instanceOf": "<device template Id>",
  "simulated": false,
  "provisioned": true,
  "approved": true,
  "properties": {
      "sensorComponent": {
          "setTemp": "30",
          "fwVersion": "2.0.1",
          "status": { "first": "first", "second": "second" },
          "$metadata": {
              "setTemp": {
                  "desiredValue": "30",
                  "desiredVersion": 3,
                  "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                  "ackVersion": 3
              },
              "fwVersion": { "ackVersion": 3 },
              "status": {
                  "desiredValue": {
                      "first": "first",
                      "second": "second"
                  },
                  "desiredVersion": 2,
                  "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                  "ackVersion": 2
              }
          },
          
      }
  },
  "installDate": { "installDate": "2020-02-01" }
}
```

## <a name="device-templates"></a>Cihaz şablonları

Her ileti veya anlık görüntü kaydı, yayımlanan bir cihaz şablonunda yapılan son dışarıya alınan iletiden itibaren bir veya daha fazla değişikliği temsil eder. Her ileti veya kayıtta gönderilen bilgiler şunları içerir:

- Yukarıdaki cihazlar akışının `instanceOf` eşleşen cihaz şablonu `id`
- cihaz şablonunun `displayName`
- Cihaz, `interfaces`ve telemetri, Özellikler ve komut tanımlarını dahil `capabilityModel`.
- `cloudProperties` tanımları
- `capabilityModel` satır içi olarak geçersiz kılmalar ve başlangıç değerleri

Silinen cihaz şablonları aktarılmaz. Şu anda, Silinen cihaz şablonları için, aktarılmış iletilerde hiçbir gösterge yok.

Event Hubs ve Service Bus için, cihaz şablonu verilerini içeren iletiler, IoT Central ' de göründüğü gibi, neredeyse gerçek zamanlı olarak olay hub 'ınıza veya Service Bus kuyruğuna veya konusuna gönderilir. 

BLOB depolama için, en son yazılan son bir dakikada bir kez aktarıldığından, tüm değişiklikleri içeren yeni bir anlık görüntü

Bu, Olay Hub 'ında veya Service Bus kuyruğu veya konusunun cihaz şablonları verileri hakkında örnek bir iletidir:

```json
{
  "body":{
      "id": "<device template id>",
      "etag": "<etag>",
      "types": ["DeviceModel"],
      "displayName": "Sensor template",
      "capabilityModel": {
          "@id": "<capability model id>",
          "@type": ["CapabilityModel"],
          "contents": [],
          "implements": [
              {
                  "@id": "<component Id>",
                  "@type": ["InterfaceInstance"],
                  "name": "sensorComponent",
                  "schema": {
                      "@id": "<interface Id>",
                      "@type": ["Interface"],
                      "displayName": "Sensor interface",
                      "contents": [
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry"],
                              "displayName": "Humidity",
                              "name": "humidity",
                              "schema": "double"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry", "SemanticType/Event"],
                              "displayName": "Error event",
                              "name": "error",
                              "schema": "integer"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Set temperature",
                              "name": "setTemp",
                              "writable": true,
                              "schema": "integer",
                              "unit": "Units/Temperature/fahrenheit",
                              "initialValue": "30"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Firmware version read only",
                              "name": "fwversion",
                              "schema": "string"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Display status",
                              "name": "status",
                              "writable": true,
                              "schema": {
                                  "@id": "urn:testInterface:status:obj:ka8iw8wka:1",
                                  "@type": ["Object"]
                              }
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Command"],
                              "commandType": "synchronous",
                              "request": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Configuration",
                                  "name": "config",
                                  "schema": "string"
                              },
                              "response": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Response",
                                  "name": "response",
                                  "schema": "string"
                              },
                              "displayName": "Configure sensor",
                              "name": "sensorConfig"
                          }
                      ]
                  }
              }
          ],
          "displayName": "Sensor capability model"
      },
      "solutionModel": {
          "@id": "<id>",
          "@type": ["SolutionModel"],
          "cloudProperties": [
              {
                  "@id": "<id>",
                  "@type": ["CloudProperty"],
                  "displayName": "Install date",
                  "name": "installDate",
                  "schema": "dateTime",
                  "valueDetail": {
                      "@id": "<id>",
                      "@type": ["ValueDetail/DateTimeValueDetail"]
                  }
              }
          ]
      }
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

Bu, blob depolamada cihazları ve özellik verilerini içeren örnek bir anlık görüntüdür. İçe aktarılmış dosyalar kayıt başına tek bir satır içerir.

```json
{
      "id": "<device template id>",
      "etag": "<etag>",
      "types": ["DeviceModel"],
      "displayName": "Sensor template",
      "capabilityModel": {
          "@id": "<capability model id>",
          "@type": ["CapabilityModel"],
          "contents": [],
          "implements": [
              {
                  "@id": "<component Id>",
                  "@type": ["InterfaceInstance"],
                  "name": "Sensor component",
                  "schema": {
                      "@id": "<interface Id>",
                      "@type": ["Interface"],
                      "displayName": "Sensor interface",
                      "contents": [
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry"],
                              "displayName": "Humidity",
                              "name": "humidity",
                              "schema": "double"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry", "SemanticType/Event"],
                              "displayName": "Error event",
                              "name": "error",
                              "schema": "integer"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Set temperature",
                              "name": "setTemp",
                              "writable": true,
                              "schema": "integer",
                              "unit": "Units/Temperature/fahrenheit",
                              "initialValue": "30"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Firmware version read only",
                              "name": "fwversion",
                              "schema": "string"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Display status",
                              "name": "status",
                              "writable": true,
                              "schema": {
                                  "@id": "urn:testInterface:status:obj:ka8iw8wka:1",
                                  "@type": ["Object"]
                              }
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Command"],
                              "commandType": "synchronous",
                              "request": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Configuration",
                                  "name": "config",
                                  "schema": "string"
                              },
                              "response": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Response",
                                  "name": "response",
                                  "schema": "string"
                              },
                              "displayName": "Configure sensor",
                              "name": "sensorconfig"
                          }
                      ]
                  }
              }
          ],
          "displayName": "Sensor capability model"
      },
      "solutionModel": {
          "@id": "<id>",
          "@type": ["SolutionModel"],
          "cloudProperties": [
              {
                  "@id": "<id>",
                  "@type": ["CloudProperty"],
                  "displayName": "Install date",
                  "name": "installDate",
                  "schema": "dateTime",
                  "valueDetail": {
                      "@id": "<id>",
                      "@type": ["ValueDetail/DateTimeValueDetail"]
                  }
              }
          ]
      }
  }
```
## <a name="data-format-change-notice"></a>Veri biçimi değişiklik bildirimi

> [!Note]
> Telemetri akış veri biçimi bu değişiklikten etkilenmez. Yalnızca cihazlar ve cihaz şablonu veri akışları etkilenir.

Önizleme uygulamanızda *cihazlar* ve *cihaz şablonları* akışları açık olan bir veri dışa aktarma Işlemi varsa, **30 Haziran 2020 tarihine**kadar dışarı aktarmayı güncelleştirmeniz gerekir. Bu, Azure Blob depolama, Azure Event Hubs ve Azure Service Bus dışarı aktarmalar için geçerlidir.

1 Şubat 2020 ' den başlayarak, cihazlar ve cihaz şablonları etkin olan uygulamalardaki tüm yeni dışarı aktarımlar yukarıda açıklanan veri biçimine sahip olacaktır. Bundan önce oluşturulan tüm dışarı aktarımlar, 30 Haziran 2020 ' e kadar eski veri biçiminde kalır, bu dışarı aktarmalar bu dışarı aktarma işleminden sonra otomatik olarak yeni veri biçimine geçirilir. Yeni veri biçimi, IoT Central ortak API 'sindeki [cihaz](https://docs.microsoft.com/rest/api/iotcentral/devices/get), [cihaz özelliği](https://docs.microsoft.com/rest/api/iotcentral/devices/getproperties), [cihaz bulutu özelliği](https://docs.microsoft.com/rest/api/iotcentral/devices/getcloudproperties) ve [cihaz şablonu](https://docs.microsoft.com/rest/api/iotcentral/devicetemplates/get) nesneleriyle eşleşir. 
 
**Cihazlar**için eski veri biçimi ve yeni veri biçimi arasındaki önemli farklar şunlardır:
- cihaz için `@id` kaldırılır, `deviceId` `id` olarak yeniden adlandırılır 
- Cihazın sağlama durumunu anlatmak için `provisioned` bayrağı eklendi
- Cihazın onay durumunu anlatmak için `approved` bayrağı eklendi
- Cihaz ve bulut özellikleri dahil `properties`, ortak API 'deki varlıklarla eşleşir

**Cihaz şablonlarında**, eski veri biçimi ve yeni veri biçimi arasındaki önemli farklar şunlardır:

- cihaz şablonu için `@id` `id` olarak yeniden adlandırıldı
- cihaz şablonu için `@type` `types`olarak yeniden adlandırılır ve artık bir dizidir

### <a name="devices-format-deprecated-as-of-3-february-2020"></a>Cihazlar (biçim 3 Şubat 2020 itibariyle kullanım dışı)
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

### <a name="device-templates-format-deprecated-as-of-3-february-2020"></a>Cihaz şablonları (biçim 3 Şubat 2020 itibariyle kullanım dışı)
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

Artık verilerinizi Azure Event Hubs, Azure Service Bus ve Azure Blob depolama 'ya aktarmayı öğrenmiş olduğunuza göre, sonraki adıma geçin:

> [!div class="nextstepaction"]
> [Web kancaları oluşturma](./howto-create-webhooks.md)
