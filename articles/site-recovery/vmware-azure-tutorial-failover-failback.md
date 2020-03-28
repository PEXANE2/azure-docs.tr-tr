---
title: Site Kurtarma ile VMware VM'ler üzerinden Azure'a geçemedi
description: Azure Site Kurtarma'da VMware VM'ler üzerinden Azure'a nasıl başarısız olunmayı öğrenin
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/16/2019
ms.custom: MVC
ms.openlocfilehash: 8501bb1a998eb08984a118bfa5d52d1e3f3e4f84
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75498085"
---
# <a name="fail-over--vmware-vms"></a>VMware VM'ler üzerinde başarısız

Bu makalede, azure site kurtarma ile şirket içi VMware sanal makine (VM) üzerinden [Azure'a](site-recovery-overview.md)nasıl başarısız olunan açıklanmaktadır.

Bu, şirket içi makineler için Azure'da olağanüstü durum kurtarma yı nasıl ayarladığınızı gösteren bir serinin beşinci öğreticisidir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * VMware VM özelliklerinin Azure gereksinimleriyle uyumlu olduğunu doğrulayın.
> * Azure'a belirli VM'ler üzerinde başarısız olun.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Mümkün olduğunda varsayılan seçenekleri kullanırlar ve olası tüm ayarları ve yolları göstermezler. Failover hakkında ayrıntılı bilgi edinmek istiyorsanız, [VM'ler ve fiziksel sunucular üzerinden Başarısız'a](site-recovery-failover.md)bakın.

Farklı başarısızlık türleri [hakkında bilgi edinin.](failover-failback-overview.md#types-of-failover) Kurtarma planında birden çok VM üzerinde başarısız olmak istiyorsanız, [bu makaleyi](site-recovery-failover.md)gözden geçirin.

## <a name="before-you-start"></a>Başlamadan önce

Önceki öğreticileri tamamlayın:

1. Azure'u, Azure'da VMware VM'lerin, Hyper-V VM'lerin ve fiziksel makinelerin şirket içi olağanüstü durum kurtarma sıiçin [ayarladığınızdan](tutorial-prepare-azure.md) emin olun.
2. Şirket içi [VMware](vmware-azure-tutorial-prepare-on-premises.md) ortamınızı olağanüstü durum kurtarma için hazırlayın. 
3. [VMware VM'ler](vmware-azure-tutorial.md)için olağanüstü durum kurtarma ayarlayın.
4. Her şeyin beklendiği gibi çalıştığından emin olmak için bir [felaket kurtarma tatbikatı](tutorial-dr-drill-azure.md) yapın.

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

Başarısız olmadan önce, VM'lerin [Azure gereksinimlerini](vmware-physical-azure-support-matrix.md#replicated-machines)karşıladığından emin olmak için VM özelliklerini denetleyin.

Özellikleri aşağıdaki gibi doğrulayın:

1. **Korumalı Öğeler'de Çoğaltılan** **Öğeler'i**seçin ve ardından doğrulamak istediğiniz VM'yi seçin.

2. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Daha fazla ayrıntıyı görüntülemek için **Özellikler'i** seçin.

3. **İşlem ve Ağ'da,** bu özellikleri gerektiği gibi değiştirebilirsiniz:
    * Azure adı
    * Kaynak grubu
    * Hedef boyutu
    * [Kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md)
    * Yönetilen disk ayarları

4. Ağ ayarlarını görüntüleyebilir ve değiştirebilirsiniz:

    * Azure VM'nin başarısız olduktan sonra bulunacağı ağ ve alt ağ.
    * Ona atanacak IP adresi.

5. **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="run-a-failover-to-azure"></a>Azure'a yük devretme işlemini çalıştırma

1. Yinelenen**Ayarlar'da,** **Settings** > başarısız olmak istediğiniz VM'yi seçin ve ardından **Failover'ı**seçin.
2. **Failover'da,** başarısız olmak için bir **Kurtarma Noktası** seçin. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
   * **Varsayılan**: Bu seçenekte öncelikle Site Recovery’ye gönderilen tüm veriler işlenir. Başarısız olduktan sonra oluşturulan Azure VM,başarısız olduğunda Site Kurtarma'ya çoğaltılan tüm verilere sahip olduğundan, en düşük Kurtarma Noktası Hedefi 'ni (RPO) sağlar.
   * **En son işlenen**: Bu seçenek, Site Kurtarma tarafından işlenen en son kurtarma noktasına VM başarısız olur. İşlenmemiş verileri işlemek için zaman harcandığından, bu seçenek düşük bir RTO (Kurtarma Süresi Hedefi) sağlar.
   * **En son uygulama tutarlılığı**: Bu seçenek, Site Kurtarma tarafından işlenen en son uygulama tutarlı kurtarma noktasına VM'de başarısız olur.
   * **Özel**: Bu seçenek, bir kurtarma noktası belirtmenizi sağlar.

3. Başarısız olmadan önce kapatma **makinesini** seçin ve başarısız olmayı tetiklemeden önce kaynak VM'leri kapatmayı deneyin. Kapatma başarısız olsa bile failover devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.

Bazı senaryolarda, failover tamamlanması yaklaşık 8 ila 10 dakika sürer ek işleme gerektirir. Daha uzun test başarısız kalma süreleri fark edebilirsiniz:

* 9,8'den eski bir Mobilite hizmet sürümü çalıştıran VMware VM'ler.
* Fiziksel sunucular.
* VMware Linux VM'ler.
* Hyper-V VM'ler fiziksel sunucu lar olarak korunur.
* DHCP hizmeti etkin olmayan VMware VM'ler.
* Aşağıdaki önyükleme sürücüleri olmayan VMware VM'ler: storvsc, vmbus, storflt, intelide, atapi.

> [!WARNING]
> Devam etmekte olan bir başarısızı iptal etmeyin. Yük devretme başlatılmadan önce VM çoğaltması durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılamaz.

## <a name="connect-to-failed-over-vm"></a>Başarısız vm'ye bağlanma

1. Uzak Masaüstü Protokolü (RDP) ve Secure Shell (SSH) kullanarak başarısız olduktan sonra Azure VM'lerine bağlanmak istiyorsanız, [gereksinimlerin karşılandığını doğrulayın]((ailover-failback-overview.md#connect-to-azure-after-failover).
2. Başarısız olduktan sonra, VM'ye gidin ve [bağlanarak doğrulayın.](../virtual-machines/windows/connect-logon.md)
3. Başarısız olduktan sonra farklı bir kurtarma noktası kullanmak **istiyorsanız, Kurtarma noktasını değiştir'i** kullanın. Bir sonraki adımda başarısız olduktan sonra, bu seçenek artık kullanılamayacaktır.
4. Doğrulamadan sonra, başarısız olduktan sonra VM'nin kurtarma noktasını sonuçlandırmak için **Commit'i** seçin.
5. Commit'den sonra, kullanılabilir tüm kurtarma noktaları silinir. Bu adım, başarısızı tamamlar.

>[!TIP]
> Başarısız olduktan sonra herhangi bir bağlantı sorunuyla karşılaşırsanız, [sorun giderme kılavuzunu](site-recovery-failover-to-azure-troubleshoot.md)izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Başarısız olduktan sonra, Azure VM'leri şirket içinde yeniden koruyun. Ardından, VM'ler yeniden korunduktan ve şirket içi siteye çoğaltıldıktan sonra, hazır olduğunuzda Azure'dan geri alının.

> [!div class="nextstepaction"]
> [Azure VM'leri](vmware-azure-reprotect.md)
> Yeniden Koruyun[Azure'dan Başarısız](vmware-azure-failback.md)
