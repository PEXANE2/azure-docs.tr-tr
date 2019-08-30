---
title: Azure blok zinciri sınırları
description: Azure blok zinciri hizmetindeki hizmet ve işlevsel sınırlara genel bakış
services: azure-blockchain
keywords: blok zinciri
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: aeed84f19da3843d043eafef9d7444661901c53e
ms.sourcegitcommit: aaa82f3797d548c324f375b5aad5d54cb03c7288
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70147140"
---
# <a name="limits-in-azure-blockchain-service"></a>Azure blok zinciri hizmeti sınırları

Azure blok zinciri hizmeti, bir üyenin sahip olduğu düğüm sayısı, konsorsiyum kısıtlamaları ve depolama tutarları gibi hizmet ve işlevsel sınırlara sahiptir.

## <a name="pricing-tier"></a>Fiyatlandırma katmanı

İşlem ve doğrulayıcı düğümlerine yönelik en fazla sınır, Azure blok zinciri hizmetini temel veya standart fiyatlandırma katmanlarında sağlayıp sağlamadığınıza bağlıdır.

| Fiyatlandırma katmanı | En fazla işlem düğümleri | En fazla Doğrulayıcı düğümleri |
|:---|:---:|:---:|
| Temel | 10 | 1\. |
| Standart | 10 | 2 |

Üye oluşturulduktan sonra temel ve standart arasındaki fiyatlandırma katmanını değiştirmek desteklenmez.

## <a name="storage-capacity"></a>Depolama kapasitesi

Muhasebe verileri ve Günlükler için düğüm başına kullanılabilecek maksimum depolama alanı miktarı 1,8 terabayt 'dir.

Azalan defter ve günlük depolama boyutu desteklenmiyor.

## <a name="consortium-limits"></a>Konsorsiyum sınırları

* **Konsorsiyumun ve üye adlarının** Azure blok zinciri hizmetindeki diğer konsorsiyum ve üye adlarından benzersiz olması gerekir.

* **Üye ve konsorsiyum adları değiştirilemez**

* **Bir konsorsiyumun tüm üyeleri aynı fiyatlandırma katmanında olmalıdır**

* **Bir konsorsiyumde katılan tüm Üyeler aynı bölgede bulunmalıdır**

    Bir konsorsiyda oluşturulan ilk üye bölgeyi belirler. Bir konsorsiyumye davet edilen Üyeler ilk üyeyle aynı bölgede bulunmalıdır. Tüm üyelerin aynı bölgeye sınırlandırılması, ağ konsensus 'in olumsuz şekilde etkilenmemesini sağlamaya yardımcı olur.

* **Konsorsiyumun en az bir Yöneticisi olmalıdır**

    Bir konsorsiyumda yalnızca bir yönetici varsa, bu kullanıcılar kendi konsorsiyumdan kaldıramazlar veya konsorsiyumun başka bir Yöneticisi eklenip yükseltilene kadar üyelerini silmez.

* **Konsorsiyumun kaldırıldığı Üyeler yeniden eklenemez**

    Bunun yerine, konsorsiyumun katılması için yeniden davet edilmesi ve yeni bir üye oluşturulması gerekir. Geçmiş işlemleri korumak için mevcut üye kaynakları silinmez.

* **Bir konsorsiyumun tüm üyeleri aynı muhasebe sürümünü kullanıyor olmalıdır**

    Azure blok zinciri hizmeti 'nde kullanılabilen düzeltme eki, güncelleştirmeler ve genel muhasebe sürümleri hakkında daha fazla bilgi için bkz. [düzeltme eki uygulama, güncelleştirmeler ve sürümler](ledger-versions.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Düzeltme eki uygulama, güncelleştirmeler ve sürümler](ledger-versions.md)
