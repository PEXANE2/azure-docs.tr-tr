---
title: İlke atama yapısının ayrıntıları
description: İlke tanımlarını ve parametreleri değerlendirme kaynaklarıyla ilişkilendirmek için Azure İlkesi tarafından kullanılan ilke atama tanımını açıklar.
ms.date: 09/23/2019
ms.topic: conceptual
ms.openlocfilehash: f03c654dfc4c8dfdf2bdc5103a5961b4d8ce1e64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265304"
---
# <a name="azure-policy-assignment-structure"></a>Azure İlkesi atama yapısı

İlke atamaları, hangi kaynakların atandığını tanımlamak için Azure İlkesi tarafından kullanılır. İlke ataması, atama sırasında söz söz le o kaynak grubu için parametrelerin değerlerini belirleyerek, aynı kaynak özelliklerini farklı uyumluluk gereksinimlerine yönelik ilke tanımlarının yeniden kullanılmasını mümkün kılar.

Bir ilke ataması oluşturmak için JSON'u kullanırsınız. İlke ataması için aşağıdaki öğeleri içerir:

- görüntü adı
- açıklama
- meta veriler
- uygulama modu
- ilke tanımı
- parametreler

Örneğin, aşağıdaki JSON dinamik parametreleri ile _DoNotEnforce_ modunda bir ilke atama gösterir:

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

Tüm Azure İlkesi örnekleri [Azure İlkesi](../samples/index.md)örneklerindedir.

## <a name="display-name-and-description"></a>Görüntü adı ve açıklama

İlke atamasını tanımlamak ve belirli kaynaklarla kullanımı için bağlam sağlamak için **displayName** ve **açıklama** kullanırsınız. **displayName** en fazla _128_ karakter uzunluğundadır ve en fazla _512_ karakter uzunluğunda açıklama **tanımlar.**

## <a name="enforcement-mode"></a>Uygulama Modu

**enforcementMode** özelliği, müşterilere ilke efektini başlatmadan veya [Azure Etkinliği günlüğündeki](../../../azure-monitor/platform/platform-logs-overview.md)girişleri tetiklemeden bir ilkenin sonucunu varolan kaynaklar üzerinde sınama olanağı sağlar. Bu senaryo genellikle "Ne Olur" olarak adlandırılır ve güvenli dağıtım uygulamalarına hizalanır. **enforcementMode** [Devre Dışı bırakma](./effects.md#disabled) efektinden farklıdır, çünkü bu etki kaynak değerlendirmesinin gerçekleşmesini engeller.

Bu özellik aşağıdaki değerlere sahiptir:

|Mod |JSON Değeri |Tür |El ile düzeltin |Etkinlik günlüğü girişi |Açıklama |
|-|-|-|-|-|-|
|Etkin |Varsayılan |string |Evet |Evet |İlke efekti kaynak oluşturma veya güncelleştirme sırasında zorlanır. |
|Devre dışı |DoNotEnforce |string |Evet |Hayır | İlke efekti kaynak oluşturma veya güncelleştirme sırasında zorlanmaz. |

**EnforcementMode** bir ilke veya girişim tanımında belirtilmemişse, _Varsayılan_ değer kullanılır. [Düzeltme görevleri,](../how-to/remediate-resources.md) **enforcementMode** _DoNotEnforce_olarak ayarlansa [bile, deployIfNotExists](./effects.md#deployifnotexists) ilkeleri için başlatılabilir.

## <a name="policy-definition-id"></a>İlke tanımı kimliği

Bu alan, bir ilke tanımının veya bir girişim tanımının tam yol adı olmalıdır.
`policyDefinitionId`bir dizi değil, bir dizedir. Birden çok ilke genellikle birlikte atanmışsa, bunun yerine bir [girişim](./definition-structure.md#initiatives) kullanılması önerilir.

## <a name="parameters"></a>Parametreler

İlke atamasının bu [kesimi, ilke tanımında veya girişim tanımında](./definition-structure.md#parameters)tanımlanan parametrelerin değerlerini sağlar.
Bu tasarım, bir ilke veya girişim tanımının farklı kaynaklarla yeniden kullanılmasını mümkün kılar, ancak farklı iş değerlerini veya sonuçlarını denetler.

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

Bu örnekte, ilke tanımında daha `prefix` önce `suffix`tanımlanan parametreler ve . Bu özel ilke **ataması DeptA** ve `suffix` **-LC'ye**ayarlar. `prefix` Aynı ilke tanımı, esneklik sağlarken ilke tanımlarının çoğaltılmasını ve karmaşıklığını azaltarak, farklı bir departman için farklı bir parametre kümesiyle yeniden kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [İlke tanımı yapısı](./definition-structure.md)hakkında bilgi edinin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](../how-to/programmatically-create.md)anlayın.
- Uyumluluk verilerini nasıl [alacağınızı](../how-to/get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları](../how-to/remediate-resources.md)nasıl düzelteriz öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.