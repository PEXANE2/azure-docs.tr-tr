---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 07/22/2020
ms.author: danlep
ms.openlocfilehash: 6878180ffedfaa53f25d2bdc6db72dcd7dd8b38b
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87384845"
---
| Kaynak | Sınır |
| --- | :--- |
| Abonelik başına bölge başına standart SKU kapsayıcı grupları | 100<sup>1</sup> |
| Abonelik başına bölge başına adanmış SKU kapsayıcı grupları | 0<sup>1</sup> |
| Kapsayıcı grubu başına kapsayıcı sayısı | 60 |
| Kapsayıcı grubu başına birim sayısı | 20 |
| Abonelik başına bölge başına standart SKU çekirdekleri (CPU) | 10<sup>1, 2</sup> | 
| Abonelik başına bölge başına K80 GPU için standart SKU çekirdekleri (CPU) | 18<sup>1, 2</sup> |
| Abonelik başına bölge başına P100 veya V100 GPU için standart SKU çekirdekleri (CPU) | 0<sup>1, 2</sup> |
| IP başına bağlantı noktası | 5 |
| Kapsayıcı örneği günlük boyutu-çalışan örnek | 4 MB |
| Kapsayıcı örneği günlük boyutu-örnek durduruldu | 16 KB veya 1.000 çizgi |
| Saat başına kapsayıcı oluşturma sayısı |300<sup>1</sup> |
| 5 dakika bir kapsayıcı oluşturma sayısı | 100<sup>1</sup> |
| Saat başına kapsayıcı silme sayısı | 300<sup>1</sup> |
| 5 dakika bir kapsayıcı silme sayısı | 100<sup>1</sup> |


<sup>1</sup> Sınır artışı istemek için bir [Azure destek isteği][azure-support]oluşturun. [Azure Ücretsiz hesabı](https://azure.microsoft.com/offers/ms-azr-0044p/) ve [öğrenciler için Azure](https://azure.microsoft.com/offers/ms-azr-0170p/) dahil ücretsiz abonelikler, sınır veya kota artışına uygun değildir. Ücretsiz aboneliğiniz varsa, bir Kullandıkça Öde aboneliğine [yükseltebilirsiniz](../articles/cost-management-billing/manage/upgrade-azure-subscription.md) .<br />
<sup>2</sup> [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) aboneliğine yönelik varsayılan sınır. Sınır diğer kategori türleri için farklı olabilir.<br/>

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
