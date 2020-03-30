---
title: Kayıt defteri nin durumunu kontrol edin
description: Yerel Docker yapılandırması ve kayıt defterine bağlantı da dahil olmak üzere bir Azure kapsayıcı kayıt defteri kullanırken sık karşılaşılan sorunları belirlemek için nasıl hızlı bir tanılama komutu çalıştırılamayı öğrenin
ms.topic: article
ms.date: 07/02/2019
ms.openlocfilehash: ea4432c9e92c4a0380517e39678814e2d1cb3bfc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74456429"
---
# <a name="check-the-health-of-an-azure-container-registry"></a>Azure kapsayıcı kayıt defterinin sistem durumunu denetleme

Azure kapsayıcı kayıt defteri kullanırken, zaman zaman sorunlarla karşılaşabilirsiniz. Örneğin, yerel ortamınızda Docker ile ilgili bir sorun nedeniyle kapsayıcı görüntüsü çekemeyebilirsiniz. Veya, bir ağ sorunu kayıt defterine bağlanmanızı engelleyebilir. 

İlk tanılama adımı olarak, ortamın durumu hakkında bilgi almak ve isteğe bağlı olarak hedef kayıt defterine erişmek için [az acr onay durumu][az-acr-check-health] komutunu çalıştırın. Bu komut Azure CLI sürüm 2.0.67 veya sonraki sürümlerde kullanılabilir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

## <a name="run-az-acr-check-health"></a>Çalıştır az acr check-health

Aşağıdaki örnekler, komutu çalıştırmanın `az acr check-health` farklı yollarını gösterir.

> [!NOTE]
> Komutu Azure Bulut Su şurama'da çalıştırArsanız, yerel ortam denetlenmez. Ancak, hedef kayıt defterine erişimi denetleyebilirsiniz.

### <a name="check-the-environment-only"></a>Yalnızca ortamı kontrol edin

Yerel Docker daemon, CLI sürümü ve Helm istemci yapılandırmasını denetlemek için komutu ek parametreler olmadan çalıştırın:

```azurecli
az acr check-health
```

### <a name="check-the-environment-and-a-target-registry"></a>Ortamı ve hedef kayıt defterini kontrol edin

Bir kayıt defterine erişimi denetlemek ve yerel ortam denetimleri gerçekleştirmek için hedef kayıt defterinin adını geçirin. Örnek:

```azurecli
az acr check-health --name myregistry
```

## <a name="error-reporting"></a>Hata raporlama

Komut bilgileri standart çıktıya kaydeder. Bir sorun algılanırsa, bir hata kodu ve açıklama sağlar. Kodlar ve olası çözümler hakkında daha fazla bilgi için [hata başvurusuna](container-registry-health-error-reference.md)bakın.

Varsayılan olarak, bir hata bulduğunda komut durur. Hata lar bulunsa bile tüm sistem durumu denetimleri için çıktı sağlayacak şekilde komutu da çalıştırabilirsiniz. Aşağıdaki `--ignore-errors` örneklerde gösterildiği gibi parametre ekleyin:

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

[az acr onay durumu][az-acr-check-health] komutu tarafından döndürülen hata kodları hakkında ayrıntılı bilgi [için, Sistem Durumu denetimi hata başvurusuna](container-registry-health-error-reference.md)bakın.

Azure Kapsayıcı Kayıt Defteri hakkında sık sorulan sorular ve bilinen diğer sorunlar için [SSS](container-registry-faq.md) bölümüne bakın.





<!-- LINKS - internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health
