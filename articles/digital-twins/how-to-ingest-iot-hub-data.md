---
title: IoT Hub’dan telemetri alma
titleSuffix: Azure Digital Twins
description: IoT Hub cihaz telemetri iletilerini alma bölümüne bakın.
author: cschormann
ms.author: cschorm
ms.date: 3/17/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 351f7ed131d545d2aa83df753cac3f26e76e4ccb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84725860"
---
# <a name="ingest-iot-hub-telemetry-into-azure-digital-twins"></a>Azure dijital TWINS 'e alma IoT Hub telemetrisi

Azure dijital TWINS, IoT cihazlarından ve diğer kaynaklardaki verilerle birlikte çalıştırılır. Azure dijital TWINS 'te kullanılacak cihaz verileri için ortak bir kaynak [IoT Hub](../iot-hub/about-iot-hub.md).

Önizleme süresince, verileri Azure dijital TWINS 'e geri alma işlemi, verileri alan ve [Azure işlevi](../azure-functions/functions-overview.md)gibi bir dış işlem kaynağı kurmak ve bu verileri, özellikleri ayarlamak veya [dijital TWINS](concepts-twins-graph.md) 'de telemetri olaylarını Işlemek Için [digitaltwins API 'lerini](how-to-use-apis-sdks.md) kullanır. 

Bu nasıl yapılır belgesi, IoT Hub telemetri alabilen bir Azure işlevi yazma işlemini adım adım göstermektedir.

## <a name="example-telemetry-scenario"></a>Örnek telemetri senaryosu

Bu nasıl yapılır-bir Azure işlevi kullanarak IoT Hub nasıl Azure dijital TWINS 'e ileti gönderileceğini özetler. Bunun için kullanabileceğiniz birçok olası yapılandırma ve eşleşen strateji vardır, ancak bu makaleye yönelik örnek aşağıdaki bölümleri içerir:
* Bilinen bir cihaz KIMLIĞIYLE IoT Hub bir termometre aygıtı.
* Eşleşen bir KIMLIK ile cihazı temsil eden dijital ikizi
* Bir odayı temsil eden dijital ikizi

> [!NOTE]
> Bu örnek, cihaz KIMLIĞI ile karşılık gelen dijital ikizi KIMLIĞI arasında basit bir KIMLIK eşleşmesi kullanır, ancak cihazdan ikizi 'e daha karmaşık eşlemeler sağlamak mümkündür (örneğin, bir eşleme tablosu ile).

Termometre cihazı tarafından bir sıcaklık telemetri olayı gönderildiğinde, *odikizi* 'ın *sıcaklık* özelliği güncellenir. Bunun gerçekleşmesini sağlamak için, bir cihazdaki telemetri olayından dijital ikizi Özellik ayarlayıcısından eşleme yaparsınız. [İkizi grafından](concepts-twins-graph.md) topoloji bilgilerini kullanarak *Oda* ikizi ' ı bulabilir ve ardından ikizi özelliğini ayarlayabilirsiniz. Diğer senaryolarda, kullanıcılar eşleşen ikizi bir özellik ayarlamak isteyebilir (Bu örnekte, *123*kimliğine sahip ikizi). Azure dijital TWINS, Telemetri verilerinin TWINS 'e nasıl eşlendiğini belirlemek için size çok esneklik sunar. 

Bu senaryo aşağıdaki diyagramda özetlenmiştir:

:::image type="content" source="media/how-to-ingest-iot-hub-data/events.png" alt-text="IoT Hub cihaz, Azure Digital TWINS 'te ikizlerini üzerinde bir sıcaklık özelliğini güncelleştiren bir Azure işlevine IoT Hub, Event Grid veya sistem konuları aracılığıyla sıcaklık telemetri gönderir." border="false":::

## <a name="prerequisites"></a>Ön koşullar

Bu örneğe devam etmeden önce, aşağıdaki önkoşulları gerçekleştirmeniz gerekir.
1. IoT Hub 'ı oluşturun. Yönergeler için [bu IoT Hub hızlı başlangıç](../iot-hub/quickstart-send-telemetry-cli.md) konusunun *IoT Hub oluşturma* bölümüne bakın.
2. IoT Hub olayları işlemek için en az bir Azure işlevi oluşturun. Bkz. nasıl yapılır: Azure dijital TWINS 'e bağlanabilecek ve Azure dijital TWINS API işlevlerini çağırasağlayan temel bir Azure işlevi oluşturmak üzere [verileri işlemek için bir Azure Işlevi ayarlama](how-to-create-azure-function.md) . Bu işlevin geri kalanı bu işlevi oluşturur.
3. Hub verileri için bir olay hedefi ayarlayın. [Azure portal](https://portal.azure.com/)IoT Hub örneğinize gidin. *Olaylar*' ın altında, Azure işleviniz için bir abonelik oluşturun. 

    :::image type="content" source="media/how-to-ingest-iot-hub-data/add-event-subscription.png" alt-text="Azure portal: olay aboneliği ekleme":::

4. *Olay aboneliği oluştur* sayfasında, alanları aşağıdaki gibi girin:
   * *Olay ABONELIĞI ayrıntıları*' nın altında, aboneliği istediğiniz şekilde adlandırın
   * *Olay türleri*altında, filtre uygulanacak olay türü olarak *cihaz telemetrisi* ' ni seçin.
      - İsterseniz diğer olay türlerine filtre ekleyin.
   * *Uç nokta ayrıntıları*altında Azure işlevinizi bir uç nokta olarak seçin

## <a name="create-an-azure-function-in-visual-studio"></a>Visual Studio 'da bir Azure işlevi oluşturma

Bu bölüm, [nasıl yapılır: verileri işlemek için bir Azure Işlevi ayarlama](how-to-create-azure-function.md)Ile aynı Visual Studio başlangıç adımlarını ve Azure işlevi iskelet 'i kullanır. Çatı, kimlik doğrulamasını işler ve Azure dijital TWINS API 'Leri yanıt olarak çağırabilmeniz için bir hizmet istemcisi oluşturur. 

İskelet işlevinin kalp olması şu şekilde olur:

```csharp
namespace FunctionSample
{
    public static class FooFunction
    {
        const string adtAppId = "https://digitaltwins.azure.net";
        private static string adtInstanceUrl = Environment.GetEnvironmentVariable("ADT_SERVICE_URL");
        private static HttpClient httpClient = new HttpClient();

        [FunctionName("Foo")]
        public static async Task Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            DigitalTwinsClient client = null;
            try
            {
                ManagedIdentityCredential cred = new ManagedIdentityCredential(adtAppId);
                DigitalTwinsClientOptions opts = 
                    new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
                client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, opts);
                                                
                log.LogInformation($"ADT service client connection created.");
            }
            catch (Exception e)
            {
                log.LogError($"ADT service client connection failed. " + e.ToString());
                return;
            }
            log.LogInformation(eventGridEvent.Data.ToString());
        }
    }
}
```

İzleyen adımlarda, IoT Hub IoT telemetri olaylarını işlemek için buna özel kod ekleyeceksiniz.  

## <a name="add-telemetry-processing"></a>Telemetri işleme ekleme

Telemetri olayları cihazdan iletiler biçiminde gelir. Telemetri işleme kodu eklemenin ilk adımı, bu cihaz iletisinin ilgili bölümünün Event Grid olayından ayıklanarak oluşur. 

Farklı cihazlar, iletilerini farklı şekilde yapılandırabileceği için, bu adımın kodu bağlı cihaza bağlıdır. 

Aşağıdaki kod, telemetri olarak JSON gönderen basit bir cihaz için bir örnek gösterir. Örnek, iletiyi gönderen cihazın cihaz KIMLIĞINI ve sıcaklık değerini ayıklar.

```csharp
JObject job = eventGridEvent.Data as JObject;
string devid = (string)job["systemProperties"].ToObject<JObject>().Property("IoT-hub-connection-device-ID").Value;
double temp = (double)job["body"].ToObject<JObject>().Property("temperature").Value;
```

Bu alıştırmanın amacını, ikizi Graf içindeki bir *odanın* sıcaklığını güncelleştirmeniz gerektiğini unutmayın. Bu, ileti hedefimizin bu cihazla ilişkili dijital ikizi olmadığı, ancak üst öğesi olan *Oda* ikizi olmadığı anlamına gelir. Üst ikizi, yukarıdaki kodu kullanarak telemetri iletisinden ayıkladığınız cihaz KIMLIĞI değerini kullanarak bulabilirsiniz.

Bunu yapmak için Azure dijital TWINS API 'Lerini kullanarak cihaz-temsil eden ikizi (Bu durumda cihazla aynı KIMLIĞE sahip olan) gelen ilişkilere erişin. Gelen ilişkide, üst öğenin KIMLIĞINI aşağıdaki kod parçacığı ile bulabilirsiniz.

Aşağıdaki kod parçacığı, bir ikizi gelen ilişkilerinin nasıl alınacağını gösterir:

```csharp
AsyncPageable<IncomingRelationship> res = client.GetIncomingRelationshipsAsync(twin_id);
await foreach (IncomingRelationship irel in res)
{
    Log.Ok($"Relationship: {irel.RelationshipName} from {irel.SourceId} | {irel.RelationshipId}");
}
```

İkizi 'in üst öğesi ilişkinin *SourceId* özelliğinde bulunur.

Bir cihazı yalnızca tek bir gelen ilişkiye sahip olacak şekilde temsil eden bir ikizi modeli için oldukça yaygındır. Bu durumda, döndürülen ilk (ve yalnızca) ilişkiyi seçebilirsiniz. Modelleriniz bu ikizi birden çok ilişki türüne izin veriyor ise, birden çok gelen ilişkilerden seçim yapmak için daha fazla belirtmeniz gerekebilir. Bunu yapmanın yaygın bir yolu, ilişkinin tarafından çekilmesi `RelationshipName` . 

*Odayı*temsil eden üst ikizi kimliğine sahip olduktan sonra, ikizi tarafından "Patch" (güncelleştirme yapabilirsiniz) yapabilirsiniz. Bunu yapmak için aşağıdaki kodu kullanın:

```csharp
UpdateOperationsUtility uou = new UpdateOperationsUtility();
uou.AppendAddOp("/Temperature", temp);
try
{
    await client.UpdateDigitalTwinAsync(twin_id, uou.Serialize());
    Log.Ok($"Twin '{twin_id}' updated successfully!");
}
...
```

### <a name="full-azure-function-code"></a>Tam Azure işlev kodu

Önceki örneklerden kodu kullanarak, bağlam içindeki tüm Azure işlevleri aşağıda verilmiştir:

```csharp
[FunctionName("ProcessHubToDTEvents")]
public async void Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
{
    // After this is deployed, in order for this function to be authorized on Azure Digital Twins APIs,
    // you'll need to turn the Managed Identity Status to "On", 
    // grab the Object ID of the function, and assign the "Azure Digital Twins Owner (Preview)" role to this function identity.

    DigitalTwinsClient client = null;
    //log.LogInformation(eventGridEvent.Data.ToString());
    // Authenticate on Azure Digital Twins APIs
    try
    {
        
        ManagedIdentityCredential cred = new ManagedIdentityCredential(adtAppId);
        client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
        log.LogInformation($"ADT service client connection created.");
    }
    catch (Exception e)
    {
        log.LogError($"ADT service client connection failed. " + e.ToString());
        return;
    }

    if (client != null)
    {
        try
        {
            if (eventGridEvent != null && eventGridEvent.Data != null)
            {
                #region Open this region for message format information
                // Telemetry message format
                //{
                //  "properties": { },
                //  "systemProperties": 
                // {
                //    "iothub-connection-device-id": "thermostat1",
                //    "iothub-connection-auth-method": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
                //    "iothub-connection-auth-generation-id": "637199981642612179",
                //    "iothub-enqueuedtime": "2020-03-18T18:35:08.269Z",
                //    "iothub-message-source": "Telemetry"
                //  },
                //  "body": "eyJUZW1wZXJhdHVyZSI6NzAuOTI3MjM0MDg3MTA1NDg5fQ=="
                //}
                #endregion

                // Reading deviceId from message headers
                log.LogInformation(eventGridEvent.Data.ToString());
                JObject job = (JObject)JsonConvert.DeserializeObject(eventGridEvent.Data.ToString());
                string deviceId = (string)job["systemProperties"]["iothub-connection-device-id"];
                log.LogInformation($"Found device: {deviceId}");

                // Extracting temperature from device telemetry
                byte[] body = System.Convert.FromBase64String(job["body"].ToString());
                var value = System.Text.ASCIIEncoding.ASCII.GetString(body);
                var bodyProperty = (JObject)JsonConvert.DeserializeObject(value);
                var temperature = bodyProperty["Temperature"];
                log.LogInformation($"Device Temperature is:{temperature}");

                // Update device Temperature property
                await AdtUtilities.UpdateTwinProperty(client, deviceId, "/Temperature", temperature, log);

                // Find parent using incoming relationships
                string parentId = await AdtUtilities.FindParent(client, deviceId, "contains", log);
                if (parentId != null)
                {
                    await AdtUtilities.UpdateTwinProperty(client, parentId, "/Temperature", temperature, log);
                }

            }
        }
        catch (Exception e)
        {
            log.LogError($"Error in ingest function: {e.Message}");
        }
    }
}
```

Gelen ilişkileri bulmak için yardımcı program işlevi:
```csharp
public static async Task<string> FindParent(DigitalTwinsClient client, string child, string relname, ILogger log)
{
    // Find parent using incoming relationships
    try
    {
        AsyncPageable<IncomingRelationship> rels = client.GetIncomingRelationshipsAsync(child);

        await foreach (IncomingRelationship ie in rels)
        {
            if (ie.RelationshipName == relname)
                return (ie.SourceId);
        }
    }
    catch (RequestFailedException exc)
    {
        log.LogInformation($"*** Error in retrieving parent:{exc.Status}:{exc.Message}");
    }
    return null;
}
```

İkizi düzeltme eki uygulamak için yardımcı program işlevi:
```csharp
public static async Task UpdateTwinProperty(DigitalTwinsClient client, string twinId, string propertyPath, object value, ILogger log)
{
    // If the twin does not exist, this will log an error
    try
    {
        // Update twin property
        UpdateOperationsUtility uou = new UpdateOperationsUtility();
        uou.AppendAddOp(propertyPath, value);
        await client.UpdateDigitalTwinAsync(twinId, uou.Serialize());
    }
    catch (RequestFailedException exc)
    {
        log.LogInformation($"*** Error:{exc.Status}/{exc.Message}");
    }
}
```

Artık IoT Hub gelen senaryo verilerini okumak ve yorumlamak için donatılmış bir Azure işleviniz var.

## <a name="debug-azure-function-apps-locally"></a>Azure Function uygulamalarında yerel olarak hata ayıklama

Azure işlevleri Event Grid tetikleyicisiyle yerel olarak hata ayıklaması mümkündür. Bunun hakkında daha fazla bilgi için bkz. [Event Grid tetikleyiciden yerel olarak hata ayıklama](../azure-functions/functions-debug-event-grid-trigger-local.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS ile veri giriş ve çıkış hakkında bilgi edinin:
* [Kavramlar: diğer hizmetlerle tümleştirme](concepts-integration.md)
