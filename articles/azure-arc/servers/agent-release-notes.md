---
title: Azure Arc etkin sunucular (Önizleme) aracısındaki yenilikler
description: Bu makalede, Azure Arc etkin sunucular (Önizleme) Aracısı için sürüm notları bulunur. Özetlenen birçok sorun için ek ayrıntıların bağlantıları vardır.
ms.topic: conceptual
ms.date: 08/31/2020
ms.openlocfilehash: 0f5060189d6f9cac620b49f414e0e27bed3e356e
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89236773"
---
# <a name="whats-new-with-azure-arc-enabled-servers-preview-agent"></a>Azure Arc etkin sunucular (Önizleme) aracısındaki yenilikler

Azure Arc etkin sunucular (Önizleme) bağlı makine Aracısı, iyileştirmeleri sürekli olarak alır. En son gelişmelerden haberdar olmak için, bu makalede hakkında bilgi verilmektedir:

- En son yayınlar
- Bilinen sorunlar
- Hata düzeltmeleri

## <a name="august-2020"></a>Ağustos 2020

Sürüm: 0,11

- Ubuntu 20,04 desteği.

- Uzantı dağıtımları için güvenilirlik iyileştirmeleri.

### <a name="known-issues"></a>Bilinen sorunlar

Linux aracısının eski bir sürümünü kullanıyorsanız ve bunu bir ara sunucu kullanacak şekilde yapılandırdıysanız, yükseltmeden sonra proxy sunucu ayarını yeniden yapılandırmanız gerekir. Bunu yapmak için öğesini çalıştırın `sudo azcmagent_proxy add http://proxyserver.local:83` .

## <a name="next-steps"></a>Sonraki adımlar

Birden çok karma makinede yay etkin sunucuları (Önizleme) değerlendirmeden veya etkinleştirmeden önce, gereksinimleri anlamak için [bağlı makine aracısına genel bakış](agent-overview.md) ' ı ve aracı hakkındaki teknik ayrıntıları ve dağıtım yöntemlerini gözden geçirin.