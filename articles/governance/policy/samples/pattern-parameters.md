---
title: 'Desen: İlke tanımındaki parametreler'
description: Bu Azure İlkesi deseni, ilke tanımında parametrelerin nasıl kullanılacağına bir örnek sağlar.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: 4921bb216ef67b561bc8986cf48239e6448beafc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77172810"
---
# <a name="azure-policy-pattern-parameters"></a>Azure İlkesi deseni: parametreler

[Parametreler](../concepts/definition-structure.md#parameters)kullanılarak gereken ilke tanımlarının sayısını azaltmak için dinamik bir ilke tanımı yapılabilir. Parametre ilke ataması sırasında tanımlanır. Parametreler, parametreyi ve nasıl kullanıldığını açıklayan önceden tanımlanmış özellikler kümesine sahiptir.

## <a name="sample-1-string-parameters"></a>Örnek 1: String parametreleri

Bu ilke tanımı, ilke atamasının kaynaklarda ne aradığını ayarlamak için **tagName** ve **tagValue** olmak üzere iki parametre kullanır. Bu biçim, ilkenin herhangi bir sayıda etiket adı ve etiket değeri birleşimleri için kullanılmasına izin verir, ancak yalnızca tek bir ilke tanımını korur.

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-1.json":::

### <a name="sample-1-explanation"></a>Örnek 1: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-1.json" range="8-13":::

İlke tanımının bu bölümünde, **tagName** parametresi bir _dize_ olarak tanımlanır ve kullanımı için bir açıklama sağlanır.

Parametre daha sonra **ilke kuralında kullanılır.eğer** ilkeyi dinamik yapmak için. Burada, **etiket Adı**değerine sahip bir etiket olan değerlendirilen alanı tanımlamak için kullanılır.

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-1.json" range="22-27" highlight="3":::

## <a name="sample-2-array-parameters"></a>Örnek 2: Dizi parametreleri

Bu ilke tanımı, Express Route Circuit kaynağının bant genişliği ayarını onaylanmış değerlerden birine yapılandırıp yapılandırmadığını kontrol etmek için tek bir parametre, **listOfBandwidthinMbps**kullanır. Eşleşmiyorsa, oluşturma veya kaynağa güncelleştirme [reddedilir.](../concepts/effects.md#deny)

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-2.json":::

### <a name="sample-2-explanation"></a>Örnek 2: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-2.json" range="6-12":::

İlke tanımının bu bölümünde, **listOfBandwidthinMbps** parametresi bir _dizi_ olarak tanımlanır ve kullanımı için bir açıklama sağlanır. Bir _dizi_olarak, eşleşecek birden çok değeri vardır.

Parametre daha sonra **rule.if** bloğunda kullanılır. _Dizi_ parametresi olarak, bir _dizi_
[koşulu](../concepts/definition-structure.md#conditions)'s **veya** **notIn** kullanılmalıdır.
Burada, tanımlanan değerlerden biri olarak **serviceProvider.bandwidthInMbps** takma karşı kullanılır.

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-2.json" range="21-24" highlight="3":::

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.