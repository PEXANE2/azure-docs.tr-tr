---
title: 'Desen: İlke tanımındaki değer işleci'
description: Bu Azure İlkesi deseni, değer işlecinin ilke tanımında nasıl kullanılacağına bir örnek sağlar.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: ace7b7cd4a765cdb8c7aa764b52b180c60508ab2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77172789"
---
# <a name="azure-policy-pattern-the-value-operator"></a>Azure İlkesi deseni: değer işleci

[Değer](../concepts/definition-structure.md#value) işleci [parametreleri,](../concepts/definition-structure.md#parameters) [desteklenen şablon işlevlerini](../concepts/definition-structure.md#policy-functions)veya belirli bir [durum](../concepts/definition-structure.md#conditions)için sağlanan değere literals değerlendirir.

> [!WARNING]
> _Şablon işlevinin_ sonucu bir hataysa, ilke değerlendirmesi başarısız olur. Başarısız bir değerlendirme örtülü bir **inkardır.** Daha fazla bilgi için [şablon hatalarından kaçınma](../concepts/definition-structure.md#avoiding-template-failures)ya da bkz.

## <a name="sample-policy-definition"></a>Örnek ilke tanımı

Bu ilke tanımı, kaynaklara parametre **tagName** _(string)_ olarak belirtilen etiketi ekler veya değiştirir ve kaynağın içinde olduğu kaynak grubundan **tagName** değerini devralır. Bu değerlendirme, kaynak oluşturulduğunda veya güncelleştirildiğinde gerçekleşir. [Değişiklik](../concepts/effects.md#modify) efekti olarak, düzeltme bir [düzeltme görevi](../how-to/remediate-resources.md)aracılığıyla varolan kaynaklarüzerinde çalıştırılabilir.

:::code language="json" source="~/policy-templates/patterns/pattern-value-operator.json":::

### <a name="explanation"></a>Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-value-operator.json" range="20-30" highlight="7,8":::

**Değer** işleci, **properties**içinde blok **varsa policyRule.if** içinde kullanılır. Bu örnekte, [mantıksal işleç](../concepts/definition-structure.md#logical-operators) **allOf** etkisi için her iki koşullu ifadeler doğru olması gerektiğini belirtmek için kullanılır, **değiştirmek**, gerçekleşecek.

**değer,** şablon işlevi [kaynakGrubu()](../../../azure-resource-manager/templates/template-functions-resource.md#resourcegroup) sonucunu boş bir değerin **eşit leri** koşuluna göre değerlendirir. Ana kaynak grubunda **tagName'de** sağlanan etiket adı varsa, koşullu durum doğru olarak değerlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.