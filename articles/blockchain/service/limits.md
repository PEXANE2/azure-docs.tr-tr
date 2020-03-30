---
title: Azure Blockchain Hizmeti sınırları
description: Azure Blockchain Hizmeti'nde hizmete ve işlevsel sınırlara genel bakış
ms.date: 11/22/2019
ms.topic: conceptual
ms.reviewer: janders
ms.openlocfilehash: f4001ee520f3f3136d1bac5ca047c80526fc92e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74455651"
---
# <a name="limits-in-azure-blockchain-service"></a>Azure Blockchain Hizmetinde Sınırlar

Azure Blockchain Hizmeti, bir üyenin sahip olabileceği düğüm sayısı, konsorsiyum kısıtlamaları ve depolama tutarları gibi hizmet ve işlevsel sınırlara sahiptir.

## <a name="pricing-tier"></a>Fiyatlandırma katmanı

İşlemler ve geçerlidüğümlerle ilgili maksimum sınırlar, Temel veya Standart fiyatlandırma katmanlarında Azure Blockchain Hizmeti sağlayıp sağlamadığınıza bağlıdır.

| Fiyatlandırma katmanı | Maksimum hareket düğümleri | Maksimum geçerlilik düğümleri |
|:---|:---:|:---:|
| Temel | 10 | 1 |
| Standart | 10 | 2 |

Üye oluşturulduktan sonra Temel ve Standart arasındaki fiyatlandırma katmanının değiştirilmesi desteklenmez.

## <a name="storage-capacity"></a>Depolama kapasitesi

Genel muhasebe verileri ve günlükleri için düğüm başına kullanılabilecek maksimum depolama miktarı 1,8 terabayttır.

Azalan genel muhasebe ve günlük depolama boyutu desteklenmez.

## <a name="consortium-limits"></a>Konsorsiyum limitleri

* **Konsorsiyum ve üye adları** Azure Blockchain Hizmeti'ndeki diğer konsorsiyum ve üye adlardan benzersiz olmalıdır.

* **Üye ve konsorsiyum adları değiştirilemez**

* **Konsorsiyumdaki tüm üyeler aynı fiyatlandırma katmanında olmalıdır**

* **Konsorsiyuma katılan tüm üyeler aynı bölgede ikamet etmelidir.**

    Bir konsorsiyumda oluşturulan ilk üye bölgeyi belirler. Konsorsiyuma davet edilen üyelerin ilk üyeyle aynı bölgede ikamet etmesi gerekmektedir. Tüm üyelerin aynı bölgeyle sınırlandırılması, ağ konsensüsunun olumsuz etkilenmemesini sağlamaya yardımcı olur.

* **Bir konsorsiyumun en az bir yöneticisi olmalıdır**

    Konsorsiyumda yalnızca bir yönetici varsa, konsorsiyuma başka bir yönetici eklenene veya terfi ettirilene kadar kendilerini konsorsiyumdan kaldıramazlar veya üyelerini silemezler.

* **Konsorsiyumdan çıkarılan üyeler bir daha eklenemez**

    Bunun yerine, konsorsiyuma katılmaya ve yeni bir üye oluşturmak için yeniden davet edilmeleri gerekir. Geçmiş işlemleri korumak için varolan üye kaynakları silinmez.

* **Konsorsiyumdaki tüm üyeler aynı genel muhasebe sürümünü kullanmalıdır**

    Azure Blockchain Hizmeti'nde bulunan yama, güncelleştirmeler ve genel muhasebe sürümleri hakkında daha fazla bilgi için [bkz.](ledger-versions.md)

## <a name="next-steps"></a>Sonraki adımlar

Yama, güncellemeler ve sürümler - [Yama](ledger-versions.md)lama ve yükseltmelerle ilgili politikalar hakkında daha fazla bilgi edinin.
