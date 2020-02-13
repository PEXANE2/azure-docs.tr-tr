---
title: 'Model: bir ilke tanımındaki parametreler'
description: Bu Azure Ilke modelinde parametrelerin bir ilke tanımında nasıl kullanılacağına ilişkin bir örnek verilmiştir.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: 4921bb216ef67b561bc8986cf48239e6448beafc
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172810"
---
# <a name="azure-policy-pattern-parameters"></a>Azure Ilke stili: parametreler

Bir ilke tanımı, [parametreleri](../concepts/definition-structure.md#parameters)kullanarak gereken ilke tanımlarının sayısını azaltmak için dinamik hale getirilebilir. Parametresi, ilke ataması sırasında tanımlanmıştır. Parametreler, parametreyi ve nasıl kullanıldığını tanımlayan önceden tanımlı özellikler kümesine sahiptir.

## <a name="sample-1-string-parameters"></a>Örnek 1: dize parametreleri

Bu ilke tanımı, ilke atamasının kaynaklar üzerinde ne kadar arama yaptığını ayarlamak için iki parametre, **TagName** ve **tagvalue** kullanır. Bu biçim, ilkenin herhangi bir sayıda etiket adı ve etiket değeri kombinasyonu için kullanılmasına izin verir, ancak yalnızca tek bir ilke tanımını korur.

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-1.json":::

### <a name="sample-1-explanation"></a>Örnek 1: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-1.json" range="8-13":::

İlke tanımının bu bölümünde, **TagName** parametresi bir _dize_ olarak tanımlanır ve kullanımı için bir açıklama sağlanır.

Daha sonra parametresi **Policyrule. If** bloğunda, ilkeyi dinamik hale getirmek için kullanılır. Burada, **TagName**değeri olan bir etiket olan değerlendirilen alanı tanımlamak için kullanılır.

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-1.json" range="22-27" highlight="3":::

## <a name="sample-2-array-parameters"></a>Örnek 2: dizi parametreleri

Bu ilke tanımı, Express Route bağlantı hattı ' nın bant genişliği ayarını onaylanan değerlerden birine yapılandırdığından emin olmak için **listOfBandwidthinMbps**tek bir parametresini kullanır. Eşleşmiyorsa, kaynağa yönelik oluşturma veya güncelleştirme [reddedilir](../concepts/effects.md#deny).

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-2.json":::

### <a name="sample-2-explanation"></a>Örnek 2: Açıklama

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-2.json" range="6-12":::

İlke tanımının bu bölümünde, **listOfBandwidthinMbps** parametresi bir _dizi_ olarak tanımlanır ve kullanımı için bir açıklama sağlanır. Bir _dizi_olarak eşleşmesi için birden çok değer vardır.

Daha sonra parametresi **Policyrule. If** bloğunda kullanılır. Bir _dizi_ parametresi olarak, bir _dizi_
[koşulunun](../concepts/definition-structure.md#conditions)veya **notın** 'in kullanılması gerekir.
Burada, tanımlı değerlerden biri olarak **ServiceProvider. bandwidthInMbps** diğer adı kullanılır.

:::code language="json" source="~/policy-templates/patterns/pattern-parameters-2.json" range="21-24" highlight="3":::

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.