---
title: Azure Uygulama Yapılandırmasını kullanarak Etkinlikleri web bitiş noktasına gönderme
description: Anahtar değeri değişiklik olaylarını bir web bitiş noktasına göndermek için Azure Uygulama Yapılandırma etkinlik aboneliklerini kullanmayı öğrenin
services: azure-app-configuration
author: lisaguthrie
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: how-to
ms.date: 02/25/2020
ms.author: lcozzens
ms.openlocfilehash: da64f22981cc33772783093cfe75daa3eac5cef1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672150"
---
# <a name="route-azure-app-configuration-events-to-a-web-endpoint-with-azure-cli"></a>Azure Uygulama Yapılandırma olaylarını Azure CLI ile bir web bitiş noktasına yönlendirin

Bu makalede, anahtar değeri değişiklik olaylarını bir web bitiş noktasına göndermek için Azure Uygulama Yapılandırması etkinlik aboneliklerini nasıl ayarlayabileceğinizi öğreneceksiniz. Azure Uygulama Yapılandırması kullanıcıları, anahtar değerler değiştirildiğinde yayılan olaylara abone olabilir. Bu olaylar web kancalarını, Azure Işlevlerini, Azure Depolama Kuyruklarını veya Azure Olay Sgarası tarafından desteklenen diğer olay işleyicilerini tetikleyebilir. Normalde olayları, olay verilerini işleyen ve eylemler gerçekleştiren bir uç noktaya gönderirsiniz. Bununla birlikte, bu makaleyi basitleştirmek için olayları iletilerin toplandığı ve görüntülendiği bir web uygulamasına gönderirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun.](https://azure.microsoft.com/free/) Azure Bulut Kabuğu'nu isteğe bağlı olarak kullanabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure CLI'nin en son sürümünü (2.0.70 veya sonraki) çalıştırdığınız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

Cloud Shell kullanmıyorsanız önce `az login` kullanarak oturum açmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Event Grid konuları Azure kaynaklarıdır ve bir Azure kaynak grubuna yerleştirilmelidir. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. 

Aşağıdaki örnek, *westus* konumunda `<resource_group_name>` adı verilen bir kaynak grubu oluşturur.  `<resource_group_name>` değerini kaynak grubunuz için benzersiz bir adla değiştirin.

```azurecli-interactive
az group create --name <resource_group_name> --location westus
```

## <a name="create-an-app-configuration-store"></a>Uygulama Yapılandırma mağazası oluşturma

Yapılandırma `<appconfig_name>` mağazanız için benzersiz bir `<resource_group_name>` adla ve daha önce oluşturduğunuz kaynak grubuyla değiştirin. DNS adı olarak kullanıldığından ad benzersiz olmalıdır.

```azurecli-interactive
az appconfig create \
  --name <appconfig_name> \
  --location westus \
  --resource-group <resource_group_name> \
  --sku free
```

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Konuya abone olmadan önce olay iletisi için uç noktayı oluşturalım. Normalde, olay verileri temelinde uç nokta eylemleri gerçekleştirir. Bu hızlı başlangıcı basitleştirmek için, olay iletilerini görüntüleyin bir [önceden oluşturulmuş web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtırsınız. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

`<your-site-name>` değerini web uygulamanız için benzersiz bir adla değiştirin. Web uygulaması adı bir DNS girdisinin parçası olduğundan benzersiz olmalıdır.

```azurecli-interactive
$sitename=<your-site-name>

az group deployment create \
  --resource-group <resource_group_name> \
  --template-uri "https://raw.githubusercontent.com/Azure-Samples/azure-event-grid-viewer/master/azuredeploy.json" \
  --parameters siteName=$sitename hostingPlanName=viewerhost
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

Şu anda iletilerin görüntülenmediği siteyi görüyor olmalısınız.

[!INCLUDE [event-grid-register-provider-cli.md](../../includes/event-grid-register-provider-cli.md)]

## <a name="subscribe-to-your-app-configuration-store"></a>Uygulama Yapılandırma mağazanıza abone olun

Event Grid’e hangi olayları izlemek istediğinizi ve bu olayların nereye gönderileceğini bildirmek için bir konuya abone olursunuz. Aşağıdaki örnek, oluşturduğunuz Uygulama Yapılandırmasına abone dir ve web uygulamanızdaki URL'yi olay bildiriminin bitiş noktası olarak geçirir. Olay aboneliğiniz için `<event_subscription_name>` öğesini bir ad ile değiştirin. `<resource_group_name>` ve `<appconfig_name>` için daha önce oluşturduğunuz değerleri kullanın.

Web uygulamanızın uç noktası `/api/updates/` sonekini içermelidir.

```azurecli-interactive
appconfigId=$(az appconfig show --name <appconfig_name> --resource-group <resource_group_name> --query id --output tsv)
endpoint=https://$sitename.azurewebsites.net/api/updates

az eventgrid event-subscription create \
  --resource-id $appconfigId \
  --name <event_subscription_name> \
  --endpoint $endpoint
```

Web uygulamanızı yeniden görüntüleyin ve buna bir abonelik doğrulama olayının gönderildiğine dikkat edin. Göz simgesini seçerek olay verilerini genişletin. Uç noktanın olay verilerini almak istediğini doğrulayabilmesi için Event Grid doğrulama olayını gönderir. Web uygulaması aboneliği doğrulamak için kod içerir.

![Abonelik olayını görüntüleme](./media/quickstarts/event-grid/view-subscription-event.png)

## <a name="trigger-an-app-configuration-event"></a>Uygulama Yapılandırma olayını tetikleme

Şimdi, Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim. `<appconfig_name>` Öncekinden'i kullanarak bir anahtar değeri oluşturun.

```azurecli-interactive
az appconfig kv set --name <appconfig_name> --key Foo --value Bar --yes
```

Olayı tetiklediniz ve Event Grid, iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Az önce gönderdiğiniz olayı görmek için web uygulamanızı görüntüleyin.

```json
[{
  "id": "deb8e00d-8c64-4b6e-9cab-282259c7674f",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/eventDemoGroup/providers/microsoft.appconfiguration/configurationstores/{appconfig-name}",
  "subject": "https://{appconfig-name}.azconfig.io/kv/Foo",
  "data": {
    "key": "Foo",
    "etag": "a1LIDdNEIV6wCnfv3xaip7fMXD3"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T18:59:54Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu Uygulama Yapılandırması ve olay aboneliğiyle çalışmaya devam etmeyi planlıyorsanız, bu makalede oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, aşağıdaki komutu kullanarak bu makalede oluşturduğunuz kaynakları silin.

`<resource_group_name>` değerini yukarıda oluşturduğunuz kaynak grubuyla değiştirin.

```azurecli-interactive
az group delete --name <resource_group_name>
```

## <a name="next-steps"></a>Sonraki adımlar

Artık konuları ve etkinlik aboneliklerini nasıl oluşturacağınız konusunda bildiğinize göre, anahtar değerli olaylar ve Etkinlik Izgarası'nın yapmanıza hangileri yardımcı olabileceği hakkında daha fazla bilgi edinin:

- [Anahtar Değer Olaylarına Tepki](concept-app-configuration-event.md)
- [Event Grid Hakkında](../event-grid/overview.md)
- [Azure Olay Izgara işleyicileri](../event-grid/event-handlers.md)
