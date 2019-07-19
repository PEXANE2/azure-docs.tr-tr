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
ms.openlocfilehash: 97c45a009b155eea7bc61a9dd337090b9e3c1b42
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309954"
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Hizmet sorumluları ile kimlik doğrulamasını Azure Container Registry

Kapsayıcı görüntü `docker push` ve `pull` kapsayıcı Kayıt defterinize erişim sağlamak için bir Azure Active Directory (Azure AD) hizmet sorumlusu kullanabilirsiniz. Hizmet sorumlusu kullanarak, "gözetimsiz" hizmetlere ve uygulamalarına erişim sağlayabilirsiniz.

## <a name="what-is-a-service-principal"></a>Hizmet sorumlusu nedir?

Azure AD *hizmet sorumluları* , aboneliğinizdeki Azure kaynaklarına erişim sağlar. Hizmet sorumlusunu bir hizmet için Kullanıcı kimliği olarak düşünebilirsiniz; burada "hizmet", kaynaklara erişmesi gereken herhangi bir uygulama, hizmet veya platformdur. Erişim haklarına sahip bir hizmet sorumlusunu, yalnızca belirttiğiniz kaynaklarla kapsamlı bir şekilde yapılandırabilirsiniz. Ardından, uygulamanızı veya hizmetinizi bu kaynaklara erişmek için hizmet sorumlusunun kimlik bilgilerini kullanacak şekilde yapılandırın.

Azure Container Registry bağlamında Azure 'da özel kayıt defteriniz için çekme, gönderme ve çekme veya diğer izinlerle bir Azure AD hizmet sorumlusu oluşturabilirsiniz. Tüm liste için, bkz. [Azure Container Registry roller ve izinler](container-registry-roles.md).

## <a name="why-use-a-service-principal"></a>Neden hizmet sorumlusu kullanmalıyım?

Bir Azure AD hizmet sorumlusu kullanarak özel kapsayıcı Kayıt defterinize kapsamlı erişim sağlayabilirsiniz. Her biri Kayıt defterinize özel erişim hakları olan her bir uygulama veya hizmetlerinizin farklı hizmet sorumluları oluşturabilirsiniz. Ve, hizmetler ve uygulamalar arasında kimlik bilgilerinin paylaşılmasını önleyebileceğiniz için kimlik bilgilerini döndürebilir veya yalnızca hizmet sorumlusu (ve dolayısıyla uygulama) için erişimi iptal edebilirsiniz.

Örneğin, Web uygulamanız buna yalnızca görüntü `pull` erişimi sağlayan bir hizmet sorumlusu kullanabilir, ancak yapı sisteminiz, `pull` hem hem de `push` erişimiyle birlikte sağlayan bir hizmet sorumlusu kullanabilir. Uygulamanız geliştirmede değişiklik yaptıysanız, derleme sistemini etkilemeden hizmet ilkesi kimlik bilgilerini döndürebilirsiniz.

## <a name="when-to-use-a-service-principal"></a>Hizmet sorumlusu ne zaman kullanılır?

**Gözetimsiz senaryolarda**kayıt defteri erişimi sağlamak için bir hizmet sorumlusu kullanmanız gerekir. Diğer bir deyişle, kapsayıcı görüntülerini otomatik olarak veya başka bir şekilde katılımsız bir şekilde iletme veya çekme gerektiren herhangi bir uygulama, hizmet veya komut dosyası.

Bir kayıt defterine bireysel erişim için, örneğin, bir kapsayıcı görüntüsünü geliştirme iş istasyonunuza el ile çektiğinizde, bunun yerine kendi [Azure AD kimliğinizi](container-registry-authentication.md#individual-login-with-azure-ad) kayıt defteri erişimi (örneğin, [az ACR LOGIN][az-acr-login]ile) kullanmanız gerekir.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="sample-scripts"></a>Örnek komut dosyaları

Azure CLı için yukarıdaki örnek betikleri GitHub 'da ve Azure PowerShell için de bulabilirsiniz:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcı Kayıt defterinize erişim verdiğiniz bir hizmet sorumlusu olduktan sonra, uygulama ve hizmetinizdeki kimlik bilgilerini gözetimsiz kayıt defteri etkileşimi için kullanabilirsiniz. Azure Container Registry ile kimlik doğrulayabilecek herhangi bir Azure hizmetinden hizmet sorumlusu kimlik bilgilerini kullanabilirsiniz. Örneklere şunlar dahildir:

* [Azure Kubernetes Service (AKS) Azure Container Registry ile kimlik doğrulama](container-registry-auth-aks.md)
* [Azure Container Instances (ACI) Azure Container Registry kimlik doğrulaması](container-registry-auth-aci.md)

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login
