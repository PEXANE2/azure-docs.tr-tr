---
title: 'Desen: İlke tanımındaki mantıksal işleçler'
description: Bu Azure İlkesi deseni, mantıksal işleçlerin ilke tanımında nasıl kullanılacağına örnek olarak sunulur.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: 8e57efaea81848c6b2d0188dbf3f91e06ed74c67
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77172852"
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

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.