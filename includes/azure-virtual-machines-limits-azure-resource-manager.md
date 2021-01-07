---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 02/10/2020
ms.author: cynthn
ms.openlocfilehash: cd3ff3fce80e66d7cd61636b4416cb2fc28f5e77
ms.sourcegitcommit: 19ffdad48bc4caca8f93c3b067d1cf29234fef47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97956450"
---
| Kaynak | Sınır |
| --- | --- |
| [Abonelik](https://azure.microsoft.com/pricing/) başına VM |Bölge başına 25.000<sup>1</sup> . |
| [Abonelik](https://azure.microsoft.com/pricing/) başına toplam VM çekirdeği sayısı |Bölge başına 20<sup>1</sup> . Limiti artırmak için desteğe başvurun. |
| Azure spot VM [abonelik](https://azure.microsoft.com/pricing/) başına toplam çekirdek |Bölge başına 20<sup>1</sup> . Limiti artırmak için desteğe başvurun. |
| Abonelik başına dv2 ve F gibi sanal makine, [abonelik](https://azure.microsoft.com/pricing/) başına çekirdek |Bölge başına 20<sup>1</sup> . Limiti artırmak için desteğe başvurun. |
| Abonelik başına [kullanılabilirlik kümesi](../articles/virtual-machines/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) sayısı |Bölge başına 2.500. |
| Kullanılabilirlik kümesi başına sanal makineler | 200 |
| [Kaynak grubu](../articles/azure-resource-manager/management/overview.md#resource-groups) başına [yakınlık yerleştirme grupları](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups-portal) | 800 | 
| Kullanılabilirlik kümesi başına sertifika | 199<sup>2</sup> |
| Abonelik başına sertifika |Sınırsız<sup>3</sup> |

<sup>1</sup> varsayılan sınırlar, ücretsiz deneme, Kullandıkça öde ve dv2, F ve G gibi seriler gibi teklif kategorisi türüne göre farklılık gösterir. Örneğin, Kurumsal Anlaşma abonelikler için varsayılan değer 350 ' dir.

SSH ortak anahtarları gibi <sup>2</sup> özellik de sertifika olarak gönderilir ve bu sınıra doğru sayılır. Bu sınırı atlamak için, sertifikaları yüklemek üzere [Windows için Azure Key Vault uzantısı](https://docs.microsoft.com/azure/virtual-machines/extensions/key-vault-windows) veya [Linux için Azure Key Vault uzantısı](https://docs.microsoft.com/azure/virtual-machines/extensions/key-vault-linux) kullanın.

<sup>3</sup> Azure Resource Manager, sertifikalar Azure Key Vault depolanır. Sertifika sayısı bir abonelik için sınırsız. Dağıtım başına, tek bir VM veya bir kullanılabilirlik kümesinden oluşan 1 MB 'lik sertifika sınırı vardır.



> [!NOTE]
> Sanal makine çekirdekleri bölgesel toplam sınıra sahiptir. Ayrıca, Dv2 ve F gibi bölge başına bir seri için sınır vardır. Bu sınırlar ayrı olarak zorlanır. Örneğin, ABD Doğu toplam VM çekirdek limiti 30, A serisi çekirdek limiti 30 ve D serisi çekirdek limiti 30 olan bir abonelik düşünün. Bu abonelik 30 a1 VM 'Leri ya da 30 D1 VM 'yi ya da ikisinin bir bileşimini bir toplam 30 çekirdeği aşmayacak şekilde dağıtabilirler. Bir bileşim örneği 10 a1 VM ve 20 D1 VM 'dir.  
> <!-- -->
>
