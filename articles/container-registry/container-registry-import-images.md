---
title: Kapsayıcı görüntülerini içeri aktarma
description: Docker komutlarını çalıştırmaya gerek kalmadan Azure API'lerini kullanarak konteyner görüntülerini Azure kapsayıcı kayıt defterine aktarın.
ms.topic: article
ms.date: 03/16/2020
ms.openlocfilehash: caf7a47ac8f7ff0e72d2e049a7013542d274a225
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80051917"
---
# <a name="import-container-images-to-a-container-registry"></a>Kapsayıcı görüntülerini konteyner kayıt defterine alma

Konteyner görüntülerini Docker komutlarını kullanmadan azure konteyner kayıt defterine kolayca içe aktarabilirsiniz (kopyalayabilirsiniz). Örneğin, görüntüleri geliştirme kayıt defterinden üretim kayıt defterine aktarın veya temel görüntüleri genel kayıt defterinden kopyalayın.

Azure Kapsayıcı Kayıt Defteri, görüntüleri varolan bir kayıt defterinden kopyalamak için bir dizi yaygın senaryoyu işler:

* Genel kayıt defterinden alma

* Aynı veya farklı bir Azure aboneliğinde başka bir Azure kapsayıcı kayıt defterinden alma

* Azure olmayan özel konteyner kayıt defterinden alma

Bir Azure kapsayıcı kayıt defterine görüntü alma Docker CLI komutları kullanarak üzerinde aşağıdaki avantajlara sahiptir:

* İstemci ortamınızın yerel bir Docker yüklemesine ihtiyacı olmadığından, desteklenen işletim sistemi türünden bağımsız olarak herhangi bir kapsayıcı görüntüsü aktarın.

* Çok mimarili görüntüleri (resmi Docker görüntüleri gibi) içe aktardığınızda, manifesto listesinde belirtilen tüm mimariler ve platformlar için görüntüler kopyalanır.

Kapsayıcı görüntülerini almak için bu makalede, Azure CLI'yi Azure Bulut BulutU'nda veya yerel olarak çalıştırmanız gerekir (sürüm 2.0.55 veya daha sonra önerilir). Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

> [!NOTE]
> Aynı kapsayıcı görüntülerini birden çok Azure bölgesine dağıtmanız gerekiyorsa, Azure Kapsayıcı Kayıt Defteri [coğrafi çoğaltmayı](container-registry-geo-replication.md)da destekler. Bir kayıt defterini coğrafi olarak kopyalayarak (Premium hizmet katmanı gereklidir), tek bir kayıt defterinden aynı görüntü ve etiket adlarıyla birden çok bölgeye hizmet edebilirsiniz.
>

## <a name="prerequisites"></a>Ön koşullar

Azure kapsayıcı kayıt defteriniz yoksa, bir kayıt defteri oluşturun. Adımlar için [Bkz. Hızlı Başlangıç: Azure CLI'yi kullanarak özel bir kapsayıcı kayıt defteri oluşturun.](container-registry-get-started-azure-cli.md)

Bir görüntüyü Azure kapsayıcı kayıt defterine aktarmak için kimliğinizin hedef kayıt defterine yazma izinleri olmalıdır (en azından Katılımcı rolü). Bkz. [Azure Kapsayıcı Kayıt Defteri rolleri ve izinleri.](container-registry-roles.md) 

## <a name="import-from-a-public-registry"></a>Genel kayıt defterinden alma

### <a name="import-from-docker-hub"></a>Docker Hub'dan İthalat

Örneğin, Docker Hub'dan *myregistry*adlı bir `hello-world:latest` kayıt defterine çok mimari görüntü almak için [az acr alma][az-acr-import] komutunu kullanın. Docker `hello-world` Hub'ın resmi bir görüntüsü olduğundan, `library` bu görüntü varsayılan depodadır. Depo adını ve isteğe bağlı olarak `--source` görüntü parametresinin değerine bir etiket ekleyin. (Bir resmin belirli bir sürümünü garanti eden, etiket yerine manifesto özetine göre isteğe bağlı olarak tanımlayabilirsiniz.)
 
```azurecli
az acr import \
  --name myregistry \
  --source docker.io/library/hello-world:latest \
  --image hello-world:latest
```

Komutu çalıştırarak `az acr repository show-manifests` birden çok bildirimin bu görüntüyle ilişkili olduğunu doğrulayabilirsiniz:

```azurecli
az acr repository show-manifests \
  --name myregistry \
  --repository hello-world
```

Aşağıdaki örnek, Docker Hub'daki depodan `tensorflow` genel bir görüntü alma:

```azurecli
az acr import \
  --name myregistry \
  --source docker.io/tensorflow/tensorflow:latest-gpu \
  --image tensorflow:latest-gpu
```

### <a name="import-from-microsoft-container-registry"></a>Microsoft Konteyner Kayıt Defteri'nden alma

Örneğin, Microsoft Kapsayıcı Kayıt Defteri'ndeki `windows` depodan en son Windows Server Core görüntüsünü aktarın.

```azurecli
az acr import \
--name myregistry \
--source mcr.microsoft.com/windows/servercore:latest \
--image servercore:latest
```

## <a name="import-from-another-azure-container-registry"></a>Başka bir Azure kapsayıcı kayıt defterinden alma

Tümleşik Azure Etkin Dizin izinlerini kullanarak başka bir Azure kapsayıcı kayıt defterinden görüntü içe aktarabilirsiniz.

* Kimliğiniz, kaynak kayıt defterinden (Reader rolü) okumak ve hedef kayıt defterine (Katılımcı rolü) yazmak için Azure Active Directory izinlerine sahip olmalıdır.

* Kayıt defteri, aynı Active Directory kiracısında aynı veya farklı bir Azure aboneliğinde olabilir.

### <a name="import-from-a-registry-in-the-same-subscription"></a>Aynı abonelikteki bir kayıt defterinden alma

Örneğin, `aci-helloworld:latest` görüntüyü aynı Azure aboneliğinde kaynak kayıt *defteri mysourceregistry'den* *myregistry'ye* taşıyın.

```azurecli
az acr import \
  --name myregistry \
  --source mysourceregistry.azurecr.io/aci-helloworld:latest \
  --image aci-helloworld:latest
```

Aşağıdaki örnek, bir resmi etiket yerine manifest sindirimi (SHA-256 karma, temsil) `sha256:...`ile içeri yedirerek içeri iter:

```azurecli
az acr import \
  --name myregistry \
  --source mysourceregistry.azurecr.io/aci-helloworld@sha256:123456abcdefg 
```

### <a name="import-from-a-registry-in-a-different-subscription"></a>Farklı bir abonelikteki kayıt defterinden alma

Aşağıdaki örnekte, *mysourceregistry* aynı Active Directory kiracı *myregistry* farklı bir abonelik olduğunu. Kaynak kayıt defterinin kaynak kimliğini `--registry` parametreyle birlikte ver. Parametrenin `--source` kayıt defteri giriş sunucusu adını değil, yalnızca kaynak deposunu ve etiketini belirlediğine dikkat edin.

```azurecli
az acr import \
  --name myregistry \
  --source samples/aci-helloworld:latest \
  --image aci-hello-world:latest \
  --registry /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sourceResourceGroup/providers/Microsoft.ContainerRegistry/registries/mysourceregistry
```

### <a name="import-from-a-registry-using-service-principal-credentials"></a>Hizmet temel kimlik bilgilerini kullanarak kayıt defterinden alma

Etkin Dizin izinlerini kullanarak erişemediğiniz bir kayıt defterinden içe aktarmak için hizmet temel kimlik bilgilerini (varsa) kullanabilirsiniz. Kaynak kayıt defterine ACRPull erişimi olan bir Active Directory [hizmet yöneticisinin](container-registry-auth-service-principal.md) appID ve şifresini tedarik edin. Hizmet sorumlusu kullanmak, yapı sistemleri ve görüntüleri kayıt defterinize aktarması gereken diğer sahipsiz sistemler için yararlıdır.

```azurecli
az acr import \
  --name myregistry \
  --source sourceregistry.azurecr.io/sourcerrepo:tag \
  --image targetimage:tag \
  --username <SP_App_ID> \
  –-password <SP_Passwd>
```

## <a name="import-from-a-non-azure-private-container-registry"></a>Azure olmayan özel konteyner kayıt defterinden alma

Kayıt defterine çekme erişimi sağlayan kimlik bilgilerini belirterek özel bir kayıt defterinden görüntü aktarın. Örneğin, özel bir Docker kayıt defterinden bir görüntü çekin: 

```azurecli
az acr import \
  --name myregistry \
  --source docker.io/sourcerepo/sourceimage:tag \
  --image sourceimage:tag \
  --username <username> \
  --password <password>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir azure kapsayıcı kayıt defterine kapsayıcı resimleri alma hakkında bir genel kayıt defteri veya başka bir özel kayıt defteri öğrendim. Ek görüntü alma seçenekleri için [az acr alma][az-acr-import] komutu başvurusuna bakın. 


<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-import]: /cli/azure/acr#az-acr-import
[azure-cli]: /cli/azure/install-azure-cli
