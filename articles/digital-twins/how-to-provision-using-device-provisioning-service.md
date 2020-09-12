---
title: DPS kullanarak cihazları otomatik olarak yönetme
titleSuffix: Azure Digital Twins
description: Cihaz sağlama hizmeti 'ni (DPS) kullanarak Azure dijital TWINS 'te IoT cihazlarını sağlamak ve devre dışı bırakmak için otomatik bir işlem ayarlama bölümüne bakın.
author: baanders
ms.author: baanders
ms.date: 9/1/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 1a7ab90cccd78c3b005487938432a0f955d50738
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89381108"
---
# <a name="auto-manage-devices-in-azure-digital-twins-using-device-provisioning-service-dps"></a>Cihaz sağlama hizmeti 'ni (DPS) kullanarak Azure dijital TWINS 'de cihazları otomatik olarak yönetme

Bu makalede, Azure Digital TWINS 'i [cihaz sağlama hizmeti (DPS)](../iot-dps/about-iot-dps.md)ile tümleştirmeyi öğreneceksiniz.

Bu makalede açıklanan çözüm, cihaz sağlama hizmeti 'ni kullanarak Azure dijital TWINS 'de IoT Hub cihazları **_sağlama_** ve **_devre dışı bırakma_** sürecini otomatikleştirmenize imkan tanır. 

_Sağlama_ ve _devre dışı bırakma_ aşamaları hakkında daha fazla bilgi Için ve tüm kurumsal IoT projelerinde ortak olan genel cihaz yönetim aşamaları kümesini daha iyi anlamak için, IoT Hub cihaz yönetimi belgelerinin [ *cihaz yaşam döngüsü* bölümüne](../iot-hub/iot-hub-device-management-overview.md#device-lifecycle) bakın.

## <a name="prerequisites"></a>Ön koşullar

Sağlamayı ayarlamadan önce, modeller ve TWINS içeren bir **Azure dijital TWINS örneğine** sahip olmanız gerekir. Bu örnek, verileri temel alarak dijital ikizi bilgilerini güncelleştirme özelliği ile de ayarlanmalıdır. 

Bu ayarı zaten yoksa, Azure dijital TWINS [*öğreticisini izleyerek oluşturabilirsiniz: uçtan uca çözümü bağlama*](tutorial-end-to-end.md). Öğretici, modellerle bir Azure dijital TWINS örneği, bağlantılı bir Azure [IoT Hub](../iot-hub/about-iot-hub.md)ve veri akışını yaymaya yönelik çeşitli [Azure işlevleri](../azure-functions/functions-overview.md) ayarlama konusunda size kılavuzluk eder.

Örneğinizi ayarlarken, bu makalenin ilerleyen kısımlarında aşağıdaki değerlere sahip olmanız gerekir. Bu değerleri yeniden toplamanız gerekiyorsa, yönergeler için aşağıdaki bağlantıları kullanın.
* Azure Digital TWINS örnek **_ana bilgisayar adı_** ([portalda bul](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values))
* Azure Event Hubs bağlantı dizesi **_bağlantı dizesi_** ([portalda bul](../event-hubs/event-hubs-get-connection-string.md#get-connection-string-from-the-portal))

Bu örnek ayrıca cihaz sağlama hizmetini kullanarak sağlamayı içeren bir **cihaz simülatörü** kullanır. Cihaz simülatörü şurada bulunur: [Azure dijital TWINS ve IoT Hub tümleştirme örneği](https://docs.microsoft.com/samples/azure-samples/digital-twins-iothub-integration/adt-iothub-provision-sample/). Örnek bağlantısına gidip başlık altındaki *posta indir* düğmesini seçerek makinenizde örnek projeyi alın. İndirilen klasörü sıkıştırmayı açın.

Cihaz simülatörü **Node.js**, sürüm 10.0. x veya üzerini temel alır. [*Geliştirme ortamınızı hazırlama*](https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md) Bu öğretici Için Node.js Windows veya Linux 'ta nasıl yükleneceğini açıklar.

## <a name="solution-architecture"></a>Çözüm mimarisi

Aşağıdaki görüntüde, cihaz sağlama hizmeti ile Azure dijital TWINS kullanılarak bu çözümün mimarisi gösterilmektedir. Hem cihaz sağlama hem de devre dışı bırakma akışını gösterir.

:::image type="content" source="media/how-to-provision-using-dps/flows.png" alt-text="Bir cihazın ve çeşitli Azure hizmetlerinin bir uçtan uca senaryosunda bir görünümü. Veri akışı, bir termostat cihaz ve DPS arasında geri ve ileri akar. Veriler aynı zamanda DPS 'den IoT Hub 'ye ve Azure dijital TWINS 'e ' ayırma ' etiketli bir Azure işlevi aracılığıyla akar. El ile ' cihaz silme ' eyleminden alınan veriler, IoT Hub > Event Hubs Azure Işlevleri > Azure dijital TWINS > aracılığıyla akar.":::

Bu makale iki bölüme ayrılmıştır:
* [*Cihaz sağlama hizmeti 'ni kullanarak cihazı otomatik sağlama*](#auto-provision-device-using-device-provisioning-service)
* [*IoT Hub yaşam döngüsü olaylarını kullanarak cihazı otomatik olarak devre dışı bırakma*](#auto-retire-device-using-iot-hub-lifecycle-events)

Mimarideki her adımın daha derin açıklamaları için, makalenin ilerleyen bölümlerinde tek tek bölümlerine bakın.

## <a name="auto-provision-device-using-device-provisioning-service"></a>Cihaz sağlama hizmeti 'ni kullanarak cihazı otomatik sağlama

Bu bölümde, cihazları aşağıdaki yoldan otomatik sağlamak için Azure dijital TWINS 'e cihaz sağlama hizmeti iliştirirsiniz. Bu, [daha önce](#solution-architecture)gösterilen tam mimarinin bir alıntısıdır.

:::image type="content" source="media/how-to-provision-using-dps/provision.png" alt-text="Flow sağlama--bir çözüm mimarisi diyagramı akışının, sayı etiketleme bölümlerinin bir alıntısıdır. Veriler, bir termostat cihazı ve DPS (cihaz > DPS için 1 ve DPS > cihaz için 5) arasında ileri ve geri akar. Veriler aynı zamanda DPS 'den IoT Hub (4) ve Azure Digital TWINS 'e (3) ' ayırma ' (2) etiketli bir Azure işlevi aracılığıyla akar.":::

İşlem akışının açıklaması aşağıda verilmiştir:
1. Cihaz, kimliğini kanıtlamak için bilgi tanımlamayı sağlayan DPS uç noktası ile iletişim kurar.
2. DPS kayıt listesine karşı kayıt KIMLIĞI ve anahtarı doğrulayarak cihaz kimliğini doğrular ve ayırmayı yapmak için bir [Azure işlevi](../azure-functions/functions-overview.md) çağırır.
3. Azure işlevi, cihaz için Azure dijital TWINS 'te yeni bir [ikizi](concepts-twins-graph.md) oluşturur.
4. DPS, cihazı bir IoT Hub 'ına kaydeder ve cihazın istenen ikizi durumunu doldurur.
5. IoT Hub 'ı cihaza cihaz KIMLIĞI bilgilerini ve IoT Hub bağlantı bilgilerini döndürür. Cihaz artık IoT Hub 'ına bağlanabilir.

Aşağıdaki bölümler, bu otomatik sağlama cihaz akışını ayarlama adımlarında size yol gösterir.

### <a name="create-a-device-provisioning-service"></a>Cihaz sağlama hizmeti oluşturma

Cihaz sağlama hizmeti kullanılarak yeni bir cihaz sağlandığında, bu cihaz için yeni bir ikizi Azure dijital TWINS 'te oluşturulabilir.

IoT cihazları sağlamak için kullanılacak bir cihaz sağlama hizmeti örneği oluşturun. Aşağıdaki Azure CLı yönergelerini kullanabilir veya Azure portal kullanabilirsiniz: [*hızlı başlangıç: IoT Hub cihaz sağlama hizmeti 'ni Azure Portal ayarlama*](../iot-dps/quick-setup-auto-provision.md).

Aşağıdaki Azure CLı komutu bir cihaz sağlama hizmeti oluşturacaktır. Bir ad, kaynak grubu ve bölge belirtmeniz gerekecektir. Bu komut, [makinenizde yüklü](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)Azure CLI 'niz varsa [Cloud Shell](https://shell.azure.com)veya yerel olarak çalıştırılabilir.

```azurecli-interactive
az iot dps create --name <Device Provisioning Service name> --resource-group <resource group name> --location <region; for example, eastus>
```

### <a name="create-an-azure-function"></a>Azure işlevi oluşturma

Ardından, bir işlev uygulaması içinde HTTP isteği ile tetiklenen bir işlev oluşturacaksınız. Uçtan uca öğreticide oluşturulan işlev uygulamasını kullanabilirsiniz ([*öğretici: uçtan uca bir çözümü bağlama*](tutorial-end-to-end.md)) veya kendi kendinize.

Bu işlev, cihaz sağlama hizmeti tarafından, yeni bir cihaz sağlayan [özel bir ayırma ilkesinde](../iot-dps/how-to-use-custom-allocation-policies.md) kullanılacaktır. Azure işlevleri ile HTTP istekleri kullanma hakkında daha fazla bilgi için bkz. Azure [*Için Azure http istek tetikleyicisi işlevleri*](../azure-functions/functions-bindings-http-webhook-trigger.md).

İşlev uygulaması projenizin içinde yeni bir işlev ekleyin. Ayrıca, projeye yeni bir NuGet paketi ekleyin: `Microsoft.Azure.Devices.Provisioning.Service` .

Yeni oluşturulan işlev kodu dosyasında aşağıdaki kodu yapıştırın.

```C#
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Microsoft.Azure.Devices.Shared;
using Microsoft.Azure.Devices.Provisioning.Service;
using System.Net.Http;
using Azure.Identity;
using Azure.DigitalTwins.Core;
using Azure.Core.Pipeline;
using Azure;
using System.Collections.Generic;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace Samples.AdtIothub
{
    public static class DpsAdtAllocationFunc
    {
        const string adtAppId = "https://digitaltwins.azure.net";
        private static string adtInstanceUrl = Environment.GetEnvironmentVariable("ADT_SERVICE_URL");
        private static readonly HttpClient httpClient = new HttpClient();

        [FunctionName("DpsAdtAllocationFunc")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, ILogger log)
        {
            // Get request body
            string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
            log.LogDebug($"Request.Body: {requestBody}");
            dynamic data = JsonConvert.DeserializeObject(requestBody);

            // Get registration ID of the device
            string regId = data?.deviceRuntimeContext?.registrationId;

            bool fail = false;
            string message = "Uncaught error";
            ResponseObj obj = new ResponseObj();

            // Must have unique registration ID on DPS request 
            if (regId == null)
            {
                message = "Registration ID not provided for the device.";
                log.LogInformation("Registration ID: NULL");
                fail = true;
            }
            else
            {
                string[] hubs = data?.linkedHubs.ToObject<string[]>();

                // Must have hubs selected on the enrollment
                if (hubs == null)
                {
                    message = "No hub group defined for the enrollment.";
                    log.LogInformation("linkedHubs: NULL");
                    fail = true;
                }
                else
                {
                    // Find or create twin based on the provided registration ID and model ID
                    dynamic payloadContext = data?.deviceRuntimeContext?.payload;
                    string dtmi = payloadContext.modelId;
                    log.LogDebug($"payload.modelId: {dtmi}");
                    string dtId = await FindOrCreateTwin(dtmi, regId, log);

                    // Get first linked hub (TODO: select one of the linked hubs based on policy)
                    obj.iotHubHostName = hubs[0];

                    // Specify the initial tags for the device.
                    TwinCollection tags = new TwinCollection();
                    tags["dtmi"] = dtmi;
                    tags["dtId"] = dtId;

                    // Specify the initial desired properties for the device.
                    TwinCollection properties = new TwinCollection();

                    // Add the initial twin state to the response.
                    TwinState twinState = new TwinState(tags, properties);
                    obj.initialTwin = twinState;
                }
            }

            log.LogDebug("Response: " + ((obj.iotHubHostName != null) ? JsonConvert.SerializeObject(obj) : message));

            return (fail)
                ? new BadRequestObjectResult(message)
                : (ActionResult)new OkObjectResult(obj);
        }

        public static async Task<string> FindOrCreateTwin(string dtmi, string regId, ILogger log)
        {
            // Create Digital Twins client
            var cred = new ManagedIdentityCredential(adtAppId);
            var client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });

            // Find existing twin with registration ID
            string dtId;
            string query = $"SELECT * FROM DigitalTwins T WHERE $dtId = '{regId}' AND IS_OF_MODEL('{dtmi}')";
            AsyncPageable<string> twins = client.QueryAsync(query);
            
            await foreach (string twinJson in twins)
            {
                // Get DT ID from the Twin
                JObject twin = (JObject)JsonConvert.DeserializeObject(twinJson);
                dtId = (string)twin["$dtId"];
                log.LogInformation($"Twin '{dtId}' with Registration ID '{regId}' found in DT");
                return dtId;
            }

            // Not found, so create new twin
            log.LogInformation($"Twin ID not found, setting DT ID to regID");
            dtId = regId; // use the Registration ID as the DT ID

            // Define the model type for the twin to be created
            Dictionary<string, object> meta = new Dictionary<string, object>()
            {
                { "$model", dtmi }
            };
            // Initialize the twin properties
            Dictionary<string, object> twinProps = new Dictionary<string, object>()
            {
                { "$metadata", meta }
            };
            twinProps.Add("Temperature", 0.0);
            await client.CreateDigitalTwinAsync(dtId, System.Text.Json.JsonSerializer.Serialize<Dictionary<string, object>>(twinProps));
            log.LogInformation($"Twin '{dtId}' created in DT");

            return dtId;
        }
    }

    public class ResponseObj
    {
        public string iotHubHostName { get; set; }
        public TwinState initialTwin { get; set; }
    }
}
```

Dosyayı kaydedin ve ardından işlev uygulamanızı yeniden yayımlayın. İşlev uygulamasını yayımlama yönergeleri için, uçtan uca öğreticinin [*uygulamayı yayımlama*](tutorial-end-to-end.md#publish-the-app) bölümüne bakın.

### <a name="configure-your-function"></a>İşlevinizi yapılandırma

Daha sonra, oluşturduğunuz Azure Digital TWINS örneğine başvuruyu içeren işlev uygulamanızda ortam değişkenlerini ayarlamanız gerekir. Uçtan uca öğreticiyi kullandıysanız ([*öğretici: uçtan uca çözümü bağlama*](tutorial-end-to-end.md)), ayar zaten yapılandırılır.

Bu Azure CLı komutuyla ayarı ekleyin:

```azurecli-interactive
az functionapp config appsettings set --settings "ADT_SERVICE_URL=https://<Azure Digital Twins instance _host name_>" -g <resource group> -n <your App Service (function app) name>
```

Son adım öğreticideki [*işlev uygulamasına Izin atama*](tutorial-end-to-end.md#assign-permissions-to-the-function-app) bölümünde açıklandığı gibi, Izinler ve yönetilen kimlik rolü atamasının işlev uygulaması için doğru yapılandırıldığından emin olun.

<!-- 
* Azure AD app registration **_Application (client) ID_** ([find in portal](../articles/digital-twins/how-to-set-up-instance-portal.md#collect-important-values))

```azurecli-interactive
az functionapp config appsettings set --settings "AdtAppId=<Application (client)" ID> -g <resource group> -n <your App Service (function app) name> 
``` -->

### <a name="create-device-provisioning-enrollment"></a>Cihaz sağlama kaydı oluşturma

Daha sonra, **özel bir ayırma işlevi**kullanarak cihaz sağlama hizmeti 'nde bir kayıt oluşturmanız gerekir. Özel ayırma ilkeleri hakkında cihaz sağlama hizmetleri makalesinin [*kayıt oluştur*](../iot-dps/how-to-use-custom-allocation-policies.md#create-the-enrollment) ve [*benzersiz cihaz anahtarlarını türet*](../iot-dps/how-to-use-custom-allocation-policies.md#derive-unique-device-keys) bölümünde bunu yapmak için yönergeleri izleyin.

Bu akıştan gezinirken, bu kaydı yeni oluşturduğunuz işleve bağlayacaksınız. Bu işlem adım adım sırasında, **cihazları hub 'lara nasıl atamak Istediğinizi seçer**. Kayıt oluşturulduktan sonra, bu makalenin cihaz simülatörünü yapılandırmak için kayıt adı ve birincil veya ikincil SAS anahtarı daha sonra kullanılacaktır.

### <a name="set-up-the-device-simulator"></a>Cihaz simülatörünü ayarlama

Bu örnek, cihaz sağlama hizmeti kullanılarak sağlamayı içeren bir cihaz simülatörü kullanır. Cihaz simülatörü şurada bulunur: [Azure dijital TWINS ve IoT Hub tümleştirme örneği](https://docs.microsoft.com/samples/azure-samples/digital-twins-iothub-integration/adt-iothub-provision-sample/). Örneği henüz indirmediyseniz, örnek bağlantısına gidip başlık altında *posta yükle* düğmesini seçerek hemen alın. İndirilen klasörü sıkıştırmayı açın.

Bir komut penceresi açın ve indirilen klasöre ve ardından *cihaz simülatör* dizinine gidin. Aşağıdaki komutu kullanarak projenin bağımlılıklarını yükler:

```cmd
npm install
```

Sonra, *. env. Template* dosyasını *. env*adlı yeni bir dosyaya kopyalayın ve bu ayarları girin:

```cmd
PROVISIONING_HOST = "global.azure-devices-provisioning.net"
PROVISIONING_IDSCOPE = "<Device Provisioning Service Scope ID>"
PROVISIONING_REGISTRATION_ID = "<Device Registration ID>"
ADT_MODEL_ID = "dtmi:contosocom:DigitalTwins:Thermostat;1"
PROVISIONING_SYMMETRIC_KEY = "<Device Provisioning Service enrollment primary or secondary SAS key>"
```

Dosyayı kaydedin ve kapatın.

### <a name="start-running-the-device-simulator"></a>Cihaz simülatörünü çalıştırmaya başla

Hala, komut pencerenizde *cihaz simülatör* dizininde, aşağıdaki komutu kullanarak cihaz simülatörünü başlatın:

```cmd
node .\adt_custom_register.js
```

Kayıtlı ve IoT Hub bağlı olduğunu ve sonra ileti gönderilmeye başladığınızı görmeniz gerekir.
:::image type="content" source="media/how-to-provision-using-dps/output.png" alt-text="Cihaz kaydı ve ileti gönderme Komut penceresi gösterme":::

### <a name="validate"></a>Doğrulama

Bu makalede ayarladığınız akışın bir sonucu olarak, cihaz Azure dijital TWINS 'e otomatik olarak kaydedilir. Aşağıdaki [Azure Digital TWıNS CLI](how-to-use-cli.md) komutunu kullanarak, oluşturduğunuz Azure dijital TWINS örneğindeki cihazın ikizi bulun.

```azurecli-interactive
az dt twin show -n <Digital Twins instance name> --twin-id <Device Registration ID>"
```

Azure dijital TWINS örneğinde bulunan cihazın ikizi görmeniz gerekir.
:::image type="content" source="media/how-to-provision-using-dps/show-provisioned-twin.png" alt-text="Yeni oluşturulan ikizi gösteren Komut penceresi":::

## <a name="auto-retire-device-using-iot-hub-lifecycle-events"></a>IoT Hub yaşam döngüsü olaylarını kullanarak cihazı otomatik olarak devre dışı bırakma

Bu bölümde, aşağıdaki yoldan cihazları otomatik olarak devre dışı bırakmak için Azure dijital TWINS 'e IoT Hub yaşam döngüsü olayları iliştirirsiniz. Bu, [daha önce](#solution-architecture)gösterilen tam mimarinin bir alıntısıdır.

:::image type="content" source="media/how-to-provision-using-dps/retire.png" alt-text="Cihaz akışını devre dışı bırakma--akış, sayı etiketleme bölümlerinin bulunduğu çözüm mimarisi diyagramının bir alıntısıdır. Termostat cihazı, diyagramdaki Azure hizmetleriyle bağlantı olmadan gösterilir. El ile ' cihaz silme ' eyleminden alınan veriler IoT Hub (1) > Event Hubs (2) > Azure Işlevleri > Azure dijital TWINS (3).":::

İşlem akışının açıklaması aşağıda verilmiştir:
1. Bir dış veya el ile işlem, IoT Hub bir cihazın silinmesini tetikler.
2. IoT Hub cihazı siler ve bir [Olay Hub 'ına](../event-hubs/event-hubs-about.md)yönlendirilecek bir [cihaz yaşam döngüsü](../iot-hub/iot-hub-device-management-overview.md#device-lifecycle) olayı oluşturur.
3. Azure işlevi, Azure dijital TWINS 'de cihazın ikizi siler.

Aşağıdaki bölümlerde, bu otomatik devre dışı bırakma cihaz akışını ayarlama adımları gösterilmektedir.

### <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Artık IoT Hub yaşam döngüsü olaylarını almak için kullanılacak bir Azure [Olay Hub](../event-hubs/event-hubs-about.md)'ı oluşturmanız gerekir. 

Aşağıdaki bilgileri kullanarak, [*Olay Hub 'ı oluşturma*](../event-hubs/event-hubs-create.md) hızlı başlangıç bölümünde açıklanan adımları izleyin:
* Uçtan uca öğreticiyi kullanıyorsanız ([*öğretici: uçtan uca bir çözümü bağlama*](tutorial-end-to-end.md)), uçtan uca öğretici için oluşturduğunuz kaynak grubunu yeniden kullanabilirsiniz.
* Olay Hub 'ınızı *lifecycleevents*veya istediğiniz bir şeyi adlandırın ve oluşturduğunuz ad alanını unutmayın. Yaşam döngüsü işlevini ayarlarken ve sonraki bölümlerde yolu IoT Hub, bunları kullanacaksınız.

### <a name="create-an-azure-function"></a>Azure işlevi oluşturma

Sonra, bir işlev uygulaması içinde Event Hubs tetiklenen bir işlev oluşturacaksınız. Uçtan uca öğreticide oluşturulan işlev uygulamasını kullanabilirsiniz ([*öğretici: uçtan uca bir çözümü bağlama*](tutorial-end-to-end.md)) veya kendi kendinize. 

Event hub tetikleyicinizi *lifecycleevents*olarak adlandırın ve Olay Hub 'ı tetikleyicisini önceki adımda oluşturduğunuz Olay Hub 'ına bağlayın. Farklı bir olay hub 'ı adı kullandıysanız, bunu aşağıdaki tetikleyici adı ile eşleşecek şekilde değiştirin.

Bu işlev, var olan bir cihazı devre dışı bırakmak için IoT Hub cihaz yaşam döngüsü olayını kullanacaktır. Yaşam döngüsü olayları hakkında daha fazla bilgi için bkz. [*telemetri dışı olayları IoT Hub*](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events). Azure işlevleri ile Event Hubs kullanma hakkında daha fazla bilgi için bkz. Azure [*için azure Event Hubs tetikleyicisi işlevleri*](../azure-functions/functions-bindings-event-hubs-trigger.md).

Yayınlanan işlev uygulamanızın içinde, *Event hub tetikleyicisi*türünde yeni bir işlev sınıfı ekleyin ve aşağıdaki kodu yapıştırın.

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Threading.Tasks;
using Azure;
using Azure.Core.Pipeline;
using Azure.DigitalTwins.Core;
using Azure.DigitalTwins.Core.Serialization;
using Azure.Identity;
using Microsoft.Azure.EventHubs;
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace Samples.AdtIothub
{
    public static class DeleteDeviceInTwinFunc
    {
        private static string adtAppId = "https://digitaltwins.azure.net";
        private static readonly string adtInstanceUrl = System.Environment.GetEnvironmentVariable("ADT_SERVICE_URL", EnvironmentVariableTarget.Process);
        private static readonly HttpClient httpClient = new HttpClient();

        [FunctionName("DeleteDeviceInTwinFunc")]
        public static async Task Run(
            [EventHubTrigger("lifecycleevents", Connection = "EVENTHUB_CONNECTIONSTRING")] EventData[] events, ILogger log)
        {
            var exceptions = new List<Exception>();

            foreach (EventData eventData in events)
            {
                try
                {
                    //log.LogDebug($"EventData: {System.Text.Json.JsonSerializer.Serialize(eventData)}");

                    string opType = eventData.Properties["opType"] as string;
                    if (opType == "deleteDeviceIdentity")
                    {
                        string deviceId = eventData.Properties["deviceId"] as string;
                        
                        // Create Digital Twin client
                        var cred = new ManagedIdentityCredential(adtAppId);
                        var client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });

                        // Find twin based on the original Registration ID
                        string regID = deviceId; // simple mapping
                        string dtId = await GetTwinId(client, regID, log);
                        if (dtId != null)
                        {
                            await DeleteRelationships(client, dtId, log);

                            // Delete twin
                            await client.DeleteDigitalTwinAsync(dtId);
                            log.LogInformation($"Twin '{dtId}' deleted in DT");
                        }
                    }
                }
                catch (Exception e)
                {
                    // We need to keep processing the rest of the batch - capture this exception and continue.
                    exceptions.Add(e);
                }
            }

            if (exceptions.Count > 1)
                throw new AggregateException(exceptions);

            if (exceptions.Count == 1)
                throw exceptions.Single();
        }


        public static async Task<string> GetTwinId(DigitalTwinsClient client, string regId, ILogger log)
        {
            string query = $"SELECT * FROM DigitalTwins T WHERE T.$dtId = '{regId}'";
            AsyncPageable<string> twins = client.QueryAsync(query);
            await foreach (string twinJson in twins)
            {
                JObject twin = (JObject)JsonConvert.DeserializeObject(twinJson);
                string dtId = (string)twin["$dtId"];
                log.LogInformation($"Twin '{dtId}' found in DT");
                return dtId;
            }

            return null;
        }

        public static async Task DeleteRelationships(DigitalTwinsClient client, string dtId, ILogger log)
        {
            var relationshipIds = new List<string>();

            AsyncPageable<string> relationships = client.GetRelationshipsAsync(dtId);
            await foreach (var relationshipJson in relationships)
            {
                BasicRelationship relationship = System.Text.Json.JsonSerializer.Deserialize<BasicRelationship>(relationshipJson);
                relationshipIds.Add(relationship.Id);
            }

            foreach (var relationshipId in relationshipIds)
            {
                client.DeleteRelationship(dtId, relationshipId);
                log.LogInformation($"Twin '{dtId}' relationship '{relationshipId}' deleted in DT");
            }
        }
    }
}
```

Projeyi kaydedin, sonra işlev uygulamasını yeniden yayımlayın. İşlev uygulamasını yayımlama yönergeleri için, uçtan uca öğreticinin [*uygulamayı yayımlama*](tutorial-end-to-end.md#publish-the-app) bölümüne bakın.

### <a name="configure-your-function"></a>İşlevinizi yapılandırma

Daha sonra, oluşturduğunuz Azure dijital TWINS örneğine ve Olay Hub 'ına başvuruyu içeren işlev uygulamanızda ortam değişkenlerini ayarlamanız gerekir. Uçtan uca öğreticiyi kullandıysanız ([*öğretici: uçtan uca çözümü bağlama*](./tutorial-end-to-end.md)), ilk ayar önceden yapılandırılmıştır.

Bu Azure CLı komutuyla ayarı ekleyin. Bu komut, [makinenizde yüklü](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)Azure CLI 'niz varsa [Cloud Shell](https://shell.azure.com)veya yerel olarak çalıştırılabilir.

```azurecli-interactive
az functionapp config appsettings set --settings "ADT_SERVICE_URL=https://<Azure Digital Twins instance _host name_>" -g <resource group> -n <your App Service (function app) name>
```

Daha sonra, yeni oluşturulan olay hub 'ına bağlanmak için işlev ortam değişkenini yapılandırmanız gerekecektir.

```azurecli-interactive
az functionapp config appsettings set --settings "EVENTHUB_CONNECTIONSTRING=<Event Hubs SAS connection string Listen>" -g <resource group> -n <your App Service (function app) name>
```

Son adım öğreticideki [*işlev uygulamasına Izin atama*](tutorial-end-to-end.md#assign-permissions-to-the-function-app) bölümünde açıklandığı gibi, Izinler ve yönetilen kimlik rolü atamasının işlev uygulaması için doğru yapılandırıldığından emin olun.

### <a name="create-an-iot-hub-route-for-lifecycle-events"></a>Yaşam döngüsü olayları için IoT Hub yolu oluşturma

Artık cihaz yaşam döngüsü olaylarını yönlendirmek için bir IoT Hub yolu ayarlamanız gerekir. Bu durumda, tarafından tanımlanan cihaz silme olaylarını özellikle dinleyecaksınız `if (opType == "deleteDeviceIdentity")` . Bu, bir cihazın ve dijital ikizi 'in kullanımdan kaldırılması sonuçlandırılıyor ve dijital ikizi öğesinin silinmesini tetikler.

IoT Hub yolu oluşturma yönergeleri şu makalede açıklanmıştır: [*farklı uç noktalara cihazdan buluta iletiler göndermek için IoT Hub ileti yönlendirmeyi kullanın*](../iot-hub/iot-hub-devguide-messages-d2c.md). *Telemetri olmayan olaylar* bölümünde, **cihaz yaşam döngüsü olaylarını** yolun veri kaynağı olarak kullanabileceğiniz açıklanmaktadır.

Bu kurulum için uygulamanız gereken adımlar şunlardır:
1. Özel bir IoT Hub Olay Hub 'ı uç noktası oluşturun. Bu uç noktanın, [*Olay Hub 'ı oluşturma*](#create-an-event-hub) bölümünde oluşturduğunuz Olay Hub 'ını hedeflemesi gerekir.
2. *Cihaz yaşam döngüsü olayları* rotası ekleyin. Önceki adımda oluşturulan uç noktayı kullanın. Cihaz yaşam döngüsü olaylarını yalnızca, yönlendirme sorgusunu ekleyerek silme olaylarını gönderecek şekilde sınırlayabilirsiniz `opType='deleteDeviceIdentity'` .
    :::image type="content" source="media/how-to-provision-using-dps/lifecycle-route.png" alt-text="Rota Ekle":::

Bu akıştan doldurduktan sonra, her şey cihazları devre dışı bırakmak için uçtan uca ayarlanır.

### <a name="validate"></a>Doğrulama

Kullanımdan kaldırma işleminin tetiklenmesi için cihazı IoT Hub el ile silmeniz gerekir.

[Bu makalenin ilk yarısında](#auto-provision-device-using-device-provisioning-service)IoT Hub ve karşılık gelen dijital ikizi bir cihaz oluşturdunuz. 

Şimdi IoT Hub gidin ve cihazı silin (bunu bir [Azure CLI komutuyla](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-delete) veya [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Devices%2FIotHubs)' de yapabilirsiniz). 

Cihaz Azure dijital TWINS 'den otomatik olarak kaldırılacak. 

Azure Digital TWINS örneğindeki cihazın ikizi silindiğini doğrulamak için aşağıdaki [Azure Digital TWINS CLI](how-to-use-cli.md) komutunu kullanın.

```azurecli-interactive
az dt twin show -n <Digital Twins instance name> --twin-id <Device Registration ID>"
```

Cihazın ikizi artık Azure dijital TWINS örneğinde bulunamadığını görmeniz gerekir.
:::image type="content" source="media/how-to-provision-using-dps/show-retired-twin.png" alt-text="İkizi gösterme Komut penceresi":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynaklara artık ihtiyacınız yoksa, bunları silmek için aşağıdaki adımları izleyin.

Azure Cloud Shell veya yerel Azure CLı kullanarak, [az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutuyla bir kaynak grubundaki tüm Azure kaynaklarını silebilirsiniz. Bu, kaynak grubunu kaldırır; Azure dijital TWINS örneği; IoT Hub ve Hub cihaz kaydı; olay Kılavuzu konusu ve ilişkili abonelikler; depolama gibi ilişkili kaynaklar da dahil olmak üzere, Olay Hub 'ları ve her ikisi de Azure Işlevleri uygulaması.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. 

```azurecli-interactive
az group delete --name <your-resource-group>
```
<!-- 
Next, delete the Azure AD app registration you created for your client app with this command:

```azurecli
az ad app delete --id <your-application-ID>
``` -->

Ardından, yerel makinenizden indirdiğiniz proje örnek klasörünü silin.

## <a name="next-steps"></a>Sonraki adımlar

Cihazlar için oluşturulan dijital TWINS, Azure dijital TWINS 'de düz bir hiyerarşi olarak depolanır, ancak model bilgileri ve kuruluş için çok düzeyli bir hiyerarşiyle zenginleştirilebilir. Bu kavram hakkında daha fazla bilgi edinmek için şunu okuyun:

* [*Kavramlar: dijital TWINS ve ikizi grafiği*](concepts-twins-graph.md)

Azure dijital TWINS 'te zaten depolanmış olan model ve grafik verilerini kullanarak bu bilgileri otomatik olarak sağlamak için özel mantık yazabilirsiniz. TWINS grafiğindeki bilgileri yönetme, yükseltme ve alma hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [*Nasıl yapılır: dijital ikizi yönetme*](how-to-manage-twin.md)
* [*Nasıl yapılır: ikizi grafiğini sorgulama*](how-to-query-graph.md)