---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 8861396db6f6b680ddb55ce020e5579dc25b118e
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188324"
---
Azure 'da bir kullanılabilirlik grubu dinleyicisi yapılandırmanın iki yolu olduğunu fark etmek önemlidir. Yollar, dinleyiciyi oluştururken kullandığınız Azure Yük dengeleyicinin türüne göre farklılık gösterir. Aşağıdaki tabloda farklar açıklanmaktadır:

| Yük dengeleyici türü | Uygulama | Şu durumlarda kullanın: |
| --- | --- | --- |
| **Dış** |, Sanal makineleri (VM 'Ler) barındıran bulut hizmetinin *Genel sanal IP adresini* kullanır. |Internet 'ten dahil olmak üzere, sanal ağ dışından dinleyiciye erişmeniz gerekir. |
| **İç** |, Dinleyici için özel bir adresle *iç yük dengeleyici* kullanır. |Dinleyiciye yalnızca aynı sanal ağ içinden erişebilirsiniz. Bu erişim, karma senaryolarda siteden siteye VPN içerir. |

> [!IMPORTANT]
> Bulut hizmetinin genel VIP 'sini (dış yük dengeleyici) kullanan bir dinleyici için, istemci, dinleyici ve veritabanları aynı Azure bölgesinde olduğu sürece çıkış ücretlerine tabi olmayacaktır. Aksi takdirde, dinleyiciyle döndürülen tüm veriler çıkış olarak kabul edilir ve normal veri aktarımı ücretleri üzerinden ücretlendirilir. 
> 
> 

ILB, yalnızca bölgesel kapsama sahip sanal ağlarda yapılandırılabilir. Benzeşim grubu için yapılandırılmış var olan sanal ağlar ıLB kullanamaz. Daha fazla bilgi için bkz. [İç Yük Dengeleyiciye genel bakış](../articles/load-balancer/load-balancer-internal-overview.md).

