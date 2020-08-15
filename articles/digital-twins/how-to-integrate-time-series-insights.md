---
title: Azure Time Series Insights ile tümleştirme
titleSuffix: Azure Digital Twins
description: Bkz. Azure dijital TWINS 'ten olay yollarını ayarlama Azure Time Series Insights.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 7/14/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: bda07d0e14ddc630bde4fdc9c869704154c1e6cc
ms.sourcegitcommit: 3bf69c5a5be48c2c7a979373895b4fae3f746757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88236361"
---
# <a name="integrate-azure-digital-twins-with-azure-time-series-insights"></a>Azure dijital TWINS 'i Azure Time Series Insights ile tümleştirme

Bu makalede, Azure dijital TWINS 'i [Azure Time Series Insights (TSI)](../time-series-insights/overview-what-is-tsi.md)ile tümleştirmeyi öğreneceksiniz.

Bu makalede açıklanan çözüm, IoT çözümünüz hakkında geçmiş verileri toplayıp analiz etmenize olanak sağlayacak. Azure dijital TWINS, verileri Time Series Insights, birden çok veri akışı ile ilişkilendirmenize ve bilgilerinizi Time Series Insights göndermeden önce standartlaştırmasını sağlayan harika bir şekilde bir uyum sağlar. 

## <a name="prerequisites"></a>Önkoşullar

Time Series Insights bir ilişki ayarlayabilmeniz için önce bir **Azure dijital TWINS örneğine**sahip olmanız gerekir. Bu örnek, verileri temel alarak dijital ikizi bilgilerini güncelleştirme özelliği ile ayarlanmalıdır, çünkü bu verileri Time Series Insights ' de izlenen şekilde görmek için ikizi bilgilerini birkaç kez güncelleştirmeniz gerekir. 

Bu ayarı zaten yoksa, Azure dijital TWINS [*öğreticisini izleyerek oluşturabilirsiniz: uçtan uca çözümü bağlama*](./tutorial-end-to-end.md). Öğreticide, dijital ikizi güncelleştirmelerini tetiklemek için bir sanal IoT cihazıyla birlikte çalışarak bir Azure dijital TWINS örneği ayarlama işleminde size yol gösterilir.

## <a name="solution-architecture"></a>Çözüm mimarisi

Aşağıdaki yoldan Azure dijital TWINS 'e Time Series Insights iliştirilecektir.

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-integrate-time-series-insights/diagram-simple.png" alt-text="Uçtan uca bir senaryoda Azure hizmetlerinin görünümü, vurgulama Time Series Insights" lightbox="media/how-to-integrate-time-series-insights/diagram-simple.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

## <a name="create-a-route-and-filter-to-twin-update-notifications"></a>Bir yol oluşturun ve ikizi Update bildirimlerine filtre uygulayın

Azure dijital TWINS örnekleri, bir ikizi durumunun güncelleştirildiği her seferinde [ikizi Update olaylarını](how-to-interpret-event-data.md) yayabilir. Bu bölümde, daha fazla işleme için bu güncelleştirme olaylarını Azure [Event Hubs](../event-hubs/event-hubs-about.md) 'a yönlendiren bir Azure dijital TWINS [**olay yolu**](concepts-route-events.md) oluşturacaksınız.

Azure dijital TWINS [*öğreticisi: uçtan uca bir çözümü bağlama*](./tutorial-end-to-end.md) bir termometre 'nin bir odanın bulunduğu dijital bir ikizi üzerinde sıcaklık özniteliğini güncelleştirmek için kullanıldığı bir senaryoya yol gösterir. Bu model, Time Series Insights mantığınızı güncelleştirmeye gerek kalmadan temel alınan veri kaynağını değiştirme esnekliği sağlayan bir IoT cihazından telemetri iletmek yerine ikizi güncelleştirmelerini kullanır.

1. İlk olarak, Azure dijital TWINS örneğinden olay alacak bir olay hub 'ı ad alanı oluşturun. Aşağıdaki Azure CLı yönergelerini kullanabilir veya Azure portal kullanabilirsiniz: [*hızlı başlangıç: Azure Portal kullanarak bir olay hub 'ı oluşturma*](../event-hubs/event-hubs-create.md).

    ```azurecli-interactive
    # Create an Event Hubs namespace. Specify a name for the Event Hubs namespace.
    az eventhubs namespace create --name <name for your Event Hubs namespace> --resource-group <resource group name> -l <region, for example: East US>
    ```

2. Ad alanı içinde bir olay hub 'ı oluşturun.

    ```azurecli-interactive
    # Create an event hub to receive twin change events. Specify a name for the event hub. 
    az eventhubs eventhub create --name <name for your Twins event hub> --resource-group <resource group name> --namespace-name <Event Hubs namespace from above>
    ```

3. Gönderme ve alma izinleriyle bir [Yetkilendirme kuralı](https://docs.microsoft.com/cli/azure/eventhubs/eventhub/authorization-rule?view=azure-cli-latest#az-eventhubs-eventhub-authorization-rule-create) oluşturun.

    ```azurecli-interactive
    # Create an authorization rule. Specify a name for the rule.
    az eventhubs eventhub authorization-rule create --rights Listen Send --resource-group <resource group name> --namespace-name <Event Hubs namespace from above> --eventhub-name <Twins event hub name from above> --name <name for your Twins auth rule>
    ```

4. Olay kılavuzunuzun konusunu Azure dijital TWINS örneğinize bağlayan bir Azure dijital TWINS [uç noktası](concepts-route-events.md#create-an-endpoint) oluşturun.

    ```azurecli
    az dt endpoint create eventhub --endpoint-name <name for your Event Hubs endpoint> --eventhub-resource-group <resource group name> --eventhub-namespace <Event Hubs namespace from above> --eventhub <Twins event hub name from above> --eventhub-policy <Twins auth rule from above> -n <your Azure Digital Twins instance name>
    ```

5. Azure dijital TWINS 'de, ikizi Update olaylarını uç noktanıza göndermek için bir [yol](concepts-route-events.md#create-an-event-route) oluşturun. Bu rotadaki filtre yalnızca ikizi güncelleştirme iletilerinin uç noktanıza geçirilmesine izin verir.

    ```azurecli
    az dt route create -n <your Azure Digital Twins instance name> --endpoint-name <Event Hub endpoint from above> --route-name <name for your route> --filter "type = 'Microsoft.DigitalTwins.Twin.Update'"
    ```

Üzerinde geçiş yapmadan önce, bu makalenin ilerleyen kısımlarında daha sonra başka bir olay hub 'ı oluşturmak üzere kullanacağınız için, *Event Hubs ad alanı* ve *kaynak grubunuzu*göz atın.

## <a name="create-an-azure-function"></a>Azure işlevi oluşturma 

Sonra, bir işlev uygulaması içinde Event Hubs tetiklenen bir işlev oluşturacaksınız. Uçtan uca öğreticide oluşturulan işlev uygulamasını kullanabilirsiniz ([*öğretici: uçtan uca bir çözümü bağlama*](./tutorial-end-to-end.md)) veya kendi kendinize. 

Bu işlev, bu ikizi Update olaylarını kendi özgün formdan JSON yaması belgeleri olarak JSON nesnelerine dönüştürür. Bu, yalnızca, TWINS 'inizden yalnızca güncelleştirilmiş ve eklenen değerleri içerir.

Azure işlevleri ile Event Hubs kullanma hakkında daha fazla bilgi için bkz. Azure [*için azure Event Hubs tetikleyicisi işlevleri*](../azure-functions/functions-bindings-event-hubs-trigger.md).

Yayınlanan işlev uygulamanızın içinde, işlev kodunu aşağıdaki kodla değiştirin.

```C#
using Microsoft.Azure.EventHubs;
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System.Threading.Tasks;
using System.Text;
using System.Collections.Generic;

namespace SampleFunctionsApp
{
    public static class ProcessDTUpdatetoTSI
    { 
        [FunctionName("ProcessDTUpdatetoTSI")]
        public static async Task Run(
            [EventHubTrigger("twins-event-hub", Connection = "EventHubAppSetting-Twins")]EventData myEventHubMessage, 
            [EventHub("tsi-event-hub", Connection = "EventHubAppSetting-TSI")]IAsyncCollector<string> outputEvents, 
            ILogger log)
        {
            JObject message = (JObject)JsonConvert.DeserializeObject(Encoding.UTF8.GetString(myEventHubMessage.Body));
            log.LogInformation("Reading event:" + message.ToString());

            // Read values that are replaced or added
            Dictionary<string, object> tsiUpdate = new Dictionary<string, object>();
            foreach (var operation in message["patch"]) {
                if (operation["op"].ToString() == "replace" || operation["op"].ToString() == "add")
                    //Convert from JSON patch path to a flattened property for TSI
                    //Example input: /Front/Temperature
                    //        output: Front.Temperature
                    string path = operation["path"].ToString().Substring(1);                    
                    path = path.Replace("/", ".");                    
                    tsiUpdate.Add(path, operation["value"]);
            }
            //Send an update if updates exist
            if (tsiUpdate.Count>0){
                tsiUpdate.Add("$dtId", myEventHubMessage.Properties["cloudEvents:subject"]);
                await outputEvents.AddAsync(JsonConvert.SerializeObject(tsiUpdate));
            }
        }
    }
}
```

Bundan sonra, işlev, oluşturduğu JSON nesnelerini Time Series Insights bağlanacak ikinci bir olay hub 'ına gönderir.

Daha sonra, bu işlevin kendi olay hub 'larınız ile bağlantı kurmak için kullanacağı bazı ortam değişkenlerini de ayarlayacaksınız.

## <a name="send-telemetry-to-an-event-hub"></a>Bir olay hub 'ına telemetri gönderme

Şimdi ikinci bir olay hub 'ı oluşturacak ve işlevinizi bu olay hub 'ına akışını sağlayacak şekilde yapılandıracaksınız. Daha sonra bu olay hub 'ı Time Series Insights bağlanacak.

### <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

İkinci olay hub 'ını oluşturmak için aşağıdaki Azure CLı yönergelerini kullanabilir veya Azure portal: [*hızlı başlangıç: Azure Portal kullanarak bir olay hub 'ı oluşturun*](../event-hubs/event-hubs-create.md).

1. *Event Hubs ad* alanınızı ve *kaynak grubu* adınızı Bu makalenin önceki kısımlarında hazırlayın

2. Yeni bir olay hub 'ı oluşturun
    ```azurecli-interactive
    # Create an event hub. Specify a name for the event hub. 
    az eventhubs eventhub create --name <name for your TSI event hub> --resource-group <resource group name from earlier> --namespace-name <Event Hubs namespace from earlier>
    ```
3. Gönderme ve alma izinlerine sahip bir [Yetkilendirme kuralı](https://docs.microsoft.com/cli/azure/eventhubs/eventhub/authorization-rule?view=azure-cli-latest#az-eventhubs-eventhub-authorization-rule-create) oluşturma
    ```azurecli-interactive
    # Create an authorization rule. Specify a name for the rule.
    az eventhubs eventhub authorization-rule create --rights Listen Send --resource-group <resource group name> --namespace-name <Event Hubs namespace from earlier> --eventhub-name <TSI event hub name from above> --name <name for your TSI auth rule>
    ```

## <a name="configure-your-function"></a>İşlevinizi yapılandırma

Daha sonra, oluşturduğunuz Olay Hub 'ları için bağlantı dizelerini içeren işlev uygulamanızda ortam değişkenlerini ayarlamanız gerekir.

### <a name="set-the-twins-event-hub-connection-string"></a>TWINS Olay Hub 'ı bağlantı dizesini ayarla

1. TWINS hub 'ı için yukarıda oluşturduğunuz yetkilendirme kurallarını kullanarak TWINS [Olay Hub 'ı bağlantı dizesini](../event-hubs/event-hubs-get-connection-string.md)alın.

    ```azurecli-interactive
    az eventhubs eventhub authorization-rule keys list --resource-group <resource group name> --namespace-name <Event Hubs namespace> --eventhub-name <Twins event hub name from earlier> --name <Twins auth rule from earlier>
    ```

2. İşlev uygulamanızda, Bağlantı dizenizi içeren bir uygulama ayarı oluşturmak için elde ettiğiniz bağlantı dizesini kullanın:

    ```azurecli-interactive
    az functionapp config appsettings set --settings "EventHubAppSetting-Twins=<Twins event hub connection string> -g <resource group> -n <your App Service (function app) name>"
    ```

### <a name="set-the-time-series-insights-event-hub-connection-string"></a>Time Series Insights Olay Hub 'ı bağlantı dizesini ayarla

1. Time Series Insights hub 'ı için yukarıda oluşturduğunuz yetkilendirme kurallarını kullanarak, TSI [Olay Hub 'ı bağlantı dizesini](../event-hubs/event-hubs-get-connection-string.md)alın:

    ```azurecli-interactive
    az eventhubs eventhub authorization-rule keys list --resource-group <resource group name> --namespace-name <Event Hubs namespace> --eventhub-name <TSI event hub name> --name <TSI auth rule>
    ```

2. İşlev uygulamanızda, Bağlantı dizenizi içeren bir uygulama ayarı oluşturun:

    ```azurecli-interactive
    az functionapp config appsettings set --settings "EventHubAppSetting-TSI=<TSI event hub connection string> -g <resource group> -n <your App Service (function app) name>"
    ```

## <a name="create-and-connect-a-time-series-insights-instance"></a>Time Series Insights örneği oluşturma ve bağlama

Sonra, ikinci olay hub 'ından verileri almak için bir Time Series Insights örneği ayarlayacaksınız. Aşağıdaki adımları izleyin ve bu işlemle ilgili daha fazla ayrıntı için bkz. [*öğretici: Azure Time Series Insights Gen2 PAYG ortamı ayarlama*](../time-series-insights/tutorials-set-up-tsi-environment.md).

1. Azure portal, bir Time Series Insights kaynağı oluşturmaya başlayın. 
    1. **PAYG (Önizleme)** fiyatlandırma katmanını seçin.
    2. Bu ortam için bir **zaman SERISI kimliği** seçmeniz gerekir. Zaman serisi KIMLIĞINIZ, Time Series Insights verilerinizi aramak için kullanacağınız üç değerden fazla olabilir. Bu öğretici için **$dtId**kullanabilirsiniz. [*Bir zaman SERISI kimliği seçmek Için en iyi yöntemler*](https://docs.microsoft.com/azure/time-series-insights/how-to-select-tsid)bölümünde kimlik değeri seçme hakkında daha fazla bilgi edinin.
    
        :::image type="content" source="media/how-to-integrate-time-series-insights/create-twin-id.png" alt-text="Time Series Insights ortamı için oluşturma portalı UX. PAYG (Önizleme) Fiyatlandırma Katmanı seçilidir ve zaman serisi KIMLIĞI Özellik adı $dtId":::

2. **İleri ' yi seçin: olay kaynağı** ve yukarıdaki Event Hubs bilgilerinizi seçin. Ayrıca, yeni bir Event Hubs Tüketici grubu oluşturmanız gerekir.
    
    :::image type="content" source="media/how-to-integrate-time-series-insights/event-source-twins.png" alt-text="Time Series Insights ortamı olay kaynağı için oluşturma portalı UX. Yukarıdaki olay hub 'ı bilgileriyle bir olay kaynağı oluşturuyorsunuz. Ayrıca yeni bir tüketici grubu oluşturuyorsunuz.":::

## <a name="begin-sending-iot-data-to-azure-digital-twins"></a>Azure dijital TWINS 'e IoT verileri göndermeye başlama

Time Series Insights veri göndermeye başlamak için Azure Digital TWINS 'te değişen veri değerleriyle Digital ikizi özelliklerini güncelleştirmeye başlamanız gerekir. [Az DT ikizi Update](https://docs.microsoft.com/cli/azure/ext/azure-iot/dt/twin?view=azure-cli-latest#ext-azure-iot-az-dt-twin-update) komutunu kullanın.

Uçtan uca öğreticiyi kullanıyorsanız ([*öğretici: uçtan uca çözümü bağlama*](tutorial-end-to-end.md)) ortam kurulumuna yardımcı olması için, örnekten *devicesimülatör* projesini çalıştırarak sanal IoT verilerini göndermeye başlayabilirsiniz. Yönergeler, öğreticinin [*benzetimini yapılandırın ve çalıştırın*](tutorial-end-to-end.md#configure-and-run-the-simulation) bölümünde bulunur.

## <a name="visualize-your-data-in-time-series-insights"></a>Time Series Insights verilerinizi görselleştirin

Şimdi, verilerin çözümlenmeye hazır Time Series Insights Örneğinizde akışı yapılmalıdır. İçinde gelen verileri araştırmak için aşağıdaki adımları izleyin.

1. Time Series Insights örneğinizi [Azure Portal](https://portal.azure.com) açın (portal arama çubuğunda örneğinizin adını arayabilirsiniz). Örneğe genel bakış bölümünde gösterilen *Time Series Insights gezgin URL 'sini* ziyaret edin.
    
    :::image type="content" source="media/how-to-integrate-time-series-insights/view-environment.png" alt-text="Time Series Insights ortamınızın Genel Bakış sekmesinde Time Series Insights Explorer URL 'sini seçin":::

2. Gezgin 'de, sol tarafta gösterilen Azure dijital TWINS 'den üç TWINS 'nizi görürsünüz. _**Thermostat67**_ öğesini seçin, **sıcaklık**' ı seçin ve **Ekle**' ye basın.

    :::image type="content" source="media/how-to-integrate-time-series-insights/add-data.png" alt-text="* * Thermostat67 * * seçin, * * sıcaklık * * öğesini seçin ve * * Ekle * * ' ye basın":::

3. Artık aşağıda gösterildiği gibi, termostat 'daki ilk sıcaklık ayarlarını görmeniz gerekir. *Room21* ve *Floor1*için aynı sıcaklık okuma güncellenir ve bu veri akışlarını kademeli olarak görselleştirebilirsiniz.
    
    :::image type="content" source="media/how-to-integrate-time-series-insights/initial-data.png" alt-text="İlk sıcaklık verileri, TSI Gezgininde grafiği oluşturulur. 68 ile 85 arasındaki rastgele değerlerin bir satırdır":::

4. Simülasyonun çok daha uzun süre çalışmasına izin verirseniz görselleştirmeniz şuna benzer şekilde görünür:
    
    :::image type="content" source="media/how-to-integrate-time-series-insights/day-data.png" alt-text="Her bir ikizi için sıcaklık verileri, farklı renklerin üç paralel satırı halinde grafiği oluşturulur.":::

## <a name="next-steps"></a>Sonraki adımlar

Dijital TWINS, varsayılan olarak Time Series Insights düz bir hiyerarşi olarak saklanır, ancak model bilgileri ve kuruluş için çok düzeyli bir hiyerarşiyle zenginleştirilebilir. Bu işlem hakkında daha fazla bilgi edinmek için şunu okuyun: 

* [*Öğretici: model tanımlama ve uygulama*](../time-series-insights/tutorials-set-up-tsi-environment.md#define-and-apply-a-model) 

Azure dijital TWINS 'te zaten depolanmış olan model ve grafik verilerini kullanarak bu bilgileri otomatik olarak sağlamak için özel mantık yazabilirsiniz. TWINS grafiğindeki bilgileri yönetme, yükseltme ve alma hakkında daha fazla bilgi edinmek için aşağıdaki başvurulara bakın:

* [*Nasıl yapılır: dijital ikizi yönetme*](./how-to-manage-twin.md)
* [*Nasıl yapılır: ikizi grafiğini sorgulama*](./how-to-query-graph.md)