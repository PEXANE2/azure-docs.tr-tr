---
title: IoT Hub’dan telemetri alma
titleSuffix: Azure Digital Twins
description: IoT Hub cihaz telemetri iletilerini alma bölümüne bakın.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 8/11/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 47e4bb291d031c41c89c88435a795004490e20a1
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88505334"
---
# <a name="ingest-iot-hub-telemetry-into-azure-digital-twins"></a>Azure dijital TWINS 'e alma IoT Hub telemetrisi

Azure dijital TWINS, IoT cihazlarından ve diğer kaynaklardaki verilerle birlikte çalıştırılır. Azure dijital TWINS 'te kullanılacak cihaz verileri için ortak bir kaynak [IoT Hub](../iot-hub/about-iot-hub.md).

Verileri Azure dijital TWINS 'e geri alma süreci, verileri alan ve özellikleri ayarlamak için [Digitaltwins API 'lerini](how-to-use-apis-sdks.md) kullanan bir [Azure işlevi](../azure-functions/functions-overview.md)gibi bir dış işlem kaynağı kurmak veya [dijital TWINS](concepts-twins-graph.md) 'de buna uygun olarak telemetri olayları tetiklemesi için kullanılır. 

Bu nasıl yapılır belgesi, IoT Hub telemetri alabilen bir Azure işlevi yazma işlemini adım adım göstermektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu örneğe devam etmeden önce aşağıdaki kaynakları önkoşul olarak ayarlamanız gerekir:
* **IoT Hub 'ı**. Yönergeler için, [bu IoT Hub hızlı başlangıç](../iot-hub/quickstart-send-telemetry-cli.md)konusunun *IoT Hub oluşturma* bölümüne bakın.
* Dijital ikizi örneğinizi çağırmak için doğru izinlere sahip **bir Azure işlevi** . Yönergeler için bkz. [*nasıl yapılır: verileri işlemek için bir Azure Işlevi ayarlama*](how-to-create-azure-function.md). 
* Cihaz telemetrinizi alacak **bir Azure dijital TWINS örneği** . Yönergeler için bkz. [*nasıl yapılır: Azure dijital TWINS örneği ve kimlik doğrulaması ayarlama*](./how-to-set-up-instance-portal.md).

### <a name="example-telemetry-scenario"></a>Örnek telemetri senaryosu

Bu nasıl yapılır-bir Azure işlevi kullanarak IoT Hub nasıl Azure dijital TWINS 'e ileti gönderileceğini özetler. Bunun için kullanabileceğiniz birçok olası yapılandırma ve eşleşen strateji vardır, ancak bu makaleye yönelik örnek aşağıdaki bölümleri içerir:
* Bilinen bir cihaz KIMLIĞIYLE IoT Hub bir termometre aygıtı.
* Eşleşen bir KIMLIK ile cihazı temsil eden dijital ikizi

> [!NOTE]
> Bu örnek, cihaz KIMLIĞI ile karşılık gelen dijital ikizi KIMLIĞI arasında basit bir KIMLIK eşleşmesi kullanır, ancak cihazdan ikizi 'e daha karmaşık eşlemeler sağlamak mümkündür (örneğin, bir eşleme tablosu ile).

Termometre cihazı tarafından bir sıcaklık telemetri olayı gönderildiğinde, dijital ikizi 'ın *sıcaklık* özelliği güncellenir. Bu senaryo aşağıdaki diyagramda özetlenmiştir:

:::image type="content" source="media/how-to-ingest-iot-hub-data/events.png" alt-text="Akış grafiğini gösteren diyagram. Grafik IoT Hub bir cihaz, Azure dijital TWINS 'teki bir ikizi üzerinde sıcaklık özelliği güncelleştiren bir Azure Işlevine IoT Hub aracılığıyla sıcaklık telemetri gönderir." border="false":::

## <a name="add-a-model-and-twin"></a>Model ve ikizi ekleme

IoT Hub bilgileriyle güncelleştirmek için bir ikizi gerekir.

Model şöyle görünür:
```JSON
{
  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",
  "@type": "Interface",
  "@context": "dtmi:dtdl:context;2",
  "contents": [
    {
      "@type": "Property",
      "name": "Temperature",
      "schema": "double"
    }
  ]
}
```

**Bu modeli TWINS örneğinizle karşıya yüklemek**IÇIN Azure CLI 'yı açın ve şu komutu çalıştırın:
```azurecli-interactive
az dt model create --models '{  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",  "@type": "Interface",  "@context": "dtmi:dtdl:context;2",  "contents": [    {      "@type": "Property",      "name": "Temperature",      "schema": "double"    }  ]}' -n {digital_twins_instance_name}
```

Daha sonra **Bu modeli kullanarak bir ikizi oluşturmanız**gerekecektir. Bir ikizi oluşturmak ve ilk sıcaklık değeri olarak 0,0 ayarlamak için aşağıdaki komutu kullanın.
```azurecli-interactive
az dt twin create --dtmi "dtmi:contosocom:DigitalTwins:Thermostat;1" --twin-id thermostat67 --properties '{"Temperature": 0.0,}' --dt-name {digital_twins_instance_name}
```

Başarılı bir ikizi Create komutunun çıkışı şöyle görünmelidir:
```json
{
  "$dtId": "thermostat67",
  "$etag": "W/\"0000000-9735-4f41-98d5-90d68e673e15\"",
  "$metadata": {
    "$model": "dtmi:contosocom:DigitalTwins:Thermostat;1",
    "Temperature": {
      "ackCode": 200,
      "ackDescription": "Auto-Sync",
      "ackVersion": 1,
      "desiredValue": 0.0,
      "desiredVersion": 1
    }
  },
  "Temperature": 0.0
}
```

## <a name="create-an-azure-function"></a>Azure işlevi oluşturma

Bu bölüm, [*nasıl yapılır: verileri işlemek için bir Azure Işlevi ayarlama*](how-to-create-azure-function.md)Ile aynı Visual Studio başlangıç adımlarını ve Azure işlevi iskelet 'i kullanır. Çatı, kimlik doğrulamasını işler ve Azure dijital TWINS API 'Leri yanıt olarak çağırabilmeniz için bir hizmet istemcisi oluşturur. 

İzleyen adımlarda, IoT Hub IoT telemetri olaylarını işlemek için buna özel kod ekleyeceksiniz.  

### <a name="add-telemetry-processing"></a>Telemetri işleme ekleme
    
Telemetri olayları cihazdan iletiler biçiminde gelir. Telemetri işleme kodu eklemenin ilk adımı, bu cihaz iletisinin ilgili bölümünün Event Grid olayından ayıklanarak oluşur. 

Farklı cihazlar, iletilerini farklı şekilde yapılandırabileceği için, **Bu adımın kodu bağlı cihaza bağlıdır.** 

Aşağıdaki kod, telemetri olarak JSON gönderen basit bir cihaz için bir örnek gösterir. Bu örnek, [*öğretici: uçtan uca çözümü bağlama*](./tutorial-end-to-end.md)konusunda tam olarak araştırılmış. Aşağıdaki kod, iletiyi gönderen cihazın ve sıcaklık değerinin cihaz KIMLIĞINI bulur.

```csharp
JObject deviceMessage = (JObject)JsonConvert.DeserializeObject(eventGridEvent.Data.ToString());
string deviceId = (string)deviceMessage["systemProperties"]["iothub-connection-device-id"];
var temperature = deviceMessage["body"]["Temperature"];
```

Sonraki kod örneği, KIMLIĞI ve sıcaklık değerini alır ve bu ikizi "Patch" (güncelleştirme yap) için kullanır.

```csharp
//Update twin using device temperature
var uou = new UpdateOperationsUtility();
uou.AppendReplaceOp("/Temperature", temperature.Value<double>());
await client.UpdateDigitalTwinAsync(deviceId, uou.Serialize());
...
```

### <a name="update-your-azure-function-code"></a>Azure işlev kodunuzu güncelleştirme

Önceki örneklerden kodu anladığınıza göre, Visual Studio 'Yu açın ve Azure işlevinizin kodunu bu örnek kodla değiştirin.

```csharp
using System;
using System.Net.Http;
using Azure.Core.Pipeline;
using Azure.DigitalTwins.Core;
using Azure.DigitalTwins.Core.Serialization;
using Azure.Identity;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace IotHubtoTwins
{
    public class IoTHubtoTwins
    {
        private static readonly string adtInstanceUrl = Environment.GetEnvironmentVariable("ADT_SERVICE_URL");
        private static readonly HttpClient httpClient = new HttpClient();

        [FunctionName("IoTHubtoTwins")]
        public async void Run([EventGridTrigger] EventGridEvent eventGridEvent, ILogger log)
        {
            if (adtInstanceUrl == null) log.LogError("Application setting \"ADT_SERVICE_URL\" not set");

            try
            {
                //Authenticate with Digital Twins
                ManagedIdentityCredential cred = new ManagedIdentityCredential("https://digitaltwins.azure.net");
                DigitalTwinsClient client = new DigitalTwinsClient(
                    new Uri(adtInstanceUrl), cred, new DigitalTwinsClientOptions 
                    { Transport = new HttpClientTransport(httpClient) });
                log.LogInformation($"ADT service client connection created.");
            
                if (eventGridEvent != null && eventGridEvent.Data != null)
                {
                    log.LogInformation(eventGridEvent.Data.ToString());

                    // Reading deviceId and temperature for IoT Hub JSON
                    JObject deviceMessage = (JObject)JsonConvert.DeserializeObject(eventGridEvent.Data.ToString());
                    string deviceId = (string)deviceMessage["systemProperties"]["iothub-connection-device-id"];
                    var temperature = deviceMessage["body"]["Temperature"];
                    
                    log.LogInformation($"Device:{deviceId} Temperature is:{temperature}");

                    //Update twin using device temperature
                    var uou = new UpdateOperationsUtility();
                    uou.AppendReplaceOp("/Temperature", temperature.Value<double>());
                    await client.UpdateDigitalTwinAsync(deviceId, uou.Serialize());
                }
            }
            catch (Exception e)
            {
                log.LogError($"Error in ingest function: {e.Message}");
            }
        }
    }
}
```

## <a name="connect-your-function-to-iot-hub"></a>İşlevinizi IoT Hub bağlama

1. Hub verileri için bir olay hedefi ayarlayın. [Azure portal](https://portal.azure.com/)IoT Hub örneğinize gidin. **Olaylar**' ın altında, Azure işleviniz için bir abonelik oluşturun. 

    :::image type="content" source="media/how-to-ingest-iot-hub-data/add-event-subscription.png" alt-text="Olay aboneliği eklemeyi gösteren Azure portal ekran görüntüsü.":::

2. **Olay aboneliği oluştur** sayfasında, alanları aşağıdaki gibi girin:
    1. **Ad**' ın altında, aboneliği istediğiniz şekilde adlandırın.
    2. **Olay şeması**altında **Event Grid şeması**' nı seçin.
    3. **Sistem konu adı**altında benzersiz bir ad seçin.
    4. **Olay türleri**altında, Filtrelenecek olay türü olarak **cihaz telemetrisi** ' ni seçin.
    5. **Uç nokta ayrıntıları**' nın altında Azure işlevinizi bir uç nokta olarak seçin.

    :::image type="content" source="media/how-to-ingest-iot-hub-data/event-subscription-2.png" alt-text="Olay aboneliği ayrıntılarını gösteren Azure portal ekran görüntüsü":::

## <a name="send-simulated-iot-data"></a>Sanal IoT verisi gönder

Yeni giriş işlevinizi test etmek için öğreticideki cihaz simülatörünü kullanın [*: uçtan uca bir çözümü bağlama*](./tutorial-end-to-end.md). Bu öğretici, C# dilinde yazılmış örnek bir proje tarafından çalıştırılır. Örnek kod şurada bulunur: [Azure dijital TWINS örnekleri](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples). Bu depoda **Devicesimülatör** projesini kullanıyorsunuz.

Uçtan uca öğreticide, aşağıdaki adımları izleyin:
1. [*Sanal cihazı IoT Hub Kaydet*](./tutorial-end-to-end.md#register-the-simulated-device-with-iot-hub)
2. [*Simülasyonu yapılandırma ve çalıştırma*](./tutorial-end-to-end.md#configure-and-run-the-simulation)

## <a name="validate-your-results"></a>Sonuçlarınızı doğrulama

Yukarıdaki cihaz simülatörünü çalıştırırken, dijital ikizi 'ın sıcaklık değeri değişecek. Azure CLı 'de, sıcaklık değerini görmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az dt twin query -q "select * from digitaltwins" -n {digital_twins_instance_name}
```

Çıktılarınız şöyle bir sıcaklık değeri içermelidir:

```json
{
  "result": [
    {
      "$dtId": "thermostat67",
      "$etag": "W/\"0000000-1e83-4f7f-b448-524371f64691\"",
      "$metadata": {
        "$model": "dtmi:contosocom:DigitalTwins:Thermostat;1",
        "Temperature": {
          "ackCode": 200,
          "ackDescription": "Auto-Sync",
          "ackVersion": 1,
          "desiredValue": 69.75806974934324,
          "desiredVersion": 1
        }
      },
      "Temperature": 69.75806974934324
    }
  ]
}
```

Değer değişikliğini görmek için yukarıdaki sorgu komutunu tekrar tekrar çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS ile veri giriş ve çıkış hakkında bilgi edinin:
* [*Kavramlar: diğer hizmetlerle tümleştirme*](concepts-integration.md)
