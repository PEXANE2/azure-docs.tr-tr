---
title: Portaldaki depoları görüntüleme
description: Docker kapsayıcı resimlerini ve desteklenen diğer yapıları barındıran Azure Konteyner Kayıt Defteri depolarını görüntülemek için Azure portalını kullanın.
ms.topic: article
ms.date: 01/05/2018
ms.openlocfilehash: 1da72706d2554610a685f71199ab14af5e30ce1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74456285"
---
# <a name="view-container-registry-repositories-in-the-azure-portal"></a>Azure portalında konteyner kayıt defteri depolarını görüntüleme

Azure Konteyner Kayıt Defteri, Docker kapsayıcı görüntülerini depolarda depolamanıza olanak tanır. Görüntüleri depolarda depolayarak, görüntü gruplarını (veya görüntülerin sürümlerini) yalıtılmış ortamlarda depolayabilirsiniz. Resimleri kayıt defterinize iterken bu depoları belirtebilir ve azure portalında içeriğini görüntüleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* **Konteyner kayıt defteri**: Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturun. Örneğin, Azure [portalını](container-registry-get-started-portal.md) veya [Azure CLI'yi](container-registry-get-started-azure-cli.md)kullanın.
* **Docker CLI**: Docker komut satırı arabirimini sağlayan yerel makinenize [Docker'ı][docker-install] yükleyin.
* **Konteyner görüntüsü**: Görüntüyü konteyner kayıt defterinize doğru itin. Görüntüleri nasıl itecek ve çekileceklerine ilişkin kılavuz için görüntüyü [it ve çek'](container-registry-get-started-docker-cli.md)e bakın.

## <a name="view-repositories-in-azure-portal"></a>Azure portalındaki depoları görüntüleme

Azure portalında resimlerinizi barındıran depoların ve resim etiketlerinin listesini görebilirsiniz.

[Push'daki](container-registry-get-started-docker-cli.md) adımları izleyip bir görüntü çektiyseniz (ve daha sonra görüntüyü silerseniz), konteyner kayıt defterinizde bir Nginx resmi olmalıdır. Bu makaledeki yönergelerde, görüntüyü bir ad alanıyla etiketlemeniz `/samples/nginx`belirtilirken, "örnekler" Bir ferahlatıcı olarak, bu makalede belirtilen [docker push][docker-push] komutu:

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

 Azure Kapsayıcı Kayıt Defteri bu tür çok düzeyli depo ad alanlarını desteklediğinden, belirli bir uygulamayla veya uygulama koleksiyonuyla ilgili resim koleksiyonlarını farklı geliştirme veya operasyonel ekiplere kapsamız. Konteyner kayıt defterlerinde bulunan depolar hakkında daha fazla bilgi için [Azure'daki Private Docker konteyner kayıt defterlerine](container-registry-intro.md)bakın.

Bir depogörüntülemek için:

1. [Azure portalında][portal] oturum açın
1. Nginx görüntüsünü ittiğiniz **Azure Kapsayıcı Kayıt Defteri'ni** seçin
1. Kayıt defterindeki görüntüleri içeren depoların listesini görmek için **Depoları** seçin
1. Bu depodaki resim etiketlerini görmek için bir depo seçin

Örneğin, Nginx görüntüsünü [Push ve pull'da](container-registry-get-started-docker-cli.md)belirtildiği gibi ittiyseniz, aşağıdakilere benzer bir şey görmeniz gerekir:

![Portaldaki depolar](./media/container-registry-repositories/container-registry-repositories.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık portaldaki depoları görüntülemenin ve depolarla çalışmanın temellerini bildiğinize göre, [Azure Kubernetes Hizmeti (AKS)](../aks/tutorial-kubernetes-prepare-app.md) kümesiyle Azure Konteyner Kayıt Defteri'ni kullanmayı deneyin.

<!-- LINKS - External -->
[docker-install]: https://docs.docker.com/engine/installation/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[portal]: https://portal.azure.com
