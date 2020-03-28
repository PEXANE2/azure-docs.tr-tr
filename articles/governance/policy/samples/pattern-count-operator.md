---
title: 'Desen: İlke tanımındaki sayı işleci'
description: Bu Azure İlkesi deseni, bir ilke tanımında sayım işlecinin nasıl kullanılacağına bir örnek sağlar.
ms.date: 01/31/2020
ms.topic: sample
ms.openlocfilehash: 88c2d1083a92732ac56ca4d6da7087cc4220d9a5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77172950"
---
# <a name="azure-policy-pattern-the-count-operator"></a>Azure İlkesi deseni: sayım işleci

[Sayım](../concepts/definition-structure.md#count) işleci, diğer \[ \* \] adın üyelerini değerlendirir.

## <a name="sample-policy-definition"></a>Örnek ilke tanımı

Bu ilke tanımı, gelen Uzak Masaüstü Protokolü (RDP) trafiğine izin verecek şekilde yapılandırılan Ağ Güvenlik Gruplarını [denetler.](../concepts/effects.md#audit)

:::code language="json" source="~/policy-templates/patterns/pattern-count-operator.json":::

### <a name="explanation"></a>Açıklama

**Sayım** işlecinin temel bileşenleri _alan,_ _nerede_ve durumdur. Her biri aşağıdaki snippet vurgulanır.

- _alan_ üyeleri değerlendirmek için hangi [takma saymak](../concepts/definition-structure.md#aliases) söyler. Burada, ağ güvenlik grubunun **güvenlik\[ \* kuralları** diğer ad _dizisine_ bakıyoruz.
- _hangi_ _dizi_ üyelerinin ölçütleri karşılamasını tanımlamak için ilke dilini kullanır. Bu örnekte, **allOf** mantıksal işleç grupları diğer ad _dizi_ özellikleriüç farklı durum değerlendirmeleri: _yön_, _erişim_, ve _hedefPortRange_.
- Bu örnekteki sayım koşulu **daha büyüktür.** Diğer ad _dizisinin_ bir veya daha fazla üyesi _where_ yan tümcesi ile eşleştiğinde Kont doğru olarak değerlendirir.

:::code language="json" source="~/policy-templates/patterns/pattern-count-operator.json" range="12-32" highlight="3,4,20":::

## <a name="next-steps"></a>Sonraki adımlar

- Diğer [desenleri ve yerleşik tanımları](./index.md)gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.