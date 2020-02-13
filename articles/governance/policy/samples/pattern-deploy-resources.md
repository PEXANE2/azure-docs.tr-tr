---
title: 'Model: bir ilke tanımıyla kaynakları dağıtma'
description: Bu Azure Ilke düzeninde, bir ilke tanımıyla kaynakların nasıl dağıtılacağı hakkında bir örnek verilmiştir.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: a8b6528afbd21c7c667e48965574c9b48c403654
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172677"
---
# <a name="azure-policy-pattern-deploy-resources"></a>Azure Ilke deseninin: kaynakları dağıtma

[Deployifnotexists](../concepts/effects.md#deployifnotexists) efekti, uyumlu olmayan bir kaynak oluştururken veya güncelleştirirken bir [Azure Resource Manager şablonu](../../../azure-resource-manager/templates/overview.md) dağıtmayı mümkün hale getirir. Bu yaklaşım, kaynakların oluşturulmasını devam etmesine izin vermek için [reddetme](../concepts/effects.md#deny) efektini kullanmak üzere tercih edilebilir, ancak bunları uyumlu hale getirmek için değişikliklerin yapılmasını sağlar.

## <a name="sample-policy-definition"></a>Örnek ilke tanımı

Bu ilke tanımı, oluşturulan veya güncellenen kaynağın `type` değerlendirmek için **alan** işlecini kullanır. Bu kaynak bir _Microsoft. Network/virtualNetworks_olduğunda, ilke yeni veya güncelleştirilmiş kaynağın konumunda bir ağ izleyicisi arar. Eşleşen bir ağ izleyicisi bulunamıyorsa, eksik kaynağı oluşturmak için Kaynak Yöneticisi şablonu dağıtılır.

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json":::

### <a name="explanation"></a>Açıklama

#### <a name="existencecondition"></a>existenceCondition

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="18-23":::

**Properties. policyRule. then. Details** bloğu, Azure ilkesine **Özellikler. policyrule. If** bloğunda oluşturulan veya güncellenen kaynakla ilgili arama yapılacağını söyler. Bu örnekte, **networkWatcherRG** kaynak grubundaki bir ağ izleyicisi, yeni veya güncelleştirilmiş kaynağın konumuna eşit `location` **alanla** birlikte bulunmalıdır. `field()` işlevi kullanmak, **existenceCondition** 'in yeni veya güncelleştirilmiş kaynaktaki özelliklere, özellikle de `location` özelliğine erişmesini sağlar.

#### <a name="roledefinitionids"></a>Roledefinitionıds

:::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="24-26":::

**Properties. policyRule. then. Details** bloğundaki **roledefinitionıds** _dizi_ özelliği, ilke tanımına, yönetilen kimliğin eklenen kaynak yöneticisi şablonunu dağıtması için gereken hakları söyler. Bu özellik, şablon dağıtımı için gerekli izinlere sahip rolleri içerecek şekilde ayarlanmalıdır, ancak ' en az ayrıcalık ' kavramını kullanması ve yalnızca gerekli işlemleri ve hiçbir şey yapmamanız gerekir.

#### <a name="deployment-template"></a>Dağıtım şablonu

İlke tanımının **dağıtım** bölümü, üç çekirdek bileşeni tanımlayan bir **Özellikler** bloğuna sahiptir:

- **Mode** -bu özellik, şablonun [Dağıtım modunu](../../../azure-resource-manager/templates/deployment-modes.md) ayarlar.

- **şablon** -bu özellik, şablonun kendisini içerir. Bu örnekte, **konum** şablonu parametresi yeni ağ izleyicisi kaynağının konumunu ayarlar.

  :::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="30-44":::
  
- **Parametreler** -bu özellik, **şablona**sunulan parametreleri tanımlar. Parametre adlarının **şablonda**tanımlananla eşleşmesi gerekir. Bu örnekte, parametresi eşleştirilecek **konum** olarak adlandırılır. **Konum** değeri, **policyrule. If** bloğunda sanal ağ olan değerlendirilen kaynağın değerini almak için `field()` işlevini yeniden kullanır.

  :::code language="json" source="~/policy-templates/patterns/pattern-deploy-resources.json" range="45-49":::

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.