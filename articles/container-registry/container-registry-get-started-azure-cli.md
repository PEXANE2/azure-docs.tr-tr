---
title: Hızlı başlangıç-kayıt defteri oluşturma-Azure CLı
description: Azure CLI ile hızlıca özel bir Docker kapsayıcısı kayıt defteri oluşturmayı öğrenin.
ms.topic: quickstart
ms.date: 06/12/2020
ms.custom: seodec18, H1Hack27Feb2017, mvc
ms.openlocfilehash: 31e917fa306330ca579266e21560d7d42c7f2bc7
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84752458"
---
# <a name="quickstart-create-a-private-container-registry-using-the-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak özel kapsayıcı kayıt defteri oluşturma

Azure Container Registry, özel Docker kapsayıcı görüntülerini depolamak için kullanılan bir yönetilen Docker kapsayıcı kayıt defteridir. Bu kılavuzda, Azure CLI kullanarak bir Azure Container Registry örneği oluşturma hakkındaki ayrıntılar yer alır. Ardından, Docker komutlarını kullanarak kayıt defterine bir kapsayıcı görüntüsü gönderin ve son olarak görüntüyü Kayıt defterinizden çekin ve çalıştırın.

Bu hızlı başlangıç, Azure CLı 'yi (sürüm 2.0.55 veya üzeri önerilir) çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

Ayrıca sisteminizde yerel olarak Docker yüklü olması gerekir. Docker [macOS][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure Cloud Shell gerekli tüm Docker bileşenlerini (`dockerd` daemon) içermediğinden, bu hızlı başlangıçta Cloud Shell’i kullanamazsınız.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bu hızlı başlangıçta, Azure Container Registry hakkında bilgi edinmek için uygun maliyetli bir seçenek olan *temel* bir kayıt defteri oluşturursunuz. Kullanılabilir hizmet katmanları hakkında daha fazla bilgi için bkz. [kapsayıcı kayıt defteri hizmet katmanları][container-registry-skus].

[az act create][az-acr-create] komutunu kullanarak bir ACR örneği oluşturun. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Aşağıdaki örnekte *myContainerRegistry007* komutu kullanılmıştır. Bunu benzersiz bir değerle güncelleştirin.

```azurecli
az acr create --resource-group myResourceGroup \
  --name myContainerRegistry007 --sku Basic
```

Kayıt defteri oluşturulduğunda çıkış aşağıdakilere benzer:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2019-01-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "mycontainerregistry007.azurecr.io",
  "name": "myContainerRegistry007",
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

`loginServer`Tam kayıt defteri adı (tümü küçük harf) olan çıktıda konusuna göz atın. Bu hızlı başlangıç boyunca, `<registry-name>` kapsayıcı kayıt defteri adı için bir yer tutucudur ve `<login-server>` kayıt defterinin oturum açma sunucusu adı için bir yer tutucudur.

## <a name="log-in-to-registry"></a>Kayıt defterinde oturum açma

Kapsayıcı görüntülerini göndermeden ve çekmeden önce, kayıt defterinde oturum açmalısınız. Bunu yapmak için [az acr login][az-acr-login] komutunu kullanın.

```azurecli
az acr login --name <registry-name>
```

Bu komut tamamlandığında `Login Succeeded` iletisi döndürülür.

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

## <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

Aşağıdaki örnek, kayıt defterinizde bulunan depoları listeler:

```azurecli
az acr repository list --name <registry-name> --output table
```

Çıktı:

```
Result
----------------
hello-world
```

Aşağıdaki örnekte, **Merhaba-Dünya** deposundaki Etiketler listelenmektedir.

```azurecli
az acr repository show-tags --name <registry-name> --repository hello-world --output table
```

Çıktı:

```
Result
--------
v1
```

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az Group Delete][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı kayıt defterini ve orada depolanan kapsayıcı görüntülerini kaldırabilirsiniz.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure CLı ile bir Azure Container Registry oluşturdunuz, kayıt defterine bir kapsayıcı görüntüsü gönderdi ve görüntüyü kayıt defterinden çekmiş ve çalıştırdınız. ACR 'ye daha ayrıntılı bir bakış için Azure Container Registry öğreticilerine geçin.

> [!div class="nextstepaction"]
> [Azure Container Registry öğreticileri][container-registry-tutorial-prepare-registry]

> [!div class="nextstepaction"]
> [Azure Container Registry görevleri öğreticileri][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli]: /cli/azure/install-azure-cli
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[container-registry-tutorial-prepare-registry]: container-registry-tutorial-prepare-registry.md