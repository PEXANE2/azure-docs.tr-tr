---
title: Hizmet sorumlusu ile kimlik doğrulaması Azure Container Registry
description: Azure Active Directory hizmet sorumlusu kullanarak özel kapsayıcı kayıt defterinizde görüntülere erişim sağlayın.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 12/13/2018
ms.author: danlep
ms.openlocfilehash: 16ad37eaa50f0c3825d131338cc4a0abdc369978
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72262877"
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Hizmet sorumluları ile kimlik doğrulamasını Azure Container Registry

Kapsayıcı Kayıt defterinize `docker push` ve `pull` erişimi sağlamak için bir Azure Active Directory (Azure AD) hizmet sorumlusu kullanabilirsiniz. Hizmet sorumlusu kullanarak, "gözetimsiz" hizmetlere ve uygulamalarına erişim sağlayabilirsiniz.

## <a name="what-is-a-service-principal"></a>Hizmet sorumlusu nedir?

Azure AD *hizmet sorumluları* , aboneliğinizdeki Azure kaynaklarına erişim sağlar. Hizmet sorumlusunu bir hizmet için Kullanıcı kimliği olarak düşünebilirsiniz; burada "hizmet", kaynaklara erişmesi gereken herhangi bir uygulama, hizmet veya platformdur. Erişim haklarına sahip bir hizmet sorumlusunu, yalnızca belirttiğiniz kaynaklarla kapsamlı bir şekilde yapılandırabilirsiniz. Ardından, uygulamanızı veya hizmetinizi bu kaynaklara erişmek için hizmet sorumlusunun kimlik bilgilerini kullanacak şekilde yapılandırın.

Azure Container Registry bağlamında Azure 'da özel kayıt defteriniz için çekme, gönderme ve çekme veya diğer izinlerle bir Azure AD hizmet sorumlusu oluşturabilirsiniz. Tüm liste için, bkz. [Azure Container Registry roller ve izinler](container-registry-roles.md).

## <a name="why-use-a-service-principal"></a>Neden hizmet sorumlusu kullanmalıyım?

Bir Azure AD hizmet sorumlusu kullanarak özel kapsayıcı Kayıt defterinize kapsamlı erişim sağlayabilirsiniz. Her biri için Kayıt defterinize özel erişim hakları olan her bir uygulama veya hizmetlerinizin farklı hizmet sorumluları oluşturun. Ve, hizmetler ve uygulamalar arasında kimlik bilgilerinin paylaşılmasını önleyebileceğiniz için kimlik bilgilerini döndürebilir veya yalnızca hizmet sorumlusu (ve dolayısıyla uygulama) için erişimi iptal edebilirsiniz.

Örneğin, Web uygulamanızı yalnızca görüntü `pull` erişimiyle birlikte sağlayan bir hizmet sorumlusu kullanacak şekilde yapılandırın, derleme sisteminiz hem @no__t hem de `pull` erişimi ile birlikte sağlayan bir hizmet sorumlusu kullanır. Uygulamanız geliştirmede değişiklik yaptıysanız, derleme sistemini etkilemeden hizmet ilkesi kimlik bilgilerini döndürebilirsiniz.

## <a name="when-to-use-a-service-principal"></a>Hizmet sorumlusu ne zaman kullanılır?

**Gözetimsiz senaryolarda**kayıt defteri erişimi sağlamak için bir hizmet sorumlusu kullanmanız gerekir. Diğer bir deyişle, kapsayıcı görüntülerini otomatik olarak veya başka bir şekilde katılımsız bir şekilde iletme veya çekme gerektiren herhangi bir uygulama, hizmet veya komut dosyası. Örnek:

  * *Çekme*: bir kayıt defterinden Kubernetes, DC/OS ve Docker sısınma dahil olmak üzere düzenleme sistemlerine kapsayıcı dağıtın. Ayrıca, kapsayıcı kayıt defterlerinden [Azure Kubernetes hizmeti (AKS)](../aks/cluster-container-registry-integration.md), [Azure Container Instances](container-registry-auth-aci.md), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/)ve diğerleri gibi ilgili Azure hizmetlerine de çekebilirsiniz.

  * *Gönderme*: kapsayıcı görüntüleri oluşturun ve Azure Pipelines veya Jenkins gibi sürekli tümleştirme ve dağıtım çözümlerini kullanarak bunları bir kayıt defterine gönderin.

Bir kayıt defterine bireysel erişim için, örneğin, bir kapsayıcı görüntüsünü geliştirme iş istasyonunuza el ile çektiğinizde, kayıt defteri erişimi (örneğin, [az ACR oturum açma][az-acr-login]ile) yerine kendı [Azure AD kimliğinizi](container-registry-authentication.md#individual-login-with-azure-ad) kullanmanızı öneririz.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

### <a name="sample-scripts"></a>Örnek komut dosyaları

Azure CLı için yukarıdaki örnek betikleri GitHub ' da ve Azure PowerShell sürümlerinin yanı sıra bulabilirsiniz:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="authenticate-with-the-service-principal"></a>Hizmet sorumlusu ile kimlik doğrulama

Kapsayıcı Kayıt defterinize erişim verdiğiniz bir hizmet sorumlusu olduktan sonra, "gözetimsiz" hizmetlere ve uygulamalarına erişim için kimlik bilgilerini yapılandırabilir veya `docker login` komutunu kullanarak bunları girebilirsiniz. Aşağıdaki değerleri kullanın:

* **Kullanıcı adı** -hizmet sorumlusu uygulama kimliği ( *istemci kimliği*olarak da bilinir)
* **Parola** -hizmet sorumlusu parolası ( *istemci gizli anahtarı*da denir)

Her değer `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` biçiminde bir GUID 'dir. 

> [!TIP]
> [Az ad SP Reset-Credentials](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset) komutunu çalıştırarak bir hizmet sorumlusunun parolasını yeniden oluşturabilirsiniz.
>

### <a name="use-credentials-with-azure-services"></a>Azure hizmetleriyle kimlik bilgilerini kullanma

Azure Container Registry ile kimlik doğrulayabilecek herhangi bir Azure hizmetinden hizmet sorumlusu kimlik bilgilerini kullanabilirsiniz.  Çeşitli senaryolar için kayıt defterinin yönetici kimlik bilgileri yerine hizmet sorumlusu kimlik bilgilerini kullanın.

Örneğin, [Azure Container Instances](container-registry-auth-aci.md)Için bir Azure Container Registry 'den görüntü çekmek üzere kimlik bilgilerini kullanın.

### <a name="use-with-docker-login"></a>Docker oturum açma ile kullanma

Ayrıca, hizmet sorumlusu kullanarak `docker login` ' yı çalıştırabilirsiniz. Aşağıdaki örnekte, hizmet sorumlusu uygulama KIMLIĞI `$SP_APP_ID` ortam değişkenine ve `$SP_PASSWD` değişkeninde bulunan parola ile geçirilir. Docker kimlik bilgilerini yönetmek için en iyi uygulamalar için [Docker Login](https://docs.docker.com/engine/reference/commandline/login/) komut başvurusuna bakın.

```bash
# Log in to Docker with service principal credentials
docker login myregistry.azurecr.io --username $SP_APP_ID --password $SP_PASSWD
```

Oturum açıldıktan sonra Docker kimlik bilgilerini önbelleğe alır.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Container Registry ile kimlik doğrulaması yapmak için diğer senaryolar için [kimlik doğrulamaya genel bakış](container-registry-authentication.md) bölümüne bakın.

* Bir kapsayıcı kayıt defteri için hizmet sorumlusu kimlik bilgilerini depolamak ve almak üzere bir Azure Anahtar Kasası kullanmanın bir örneği için [ACR görevlerini kullanarak bir kapsayıcı görüntüsü oluşturma ve dağıtma](container-registry-tutorial-quick-task.md)öğreticisine bakın.

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login
