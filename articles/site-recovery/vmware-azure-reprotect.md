---
title: VMware VM'leri Azure Site Kurtarma ile şirket içi bir siteye yeniden koruma
description: Azure Site Kurtarma ile Azure'a geçtikten sonra VMware VM'leri nasıl yeniden koruyacağınızı öğrenin.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: mayg
ms.openlocfilehash: 976888f57269cc9fe6107a38e30d78c73eb5c124
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257179"
---
# <a name="reprotect-from-azure-to-on-premises"></a>Azure’dan şirket içi ortama yeniden koruma

Şirket içi VMware VM'lerinin veya fiziksel sunucuların Azure'da [başarısız](site-recovery-failover.md) olmasının ardından, şirket içi sitenize geri dönmenin ilk adımı, başarısız lık sırasında oluşturulan Azure VM'lerini yeniden korumaktır. Bu makalede, bunun nasıl yapılacağını açıklanmaktadır. 

## <a name="before-you-begin"></a>Başlamadan önce

1. Azure'da bir işlem sunucusu ve şirket içi ana hedef sunucu oluşturma ve siteden siteye VPN veya ExpressRoute özel eşlemesini hata geri almak için yapılandırmak da dahil olmak üzere yeniden koruma ve geri dönüşe hazırlanmak için [bu makaledeki](vmware-azure-prepare-failback.md) adımları izleyin.
2. Şirket içi yapılandırma sunucusunun çalışır durumda olduğundan ve Azure'a bağlı olduğundan emin olun. Azure'a geçerken şirket içi siteye erişilemeyebilir ve yapılandırma sunucusu kullanılamayabilir veya kapatılabilir. Geri hata sırasında, VM yapılandırma sunucusu veritabanında bulunmalıdır. Aksi takdirde, failback başarısız olur.
3. Şirket içi ana hedef sunucudaki anlık görüntüleri silin. Anlık görüntüler varsa yeniden koruma çalışmaz.  VM'deki anlık görüntüler, yeniden koruma işi sırasında otomatik olarak birleştirilir.
4. Çoklu VM tutarlılığı için bir çoğaltma grubuna toplanan VM'leri yeniden koruyorsanız, hepsinin aynı işletim sistemine (Windows veya Linux) sahip olduğundan emin olun ve dağıttığınız ana hedef sunucunun aynı işletim sistemi türüne sahip olduğundan emin olun. Çoğaltma grubundaki tüm VM'ler aynı ana hedef sunucuyu kullanmalıdır.
5. Geri dönüş için [gerekli bağlantı noktalarını](vmware-azure-prepare-failback.md#ports-for-reprotectionfailback) açın.
6. Başarısız olmadan önce vCenter Server'ın bağlı olduğundan emin olun. Aksi takdirde, disklerin bağlantısını kesmek ve sanal makineye takmak başarısız olur.
7. Bir vCenter sunucusu başarısız olduğunuz VM'leri yönetiyorsa, gerekli izinlere sahip olduğundan emin olun. Salt okunur kullanıcı vCenter bulma gerçekleştirin ve sanal makineleri korumak, koruma başarılı olur ve başarısız çalışır. Ancak, yeniden koruma sırasında, veri depoları bulunamadığından ve yeniden koruma sırasında listelenmediği için başarısız olur. Bu sorunu gidermek için, vCenter kimlik bilgilerini uygun bir [hesap/izinle](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)güncelleştirebilir ve ardından işi yeniden deneyebilirsiniz. 
8. Sanal makinelerinizi oluşturmak için bir şablon kullandıysanız, her VM'nin diskler için kendi UUID'si olduğundan emin olun. Her ikisi de aynı şablondan oluşturulduğu için şirket içi VM UUID ana hedef sunucunun UUID'si ile çakışıyorsa, yeniden koruma başarısız olur. Farklı bir şablondan dağıtın.
9. Alternatif bir vCenter Server'a geri dönülmeye çalışıyorsanız, yeni vCenter Server ve ana hedef sunucunun bulunduğundan emin olun. Genellikle veri depoları erişilemiyorsa veya **Reprotect'de**görülemiyorsa.
10. Başarısız olamadığınız aşağıdaki senaryoları doğrulayın:
    - ESXi 5.5 ücretsiz sürümü veya vSphere 6 Hypervisor ücretsiz sürümünü kullanıyorsanız. Farklı bir sürüme yükseltin.
    - Windows Server 2008 R2 SP1 fiziksel sunucunuz varsa.
    - VMware VM'ler Hyper-V'ye geri dönemez.
    - [Geçirilen VM'ler.](migrate-overview.md#what-do-we-mean-by-migration)
    - Başka bir kaynak grubuna taşınmış bir VM.
    - Silinmiş bir yineleme Azure VM.
    - Korunmayan bir yineleme Azure VM (şirket içi siteye çoğaltma).
10. Kullanabileceğiniz [geri dönüş türlerini gözden geçirin](concepts-types-of-failback.md) - özgün konum kurtarma ve alternatif konum kurtarma.


## <a name="enable-reprotection"></a>Yeniden koruma yı etkinleştirme

Çoğaltmayı etkinleştirin. Belirli VM'leri veya kurtarma planını yeniden koruyabilirsiniz:

- Bir kurtarma planını yeniden koruyorsanız, korunan her makineiçin değerleri sağlamanız gerekir.
- VM'ler çoklu VM tutarlılığı için bir çoğaltma grubuna aitse, yalnızca kurtarma planı kullanılarak yeniden korunabilirler. Çoğaltma grubundaki VM'ler aynı ana hedef sunucuyu kullanmalıdır

### <a name="before-you-start"></a>Başlamadan önce

- Başarısız olduktan sonra Azure'da bir VM önyüklemesi yaptıktan sonra aracının yapılandırma sunucusuna (15 dakikaya kadar) kaydolması biraz zaman alır. Bu süre zarfında yeniden koruyamazsınız ve bir hata iletisi aracının yüklü olmadığını gösterir. Bu durumda, birkaç dakika bekleyin ve sonra yeniden koruyun.
- Azure VM'yi mevcut bir şirket içi VM'ye geri vermek istemiyorsanız, ana hedef sunucunun ESXi ana bilgisayarında okuma/yazma erişimiyle şirket içi VM veri depolarını monte edin.
- Alternatif bir konuma geri dönmek istiyorsanız (örneğin şirket içi VM yoksa, ana hedef sunucu için yapılandırılan bekletme sürücüsünü ve veri deposunu seçin. Şirket içi siteye geri döndüğünüzde, failback koruma planındaki VMware sanal makineleri ana hedef sunucuyla aynı veri deposunu kullanır. VCenter'da yeni bir VM oluşturulur.

Yeniden korumayı aşağıdaki gibi etkinleştirin:

1. **Vault** > **Çoğaltılan öğeleri**seçin. Üzerinde başarısız olan sanal makineyi sağ tıklatın ve ardından **Yeniden Koruyun'u**seçin. Veya komut düğmelerinden makineyi seçin ve ardından **Yeniden Koruyun'u**seçin.
2. **Azure'dan Şirket Içi** koruma yönünün seçildiğini doğrulayın.
3. **Master Target Server** ve Process **Server'da**şirket içi ana hedef sunucusunu ve işlem sunucusunu seçin.  
4. **Datastore**için, diskleri şirket içinde kurtarmak istediğiniz veri mağazasını seçin. Bu seçenek, şirket içi sanal makine silindiğinde ve yeni diskler oluşturmanız gerektiğinde kullanılır. Diskler zaten varsa bu seçenek yoksayılır. Yine de bir değer belirtmeniz gerekir.
5. Bekletme sürücüsünü seçin.
6. Yeniden çalışma ilkesi otomatik olarak seçilir.
7. Yeniden korunmaya başlamak için **Tamam'ı** seçin.

    ![İletişim kutusunu yeniden koruma](./media/vmware-azure-reprotect/reprotectinputs.png)
    
8. Bir iş, Azure VM'yi şirket içi siteye çoğaltmaya başlar. **İşler** sekmesinde ilerleme durumunu izleyebilirsiniz.
    - Yeniden koruma başarılı olduğunda, VM korumalı bir duruma girer.
    - Şirket içi VM yeniden koruma sırasında kapatılır. Bu işlem, çoğaltma sırasında veri tutarlığını sağlar.
    - Yeniden koruma bittikten sonra şirket içi VM'yi açmayın.
   

## <a name="next-steps"></a>Sonraki adımlar

- Herhangi bir sorunla karşılaşırsanız, [sorun giderme makalesini](vmware-azure-troubleshoot-failback-reprotect.md)gözden geçirin.
- Azure VM'ler korunduktan sonra [bir geri ödeme çalıştırabilirsiniz.](vmware-azure-failback.md) Failback, Azure VM'yi kapatır ve şirket içi VM'yi önyüklemeye neden eder. Uygulama için biraz kapalı kalma süresi bekleyin ve buna göre bir geri dönüş süresi seçin.


