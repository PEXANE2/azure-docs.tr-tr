---
title: Azure SignalR Hizmeti etkinlikleri Etkinlik Ağılasına nasıl gönderilir?
description: SignalR Hizmetiniz için Olay Izgara olaylarını nasıl etkinleştirdiğinizi ve ardından istemci bağlantısına bağlı/bağlantısı kesilen olayları örnek bir uygulamaya nasıl göndereceğinizi gösteren bir kılavuz.
services: signalr
author: chenyl
ms.service: signalr
ms.topic: conceptual
ms.date: 11/13/2019
ms.author: chenyl
ms.openlocfilehash: a76c9aaabf984723e2b60a7cd42425c9b29c916a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76710822"
---
# <a name="how-to-send-events-from-azure-signalr-service-to-event-grid"></a>Azure SignalR Hizmeti'nden Event Grid'e olayları gönderme

Azure Olay Idamı, pub alt modeli kullanarak tek tip olay tüketimi sağlayan tam olarak yönetilen bir olay yönlendirme hizmetidir. Bu kılavuzda, bir Azure SinyalR Hizmeti oluşturmak, bağlantı olaylarına abone olmak ve olayları almak için örnek bir web uygulaması dağıtmak için Azure CLI'yi kullanıyorsunuz. Son olarak, bağlantı kurabilir, bağlantıyı kesebilir ve örnek uygulamada olay yükünü görebilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz][azure-account] bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu makaledeki Azure CLI komutları **Bash** kabuğu için biçimlendirilir. PowerShell veya Command Prompt gibi farklı bir kabuk kullanıyorsanız, satır devam karakterlerini veya değişken atama satırlarını buna göre ayarlamanız gerekebilir. Bu makalede, gerekli komut düzenleme miktarını en aza indirmek için değişkenler kullanır.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarınızı dağıtıp yönettiğiniz mantıksal bir kapsayıcıdır. Aşağıdaki [az grup oluşturma][az-group-create] komutu *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturur. Kaynak grubunuz için farklı bir ad kullanmak `RESOURCE_GROUP_NAME` istiyorsanız, farklı bir değerayarlayın.

```azurecli-interactive
RESOURCE_GROUP_NAME=myResourceGroup

az group create --name $RESOURCE_GROUP_NAME --location eastus
```

## <a name="create-a-signalr-service"></a>SignalR Hizmeti oluşturma

Ardından, aşağıdaki komutları içeren kaynak grubuna bir Azure SinyalHizmeti dağıtın.
```azurecli-interactive
SIGNALR_NAME=SignalRTestSvc

az signalr create --resource-group $RESOURCE_GROUP_NAME --name $SIGNALR_NAME --sku Free_F1
```

SignalR Hizmeti oluşturulduktan sonra, Azure CLI çıktısını aşağıdakilere benzer şekilde döndürür:

```json
{
  "externalIp": "13.76.156.152",
  "hostName": "clitest.servicedev.signalr.net",
  "hostNamePrefix": "clitest",
  "id": "/subscriptions/28cf13e2-c598-4aa9-b8c8-098441f0827a/resourceGroups/clitest1/providers/Microsoft.SignalRService/SignalR/clitest",
  "location": "southeastasia",
  "name": "clitest",
  "provisioningState": "Succeeded",
  "publicPort": 443,
  "resourceGroup": "clitest1",
  "serverPort": 443,
  "sku": {
    "capacity": 1,
    "family": null,
    "name": "Free_F1",
    "size": "F1",
    "tier": "Free"
  },
  "tags": null,
  "type": "Microsoft.SignalRService/SignalR",
  "version": "1.0"
}

```

## <a name="create-an-event-endpoint"></a>Olay bitiş noktası oluşturma

Bu bölümde, Önceden oluşturulmuş bir örnek web uygulamasını Azure Uygulama Hizmeti'ne dağıtmak için GitHub deposunda bulunan bir Kaynak Yöneticisi şablonu kullanırsınız. Daha sonra, kayıt defterinizin Olay Idamı olaylarına abone olun ve bu uygulamayı olayların gönderildiği bitiş noktası olarak belirtirsiniz.

Örnek uygulamayı dağıtmak için `SITE_NAME` web uygulamanız için benzersiz bir ada ayarlayın ve aşağıdaki komutları uygulayın. Web uygulamasının tam nitelikli alan adının (FQDN) bir parçasını oluşturduğundan, site adı Azure içinde benzersiz olmalıdır. Daha sonraki bir bölümde, kayıt defterinizin olaylarını görüntülemek için bir web tarayıcısında uygulamanın FQDN'sine gidin.

```azurecli-interactive
SITE_NAME=<your-site-name>

az group deployment create \
    --resource-group $RESOURCE_GROUP_NAME \
    --template-uri "https://raw.githubusercontent.com/Azure-Samples/azure-event-grid-viewer/master/azuredeploy.json" \
    --parameters siteName=$SITE_NAME hostingPlanName=$SITE_NAME-plan
```

Dağıtım başarılı olduğunda (birkaç dakika sürebilir), bir tarayıcı açın ve çalışırken olduğundan emin olmak için web uygulamanıza gidin:

`http://<your-site-name>.azurewebsites.net`

[!INCLUDE [event-grid-register-provider-cli.md](../../includes/event-grid-register-provider-cli.md)]

## <a name="subscribe-to-registry-events"></a>Kayıt etkinliklerine abone olun

Olay Izgara'sında, hangi olayları izlemek istediğinizi ve bunları nereye göndereceğinizi söylemek için bir *konuya* abone olursunuz. Aşağıdaki [az eventgrid olay-aboneliği oluşturma][az-eventgrid-event-subscription-create] komutu oluşturduğunuz Azure SignalR Hizmeti'ne abone olur ve web uygulamanızın URL'sini etkinlik göndermesi gereken bitiş noktası olarak belirtir. Önceki bölümlerde dolduran çevre değişkenleri burada yeniden kullanıldığından, hiçbir düzenlemesi gerekmez.

```azurecli-interactive
SIGNALR_SERVICE_ID=$(az signalr show --resource-group $RESOURCE_GROUP_NAME --name $SIGNALR_NAME --query id --output tsv)
APP_ENDPOINT=https://$SITE_NAME.azurewebsites.net/api/updates

az eventgrid event-subscription create \
    --name event-sub-signalr \
    --source-resource-id $SIGNALR_SERVICE_ID \
    --endpoint $APP_ENDPOINT
```

Abonelik tamamlandığında, aşağıdakilere benzer çıktı görmeniz gerekir:

```JSON
{
  "deadLetterDestination": null,
  "destination": {
    "endpointBaseUrl": "https://$SITE_NAME.azurewebsites.net/api/updates",
    "endpointType": "WebHook",
    "endpointUrl": null
  },
  "filter": {
    "includedEventTypes": [
      "Microsoft.SignalRService.ClientConnectionConnected",
      "Microsoft.SignalRService.ClientConnectionDisconnected"
    ],
    "isSubjectCaseSensitive": null,
    "subjectBeginsWith": "",
    "subjectEndsWith": ""
  },
  "id": "/subscriptions/28cf13e2-c598-4aa9-b8c8-098441f0827a/resourceGroups/myResourceGroup/providers/Microsoft.SignalRService/SignalR/SignalRTestSvc/providers/Microsoft.EventGrid/eventSubscriptions/event-sub-signalr",
  "labels": null,
  "name": "event-sub-signalr",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "retryPolicy": {
    "eventTimeToLiveInMinutes": 1440,
    "maxDeliveryAttempts": 30
  },
  "topic": "/subscriptions/28cf13e2-c598-4aa9-b8c8-098441f0827a/resourceGroups/myResourceGroup/providers/microsoft.signalrservice/signalr/SignalRTestSvc",
  "type": "Microsoft.EventGrid/eventSubscriptions"
}
```

## <a name="trigger-registry-events"></a>Kayıt defteri olaylarını tetikleme

SignalR Hizmeti'ne `Serverless Mode` bir istemci bağlantısı kurmak için servis moduna geçin ve kurun. [Serverless Sample'ı](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/Serverless) referans olarak alabilirsiniz.

```bash
git clone git@github.com:aspnet/AzureSignalR-samples.git

cd samples/Management

# Start the negotiation server
# Negotiation server is responsible for generating access token for clients
cd NegotitationServer
dotnet user-secrets set Azure:SignalR:ConnectionString "<Connection String>"
dotnet run

# Use a separate command line
# Start a client
cd SignalRClient
dotnet run
```

## <a name="view-registry-events"></a>Kayıt defteri olaylarını görüntüleme

Şimdi bir istemciyi SignalR Hizmeti'ne bağladınız. Olay Izgara Görüntüleyici web uygulamanıza `ClientConnectionConnected` gidin ve bir olay görmeniz gerekir. İstemciyi sonlandırırsanız, bir `ClientConnectionDisconnected` olay da görürsünüz.

<!-- LINKS - External -->
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[sample-app]: https://github.com/dbarkol/azure-event-grid-viewer

<!-- LINKS - Internal -->
[az-eventgrid-event-subscription-create]: /cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create
[az-group-create]: /cli/azure/group#az-group-create
