---
title: 'Öğretici: Etiket yönetimini yönetme'
description: Bu öğreticide, yeni ve varolan kaynaklar üzerinde bir etiket yönetim modeli oluşturmak ve uygulamak için Azure İlkesini Değiştir efektini kullanırsınız.
ms.date: 04/21/2020
ms.topic: tutorial
ms.openlocfilehash: 6319bbde2fdc8f78e2743dd5f1565c8680433fea
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759072"
---
# <a name="tutorial-manage-tag-governance-with-azure-policy"></a>Öğretici: Azure İlkesi ile etiket yönetimini yönetme

[Etiketler,](../../../azure-resource-manager/management/tag-resources.md) Azure kaynaklarınızı taksonomi olarak düzenlemenin önemli bir parçasıdır. Etiket [yönetimi için en iyi uygulamaları](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#naming-and-tagging-resources)takip ederken, etiketler iş ilkelerinizi Azure İlkesi ile uygulamak veya Maliyet Yönetimi ile maliyetleri [izlemek](../../../cost-management-billing/costs/cost-mgt-best-practices.md#organize-and-tag-your-resources)için temel oluşturabilir.
Etiketleri nasıl veya neden kullanırsanız kullanın, Azure kaynaklarınızda bu etiketleri hızla eklemeniz, değiştirmeniz ve kaldırmanız önemlidir. Azure kaynağınızın etiketlemeyi destekleyip desteklemedığını görmek için [Etiket desteğine](../../../azure-resource-manager/management/tag-support.md)bakın.

Azure İlkesi'nin [Değiştir](../concepts/effects.md#modify) efekti, kaynak yönetiminin hangi aşamasında olursanız olun etiketlerin yönetimine yardımcı olmak üzere tasarlanmıştır. **Değiştir:**

- Bulutta yenisiniz ve etiket yönetimi niz yok
- Zaten hiçbir etiket yönetimi ile binlerce kaynak var
- Zaten değiştirilmesi gereken mevcut bir taksonomi var

Bu öğreticide, aşağıdaki görevleri tamamlayasınız:

> [!div class="checklist"]
> - İş gereksinimlerinizi belirleyin
> - Her gereksinimi bir ilke tanımıyla eşle
> - Etiket politikalarını bir girişimde gruplandırma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliğinizin olması gerekir. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="identify-requirements"></a>Gereksinimleri belirleme

Yönetim kontrollerinin iyi bir şekilde uygulanması gibi, gereksinimler de iş gereksinimlerinize gelmeli ve teknik kontroller oluşturmadan önce iyi anlaşılmalıdır. Bu senaryo öğreticisi için, aşağıdaki öğeler iş gereksinimlerimizdir:

- Tüm kaynaklarda iki gerekli etiketleri: _CostCenter_ ve _Env_
- _CostCenter_ tüm kapsayıcılar ve bireysel kaynaklar üzerinde olmalıdır
  - Kaynaklar içinde oldukları kapsayıcıdan devralır, ancak tek tek geçersiz kılınabilir
- _Env_ tüm kaplar ve bireysel kaynaklar üzerinde var olmalıdır
  - Kaynaklar, kapsayıcı adlandırma şemasına göre ortamı belirler ve geçersiz kılınmayabilir
  - Bir kapsayıcıdaki tüm kaynaklar aynı ortamın parçasıdır

## <a name="configure-the-costcenter-tag"></a>CostCenter etiketini yapılandırma

Azure İlkesi tarafından yönetilen bir Azure ortamına özgü olarak, _CostCenter_ etiket gereksinimleri aşağıdakileri çağırır:

- _CostCenter_ etiketini eksik kaynak gruplarını reddetme
- Eksik olduğunda ana kaynak grubundan _CostCenter_ etiketini eklemek için kaynakları değiştirme

### <a name="deny-resource-groups-missing-the-costcenter-tag"></a>CostCenter etiketini eksik kaynak gruplarını reddetme

Kaynak grubu için _CostCenter_ kaynak grubunun adıyla belirlenemediğinden, kaynak grubu oluşturmak için istekte etiketin tanımlanması gerekir. [Reddet](../concepts/effects.md#deny) efekti ile aşağıdaki ilke _kuralı, CostCenter_ etiketine sahip olmayan kaynak gruplarının oluşturulmasını veya güncelleştirilmesini engeller:

```json
"if": {
    "allOf": [{
            "field": "type",
            "equals": "Microsoft.Resources/subscriptions/resourceGroups"
        },
        {
            "field": "tags['CostCenter']",
            "exists": false
        }
    ]
},
"then": {
    "effect": "deny"
}
```

> [!NOTE]
> Bu ilke kuralı bir kaynak grubunu hedefalişe işaret ettiği için, ilke tanımındaki _mod_ 'Dizine Eklenmiş' yerine 'Tümü' olmalıdır.

### <a name="modify-resources-to-inherit-the-costcenter-tag-when-missing"></a>Eksik olduğunda CostCenter etiketini devralmak için kaynakları değiştirme

İkinci _CostCenter_ gereksinimi, herhangi bir kaynağın etiketi eksik olduğunda ana kaynak grubundan devralmasıdır. Etiket kaynakta zaten tanımlanmışsa, ana kaynak grubundan farklı olsa bile, tek başına bırakılmalıdır. Aşağıdaki ilke kuralı [değiştir'i](../concepts/effects.md#modify)kullanır:

```json
"policyRule": {
    "if": {
        "field": "tags['CostCenter']",
        "exists": "false"
    },
    "then": {
        "effect": "modify",
        "details": {
            "roleDefinitionIds": [
                "/providers/microsoft.authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
            ],
            "operations": [{
                "operation": "add",
                "field": "tags['CostCenter']",
                "value": "[resourcegroup().tags['CostCenter']]"
            }]
        }
    }
}
```

Bu ilke kuralı, varolan kaynakları [düzeltirken](../how-to/remediate-resources.md) varsa etiket değerini değiştirmek istemediğimiz için **addOrReplace** yerine **ekle** işlemini kullanır. Ayrıca, ana `[resourcegroup()]` kaynak grubundan etiket değerini almak için şablon işlevini kullanır.

> [!NOTE]
> Bu ilke kuralı etiketleri destekleyen kaynakları hedefleolduğundan, ilke tanımındaki _mod_ 'Dizinlenmiş' olmalıdır. Bu yapılandırma, bu ilkenin kaynak gruplarını atlamasını da sağlar.

## <a name="configure-the-env-tag"></a>Env etiketini yapılandırma

Azure İlkesi tarafından yönetilen bir Azure ortamına özgü olarak, _Env_ etiket gereksinimleri aşağıdakileri çağırır:

- Kaynak grubunun adlandırma düzenini temel alan kaynak grubundaki _Env_ etiketini değiştirme
- Kaynak grubundaki tüm kaynaklardaki _Env_ etiketini ana kaynak grubuyla aynı şekilde değiştirme

### <a name="modify-resource-groups-env-tag-based-on-name"></a>Kaynak gruplarını ada göre Env etiketini değiştirme

Azure ortamınızda bulunan her ortam için [Değişiklik](../concepts/effects.md#modify) ilkesi gereklidir. Her biri için Değiştir ilkesi şu ilke tanımına benzer:

```json
"policyRule": {
    "if": {
        "allOf": [{
            "field": "type",
            "equals": "Microsoft.Resources/subscriptions/resourceGroups"
        },
        {
            "field": "name",
            "like": "prd-*"
        }
    ]
    },
    "then": {
        "effect": "modify",
        "details": {
            "roleDefinitionIds": [
                "/providers/microsoft.authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
            ],
            "operations": [{
                "operation": "addOrReplace",
                "field": "tags['Env']",
                "value": "Production"
            }]
        }
    }
}
```

> [!NOTE]
> Bu ilke kuralı bir kaynak grubunu hedefalişe işaret ettiği için, ilke tanımındaki _mod_ 'Dizine Eklenmiş' yerine 'Tümü' olmalıdır.

Bu ilke yalnızca kaynak gruplarını üretim kaynakları `prd-`için kullanılan örnek adlandırma şemasıyla eşleşir. Daha karmaşık adlandırma şeması, bu örnekteki **gibi** tek yerine birkaç **maç** koşuluyla elde edilebilir.

### <a name="modify-resources-to-inherit-the-env-tag"></a>Env etiketini devralmak için kaynakları değiştirme

İş gereksinimi, tüm kaynakların ana kaynak grubunun yaptığı _Env_ etiketine sahip olmasını gerektirir. Bu etiket geçersiz kılınamaz, bu nedenle [değiştir](../concepts/effects.md#modify) efektiyle **addOrReplace** işlemini kullanırız. Örnek Değiştir ilkesi aşağıdaki kurala benzer:

```json
"policyRule": {
    "if": {
        "anyOf": [{
            "field": "tags['Env']",
            "notEquals": "[resourcegroup().tags['Env']]"
        },
        {
            "field": "tags['Env']",
            "exists": false
        }
    ]
    },
    "then": {
        "effect": "modify",
        "details": {
            "roleDefinitionIds": [
                "/providers/microsoft.authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
            ],
            "operations": [{
                "operation": "addOrReplace",
                "field": "tags['Env']",
                "value": "[resourcegroup().tags['Env']]"
            }]
        }
    }
}
```

> [!NOTE]
> Bu ilke kuralı etiketleri destekleyen kaynakları hedefleolduğundan, ilke tanımındaki _mod_ 'Dizinlenmiş' olmalıdır. Bu yapılandırma, bu ilkenin kaynak gruplarını atlamasını da sağlar.

Bu ilke kuralı, _Env_ etiketi için ana kaynak grupları değeri olmayan veya _Env_ etiketini eksik olan tüm kaynakları arar. Eşleşen kaynakların _Env_ etiketi, kaynaküzerinde zaten var olsa da farklı bir değere sahip olsa bile, ana kaynak grupları değerine ayarlanmıştır.

## <a name="assign-the-initiative-and-remediate-resources"></a>Girişimi atama ve kaynakları düzeltme

Yukarıdaki etiket ilkeleri oluşturulduktan sonra, etiket yönetimi için bunları tek bir girişimde birleştirin ve bunları bir yönetim grubuna veya aboneye atayın. Girişim ve dahil ilkeler daha sonra varolan kaynakların uyumluluğunu değerlendirir ve ilke kuralındaki **if** özelliğiyle eşleşen yeni veya güncelleştirilmiş kaynaklara yönelik istekleri değiştirir. Ancak, ilke, tanımlanmış etiket değişiklikleriyle varolan uyumlu olmayan kaynakları otomatik olarak güncelleştirmez.

[DeployIfNotExists](../concepts/effects.md#deployifnotexists) ilkeleri gibi, **Değiştir** ilkesi varolan uyumlu olmayan kaynakları değiştirmek için düzeltme görevlerini kullanır. Uyumlu olmayan [Kaynaklarınızı](../how-to/remediate-resources.md) **değiştirmek** ve etiketleri tanımlanmış taksonominize göre düzeltmek için kaynakları nasıl düzeltin yönergeleri izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticideki kaynaklarla çalışmayı bitirdiyseniz, yukarıda oluşturulan atamaları veya tanımları silmek için aşağıdaki adımları kullanın:

1. Azure İlkesi sayfasının sol tarafında **Yazma** altında **Tanımlar** 'ı (veya bir atamayı silmeye çalışıyorsanız **Atamalar)** seçin.

1. Kaldırmak istediğiniz yeni girişim veya tanımını (ya da atamayı) arayın.

1. Satıra sağ tıklayın ya da tanımın (veya atamanın) sonundaki üç noktayı seçip **Tanımı sil** (veya **Atamayı sil**) öğesini seçin.

## <a name="review"></a>Gözden geçirme

Bu öğreticide, aşağıdaki görevleri öğrendiniz:

> [!div class="checklist"]
> - İş gereksinimlerinizi belirleme
> - Her gereksinimi bir ilke tanımına göre eşledi
> - Etiket politikalarını bir girişim olarak gruplandırın

## <a name="next-steps"></a>Sonraki adımlar

İlke tanımlarının yapıları hakkında daha fazla bilgi edinmek için şu makaleyi gözden geçirin:

> [!div class="nextstepaction"]
> [Azure İlkesi tanım yapısı](../concepts/definition-structure.md)