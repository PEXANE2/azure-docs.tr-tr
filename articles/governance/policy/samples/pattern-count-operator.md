---
title: 'Model: bir ilke tanımındaki Count işleci'
description: Bu Azure Ilke deseninin Count işlecinin bir ilke tanımında nasıl kullanılacağına ilişkin bir örnek sağlar.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: 88c2d1083a92732ac56ca4d6da7087cc4220d9a5
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172950"
---
# <a name="azure-policy-pattern-the-count-operator"></a>Azure Ilke stili: Count işleci

[Count](../concepts/definition-structure.md#count) işleci, bir \[\*\] diğer adının üyelerini değerlendirir.

## <a name="sample-policy-definition"></a>Örnek ilke tanımı

Bu ilke tanımı, gelen Uzak Masaüstü Protokolü (RDP) trafiğine izin verecek şekilde yapılandırılmış ağ güvenlik gruplarını [denetler](../concepts/effects.md#audit) .

:::code language="json" source="~/policy-templates/patterns/pattern-count-operator.json":::

### <a name="explanation"></a>Açıklama

**Count** işlecinin temel bileşenleri _alan_, _nerede_ve durumdur. Her biri aşağıdaki kod parçacığında vurgulanır.

- _alan_ , üyelerini değerlendirmek için kullanılacak [diğer ad](../concepts/definition-structure.md#aliases) sayısını belirtir. Burada, ağ güvenlik grubunun \*\]diğer ad _dizisine_ **\[SecurityRules** ' a bakıyoruz.
- , hangi _dizi_ üyelerinin ölçütlere uygun olduğunu tanımlamak için ilke _dilini kullanır._ Bu örnekte, **allof** Logical operator diğer ad _dizisi_ özelliklerinin üç farklı koşul değerlendirmesini gruplandırır: _Direction_, _Access_ve _destinationPortRange_.
- Bu örnekteki sayı koşulu **daha büyüktür**. Diğer ad _dizisinin_ bir veya daha fazla üyesi _WHERE_ yan tümcesiyle eşleştiğinde Count değeri true olarak değerlendirilir.

:::code language="json" source="~/policy-templates/patterns/pattern-count-operator.json" range="12-32" highlight="3,4,20":::

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.