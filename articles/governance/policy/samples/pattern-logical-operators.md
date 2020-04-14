---
title: 'Desen: İlke tanımındaki mantıksal işleçler'
description: Bu Azure İlkesi deseni, mantıksal işleçlerin ilke tanımında nasıl kullanılacağına örnek olarak sunulur.
ms.date: 04/15/2020
ms.topic: sample
ms.openlocfilehash: 691383b1f8ae34bbd51ce7f4f9310980e3c66537
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81272517"
---
# <a name="azure-policy-pattern-logical-operators"></a>Azure İlkesi deseni: mantıksal işleçler

İlke tanımı birkaç koşullu deyim içerebilir. Her ifadenin doğru olması gerekebilir veya bunların yalnızca bazılarının doğru olması gerekebilir. Bu ihtiyaçları desteklemek için, dil **değil**için [mantıksal işleçleri](../concepts/definition-structure.md#logical-operators) vardır , **allOf**, ve **anyOf**. İsteğe bağlıdırlar ve karmaşık senaryolar oluşturmak için iç içe olabilir.

## <a name="sample-1-one-logical-operator"></a>Örnek 1: Bir mantıksal işleç

Bu ilke tanımı, otomatik arızaların ve birden çok yazma yerinin yapılandırılıp yapılmaması için CosmosDB hesaplarını değerlendirir. Bunlar olmadığında, [denetim,](../concepts/effects.md#audit) uyumlu olmayan kaynak oluşturulduğunda veya güncelleştirildiğinde bir günlük girişi tetikler ve oluşturur.

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-1.json":::

### <a name="sample-1-explanation"></a>Örnek 1: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-1.json" range="6-22" highlight="3":::

**İlke Kural.eğer** blok üç koşulun da doğru olduğundan emin olmak için tek bir **allOf** kullanırsa.
Yalnızca tüm bu koşullar doğru değerlendirildiğinde **denetim** efekti tetiklemektedir.

## <a name="sample-2-multiple-logical-operators"></a>Örnek 2: Birden çok mantıksal işleç

Bu ilke tanımı, bir adlandırma deseni için kaynakları değerlendirir. Bir kaynak eşleşmiyorsa, [reddedilir.](../concepts/effects.md#deny)

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-2.json":::

### <a name="sample-2-explanation"></a>Örnek 2: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-2.json" range="7-21" highlight="2,3,9":::

Bu **policyRule.if** bloğu da tek bir **allOf**içerir, ancak her koşul mantıksal **işleç** ile sarılır. Mantıksal **olmayan** işleci içinde koşullu önce değerlendirir ve daha sonra tüm yan tümcedoğru veya yanlış olup olmadığını belirlemek için **değil** değerlendirir. **Her** iki mantıksal işleç de doğru değerlendirmek, ilke etkisi tetikler.

## <a name="sample-3-combining-logical-operators"></a>Örnek 3: Mantıksal işleçleri birleştirme

Bu ilke tanımı, izlemenin etkin olup olmadığını veya izlemenin başarılı bir durumda olup olmadığını görmek için Java Spring hesaplarını değerlendirir.

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-3.json":::

### <a name="sample-3-explanation"></a>Örnek 3: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-logical-operators-3.json" range="6-28" highlight="3,8":::

Bu **ilke Kuralı.if** bloğu hem **allOf'u** hem de **tüm** mantıksal işleçleri içerir. **AnyOf** mantıksal işleci, dahil edilen bir koşul doğru olduğu sürece doğru değerlendirir. _Türü_ **allOf**özünde olduğu gibi, her zaman doğru değerlendirmek gerekir. **AnyOf'daki** _koşullardan_ biri ve türü doğruysa, ilke efekti tetikler.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.