---
title: 'Model: bir ilke tanımındaki alan özellikleri'
description: Bu Azure Ilke düzeninde, alan özelliklerinin bir ilke tanımında nasıl kullanılacağına ilişkin bir örnek verilmiştir.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: e65767dd9cbe7b2192c21f779643289e5a7fc45e
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172866"
---
# <a name="azure-policy-pattern-field-properties"></a>Azure Ilke deseninin: alan özellikleri

[Alan](../concepts/definition-structure.md#fields) işleci belirtilen bir [koşul](../concepts/definition-structure.md#conditions)için belirtilen özelliği veya [diğer adı](../concepts/definition-structure.md#aliases) bir sağlanan değere değerlendirir.

## <a name="sample-policy-definition"></a>Örnek ilke tanımı

Bu ilke tanımı, kuruluşunuzun coğrafi konum gereksinimlerini karşılayan izin verilen bölgeleri tanımlamanızı sağlar. İzin verilen kaynaklar, **Lıfallodilimlocations** (_dizi_) parametresinde tanımlanmıştır. Tanımıyla eşleşen kaynaklar [reddedilir](../concepts/effects.md#deny).

:::code language="json" source="~/policy-templates/patterns/pattern-fields.json":::

### <a name="explanation"></a>Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-fields.json" range="18-36" highlight="3,7,11":::

**Alan** işleci, **allof** [mantıksal işleci](../concepts/definition-structure.md#logical-operators) içinde üç kez kullanılır.

- İlk kullanım, **Notın** koşulunun `location` özelliğini, **Lıfallodilimlocations** parametresine göre değerlendirir. **Notın** , bir _dizi_ beklediği ve parametresi bir _dizi_olduğu için çalışıyor. Oluşturulan veya güncellenen kaynağın `location` onaylanan listede yoksa, bu öğe true olarak değerlendirilir.
- İkinci kullanım `location` özelliğini de değerlendirir, ancak kaynağın _Global_olup olmadığını görmek Için **notquals** koşulunu kullanır. Oluşturulan veya güncelleştirilmiş kaynağın `location` _genel_değilse, bu öğe true olarak değerlendirilir.
- Son kullanım `type` özelliğini değerlendirir ve kaynak türünün _Microsoft. AzureActiveDirectory/b2cDirectories_olmadığını doğrulamak Için **notquals** koşulunu kullanır. Değilse, bu öğe true olarak değerlendirilir.

**Allof** Logical işlecinin tüm üç koşul deyimleri doğru olarak değerlendirilüyorsa, kaynak oluşturma veya güncelleştirme Azure ilkesi tarafından engellenir.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.