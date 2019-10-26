---
title: Hyper-V ' d i kullanan müşterilerin sahip olduğu siteler arasında ve SCVMM tarafından Azure 'a yönetilen siteler arasında DR 'nin kullanım dışı bırakılması | Microsoft Docs
description: Hyper-V ' d i kullanarak müşterilerin sahip olduğu siteler arasında ve SCVMM ile Azure arasında yönetilen siteler arasında ve diğer seçenekler arasında DR 'nin kullanımdan kalkmasıyla ilgili ayrıntılar
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 09/13/2019
ms.author: rajanaki
ms.openlocfilehash: 972223815810917684f35c4e99f04e1ab5b882c8
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72952127"
---
# <a name="upcoming-deprecation-of-dr-between-customer-owned-sites-using-hyper-v-and-between-sites-managed-by-scvmm-to-azure"></a>Hyper-V ' d e ait müşterilerin sahip olduğu siteler arasında ve SCVMM tarafından Azure 'a yönetilen siteler arasında DR 'nin kullanım dışı bırakılması

Bu makalede, gelecekteki kullanım dışı bırakma planı, ilgili etkileri ve Azure Site Recovery tarafından desteklenen aşağıdaki senaryolar için kullanılabilir alternatif seçenekler açıklanmaktadır. 

- Müşterinin sahip olduğu siteler arasında SCVMM tarafından yönetilen Hyper-V VM 'Leri arasındaki DR 
- SCVMM tarafından Azure 'a yönetilen Hyper-V VM 'lerinin DR

> [!IMPORTANT]
> Bu senaryolar Şu anda kullanımdan kaldırma için işaretlendi ve müşterilerin ortamları kesintiye uğramaması için en erken düzeltme adımlarını ele geçirmesine beklenmektedir. 
 

## <a name="what-changes-should-you-expect"></a>Hangi değişiklikleri beklemeniz gerekir?

- 2019 Kasım 'Dan itibaren, bu senaryolar için boardings üzerinde yeni kullanıcı için izin verilmez. Yük devretme, yük devretme testi, izleme vb. dahil olmak üzere **var olan çoğaltmalar ve yönetim işlemleri** **etkilenmeyecektir**.

- Senaryolar kullanım dışı olduktan sonra, müşteri önerilen adımları izlemediği takdirde aşağıdaki bazı etkileri olur.

    - SCVMM tarafından yönetilen, şirkete ait siteler arasında çalışan Hyper-V VM 'Ler arasında DR: Hyper-V çoğaltma 'nın temel özelliği çalışmaya devam edecek, ancak müşteriler, DR ile ilgili olan işlemleri görüntüleyemez, yönetemez veya gerçekleştiremez. Azure Sire kurtarma deneyimi aracılığıyla Azure portal. 
    - SCVMM tarafından Azure 'a yönetilen Hyper-V VM 'lerinin DR: var olan çoğaltmalar kesintiye uğracaktır ve müşteriler Azure Site Recovery aracılığıyla DR ile ilgili işlemleri görüntüleyemez, yönetemez veya gerçekleştiremez.


## <a name="recommended-actions-to-be-taken"></a>Gerçekleştirilecek Önerilen Eylemler

Bunun nedeni, müşterinin, senaryo kullanım dışı olduktan sonra DR stratejilerinin etkilenmemesini sağlamak için gereken alternatif seçeneklerdir. 

- [Hyper-V konaklarındaki VM 'ler Için Azure 'U Dr hedefi olarak kullanmaya başlamak](hyper-v-azure-tutorial.md)için seçin.

> [!IMPORTANT]
> Şirket içi ortamınızın hala SCVMMM 'e sahip olabileceğini lütfen unutmayın, ancak ASR 'yi yalnızca Hyper-V konaklarına başvurularla yapılandıracaksınız.

- Siteden siteye çoğaltmaya devam etmeyi, ancak temel [Hyper-V çoğaltma çözümünü](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)kullanmayı seçin, ancak Azure Portal Azure SITE Recovery kullanarak Dr yapılandırmasını yönetemeyecektir. 


## <a name="next-steps"></a>Sonraki Adımlar
Kullanımdan kaldırma planlayın ve altyapınız ve işletmeniz için en uygun alternatif bir seçenek belirleyin. Bu durum ile ilgili herhangi bir sorgunuz varsa, lütfen Microsoft Desteği ulaşın

