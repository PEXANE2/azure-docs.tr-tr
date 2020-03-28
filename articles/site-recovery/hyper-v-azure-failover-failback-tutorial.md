---
title: Azure Site Kurtarma'da Hyper-V VM'lerin Azure'a başarısız olmasını ayarlama
description: Azure Site Kurtarma ile Hyper-V VM'ler üzerinden Azure'a nasıl geçemeyeceğinizi öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/16/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 03826abf6da94859c510f4c127dfce035aa79370
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75498166"
---
# <a name="fail-over-hyper-v-vms-to-azure"></a>Hyper-V V'ler üzerinden Azure'a geçemedi

Bu öğretici, [Azure Site Kurtarma](site-recovery-overview.md)ile Hyper-V VM'ler üzerinden Azure'a nasıl başarısız olunan açıklanmaktadır. Yük devrettikten sonra şirket içi siteniz kullanılabilir olduğunda yeniden çalıştırabilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure gereksinimlerine uygun olup olduğunu kontrol etmek için Hyper-V VM özelliklerini doğrulayın.
> * Azure'a belirli VM'ler üzerinde başarısız olun.


Bu öğretici, serideki beşinci öğreticidir. Önceki öğreticilerdeki adımları zaten tamamladığınız varsayılır.    

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi Hyper-V’leri hazırlama](tutorial-prepare-on-premises-hyper-v.md)
3. [Hyper-V VM'leri](tutorial-hyper-v-to-azure.md) veya [System Center VMM bulutlarında yönetilen Hyper-V VM'leri](tutorial-hyper-v-vmm-to-azure.md) için olağanüstü durum kurtarmayı ayarlama
4. [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)

Farklı başarısızlık türleri [hakkında bilgi edinin.](failover-failback-overview.md#types-of-failover) Kurtarma planında birden çok VM üzerinde başarısız olmak istiyorsanız, [bu makaleyi](site-recovery-failover.md)gözden geçirin.

## <a name="prepare-for-failover"></a>Yük devretme hazırlığı 
VM'de anlık görüntü olmadığından ve şirket içi VM'nin yeniden çalışma sırasında kapalı olduğundan emin olun. Bu işlem, çoğaltma sırasında veri tutarlığını sağlar. Yeniden çalışma sırasında şirket içi VM'yi açmayın. 

Yük devretme ve yeniden çalışma üç aşamalıdır:

1. **Azure'a yük devretme**: Şirket içi Hyper-V VM'lerinden Azure'a yük devretme gerçekleştirin.
2. **Şirket içinde yeniden çalışma**: Şirket içi site kullanılabilir duruma geldiğinde Azure VM'lerinden şirket içine yük devretme gerçekleştirin. Azure'daki veriler şirket içi siteyle eşitlenmeye başlar ve tamamlandığında şirket içindeki VM'ler açılır.  
3. **Şirket içi VM'lerde tersine çoğaltma**: Şirket içinde yeniden çalışma süreci tamamlandıktan sonra şirket içindeki VM'leri Azure'da çoğaltmaya başlamak için tersine çoğaltma gerçekleştirin.

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

Yük devretme öncesinde VM özelliklerini doğrulayın ve VM’nin [Azure gereksinimlerini](hyper-v-azure-support-matrix.md#replicated-vms) karşıladığından emin olun.

**Korunan Öğeler**’de **Çoğaltılan Öğeler** > VM seçeneğine tıklayın.

1. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.

1. **Bilgi İşlem ve Ağ'da,** Azure adını, kaynak grubunu, hedef boyutunu, [kullanılabilirlik kümesini](../virtual-machines/windows/tutorial-availability-sets.md)ve yönetilen disk ayarlarını değiştirebilirsiniz.

1. Ağ ayalarını, yük devretmeden sonra Azure VM’nin yerleştirileceği ağ/alt ağ ve VM’ye atanacak IP adresi dahil olmak üzere görüntüleyebilir ve değiştirebilirsiniz.

1. **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="fail-over-to-azure"></a>Azure'a yük devretme

1. **Ayarlar** > **Çoğaltılan öğelerde,** VM > **Failover'ı**tıklatın.
2. **Yük devretme** bölümünde **En son** kurtarma noktasını seçin. 
3. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery, yük devretmeyi tetiklemeden önce kaynak VM'leri kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
4. Yük devretmeyi doğruladıktan sonra **Yürüt**'e tıklayın. Bu işlem tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Devam eden bir yük devretme işlemini iptal etmeyin**: Devam eden bir işlemi iptal ederseniz yük devretme durdurulur ancak VM yeniden çoğaltılmaz.

## <a name="connect-to-failed-over-vm"></a>Başarısız vm'ye bağlanma

1. Uzak Masaüstü Protokolü (RDP) ve Secure Shell (SSH) kullanarak başarısız olduktan sonra Azure VM'lerine bağlanmak istiyorsanız, [gereksinimlerin karşılandığını doğrulayın.](failover-failback-overview.md#connect-to-azure-after-failover)
2. Başarısız olduktan sonra, VM'ye gidin ve [bağlanarak doğrulayın.](../virtual-machines/windows/connect-logon.md)
3. Başarısız olduktan sonra farklı bir kurtarma noktası kullanmak **istiyorsanız, Kurtarma noktasını değiştir'i** kullanın. Bir sonraki adımda başarısız olduktan sonra, bu seçenek artık kullanılamayacaktır.
4. Doğrulamadan sonra, başarısız olduktan sonra VM'nin kurtarma noktasını sonuçlandırmak için **Commit'i** seçin.
5. Commit'den sonra, kullanılabilir tüm kurtarma noktaları silinir. Bu adım, başarısızı tamamlar.

>[!TIP]
> Başarısız olduktan sonra herhangi bir bağlantı sorunuyla karşılaşırsanız, [sorun giderme kılavuzunu](site-recovery-failover-to-azure-troubleshoot.md)izleyin.


## <a name="next-steps"></a>Sonraki adımlar

Başarısız olduktan sonra, Azure VM'lerini Azure'dan şirket içinde çoğaltmak için yeniden koruyun. Ardından, VM'ler yeniden korunduktan ve şirket içi siteye çoğaltıldıktan sonra, hazır olduğunuzda Azure'dan geri alının.
