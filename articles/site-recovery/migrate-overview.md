---
title: Azure 'a geçiş için Azure geçişi ve Site Recovery karşılaştırın
description: Site Recovery yerine geçiş için Azure geçişi kullanmanın avantajlarını özetler.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/06/2020
ms.author: raynew
ms.openlocfilehash: 358efaa1493aa08fb76c9bb83e0e4289950e0969
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87844329"
---
# <a name="migrating-to-azure"></a>Azure’a Geçiş

Geçiş için Azure geçişi hizmetini, Azure Site Recovery hizmeti yerine VM 'Leri ve sunucuları Azure 'a geçirmek için kullanmanızı öneririz. Azure geçişi hakkında [daha fazla bilgi edinin](../migrate/migrate-services-overview.md) .


## <a name="why-use-azure-migrate"></a>Azure Geçişi hizmetini neden kullanmalısınız?

Geçiş için Azure geçişi 'nin kullanılması çok sayıda avantaj sağlar:
 
 
- Azure geçişi, bulma, değerlendirme ve Azure 'a geçiş için merkezi bir merkez sağlar.
- Azure geçişi 'nin kullanılması, Azure geçiş araçları, diğer Azure hizmetleri ve üçüncü taraf araçlarla birlikte çalışabilirlik ve gelecekteki genişletilebilirlik sağlar.
- Azure geçişi: sunucu geçiş aracı, Azure 'a sunucu geçişi için tasarlanmıştır. Geçiş için en iyi duruma getirilmiştir. Doğrudan geçişle ilgili olmayan kavramlar ve senaryolar hakkında bilgi almanız gerekmez. 
- Bir VM için çoğaltma işlemi başlatıldığında 180 gün boyunca geçiş için bir araç kullanım ücreti yoktur. Bu, geçişi tamamlamaya yönelik bir zaman sağlar. Yalnızca Çoğaltmada kullanılan depolama ve ağ kaynakları için ve test geçişleri sırasında tüketilen işlem ücretleri için ödeme yaparsınız.
- Azure geçişi, Site Recovery tarafından desteklenen tüm geçiş senaryolarını destekler. Ayrıca, VMware VM 'Leri için Azure geçişi aracısız bir geçiş seçeneği sağlar.
- Yalnızca Azure geçişi: sunucu geçiş aracı için yeni geçiş özelliklerini önceliklendiriyoruz. Bu özellikler Site Recovery için hedeflenmemektedir.

## <a name="when-to-use-site-recovery"></a>Site Recovery ne zaman kullanılır?

Site Recovery kullanılmalıdır:

- Şirket içi makinelerin Azure 'a olağanüstü durum kurtarması için.
- Azure bölgeleri arasında Azure VM 'lerinin olağanüstü durum kurtarması için.

Şirket içi sunucuları Azure 'a geçirmek için Azure geçişi kullanmanızı öneririz, ancak geçiş yolculuğunu Site Recovery ile zaten başlattıysanız, geçiş sürecinizi tamamlayabilmeniz için kullanmaya devam edebilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

> Azure geçişi ile ilgili [sık sorulan soruları gözden geçirin](../migrate/resources-faq.md) .
