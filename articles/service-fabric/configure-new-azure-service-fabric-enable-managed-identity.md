---
title: Yönetilen kimlikle yeni bir Service Fabric kümesi dağıtma
description: Bu makalede, yönetilen kimlik etkin olan yeni bir Service Fabric kümesinin nasıl oluşturulacağı gösterilmektedir
ms.topic: article
ms.date: 12/09/2019
ms.openlocfilehash: 4893fe47de78445a7dccb4f5800498b30cd6c1f2
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614868"
---
# <a name="create-a-new-azure-service-fabric-cluster-with-managed-identity-support-preview"></a>Yönetilen kimlik desteği (Önizleme) ile yeni bir Azure Service Fabric kümesi oluşturma

Azure Service Fabric uygulamaları için yönetilen kimlik özelliğine erişebilmek için öncelikle kümede yönetilen kimlik belirteci hizmetini etkinleştirmeniz gerekir. Bu hizmet, yönetilen kimliklerini kullanan Service Fabric uygulamalarının kimlik doğrulamasından ve kendi adına erişim belirteçleri elde etmeye sorumludur. Hizmet etkinleştirildikten sonra, sol bölmedeki **sistem** bölümü altında, diğer sistem hizmetleri ' nin yanında bulunan **Fabric:/System/Managedıdentitytokenservice** altında çalışan Service Fabric Explorer görebilirsiniz.

> [!NOTE]
> **Yönetilen kimlik belirteci hizmetini**etkinleştirmek için Service Fabric Runtime sürümü 6.5.658.9590 veya üzeri gereklidir.  

## <a name="enable-the-managed-identity-token-service"></a>Yönetilen kimlik belirteci hizmetini etkinleştirin 
Küme oluşturma sırasında yönetilen kimlik belirteci hizmetini etkinleştirmek için aşağıdaki kod parçacığını bir Azure Resource Manager şablonunda kullanabilirsiniz:

```json
"fabricSettings": [
    {
        "name": "ManagedIdentityTokenService",
        "parameters": [
            {
                "name": "IsEnabled",
                "value": "true"
            }
        ]
    }
]
```

## <a name="errors"></a>Hatalar

Dağıtım Bu iletiyle başarısız olursa, kümenin gerekli Service Fabric sürümünde olmadığı anlamına gelir (desteklenen en düşük çalışma zamanı 6,5 CU2 UYGULAMAZSANıZ):



```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```


## <a name="related-articles"></a>İlgili makaleler
* Azure Service Fabric [yönetilen kimlik desteğini](./concepts-managed-identity.md) gözden geçirme

* [Mevcut bir Azure Service Fabric kümesinde yönetilen kimlik desteğini etkinleştir](./configure-existing-cluster-enable-managed-identity-token-service.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Sistem tarafından atanan yönetilen kimlik ile Azure Service Fabric uygulaması dağıtma](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Kullanıcı tarafından atanan yönetilen kimlik ile bir Azure Service Fabric uygulaması dağıtma](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Hizmet kodundan Service Fabric uygulamasının yönetilen kimliğinden yararlanın](./how-to-managed-identity-service-fabric-app-code.md)
* [Azure Service Fabric uygulamasına diğer Azure kaynaklarına erişim izni verme](./how-to-grant-access-other-resources.md)