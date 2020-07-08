---
title: Azure Güvenlik Merkezi 'nde özel güvenlik ilkeleri oluşturma | Microsoft Docs
description: Azure Güvenlik Merkezi tarafından izlenen Azure özel ilke tanımları.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: memildin
ms.openlocfilehash: 6f11de7f38ff97b1141afa48c26788a171727cb6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559555"
---
# <a name="using-custom-security-policies"></a>Özel güvenlik ilkeleri kullanma

Azure Güvenlik Merkezi, sistem ve ortamınızın güvenliğini sağlamaya yardımcı olmak için güvenlik önerileri oluşturur. Bu öneriler, tüm müşterilere sağlanan genel, varsayılan güvenlik ilkesine dahil olan sektör en iyi uygulamalarına dayalıdır. Ayrıca, güvenlik merkezi 'nin sektör ve mevzuat standartları hakkında bilgi edinmeleri de gelebilir.

Bu özellikle kendi *özel* girişimlerinizi ekleyebilirsiniz. Daha sonra ortamınız oluşturduğunuz ilkeleri izmazsa öneriler alacaksınız. Oluşturmakta olduğunuz özel girişimler, mevzuata uygunluk panosundaki yerleşik girişimlerle birlikte görünür ve bu da [yasal uyumluluğun geliştirilmesi](security-center-compliance-dashboard.md)öğreticide açıklanmıştır.

[Azure ilkesi belgelerinde](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#definition-location)açıklandığı gibi, özel girişimleriniz için bir konum belirttiğinizde, bir yönetim grubu veya abonelik olması gerekir. 

## <a name="to-add-a-custom-initiative-to-your-subscription"></a>Aboneliğinize özel bir girişim eklemek için 

1. Güvenlik Merkezi 'nin kenar çubuğundan **güvenlik ilkesi** sayfasını açın.

1. Özel bir girişim eklemek istediğiniz bir abonelik veya yönetim grubu seçin.

    [![Özel ilkenizi oluşturacağınız bir abonelik seçme](media/custom-security-policies/custom-policy-selecting-a-subscription.png)](media/custom-security-policies/custom-policy-selecting-a-subscription.png#lightbox)

    > [!NOTE]
    > Güvenlik Merkezi 'nde değerlendirilmek ve gösterilmesi için, abonelik düzeyinde (veya üzeri) özel standartlar eklemeniz gerekir. 
    >
    > Özel bir standart eklediğinizde, bu kapsama bir *girişim* atar. Bu nedenle, bu atama için gereken en geniş kapsamı seçmenizi öneririz.

1. Güvenlik İlkesi sayfasında, özel girişimlerinizin altında **özel girişim Ekle**' ye tıklayın.

    [![Özel girişim Ekle 'ye tıklayın](media/custom-security-policies/custom-policy-add-initiative.png)](media/custom-security-policies/custom-policy-add-initiative.png#lightbox)

    Aşağıdaki sayfa görüntülenir:

    ![İlke oluşturma veya ekleme](media/custom-security-policies/create-or-add-custom-policy.png)

1. Özel girişimler Ekle sayfasında, kuruluşunuzda önceden oluşturulmuş özel ilkelerin listesini gözden geçirin. Aboneliğinize atamak istediğiniz bir tane görürseniz **Ekle**' ye tıklayın. Listede gereksinimlerinizi karşılayan bir girişim yoksa, bu adımı atlayın.

1. Yeni bir özel girişim oluşturmak için:

    1. **Yeni oluştur**' a tıklayın.
    1. Tanımın konumunu ve adını girin.
    1. Dahil edilecek ilkeleri seçin ve **Ekle**' ye tıklayın.
    1. İstediğiniz parametreleri girin.
    1. **Kaydet**’e tıklayın.
    1. Özel girişimler Ekle sayfasında Yenile ' ye tıklayın. Yeni girişim kullanılabilir olarak gösterilir.
    1. **Ekle** ' ye tıklayın ve aboneliğinize atayın.

    > [!NOTE]
    > Yeni girişimler oluşturmak için abonelik sahibi kimlik bilgileri gerekir. Azure rolleri hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'Nde izinler](security-center-permissions.md).

    Yeni girişim devreye girer ve etkiyi aşağıdaki iki yolla görebilirsiniz:

    * Güvenlik Merkezi kenar çubuğundan Ilke & uyumluluğu altında, **mevzuat uyumluluğu**' nu seçin. Uyumluluk panosu, yeni özel girişiminizi yerleşik girişimlerle birlikte göstermek için açılır.
    
    * Ortamınız tanımladığınız ilkeleri izmazsa öneriler almaya başlar.

1. İlkenize yönelik sonuç önerilerini görmek için kenar çubuğundan **öneriler** ' e tıklayarak öneriler sayfasını açın. Öneriler "özel" bir etiketle görüntülenir ve yaklaşık bir saat içinde kullanılabilir.

    [![Özel öneriler](media/custom-security-policies/custom-policy-recommendations.png)](media/custom-security-policies/custom-policy-recommendations-in-context.png#lightbox)

## <a name="enhancing-your-custom-recommendations-with-detailed-information"></a>Ayrıntılı bilgilerle özel önerilerinizi geliştirme

Azure Güvenlik Merkezi ile sağlanan yerleşik öneriler, önem düzeyleri ve Düzeltme yönergeleri gibi ayrıntıları içerir. Bu tür bilgileri özel önerilerinizi eklemek istiyorsanız Azure portal görünmesini veya önerilerinizi her yerden eriştiğinizde, REST API kullanmanız gerekir. 

Ekleyebileceğiniz iki bilgi türü şunlardır:

- **Düzeltici açıklama** – dize
- **Önem derecesi** – enum [düşük, orta, yüksek]

Meta veriler, özel girişim 'nin parçası olan bir ilkenin ilke tanımına eklenmelidir. Aşağıda gösterildiği gibi ' securityCenter ' özelliğinde olması gerekir:

```json
 "metadata": {
    "securityCenter": {
        "RemediationDescription": "Custom description goes here",
        "Severity": "High",
    },
```

Meta veri/securityCenter özelliği de dahil olmak üzere özel ilkeye bir örnek aşağıda verilmiştir:

  ```json
  {
"properties": {
    "displayName": "Security - ERvNet - AuditRGLock",
    "policyType": "Custom",
    "mode": "All",
    "description": "Audit required resource groups lock",
    "metadata": {
        "securityCenter": {
            "remediationDescription": "Resource Group locks can be set via Azure Portal -> Resource Group -> Locks",
            "severity": "High"
        }
    },
    "parameters": {
        "expressRouteLockLevel": {
            "type": "String",
            "metadata": {
                "displayName": "Lock level",
                "description": "Required lock level for ExpressRoute resource groups."
            },
            "allowedValues": [
                "CanNotDelete",
                "ReadOnly"
            ]
        }
    },
    "policyRule": {
        "if": {
            "field": "type",
            "equals": "Microsoft.Resources/subscriptions/resourceGroups"
        },
        "then": {
            "effect": "auditIfNotExists",
            "details": {
                "type": "Microsoft.Authorization/locks",
                "existenceCondition": {
                    "field": "Microsoft.Authorization/locks/level",
                    "equals": "[parameters('expressRouteLockLevel')]"
                }
            }
        }
    }
}
}
  ```

SecurityCenter özelliğini kullanmanın başka bir örneği için [REST API belgelerinin bu bölümüne](https://docs.microsoft.com/rest/api/securitycenter/assessmentsmetadata/createinsubscription#examples)bakın.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel güvenlik ilkeleri oluşturmayı öğrendiniz. 

Diğer ilgili malzemeler için aşağıdaki makalelere bakın: 

- [Güvenlik ilkelerine genel bakış](tutorial-security-policy.md)
- [Yerleşik güvenlik ilkelerinin listesi](security-center-policy-definitions.md)