---
title: Azure Blockchain Hizmeti sınırları
description: Azure Blockchain Hizmeti'nde hizmete ve işlevsel sınırlara genel bakış
ms.date: 04/02/2020
ms.topic: conceptual
ms.reviewer: ravastra
ms.openlocfilehash: 71e1bebf10fa0142870d03977182472da1ad031f
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676528"
---
# <a name="limits-in-azure-blockchain-service"></a>Azure Blockchain Hizmetinde Sınırlar

Azure Blockchain Hizmeti, bir üyenin sahip olabileceği düğüm sayısı, konsorsiyum kısıtlamaları ve depolama tutarları gibi hizmet ve işlevsel sınırlara sahiptir.

## <a name="pricing-tier"></a>Fiyatlandırma katmanı

İşlemler ve geçerlidüğümlerle ilgili maksimum sınırlar, Azure Blockchain Hizmetini temel veya standart fiyatlandırma katmanlarında sağlayıp sağlamadığınıza bağlıdır.

| Fiyatlandırma katmanı | Maksimum hareket düğümleri | Maksimum geçerlilik düğümleri |
|:---|:---:|:---:|
| Temel | 10 | 1 |
| Standart | 10 | 2 |

Konsorsiyum ağınızın en az iki Azure Blockchain Hizmeti standart katman düğümü olmalıdır. Standart katman düğümleri iki doğrulayıcı düğüm içerir. [İstanbul Bizans Fay Toleransı konsensüsünü](https://github.com/jpmorganchase/quorum/wiki/Quorum-Consensus)karşılamak için dört doğrulayıcı düğüm gereklidir.

Temel katmanı kullanmak geliştirme, test ve kavramların kanıtı içindir. Üretim sınıfı dağıtımları için standart katmanı kullanın. Blockchain Data Manager kullanıyorsanız veya yüksek hacimli özel işlemler *gönderiyorsanız, Standart* katmanı da kullanmalısınız.

Üye oluşturulduktan sonra temel ve standart arasındaki fiyatlandırma katmanının değiştirilmesi desteklenmez.

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

## <a name="performance"></a>Performans

Her işlem gönderimi için *eth.estimate* gaz işlevini kullanmayın. *Eth.estimate* işlevi bellek yoğundur. İşlevi birden çok kez çağırmak, saniyede hareketleri önemli ölçüde azaltır.

Mümkünse, işlemleri göndermek için konservatif bir gaz değeri kullanın ve *eth.estimate*kullanımını en aza indirin.

## <a name="next-steps"></a>Sonraki adımlar

Yama, güncellemeler ve sürümler - [Yama](ledger-versions.md)lama ve yükseltmelerle ilgili politikalar hakkında daha fazla bilgi edinin.
