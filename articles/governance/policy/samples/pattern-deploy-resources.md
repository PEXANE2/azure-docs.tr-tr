---
title: 'Desen: İlke tanımıyla kaynakları dağıtma'
description: Bu Azure İlkesi deseni, ilke tanımıyla kaynakların nasıl dağıtılanacak larına bir örnek sağlar.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: a8b6528afbd21c7c667e48965574c9b48c403654
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77172677"
---
# <a name="azure-policy-pattern-deploy-resources"></a>Azure İlkesi deseni: kaynakları dağıtma

[DeployIfNotExists](../concepts/effects.md#deployifnotexists) efekti, uyumlu olmayan bir kaynak oluştururken veya güncellerken bir [Azure Kaynak Yöneticisi şablonu](../../../azure-resource-manager/templates/overview.md) dağıtmayı mümkün kılar. Bu yaklaşım, kaynakların oluşturulmaya devam etmesine izin verdiği için [reddet](../concepts/effects.md#deny) efektini kullanmak için tercih edilebilir, ancak değişikliklerin uyumlu olmasını sağlamak için yapılmasını sağlar.

## <a name="sample-policy-definition"></a>Örnek ilke tanımı

Bu ilke **field** tanımı, oluşturulan `type` veya güncelleştirilen kaynağıdeğerlendirmek için alan işleci kullanır. Bu kaynak bir _Microsoft.Network/virtualNetworks_olduğunda, ilke yeni veya güncelleştirilmiş kaynağın konumunda bir ağ izleyicisi arar. Eşleşen bir ağ izleyicisi bulunamazsa, eksik kaynağı oluşturmak için Kaynak Yöneticisi şablonu dağıtılır.

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json":::

### <a name="explanation"></a>Açıklama

#### <a name="existencecondition"></a>existenceCondition

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="18-23":::

**Properties.policyRule.then.details block,** Azure İlke'ye **properties.policyRule.if** block'ta oluşturulan veya güncelleştirilen kaynakla ilgili nelere bakması gerektiğini söyler. Bu örnekte, kaynak grubu **ağıWatcherRG'deki** bir ağ izleyicisinin, yeni veya güncelleştirilmiş kaynağın konumuna eşit **alana** `location` sahip olması gerekir. İşlevin `field()` **kullanılması, existenceCondition'ın** yeni veya güncelleştirilmiş `location` kaynaktaki özelliklere, özellikle de özellikteki özelliklere erişmesine olanak tanır.

#### <a name="roledefinitionids"></a>roleDefinitionIds

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="24-26":::

**RoleDefinitionIds** _dizi_ özelliği **properties.policyRule.then.details** block, yönetilen kimliğin eklenmiş Kaynak Yöneticisi şablonuna dağıtmak için hangi hakların olması gerektiğini ilke tanımını söyler. Bu özellik, şablon dağıtımının gerektirdiği izinlere sahip rolleri içerecek şekilde ayarlanmalıdır, ancak 'en az ayrıcalık ilkesi' kavramını kullanmalıdır ve yalnızca gerekli işlemlere sahip olmalıdır ve başka bir şey olmamalıdır.

#### <a name="deployment-template"></a>Dağıtım şablonu

İlke tanımının **dağıtım** bölümünde üç temel bileşeni tanımlayan bir **özellik** bloğu vardır:

- **mod** - Bu özellik şablonun [dağıtım modunu](../../../azure-resource-manager/templates/deployment-modes.md) ayarlar.

- **şablon** - Bu özellik şablonun kendisini içerir. Bu örnekte, **konum** şablonu parametresi yeni ağ izleyicisi kaynağının konumunu ayarlar.

  :::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="30-44":::
  
- **parametreler** - Bu özellik **şablona**sağlanan parametreleri tanımlar. Parametre adları **şablonda**tanımlananla eşleşmelidir. Bu örnekte, parametre eşleşecek **konum** olarak adlandırılır. **Konum** değeri, değerlendirilen kaynağın değerini almak için `field()` işlevi yeniden kullanır, bu da **rule.if** bloğundaki sanal ağdır.

  :::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="45-49":::

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.