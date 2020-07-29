---
title: İlke atama yapısının ayrıntıları
description: Değerlendirme için kaynaklarla ilke tanımlarını ve parametreleri ilişkilendirmek üzere Azure Ilkesi tarafından kullanılan ilke atama tanımını açıklar.
ms.date: 04/15/2020
ms.topic: conceptual
ms.openlocfilehash: c9f400aa72508822f8fff2fe41fb17ce98339737
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84205050"
---
# <a name="azure-policy-assignment-structure"></a>Azure İlkesi atama yapısı

İlke atamaları, hangi ilkelerin veya girişimlerin hangi kaynaklara atandığını tanımlamak için Azure Ilkesi tarafından kullanılır. İlke ataması, atama zamanında bu kaynak grubu için parametrelerin değerlerini belirleyebilir ve aynı kaynak özelliklerini, uyumluluk için farklı ihtiyaçlarla ele alan ilke tanımlarının yeniden kullanılmasını mümkün kılar.

Bir ilke ataması oluşturmak için JSON kullanırsınız. İlke ataması için öğeleri içerir:

- görünen ad
- açıklama
- meta veriler
- zorlama modu
- Dışlanan kapsamlar
- ilke tanımı
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
        "notScopes": [],
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

**Enforcementmode** özelliği, müşterilere ilke efektini başlatmadan veya [Azure etkinlik günlüğündeki](../../../azure-monitor/platform/platform-logs-overview.md)girişleri tetiklemeden mevcut kaynaklarda bir ilkenin sonucunu test etme yeteneği sağlar. Bu senaryo genellikle "What If" olarak adlandırılır ve güvenli dağıtım uygulamalarına hizalanır. **Enforcementmode** [devre dışı](./effects.md#disabled) etkilerden farklıdır, çünkü bu efekt kaynak değerlendirmesinin hiç oluşmasını önler.

Bu özellik aşağıdaki değerlere sahiptir:

|Mod |JSON değeri |Tür |El ile düzelt |Etkinlik günlüğü girişi |Açıklama |
|-|-|-|-|-|-|
|Etkin |Varsayılan |string |Yes |Evet |İlke etkisi, kaynak oluşturma veya güncelleştirme sırasında zorlanır. |
|Devre dışı |Donotenzorlamalı |string |Evet |Hayır | İlke etkisi, kaynak oluşturma veya güncelleştirme sırasında zorlanmaz. |

İlke veya girişim tanımında **Enforcementmode** belirtilmemişse, _varsayılan_ değer kullanılır. **Enforcementmode** , _Donotenzorlamalı_olarak ayarlandığında bile, dağıtım [görevleri](../how-to/remediate-resources.md) [deployifnotexists](./effects.md#deployifnotexists) ilkeleri için başlatılabilir.

## <a name="excluded-scopes"></a>Dışlanan kapsamlar

Atamanın **kapsamı** tüm alt kaynak kapsayıcılarını ve alt kaynakları içerir. Bir alt kaynak kapsayıcısına veya alt kaynağa tanım uygulanmazsa, **Notscopes**ayarları ayarlanarak her biri değerlendirmeden dışlanabilirler. Bu özellik bir veya daha fazla kaynak kapsayıcısının veya kaynağın değerlendirmesinden DIŞLANMASINI sağlamak için bir dizidir. **Notscopes** , ilk atama oluşturulduktan sonra eklenebilir veya güncelleştirilir.

## <a name="policy-definition-id"></a>İlke tanımı KIMLIĞI

Bu alan, bir ilke tanımının ya da bir girişim tanımının tam yol adı olmalıdır.
`policyDefinitionId`dizi değil bir dizedir. Bunun yerine bir [girişim](./initiative-definition-structure.md) kullanmak için birden çok ilke genellikle birlikte atanırsa, bu önerilir.

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

Bu örnekte, daha önce ilke tanımında tanımlanan parametreler ve ' dir `prefix` `suffix` . Bu ilke ataması `prefix` , **depta** ve `suffix` to **-LC**olarak ayarlanır. Aynı ilke tanımı farklı bir departman için farklı bir parametre kümesiyle yeniden kullanılabilir, ancak esneklik sağlarken ilke tanımlarının çoğaltılmasını ve karmaşıklığını azaltır.

## <a name="next-steps"></a>Sonraki adımlar

- [İlke tanımı yapısı](./definition-structure.md)hakkında bilgi edinin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](../how-to/programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](../how-to/get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](../how-to/remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.