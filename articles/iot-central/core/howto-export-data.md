---
title: Azure IoT Merkezi verilerinizi dışa aktarma | Microsoft Dokümanlar
description: Azure IoT Merkezi uygulamanızdan verileri Azure Etkinlik Hub'larına, Azure Hizmet Veri Veri Stor'larına ve Azure Blob depolama alanına nasıl dışa aktarırım?
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 04/07/2020
ms.topic: how-to
ms.service: iot-central
manager: corywink
ms.openlocfilehash: c83c97aab43b6978922202cc96ff92e1e046a7e2
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811631"
---
# <a name="export-iot-data-to-destinations-in-azure"></a>IoT verilerini Azure'daki hedeflere dışa aktarma

*Bu konu yöneticiler için geçerlidir.*

Bu makalede, Azure IoT Central'daki veri dışa aktarma özelliğinin nasıl kullanılacağı açıklanmaktadır. Bu özellik, verilerinizi sürekli olarak **Azure Etkinlik Hub'larına,** **Azure Hizmet Veri Tos'una**veya **Azure Blob depolama** örneklerine aktarmanızı sağlar. Veri dışa aktarma JSON biçimini kullanır ve telemetri, aygıt bilgileri ve aygıt şablonu bilgilerini içerebilir. Dışa aktarılan verileri aşağıdakiler için kullanın:

- Sıcak yol öngörüleri ve analizler. Bu seçenek, Azure Akış Analizi'nde özel kuralları tetikleme, Azure Logic Apps'ta özel iş akışlarını tetikleme veya dönüştürülmek üzere Azure İşlerinden geçirmeyi içerir.
- Azure Machine Learning'deki eğitim modelleri veya Microsoft Power BI'de uzun vadeli eğilim analizi gibi soğuk yol analitiği.

> [!Note]
> Veri dışa aktarmayı açtığınızda, yalnızca o andan itibaren verileri alırsınız. Şu anda, veri dışa aktarmanın kapalı olduğu bir süre için veri alınamaz. Daha fazla geçmiş veri tutmak için veri dışa aktarmasını erken açın.

## <a name="prerequisites"></a>Ön koşullar

IoT Central uygulamanızda yönetici olmalısınız veya Veri verme izinleriniz olmalıdır.

## <a name="set-up-export-destination"></a>İhracat hedefini ayarlama

Veri dışa aktarma yapınızı yapılandırmadan önce dışa aktarma hedefinizin bulunması gerekir.

### <a name="create-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Dışa aktaracak varolan bir Olay Hub'ı ad alanınız yoksa aşağıdaki adımları izleyin:

1. Azure [portalında yeni bir Etkinlik Hub'ları ad alanı](https://ms.portal.azure.com/#create/Microsoft.EventHub)oluşturun. [Azure Etkinlik Hub'ları dokümanlarında](../../event-hubs/event-hubs-create.md)daha fazla bilgi edinebilirsiniz.

2. Bir abonelik seçin. Verileri, IoT Central uygulamanızla aynı abonelikte olmayan diğer aboneliklere aktarabilirsiniz. Bu durumda bir bağlantı dizesi kullanarak bağlanırsınız.

3. Olay Hub'ları ad alanınızda bir olay hub'ı oluşturun. Ad alanınıza gidin ve olay merkezi örneği oluşturmak için en üstte **+ Event Hub'ı** seçin.

### <a name="create-service-bus-namespace"></a>Hizmet Veri Günü ad alanı oluşturma

Dışa aktaracak varolan bir Hizmet Veri Günü ad alanınız yoksa aşağıdaki adımları izleyin:

1. Azure [portalında yeni](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5)bir Hizmet Veri Günü ad alanı oluşturun. [Azure Hizmet Veri Yolu dokümanlarında](../../service-bus-messaging/service-bus-create-namespace-portal.md)daha fazla bilgi edinebilirsiniz.
2. Bir abonelik seçin. Verileri, IoT Central uygulamanızla aynı abonelikte olmayan diğer aboneliklere aktarabilirsiniz. Bu durumda bir bağlantı dizesi kullanarak bağlanırsınız.

3. Dışa aktarmak için bir kuyruk veya konu oluşturmak için Servis Veri Aracı ad alanınıza gidin ve + Sıra veya **+ Konu'nuzu**seçin. **+ Queue**

Dışa aktarma hedefi olarak Servis Veri Servisi'ni seçtiğinizde, kuyrukların ve konuların Oturumlar veya Yinelenen Algılama etkin leştirilmiş olmamalıdır. Bu seçeneklerden biri etkinse, bazı iletiler kuyruğa veya konunuza gelmez.

### <a name="create-storage-account"></a>Depolama hesabı oluştur

Dışa aktaracak varolan bir Azure depolama hesabınız yoksa aşağıdaki adımları izleyin:

1. Azure [portalında yeni](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)bir depolama hesabı oluşturun. Yeni [Azure Blob depolama hesapları](https://aka.ms/blobdocscreatestorageaccount) veya [Azure Veri Gölü Depolama v2 depolama hesapları](../../storage/blobs/data-lake-storage-quickstart-create-account.md)oluşturma hakkında daha fazla bilgi edinebilirsiniz. Veri dışa aktarma yalnızca blok blobs destekleyen depolama hesaplarına veri yazabilirsiniz. Aşağıdaki liste bilinen uyumlu depolama hesabı türlerini gösterir:

    |Performans Katmanı|Hesap Türü|
    |-|-|
    |Standart|Genel Amaçlı V2|
    |Standart|Genel Amaçlı V1|
    |Standart|Blob depolama|
    |Premium|Blob depolamayı engelle|

2. Depolama hesabınızda bir kapsayıcı oluşturun. Depolama hesabınıza gidin. **Blob Servisi**altında, **Gözat Blobs**seçin. Yeni bir kapsayıcı oluşturmak için üstkısmında **+ Kapsayıcı'yı** seçin.

## <a name="set-up-data-export"></a>Veri dışa aktarma ayarlama

Artık veri aktarmak için bir hedef var, veri dışa aktarmak için aşağıdaki adımları izleyin.

1. IoT Central uygulamanızda oturum açın.

2. Sol bölmede Veri **dışa aktarma'yı**seçin.

    > [!Tip]
    > **Veri dışa aktarımını** sol bölmede görmüyorsanız, uygulamanızda veri dışa aktarma yapılandırma izniniz yoktur. Veri dışa aktarma ayarlamak için bir yöneticiyle konuşun.

3. Sağ üstteki **+ Yeni** düğmesini seçin. Dışa aktarma noktanız olarak **Azure Etkinlik Hub'larından,** **Azure Hizmet Veri Tos'undan**veya **Azure Blob depolama sununuzdan** birini seçin. Uygulama başına en fazla ihracat sayısı beştir.

    ![Yeni veri dışa aktarma oluşturma](media/howto-export-data/new-export-definition.png)

4. Açılan liste kutusunda, **Olay Hub'larınızı,** Hizmet **Veri Tos ad alanınızı,** **Depolama Hesabı ad alanını**veya bağlantı **dizesini girin'** i seçin.

    - Yalnızca Depolama Hesapları, Olay Hub'ları ad alanları ve Hizmet Veri Merkezi ad alanlarını IoT Central uygulamanızla aynı abonelikte görürsünüz. Bu aboneliğin dışındaki bir hedefe dışa aktarmak istiyorsanız, **bağlantı dizesi girin** ve bir sonraki adımı görün.
    - Ücretsiz fiyatlandırma planı kullanılarak oluşturulan uygulamalar için, veri dışa aktarma yapılandırmanın tek yolu bir bağlantı dizesi geçer. Ücretsiz fiyatlandırma planındaki uygulamaların ilişkili bir Azure aboneliği yoktur.

    ![Yeni Etkinlik Hub'ı Oluşturma](media/howto-export-data/export-event-hub.png)

5. (İsteğe bağlı) **Bağlantı dizesi girin'i**seçtiyseniz, bağlantı dizenizi yapıştırmak için yeni bir kutu görüntülenir. Bağlantı dizesini almak için:
    - Etkinlik Hub'ları veya Hizmet Veri Servisi, Azure portalındaki ad alanına gidin:
        - **Ayarlar**altında **Paylaşılan Erişim İlkeleri'ni** seçin
        - Varsayılan **RootManageSharedAccessKey'i** seçin veya yeni bir tane oluşturun
        - Birincil veya ikincil bağlantı dizesini kopyalama
    - Depolama hesabı, Azure portalındaki depolama hesabına gidin:
        - **Ayarlar**altında **Access tuşlarını** seçin
        - Key1 bağlantı dizesini veya key2 bağlantı dizesini kopyalama

6. Açılan liste kutusundan bir olay hub'ı, sıra, konu veya kapsayıcı seçin.

7. **Dışa aktarmak için Veri**altında, Türü A.C.'ye ayarlayarak dışa aktarılabilmek için veri türlerini seçin. **On**

8. Veri dışa aktarmayı açmak **için, Etkin** geçişinin **Üzerinde**olduğundan emin olun. **Kaydet'i**seçin.

9. Birkaç dakika sonra verileriniz seçtiğiniz hedefte görünür.

## <a name="export-contents-and-format"></a>İçeriği ve biçimi dışa aktarma

Dışa aktarılan telemetri verileri, aygıtlarınızın Yalnızca telemetri değerlerinin değil, IoT Central'a gönderdiği iletinin tamamını içerir. Dışa aktarılan aygıtverileri tüm aygıtların özelliklerinde ve meta verilerinde değişiklikler içerir ve dışa aktarılan aygıt şablonları tüm aygıt şablonlarında değişiklikler içerir.

Olay Hub'ları ve Hizmet Veri Servisi için veriler neredeyse gerçek zamanlı olarak dışa aktarılır. Veriler `body` özellikte ve JSON biçimindedir. Örnekler için aşağıya bakın.

Blob depolama için, veriler dakikada bir dışa aktarılır ve her dosya son dışa aktarılan dosyadan bu yana değişiklik toplu işlerini içeren bir dosyayla birlikte dışa aktarılır. Dışa aktarılan veriler JSON biçiminde üç klasöre yerleştirilir. Depolama hesabınızdaki varsayılan yollar şunlardır:

- Telemetri: _{container}/{app-id}/telemetri/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_
- Cihazlar: _{container}/{app-id}/devices/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_
- Cihaz şablonları: _{container}/{app-id}/deviceTemplates/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_

Azure portalında dışa aktarılan dosyalara göz atmak için dosyaya gidin ve **Blob'u Edin** sekmesini seçin.

## <a name="telemetry"></a>Telemetri

Olay Hub'ları ve Servis Veri Meskeni için IoT Central, bir aygıttan iletiyi aldıktan hemen sonra yeni bir ileti dışa aktarmaz. Dışa aktarılan her ileti, aygıtın gövde mülkünde JSON biçiminde gönderdiği tam iletiyi içerir.

Blob depolama için iletiler toplu olarak paketlenir ve dakikada bir dışa aktarılır. Dışa aktarılan dosyalar, [IoT Hub ileti yönlendirmesi](../../iot-hub/tutorial-routing.md) tarafından blob depolamasına dışa aktarılan ileti dosyalarıyla aynı biçimi kullanır.

> [!NOTE]
> Blob depolama için, aygıtlarınızın `contentType: application/JSON` iletileri gönderdiğinden `contentEncoding:utf-8` emin `utf-16` `utf-32`olun ve (veya , ) Bir örnek için [IoT Hub belgelerine](../../iot-hub/iot-hub-devguide-routing-query-syntax.md#message-routing-query-based-on-message-body) bakın.

Telemetriyi gönderen aygıt aygıt kimliğiyle temsil edilir (aşağıdaki bölümlere bakın). Aygıtların adlarını almak için aygıt verilerini dışa aktarın ve aygıt iletisinin **aygıtKimliği** ile eşleşen **deviceId bağlantısını** kullanarak her iletiyi ilişkilendirin.

Aşağıdaki örnekte, bir olay hub'ından veya Hizmet Veri Gönderi kuyruğundan veya konusundan alınan bir ileti gösterilmektedir:

```json
{
  "temp":81.129693132351775,
  "humid":59.488071477541247,
  "EventProcessedUtcTime":"2020-04-07T09:41:15.2877981Z",
  "PartitionId":0,
  "EventEnqueuedUtcTime":"2020-04-07T09:38:32.7380000Z"
}
```

Bu ileti, gönderen aygıtın aygıt kimliğini içermez.

Bir Azure Akış Analizi sorgusundaki ileti verilerinden aygıt kimliğini almak için [GetMetadataPropertyValue](https://docs.microsoft.com/stream-analytics-query/getmetadatapropertyvalue) işlevini kullanın. Örneğin, [Akış Analizi, Azure İşlevleri ve SendGrid'i kullanarak özel kurallariçeren Azure IoT Merkezi'ni Genişlet'teki](./howto-create-custom-rules.md)sorguya bakın.

Aygıt kimliğini Azure Databricks veya Apache Spark çalışma alanında almak için [systemProperties'i](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/structured-streaming-eventhubs-integration.md)kullanın. Örneğin, [Azure Veri Tuğlaları'nı kullanarak özel analitiği olan Azure IoT Merkezi'ndeki](./howto-create-custom-analytics.md)Veri Tuğlaları çalışma alanına bakın.

Aşağıdaki örnek, blob depolamasına dışa aktarılan bir kaydı gösterir:

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

Anlık görüntüdeki her ileti veya kayıt, son dışa aktarılan iletiden bu yana aygıtta ve aygıtında ve bulut özelliklerinde bir veya daha fazla değişikliği temsil eder. İleti şunları içerir:

- `id`cihazın IoT Central'daki
- `displayName`cihazın
- Aygıt şablonu kimliği içinde`instanceOf`
- `simulated`bayrak, cihaz simüle edilmiş bir cihazsa doğru
- `provisioned`bayrak, cihaz sağlanmış sayılsa doğru
- `approved`bayrak, cihaz veri göndermek için onaylanmışsa doğru
- Özellik değerleri
- `properties`aygıt ve bulut özellikleri değerleri de dahil olmak üzere

Silinen aygıtlar dışa aktarılmaz. Şu anda, silinen aygıtlar için dışa aktarılan iletilerde gösterge yok.

Olay Hub'ları ve Servis Veri Servisi için IoT Central, olay merkezinize veya Servis Veri Servisi kuyruğunuza veya konuya neredeyse gerçek zamanlı olarak cihaz verileri içeren iletiler gönderir.

Blob depolama için, yazılan son dosyadan bu yana tüm değişiklikleri içeren yeni bir anlık görüntü dakikada bir dışa aktarılır.

Aşağıdaki örnek ileti, bir olay hub'ındaki veya Hizmet Veri İşlemi sırası veya konusundaki aygıtlar ve özellikler verileri hakkındaki bilgileri gösterir:

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

Bu anlık görüntü, Blob depolamasındaki aygıtları ve özellikleri gösteren bir örnek iletidir. Dışa aktarılan dosyalar kayıt başına tek bir satır içerir.

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

## <a name="device-templates"></a>Aygıt şablonları

Her ileti veya anlık görüntü kaydı, son dışa aktarılan iletiden bu yana yayımlanmış bir aygıt şablonunda bir veya daha fazla değişikliği temsil eder. Her iletide veya kayda gönderilen bilgiler şunları içerir:

- `id`yukarıdaki aygıt `instanceOf` akışıyla eşleşen aygıt şablonunun
- `displayName`aygıt şablonunun
- Onun `interfaces` `capabilityModel` ve telemetri, özellikleri ve komut tanımları da dahil olmak üzere cihaz
- `cloudProperties`Tanım
- Geçersiz kılar ve ilk değerler, satır satır`capabilityModel`

Silinen aygıt şablonları dışa aktarılmaz. Şu anda, silinen aygıt şablonları için dışa aktarılan iletilerde gösterge bulunmamaktadır.

Olay Hub'ları ve Servis Veri Servisi için IoT Central, olay merkezinize veya Hizmet Veri Servisi kuyruğunuza veya konuya neredeyse gerçek zamanlı olarak aygıt şablonu verilerini içeren iletiler gönderir.

Blob depolama için, yazılan son dosyadan bu yana tüm değişiklikleri içeren yeni bir anlık görüntü dakikada bir dışa aktarılır.

Bu örnekte, olay hub'ında veya Hizmet Veri İşleme sırası veya konusunda aygıt şablonları verileriyle ilgili bir ileti gösterilmektedir:

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

Bu örnek anlık görüntü, Blob depolama sında aygıt ve özellikleri veri içeren bir ileti gösterir. Dışa aktarılan dosyalar kayıt başına tek bir satır içerir.

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

## <a name="data-format-change-notice"></a>Veri biçimi değişikliği bildirimi

> [!Note]
> Telemetri akışı veri biçimi bu değişiklikten etkilenmez. Yalnızca aygıtlar ve aygıt şablonları veri akışları etkilenir.

*Önizleme* uygulamanızda Cihazlar ve *Aygıt şablonları* açık olan varolan bir veri dışa aktarımınız varsa, dışa aktarma nızı **30 Haziran 2020'ye**kadar güncelleyin. Bu gereksinim, Azure Blob depolama, Azure Etkinlik Hub'ları ve Azure Hizmet Veri Tos'una yapılan dışa aktarmalar için geçerlidir.

3 Şubat 2020'den itibaren, Aygıt lar ve Aygıt şablonları etkinleştirilmiş uygulamalardaki tüm yeni dışa aktarımlar yukarıda açıklanan veri biçimine sahip olacaktır. Bu tarihten önce oluşturulan tüm dışa aktarımlar 30 Haziran 2020 tarihine kadar eski veri biçiminde kalır ve bu süre içinde bu dışaaklar otomatik olarak yeni veri biçimine geçirilir. Yeni veri biçimi, IoT Merkezi genel API'deki [aygıt,](https://docs.microsoft.com/rest/api/iotcentral/devices/get) [aygıt özelliği,](https://docs.microsoft.com/rest/api/iotcentral/devices/getproperties) [aygıt bulut özelliği](https://docs.microsoft.com/rest/api/iotcentral/devices/getcloudproperties)ve aygıt [şablonu](https://docs.microsoft.com/rest/api/iotcentral/devicetemplates/get) nesneleriyle eşleşir.

**Cihazlar**için, eski veri biçimi ve yeni veri biçimi arasındaki önemli farklar şunlardır:
- `@id`cihaz için kaldırılır, `deviceId` yeniden adlandırılır`id` 
- `provisioned`aygıtın sağlama durumunu açıklamak için bayrak eklenir
- `approved`aygıtın onay durumunu açıklamak için bayrak eklenir
- `properties`aygıt ve bulut özellikleri de dahil olmak üzere, kamu API'sindeki varlıklarla eşleşir

**Aygıt şablonları**için, eski veri biçimi ile yeni veri biçimi arasındaki önemli farklar şunlardır:

- `@id`için aygıt şablonu yeniden adlandırıldı`id`
- `@type`aygıt şablonu için `types`yeniden adlandırılmış ve şimdi bir dizi

### <a name="devices-format-deprecated-as-of-3-february-2020"></a>Cihazlar (3 Şubat 2020 itibariyle amortismana hazır)

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

### <a name="device-templates-format-deprecated-as-of-3-february-2020"></a>Aygıt şablonları (3 Şubat 2020 itibariyle amortismana hazır biçimi)

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

Verilerinizi Azure Etkinlik Hub'larına, Azure Hizmet Veri Veri Tos'una ve Azure Blob depolama alanına nasıl aktaracağınız artık bir sonraki adıma devam edin:

> [!div class="nextstepaction"]
> [Webhooks nasıl oluşturulur](./howto-create-webhooks.md)
