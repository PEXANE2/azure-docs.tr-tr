---
title: 'Model: bir ilke tanımındaki mantıksal işleçler'
description: Bu Azure Ilke modelinde, mantıksal işleçlerin bir ilke tanımında nasıl kullanılacağına ilişkin örnekler verilmektedir.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: 8e57efaea81848c6b2d0188dbf3f91e06ed74c67
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172852"
---
# <a name="azure-policy-pattern-logical-operators"></a>Azure Ilke stili: mantıksal işleçler

Bir ilke tanımı, birkaç koşullu deyim içerebilir. Her deyimin doğru olması veya yalnızca bazılarının doğru olması gerekir. Bu ihtiyaçları desteklemek için, dilin [mantıksal işleçleri](../concepts/definition-structure.md#logical-operators) **değildir**, **allof**ve **anyfor**. Bunlar isteğe bağlıdır ve karmaşık senaryolar oluşturmak için iç içe olabilir.

## <a name="sample-1-one-logical-operator"></a>Örnek 1: bir mantıksal işleç

Bu ilke tanımı, otomatik yük devretme ve birden fazla yazma konumu yapılandırılıp yapılandırılmadığını görmek için CosmosDB hesaplarını değerlendirir. Olmadığında [Denetim](../concepts/effects.md#audit) tetiklenir ve uyumlu olmayan kaynak oluşturulduğunda veya güncelleştirilirken bir günlük girişi oluşturur.

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-1.json":::

### <a name="sample-1-explanation"></a>Örnek 1: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-1.json" range="6-22" highlight="3":::

**Policyrule. If** bloğu, üç koşulun de doğru olmasını sağlamak için tek bir **allof** kullanır.
Yalnızca bu koşulların tümü doğru olarak değerlendirilse **Denetim** efekti tetikler.

## <a name="sample-2-multiple-logical-operators"></a>Örnek 2: birden çok mantıksal işleçler

Bu ilke tanımı, bir adlandırma deseninin kaynaklarını değerlendirir. Bir kaynak eşleşmezse, [reddedilir](../concepts/effects.md#deny).

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-2.json":::

### <a name="sample-2-explanation"></a>Örnek 2: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-2.json" range="7-21" highlight="2,3,9":::

Bu **Policyrule. If** bloğu tek bir **allof**de içeriyorsa, ancak her koşul mantıksal **olmayan** işleçle sarmalanır. **Not** Logical işlecinin içindeki koşul ilk olarak değerlendirilir ve sonra yan tümcesinin doğru mi yoksa yanlış mi olduğunu belirlememe sonucunu **vermez** . Her iki mantıksal işleç de true olarak **değerlendirilmiyorsa,** ilke efekti tetikler.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.