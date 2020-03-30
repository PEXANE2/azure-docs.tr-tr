---
title: Quickstart - Docker kapsayıcısını konteyner örneğine dağıtma - Azure CLI
description: Bu hızlı başlatmada, yalıtılmış bir Azure kapsayıcı örneğinde çalışan kapsayıcı laştırılmış bir web uygulamasını hızla dağıtmak için Azure CLI'yi kullanırsınız
ms.topic: quickstart
ms.date: 03/21/2019
ms.custom:
- seo-python-october2019
- seodec18
- mvc
ms.openlocfilehash: e5cad7d9141963e5062423545f7e5b94f0575152
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78252195"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-cli"></a>Hızlı başlatma: Azure CLI'yi kullanarak Azure'da bir kapsayıcı örneğini dağıtma

Azure'da sunucusuz Docker kapsayıcılarını basitlik ve hızla çalıştırmak için Azure Kapsayıcı Örnekleri'ni kullanın. Azure Kubernetes Hizmeti gibi tam bir kapsayıcı düzenleme platformuna ihtiyacınız olmadığında uygulamayı isteğe bağlı bir kapsayıcı örneğine dağıtın.

Bu hızlı başlangıçta, yalıtılmış bir Docker kapsayıcısı dağıtmak ve uygulamasını tam nitelikli bir etki alanı adı (FQDN) ile kullanılabilir hale getirmek için Azure CLI'yi kullanırsınız. Tek bir dağıtım komutunu çalıştırdıktan birkaç saniye sonra, kapsayıcıda çalışan uygulamaya göz atabilirsiniz:

![Tarayıcıda Azure Kapsayıcı Örnekleri'ne dağıtılan bir uygulamayı görüntüleme][aci-app-browser]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz][azure-account] bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıcı tamamlamak için Azure Cloud Shell veya yerel bir Azure CLI yüklemesi kullanabilirsiniz. Yerel olarak kullanmak isterseniz, sürüm 2.0.55 veya daha sonra önerilir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Tüm Azure kaynakları gibi Azure kapsayıcı örneklerinin de bir kaynak grubuna dağıtılması gerekir. Kaynak grupları, ilgili Azure kaynaklarını düzenlemenizi ve yönetmenizi sağlar.

İlk olarak aşağıdaki [az group create][az-group-create] komutunu kullanarak *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Artık bir kaynak grubuna sahip olduğunuza göre Azure'da kapsayıcı çalıştırabilirsiniz. Azure CLI ile kapsayıcı örneği oluşturmak için [az container create][az-container-create] komutunda bir kaynak grubu adı, kapsayıcı örneği adı ve Docker kapsayıcı görüntüsü belirtin. Bu hızlı başlangıçta, genel `mcr.microsoft.com/azuredocs/aci-helloworld` görüntüyü kullanırsınız. Bu resim, statik bir HTML sayfası sunan Düğüm.js ile yazılmış küçük bir web uygulamasını paketler.

Açılacak bir veya daha fazla bağlantı noktası, DNS ad etiketi ya da ikisini birden belirterek kapsayıcılarınızı internete açabilirsiniz. Bu hızlı başlangıçta, web uygulamasına genel olarak erişilebilen bir DNS ad etiketi içeren bir kapsayıcı dağıtırsınız.

Kapsayıcı örneğini başlatmak için aşağıdakine benzer bir komut uygulayın. Örneği `--dns-name-label` oluşturduğunuz Azure bölgesinde benzersiz bir değer ayarlayın. "DNS ad etiketi kullanılamıyor" hata iletisiyle karşılaşırsanız farklı bir DNS ad etiketi deneyin.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image mcr.microsoft.com/azuredocs/aci-helloworld --dns-name-label aci-demo --ports 80
```

Birkaç saniye içinde Azure CLI'den dağıtımın tamamlandığını belirten bir yanıt almanız gerekir. Durumunu [az container show][az-container-show] komutuyla denetleyebilirsiniz:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

Komutu çalıştırdığınızda, kapsayıcının tam etki alanı adı (FQDN) ve sağlama durumu görüntülenir.

```output
FQDN                               ProvisioningState
---------------------------------  -------------------
aci-demo.eastus.azurecontainer.io  Succeeded
```

Kapsayıcı başarılı `ProvisioningState` **ise,** tarayıcınızda fqdn gidin. Aşağıdakine benzer bir web sayfası görüyorsanız kendinizi tebrik edebilirsiniz! Docker kapsayıcısında çalışan bir uygulamayı başarıyla Azure'a dağıttınız.

![Tarayıcıda Azure Kapsayıcı Örnekleri'ne dağıtılan bir uygulamayı görüntüleme][aci-app-browser]

İlk seferde uygulama görüntülenmezse DNS kayıtlarının yayılması için birkaç saniye bekleyip tarayıcınızı yenilemeyi deneyebilirsiniz.

## <a name="pull-the-container-logs"></a>Kapsayıcı günlüklerini çekme

Kapsayıcıdaki veya üzerinde çalışan uygulamalardaki sorunları gidermek (veya yalnızca çıkışını görmek) istediğinizde kapsayıcı örneğinin günlüklerinden başlayın.

[az container logs][az-container-logs] komutu ile kapsayıcı örneğinin günlüklerini çekin:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Çıkış, kapsayıcının günlüklerini görüntüler ve uygulamayı tarayıcınızda görüntülediğinizde oluşturulan HTTP GET isteklerini göstermelidir.

```output
listening on port 80
::ffff:10.240.255.55 - - [21/Mar/2019:17:43:53 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [21/Mar/2019:17:44:36 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [21/Mar/2019:17:44:36 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
```

## <a name="attach-output-streams"></a>Çıkış akışları ekleme

Günlükleri görüntülemeye ek olarak, yerel standart çıkış ve standart hata akışlarınızı kapsayıcınınkine ekleyebilirsiniz.

İlk olarak, yerel konsolunuzu kapsayıcının çıkış akışlarına eklemek için [az kapsayıcı ekleme][az-container-attach] komutunu uygulayın:

```azurecli-interactive
az container attach --resource-group myResourceGroup --name mycontainer
```

Bağlandıktan sonra, ek çıkışlar oluşturmak için tarayıcınızı birkaç defa yenileyin. İşlemi tamamladığınızda `Control+C` ile konsolunuzu ayırın. Aşağıdakine benzer bir çıktı görmeniz gerekir:

```output
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2019-03-21 17:27:20+00:00) pulling image "mcr.microsoft.com/azuredocs/aci-helloworld"
(count: 1) (last timestamp: 2019-03-21 17:27:24+00:00) Successfully pulled image "mcr.microsoft.com/azuredocs/aci-helloworld"
(count: 1) (last timestamp: 2019-03-21 17:27:27+00:00) Created container
(count: 1) (last timestamp: 2019-03-21 17:27:27+00:00) Started container

Start streaming logs:
listening on port 80

::ffff:10.240.255.55 - - [21/Mar/2019:17:43:53 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [21/Mar/2019:17:44:36 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [21/Mar/2019:17:44:36 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [21/Mar/2019:17:47:01 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.56 - - [21/Mar/2019:17:47:12 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcıyla işiniz bittiğinde [az container delete][az-container-delete] komutunu kullanarak kapsayıcıyı kaldırın:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Kapsayıcının silindiğini doğrulamak için, [az container list](/cli/azure/container#az-container-list) komutunu yürütün:

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

**mycontainer** kapsayıcısı komut çıkışında görünmemelidir. Kaynak grubunda başka kapsayıcınız yoksa, çıkış görüntülenmez.

*myResourceGroup* kaynak grubuyla ve içindeki kaynaklarla işiniz bittiyse [az group delete][az-group-delete] komutuyla silin:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, herkese açık bir Microsoft resmi kullanarak bir Azure kapsayıcı örneği oluşturdunuz. Kapsayıcı görüntüsünü oluşturup özel bir Azure kapsayıcı kayıt defterinden dağıtmak istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi](./container-instances-tutorial-prepare-app.md)

Azure'da bir düzenleme sisteminde kapsayıcıları çalıştırma seçeneklerini denemek için [Azure Kubernetes Hizmeti 'ne (AKS)][container-service] hızlı başlangıçlar bakın.

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/view-an-application-running-in-an-azure-container-instance.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/
[node-js]: https://nodejs.org

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-create]: /cli/azure/container#az-container-create
[az-container-delete]: /cli/azure/container#az-container-delete
[az-container-list]: /cli/azure/container#az-container-list
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
