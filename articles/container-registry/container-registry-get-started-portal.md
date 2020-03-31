---
title: Quickstart - Portalda kayıt defteri oluşturma
description: Azure portalı ile Azure Konteyner Kayıt Defteri'nde özel bir Docker kayıt defteri oluşturmayı hızlı bir şekilde öğrenin.
ms.topic: quickstart
ms.date: 03/03/2020
ms.custom: seodec18, mvc
ms.openlocfilehash: 6fe6358655f50ab783b4017efa8ee1db351cd018
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79409312"
---
# <a name="quickstart-create-a-private-container-registry-using-the-azure-portal"></a>Hızlı başlatma: Azure portalını kullanarak özel bir kapsayıcı kayıt defteri oluşturma

Azure kapsayıcı kayıt defteri, Azure'da özel Docker kapsayıcı resimlerini ve ilgili yapıları depolayıp yönetebileceğiniz özel bir Docker kayıt defteridir. Bu hızlı başlangıçta, Azure portalıyla bir kapsayıcı kayıt defteri oluşturursunuz. Ardından, bir kapsayıcı görüntüsünü kayıt defterine itmek için Docker komutlarını kullanın ve son olarak resmi kayıt defterinizden çekip çalıştırın.

Kapsayıcı görüntüleriyle çalışmak için kayıt defterine giriş yapmak için bu hızlı başlangıç, Azure CLI'yi (sürüm 2.0.55 veya daha sonra önerilir) çalıştırmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

Ayrıca sisteminizde yerel olarak Docker yüklü olması gerekir. Docker [Mac][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Kaynak > **Kapsayıcılar** > **Kayıt Defteri** **Oluştur'u**seçin.

![Azure portalında kapsayıcı kayıt defteri oluşturma][qs-portal-01]

Temel **bilgiler** sekmesine Kaynak **grubu** ve **Kayıt Defteri adı**için değerler girin. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Bu hızlı başlangıçta `West US` konumunda `myResourceGroup` adlı yeni bir kaynak grubu oluşturun ve **SKU** olarak ‘Temel’ seçeneğini belirleyin. 

![Azure portalında kapsayıcı kayıt defteri oluşturma][qs-portal-03]

Kalan ayarlar için varsayılan değerleri kabul edin. Sonra **Gözden Geçir + oluştur'u**seçin. Ayarları inceledikten sonra **Oluştur'u**seçin.

Bu hızlı başlangıçta, Azure Konteyner Kayıt Defteri'ni öğrenen geliştiriciler için maliyet için en uygun seçenek olan *bir Temel* kayıt defteri oluşturursunuz. Kullanılabilir hizmet katmanları hakkında ayrıntılı bilgi için [Konteyner kayıt defteri SUS'larına][container-registry-skus]bakın.

Dağıtım **başarılı** iletisi göründüğünde, portaldaki kapsayıcı kayıt defterini seçin. 

![Azure portalında Konteyner kayıt defterine genel bakış][qs-portal-05]

**Giriş sunucusunun**değerine dikkat edin. Docker ile görüntüleri itip çektiğinde bu değeri aşağıdaki adımlarda kullanırsınız.

## <a name="log-in-to-registry"></a>Kayıt defterinde oturum açma

Kapsayıcı görüntülerini gönderip çekmeden önce ACR örneğinde oturum açmalısınız. İşletim sisteminizde bir komut kabuğu açın ve Azure CLI'deki [az acr giriş][az-acr-login] komutunu kullanın. (Oturum açarken yalnızca kayıt defteri adını belirtin. 'azurecr.io' sonekieklemesini eklemeyin.)

```azurecli
az acr login --name <acrName>
```

Bu komut tamamlandığında `Login Succeeded` döndürülür. 

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

## <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

Kayıt defterinizdeki resimleri listelemek için, portaldaki kayıt defterinize gidin ve **Depoları**seçin, ardından oluşturduğunuz depoyu `docker push`seçin.

Bu örnekte, **merhaba dünyası** deposunu seçiyoruz ve Etiketler `v1`altında etiketli **Tags**resmi görebiliyoruz.

![Azure portalında kapsayıcı görüntülerini listele][qs-portal-09]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynaklarınızı temizlemek için portaldaki **myResourceGroup** kaynak grubuna gidin. Kaynak grubu yüklendikten sonra kaynak **grubunu,** kapsayıcı kayıt defterini ve depolanan kapsayıcı görüntülerini kaldırmak için kaynak grubunu sil'e tıklayın.

![Azure portalında kaynak grubunu silme][qs-portal-08]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure portalı ile bir Azure Kapsayıcı Kayıt Defteri oluşturdunuz, bir kapsayıcı görüntüsünü ittiniz ve resmi kayıt defterinden çekip çalıştırdın. ACR'ye daha derinlemesine bakmak için Azure Konteyner Kayıt Defteri öğreticilerine devam edin.

> [!div class="nextstepaction"]
> [Azure Konteyner Kayıt Defteri öğreticileri][container-registry-tutorial-quick-task]

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
