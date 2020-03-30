---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 02/10/2020
ms.author: cynthn
ms.openlocfilehash: 90dc5a067c05cadb3d5e102435b2e3d3de803e28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334589"
---
| Kaynak | Sınır |
| --- | --- |
| [Abonelik](../articles/billing-buy-sign-up-azure-subscription.md) başına VM |Bölge başına 25.000<sup>1.</sup> |
| [Abonelik](../articles/billing-buy-sign-up-azure-subscription.md) başına toplam VM çekirdeği sayısı |Bölge başına 20<sup>1.</sup> Sınırı artırmak için desteğe başvurun. |
| Azure Spot VM [abonelik](../articles/billing-buy-sign-up-azure-subscription.md) başına toplam çekirdek |Bölge başına 20<sup>1.</sup> Sınırı artırmak için desteğe başvurun. |
| Dv2 ve F gibi seri başına VM, [abonelik](../articles/billing-buy-sign-up-azure-subscription.md) başına çekirdek |Bölge başına 20<sup>1.</sup> Sınırı artırmak için desteğe başvurun. |
| Abonelik başına [kullanılabilirlik kümeleri](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) |Bölge başına 2,000. |
| Kullanılabilirlik kümesi başına sanal makineler | 200 |
| Abonelik başına sertifikalar |Sınırsız<sup>2</sup> |

<sup>1.1.2</sup> Varsayılan sınırlar, Ücretsiz Deneme ve You-You-Go Öde gibi teklif kategori türüne ve Dv2, F ve G gibi serilere göre değişir. Örneğin, Kurumsal Sözleşme abonelikleri için varsayılan değer 350'dir.

<sup>2.000</sup> Azure Kaynak Yöneticisi ile sertifikalar Azure Anahtar Kasası'nda depolanır. Sertifika sayısı abonelik için sınırsızdır. Dağıtım başına 1 MB'lık sertifika sınırı vardır ve bu sınır tek bir VM veya kullanılabilirlik kümesinden oluşur.

> [!NOTE]
> Sanal makine çekirdeklerinin bölgesel bir toplam sınırı vardır. Ayrıca Dv2 ve F gibi bölgesel boyut başına seriler için de bir sınırı var. Bu sınırlar ayrı ayrı uygulanır. Örneğin, ABD Doğu toplam VM çekirdek limiti 30, A serisi çekirdek limiti 30 ve D serisi çekirdek limiti 30 olan bir abonelik düşünün. Bu abonelik, toplam 30 çekirdeği aşmamak üzere 30 A1 VM veya 30 D1 VM dağıtabilir. Bir kombinasyon örneği 10 A1 VM ve 20 D1 VM'dir.  
> <!-- -->
> 
