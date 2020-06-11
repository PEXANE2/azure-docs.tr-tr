---
title: Azure haritalar ınkapısının haritasını güncelleştirmek için Azure dijital TWINS kullanma
titleSuffix: Azure Digital Twins
description: Azure haritalar 'da gösterilen bilgileri güncelleştirmek için ikizi Graph ve Azure dijital TWINS bildirimlerini kullanabileceğiniz bir Azure işlevi oluşturma bölümüne bakın.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 6/3/2020
ms.topic: how-to
ms.service: digital-twins
ms.reviewer: baanders
ms.openlocfilehash: cedd91ed0a6bcce01aea8fd56f06f6523c590016
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84680178"
---
# <a name="use-azure-digital-twins-to-update-an-azure-maps-indoor-map"></a>Azure haritalar ınkapısının haritasını güncelleştirmek için Azure dijital TWINS kullanma

Bu makalede, Azure [Maps](../azure-maps/about-azure-maps.md)kullanarak bir *ınkapılı haritada* görüntülenecek bilgileri güncelleştirmek Için Azure dijital TWINS verilerini kullanmak için gereken adımlar açıklanmaktadır. Azure dijital TWINS, IoT cihaz ilişkilerinizin bir grafiğini depolar ve Telemetriyi farklı uç noktalara yönlendirir ve bu, haritalar üzerinde bilgilendirici Yerpaylaşımları güncelleştirmek için mükemmel bir hizmet sağlar.

Bu şekilde nasıl ele alınacaktır:

1. Azure Digital TWINS örneğinizi, ikizi Update olaylarını [Azure işlevleri](../azure-functions/functions-overview.md)'ndeki bir işleve göndermek için yapılandırma.
2. Azure Maps ınkapılı haritalar Özellik stateset 'i güncelleştirmek için bir Azure işlevi oluşturma.
3. Haritalar KIMLIĞINIZI ve özellik stateset KIMLIĞINIZI Azure dijital TWINS grafiğinde depolama.

### <a name="prerequisites"></a>Ön koşullar

* Azure dijital TWINS [öğreticisini izleyin: uçtan uca çözümü bağlama](./tutorial-end-to-end.md).
    * Bu ikizi ek bir uç nokta ve rotayla genişletiyorsunuz. Ayrıca, bu öğreticiden işlev uygulamanıza başka bir işlev da eklersiniz. 
* Azure haritalar öğreticisini izleyin: bir *özellik stateset*Ile Azure Maps ınkapısı haritası oluşturmak üzere [ınkapılı haritalar oluşturmak Için Azure haritalar Oluşturucu kullanın](../azure-maps/tutorial-creator-indoor-maps.md) .
    * [Özellik statesets](../azure-maps/creator-indoor-maps.md#feature-statesets) 'ler, odalar veya ekipman gibi veri kümesi özelliklerine atanan dinamik Özellikler (eyaletler) koleksiyonlarıdır. Yukarıdaki Azure haritalar öğreticisinde, stateset özelliği bir haritada görüntülenecek oda durumunu depolar.
    * Özellik *stateset ID* ve Azure Maps *abonelik kimliği*gereklidir.

### <a name="topology"></a>Topoloji

Aşağıdaki görüntüde, bu öğreticideki ınkapımaps tümleştirme öğelerinin, daha büyük, uçtan uca bir Azure dijital TWINS senaryosuna hangi noktada uyduğunu gösterilmektedir.

:::image type="content" source="media/how-to-integrate-maps/maps-integration-topology.png" alt-text="Uçtan uca bir senaryoda Azure hizmetlerinin bir görünümü olan, ınkapıharitaları tümleştirme parçasını vurgulama" lightbox="media/how-to-integrate-maps/maps-integration-topology.png":::

## <a name="create-a-function-to-update-a-map-when-twins-update"></a>TWINS güncelleştirme sırasında Haritayı güncelleştirmek için bir işlev oluşturma

İlk olarak, tüm ikizi Update olaylarını bir olay kılavuzu konusuna iletmek için Azure dijital TWINS 'te bir yol oluşturacaksınız. Daha sonra, bu güncelleştirme iletilerini okumak ve Azure Maps 'ta bir özellik stateset 'i güncelleştirmek için bir Azure işlevi kullanacaksınız. 

## <a name="create-a-route-and-filter-to-twin-update-notifications"></a>Bir yol oluşturun ve ikizi Update bildirimlerine filtre uygulayın

Azure dijital TWINS örnekleri, bir ikizi durumunun güncelleştirildiği her seferinde ikizi Update olaylarını yayabilir. [Azure dijital TWINS öğreticisi: Yukarıdaki bir uçtan uca çözümü](./tutorial-end-to-end.md) , bir odanın ikizi 'e iliştirilmiş bir sıcaklık özniteliğini güncelleştirmek için bir termometre 'nin kullanıldığı bir senaryoya yol gösterir. Bu çözümü, TWINS güncelleştirme bildirimlerine abone olarak ve haritalarımızı güncelleştirmek için bu bilgileri kullanarak genişlettireceksiniz.

Bu kalıp, IoT cihazı yerine doğrudan ikizi ' yi okur, bu da, eşleme mantığımızı güncelleştirmeye gerek kalmadan sıcaklığın temelindeki veri kaynağını değiştirme esnekliği sağlar. Örneğin, eşleme mantığımızı güncelleştirmeye gerek kalmadan, birden fazla salon ölçüm ekleyebilir veya bu odayı başka bir oda ile bir termometre paylaşmak üzere ayarlayabilirsiniz.

1. Azure dijital TWINS örneğinden olay alacak bir Event Grid konusu oluşturun.
    ```azurecli
    az eventgrid topic create -g <your-resource-group-name> --name <your-topic-name> -l <region>
    ```

2. Olay kılavuzunuzun konusunu Azure dijital TWINS 'e bağlamak için bir uç nokta oluşturun.
    ```azurecli
    az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> -n <your-Azure-Digital-Twins-instance-name>
    ```

3. Azure dijital TWINS 'de, ikizi Update olaylarını uç noktanıza göndermek için bir yol oluşturun.
    ```azurecli
    az dt route create -n <your-Azure-Digital-Twins-instance-name> --endpoint-name <Event-Grid-endpoint-name> --route-name <my_route> --filter "{ "endpointId": "<endpoint-ID>","filter": "type = 'Microsoft.DigitalTwins.Twin.Update'"}"
    ```

## <a name="create-an-azure-function-to-update-maps"></a>Haritaları güncelleştirmek için bir Azure işlevi oluşturma

[Uçtan uca öğreticiden](./tutorial-end-to-end.md)işlev uygulamamız içinde Event Grid tetiklenen bir işlev oluşturacağız. Bu işlev, bir odanın sıcaklığını güncelleştirmek için bu bildirimleri paketten çıkarın ve Azure Maps özelliği stateset 'e güncelleştirmeler gönderir. 

Başvuru bilgileri için aşağıdaki belgeye bakın: [Azure işlevleri için Azure Event Grid tetikleyicisi](https://docs.microsoft.com/azure/azure-functions/functions-bindings-event-grid-trigger).

İşlev kodunu aşağıdaki kodla değiştirin. Yalnızca TWINS 'in bulunduğu güncelleştirmeleri filtreleyerek, güncelleştirilmiş sıcaklığın okunmasını ve bu bilgileri Azure Maps 'a göndermeyecektir.

```C#
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System;
using System.Threading.Tasks;
using System.Net.Http;

namespace SampleFunctionsApp
{
    public static class ProcessDTUpdatetoMaps
    {   //Read maps credentials from application settings on function startup
        private static string statesetID = Environment.GetEnvironmentVariable("statesetID");
        private static string subscriptionKey = Environment.GetEnvironmentVariable("subscription-key");
        private static HttpClient httpClient = new HttpClient();

        [FunctionName("ProcessDTUpdatetoMaps")]
        public static async Task Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            JObject message = (JObject)JsonConvert.DeserializeObject(eventGridEvent.Data.ToString());
            log.LogInformation("Reading event from twinID:" + eventGridEvent.Subject.ToString() + ": " +
                eventGridEvent.EventType.ToString() + ": " + message["data"]);

            //Parse updates to "space" twins
            if (message["data"]["modelId"].ToString() == "dtmi:contosocom:DigitalTwins:Space;1")
            {   //Set the ID of the room to be updated in our map. 
                //Replace this line with your logic for retrieving featureID. 
                string featureID = "UNIT103";

                //Iterate through the properties that have changed
                foreach (var operation in message["data"]["patch"])
                {
                    if (operation["op"].ToString() == "replace" && operation["path"].ToString() == "/Temperature")
                    {   //Update the maps feature stateset
                        var postcontent = new JObject(new JProperty("States", new JArray(
                            new JObject(new JProperty("keyName", "temperature"),
                                 new JProperty("value", operation["value"].ToString()),
                                 new JProperty("eventTimestamp", DateTime.Now.ToString("s"))))));

                        var response = await httpClient.PostAsync(
                            $"https://atlas.microsoft.com/featureState/state?api-version=1.0&statesetID={statesetID}&featureID={featureID}&subscription-key={subscriptionKey}",
                            new StringContent(postcontent.ToString()));

                        log.LogInformation(await response.Content.ReadAsStringAsync());
                    }
                }
            }
        }
    }
}
```

İşlev uygulamanızda iki ortam değişkeni ayarlamanız gerekir. Bunlardan biri [Azure Maps birincil abonelik anahtarınıza](../azure-maps/quick-demo-map-app.md#get-the-primary-key-for-your-account), diğeri Ise [Azure HARITALAR stateset Kimliğinizle](../azure-maps/tutorial-creator-indoor-maps.md#create-a-feature-stateset)biridir.

```azurecli-interactive
az functionapp config appsettings set --settings "subscription-key=<your-Azure-Maps-primary-subscription-key> -g <your-resource-group> -n <your-App-Service-(function-app)-name>"
az functionapp config appsettings set --settings "statesetID=<your-Azure-Maps-stateset-ID> -g <your-resource-group> -n <your-App-Service-(function-app)-name>
```

### <a name="view-live-updates-on-your-map"></a>Haritadaki canlı güncelleştirmeleri görüntüleme

Canlı güncelleştirme sıcaklığını görmek için aşağıdaki adımları izleyin:

1. Azure dijital TWINS öğreticisindeki **Devicesimülatör** projesini çalıştırarak sanal IoT verilerini göndermeye başlayın [: uçtan uca bir çözüm bağlayın](tutorial-end-to-end.md). Bu yönergeler, [*benzetimi Yapılandır ve Çalıştır*](././tutorial-end-to-end.md#configure-and-run-the-simulation) bölümünde bulunur.
2. Azure haritalar Oluşturucu 'da oluşturulan ınkapısı haritalarınızı oluşturmak için [ **Azure Maps ınkapısı** modülünü](../azure-maps/how-to-use-indoor-module.md) kullanın.
    1. Örneğin, HTML 'yi örnek: ınkapıharitaları öğreticisinin [*ınkapıharitaları modülünü kullanın*](../azure-maps/how-to-use-indoor-module.md#example-use-the-indoor-maps-module) [: Azure Maps ınkapısı haritaları modülünü](../azure-maps/how-to-use-indoor-module.md) yerel bir dosya ile kullanın.
    1. Yerel HTML dosyasındaki *tilesetıd* ve *statesetıd* değerlerini değerlerinizle değiştirin.
    1. Bu dosyayı tarayıcınızda açın.

Her iki örnek de sıcaklığın uyumlu bir aralığa gönderilmesini sağlamak için, her 30 saniyede bir haritada oda 121 güncelleştirme rengini görmeniz gerekir.

:::image type="content" source="media/how-to-integrate-maps/maps-temperature-update.png" alt-text="Oda 121 renkli turuncu gösteren bir Office Haritası":::

## <a name="store-your-maps-information-in-azure-digital-twins"></a>Haritalar bilgilerinizi Azure dijital TWINS 'te depolayın

Haritalar bilgilerinizi güncelleştirmek için bir tek kodlanmış çözümünüz olduğuna göre, ınkapımanız eşlemenizi güncelleştirmek için gereken tüm bilgileri depolamak için Azure dijital TWINS grafiğini kullanabilirsiniz. Bu, sırasıyla, her haritanın ve konumun durum kümesi KIMLIĞINI, eşleme abonelik KIMLIĞINI ve özellik KIMLIĞINI içerir. 

Bu belirli örneğe yönelik bir çözüm, her üst düzey alanın bir stateset KIMLIĞI ve Maps abonelik KIMLIĞI özniteliğiyle güncelleştirilmesini ve her odanın bir özellik KIMLIĞINE sahip olacak şekilde güncelleştirilmesini içerir. İkizi grafiğini başlatırken bu değerleri bir kez ayarlamanız gerekir, ardından bu değerleri her bir ikizi Update olayı için sorgulayın.

Topolojinizin yapılandırmasına bağlı olarak, bu üç özniteliği Haritalarınızın ayrıntı düzeyi ile ilişkili farklı düzeylerde depolayabileceksiniz.

## <a name="next-steps"></a>Sonraki adımlar

TWINS grafiğindeki bilgileri yönetme, yükseltme ve alma hakkında daha fazla bilgi edinmek için aşağıdaki başvurulara bakın:

* [Nasıl yapılır: dijital ikizi yönetme](./how-to-manage-twin.md)
* [Nasıl yapılır: ikizi grafiğini sorgulama](./how-to-query-graph.md)