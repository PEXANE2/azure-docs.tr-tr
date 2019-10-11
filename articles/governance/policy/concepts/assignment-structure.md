---
title: İlke atama yapısının ayrıntıları
description: Değerlendirme için kaynaklarla ilke tanımlarını ve parametreleri ilişkilendirmek üzere Azure Ilkesi tarafından kullanılan ilke atama tanımını açıklar.
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/23/2019
ms.topic: conceptual
ms.service: azure-policy
ms.openlocfilehash: a75c64ebb6ba3eeffeccd98cf41365fe96218573
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72255900"
---
# <a name="azure-policy-assignment-structure"></a>Azure Ilke atama yapısı

İlke atamaları, ilkeler veya girişimler sırasında hangi kaynakların atandığını tanımlamak için Azure Ilkesi tarafından kullanılır. İlke ataması, atama zamanında bu kaynak grubu için parametrelerin değerlerini belirleyebilir ve aynı kaynak özelliklerini, uyumluluk için farklı ihtiyaçlarla ele alan ilke tanımlarının yeniden kullanılmasını mümkün kılar.

Bir ilke ataması oluşturmak için JSON kullanırsınız. İlke ataması için öğeleri içerir:

- görünen ad
- açıklama
- meta veriler
- zorlama modu
- İlke tanımı
- parametreler

Örneğin, aşağıdaki JSON, _Donotenzorlama_ modunda dinamik parametrelerle bir ilke atamasını göstermektedir:

```json
{
    "properties": {
        "displayName": "Enforce resource naming rules",
        "description": "Force resource names to begin with DeptA and end with -LC",
        "metadata": {
            "assignedBy": "Cloud Center of Excellence"
        },
        "enforcementMode": "DoNotEnforce",
        "policyDefinitionId": "/subscriptions/{mySubscriptionID}/providers/Microsoft.Authorization/policyDefinitions/ResourceNaming",
        "parameters": {
            "prefix": {
                "value": "DeptA"
            },
            "suffix": {
                "value": "-LC"
            }
        }
    }
}
```

Tüm Azure Ilke örnekleri [Azure ilke örneklerimizle](../samples/index.md).

## <a name="display-name-and-description"></a>Görünen ad ve açıklama

**DisplayName** ve **Description** kullanarak ilke atamasını tanımlayabilir ve belirli bir kaynak kümesiyle kullanımı için bağlam sağlayabilirsiniz. **DisplayName** , en fazla _128_ karakter uzunluğunda ve en fazla _512_ karakter uzunluğunda bir **Açıklama** içeriyor.

## <a name="enforcement-mode"></a>Zorlama modu

**Enforcementmode** özelliği, müşterilere ilke efektini başlatmadan veya [Azure etkinlik günlüğündeki](../../../azure-monitor/platform/activity-logs-overview.md)girişleri tetiklemeden mevcut kaynaklarda bir ilkenin sonucunu test etme yeteneği sağlar. Bu senaryo genellikle "What If" olarak adlandırılır ve güvenli dağıtım uygulamalarına hizalanır. **Enforcementmode** [devre dışı](./effects.md#disabled) etkilerden farklıdır, çünkü bu efekt kaynak değerlendirmesinin hiç oluşmasını önler.

Bu özellik aşağıdaki değerlere sahiptir:

|Mod |JSON değeri |Tür |El ile düzelt |Etkinlik günlüğü girişi |Açıklama |
|-|-|-|-|-|-|
|Etkin |Varsayılan |dize |Evet |Evet |İlke etkisi, kaynak oluşturma veya güncelleştirme sırasında zorlanır. |
|Devre dışı |Donotenzorlamalı |dize |Evet |Hayır | İlke etkisi, kaynak oluşturma veya güncelleştirme sırasında zorlanmaz. |

İlke veya girişim tanımında **Enforcementmode** belirtilmemişse, _varsayılan_ değer kullanılır. **Enforcementmode** , _Donotenzorlamalı_olarak ayarlandığında bile, dağıtım [görevleri](../how-to/remediate-resources.md) [deployifnotexists](./effects.md#deployifnotexists) ilkeleri için başlatılabilir.

## <a name="policy-definition-id"></a>İlke tanımı KIMLIĞI

Bu alan, bir ilke tanımının ya da bir girişim tanımının tam yol adı olmalıdır.
`policyDefinitionId` bir diziyse dize değil. Bunun yerine bir [girişim](./definition-structure.md#initiatives) kullanmak için birden çok ilke genellikle birlikte atanırsa, bu önerilir.

## <a name="parameters"></a>Parametreler

İlke atamasının bu segmenti, [ilke tanımı veya girişim tanımında](./definition-structure.md#parameters)tanımlanan parametrelerin değerlerini sağlar.
Bu tasarım, farklı kaynaklarla bir ilke veya girişim tanımını yeniden kullanmayı mümkün kılar, ancak farklı iş değerlerini veya sonuçlarını denetleyebilir.

```json
"parameters": {
    "prefix": {
        "value": "DeptA"
    },
    "suffix": {
        "value": "-LC"
    }
}
```

Bu örnekte, ilke tanımında daha önce tanımlanan parametreler `prefix` ve `suffix` ' dir. Bu ilke ataması `prefix` ' dan **Deptası** ve `suffix` **-LC**' i belirler. Aynı ilke tanımı farklı bir departman için farklı bir parametre kümesiyle yeniden kullanılabilir, ancak esneklik sağlarken ilke tanımlarının çoğaltılmasını ve karmaşıklığını azaltır.

## <a name="next-steps"></a>Sonraki adımlar

- [İlke tanımı yapısı](./definition-structure.md)hakkında bilgi edinin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](../how-to/programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](../how-to/getting-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](../how-to/remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.