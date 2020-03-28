---
title: 'Desen: İlke tanımındaki alan özellikleri'
description: Bu Azure İlkesi deseni, alan özelliklerinin ilke tanımında nasıl kullanılacağına bir örnek sağlar.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: e65767dd9cbe7b2192c21f779643289e5a7fc45e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77172866"
---
# <a name="azure-policy-pattern-field-properties"></a>Azure İlkesi deseni: alan özellikleri

[Alan](../concepts/definition-structure.md#fields) işleci, belirtilen özelliği veya [diğer adı](../concepts/definition-structure.md#aliases) belirli bir [koşul](../concepts/definition-structure.md#conditions)için sağlanan değere değerlendirir.

## <a name="sample-policy-definition"></a>Örnek ilke tanımı

Bu ilke tanımı, kuruluşunuzun coğrafi konum gereksinimlerini karşılayan izin verilen bölgeleri tanımlamanızı sağlar. İzin verilen kaynaklar parametre **listesinde tanımlanırOfAllowedLocations** _(dizi)._ Tanımla eşleşen kaynaklar [reddedilir.](../concepts/effects.md#deny)

:::code language="json" source="~/policy-templates/patterns/pattern-fields.json":::

### <a name="explanation"></a>Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-fields.json" range="18-36" highlight="3,7,11":::

**Alan** işleci [mantıksal işleç](../concepts/definition-structure.md#logical-operators) **allOf**içinde üç kez kullanılır.

- İlk kullanım da `location` **listOfAllowedLocations** parametrenotIn koşulu ile özelliği değerlendirir. **notIn** bir _dizi_ beklediği gibi **notIn** çalışır ve parametre bir _dizidir._ Oluşturulan `location` veya güncelleştirilen kaynağın onaylı listesinde değilse, bu öğe doğru olarak değerlendirilir.
- İkinci kullanım da `location` özelliği değerlendirir, ancak kaynak _genel_olup olmadığını görmek için **notEquals** koşulunu kullanır. Oluşturulan `location` veya güncelleştirilen kaynağın _geneli_genel değilse, bu öğe doğru olarak değerlendirilir.
- Son kullanım `type` özelliği değerlendirir ve kaynak türünü doğrulamak için **notEquals** koşulunu kullanır _Microsoft.AzureActiveDirectory/b2cDirectories_değildir. Değilse, bu öğe doğru değerlendirir.

**allOf** mantıksal işlecindeki üç koşul deyiminin tümü doğru olarak değerlendirilirse, kaynak oluşturma veya güncelleştirme Azure İlkesi tarafından engellenir.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.