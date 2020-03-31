---
title: Azure Site Kurtarma'da VMware olağanüstü durum kurtarma yapılandırma sunucusu gereksinimleri
description: Bu makalede, Azure Site Kurtarma ile VMware olağanüstü durum kurtarma yapılandırma sunucusunu Azure'a dağıtırken destek ve gereksinimleri açıklanmaktadır
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.date: 09/09/2019
ms.author: raynew
ms.openlocfilehash: 0b0942b517c8dc83c048bd1203a58d9861515dfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257413"
---
# <a name="configuration-server-requirements-for-vmware-disaster-recovery-to-azure"></a>Azure'a VMware olağanüstü durum kurtarma için yapılandırma sunucusu gereksinimleri

VMware VM'lerin ve fiziksel sunucuların Azure'da olağanüstü kurtarma durumu için [Azure Site Kurtarma'yı](site-recovery-overview.md) kullandığınızda şirket içi yapılandırma sunucusu dağıtırsınız.

- Yapılandırma sunucusu şirket içi VMware ve Azure arasındaki iletişimi koordine eder. Ayrıca veri çoğaltma yönetir.
- Yapılandırma sunucusu bileşenleri ve işlemleri hakkında [daha fazla bilgi edinin.](vmware-azure-architecture.md)

## <a name="configuration-server-deployment"></a>Yapılandırma sunucusu dağıtımı

VMware VM'lerin Azure'a olağanüstü kurtarması için yapılandırma sunucusunu VMware VM olarak dağıtabilirsiniz.

- Site Kurtarma, Azure portalından indirdiğiniz ve yapılandırma sunucusu VM'yi kurmak için vCenter Server'a aktardığınız bir OVA şablonu sağlar.
- OVA şablonu kullanarak yapılandırma sunucusunu dağıttığınızda, VM bu makalede listelenen gereksinimlere otomatik olarak uyar.
- Yapılandırma sunucusunu OVA şablonu kullanarak kurmanızı şiddetle öneririz. Ancak, VMware VM'ler için olağanüstü durum kurtarma ayarlıyorsanız ve OVA şablonunu kullanamıyorsanız, [sağlanan bu yönergeleri](physical-azure-set-up-source.md)kullanarak yapılandırma sunucusunu dağıtabilirsiniz.
- Şirket içi fiziksel makinelerin olağanüstü durum kurtarması için yapılandırma sunucusunu Azure'a dağıtıyorsanız, [bu makaledeki](physical-azure-set-up-source.md)yönergeleri izleyin. 

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="next-steps"></a>Sonraki adımlar
[VMware VM'lerin](vmware-azure-tutorial.md) Azure'a olağanüstü durum kurtarmasını ayarlayın.
