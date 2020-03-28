---
title: Azure Site Kurtarma ile Azure'da olağanüstü durum kurtarma matkabı çalıştırın
description: Azure Site Kurtarma ile şirket içi ve Azure'a olağanüstü durum kurtarma matkabını nasıl çalıştırılayarınızı öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/12/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 5bd9f5316f8b8799633de8c0c84c61424c0e4f4a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238905"
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Azure’da olağanüstü durum kurtarma tatbikatı çalıştırma

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanarak şirket içi bir makineiçin Azure'a olağanüstü durum kurtarma demlemesi nasıl çalıştırılacak açıklanmaktadır. Tatbikat, veri kaybı olmadan çoğaltma stratejinizi doğrular.


Bu, şirket içi makineler için Azure'da olağanüstü durum kurtarma yı nasıl ayarladığınızı gösteren bir serinin dördüncü öğreticisidir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Yük devretme testi için yalıtılmış bir ağ ayarlama
> * Yük devretme sonrasında Azure VM'ye bağlanmak için hazırlık yapma
> * Tek bir makine için bir test başarısızlığını çalıştırın.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Olağanüstü durum kurtarma tatbikat adımları hakkında daha ayrıntılı bilgi edinmek istiyorsanız, [bu makaleyi inceleyin.](site-recovery-test-failover-to-azure.md)

## <a name="before-you-start"></a>Başlamadan önce

Önceki öğreticileri tamamlayın:

1. Azure'u, Azure'da VMware VM'lerin, Hyper-V VM'lerin ve fiziksel makinelerin şirket içi olağanüstü durum kurtarma sıiçin [ayarladığınızdan](tutorial-prepare-azure.md) emin olun.
2. Şirket içi [VMware](vmware-azure-tutorial-prepare-on-premises.md) veya [Hyper-V](hyper-v-prepare-on-premises-tutorial.md) ortamınızı olağanüstü durum kurtarma için hazırlayın. Fiziksel sunucular için olağanüstü durum kurtarma ayarlıyorsanız, [destek matrisini](vmware-physical-secondary-support-matrix.md)gözden geçirin.
3. [VMware VM'ler,](vmware-azure-tutorial.md) [Hyper-V VM'ler](hyper-v-azure-tutorial.md)veya [fiziksel makineler](physical-azure-disaster-recovery.md)için olağanüstü durum kurtarma yı ayarlayın.
 

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

Bir yük devretme testi çalıştırmadan önce VM özelliklerini doğrulayın ve [Hyper-V VM](hyper-v-azure-support-matrix.md#replicated-vms), [VMware VM](vmware-physical-azure-support-matrix.md#replicated-machines)'nin Azure gereksinimlerine uygun olduğundan emin olun.

1. **Korunan Öğeler**’de **Çoğaltılan Öğeler** > VM seçeneğine tıklayın.
2. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.
3. **Bilgi İşlem ve Ağ'da,** Azure adını, kaynak grubunu, hedef boyutunu, kullanılabilirlik kümesini ve yönetilen disk ayarlarını değiştirebilirsiniz.
4. Ağ ayalarını, yük devretmeden sonra Azure VM’nin yerleştirileceği ağ/alt ağ ve VM’ye atanacak IP adresi dahil olmak üzere görüntüleyebilir ve değiştirebilirsiniz.
5. **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="create-a-network-for-test-failover"></a>Yük devretme testi için ağ oluşturma

Yük devretme testi için her VM'nin **İşlem ve Ağ** ayarlarından üretim kurtarma sitesi ağınızdan yalıtılmış bir ağ seçmenizi öneririz. Varsayılan olarak yeni oluşturduğunuz Azure sanal ağları diğer ağlardan yalıtılmış olur. Test ağı üretim ağınıza benzer olmalıdır:

- Test ağında üretim ağınızdaki sayıda alt ağ olmalıdır. Alt ağların adı aynı olmalıdır.
- Test ağında aynı IP adresi aralığı kullanılmalıdır.
- Test ağının DNS sunucusunu **İşlem ve Ağ** ayarlarında DNS VM için belirtilen IP adresiyle güncelleştirin. Daha fazla ayrıntı için [Active Directory için yük devretme testi ile ilgili dikkat edilmesi gerekenler](site-recovery-active-directory.md#test-failover-considerations) sayfasını okuyun.

## <a name="run-a-test-failover-for-a-single-vm"></a>Tek bir VM için yük devretme testi çalıştırma

Yük devretme testi çalıştırdığınızda şunlar olur:

1. Yük devretme için gerekli tüm koşulların karşılandığından emin olmak için bir önkoşul denetimi çalıştırılır.
2. Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
3. Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.

Yük devretme testini aşağıdaki gibi çalıştırın:

1. **Yinelenen Ayarlar'da** > **Replicated Items**VM > **+Test Failover'ı**tıklatın.
2. Bu öğretici için **Son işlenen** kurtarma noktasını seçin. Bu işlem VM yükünü zamandaki en son kullanılabilir noktaya devreder. Zaman damgası gösterilir. Bu seçenekte, verileri işlemek için zaman harcanmadığından düşük bir RTO (kurtarma süresi hedefi) sağlanır.
3. **Yük Devretme Testi** bölümünde, yük devretme gerçekleştikten sonra Azure VM’lerinin bağlanacağı hedef Azure ağını seçin.
4. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. Sanal makine özelliklerini açmak için sanal makineye tıklayarak ilerleme durumunu izleyebilirsiniz. Ya da kasa adı > **Ayarlar** > **İşler** >
   **Site Recovery işleri** bölümünde **Yük Devretme Testi** işine tıklayabilirsiniz.
5. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. Sanal makinenin uygun boyutta olduğundan, doğru ağa bağlandığından ve çalıştığından emin olun.
6. Şimdi Azure’da çoğaltılan sanal makineye bağlanabiliyor olmanız gerekir.
7. Yük devretme testi sırasında oluşturulan Azure sanal makinelerini silmek için, VM’de **Yük devretme testini temizle**’ye tıklayın. **Notlar**’da, yük devretme testiyle ilişkili gözlemlerinizi kaydedin ve saklayın.

Bazı senaryolarda yük devretme için sekiz ila on dakikada tamamlanan ek işlem gerekebilir. VMware Linux makinelerinde, DHCP hizmeti etkinleştirilmemiş VMware VM’lerinde ve storvsc, vmbus, storflt, intelide, atapi önyükleme sürücülerine sahip olmayan VMware VM’lerinde uzun yük devretme testi süreleriyle karşılaşabilirsiniz.

## <a name="connect-after-failover"></a>Başarısız olduktan sonra bağlan

Başarısız olduktan sonra RDP/SSH kullanarak Azure VM'lerine bağlanmak istiyorsanız, [bağlanmaya hazırlanın.](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) Başarısız olduktan sonra herhangi bir bağlantı sorunuyla karşılaşırsanız, [sorun giderme](site-recovery-failover-to-azure-troubleshoot.md) kılavuzunu izleyin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [VMware VM'ler](vmware-azure-tutorial-failover-failback.md)
> için bir failover ve failback
> [çalıştırın Hyper-V VM'ler için bir failover ve failback](hyper-v-azure-failover-failback-tutorial.md)[çalıştırın Fiziksel makineler için bir failover ve failback çalıştırın](physical-to-azure-failover-failback.md)
