---
title: Kayıt defteri sistem durumunu denetle
description: Yerel Docker yapılandırması ve kayıt defteri bağlantısı dahil olmak üzere bir Azure Container Registry kullanırken sık karşılaşılan sorunları belirlemek için hızlı tanılama komutunu çalıştırmayı öğrenin
ms.topic: article
ms.date: 07/02/2019
ms.openlocfilehash: ea4432c9e92c4a0380517e39678814e2d1cb3bfc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74456429"
---
# <a name="check-the-health-of-an-azure-container-registry"></a>Azure Container Registry 'nin sistem durumunu denetleme

Azure Container Registry kullanırken, zaman zaman sorunlarla karşılaşabilirsiniz. Örneğin, yerel ortamınızda Docker ile ilgili bir sorun nedeniyle bir kapsayıcı görüntüsünü çekmeyebilirsiniz. Ya da bir ağ sorunu, kayıt defterine bağlanmanızı engelleyebilir. 

İlk tanılama adımı olarak, ortamın sistem durumu ve isteğe bağlı olarak bir hedef kayıt defterine erişim hakkında bilgi almak için [az ACR Check-Health][az-acr-check-health] komutunu çalıştırın. Bu komut, Azure CLı sürüm 2.0.67 veya sonraki sürümlerinde kullanılabilir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

## <a name="run-az-acr-check-health"></a>Çalıştır az ACR Check-Health

Takip örnekleri, `az acr check-health` komutunu çalıştırmanın farklı yollarını gösterir.

> [!NOTE]
> Komutu Azure Cloud Shell ' de çalıştırırsanız, yerel ortam denetlenmez. Ancak, bir hedef kayıt defterine erişimi denetleyebilirsiniz.

### <a name="check-the-environment-only"></a>Yalnızca ortamı denetle

Yerel Docker Daemon, CLı sürümü ve Held istemci yapılandırmasını denetlemek için, ek parametre olmadan komutunu çalıştırın:

```azurecli
az acr check-health
```

### <a name="check-the-environment-and-a-target-registry"></a>Ortamı ve hedef kayıt defterini denetleme

Bir kayıt defterine erişimi denetlemek ve yerel ortam denetimleri gerçekleştirmek için bir hedef kayıt defteri adı geçirin. Örneğin:

```azurecli
az acr check-health --name myregistry
```

## <a name="error-reporting"></a>Hata bildirimi

Komut, bilgileri standart çıktıya kaydeder. Bir sorun algılanırsa, bir hata kodu ve açıklaması sağlar. Kodlar ve olası çözümler hakkında daha fazla bilgi için bkz. [hata başvurusu](container-registry-health-error-reference.md).

Varsayılan olarak, komut her bir hata bulduğunda yanıt vermez. Ayrıca, hatalar bulunsa bile, tüm sistem durumu denetimleri için çıkış sağlamak üzere komutunu çalıştırabilirsiniz. Aşağıdaki örneklerde `--ignore-errors` gösterildiği gibi parametresini ekleyin:

```azurecli
# Check environment only
az acr check-health --ignore-errors

# Check environment and target registry
az acr check-health --name myregistry --ignore-errors
```      

Örnek çıktı:

```console
$ az acr check-health --name myregistry --ignore-errors --yes

Docker daemon status: available
Docker version: Docker version 18.09.2, build 6247962
Docker pull of 'mcr.microsoft.com/mcr/hello-world:latest' : OK
ACR CLI version: 2.2.9
Helm version:
Client: &version.Version{SemVer:"v2.14.1", GitCommit:"5270352a09c7e8b6e8c9593002a73535276507c0", GitTreeState:"clean"}
DNS lookup to myregistry.azurecr.io at IP 40.xxx.xxx.162 : OK
Challenge endpoint https://myregistry.azurecr.io/v2/ : OK
Fetch refresh token for registry 'myregistry.azurecr.io' : OK
Fetch access token for registry 'myregistry.azurecr.io' : OK
```  



## <a name="next-steps"></a>Sonraki adımlar

[Az ACR Check-Health][az-acr-check-health] komutu tarafından döndürülen hata kodları hakkındaki ayrıntılar için bkz. [sistem durumu denetimi hata başvurusu](container-registry-health-error-reference.md).

Sık sorulan sorular ve Azure Container Registry ilgili diğer bilinen sorunlar için [SSS](container-registry-faq.md) bölümüne bakın.





<!-- LINKS - internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health
