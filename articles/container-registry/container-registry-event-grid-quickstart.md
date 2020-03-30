---
title: Quickstart - Olay Izgara'sına etkinlik gönderme
description: Bu hızlı başlatmada, kapsayıcı kayıt defteriniz için Olay Izgara olaylarını etkinleştirirsiniz, ardından kapsayıcı görüntüsü itme ve olayları örnek bir uygulamaya silme gönderirsiniz.
ms.topic: article
ms.date: 08/23/2018
ms.custom: seodec18
ms.openlocfilehash: dbeba56820a520e3435eeb0c5c8dbc5aae981241
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78403231"
---
# <a name="quickstart-send-events-from-private-container-registry-to-event-grid"></a>Hızlı başlatma: Olayları özel kapsayıcı kayıt defterinden Olay Izgarasına gönderme

Azure Olay Ağıt, bir yayımlama-abone etme modelini kullanarak tek tip olay tüketimi sağlayan tam olarak yönetilen bir olay yönlendirme hizmetidir. Bu hızlı başlangıçta, bir kapsayıcı kayıt defteri oluşturmak, kayıt defteri olaylarına abone olmak ve olayları almak için örnek bir web uygulaması dağıtmak için Azure CLI'yi kullanırsınız. Son olarak, kapsayıcı `push` `delete` görüntüsünü ve olaylarını tetikler ve örnek uygulamada olay yükünü görüntülersiniz.

Bu makaledeki adımları tamamladıktan sonra, konteyner kayıt defterinizden Olay Izgara'ya gönderilen olaylar örnek web uygulamasında görünür:

![Alınan üç etkinlikle örnek web uygulamasını işleyen web tarayıcısı][sample-app-01]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz][azure-account] bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu makaledeki Azure CLI komutları **Bash** kabuğu için biçimlendirilir. PowerShell veya Command Prompt gibi farklı bir kabuk kullanıyorsanız, satır devam karakterlerini veya değişken atama satırlarını buna göre ayarlamanız gerekebilir. Bu makalede, gerekli komut düzenleme miktarını en aza indirmek için değişkenler kullanır.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarınızı dağıtıp yönettiğiniz mantıksal bir kapsayıcıdır. Aşağıdaki [az grup oluşturma][az-group-create] komutu *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturur. Kaynak grubunuz için farklı bir ad kullanmak `RESOURCE_GROUP_NAME` istiyorsanız, farklı bir değerayarlayın.

```azurecli-interactive
RESOURCE_GROUP_NAME=myResourceGroup

az group create --name $RESOURCE_GROUP_NAME --location eastus
```

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Ardından, aşağıdaki komutları içeren kaynak grubuna bir kapsayıcı kayıt defteri dağıtın. [Az acr oluşturma][az-acr-create] komutunu çalıştırmadan önce, kayıt defteriniz için bir ada ayarlayın. `ACR_NAME` Ad Azure içinde benzersiz olmalıdır ve 5-50 alfanümerik karakterle sınırlıdır.

```azurecli-interactive
ACR_NAME=<acrName>

az acr create --resource-group $RESOURCE_GROUP_NAME --name $ACR_NAME --sku Basic
```

Kayıt defteri oluşturulduktan sonra, Azure CLI çıktısını aşağıdakilere benzer şekilde döndürür:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2018-08-16T20:02:46.569509+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myregistry",
  "location": "eastus",
  "loginServer": "myregistry.azurecr.io",
  "name": "myregistry",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
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

Dağıtım başarılı olduktan sonra (birkaç dakika sürebilir), bir tarayıcı açın ve çalışırken olduğundan emin olmak için web uygulamanıza gidin:

`http://<your-site-name>.azurewebsites.net`

Olay iletileri görüntülenmeden işlenen örnek uygulamayı görmelisiniz:

![Hiçbir etkinlik görüntülenmeden örnek web uygulamasını gösteren web tarayıcısı][sample-app-02]

[!INCLUDE [event-grid-register-provider-cli.md](../../includes/event-grid-register-provider-cli.md)]

## <a name="subscribe-to-registry-events"></a>Kayıt etkinliklerine abone olun

Olay Izgara'sında, hangi olayları izlemek istediğinizi ve bunları nereye göndereceğinizi söylemek için bir *konuya* abone olursunuz. Aşağıdaki [az eventgrid olay-abonelik oluşturma][az-eventgrid-event-subscription-create] komutu oluşturduğunuz konteyner kayıt defterine abone olur ve web uygulamanızın URL'sini olayları göndermesi gereken bitiş noktası olarak belirtir. Önceki bölümlerde dolduran çevre değişkenleri burada yeniden kullanıldığından, hiçbir düzenlemesi gerekmez.

```azurecli-interactive
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)
APP_ENDPOINT=https://$SITE_NAME.azurewebsites.net/api/updates

az eventgrid event-subscription create \
    --name event-sub-acr \
    --source-resource-id $ACR_REGISTRY_ID \
    --endpoint $APP_ENDPOINT
```

Abonelik tamamlandığında, aşağıdakilere benzer çıktı görmeniz gerekir:

```json
{
  "destination": {
    "endpointBaseUrl": "https://eventgridviewer.azurewebsites.net/api/updates",
    "endpointType": "WebHook",
    "endpointUrl": null
  },
  "filter": {
    "includedEventTypes": [
      "All"
    ],
    "isSubjectCaseSensitive": null,
    "subjectBeginsWith": "",
    "subjectEndsWith": ""
  },
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myregistry/providers/Microsoft.EventGrid/eventSubscriptions/event-sub-acr",
  "labels": null,
  "name": "event-sub-acr",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "topic": "/subscriptions/<Subscription ID>/resourceGroups/myresourcegroup/providers/microsoft.containerregistry/registries/myregistry",
  "type": "Microsoft.EventGrid/eventSubscriptions"
}
```

## <a name="trigger-registry-events"></a>Kayıt defteri olaylarını tetikleme

Artık örnek uygulama çalışır durumda ve Olay Grid ile kayıt defterinize abone olduğunuza göre, bazı etkinlikler oluşturmaya hazırsınız. Bu bölümde, bir kapsayıcı resmi oluşturmak ve kayıt defterinize itmek için ACR Görevleri'ni kullanırsınız. ACR Görevleri, Azure Konteyner Kayıt Defteri'nin yerel makinenize yüklenmiş Docker Engine'e gerek kalmadan bulutta kapsayıcı görüntüleri oluşturmanıza olanak tanıyan bir özelliğidir.

### <a name="build-and-push-image"></a>Görüntü oluşturma ve itme

GitHub deposunun içeriğinden bir kapsayıcı görüntüsü oluşturmak için aşağıdaki Azure CLI komutunu uygulayın. Varsayılan olarak, ACR Görevleri başarıyla oluşturulmuş bir görüntüyü kayıt defterinize `ImagePushed` otomatik olarak iter ve bu da olayı oluşturur.

```azurecli-interactive
az acr build --registry $ACR_NAME --image myimage:v1 -f Dockerfile https://github.com/Azure-Samples/acr-build-helloworld-node.git
```

ACR Görevleri oluşturur ken ve sonra görüntüitiniter zaman aşağıdakine benzer çıktı görmelisiniz. Aşağıdaki örnek çıktı kısalık için kesildi.

```output
Sending build context to ACR...
Queued a build with build ID: aa2
Waiting for build agent...
2018/08/16 22:19:38 Using acb_vol_27a2afa6-27dc-4ae4-9e52-6d6c8b7455b2 as the home volume
2018/08/16 22:19:38 Setting up Docker configuration...
2018/08/16 22:19:39 Successfully set up Docker configuration
2018/08/16 22:19:39 Logging in to registry: myregistry.azurecr.io
2018/08/16 22:19:55 Successfully logged in
Sending build context to Docker daemon  94.72kB
Step 1/5 : FROM node:9-alpine
...
```

Yerleşik görüntünün kayıt defterinizde olduğunu doğrulamak için, "myimage" deposundaki etiketleri görüntülemek için aşağıdaki komutu uygulayın:

```azurecli-interactive
az acr repository show-tags --name $ACR_NAME --repository myimage
```

Oluşturduğun görüntünün "v1" etiketi, aşağıdakilere benzer şekilde çıktıda görünmelidir:

```output
[
  "v1"
]
```

### <a name="delete-the-image"></a>Resmi silme

Şimdi, az `ImageDeleted` [acr deposu silme][az-acr-repository-delete] komutu ile görüntüyü silerek bir olay oluşturun:

```azurecli-interactive
az acr repository delete --name $ACR_NAME --image myimage:v1
```

Aşağıdakilere benzer çıktılar görmelisiniz, manifesto ve ilişkili görüntüleri silmek için onay istemeniz gerekir:

```output
This operation will delete the manifest 'sha256:f15fa9d0a69081ba93eee308b0e475a54fac9c682196721e294b2bc20ab23a1b' and all the following images: 'myimage:v1'.
Are you sure you want to continue? (y/n): 
```

## <a name="view-registry-events"></a>Kayıt defteri olaylarını görüntüleme

Şimdi bir resmi kayıt defterinize itmiş ve sonra silmişsiniz. Olay Izgara Görüntüleyici web uygulamanıza `ImageDeleted` gidin `ImagePushed` ve hem olayları hem de olayları görmeniz gerekir. Ayrıca, [kayıt defteri olaylarına abone](#subscribe-to-registry-events) ol bölümündeki komutu çalıştırarak oluşturulan bir abonelik doğrulama olayı da görebilirsiniz.

Aşağıdaki ekran görüntüsü, üç olayla birlikte örnek `ImageDeleted` uygulamayı gösterir ve olay ayrıntılarını göstermek üzere genişletilir.

![ImagePushed ve ImageDeleted olayları ile örnek uygulamayı gösteren web tarayıcısı][sample-app-03]

Tebrikler! Olayları `ImagePushed` ve `ImageDeleted` olayları görürseniz, kayıt defteriniz Olayları Olay Izgara'sına gönderiyor ve Olay Grid bu olayları web uygulama bitiş noktanıza gönderiyor.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta oluşturduğunuz kaynaklarla işimiz bittiğinde, aşağıdaki Azure CLI komutuyla bunların hepsini silebilirsiniz. Bir kaynak grubunu sildiğinizde, içerdiği tüm kaynaklar kalıcı olarak silinir.

**UYARI**: Bu işlem geri alınamaz. Komutu çalıştırmadan önce gruptaki kaynaklardan hiçbirine artık ihtiyacınız olmadığından emin olun.

```azurecli-interactive
az group delete --name $RESOURCE_GROUP_NAME
```

## <a name="event-grid-event-schema"></a>Olay Izgara olay şeması

Olay Kılavuzu belgelerinde Azure Kapsayıcı Kayıt Defteri olay iletisi şema referansını bulabilirsiniz:

[Konteyner Kayıt Defteri için Azure Olay Izgara olay şeması](../event-grid/event-schema-container-registry.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir kapsayıcı kayıt defteri dağıttınız, ACR Görevleri içeren bir resim oluşturmuşsunuz, sildiniz ve örnek bir uygulamayla Olay Grid'inizdeki kayıt defterinizin olaylarını tüketmişsiniz. Ardından, temel görüntü güncelleştirmesinde otomatik yapılar da dahil olmak üzere bulutta kapsayıcı görüntüleri oluşturma hakkında daha fazla bilgi edinmek için ACR Görevleri öğreticisine geçin:

> [!div class="nextstepaction"]
> [ACR Görevleri ile bulutta kapsayıcı görüntüleri oluşturun](container-registry-tutorial-quick-task.md)

<!-- IMAGES -->
[sample-app-01]: ./media/container-registry-event-grid-quickstart/sample-app-01.png
[sample-app-02]: ./media/container-registry-event-grid-quickstart/sample-app-02-no-events.png
[sample-app-03]: ./media/container-registry-event-grid-quickstart/sample-app-03-with-events.png

<!-- LINKS - External -->
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[sample-app]: https://github.com/dbarkol/azure-event-grid-viewer

<!-- LINKS - Internal -->
[az-acr-create]: /cli/azure/acr/repository
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-eventgrid-event-subscription-create]: /cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create
[az-group-create]: /cli/azure/group#az-group-create
