---
title: 'Desen: İlke tanımının etkileri'
description: Bu Azure İlkesi deseni, ilke tanımının farklı efektlerinin nasıl kullanılacağına örnek sağlar.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: 1a9aec50bd328b76271d54f7830c75e0848d3cde
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80372630"
---
# <a name="azure-policy-pattern-effects"></a>Azure İlkesi deseni: efektler

Azure İlkesi, hizmetin uyumlu olmayan kaynaklara nasıl tepki vereceğini belirleyen bir dizi [efekte](../concepts/effects.md) sahiptir. Bazı efektler basittir ve diğerleri birkaç özellik gerektirirken ilke tanımında ek özellikler gerektirmez.

## <a name="sample-1-simple-effect"></a>Örnek 1: Basit etki

Bu ilke tanımı, parametre etiketinde tanımlanan **etiketin** değerlendirilen kaynakta var olup olmadığını denetler. Etiket henüz yoksa, [değiştirme](../concepts/effects.md#modify) efekti, etiketi parametre **tagValue'deki**değere eklemek için tetiklenir.

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-1.json":::

### <a name="sample-1-explanation"></a>Örnek 1: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-1.json" range="40-50":::

**Bir değişiklik efekti,** **roleDefinitionIds** ve **işlemleri**tanımlayan **ilke Kuralı.then.details** bloğu gerektirir. Bu parametreler, etiketi eklemek ve kaynağı düzeltmek ve gerçekleştirmek için işlemi **değiştirmek** için hangi rollerin gerekli olduğunu Azure Politikası'na bildirir. Bu örnekte, **işlem** _ekle_ ve parametreler etiketi ve değerini ayarlamak için kullanılır.

## <a name="sample-2-complex-effect"></a>Örnek 2: Karmaşık etki

Bu ilke tanımı, **parametreleryayım** ve **türde**tanımlanan bir uzantı olmadığında her sanal makineyi denetler. Tanımlanan parametrelerle eşleşen bir örneğin var olup olmadığını görmek için sanal makineyle ilgili bir kaynağı [denetlemekIfNotExists](../concepts/effects.md#auditifnotexists) kullanır. Bu örnek, **uzantıları** türünü denetler.

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-2.json":::

### <a name="sample-2-explanation"></a>Örnek 2: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-2.json" range="45-58":::

**AuditIfNotExists** etkisi, hem **bir türü** hem de **varoluş Koşulunu** tanımlamak için **ilkkural.then.details** bloğunu gerektirir. **ExistenceCondition,** eşleşen bir ilgili kaynağın var olup olmadığını belirlemek için [mantıksal işleçler](../concepts/definition-structure.md#logical-operators)gibi ilke dili öğelerini kullanır. Bu örnekte, her diğer [ad](../concepts/definition-structure.md#aliases) aleyhine denetlenen değerler parametrelerde tanımlanır.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.