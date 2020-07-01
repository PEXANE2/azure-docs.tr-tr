---
title: 'Model: bir ilke tanımının etkileri'
description: Bu Azure Ilke deseninin farklı etkileri bir ilke tanımının nasıl kullanılacağına ilişkin bir örnek sağlar.
ms.date: 06/29/2020
ms.topic: sample
ms.openlocfilehash: 9c7e7689aa837a2dba0992112df3cd36ac9affe6
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85565736"
---
# <a name="azure-policy-pattern-effects"></a>Azure Ilke stili: efektler

Azure Ilkesinde, hizmetin uyumlu olmayan kaynaklar için nasıl yeniden davranması gerektiğini belirten bir dizi [efekt](../concepts/effects.md) vardır. Bazı etkiler basittir ve diğer bazı özellikler gerektirdiğinden ilke tanımında ek özellik gerektirmez.

## <a name="sample-1-simple-effect"></a>Örnek 1: basit efekt

Bu ilke tanımı, değerlendirilen kaynakta, **TagName** parametresinde tanımlanan etiketin mevcut olup olmadığını denetler. Etiket henüz yoksa, etiket etiketi **etiketli değeri olan**etiketi eklemek için [değiştirme](../concepts/effects.md#modify) efekti tetiklenir.

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-1.json":::

### <a name="sample-1-explanation"></a>Örnek 1: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-1.json" range="40-50":::

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