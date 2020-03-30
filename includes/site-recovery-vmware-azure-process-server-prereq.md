---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 11/28/2019
ms.author: raynew
ms.openlocfilehash: de15e3028cf22cdd03ce29385278fc5e2babaa9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74566381"
---
Bu makalede aşağıdaki durumlar varsayılır

1. Şirket içi ağınız ile Azure Sanal Ağ arasında **Siteden Siteye VPN** veya **Express Route** bağlantısı zaten oluşturulmuştur.
2. Kullanıcı hesabınız sanal makinelerin yük devrettiği Azure Aboneliğinde yeni bir sanal makine oluşturma izinlerine sahiptir.
3. Aboneliğinizde, yeni bir Process Server sanal makinesini döndürmek için en az 8 Çekirdek vardır.
4. **Configuration Server Parolanız** mevcuttur.

> [!TIP]
> Sanal makinelerin yük devrettiği Azure Sanal Ağından Configuration Server (şirket içinde çalışan) 443 bağlantı noktasını bağlayabildiğinizden emin olun.
