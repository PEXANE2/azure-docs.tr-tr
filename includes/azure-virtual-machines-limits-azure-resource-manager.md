---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 02/10/2020
ms.author: cynthn
ms.openlocfilehash: a9c830025b9ce6c93bf106a8348b0f9dc6521582
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95555913"
---
| Kaynak | Sınır |
| --- | --- |
| [Abonelik](https://azure.microsoft.com/pricing/) başına VM |Bölge başına 25.000<sup>1</sup> . |
| [Abonelik](https://azure.microsoft.com/pricing/) başına toplam VM çekirdeği sayısı |Bölge başına 20<sup>1</sup> . Limiti artırmak için desteğe başvurun. |
| Azure spot VM [abonelik](https://azure.microsoft.com/pricing/) başına toplam çekirdek |Bölge başına 20<sup>1</sup> . Limiti artırmak için desteğe başvurun. |
| Abonelik başına dv2 ve F gibi sanal makine, [abonelik](https://azure.microsoft.com/pricing/) başına çekirdek |Bölge başına 20<sup>1</sup> . Limiti artırmak için desteğe başvurun. |
| Abonelik başına [kullanılabilirlik kümesi](../articles/virtual-machines/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) sayısı |Bölge başına 2.500. |
| Kullanılabilirlik kümesi başına sanal makineler | 200 |
| Abonelik başına sertifika |Sınırsız<sup>2</sup> |

<sup>1</sup> Varsayılan sınırlar, ücretsiz deneme, Kullandıkça öde ve dv2, F ve G gibi seriler gibi teklif kategorisi türüne göre farklılık gösterir. Örneğin, Kurumsal Anlaşma abonelikler için varsayılan değer 350 ' dir.

<sup>2</sup> Azure Resource Manager, sertifikalar Azure Key Vault depolanır. Sertifika sayısı bir abonelik için sınırsız. Dağıtım başına, tek bir VM veya bir kullanılabilirlik kümesinden oluşan 1 MB 'lik sertifika sınırı vardır.

> [!NOTE]
> Sanal makine çekirdekleri bölgesel toplam sınıra sahiptir. Ayrıca, Dv2 ve F gibi bölge başına bir seri için sınır vardır. Bu sınırlar ayrı olarak zorlanır. Örneğin, ABD Doğu toplam VM çekirdek limiti 30, A serisi çekirdek limiti 30 ve D serisi çekirdek limiti 30 olan bir abonelik düşünün. Bu abonelik 30 a1 VM 'Leri ya da 30 D1 VM 'yi ya da ikisinin bir bileşimini bir toplam 30 çekirdeği aşmayacak şekilde dağıtabilirler. Bir bileşim örneği 10 a1 VM ve 20 D1 VM 'dir.  
> <!-- -->
>