---
title: Kaynakları etiketleme ilkeleri
description: Etiket uyumluluğunu sağlamak için atayabileceğiniz Azure Ilkelerini açıklar.
ms.topic: conceptual
ms.date: 03/20/2020
ms.openlocfilehash: e3eeb28ea23b18c3492f68d2fac294fc014420c5
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82147859"
---
# <a name="assign-policies-for-tag-compliance"></a>Etiket uyumluluğu için ilke atama

Etiketleme kuralları ve kurallarını zorlamak için [Azure ilkesi](../../governance/policy/overview.md) 'ni kullanırsınız. Bir ilke oluşturarak, kuruluşunuz için beklenen etiketlere sahip olmayan, aboneliğinize dağıtılan kaynakların senaryosundan kaçınabilirsiniz. Etiketlerin el ile uygulanması veya uyumlu olmayan kaynakları aramak yerine, dağıtım sırasında gerekli etiketleri otomatik olarak uygulayan bir ilke oluşturursunuz. Etiketler artık yeni [değişiklik](../../governance/policy/concepts/effects.md#modify) efekti ve bir [Düzeltme göreviyle](../../governance/policy/how-to/remediate-resources.md)mevcut kaynaklara da uygulanabilir. Aşağıdaki bölümde, Etiketler için örnek ilkeler gösterilmektedir.

## <a name="policies"></a>İlkeler

[!INCLUDE [Tag policies](../../../includes/policy/samples/bycat/policies-tags.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Kaynakları etiketleme hakkında bilgi edinmek için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](tag-resources.md).
* Tüm kaynak türleri etiketleri desteklemez. Bir kaynak türüne etiket uygulayıp uygulayacağınızı öğrenmek için bkz. [Azure kaynakları Için etiket desteği](tag-support.md).
