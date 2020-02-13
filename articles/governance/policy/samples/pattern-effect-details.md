---
title: 'Model: bir ilke tanımının etkileri'
description: Bu Azure Ilke deseninin farklı etkileri bir ilke tanımının nasıl kullanılacağına ilişkin bir örnek sağlar.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: b86a24bc0af6c9bdd7b29bb0a931d6c78865218b
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77173006"
---
# <a name="azure-policy-pattern-effects"></a>Azure Ilke stili: efektler

Azure Ilkesinde, hizmetin uyumlu olmayan kaynaklar için nasıl yeniden davranması gerektiğini belirten bir dizi [efekt](../concepts/effects.md) vardır. Bazı etkiler basittir ve diğer bazı özellikler gerektirdiğinden ilke tanımında ek özellik gerektirmez.

## <a name="sample-1-simple-effect"></a>Örnek 1: basit efekt

Bu ilke tanımı, değerlendirilen kaynakta, **TagName** parametresinde tanımlanan etiketin mevcut olup olmadığını denetler. Etiket henüz yoksa, etiket etiketi **etiketli değeri olan**etiketi eklemek için [değiştirme](../concepts/effects.md#modify) efekti tetiklenir.

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-1.json":::

### <a name="sample-1-explanation"></a>Örnek 1: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-1.json" range="30-34":::

Bir **değiştirme** etkisi, **roledefinitionıds** ve **işlemlerini**tanımlayan **policyrule. then. Details** bloğunu gerektirir. Bu parametreler, Azure Ilkesine etiketi eklemek ve kaynağı **düzeltmek** ve bu işlemi gerçekleştirmek için hangi rollerin gerekli olduğunu bildirir. Bu örnekte, **işlem** _ekler_ ve bu parametreleri etiketi ve değerini ayarlamak için kullanılır.

## <a name="sample-2-complex-effect"></a>Örnek 2: karmaşık efekt

Bu ilke tanımı, **Yayımcı** ve **tür**parametrelerinde tanımlanan bir uzantı yoksa her sanal makineyi denetler. Tanımlı parametrelerle eşleşen bir örnek olup olmadığını görmek üzere sanal makineyle ilgili bir kaynağı denetlemek için [Auditınotexists](../concepts/effects.md#auditifnotexists) kullanır. Bu örnek, **Uzantılar** türünü denetler.

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-2.json":::

### <a name="sample-2-explanation"></a>Örnek 2: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-2.json" range="45-58":::

Bir **Auditınotexists** efekti, bir **türü** ve aranacak **existenceCondition** tanımlamak için **policyrule. then. Details** bloğunu gerektirir. **ExistenceCondition** , eşleşen bir ilişkili kaynağın mevcut olup olmadığını anlamak için [mantıksal işleçler](../concepts/definition-structure.md#logical-operators)gibi ilke dili öğelerini kullanır. Bu örnekte, her bir [diğer ada](../concepts/definition-structure.md#aliases) karşı denetlenen değerler parametrelerde tanımlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.