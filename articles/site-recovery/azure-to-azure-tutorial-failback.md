---
title: Azure Site Kurtarma hizmetiyle Azure VM'lerini birincil bölgeye geri getirin.
description: Azure Site Kurtarma hizmetiyle Azure VM'leri birincil bölgeye nasıl geri döndüreceklerini açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/14/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: c27b7bf29e5f124fdcfb886b658fd8e9d4cc48fe
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74091356"
---
# <a name="fail-back-an-azure-vm-between-azure-regions"></a>Azure bölgeleri arasında bir Azure VM'yi geri başarısız

[Azure Site Kurtarma](site-recovery-overview.md) hizmeti, şirket içi makineleri ve Azure sanal makinelerini (VM) yöneterek ve düzenleyerek olağanüstü durum kurtarma stratejinize katkıda bulunur.

Bu öğretici, tek bir Azure VM'ini nasıl geri almayacağımı açıklar. Başarısız olduktan sonra, kullanılabilir olduğunda birincil bölgeye geri dönmeniz gerekir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> 
> * İkincibölgedeki VM'yi geri başarısız edin.
> * Birincil VM'yi ikincil bölgeye yeniden koruyun.
> 
> [!NOTE]
> 
> Bu öğretici, hedef bölgeye birkaç VM üzerinden başarısız ve en az özelleştirmeler ile kaynak bölgeye geri yardımcı olur. Daha ayrıntılı talimatlar için [Azure VM'lerde nasıl yap'ın nasıl yapılacağını gözden](https://docs.microsoft.com/azure/virtual-machines/windows/)geçirin.

## <a name="before-you-start"></a>Başlamadan önce

* VM'nin durumunun **Failover olduğundan**emin olun.
* Birincil bölgenin kullanılabilir olup olmadığını ve içinde yeni kaynaklar oluşturup erişebildiğinizi denetleyin.
* Yeniden korumanın etkin olduğundan emin olun.

## <a name="fail-back-to-the-primary-region"></a>Birincil bölgeye geri dönme

VM'ler yeniden korunduktan sonra, gerektiğinde birincil bölgeye geri dönebilirsiniz.

1. **Kasada, Çoğaltılan öğeleri**seçin ve sonra yeniden korunan VM'yi seçin.

    ![Birincil başarısız](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

2. **Çoğaltılan öğelerde,** VM'yi seçin ve ardından **Failover'ı**seçin.
3. **Failover'da,** başarısız olmak için bir kurtarma noktası seçin:
    - **En son (varsayılan)**: Site Kurtarma hizmetindeki tüm verileri işler ve en düşük kurtarma noktası hedefini (RPO) sağlar.
    - **En son işlenen**: VM'yi Site Kurtarma tarafından işlenen en son kurtarma noktasına geri döner.
    - **Özel**: Belirli bir kurtarma noktasına kadar başarısız olur. Bu seçenek, bir yük devretme testi gerçekleştirmek için faydalıdır.
4. Site Kurtarma'nın failover'ı tetiklemeden önce DR bölgesindeki VM'leri kapatmayı denemesini istiyorsanız, başarısız olmadan **önce kapatma makinesini** seçin. Kapatma başarısız olsa bile hata devam eder. 
5. Yük devretme ilerleme durumunu **İşler** sayfasından takip edin.
6. Başarısızlık tamamlandıktan sonra, vm'de oturum açarak vm'yi doğrulayın. Gerektiğinde kurtarma noktasını değiştirebilirsiniz.
7. Başarısızı doğruladıktan sonra, **başarısızı gerçekleştir'i**seçin. Taahhüt, kullanılabilir tüm kurtarma noktalarını siler. Kurtarma noktası nı değiştir seçeneği artık kullanılamıyor.
8. VM üzerinde başarısız ve geri başarısız olarak göstermelidir.

    ![Birincil ve ikincil bölgelerde VM](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

> [!NOTE]
> Site Kurtarma uzantısı sürümü 9.28.x.x çalıştıran makineler için [güncelleştirme toplama 40](https://support.microsoft.com/help/4521530/update-rollup-40-for-azure-site-recovery) Site Kurtarma ikincil felaket kurtarma bölgesinde makineleri temizler, geri başarısız ve VM'ler yeniden korunduktan sonra. İkinci bölgedeki VM'leri ve NIC'leri el ile silmenize gerek yoktur. Geri başarısız olduktan sonra çoğaltmayı tamamen devre dışı ederseniz, Site Kurtarma, VM'lere ve NIC'lere ek olarak olağanüstü durum kurtarma bölgesindeki diskleri temizler.

## <a name="next-steps"></a>Sonraki adımlar

Yeniden koruma akışı hakkında [daha fazla bilgi edinin.](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection)
