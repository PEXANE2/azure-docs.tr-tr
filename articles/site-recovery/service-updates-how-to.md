---
title: Azure Site Kurtarma'da güncelleştirmeler ve bileşen yükseltmeleri
description: Azure Site Kurtarma hizmeti güncelleştirmelerine ve bileşen yükseltmelerine genel bakış sağlar.
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: rajanaki
ms.openlocfilehash: 3489f7f812798504d0c57a265a04e57344105419
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257829"
---
# <a name="service-updates-in-site-recovery"></a>Site Kurtarma'da hizmet güncelleştirmeleri

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) güncelleştirmelerine genel bir bakış sağlar ve Site Kurtarma bileşenlerinin nasıl yükseltilir şekilde yükseltilen anlatılmaktadır.

Site Kurtarma, hizmet güncelleştirmelerini düzenli olarak yayımlar. Güncelleştirmeler yeni özellikler, destek geliştirmeleri, bileşen güncelleştirmeleri ve hata düzeltmeleri içerir. En son özelliklerden ve düzeltmelerden yararlanmak için Site Kurtarma bileşenlerinin en son sürümlerini çalıştırmanızı öneririz. 
 
 
## <a name="updates-support"></a>Güncellemeler desteği

### <a name="support-statement-for-azure-site-recovery"></a>Azure Site Kurtarma için destek bildirimi

Her zaman en son bileşen sürümlerine yükseltmenizi öneririz:

**Bir Azure Site Kurtarma bileşeninin yayımlanan her yeni sürümü 'N' ile 'N-4' altındaki tüm sürümler desteksiz olarak kabul edilir.** 

> [!IMPORTANT]
> Resmi destek, > N-4 sürümünden N sürümüne yükseltme içindir. Örneğin, N-6'da çalıştırıyorsanız, önce N-4'e yükseltmeniz ve ardından N'ye yükseltmeniz gerekir.


### <a name="links-to-currently-supported-update-rollups"></a>Şu anda desteklenen güncelleştirme toplama bağlantıları

 [Bu makalede](site-recovery-whats-new.md)en son güncelleştirme toplama (sürüm N) gözden geçirin. Site Kurtarma'nın N-4 sürümleri için destek sağladığını unutmayın.



## <a name="component-expiry"></a>Bileşen in son kullanma tarihi

Site Kurtarma, süresi dolan bileşenleri (veya son kullanma tarihine yaklaşır) e-posta yoluyla (e-posta bildirimlerine abone olduysanız) veya portaldaki kasa panosunda size bildirir.

- Ayrıca, güncelleştirmeler kullanılabilir olduğunda, portaldaki senaryonuzun altyapı görünümünde bileşenin yanında **kullanılabilir** güncelleştirme düğmesi görüntülenir. Bu düğme, en son bileşen sürümünü indirmek için sizi bir bağlantıya yönlendirir.
-  Hyper-V VM'leri kopyalamıyorsanız, kasa pano bildirimleri kullanılamaz. 

E-posta bildirimleri aşağıdaki gibi gönderilir.

**Zaman** | **Frequency**
--- | ---
Bileşenin sona ermesinden 60 gün önce | Bir kez iki haftada bir
Sonraki 53 gün | Haftada bir kez
Son 7 gün | Günde bir kez
Sona erdikten sonra | Bir kez iki haftada bir


### <a name="upgrading-outside-official-support"></a>Resmi destek dışında yükseltme

Bileşen sürümünüz ile en son sürüm sürümünüz arasındaki fark dörtten büyükse, bu destek dışı olarak kabul edilir. Bu durumda, aşağıdaki gibi yükseltme: 

1. Şu anda yüklü olan bileşeni geçerli sürümünüze artı dört yükseltin. Örneğin, sürümünüz 9,16 ise, 9,20'ye yükseltin.
2. Ardından, bir sonraki uyumlu sürüme yükseltin. Örneğimizde, 9.16'yı 9.20'ye yükselttikten sonra 9.24'e yükseltin. 

İlgili tüm bileşenler için aynı işlemi izleyin.

### <a name="support-for-latest-operating-systemskernels"></a>En son işletim sistemleri/çekirdekler için destek

> [!NOTE]
> Zamanlanmış bir bakım pencereniz varsa ve yeniden başlatma da dahilse, önce Site Kurtarma bileşenlerini yükseltmenizi ve ardından bakım penceresinde zamanlanan etkinliklerin geri kalanına devam etmemi öneririz.

1. İşletim sistemi/çekirdek sürümlerini yükseltmeden önce, hedef sürümün Site Kurtarma'yı desteklenep desteklenmeyip desteklenmeyip desteklenmeyini doğrulayın. 

    - [Azure VM](azure-to-azure-support-matrix.md#replicated-machine-operating-systems) desteği.
    - [VMware/fiziksel sunucu](vmware-physical-azure-support-matrix.md#replicated-machines) desteği
    - [Hiper-V](hyper-v-azure-support-matrix.md#replicated-vms) desteği.
2. Neyi yükseltmek istediğinizi öğrenmek için [kullanılabilir güncelleştirmeleri](site-recovery-whats-new.md) gözden geçirin.
3. En son Site Kurtarma sürümüne yükseltin.
4. İşletim sistemini/çekirdeğini gerekli sürümlere yükseltin.
5. Yeni -den başlatma.


Bu işlem, makine işletim sisteminin/çekirdeğinin en son sürüme yükseltilmesini ve yeni sürümü desteklemek için gereken en son Site Kurtarma değişikliklerinin makineye yüklenmesini sağlar.

## <a name="azure-vm-disaster-recovery-to-azure"></a>Azure'a Azure VM olağanüstü durum kurtarma

Bu senaryoda, [otomatik güncelleştirmeleri etkinleştirmenizi](azure-to-azure-autoupdate.md)şiddetle öneririz. Site Kurtarma'nın güncelleştirmeleri aşağıdaki gibi yönetmesine izin verebilirsiniz:

- Çoğaltma yı etkinleştirme işlemi sırasında.
- Tonoz içindeki uzantı güncelleştirme ayarlarını ayarlayarak.

Güncelleştirmeleri el ile yönetmek istiyorsanız, aşağıdakileri yapın:

1. **Çoğaltılan Öğeler**> kasasında, ekranın üst kısmındaki bu bildirimi tıklatın: 
    
    **Yeni Site Kurtarma çoğaltma aracısı güncelleştirmesi kullanılabilir. Yüklemek için tıklayın ->**

4. Güncelleştirmeyi uygulamak istediğiniz VM'leri seçin ve ardından **Tamam'ı**tıklatın.


## <a name="vmware-vmphysical-server-disaster-recovery-to-azure"></a>Azure'a VMware VM/fiziksel sunucu olağanüstü durum kurtarma

1. Geçerli sürümünüze ve [destek bildiriminize](#support-statement-for-azure-site-recovery)bağlı olarak, [bu yönergeleri](vmware-azure-deploy-configuration-server.md#upgrade-the-configuration-server)kullanarak güncelleştirmeyi önce şirket içi yapılandırma sunucusuna yükleyin. 
2. Ölçeklendirme işlem sunucularınız varsa, bu yönergeleri kullanarak bunları daha sonra [güncelleştirin.](vmware-azure-manage-process-server.md#upgrade-a-process-server)
3. Korunan her makinedeki Mobilite aracısını güncelleştirmek için [bu](vmware-physical-manage-mobility-service.md#update-mobility-service-from-azure-portal) makaleye bakın.

### <a name="reboot-after-mobility-service-upgrade"></a>Mobilite hizmeti yükseltmeden sonra yeniden başlatın

En son değişikliklerin kaynak makineye yüklendiğinden emin olmak için Mobilite hizmetinin her yükseltmesi sonrasında yeniden başlatma önerilir.

Son yeniden başlatma sırasında aracı sürümü ile geçerli sürüm arasındaki fark dörtten büyük olmadığı sürece yeniden başlatma zorunlu değildir.

Tablodaki örnek, bunun nasıl çalıştığını gösterir.

|**Aracı sürümü (son yeniden başlatma)** | **Hedef yükseltme sürümü** | **Zorunlu yeniden başlatma?**|
|---------|---------|---------|
|9.16 |  9.18 | Zorunlu değil|
|9.16 | 9.19 | Zorunlu değil|
| 9.16 | 9.20 | Zorunlu değil
 | 9.16 | 9.21 | Zorunlu.<br/><br/> 9.20'ye yükseltin, sonra 9.21'e yükseltmeden önce yeniden başlatın.

## <a name="hyper-v-vm-disaster-recovery-to-azure"></a>Azure'a Hyper-V VM olağanüstü durum kurtarma

### <a name="between-a-hyper-v-site-and-azure"></a>Hyper-V sitesi ile Azure arasında

1. Güncelleştirmeyi Microsoft Azure Site Kurtarma Sağlayıcısı için indirin.
2. Sağlayıcıyı Site Kurtarma'da kayıtlı her Hyper-V sunucusuna yükleyin. Bir küme çalıştırıyorsanız, tüm küme düğümlerini yükseltin.


## <a name="between-an-on-premises-vmm-site-and-azure"></a>Şirket içi VMM sitesi ile Azure arasında
1. Güncelleştirmeyi Microsoft Azure Site Kurtarma Sağlayıcısı için indirin.
2. Sağlayıcıyı VMM sunucusuna yükleyin. VMM bir kümede dağıtılırsa, Sağlayıcıyı tüm küme düğümlerine yükleyin.
3. Tüm Hyper-V ana bilgisayarlarına veya küme düğümlerine en son Microsoft Azure Kurtarma Hizmetleri aracısını yükleyin.


## <a name="between-two-on-premises-vmm-sites"></a>İki şirket içi VMM sitesi arasında
1. Microsoft Azure Site Kurtarma Sağlayıcısı için en son güncelleştirmeyi indirin.
2. İkincil kurtarma sitesini yöneten VMM sunucusuna en son Sağlayıcıyı yükleyin. VMM bir kümede dağıtılırsa, Sağlayıcıyı tüm küme düğümlerine yükleyin.
3. Kurtarma sitesi güncelleştirildikten sonra, Sağlayıcıyı birincil siteyi yöneten VMM sunucusuna yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Yeni güncelleştirmeleri ve bültenleri izlemek için [Azure Güncellemeleri](https://azure.microsoft.com/updates/?product=site-recovery) sayfamızı takip edin.
