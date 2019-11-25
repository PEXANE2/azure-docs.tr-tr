---
title: Quickstart - Create registry in portal
description: Quickly learn to create a private Docker registry in Azure Container Registry with the Azure portal.
ms.topic: quickstart
ms.date: 01/22/2019
ms.custom: seodec18, mvc
ms.openlocfilehash: c5e6de0fdb7d0e46fa9be28483fd894289baf1a3
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74455227"
---
# <a name="quickstart-create-a-private-container-registry-using-the-azure-portal"></a>Quickstart: Create a private container registry using the Azure portal

Azure kapsayıcı kayıt defteri, Azure’da özel Docker kapsayıcısı görüntülerinizi depolayıp yönetebileceğiniz özel bir Docker kayıt defteridir. Bu hızlı başlangıçta, Azure portalıyla bir kapsayıcı kayıt defteri oluşturursunuz. Then, use Docker commands to push a container image into the registry, and finally pull and run the image from your registry.

To log in to the registry to work with container images, this quickstart requires that you are running the Azure CLI (version 2.0.55 or later recommended). Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli].

Ayrıca sisteminizde yerel olarak Docker yüklü olması gerekir. Docker, [Mac][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

**Kaynak oluştur** > **Kapsayıcılar** > **Container Registry**'yi seçin.

![Azure portalında kapsayıcı kayıt defteri oluşturma][qs-portal-01]

**Kaynak Defteri Adı** ve **Kaynak grubu** değerlerini girin. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Bu hızlı başlangıçta `West US` konumunda `myResourceGroup` adlı yeni bir kaynak grubu oluşturun ve **SKU** olarak ‘Temel’ seçeneğini belirleyin. **Oluştur**’u seçerek ACR örneğini dağıtın.

![Create container registry in the Azure portal][qs-portal-03]

In this quickstart you create a *Basic* registry, which is a cost-optimized option for developers learning about Azure Container Registry. For details on available service tiers, see [Container registry SKUs][container-registry-skus].

When the **Deployment succeeded** message appears, select the container registry in the portal. 

![Container registry Overview in the Azure portal][qs-portal-05]

Take note of the value of the **Login server**. You use this value in the following steps while working with your registry with the Azure CLI and Docker.

## <a name="log-in-to-registry"></a>Kayıt defterinde oturum açma

Kapsayıcı görüntülerini gönderip çekmeden önce ACR örneğinde oturum açmalısınız. Open a command shell in your operating system, and use the [az acr login][az-acr-login] command in the Azure CLI.

```azurecli
az acr login --name <acrName>
```

Bu komut tamamlandığında `Login Succeeded` döndürülür. 

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

## <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

To list the images in your registry, navigate to your registry in the portal and select **Repositories**, then select the repository you created with `docker push`.

In this example, we select the **hello-world** repository, and we can see the `v1`-tagged image under **TAGS**.

![List container images in the Azure portal][qs-portal-09]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

To clean up your resources, navigate to the **myResourceGroup** resource group in the portal. Once the resource group is loaded click on **Delete resource group** to remove the resource group, the container registry, and the container images stored there.

![Azure portalında kaynak grubunu silme][qs-portal-08]

## <a name="next-steps"></a>Sonraki adımlar

In this quickstart, you created an Azure Container Registry with the Azure portal, pushed a container image, and pulled and ran the image from the registry. Continue to the Azure Container Registry tutorials for a deeper look at ACR.

> [!div class="nextstepaction"]
> [Azure Container Registry tutorials][container-registry-tutorial-quick-task]

<!-- IMAGES -->
[qs-portal-01]: ./media/container-registry-get-started-portal/qs-portal-01.png
[qs-portal-02]: ./media/container-registry-get-started-portal/qs-portal-02.png
[qs-portal-03]: ./media/container-registry-get-started-portal/qs-portal-03.png
[qs-portal-05]: ./media/container-registry-get-started-portal/qs-portal-05.png
[qs-portal-08]: ./media/container-registry-get-started-portal/qs-portal-08.png
[qs-portal-09]: ./media/container-registry-get-started-portal/qs-portal-09.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-login]: /cli/azure/acr#az-acr-login
