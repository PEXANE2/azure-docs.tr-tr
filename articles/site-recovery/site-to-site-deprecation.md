---
title: Azure Site Kurtarma ' yı kullanarak müşteri tarafından yönetilen siteler (VMM ile) arasında olağanüstü durum kurtarma yıkıntımı | Microsoft Dokümanlar
description: Hyper-V kullanan müşteriye ait siteler arasında ve SCVMM tarafından Azure'a yönetilen siteler ve alternatif seçenekler arasında DR'nin yaklaşan amortismanı hakkında ayrıntılar
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/25/2020
ms.author: rajanaki
ms.openlocfilehash: 208177d10e9002fafe2495710da229541a11a43e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77661679"
---
# <a name="deprecation-of-disaster-recovery-between-customer-managed-sites-with-vmm-using-azure-site-recovery"></a>Azure Site Kurtarma'yı kullanarak müşteri tarafından yönetilen siteler (VMM ile) arasında olağanüstü durum kurtarma amortismanı

Bu makalede, yaklaşan amortisman planı, ilgili etkileri ve aşağıdaki senaryo için müşteriler için kullanılabilir alternatif seçenekler açıklanmaktadır:

Site Kurtarma kullanarak System Center Virtual Machine Manager (SCVMM) tarafından yönetilen müşteriye ait siteler arasında DR

> [!IMPORTANT]
> Müşterilerin ortamlarında herhangi bir aksama yaşanmaması için en erken düzeltme adımlarını almaları tavsiye edilir. 

## <a name="what-changes-should-you-expect"></a>Ne gibi değişiklikler beklemelisiniz?

- Mart 2020'den itibaren, Hyper-V VM'lerin siteden siteye çoğaltılmasıyla birlikte azure portal bildirimleri & e-posta iletişimleri alacaksınız. Amortismanın Mart 2023'te yapılması planlanıyor.

- Varolan bir yapılandırmanız varsa, kurulumüzerinde hiçbir etki olmayacaktır.

- Müşteri alternatif yaklaşımları izlenmedikçe senaryolar amortismana uğradıktan sonra, varolan çoğaltmalar kesintiye uğrayabilir. Müşteriler, Azure portalındaki Azure Site Kurtarma deneyimi aracılığıyla DR ile ilgili işlemleri görüntüleyemez, yönetemez veya gerçekleştiremez.
 
## <a name="alternatives"></a>Alternatifler 

Aşağıda, senaryo amortismana uğradıktan sonra müşterinin DR stratejisinin etkilenmemesini sağlamak için seçebileceği alternatifler verilmiştir. 

- Seçenek 1 (Önerilen): [DR hedefi olarak Azure'u kullanmaya başlamayı](hyper-v-vmm-azure-tutorial.md)seçin.


- Seçenek 2: Temel [Hyper-V Çoğaltma çözümlerini](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)kullanarak siteden siteye çoğaltma işlemine devam etmeyi seçin, ancak Azure portalında Azure Site Kurtarma'yı kullanarak DR yapılandırmalarını yönetemeyeceksiniz. 


## <a name="remediation-steps"></a>Düzeltme adımları

Seçenek 1 ile gitmeyi seçiyorsanız, lütfen aşağıdaki adımları uygulayın:

1. [VMM'lerle ilişkili tüm sanal makinelerin korumasını devre dışı dışı kalıyorum.](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-secondary-vmm-server-using-the-system-center-vmm-to-vmm-scenario) Çoğaltmayı **Devre Dışı** Ve kaldırma seçeneğini kullanın veya şirket içi çoğaltma ayarlarının temizlenmesini sağlamak için belirtilen komut dosyalarını çalıştırın. 

2. Siteden siteye çoğaltma yapılandırmasından [tüm VMM sunucularını açın.](site-recovery-manage-registration-and-protection.md#unregister-a-vmm-server)

3. Azure kaynaklarını VM'lerinizin çoğaltılmasını etkinleştirmek için [hazırlayın.](tutorial-prepare-azure-for-hyperv.md)
4. [Şirket içi Hyper-V sunucuları hazırlayın](hyper-v-prepare-on-premises-tutorial.md)
5. [VMM bulutundaki VM'ler için çoğaltma yı ayarlama](hyper-v-vmm-azure-tutorial.md)
6. İsteğe bağlı ama önerilen: [Dr matkap çalıştırın](tutorial-dr-drill-azure.md)

Hyper-V çoğaltma kullanma seçeneği 2 ile gitmeyi seçiyorsanız, aşağıdaki adımları uygulayın:

1. **Korumalı Öğeler** > **Çoğaltılan Öğelerde,** **çoğaltmayı devre dışı >** makineyi sağ tıklatın.
2. **Çoğaltmayı Devre Dışı Kaldır'da** **Kaldır'ı**seçin.

    Bu, çoğaltılan öğeyi Azure Site Kurtarma'dan kaldırır (faturalandırma durdurulur). Şirket içi sanal makinedeki çoğaltma yapılandırması **temizlenmez.** 

## <a name="next-steps"></a>Sonraki adımlar
Amortisman için plan yapın ve altyapınız ve işletmeniz için en uygun alternatif seçeneği seçin. Bu konuyla ilgili herhangi bir sorunuz varsa, Microsoft Destek'e ulaşın

