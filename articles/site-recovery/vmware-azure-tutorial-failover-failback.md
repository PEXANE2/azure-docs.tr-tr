---
title: Site Recovery ile VMware VM 'lerinden Azure 'a yük devretme
description: VMware VM 'lerini yük devretme Azure Site Recovery Azure 'a nasıl devretireceğinizi öğrenin
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/16/2019
ms.custom: MVC
ms.openlocfilehash: 8501bb1a998eb08984a118bfa5d52d1e3f3e4f84
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/26/2019
ms.locfileid: "75498085"
---
# <a name="fail-over--vmware-vms"></a>VMware VM 'lerini devreder

Bu makalede, şirket içi VMware sanal makinesinin (VM) [Azure Site Recovery](site-recovery-overview.md)ile Azure 'a yük devretme işlemi açıklanır.

Bu, şirket içi makineler için Azure 'da olağanüstü durum kurtarmanın nasıl ayarlanacağını gösteren bir serideki beşinci öğreticidir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VMware VM özelliklerinin Azure gereksinimleriyle uyumlu olduğunu doğrulayın.
> * Belirli VM 'Lerin yükünü Azure 'a devreder.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yük devretme hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [VM 'ler ve fiziksel sunucular](site-recovery-failover.md).

Farklı yük devretme türleri [hakkında bilgi edinin](failover-failback-overview.md#types-of-failover) . Bir kurtarma planında birden çok VM 'nin yükünü devretmek istiyorsanız [Bu makaleyi](site-recovery-failover.md)gözden geçirin.

## <a name="before-you-start"></a>Başlamadan önce

Önceki öğreticilerini doldurun:

1. VMware VM 'Leri, Hyper-V VM 'Leri ve fiziksel makinelerin Azure 'a şirket içi olağanüstü durum kurtarması için [Azure 'u ayarladığınızdan](tutorial-prepare-azure.md) emin olun.
2. Şirket içi [VMware](vmware-azure-tutorial-prepare-on-premises.md) ortamınızı olağanüstü durum kurtarma için hazırlayın. 
3. [VMware VM 'leri](vmware-azure-tutorial.md)için olağanüstü durum kurtarmayı ayarlayın.
4. Her şeyin beklendiği gibi çalıştığından emin olmak için bir [olağanüstü durum kurtarma ayrıntısı](tutorial-dr-drill-azure.md) çalıştırın.

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

Yük devretmeyi çalıştırmadan önce VM 'Lerin [Azure gereksinimlerini](vmware-physical-azure-support-matrix.md#replicated-machines)karşıladığından emin olmak için VM özelliklerini denetleyin.

Özellikleri aşağıdaki gibi doğrulayın:

1. **Korunan öğeler**' de **çoğaltılan öğeler**' i seçin ve ardından doğrulamak istediğiniz VM 'yi seçin.

2. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Daha fazla ayrıntı görüntülemek için **Özellikler** ' i seçin.

3. **İşlem ve ağ**bölümünde, bu özellikleri gerektiği şekilde değiştirebilirsiniz:
    * Azure adı
    * Kaynak grubu
    * Hedef boyutu
    * [Kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md)
    * Yönetilen disk ayarları

4. Aşağıdakiler dahil olmak üzere ağ ayarlarını görüntüleyebilir ve değiştirebilirsiniz:

    * Yük devretmeden sonra Azure VM 'nin bulunduğu ağ ve alt ağ.
    * Kendisine atanacak IP adresi.

5. **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="run-a-failover-to-azure"></a>Azure'a yük devretme işlemini çalıştırma

1. **Yinelenen öğeler** > **Ayarlar** ' da, yük devretmek istediğiniz VM 'Yi seçin ve ardından **Yük devretme**' yı seçin.
2. **Yük devretme** kısmında, yük devredeceğiniz bir **Kurtarma Noktası** seçin. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
   * **Varsayılan**: Bu seçenekte öncelikle Site Recovery’ye gönderilen tüm veriler işlenir. Yük devretmeden sonra oluşturulan Azure VM, yük devretme tetiklendiğinde Site Recovery çoğaltılan tüm verilere sahip olduğundan en düşük kurtarma noktası hedefini (RPO) sağlar.
   * **En son işlenen**: Bu seçenek VM 'yi Site Recovery tarafından işlenen en son kurtarma noktasına devreder. İşlenmemiş verileri işlemek için zaman harcanmadığından, bu seçenek düşük bir RTO (kurtarma süresi hedefi) sağlar.
   * **En son uygulamayla tutarlı**: Bu seçenek VM 'yi, Site Recovery tarafından işlenen en son uygulamayla tutarlı kurtarma noktasına devreder.
   * **Özel**: Bu seçenek, bir kurtarma noktası belirtmenizi sağlar.

3. Yük devretmeyi tetiklemeden önce kaynak VM 'Leri kapatmayı denemek için **yük devretmeye başlamadan önce makineyi kapat** ' ı seçin. Yük devretme, kapatılma başarısız olsa bile devam eder. Yük devretme işlemini **İşler** sayfasında takip edebilirsiniz.

Bazı senaryolarda, yük devretme işleminin tamamlanabilmesi için 8 ila 10 dakika geçen ek işleme gerekir. İçin yük devretme sürelerinin daha uzun süre test edebilirsiniz:

* 9,8 'den eski bir Mobility hizmeti sürümü çalıştıran VMware VM 'Leri.
* Fiziksel sunucular.
* VMware Linux VM 'Leri.
* Fiziksel sunucu olarak korunan Hyper-V VM 'Leri.
* DHCP hizmeti etkinleştirilmemiş VMware VM 'Leri.
* Şu önyükleme sürücülerine sahip olmayan VMware VM 'Leri: storvsc, VMBus, storflt, intelide, Atapi.

> [!WARNING]
> Devam eden bir yük devretme işlemini iptal etmeyin. Yük devretme başlatılmadan önce VM çoğaltması durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.

## <a name="connect-to-failed-over-vm"></a>Yük devredilen VM 'ye bağlanma

1. Yük devretmeden sonra Uzak Masaüstü Protokolü (RDP) ve Secure Shell (SSH) kullanarak Azure VM 'lerine bağlanmak istiyorsanız, [gereksinimlerin karşılandığını doğrulayın] ((aılover-yeniden çalışma-genel bakış. MD # Connect-to-Azure-After-yük devretme).
2. Yük devretmeden sonra VM 'ye gidin ve bu ağa [bağlanarak](../virtual-machines/windows/connect-logon.md) doğrulayın.
3. Yük devretmeden sonra farklı bir kurtarma noktası kullanmak istiyorsanız, **değişiklik kurtarma noktasını** kullanın. Bir sonraki adımda yük devretmeyi kaydettikten sonra bu seçenek artık kullanılamaz.
4. Doğrulamadan sonra, yük devretmeden sonra sanal makinenin kurtarma noktasını sonlandırmak için **Yürüt** ' ü seçin.
5. Kaydettikten sonra, tüm kullanılabilir kurtarma noktaları silinir. Bu adım yük devretmeyi tamamlar.

>[!TIP]
> Yük devretmeden sonra herhangi bir bağlantı sorunlarıyla karşılaşırsanız, [sorun giderme kılavuzunu](site-recovery-failover-to-azure-troubleshoot.md)izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Yük devretmeden sonra Azure VM 'lerini şirket içine yeniden koruyun. Ardından, VM 'Ler yeniden korunduktan ve şirket içi siteye çoğaltıldıktan sonra, hazırsanız Azure 'dan yeniden yük devreder.

> [!div class="nextstepaction"]
> Azure [VM 'Leri yeniden koruma](vmware-azure-reprotect.md)
> [Azure 'dan geri](vmware-azure-failback.md) dönme
