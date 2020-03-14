---
title: VMware VM 'lerini Azure Site Recovery ile şirket içi bir siteye yeniden koruma
description: Azure Site Recovery ile Azure 'a yük devretmeden sonra VMware VM 'lerini yeniden nasıl koruyacağınızı öğrenin.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: mayg
ms.openlocfilehash: 976888f57269cc9fe6107a38e30d78c73eb5c124
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79257179"
---
# <a name="reprotect-from-azure-to-on-premises"></a>Azure’dan şirket içi ortama yeniden koruma

Şirket içi VMware VM 'lerini veya fiziksel sunucuları Azure 'a [yük devrettikten](site-recovery-failover.md) sonra, şirket içi sitenize geri dönme aşamasında ilk adım yük devretme sırasında oluşturulan Azure VM 'lerinin yeniden korunmasında olur. Bu makalede bunun nasıl yapılacağı açıklanır. 

## <a name="before-you-begin"></a>Başlamadan önce

1. Azure 'da bir işlem sunucusu ve şirket içi ana hedef sunucusu ayarlama ve bir siteden siteye VPN veya ExpressRoute özel eşlemesini yeniden çalışma için yapılandırma dahil, yeniden koruma ve yeniden çalışma için hazırlanmak üzere [Bu makaledeki](vmware-azure-prepare-failback.md) adımları izleyin.
2. Şirket içi yapılandırma sunucusunun çalıştığından ve Azure 'a bağlı olduğundan emin olun. Azure 'a yük devretme sırasında şirket içi siteye erişilemeyebilir ve yapılandırma sunucusu kullanılamıyor olabilir veya kapatılabilir. Yeniden çalışma sırasında VM 'nin yapılandırma sunucusu veritabanında mevcut olması gerekir. Aksi takdirde, yeniden çalışma başarısız olur.
3. Şirket içi ana hedef sunucusundaki tüm anlık görüntüleri silin. Anlık görüntüler varsa yeniden koruma çalışmaz.  Yeniden koruma işi sırasında VM 'deki anlık görüntüler otomatik olarak birleştirilir.
4. Çoklu VM tutarlılığı için bir çoğaltma grubuna toplanan VM 'Leri yeniden koruyorsanız, tümünün aynı işletim sistemine (Windows veya Linux) sahip olduklarından emin olun ve dağıttığınız ana hedef sunucunun aynı işletim sistemi türüne sahip olduğundan emin olun. Bir çoğaltma grubundaki tüm VM 'Lerin aynı ana hedef sunucuyu kullanması gerekir.
5. Yeniden çalışma için [gereken bağlantı noktalarını](vmware-azure-prepare-failback.md#ports-for-reprotectionfailback) açın.
6. Yeniden çalışma öncesinde vCenter Server bağlı olduğundan emin olun. Aksi takdirde, disklerin bağlantısının kesilmesi ve sanal makineye geri eklenmesi başarısız olur.
7. Bir vCenter sunucusu, geri dönecek VM 'Leri yönetirse, gerekli izinlere sahip olduğunuzdan emin olun. Salt okuma Kullanıcı vCenter bulmayı ve sanal makineleri koruyorsa, koruma başarılı olur ve yük devretme işlemi gerçekleştirilir. Bununla birlikte, yeniden koruma sırasında, veri depoları keşfedilmeden ve yeniden koruma sırasında listelenmediği için yük devretme başarısız olur. Bu sorunu çözmek için vCenter kimlik bilgilerini [uygun bir hesap/izinlerle](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)güncelleştirebilir ve ardından işi yeniden deneyebilirsiniz. 
8. Sanal makinelerinizi oluşturmak için bir şablon kullandıysanız, her VM 'nin diskler için kendi UUID 'sine sahip olduğundan emin olun. Aynı şablondan oluşturulduğundan, şirket içi VM UUID 'SI ana hedef sunucunun UUID 'SI ile çakışıyor, yeniden koruma başarısız olur. Farklı bir şablondan dağıtın.
9. Alternatif bir vCenter Server geri yüklüyorsanız, yeni vCenter Server ve ana hedef sunucunun bulunduğundan emin olun. Genellikle veri depoları erişilemez veya **yeniden koruma**bölümünde görünmez.
10. Geri gerçekleştiremeyecek aşağıdaki senaryoları doğrulayın:
    - ESXi 5,5 Free Edition ya da vSphere 6 hiper yönetici ücretsiz sürümü kullanıyorsanız. Farklı bir sürüme yükseltin.
    - Windows Server 2008 R2 SP1 fiziksel sunucunuz varsa.
    - VMware VM 'Leri Hyper-V ' d e geri yük devredemeyebilir.
    - [Geçirilen](migrate-overview.md#what-do-we-mean-by-migration)VM 'ler.
    - Başka bir kaynak grubuna taşınan bir VM.
    - Silinen bir çoğaltma Azure VM 'si.
    - Korunmayan bir çoğaltma Azure VM 'si (Şirket içi siteye çoğaltma).
10. Yeniden [çalışma türlerini gözden geçirin](concepts-types-of-failback.md) -özgün konum kurtarma ve alternatif konum kurtarma kullanabilirsiniz.


## <a name="enable-reprotection"></a>Yeniden korumayı etkinleştir

Çoğaltmayı etkinleştirin. Belirli VM 'Leri veya bir kurtarma planını yeniden koruyabilirsiniz:

- Bir kurtarma planını yeniden koruyorduysanız, korunan her makinenin değerlerini sağlamanız gerekir.
- VM 'Ler çoklu VM tutarlılığı için bir çoğaltma grubuna aitse, yalnızca bir kurtarma planı kullanılarak yeniden korunabilir. Bir çoğaltma grubundaki VM 'Lerin aynı ana hedef sunucuyu kullanması gerekir

### <a name="before-you-start"></a>Başlamadan önce

- Yük devretmeden sonra Azure 'da bir VM önyüklendiğinde, aracının yapılandırma sunucusuna (15 dakikaya kadar) kaydolması biraz zaman alır. Bu süre boyunca yeniden koruyamazsınız ve bir hata mesajı aracının yüklenmediğini gösterir. Bu durumda, birkaç dakika bekleyin ve sonra yeniden koruyun.
- Azure VM 'yi mevcut bir şirket içi VM 'ye geri yüklemek istiyorsanız, şirket içi VM veri depolarını, ana hedef sunucunun ESXi konağına okuma/yazma erişimiyle bağlayın.
- Alternatif bir konuma yeniden yük devretmek istiyorsanız, örneğin şirket içi VM yoksa, ana hedef sunucusu için yapılandırılan bekletme sürücüsünü ve veri deposunu seçin. Şirket içi siteye geri döndüğünüzde, yeniden çalışma koruma planındaki VMware sanal makineleri, ana hedef sunucuyla aynı veri deposunu kullanır. Daha sonra vCenter 'da yeni bir VM oluşturulur.

Yeniden korumayı aşağıdaki şekilde etkinleştirin:

1. **Çoğaltılan öğeleri** > **kasa** seçin. Yük devredilen sanal makineye sağ tıklayın ve ardından **yeniden koru**' yı seçin. Ya da, komut düğmelerinden makineyi seçip **yeniden koru**' yı seçin.
2. **Azure 'dan şirket içi** koruma yönünün seçildiğini doğrulayın.
3. **Ana hedef sunucusu** ve **işlem sunucusu**' nda, şirket içi ana hedef sunucusunu ve işlem sunucusunu seçin.  
4. **Veri deposu**için, şirket içi diskleri kurtarmak istediğiniz veri deposunu seçin. Bu seçenek, şirket içi sanal makine silindiğinde ve yeni diskler oluşturmanız gerektiğinde kullanılır. Diskler zaten mevcutsa bu seçenek yoksayılır. Yine de bir değer belirtmeniz gerekir.
5. Bekletme sürücüsünü seçin.
6. Yeniden çalışma ilkesi otomatik olarak seçilir.
7. Yeniden korumaya başlamak için **Tamam ' ı** seçin.

    ![Yeniden Koru iletişim kutusu](./media/vmware-azure-reprotect/reprotectinputs.png)
    
8. Bir iş, Azure VM 'yi şirket içi siteye çoğaltmaya başlar. **İşler** sekmesinde ilerleme durumunu izleyebilirsiniz.
    - Yeniden koruma başarılı olduğunda, VM korumalı bir duruma girer.
    - Şirket içi VM yeniden koruma sırasında kapatılır. Bu işlem, çoğaltma sırasında veri tutarlığını sağlar.
    - Yeniden koruma tamamlandıktan sonra şirket içi VM 'yi açık duruma getirin.
   

## <a name="next-steps"></a>Sonraki adımlar

- Herhangi bir sorunla karşılaşırsanız [sorun giderme makalesini](vmware-azure-troubleshoot-failback-reprotect.md)inceleyin.
- Azure VM 'Leri korunduktan sonra yeniden [çalışma çalıştırabilirsiniz](vmware-azure-failback.md). Yeniden çalışma, Azure VM 'yi kapatır ve şirket içi VM 'yi önyükler. Uygulama için bazı kesinti süreleri bekliyor ve uygun bir yeniden çalışma süresi seçin.


