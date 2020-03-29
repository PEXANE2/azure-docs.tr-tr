---
title: CLI kullanarak Azure Medya Hizmetleri olaylarını Olay Izgaraile izleyin | Microsoft Dokümanlar
description: Bu makalede, Azure Medya Hizmetleri olaylarını izlemek için Olay Ağıt'a nasıl abone olunur gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 11/09/2018
ms.author: juliako
ms.openlocfilehash: 619d40ab56715b4444d8e5649c7fb3401b3f57ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71937281"
---
# <a name="create-and-monitor-media-services-events-with-event-grid-using-the-azure-cli"></a>Azure CLI'yi kullanarak Olay Ağıtı ile Medya Hizmetleri etkinlikleri oluşturma ve izleme

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu hizmet, olay iletilerini abonelere yönlendirmek için [olay aboneliklerini](../../event-grid/concepts.md#event-subscriptions) kullanır. Medya Hizmetleri olayları, verilerinizdeki değişikliklere yanıt vermek için gereken tüm bilgileri içerir. EventType özelliği "Microsoft.Media" ile başladığı için bir Medya Hizmetleri olayını tanımlayabilirsiniz. Daha fazla bilgi için Medya [Hizmetleri etkinlik şemalarına](media-services-event-schemas.md)bakın.

Bu makalede, Azure Medya Hizmetleri hesabınızdaki etkinliklere abone olmak için Azure CLI'yi kullanıyorsunuz. Ardından, sonucu görüntülemek için olayları tetiklersiniz. Normalde olayları, olay verilerini işleyen ve eylemler gerçekleştiren bir uç noktaya gönderirsiniz. Bu makalede, olayları iletileri toplayan ve görüntüleyen bir web uygulamasına gönderirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) bir hesap oluşturun.
- CLI'yi yerel olarak yükleyin ve kullanın, bu makale azure CLI sürüm 2.0 veya daha sonragerektirir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

    Şu anda, tüm [Medya Hizmetleri v3 CLI](https://aka.ms/ams-v3-cli-ref) komutları Azure Bulut Kabuğu'nda çalışmaz. CLI'nin yerel olarak kullanılması önerilir.

- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)

    Kaynak grubu adı ve Medya Hizmetleri hesap adı için kullandığınız değerleri hatırladığınızdan emin olun.

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Medya Hizmetleri hesabının olaylarına abone olmadan önce, olay iletisi için bitiş noktasını oluşturalım. Normalde, olay verileri temelinde uç nokta eylemleri gerçekleştirir. Bu makalede, olay iletilerini görüntüleyen önceden oluşturulmuş bir [web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtmış sınız. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

1. Çözümü aboneliğinize dağıtmak için **Azure'a Dağıt**'ı seçin. Azure portalında parametre değerlerini girin.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

1. Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

"Azure Olay Izgara Görüntüleyicisi" sitesine geçerseniz, henüz herhangi bir etkinliği olmadığını görürsünüz.
   
[!INCLUDE [event-grid-register-provider-portal.md](../../../includes/event-grid-register-provider-portal.md)]

## <a name="set-the-azure-subscription"></a>Azure aboneliğini ayarlama

Aşağıdaki komutta, Media Services hesabı için kullanmak istediğiniz Azure abonelik kimliğini sağlayın. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)’e giderek erişimine sahip olduğunuz aboneliklerin listesini görebilirsiniz.

```azurecli
az account set --subscription mySubscriptionId
```

## <a name="subscribe-to-media-services-events"></a>Medya Hizmetleri etkinliklerine abone olun

Olay Grid'e hangi olayları izlemek istediğinizi söylemek için bir makaleye abone olabilirsiniz. Aşağıdaki örnek, oluşturduğunuz Medya Hizmetleri hesabına abone dir ve URL'yi olay bildiriminin bitiş noktası olarak oluşturduğunuz web sitesinden geçirir. 

Etkinlik `<event_subscription_name>` aboneliğiniz için benzersiz bir adla değiştirin. Medya `<resource_group_name>` `<ams_account_name>`Hizmetleri hesabı oluştururken kullandığınız değerleri için ve kullanın. Bunun `<endpoint_URL>`için, web uygulamanızın URL'sini sağlayın ve ana sayfa URL'sine ekleyin. `api/updates` Abone yken bitiş noktasını belirterek, Olay Grid olayların bu uç noktaya yönlendirmesini işler. 

1. Kaynak kimliğini alma

    ```azurecli
    amsResourceId=$(az ams account show --name <ams_account_name> --resource-group <resource_group_name> --query id --output tsv)
    ```

    Örnek:

    ```
    amsResourceId=$(az ams account show --name amsaccount --resource-group amsResourceGroup --query id --output tsv)
    ```

2. Etkinliklere abone olun

    ```azurecli
    az eventgrid event-subscription create \
    --source-resource-id $amsResourceId \
    --name <event_subscription_name> \
    --endpoint <endpoint_URL>
    ```

    Örnek:

    ```
    az eventgrid event-subscription create --source-resource-id $amsResourceId --name amsTestEventSubscription --endpoint https://amstesteventgrid.azurewebsites.net/api/updates/
    ```    

    > [!TIP]
    > Doğrulama el sıkışma uyarısı alabilirsiniz. Birkaç dakika verin ve el sıkışma doğrulamak gerekir.

Şimdi, Olay Idamı'nın iletiyi bitiş noktanıza nasıl dağıttığını görmek için olayları tetikleyelim.

## <a name="send-an-event-to-your-endpoint"></a>Uç noktanıza olay gönderme

Bir kodlama işi çalıştırarak Medya Hizmetleri hesabı için olayları tetikleyebilirsiniz. Bir dosyayı kodlamak ve olayları göndermeye başlamak için [bu hızlı başlatıizleyin.](stream-files-dotnet-quickstart.md) 

Web uygulamanızı yeniden görüntüleyin ve buna bir abonelik doğrulama olayının gönderildiğine dikkat edin. Uç noktanın olay verilerini almak istediğini doğrulayabilmesi için Event Grid doğrulama olayını gönderir. Bitiş `validationResponse` `validationCode`noktası. Daha fazla bilgi için [Olay Izgara güvenliği ve kimlik doğrulaması'na](../../event-grid/security-authentication.md)bakın. Aboneliği nasıl doğruladığını görmek için web uygulama kodunu görüntüleyebilirsiniz.

> [!TIP]
> Göz simgesini seçerek olay verilerini genişletin. Tüm olayları görüntülemek istiyorsanız sayfayı yenilemayın.

![Abonelik olayını görüntüleme](./media/monitor-events-portal/view-subscription-event.png)

## <a name="next-steps"></a>Sonraki adımlar

[Karşıya yükleme, kodlama ve akışla aktarma](stream-files-tutorial-with-api.md)

