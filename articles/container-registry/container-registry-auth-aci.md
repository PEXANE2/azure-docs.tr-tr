---
title: Konteyner Örneklerinden Erişim
description: Azure Etkin Dizin hizmet yöneticisini kullanarak Azure Kapsayıcı Örnekleri'nden özel kapsayıcı kayıt defterinizdeki resimlere nasıl erişileceğinizi öğrenin.
ms.topic: article
ms.date: 04/23/2018
ms.openlocfilehash: b1bc8119c495dea99c6bdc4923db198d041a1e9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74456511"
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-instances"></a>Azure Kapsayıcı Örnekleri'nden Azure Kapsayıcı Kayıt Defteri ile kimlik doğrulaması

Azure Kapsayıcı Kayıt Defteri'ndeki özel konteyner kayıtlarınıza erişim sağlamak için bir Azure Etkin Dizin (Azure AD) hizmet ilkesini kullanabilirsiniz.

Bu makalede, kayıt defterinize *çekme* izinleri içeren bir Azure AD hizmet ilkesi oluşturmayı ve yapılandırmayı öğrenirsiniz. Ardından, azure kapsayıcı örneklerinde (ACI) kimlik doğrulaması için hizmet ilkesini kullanarak özel kayıt defterinizden resmini çeken bir kapsayıcı başlatırsınız.

## <a name="when-to-use-a-service-principal"></a>Hizmet sorumlusu ne zaman kullanılır?

Otomatik veya gözetimsiz bir şekilde kapsayıcı örnekleri oluşturan uygulamalar veya hizmetler gibi **başsız senaryolarda**ACI'den kimlik doğrulaması için bir hizmet ilkesi kullanmanız gerekir.

Örneğin, her gece çalışan ve bazı verileri işlemek için görev tabanlı bir [kapsayıcı örneği](../container-instances/container-instances-restart-policy.md) oluşturan otomatik bir komut dosyanız varsa, kayıt defterine kimlik doğrulamak için yalnızca çekme izinlerine sahip bir hizmet ilkesi kullanabilir. Daha sonra, diğer hizmetleri ve uygulamaları etkilemeden hizmet sorumlusunun kimlik bilgilerini döndürebilir veya erişimini tamamen iptal edebilirsiniz.

Kayıt defteri [yöneticisi kullanıcısı](container-registry-authentication.md#admin-account) devre dışı bırakıldığında hizmet ilkeleri de kullanılmalıdır.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="authenticate-using-the-service-principal"></a>Hizmet ilkesini kullanarak kimlik doğrulaması

Bir hizmet ilkesini kullanarak Azure Kapsayıcı Örnekleri'nde bir `--registry-username`kapsayıcı başlatmak `--registry-password`için, kimliğini ve parolasını belirtin.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerregistry.azurecr.io/myimage:v1 \
    --registry-login-server mycontainerregistry.azurecr.io \
    --registry-username <service-principal-ID> \
    --registry-password <service-principal-password>
```

## <a name="sample-scripts"></a>Örnek komut dosyaları

Azure CLI için önceki örnek komut dosyalarını GitHub'da ve Azure PowerShell sürümlerinde bulabilirsiniz:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler, hizmet ilkeleri ve ACR ile çalışma hakkında ek ayrıntılar içerir:

* [Hizmet ilkeleriyle Azure Kapsayıcı Kayıt Defteri kimlik doğrulaması](container-registry-auth-service-principal.md)
* [Azure Kubernetes Hizmetinden (AKS) Azure Konteyner Kayıt Defteri ile kimlik doğrulaması](../aks/cluster-container-registry-integration.md)

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
