---
title: Azure Site Recovery ile sunucu ve VM 'Leri Azure 'a geçirme
description: Şirket içi ve Azure IaaS VM 'lerinin Azure Site Recovery hizmeti kullanılarak Azure 'a nasıl geçirileceği açıklanmaktadır.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/27/2020
ms.author: raynew
ms.openlocfilehash: aaf01dcd63c21e4741456f4f7fccaf22b8fbfffc
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87281319"
---
# <a name="about-migration"></a>Geçiş hakkında

Azure geçişi hizmetini kullanarak VM 'Leri ve sunucuları Azure Site Recovery hizmeti yerine Azure 'a geçirin. Azure geçişi hakkında [daha fazla bilgi edinin](../migrate/migrate-services-overview.md) .


## <a name="why-use-azure-migrate"></a>Azure Geçişi hizmetini neden kullanmalısınız?

Geçiş için Azure geçişi 'nin kullanılması çok sayıda avantaj sağlar:
 
 
- Azure geçişi, bulma, değerlendirme ve Azure 'a geçiş için merkezi bir merkez sağlar.
- Azure geçişi 'nin kullanılması, Azure geçiş araçları, diğer Azure hizmetleri ve üçüncü taraf araçlarla birlikte çalışabilirlik ve gelecekteki genişletilebilirlik sağlar.
- Azure geçişi: sunucu geçiş aracı, Azure 'a sunucu geçişi için tasarlanmıştır. Geçiş için en iyi duruma getirilmiştir. Doğrudan geçişle ilgili olmayan kavramlar ve senaryolar hakkında bilgi almanız gerekmez. 
- Bir VM için çoğaltma işlemi başlatıldığında 180 gün boyunca geçiş için bir araç kullanım ücreti yoktur. Bu, geçişi tamamlamaya yönelik bir zaman sağlar. Yalnızca Çoğaltmada kullanılan depolama ve ağ kaynakları için ve test geçişleri sırasında tüketilen işlem ücretleri için ödeme yaparsınız.
- VMware VM 'Leri için Azure geçişi, aracı tabanlı geçişe ek olarak aracısız geçiş sağlar.
- Yalnızca Azure geçişi: sunucu geçiş aracı için yeni geçiş özelliklerini önceliklendiriyoruz.

## <a name="when-to-use-site-recovery"></a>Site Recovery ne zaman kullanılır?

Site Recovery kullanılmalıdır:

- Şirket içi makinelerin Azure 'a olağanüstü durum kurtarması için.
- Azure bölgeleri arasında Azure VM 'lerinin olağanüstü durum kurtarması için.

Şirket içi sunucuları Azure 'a geçirmek için Azure geçişi kullanmanızı öneririz, ancak geçiş yolculuğunu Site Recovery ile zaten başlattıysanız, geçiş sürecinizi tamamlayabilmeniz için kullanmaya devam edebilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

> Azure geçişi ile ilgili [sık sorulan soruları gözden geçirin](../migrate/resources-faq.md) .
