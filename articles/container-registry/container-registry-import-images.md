---
title: Kapsayıcı görüntülerini içeri aktarma
description: Docker komutlarını çalıştırmaya gerek kalmadan Azure API 'Lerini kullanarak kapsayıcı görüntülerini bir Azure Container Registry 'ye aktarın.
ms.topic: article
ms.date: 03/16/2020
ms.openlocfilehash: a7a6566540880d027b1dc3428d394b352f34318d
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86023525"
---
# <a name="import-container-images-to-a-container-registry"></a>Kapsayıcı görüntülerini kapsayıcı kayıt defterine aktarma

Kapsayıcı görüntülerini Docker komutlarını kullanmadan bir Azure Container Registry 'ye kolayca içeri aktarabilirsiniz (kopyalayabilirsiniz). Örneğin, bir geliştirme kayıt defterindeki görüntüleri bir üretim kayıt defterine aktarın veya temel görüntüleri ortak bir kayıt defterinden kopyalayın.

Azure Container Registry, var olan bir kayıt defterinden görüntüleri kopyalamak için bir dizi yaygın senaryoyu işler:

* Ortak bir kayıt defterinden içeri aktarma

* Aynı veya farklı bir Azure aboneliğinde başka bir Azure Container Registry 'den içeri aktarma

* Azure olmayan özel kapsayıcı kayıt defterinden içeri aktarma

Azure Container Registry 'de görüntü içeri aktarma işlemi Docker CLı komutlarını kullanarak aşağıdaki avantajlara sahiptir:

* İstemci ortamınız yerel bir Docker yüklemesine ihtiyaç duymadığından, desteklenen işletim sistemi türünden bağımsız olarak tüm kapsayıcı görüntülerini içeri aktarın.

* Multi-Architecture görüntülerini (resmi Docker görüntüleri gibi) içeri aktardığınızda, bildirim listesinde belirtilen tüm mimarilerin ve platformların görüntüleri bir kopyası alınır.

Kapsayıcı görüntülerini içeri aktarmak için bu makale, Azure CLı 'yı Azure Cloud Shell veya yerel olarak (sürüm 2.0.55 veya üzeri önerilir) çalıştırmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

> [!NOTE]
> Aynı kapsayıcı görüntülerini birden çok Azure bölgesinde dağıtmanız gerekiyorsa Azure Container Registry [Coğrafi çoğaltmayı](container-registry-geo-replication.md)da destekler. Bir kayıt defteri coğrafi olarak çoğaltılırken (Premium hizmet katmanı gereklidir), tek bir kayıt defterinden aynı görüntü ve etiket adları ile birden çok bölgeye hizmet verebilirsiniz.
>

## <a name="prerequisites"></a>Ön koşullar

Zaten bir Azure Container kayıt defteriniz yoksa bir kayıt defteri oluşturun. Adımlar için bkz. [hızlı başlangıç: Azure CLI kullanarak özel kapsayıcı kayıt defteri oluşturma](container-registry-get-started-azure-cli.md).

Bir görüntüyü Azure Container Registry 'ye aktarmak için, kimliğinizin hedef kayıt defterine (en az katkıda bulunan rolü) yazma izinleri olması gerekir. Bkz. [Azure Container Registry rolleri ve izinleri](container-registry-roles.md). 

## <a name="import-from-a-public-registry"></a>Ortak bir kayıt defterinden içeri aktarma

### <a name="import-from-docker-hub"></a>Docker Hub 'dan içeri aktar

Örneğin, [az acr import][az-acr-import] `hello-world:latest` Docker Hub 'ından Multi-Architecture görüntüsünü *myregistry*adlı bir kayıt defterine aktarmak için az ACR Import komutunu kullanın. `hello-world`, Docker Hub 'ından resmi bir görüntü olduğundan, bu görüntü varsayılan `library` depodadır. Depo adını ve isteğe bağlı olarak Image parametresinin değerine bir etiketi ekleyin `--source` . (Bir görüntüyü, bir görüntünün belirli bir sürümünü garanti eden etiketi yerine, bildirim özetine göre belirleyebilirsiniz.)
 
```azurecli
az acr import \
  --name myregistry \
  --source docker.io/library/hello-world:latest \
  --image hello-world:latest
```

Komutu çalıştırarak bu görüntüyle ilişkili birden fazla bildirimin olduğunu doğrulayabilirsiniz `az acr repository show-manifests` :

```azurecli
az acr repository show-manifests \
  --name myregistry \
  --repository hello-world
```

Aşağıdaki örnek, `tensorflow` Docker Hub 'daki depodan ortak bir görüntü içeri aktarır:

```azurecli
az acr import \
  --name myregistry \
  --source docker.io/tensorflow/tensorflow:latest-gpu \
  --image tensorflow:latest-gpu
```

### <a name="import-from-microsoft-container-registry"></a>Microsoft Container Registry içeri aktar

Örneğin, `ltsc2019` Windows Server çekirdek görüntüsünü `windows` Microsoft Container Registry deposundan içeri aktarın.

```azurecli
az acr import \
--name myregistry \
--source mcr.microsoft.com/windows/servercore:ltsc2019 \
--image servercore:ltsc2019
```

## <a name="import-from-another-azure-container-registry"></a>Başka bir Azure Container Registry 'den içeri aktar

Bir görüntüyü, tümleşik Azure Active Directory izinleri kullanarak başka bir Azure Container kayıt defterinden içeri aktarabilirsiniz.

* Kimliğiniz, kaynak kayıt defterinden (okuyucu rolü) okumak ve hedef kayıt defterine (katkıda bulunan rolü) yazmak için Azure Active Directory izinlere sahip olmalıdır.

* Kayıt defteri aynı Active Directory kiracısında aynı veya farklı bir Azure aboneliğinde olabilir.

### <a name="import-from-a-registry-in-the-same-subscription"></a>Aynı abonelikteki bir kayıt defterinden içeri aktarma

Örneğin, `aci-helloworld:latest` bir kaynak kayıt defteri *mysourceregyıpdan* aynı Azure aboneliğindeki *myregistry* 'e ait görüntüyü içeri aktarın.

```azurecli
az acr import \
  --name myregistry \
  --source mysourceregistry.azurecr.io/aci-helloworld:latest \
  --image aci-helloworld:latest
```

Aşağıdaki örnek, bir görüntüyü etiketi yerine manifest Digest (SHA-256 karması, olarak temsil edilir) ile içe aktarır `sha256:...` :

```azurecli
az acr import \
  --name myregistry \
  --source mysourceregistry.azurecr.io/aci-helloworld@sha256:123456abcdefg 
```

### <a name="import-from-a-registry-in-a-different-subscription"></a>Farklı bir abonelikteki kayıt defterinden içeri aktarma

Aşağıdaki örnekte, *mysourceregbakanlığı* aynı Active Directory kiracısındaki *myregistry* 'den farklı bir abonelikte yer verilir. Kaynak kayıt defterinin parametresi ile kaynak KIMLIĞINI sağlayın `--registry` . `--source`Parametresinin kayıt defteri oturum açma sunucusu adını değil yalnızca kaynak depoyu ve etiketi belirttiğinden emin olun.

```azurecli
az acr import \
  --name myregistry \
  --source samples/aci-helloworld:latest \
  --image aci-hello-world:latest \
  --registry /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sourceResourceGroup/providers/Microsoft.ContainerRegistry/registries/mysourceregistry
```

### <a name="import-from-a-registry-using-service-principal-credentials"></a>Hizmet sorumlusu kimlik bilgilerini kullanarak bir kayıt defterinden içeri aktarma

Active Directory izinleri kullanarak erişekullanmadığınız bir kayıt defterinden içeri aktarmak için, hizmet sorumlusu kimlik bilgilerini (varsa) kullanabilirsiniz. Kaynak kayıt defterine ACRPull erişimi olan Active Directory [hizmet sorumlusunun](container-registry-auth-service-principal.md) AppID 'sini ve parolasını sağlayın. Hizmet sorumlusu kullanmak, derleme sistemleri ve görüntüleri kayıt defterinize aktarması gereken diğer katılımsız sistemler için yararlıdır.

```azurecli
az acr import \
  --name myregistry \
  --source sourceregistry.azurecr.io/sourcerrepo:tag \
  --image targetimage:tag \
  --username <SP_App_ID> \
  –-password <SP_Passwd>
```

## <a name="import-from-a-non-azure-private-container-registry"></a>Azure olmayan özel kapsayıcı kayıt defterinden içeri aktarma

Kayıt defterine çekme erişimini etkinleştiren kimlik bilgilerini belirterek özel bir kayıt defterinden görüntü içeri aktarın. Örneğin, özel bir Docker kayıt defterinden bir görüntü çekin: 

```azurecli
az acr import \
  --name myregistry \
  --source docker.io/sourcerepo/sourceimage:tag \
  --image sourceimage:tag \
  --username <username> \
  --password <password>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kapsayıcı görüntülerini genel bir kayıt defterine veya başka bir özel kayıt defterinden Azure Container Registry 'ye aktarma hakkında bilgi edindiniz. Ek görüntü içeri aktarma seçenekleri için, [az ACR Import][az-acr-import] komut başvurusuna bakın. 


<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-import]: /cli/azure/acr#az-acr-import
[azure-cli]: /cli/azure/install-azure-cli
