---
title: Azure Site Recovery kullanarak müşteri tarafından yönetilen siteler arasında (VMM ile) olağanüstü durum kurtarmayı kullanımdan kaldırma | Microsoft Docs
description: Hyper-V ' d i kullanarak müşterilerin sahip olduğu siteler arasında ve SCVMM ile Azure arasında yönetilen siteler arasında ve diğer seçenekler arasında DR 'nin kullanımdan kalkmasıyla ilgili ayrıntılar
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/25/2020
ms.author: rajanaki
ms.openlocfilehash: 29a939452d9b90bd8afda7db4e115d10956ee5e5
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77606639"
---
# <a name="deprecation-of-disaster-recovery-between-customer-managed-sites-with-vmm-using-azure-site-recovery"></a>Azure Site Recovery kullanarak müşteri tarafından yönetilen siteler arasında (VMM ile) olağanüstü durum kurtarmayı kullanımdan kaldırma

Bu makalede gelecek kullanım dışı bırakma planı, ilgili etkileri ve müşteriler için aşağıdaki senaryo için kullanılabilecek alternatif seçenekler açıklanmaktadır:

Site Recovery kullanarak System Center Virtual Machine Manager (SCVMM) tarafından yönetilen müşterilerin sahip olduğu siteler arasında DR

> [!IMPORTANT]
> Müşterilerin ortamlarına herhangi bir kesinti yaşamamak için en erken düzeltme adımlarını gerçekleştirmeniz önerilir. 

## <a name="what-changes-should-you-expect"></a>Hangi değişiklikleri beklemeniz gerekir?

- 2019 Kasım 'Dan itibaren, bu senaryolar için boardings üzerinde yeni kullanıcı için izin verilmez. Yük devretme, yük devretme testi, izleme vb. dahil olmak üzere **var olan çoğaltmalar ve yönetim işlemleri** **etkilenmeyecektir**.

- Mevcut bir yapılandırmanız varsa, yeni VMMs 'yi kaydedemeyeceksiniz.

- Müşteri diğer yaklaşımları izliyorsa senaryolar kullanım dışı olduktan sonra var olan çoğaltmalar bozulabilir. Müşteriler, Azure portal Azure Site Recovery deneyimi aracılığıyla DR ile ilgili işlemleri görüntüleyemez, yönetemez veya gerçekleştiremez.
 
## <a name="alternatives"></a>Alternatifler 

Bu, senaryonun kullanım dışı olduktan sonra DR stratejilerinin etkilenmemesini sağlamak için müşterinin aralarından seçim yapabileceğiniz alternatiflerdir. 

- Seçenek 1 (önerilir): [Hyper-V konaklarındaki VM 'ler Için Azure 'U Dr hedefi olarak kullanmaya başlamak](hyper-v-azure-tutorial.md)için seçin.

    > [!IMPORTANT]
    > Şirket içi ortamınızın hala SCVMMM olduğunu, ancak ASR 'yi yalnızca Hyper-V konaklarına başvurularla yapılandırabileceğinizi unutmayın.

- 2\. seçenek: temel [Hyper-V çoğaltma çözümünü](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)kullanarak siteden siteye çoğaltmaya devam etmeyi seçin, ancak Azure Portal Azure SITE Recovery kullanarak Dr yapılandırmalarının yönetimi için işlem yapmanız gerekmez. 


## <a name="remediation-steps"></a>Düzeltme adımları

1\. seçenek ile devam etmek istiyorsanız lütfen aşağıdaki adımları yürütün:

1. [VMMs ile ilişkili tüm sanal makinelerin korumasını devre dışı bırakın](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-secondary-vmm-server-using-the-system-center-vmm-to-vmm-scenario). Şirket içi çoğaltma ayarlarının temizlendiğinden emin olmak için **çoğaltmayı devre dışı bırak ve Kaldır** seçeneğini kullanın veya belirtilen betikleri çalıştırın. 

2. [Tüm VMM sunucularının kaydını sil](site-recovery-manage-registration-and-protection.md#unregister-a-vmm-server)

3. Sanal makinelerinizin çoğaltılmasını etkinleştirmek için [Azure kaynaklarını hazırlayın](tutorial-prepare-azure-for-hyperv.md) .
4. [Şirket içi Hyper-V sunucularını hazırlama](hyper-v-prepare-on-premises-tutorial.md)

> [!IMPORTANT]
> VMM 'yi hazırlama bölümündeki adımları yürütmeniz gerekmez.

5. [VM 'Ler için çoğaltmayı ayarlama](hyper-v-azure-tutorial.md)
6. İsteğe bağlı ancak önerilir: [Dr detayına git](tutorial-dr-drill-azure.md)

Hyper-V çoğaltmasını kullanma seçeneği 2 ' yi seçerek, aşağıdaki adımları yürütün:

1. **Çoğaltılmış**öğeler > **korunan öğeler** ' de, **çoğaltmayı devre dışı bırakmak**> makineye sağ tıklayın.
2. **Çoğaltmayı devre dışı bırak**' da **Kaldır**' ı seçin.

    Bu, çoğaltılan öğeyi Azure Site Recovery kaldırır (Faturalandırma durdurulur). Şirket içi **sanal makinede çoğaltma yapılandırması temizlenmeyecektir.** 

## <a name="next-steps"></a>Sonraki adımlar
Kullanımdan kaldırma planlayın ve altyapınız ve işletmeniz için en uygun alternatif bir seçenek belirleyin. Bu ile ilgili herhangi bir sorgunuz varsa Microsoft Desteği ulaşın

