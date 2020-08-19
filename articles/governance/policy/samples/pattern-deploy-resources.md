---
title: 'Model: bir ilke tanımıyla kaynakları dağıtma'
description: Bu Azure Ilke düzeninde, bir ilke tanımıyla kaynakların nasıl dağıtılacağı hakkında bir örnek verilmiştir.
ms.date: 08/17/2020
ms.topic: sample
ms.openlocfilehash: 705c751183a0b71c8628a505f08ef112bca1cbb7
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88545616"
---
# <a name="azure-policy-pattern-deploy-resources"></a>Azure Ilke deseninin: kaynakları dağıtma

[Deployifnotexists](../concepts/effects.md#deployifnotexists) efekti, uyumlu olmayan bir kaynak oluştururken veya güncelleştirirken bir [Azure Resource Manager şablonu](../../../azure-resource-manager/templates/overview.md) (ARM şablonu) dağıtmayı mümkün hale getirir. Bu yaklaşım, kaynakların oluşturulmasını devam etmesine izin vermek için [reddetme](../concepts/effects.md#deny) efektini kullanmak üzere tercih edilebilir, ancak bunları uyumlu hale getirmek için değişikliklerin yapılmasını sağlar.

## <a name="sample-policy-definition"></a>Örnek ilke tanımı

Bu ilke tanımı, **field** `type` oluşturulan veya güncellenen kaynak sayısını değerlendirmek için alan işlecini kullanır. Bu kaynak bir _Microsoft. Network/virtualNetworks_olduğunda, ilke yeni veya güncelleştirilmiş kaynağın konumunda bir ağ izleyicisi arar. Eşleşen bir ağ izleyicisi bulunamıyorsa, eksik kaynağı oluşturmak için ARM şablonu dağıtılır.

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json":::

### <a name="explanation"></a>Açıklama

#### <a name="existencecondition"></a>existenceCondition

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="18-23":::

**Properties. policyRule. then. Details** bloğu, Azure ilkesine **Özellikler. policyrule. If** bloğunda oluşturulan veya güncellenen kaynakla ilgili arama yapılacağını söyler. Bu örnekte, **networkWatcherRG** kaynak grubundaki bir ağ izleyicisi, **field** `location` Yeni veya güncelleştirilmiş kaynağın konumuna eşit alana sahip olmalıdır. İşlevinin kullanılması, `field()` **existenceCondition** 'in yeni veya güncelleştirilmiş kaynaktaki özelliklere, özellikle de özelliğine erişmesini sağlar `location` .

#### <a name="roledefinitionids"></a>Roledefinitionıds

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="24-26":::

**Properties. policyRule. then. Details** bloğundaki **roledefinitionıds** _dizi_ özelliği, ilke tanımına, yönetilen kimliğin eklenen kaynak yöneticisi şablonunu dağıtması için gereken hakları söyler. Bu özellik, şablon dağıtımı için gerekli izinlere sahip rolleri içerecek şekilde ayarlanmalıdır, ancak ' en az ayrıcalık ' kavramını kullanması ve yalnızca gerekli işlemleri ve hiçbir şey yapmamanız gerekir.

#### <a name="deployment-template"></a>Dağıtım şablonu

İlke tanımının **dağıtım** bölümü, üç çekirdek bileşeni tanımlayan bir **Özellikler** bloğuna sahiptir:

- **Mode** -bu özellik, şablonun [Dağıtım modunu](../../../azure-resource-manager/templates/deployment-modes.md) ayarlar.

- **şablon** -bu özellik, şablonun kendisini içerir. Bu örnekte, **konum** şablonu parametresi yeni ağ izleyicisi kaynağının konumunu ayarlar.

  :::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="30-44":::
  
- **Parametreler** -bu özellik, **şablona**sunulan parametreleri tanımlar. Parametre adlarının **şablonda**tanımlananla eşleşmesi gerekir. Bu örnekte, parametresi eşleştirilecek **konum** olarak adlandırılır. **Konum** değeri, `field()` **policyrule. If** bloğunda sanal ağı olan değerlendirilen kaynağın değerini almak için işlevini yeniden kullanır.

  :::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="45-49":::

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.