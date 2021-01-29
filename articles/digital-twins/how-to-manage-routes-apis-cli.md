---
title: Uç noktaları ve yolları yönetme (API 'Ler ve CLı)
titleSuffix: Azure Digital Twins
description: Bkz. Azure dijital TWINS verileri için uç noktaları ve olay yollarını ayarlama ve yönetme.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 11/18/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: e2623ebf929f6a24cfc977896acea514634ffb23
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99054523"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins-apis-and-cli"></a>Azure dijital TWINS 'te uç noktaları ve yolları yönetme (API 'Ler ve CLı)

[!INCLUDE [digital-twins-route-selector.md](../../includes/digital-twins-route-selector.md)]

Azure dijital TWINS 'de, [olay bildirimlerini](how-to-interpret-event-data.md) aşağı akış hizmetlerine veya bağlı işlem kaynaklarına yönlendirebilirsiniz. Bunu yapmak için öncelikle olayları alabilecek **uç noktaları** ayarlamanız gerekir. Daha sonra, Azure dijital TWINS tarafından oluşturulan olayların hangi uç noktalara teslim edildiğini belirten  [**olay yolları**](concepts-route-events.md) oluşturabilirsiniz.

Bu makalede [REST API 'leri](/rest/api/azure-digitaltwins/), [.net (C#) SDK](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true)ve [Azure dijital TWINS CLI](how-to-use-cli.md)ile uç noktalar ve rotalar oluşturma işlemi adım adım açıklanmaktadır.

Alternatif olarak, [Azure Portal](https://portal.azure.com)uç noktalarını ve yolları da yönetebilirsiniz. Bunun yerine portalı kullanan Bu makalenin bir sürümü için bkz. [*nasıl yapılır: uç noktaları ve yolları yönetme (portal)*](how-to-manage-routes-portal.md).

## <a name="prerequisites"></a>Önkoşullar

- Bir **Azure hesabınızın** olması gerekir ( [buradan](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)birini ücretsiz olarak ayarlayabilirsiniz)
- Azure aboneliğinizde bir **Azure dijital TWINS örneği** gerekir. Zaten bir örneğiniz yoksa, [*nasıl yapılır: örnek ve kimlik doğrulaması ayarlama*](how-to-set-up-instance-cli.md)bölümündeki adımları kullanarak bir tane oluşturabilirsiniz. Bu makalede daha sonra kullanmak için kurulum 'un aşağıdaki değerlerini kullanın:
    - Örnek adı
    - Kaynak grubu

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]
    
## <a name="create-an-endpoint-for-azure-digital-twins"></a>Azure dijital TWINS için uç nokta oluşturma

Bunlar, örneğiniz için oluşturabileceğiniz desteklenen uç nokta türleridir:
* [Event Grid](../event-grid/overview.md)
* [Event Hubs](../event-hubs/event-hubs-about.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Farklı uç nokta türleri hakkında daha fazla bilgi için bkz. [*Azure mesajlaşma hizmetleri arasında seçim yapın*](../event-grid/compare-messaging-services.md).

Bu bölümde, Azure CLı kullanılarak bu uç noktaların nasıl oluşturulduğu açıklanmaktadır. Ayrıca, [Digitaltwınsendpoint denetim düzlemi API 'leri](/rest/api/digital-twins/controlplane/endpoints)ile uç noktaları yönetebilirsiniz.

[!INCLUDE [digital-twins-endpoint-resources.md](../../includes/digital-twins-endpoint-resources.md)]

### <a name="create-the-endpoint"></a>Uç noktayı oluşturma

Uç nokta kaynaklarını oluşturduktan sonra bunları bir Azure dijital TWINS uç noktası için kullanabilirsiniz. Aşağıdaki örneklerde, `az dt endpoint create` [Azure Digital TWıNS CLI](how-to-use-cli.md)için komutunu kullanarak uç noktaların nasıl oluşturulacağı gösterilmektedir. Komutlardaki yer tutucuları kendi kaynaklarınızın ayrıntıları ile değiştirin.

Event Grid uç noktası oluşturmak için:

```azurecli-interactive
az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> -n <your-Azure-Digital-Twins-instance-name>
```

Event Hubs uç noktası oluşturmak için:
```azurecli-interactive
az dt endpoint create eventhub --endpoint-name <Event-Hub-endpoint-name> --eventhub-resource-group <Event-Hub-resource-group> --eventhub-namespace <Event-Hub-namespace> --eventhub <Event-Hub-name> --eventhub-policy <Event-Hub-policy> -n <your-Azure-Digital-Twins-instance-name>
```

Service Bus konu uç noktası oluşturmak için:
```azurecli-interactive 
az dt endpoint create servicebus --endpoint-name <Service-Bus-endpoint-name> --servicebus-resource-group <Service-Bus-resource-group-name> --servicebus-namespace <Service-Bus-namespace> --servicebus-topic <Service-Bus-topic-name> --servicebus-policy <Service-Bus-topic-policy> -n <your-Azure-Digital-Twins-instance-name>
```

Bu komutları başarıyla çalıştırdıktan sonra, olay Kılavuzu, Olay Hub 'ı veya Service Bus konusu, Azure dijital TWINS 'in içindeki bir uç nokta olarak, bağımsız değişkenle sağladığınız ad ile birlikte kullanılabilir `--endpoint-name` . Genellikle bu adı, [Bu makalede daha sonra](#create-an-event-route)oluşturacağınız bir **olay yolunun** hedefi olarak kullanacaksınız.

### <a name="create-an-endpoint-with-dead-lettering"></a>Etkin olmayan bir uç nokta oluşturma

Bir uç nokta belirli bir süre içinde bir olayı teslim edimezse veya olayı belirli bir sayıda sunmaya çalıştıktan sonra, teslim edilmemiş olayı bir depolama hesabına gönderebilir. Bu işlem, **atılacak** olarak bilinir.

Atılacak alma hakkında daha fazla bilgi edinmek için bkz. [*Kavramlar: olay yolları*](concepts-route-events.md#dead-letter-events). Bir uç noktanın etkin olmayan bir şekilde nasıl ayarlanacağı hakkında yönergeler için, bu bölümün geri kalanında devam edin.

#### <a name="set-up-storage-resources"></a>Depolama kaynaklarını ayarlama

Atılacak mektup konumunu ayarlamadan önce, Azure hesabınızda ayarlanmış bir [kapsayıcı](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container) içeren bir [depolama hesabınız](../storage/common/storage-account-create.md?tabs=azure-portal) olmalıdır. 

Uç noktayı daha sonra oluştururken bu kapsayıcının URL 'sini sağlarsınız. Atılacak harf konumu, uç noktaya bir [SAS belirtecine](../storage/common/storage-sas-overview.md)sahip kapsayıcı URL 'si olarak sunulacaktır. Bu belirteç `write` , depolama hesabındaki hedef kapsayıcı için izin istiyor. Tamamen oluşturulmuş URL şu biçimde olacaktır: `https://<storageAccountname>.blob.core.windows.net/<containerName>?<SASToken>` .

Sonraki bölümde uç nokta bağlantısını ayarlamaya hazırlanmak için Azure hesabınızda bu depolama kaynaklarını ayarlamak üzere aşağıdaki adımları izleyin.

1. Azure aboneliğinizde bir **depolama hesabı** oluşturmak için [*depolama hesabı oluşturma*](../storage/common/storage-account-create.md?tabs=azure-portal) bölümündeki adımları izleyin. Daha sonra kullanmak için depolama hesabı adını bir yere göz önüne alın.
2. Yeni depolama hesabı içinde **kapsayıcı** oluşturmak Için [*kapsayıcı oluşturma*](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container) bölümündeki adımları izleyin. Daha sonra kullanmak için kapsayıcı adı ' nı bir yere getirin.
3. Daha sonra, depolama hesabınız için bitiş noktasının bu noktaya erişmek için kullanabileceği bir **SAS belirteci** oluşturun. [Azure Portal](https://ms.portal.azure.com/#home) depolama hesabınıza giderek başlayın (portal arama çubuğu ile adı ile bulabilirsiniz).
4. Depolama hesabı sayfasında, SAS belirtecini ayarlamaya başlamak için sol gezinti çubuğundaki _paylaşılan erişim imzası_ bağlantısını seçin.

    :::image type="content" source="./media/how-to-manage-routes-apis-cli/generate-sas-token-1.png" alt-text="Azure portal depolama hesabı sayfası" lightbox="./media/how-to-manage-routes-apis-cli/generate-sas-token-1.png":::

1. *Paylaşılan erişim imzası sayfasında*, *izin verilen hizmetler* ve *izin verilen kaynak türleri* altında istediğiniz ayarları seçin. Her kategoride en az bir kutu seçmeniz gerekir. *Izin verilen izinler* altında, **yaz** ' ı seçin (isterseniz diğer izinleri de seçebilirsiniz).
1. Geri kalan ayarlar için istediğiniz değerleri ayarlayın.
1. İşiniz bittiğinde SAS belirtecini oluşturmak için _SAS ve bağlantı dizesi oluştur_ düğmesini seçin. 

    :::image type="content" source="./media/how-to-manage-routes-apis-cli/generate-sas-token-2.png" alt-text="Azure portal bir SAS belirteci oluşturmak için tüm ayar seçimini gösteren ve ' SA ve bağlantı dizesi oluştur ' düğmesini vurgulamadaki depolama hesabı sayfası" lightbox="./media/how-to-manage-routes-apis-cli/generate-sas-token-2.png"::: 

1. Bu işlem, ayar seçimlerinin altında, aynı sayfanın alt kısmında birkaç SAS ve bağlantı dizesi değeri oluşturur. Değerleri görüntülemek için aşağı kaydırın ve **SAS belirteç** değerini kopyalamak Için *Panoya Kopyala* simgesini kullanın. Daha sonra kullanmak üzere kaydedin.

    :::image type="content" source="./media/how-to-manage-routes-apis-cli/copy-sas-token.png" alt-text="Kullanılmayan harf gizli dosyasında kullanmak için SAS belirtecini kopyalayın." lightbox="./media/how-to-manage-routes-apis-cli/copy-sas-token.png":::
    
#### <a name="configure-the-endpoint"></a>Uç noktayı yapılandırma

Etkin olmayan bir uç nokta oluşturmak için Azure Resource Manager API 'Lerini kullanarak uç nokta oluşturabilirsiniz. 

1. İlk olarak, bir uç nokta oluşturma isteği ayarlamak için [Azure Resource Manager API 'leri belgelerini](/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate) kullanın ve gerekli istek parametrelerini girin. 

2. Sonra, `deadLetterSecret` istek **gövdesinde** Özellikler nesnesine bir alan ekleyin. Bu değeri aşağıdaki şablona göre ayarlayın, bu, [önceki bölümde](#set-up-storage-resources)topladığınız depolama hesabı adı, kapsayıcı adı ve SAS belirteci DEĞERINDEN bir URL öğelerini.
      
  :::code language="json" source="~/digital-twins-docs-samples/api-requests/deadLetterEndpoint.json":::

3. Uç noktayı oluşturmak için isteği gönderin.

Bu isteği yapılandırma hakkında daha fazla bilgi için bkz. Azure Digital TWINS REST API belgeleri: [uç noktalar-DigitalTwinsEndpoint CreateOrUpdate](/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate).

### <a name="message-storage-schema"></a>İleti depolama şeması

Etkin olmayan uç nokta ayarlandıktan sonra, kullanılmayan iletiler depolama hesabınızda aşağıdaki biçimde depolanır:

`{container}/{endpointName}/{year}/{month}/{day}/{hour}/{eventId}.json`

Kullanılmayan iletiler, özgün uç noktanıza teslim edilmesi amaçlanan özgün olayın şemasıyla eşleşir.

Aşağıda, bir [ikizi Create bildirimi](how-to-interpret-event-data.md#digital-twin-life-cycle-notifications)için atılacak ileti iletisine bir örnek verilmiştir:

```json
{
  "specversion": "1.0",
  "id": "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.DigitalTwins.Twin.Create",
  "source": "<yourInstance>.api.<yourregion>.da.azuredigitaltwins-test.net",
  "data": {
    "$dtId": "<yourInstance>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "$etag": "W/\"xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
    "TwinData": "some sample",
    "$metadata": {
      "$model": "dtmi:test:deadlettermodel;1",
      "room": {
        "lastUpdateTime": "2020-10-14T01:11:49.3576659Z"
      }
    }
  },
  "subject": "<yourInstance>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "time": "2020-10-14T01:11:49.3667224Z",
  "datacontenttype": "application/json",
  "traceparent": "00-889a9094ba22b9419dd9d8b3bfe1a301-f6564945cb20e94a-01"
}
```

## <a name="create-an-event-route"></a>Olay yolu oluşturma

**Önkoşul**: bir yol oluşturmak için geçiş yapabilmeniz için önce Bu makalenin önceki kısımlarında açıklandığı gibi uç noktalar oluşturmanız gerekir. Uç noktalarınız kurulum tamamlandıktan sonra bir olay rotası oluşturmaya devam edebilirsiniz.

> [!NOTE]
> Son noktalarınızı dağıttıysanız, yeni bir olay yolu için kullanmayı denemeden **önce** bunların dağıtımını tamamladığınızı doğrulayın. Uç noktalar kullanılamadığından yönlendirme dağıtımı başarısız olursa birkaç dakika bekleyip yeniden deneyin.
>
> Bu akışı komut dosyası olarak belirlerseniz, uç nokta hizmeti 'nin yönlendirme kurulumuna geçmeden önce dağıtımı tamamlaması için 2-3 dakika boyunca bir süre oluşturarak bu işlemi hesaba eklemek isteyebilirsiniz.

Azure dijital TWINS 'den bir uç noktaya gerçek veri göndermek için bir **olay yolu** tanımlamanız gerekir. Olay rotaları, sistem ve aşağı akış hizmetleri genelinde olay akışını bağlamak için kullanılır.

Bir yol tanımı şu öğeleri içerebilir:
* Kullanmak istediğiniz yol adı
* Kullanmak istediğiniz uç noktanın adı
* Uç noktaya gönderilen olayları tanımlayan filtre 

Yol adı yoksa, Azure dijital TWINS dışında hiçbir ileti yönlendirilmez. Bir yol adı varsa ve filtre ise `true` , tüm iletiler uç noktaya yönlendirilir. Bir yol adı varsa ve farklı bir filtre eklenirse, iletiler filtreye göre filtrelenecektir.

Bir yol birden çok bildirimin ve olay türünün seçilebilmelidir. 

Olay yolları Azure Digital TWINS [ **eventroutes** veri düzlemi API 'leri](/rest/api/digital-twins/dataplane/eventroutes) veya [ **az DT Route** CLI komutlarıyla](/cli/azure/ext/azure-iot/dt/route?view=azure-cli-latest&preserve-view=true)oluşturulabilir. Bu bölümün geri kalanında oluşturma sürecinde adım adım gösterilmektedir.

### <a name="create-routes-with-the-apis-and-c-sdk"></a>API 'Ler ve C# SDK 'Sı ile rotalar oluşturma

Olay yollarını tanımlamanın bir yolu, [veri düzlemi API](how-to-use-apis-sdks.md#overview-data-plane-apis)'lardır. Bu bölümdeki örnekler [.net (C#) SDK 'sını](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true)kullanır.

`CreateOrReplaceEventRouteAsync` , bir olay yolu eklemek için kullanılan SDK çağrıdır. Kullanım örneği aşağıda verilmiştir:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/eventRoute_operations.cs" id="CreateEventRoute":::
    
> [!TIP]
> Tüm SDK işlevleri, zaman uyumlu ve zaman uyumsuz sürümlerde gelir.

#### <a name="event-route-sample-sdk-code"></a>Olay rotası örnek SDK kodu

Aşağıdaki örnek yöntem, C# SDK ile bir olay yolunun nasıl oluşturulduğunu, ekleneceğini ve silineceğini gösterir:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/eventRoute_operations.cs" id="FullEventRouteSample":::

### <a name="create-routes-with-the-cli"></a>CLı ile rotalar oluşturma

Yollar, Azure Digital TWINS CLı için [az DT Route](/cli/azure/ext/azure-iot/dt/route?view=azure-cli-latest&preserve-view=true) komutları kullanılarak da yönetilebilir. 

CLı ve kullanılabilen komutları kullanma hakkında daha fazla bilgi için bkz. [*nasıl yapılır: Azure dijital TWINS CLI 'Yi kullanma*](how-to-use-cli.md).

## <a name="filter-events"></a>Olayları filtreleme

Bitiş noktaları, filtrelemeden Azure dijital TWINS 'ten çeşitli olaylar alır:
* Azure Digital TWINS hizmet API 'SI kullanılarak [dijital TWINS](concepts-twins-graph.md) tarafından tetiklenen telemetri
* İkizi özellik değişikliği bildirimleri, Azure dijital TWINS örneğindeki herhangi bir ikizi için özellik değişikliklerinde harekete geçirilir
* Yaşam döngüsü olayları, TWINS veya ilişkiler oluşturulduğunda veya silindiğinde tetiklenir

Olay yolıza bir uç nokta için **filtre** ekleyerek gönderilmekte olan olayları kısıtlayabilirsiniz.

Bir filtre eklemek için, aşağıdaki gövdeyi kullanarak *https://{-Azure-Digital-Twins-hostname}/eventRoutes/{Event-Route-Name}. api-Version = 2020-10-31* IÇIN bir PUT isteği kullanabilirsiniz:

:::code language="json" source="~/digital-twins-docs-samples/api-requests/filter.json":::

Desteklenen yol filtreleri aşağıda verilmiştir. Yukarıdaki istek gövdesinde yer tutucuyu değiştirmek için *filtre metin şeması* sütunundaki ayrıntıyı kullanın `<filter-text>` .

[!INCLUDE [digital-twins-route-filters](../../includes/digital-twins-route-filters.md)]

[!INCLUDE [digital-twins-route-metrics](../../includes/digital-twins-route-metrics.md)]

## <a name="next-steps"></a>Sonraki adımlar

Alabilmeniz için farklı olay iletisi türleri hakkında bilgi edinin:
* [*Nasıl yapılır: olay verilerini yorumlama*](how-to-interpret-event-data.md)
