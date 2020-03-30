---
title: Azure Site Kurtarma ile VMware VM olağanüstü durum kurtarma da sorun giderme
description: Bu makalede, Azure Site Kurtarma ile Azure'a VMware VM olağanüstü durum kurtarma sırasında hata geri ödeme ve yeniden koruma sorunları gidermek için yollar açıklanmaktadır.
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: b577b82585ffad0547818b4f19554a2f39cb830c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75498093"
---
# <a name="troubleshoot-failback-to-on-premises-from-azure"></a>Azure’dan şirket içine yapılan yeniden çalışma işleminde sorun giderme

Bu makalede, [Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak Azure'a başarısız olduktan sonra Azure VMware altyapınıza Azure VMware altyapınıza geri döndüğünüzde karşılaşabileceğiniz sorunları nasıl giderilebilirsiniz.

Failback aslında iki ana adım içerir. İlk adımda, başarısız olduktan sonra Azure VM'lerini şirket içinde yeniden korumanız gerekir, böylece çoğaltmaya başlarlar. İkinci adım, şirket içi sitenize geri dönmek için Azure'dan bir başarısızlık çalıştırmaktır.

## <a name="common-issues"></a>Genel sorunlar

- Salt okunur kullanıcı vCenter bulma gerçekleştirin ve sanal makineleri korumak, koruma başarılı olur ve başarısız çalışır. Yeniden koruma sırasında, veri depoları keşfedilemediği için başarısız olur. Bir belirti, veri depoları yeniden koruma sırasında listelenmemiş olmasıdır. Bu sorunu gidermek için, vCenter kimlik bilgilerini izinleri olan uygun bir hesapla güncelleştirebilir ve ardından işi yeniden deneyebilirsiniz.
- Bir Linux sanal makinesini geri aldığınızda ve şirket içinde çalıştırdığınızda, Network Manager paketinin makineden kaldırıldığını görebilirsiniz. Bu uninstallation, sanal makine Azure'da kurtarıldığında Ağ Yöneticisi paketi kaldırıldığından oluşur.
- Bir Linux sanal makinesi statik bir IP adresiyle yapılandırıldığında ve Azure'da başarısız olduğunda, IP adresi DHCP'den elde edilir. Şirket içinde başarısız olduğunuzda, sanal makine IP adresini elde etmek için DHCP kullanmaya devam eder. Makinede el ile oturum açın ve gerekirse IP adresini statik bir adrese geri ayarlayın. Bir Windows sanal makine statik IP adresini yeniden edinebilir.
- ESXi 5.5 ücretsiz sürümü veya vSphere 6 Hypervisor ücretsiz sürümünü kullanırsanız, failover başarılı olur, ancak failback başarılı olmaz. Başarısız olmayı etkinleştirmek için her iki programın değerlendirme lisansına yükseltin.
- İşlem sunucusundan yapılandırma sunucusuna erişemezseniz, bağlantı noktası 443'teki yapılandırma sunucusuna bağlantıyı denetlemek için Telnet'i kullanın. İşlem sunucusundan yapılandırma sunucusuna ping yapmayı da deneyebilirsiniz. Bir işlem sunucusu, yapılandırma sunucusuna bağlandığında da sinyal vermelidir.
- Fiziksel bir şirket içi sunucu olarak korunan Bir Windows Server 2008 R2 SP1 sunucusu Azure'dan şirket içi bir siteye geri döndürülemez.
- Aşağıdaki durumlarda başarısız olamazsınız:
    - Makineleri Azure'a geçtiniz. [Daha fazla bilgi edinin](migrate-overview.md#what-do-we-mean-by-migration).
    - Bir VM'yi başka bir kaynak grubuna taşıdınız.
    - Azure VM'yi sildiniz.
    - VM'nin korumasını devre dışı bıraktın.
    - VM'yi Azure'da el ile oluşturdunuz. Makinenin başlangıçta şirket içinde korunması ve yeniden korumadan önce Azure'da başarısız olması gerekir.
    - Yalnızca bir ESXi ana bilgisayar için başarısız olabilirsiniz. VMware VM'leri veya fiziksel sunucuları Hyper-V ana bilgisayarlarına, fiziksel makinelere veya VMware iş istasyonlarına geri veremezsin.


## <a name="troubleshoot-reprotection-errors"></a>Yeniden koruma hatalarını giderme

Bu bölümde, sık karşılaşılan yeniden koruma hataları ve bunların nasıl düzeltilen ayrıntıları.

### <a name="error-code-95226"></a>Hata kodu 95226

**Azure sanal makinesi şirket içi yapılandırma sunucusuna erişemediği için Yeniden Koruma başarısız oldu.**

Bu hata şu anda oluşur:

* Azure VM şirket içi yapılandırma sunucusuna erişemez. VM keşfedilemez ve yapılandırma sunucusuna kaydedilemez.
* InMage Scout uygulama hizmeti, başarısız olduktan sonra Azure VM'de çalışmıyor. Hizmet, şirket içi yapılandırma sunucusuyla iletişim için gereklidir.

Bu sorunu çözmek için:

* Azure VM ağının Azure VM'nin şirket içi yapılandırma sunucusuyla iletişim kurmasına izin verilebiyi kontrol edin. Şirket içi veri merkezinize siteden siteye VPN ayarlayabilir veya Azure VM'nin sanal ağında özel bakışla azure ExpressRoute bağlantısını yapılandırabilirsiniz.
* VM şirket içi yapılandırma sunucusuyla iletişim kurabiliyorsa, VM'de oturum açın. Daha sonra InMage Scout uygulama hizmetini kontrol edin. Çalışmadığını görürseniz, hizmeti el ile başlatın. Hizmet başlangıç türünün **Otomatik**olarak ayarlı olduğundan denetleyin.

### <a name="error-code-78052"></a>Hata kodu 78052

**Sanal makine için koruma tamamlanamadı.**

Bu sorun, başarısız olduğunuz ana hedef sunucuda zaten aynı ada sahip bir VM varsa olabilir.

Bu sorunu çözmek için:

* Farklı bir ana bilgisayarda farklı bir ana hedef sunucu seçin, böylece yeniden koruma makineyi farklı bir ana bilgisayarda oluşturur ve adlar çarpışmıyor.
* Ana hedefi, ad çakışması gerçekleşmeyecek farklı bir ana bilgisayara taşımak için vMotion'ı da kullanabilirsiniz. Varolan VM bir sokak makinesiyse, aynı ESXi ana bilgisayarda yeni VM'nin oluşturulabilmesi için yeniden adlandırın.


### <a name="error-code-78093"></a>Hata kodu 78093

**VM asılı bir durumda, çalışmıyor veya erişilebilir değil.**

Bu sorunu çözmek için:

Azure VM'nin başarısız olmuş bir VM'yi yeniden korumak için çalışması gerekir, böylece Mobility Service yapılandırma sunucusuna şirket içinde kaydolur ve işlem sunucusuyla iletişim kurarak çoğaltmaişlemine başlayabilir. Makine yanlış bir ağdaysa veya çalışmıyorsa (yanıt vermiyor veya kapanmıyorsa), yapılandırma sunucusu yeniden koruma yı başlatmak için VM'deki Mobilite Hizmetine erişemez.

* VM'yi yeniden başlatın, böylece şirket içinde iletişim kurmaya başlayabilir.
* Azure sanal makinesini başlattıktan sonra yeniden koruma işini yeniden başlatın.

### <a name="error-code-8061"></a>Hata kodu 8061

**Veri deposuna ESXi ana bilgisayardan erişilemez.**

Başarısız olmak için [ana hedef ön koşulları ve desteklenen veri depolarını](vmware-azure-prepare-failback.md#deploy-a-separate-master-target-server) denetleyin.


## <a name="troubleshoot-failback-errors"></a>Sorun giderme hataları

Bu bölümde, geri dönüş sırasında karşılaşabileceğiniz sık karşılaşılan hatalar açıklanmaktadır.

### <a name="error-code-8038"></a>Hata kodu 8038

**Hata nedeniyle şirket içi sanal makinegetirmek için başarısız oldu.**

Bu sorun, şirket içi VM yeterli bellek sağlanan olmayan bir ana bilgisayarda getirildiğinde olur. 

Bu sorunu çözmek için:

* ESXi ana bilgisayarüzerinde daha fazla bellek sağlama.
* Buna ek olarak, VM'yi VM'yi önyüklemeye yetecek belleğe sahip başka bir ESXi ana bilgisayara taşımak için vMotion'ı kullanabilirsiniz.
