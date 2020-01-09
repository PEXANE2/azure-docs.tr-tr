---
title: Azure Service Fabric-yönetilen kimlik desteğini etkinleştirmek için mevcut bir Azure Service Fabric kümesini yapılandırın
description: Bu makalede, Yönetilen kimlikler için desteği etkinleştirmek üzere mevcut bir Azure Service Fabric kümesini nasıl yapılandırabileceğiniz gösterilmektedir
ms.topic: article
ms.date: 12/09/2019
ms.openlocfilehash: 13b8b38a206b0dae0877263a5cda56a134d4788d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75351601"
---
# <a name="configure-an-existing-azure-service-fabric-cluster-to-enable-managed-identity-support-preview"></a>Yönetilen kimlik desteğini etkinleştirmek için mevcut bir Azure Service Fabric kümesi yapılandırma (Önizleme)
Azure Service Fabric uygulamaları için yönetilen kimlik özelliğine erişebilmek için öncelikle kümede **yönetilen kimlik belirteci hizmetini** etkinleştirmeniz gerekir. Bu hizmet, yönetilen kimliklerini kullanan Service Fabric uygulamalarının kimlik doğrulamasından ve kendi adına erişim belirteçleri elde etmeye sorumludur. Hizmet etkinleştirildikten sonra, sol bölmedeki **sistem** bölümü altında, **doku:/System/Managedıdentitytokenservice**adı altında çalışan Service Fabric Explorer görebilirsiniz.

> [!NOTE]
> **Yönetilen kimlik belirteci hizmetini**etkinleştirmek için Service Fabric Runtime sürümü 6.5.658.9590 veya üzeri gereklidir.  
> 
> Küme kaynağını açıp **temel** bileşenler bölümündeki **Service Fabric sürümü** özelliğini denetleyerek Azure Portal bir kümenin Service Fabric sürümünü bulabilirsiniz.
> 
> Küme **el ile** yükseltme modundaysa, önce bunu 6.5.658.9590 veya sonraki bir sürüme yükseltmeniz gerekir.


## <a name="enable-the-managed-identity-token-service-in-an-existing-cluster"></a>Mevcut bir kümede yönetilen kimlik belirteci hizmetini etkinleştirin
Mevcut bir kümede yönetilen kimlik belirteci hizmetini etkinleştirmek için iki değişiklik belirten bir küme yükseltmesi başlatmanız gerekir: yönetilen kimlik belirteci hizmetini etkinleştirme ve her bir düğümün yeniden başlatılmasını isteme. Bunu yapmak için, Azure Resource Manager şablonuna aşağıdaki iki parçacığı ekleyin:

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

Değişikliklerin etkili olabilmesi için yükseltme ilkesini, yükseltmenin kümede ilerledikçe her düğümde Service Fabric çalışma zamanının zorla yeniden başlatılmasını belirtmek için de değiştirmeniz gerekir. Bu yeniden başlatma, yeni etkinleştirilmiş sistem hizmetinin her düğüm üzerinde başlatılmış ve çalışır olmasını sağlar. Aşağıdaki kod parçacığında, temel ayar `forceRestart`. ayarların geri kalanı için mevcut değerlerinizi kullanın.  

```json
"upgradeDescription": {
    "forceRestart": true,
    "healthCheckRetryTimeout": "00:45:00",
    "healthCheckStableDuration": "00:05:00",
    "healthCheckWaitDuration": "00:05:00",
    "upgradeDomainTimeout": "02:00:00",
    "upgradeReplicaSetCheckTimeout": "1.00:00:00",
    "upgradeTimeout": "12:00:00"
}
```

> [!NOTE]
> Yükseltmenin başarıyla tamamlanmasından sonra, sonraki yükseltmelerin etkilerini en aza indirmek için `forceRestart` ayarını geri almayı unutmayın. 

## <a name="errors-and-troubleshooting"></a>Hatalar ve sorun giderme

Dağıtım aşağıdaki iletiyle başarısız olursa, kümenin yeterince yüksek bir Service Fabric sürümünde çalışmadığı anlamına gelir:

```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```

## <a name="next-steps"></a>Sonraki adımlar
* [Sistem tarafından atanan yönetilen kimlik ile Azure Service Fabric uygulaması dağıtma](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Kullanıcı tarafından atanan yönetilen kimlik ile bir Azure Service Fabric uygulaması dağıtma](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Hizmet kodundan Service Fabric uygulamasının yönetilen kimliğinden yararlanın](./how-to-managed-identity-service-fabric-app-code.md)
* [Azure Service Fabric uygulamasına diğer Azure kaynaklarına erişim izni verme](./how-to-grant-access-other-resources.md)