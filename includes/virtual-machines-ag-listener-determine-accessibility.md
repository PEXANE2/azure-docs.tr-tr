---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 8861396db6f6b680ddb55ce020e5579dc25b118e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188324"
---
Azure'da kullanılabilirlik grubu dinleyicisini yapılandırmanın iki yolu olduğunu fark etmek önemlidir. Dinleyiciyi oluştururken kullandığınız Azure yük bakiyesi türünde yollar farklıdır. Aşağıdaki tablofarklılıkları açıklar:

| Yük dengeleyici türü | Uygulama | Şu durumlarda kullanın: |
| --- | --- | --- |
| **Dış** |Sanal makineleri (VM) barındıran bulut hizmetinin *genel sanal IP adresini* kullanır. |Dinleyiciye Internet de dahil olmak üzere sanal ağın dışından erişmeniz gerekir. |
| **Iç** |Dinleyici için özel bir adresi olan bir *dahili yük dengeleyicisi* kullanır. |Dinleyiciye yalnızca aynı sanal ağ içinden erişebilirsiniz. Bu erişim, karma senaryolarda siteden siteye VPN içerir. |

> [!IMPORTANT]
> İstemci, dinleyici ve veritabanları aynı Azure bölgesinde olduğu sürece bulut hizmetinin genel VIP'sini (dış yük bakiyesi) kullanan bir dinleyici için çıkış ücreti ödemezsiniz. Aksi takdirde, dinleyici aracılığıyla döndürülen veriler çıkış olarak kabul edilir ve normal veri aktarım hızlarında ücretlendirilir. 
> 
> 

ILB yalnızca bölgesel kapsama sahip sanal ağlarda yapılandırılabilir. Yakınlık grubu için yapılandırılan varolan sanal ağlar ILB kullanamaz. Daha fazla bilgi için [İç yük dengeleyicisi genel bakış](../articles/load-balancer/load-balancer-internal-overview.md)bakın.

