---
title: Azure Site Kurtarma hizmeti hakkında genel sorular
description: Bu makalede, Azure Site Kurtarma ile ilgili popüler genel sorular tartışılmaktadır.
ms.topic: conceptual
ms.date: 1/24/2020
ms.author: raynew
ms.openlocfilehash: a9d0ae4a6e60a72bbb1148aca1a75c44506b2e9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257686"
---
# <a name="general-questions-about-azure-site-recovery"></a>Azure Site Kurtarma ile ilgili genel sorular

Bu makalede, Azure Site Kurtarma hakkında sık sorulan sorular özetlenmiştir. Belirli senaryolar için bu makaleleri gözden geçirin

- [Azure'a Azure'da Olağanüstü Durum Kurtarma ile ilgili sorular](azure-to-azure-common-questions.md)
- [Azure'a VMware VM olağanüstü durum kurtarma ile ilgili sorular](vmware-azure-common-questions.md)
- [Azure'a Hyper-V VM olağanüstü durum kurtarma ile ilgili sorular](hyper-v-azure-common-questions.md)
 
## <a name="general"></a>Genel

### <a name="what-does-site-recovery-do"></a>Site Recovery ne işe yarar?
Site Kurtarma, bölgeler arasında Azure VM'lerinin çoğaltılmasını, şirket içi sanal makineleri ve fiziksel sunucuları Azure'a ve şirket içi makineleri Azure'a düzenleyerek ve otomatikleştirerek iş sürekliliği nize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. ikincil veri merkezi. [Daha fazla bilgi edinin](site-recovery-overview.md).

### <a name="can-i-protect-a-virtual-machine-that-has-a-docker-disk"></a>Docker diski olan sanal bir makineyi koruyabilir miyim?

Hayır, bu desteklenmeyen bir senaryo.

## <a name="service-providers"></a>Servis sağlayıcılar

### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Ben bir servis sağlayıcıyım. Site Kurtarma özel ve paylaşılan altyapı modelleri için çalışıyor mu?
Evet, Site Recovery hem adanmış hem de paylaşılan altyapı modellerini destekler.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Bir hizmet sağlayıcısı için kiracımın kimliği Site Kurtarma hizmetiyle paylaşılır mı?
Hayır. Kiracı kimliği anonim kalır. Kiracılarınızın Site Recovery portalına erişmesi gerekmez. Yalnızca hizmet sağlayıcı yöneticisi portal ile etkileşim sağlar.

### <a name="will-tenant-application-data-ever-go-to-azure"></a>Kiracı uygulama verileri Azure'a gidecek mi?
Hizmet sağlayıcının sahip olduğu siteler arasında çoğaltma yapılırken uygulama verileri asla Azure'a gitmez. Veriler aktarım sırasında şifrelenir ve doğrudan servis sağlayıcı siteleri arasında çoğaltılır.

Azure'da çoğaltma yaparken, uygulama verileri Azure depolama alanına gönderilir ancak Site Recovery hizmetine gönderilmez. Veriler akiş içinde şifrelenir ve Azure'da şifrelenir.

### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Kiracılarıma herhangi bir Azure hizmeti için fatura gönderilir mi?
Hayır. Azure'ın faturalama bağlantısı doğrudan hizmet sağlayıcı ile kurulur. Hizmet sağlayıcılar, kiracıları için belirli faturaları oluşturmaktan sorumludur.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Azure'a çoğaltma işlemi gerçekleştirirken sanal makinelerin Azure'da sürekli olarak çalışması gerekir mi?
Hayır, Veriler aboneliğinizde Azure depolama alanına çoğaltılır. Yük devretme testi (DR detayı) veya gerçek bir yük devretme gerçekleştirdiğinizde, Site Recovery otomatik olarak aboneliğinizde sanal makineler oluşturur.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Azure'a çoğaltma yaptığımda kiracı düzeyinde yalıtım sağlanır mı?
Evet.

### <a name="what-platforms-do-you-currently-support"></a>Şu anda hangi platformları destekliyorsanız?
Azure Paketi, Bulut Platformu Sistemi ve System Center tabanlı (2012 ve üzeri) dağıtımları destekliyoruz. Azure Paketi ve Site Kurtarma tümleştirmesi hakkında [daha fazla bilgi edinin.](https://technet.microsoft.com/library/dn850370.aspx)

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Tek bir Azure Paketi'ni ve tek bir VMM sunucusu dağıtımını destekliyor musunuz?
Evet, Hyper-V sanal makinelerini Azure'da veya hizmet sağlayıcı siteler arasında çoğaltabilirsiniz.  Hizmet sağlayıcı siteleri arasında çoğaltma yaptığınızda, Azure runbook tümleştirmesi kullanılamıyor.

## <a name="pricing"></a>Fiyatlandırma

### <a name="where-can-i-find-pricing-information"></a>Fiyatlandırma bilgilerini nerede bulabilirim?
[Site Kurtarma fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/) ayrıntılarını inceleyin.


### <a name="how-can-i-calculate-approximate-charges-during-the-use-of-site-recovery"></a>Site Kurtarma'nın kullanımı sırasında yaklaşık ücretleri nasıl hesaplayabilirim?

Site Kurtarma'yı kullanırken maliyetleri tahmin etmek için [fiyatlandırma hesaplayıcısını](https://aka.ms/asr_pricing_calculator) kullanabilirsiniz.

Maliyetler hakkında ayrıntılı tahmin [için, VMware](https://aka.ms/siterecovery_deployment_planner) veya [Hyper-V](https://aka.ms/asr-deployment-planner)için dağıtım planlayıcısı aracını çalıştırın ve [maliyet tahmin raporunu](https://aka.ms/asr_DP_costreport)kullanın.


### <a name="managed-disks-are-now-used-to-replicate-vmware-vms-and-physical-servers-do-i-incur-additional-charges-for-the-cache-storage-account-with-managed-disks"></a>Yönetilen diskler artık VMware VM'leri ve fiziksel sunucuları çoğaltmak için kullanılır. Yönetilen disklerle önbellek depolama hesabı için ek ücrete tabi miyim?

Hayır, önbellek için ek ücret yoktur. Standart depolama hesabına çoğaltmayaptığınızda, bu önbellek depolama alanı aynı hedef depolama hesabının bir parçasıdır.

### <a name="i-have-been-an-azure-site-recovery-user-for-over-a-month-do-i-still-get-the-first-31-days-free-for-every-protected-instance"></a>Bir aydan uzun süredir Azure Site Kurtarma kullanıcısıyım. Hala her korunan örnek için ilk 31 gün ücretsiz olsun mı?

Evet. Her korumalı örnek, ilk 31 gün boyunca Azure Site Kurtarma ücreti ödemez. Örneğin, son 6 ay içinde 10 örneği koruyorsanız ve 11. İlk 10 örnek, 31 günden uzun süredir korundukları için Azure Site Kurtarma ücretlerine maruz kalır.

### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>İlk 31 gün boyunca başka azure ücretlerine maruz kalacak mıyım?

Evet, Site Kurtarma korumalı bir örneğin ilk 31 günü boyunca ücretsiz olsa da, Azure Depolama, depolama işlemleri ve veri aktarımı için ücrete tabi olabilirsiniz. Kurtarılan bir sanal makine de Azure bilgi işlem ücretlerine neden olabilir.


### <a name="is-there-a-cost-associated-to-perform-disaster-recovery-drillstest-failover"></a>Olağanüstü durum kurtarma matkapları/test başarısızları gerçekleştirmek için ilişkili bir maliyet var mı?

DR matkap için ayrı bir maliyet yoktur. Test başarısız olduktan sonra VM oluşturulduktan sonra bilgi işlem ücretleri olacaktır.



## <a name="security"></a>Güvenlik

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Çoğaltılan veriler Site Recovery hizmetine gönderilir mi?
Hayır, Site Kurtarma çoğaltılan verileri engellemez ve sanal makinelerinizde veya fiziksel sunucularınızda ne yle ilgili herhangi bir bilgiye sahip değildir.
Çoğaltma verileri şirket içi Hyper-V ana bilgisayarları, VMware hiper yöneticileri veya fiziksel sunucular ile Azure depolama alanı ya da ikincil siteniz arasında değiştirilir. Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  

Site Kurtarma ISO 27001:2013, 27018, HIPAA, DPA sertifikalı ve SOC2 ve FedRAMP JAB değerlendirmeleri sürecindedir.

### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Uyumluluk nedenleriyle, şirket içi meta verilerimiz bile aynı coğrafi bölgede kalmalıdır. Site Kurtarma bize yardımcı olabilir mi?
Evet. Bir bölgede bir Site Kurtarma kasası oluşturduğunuzda, çoğaltma ve başarısızolmasını etkinleştirmek ve düzenlemek için ihtiyacımız olan tüm meta verilerin o bölgenin coğrafi sınırları içinde kalmasını sağlarız.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?
Sanal makineler ve fiziksel sunucular için, şirket içi siteler arasında şifreleme-in-transit desteklenir. Azure'a çoğalan sanal makineler ve fiziksel sunucular için hem geçici şifreleme hem de [şifreleme (Azure'da)](https://docs.microsoft.com/azure/storage/storage-service-encryption) desteklenir.

### <a name="how-can-i-enforce-tls-12-on-all-on-premises-azure-site-recovery-components"></a>TLS 1.2'yi şirket içi tüm Azure Site Kurtarma bileşenlerinde nasıl uygulayabilirim?
Çoğaltılan öğelere yüklenen mobilite aracıları Process Server'a yalnızca TLS 1.2 ile iletişim kurar. Ancak Configuration Server'dan Azure'a ve Process Server'dan Azure'a iletişim TLS 1.1 veya 1.0'da olabilir. Sizin kurduğunuz tüm Yapılandırma Sunucularında ve İşlem Sunucularında TLS 1.2'yi uygulamak için lütfen [kılavuzu](https://support.microsoft.com/en-us/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi) izleyin.


## <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

### <a name="what-can-site-recovery-protect"></a>Site Kurtarma neleri koruyabilir?
* **Azure VM'leri**: Site Kurtarma, desteklenen bir Azure VM'de çalışan tüm iş yükünü çoğaltabilir
* **Hyper-V sanal makineler**: Site Kurtarma, Hyper-V VM üzerinde çalışan tüm iş yükünü koruyabilir.
* **Fiziksel sunucular**: Site Kurtarma, Windows veya Linux çalıştıran fiziksel sunucuları koruyabilir.
* **VMware sanal makineleri**: Site Kurtarma, VMware VM'de çalışan tüm iş yükünü koruyabilir.

### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Site Recovery ile hangi iş yüklerini koruyabilirim?
Desteklenen bir VM veya fiziksel sunucuda çalışan iş yüklerinin çoğunu korumak için Site Kurtarma'yı kullanabilirsiniz. Site Kurtarma, uygulamaların akıllı bir duruma kurtarılabilmeleri için uygulamaya duyarlı çoğaltma için destek sağlar. SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir ve Oracle, SAP, IBM ve Red Hat gibi önde gelen satıcılarla yakın çalışır. İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Site Recovery ile şubelerim için olağanüstü durum kurtarma işlemini yönetebilir miyim?
Evet. Office ofislerinizde çoğaltma ve başarısızlığı düzenlemek için Site Kurtarma'yı kullandığınızda, merkezi bir konumda tüm şube iş yüklerinizi birleşik bir şekilde düzenleme ve görüntüleme elde elabilirsiniz. Şubelerinizi ziyaret etmenize gerek kalmadan kolaylıkla tüm şubelerinizin olağanüstü durum kurtarma işlemlerini yönetebilir ve yük devretmeler çalıştırabilirsiniz.


### <a name="is-disaster-recovery-supported-for-azure-vms"></a>Azure VM'ler için olağanüstü durum kurtarma desteklenir mi?

Evet, Site Kurtarma, Azure bölgeleri arasındaki Azure VM'leri için olağanüstü durum sağlar. Azure VM olağanüstü durum kurtarma yla ilgili [sık sorulan soruları gözden geçirin.](azure-to-azure-common-questions.md)

### <a name="is-disaster-recovery-supported-for-vmware-vms"></a>VMware VM'ler için olağanüstü durum kurtarma desteklenir mi?

Evet, Site Kurtarma şirket içi VMware VM'lerin olağanüstü kurtarma yı destekler. VMware VM'lerin olağanüstü durum kurtarma yla ilgili [sık sorulan soruları gözden geçirin.](vmware-azure-common-questions.md)

### <a name="is-disaster-recovery-supported-for-hyper-v-vms"></a>Hyper-V VM'ler için olağanüstü durum kurtarma desteklenir mi?
Evet, Site Kurtarma, şirket içi Hyper-V V M'lerin olağanüstü kurtarma yı destekler. Hyper-V VM'lerin olağanüstü durum kurtarması için [sık sorulan soruları gözden geçirin.](hyper-v-azure-common-questions.md)

## <a name="is-disaster-recovery-supported-for-physical-servers"></a>Olağanüstü durum kurtarma fiziksel sunucular için desteklenir mi?
Evet, Site Kurtarma, Windows ve Linux'u Azure'a veya ikincil bir siteye çalıştıran şirket içi fiziksel sunucuların olağanüstü kurtarmayı destekler. [Azure'a](vmware-physical-azure-support-matrix.md#replicated-machines)ve[ikincil bir siteye](vmware-physical-secondary-support-matrix.md#replicated-vm-support)olağanüstü durum kurtarma gereksinimleri hakkında bilgi edinin.
Fiziksel sunucuların başarısız olduktan sonra Azure'da VM olarak çalışacağına dikkat edin. Azure'dan şirket içi fiziksel sunucuya geri dönüş şu anda desteklenmemektedir. Yalnızca vmware sanal makineye geri dönebilirsiniz.





## <a name="replication"></a>Çoğaltma

### <a name="can-i-replicate-over-a-site-to-site-vpn-to-azure"></a>Siteden siteye VPN üzerinden Azure'a çoğaltabilir miyim?
Azure Site Kurtarma, verileri genel bir bitiş noktası üzerinden bir Azure depolama hesabına veya yönetilen disklerde çoğaltır. Çoğaltma siteden siteye VPN üzerinden değil. 

### <a name="why-cant-i-replicate-over-vpn"></a>Vpn üzerinden neden kopyalamıyorum?

Azure'da çoğaltma yaptığınızda, çoğaltma trafiği bir Azure Depolama'nın ortak uç noktalarına ulaşır. Böylece yalnızca genel internet üzerinden veya ExpressRoute (Microsoft'un bakışveya varolan bir genel bakış) üzerinden çoğaltabilirsiniz.

### <a name="can-i-use-riverbed-steelheads-for-replication"></a>Çoğaltma için Riverbed SteelHeads kullanabilir miyim?

Ortağımız Riverbed, Azure Site Kurtarma ile çalışma konusunda ayrıntılı rehberlik sağlar. Çözüm [kılavuzunu gözden geçirin.](https://community.riverbed.com/s/article/DOC-4627)

### <a name="can-i-use-expressroute-to-replicate-virtual-machines-to-azure"></a>Sanal makineleri Azure'a çoğaltmak için ExpressRoute'u kullanabilir miyim?
Evet, ExpressRoute şirket içi sanal makineleri Azure'a çoğaltmak için [kullanılabilir.](concepts-expressroute-with-site-recovery.md)

- Azure Site Kurtarma, verileri genel bir bitiş noktası üzerinden Azure Depolama'ya çoğaltır. Site Kurtarma çoğaltma için ExpressRoute'u kullanmak için [Microsoft'a bakan](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) bir bakış ayarlamanız veya varolan bir [genel bakış](../expressroute/about-public-peering.md) (yeni devreler için amortismana uygun) kullanmanız gerekir.
- Microsoft'un bakışları çoğaltma için önerilen yönlendirme etki alanıdır.
- Çoğaltma özel bakış üzerinde desteklenmez.
- VMware makinelerini veya fiziksel makineleri koruyorsanız, Configuration Server [için Ağ Gereksinimleri'nin](vmware-azure-configuration-server-requirements.md#network-requirements) de karşılandığından emin olun. Site Kurtarma çoğaltmasının düzenlenmesi için Configuration Server tarafından belirli URL'lere bağlantı gereklidir. ExpressRoute bu bağlantı için kullanılamaz.
- Sanal makineler bir Azure sanal ağı üzerinden başarısız olduktan sonra, Azure sanal ağıyla özel [eşleme](../expressroute/expressroute-circuit-peerings.md#privatepeering) kurulumlarını kullanarak bu makinelere erişebilirsiniz.


### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-or-managed-disk-do-i-need"></a>Azure'da çoğaltırsam, ne tür bir depolama hesabına veya yönetilen diske ihtiyacım olur?

Bir LRS veya GRS depolama gerekir. Bölgesel bir kesintinin meydana gelmesi veya birincil bölgenin kurtarılamaması durumunda verilerin korunması için GRS'yi tavsiye ederiz. Hesabın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir. Azure portalında Site Kurtarma dağıtTığınızda Premium depolama alanı VMware VM, Hyper-V VM ve fiziksel sunucu çoğaltma için desteklenir. Yönetilen diskler yalnızca LRS'yi destekler.

### <a name="how-often-can-i-replicate-data"></a>Verileri ne sıklıkta çoğaltabilirim?
* **Hiper-V:** Hyper-V VM'ler her 30 saniyede bir çoğaltılabilir (premium depolama hariç), beş dakika veya 15 dakika.
* **Azure VM'ler, VMware VM'ler, fiziksel sunucular:** Çoğaltma sıklığı burada önemli değil. Çoğaltma süreklidir.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Çoğaltmayı varolan kurtarma sitesinden başka bir üçüncül siteye genişletebilir miyim?
Genişletilmiş veya zincir çoğaltma desteklenmez. Geri bildirim [forumunda](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959)bu özelliği isteyin.

### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Azure'a ilk kez çoğaltma yaparken çevrimdışı bir çoğaltma gerçekleştirebilir miyim?
Bu özellik desteklenmez. [Geri bildirim forumunda](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from)bu özelliği isteyin.

### <a name="can-i-exclude-specific-disks-from-replication"></a>Belirli diskleri çoğaltmanın dışında tutabilir miyim?
Bu, Azure portalını kullanarak VMware VM'leri ve Hyper-V VM'leri Azure'a kopyalarken desteklenir.

### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Sanal makineleri dinamik disklerle çoğaltabilir miyim?
Hyper-V sanal makinelerinçoğaltılmasında ve VMware VM'leri ve fiziksel makineleri Azure'a kopyalarken dinamik diskler desteklenir. İşletim sistemi diski temel bir disk olmalıdır.


### <a name="can-i-throttle-bandwidth-allotted-for-replication-traffic"></a>Çoğaltma trafiği için ayrılan bant genişliğini daraltabilir miyim?
Evet. Bu makalelerde bant genişliğini azaltma hakkında daha fazla bilgi edinebilirsiniz:

* [VMware VM'leri ve fiziksel sunucuları çoğaltmak için kapasite planlaması](site-recovery-plan-capacity-vmware.md)
* [Hyper-V V MM'leri Azure'a çoğaltmak için kapasite planlaması](site-recovery-capacity-planning-for-hyper-v-replication.md)



## <a name="failover"></a>Yük devretme
### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-vms-after-failover"></a>Azure'da başarısız oluyorsam, başarısız olduktan sonra Azure VM'lerine nasıl erişebilirim?

Azure VM'lerine güvenli bir İnternet bağlantısı, siteden siteye VPN veya Azure ExpressRoute üzerinden erişebilirsiniz. Bağlanmak için bir dizi şey hazırlamanız gerekir. [Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Azure'da başarısız olursam, Azure verilerimin esnek olduğundan nasıl emin olur?
Azure esneklik için tasarlanmıştır. Site Kurtarma, Azure SLA'ya uygun olarak ikincil bir Azure veri merkezine geçememesi için zaten tasarlandı. Bu durumda, meta verilerinizin ve kasalarınızın kasanız için seçtiğiniz coğrafi bölgede kalmasını sağlıyoruz.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>İki veri merkezi arasında çoğalıyorsam, birincil veri merkezim de beklenmeyen bir kesinti yle karşılanırsa ne olur?
İkincil siteden planlanmamış bir yük devretme tetikleyebilirsiniz. Bu yük devretme işlemini gerçekleştirmek için Site Recovery'nin birincil siteye bağlanması gerekmez.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?
Yük devretme işlemi otomatik değildir. Portalda tek bir tıklamayla başarısızlıklar başlatın veya bir başarısızı tetiklemek için [Site Kurtarma PowerShell'i](/powershell/module/az.recoveryservices) kullanabilirsiniz. Geri başarısız Site Kurtarma portalında basit bir eylemdir.

Otomatikleştirmek için, sanal bir makine arızası algılamak ve ardından SDK'yı kullanarak başarısızlığı tetiklemek için şirket içi Orchestrator veya Operations Manager'ı kullanabilirsiniz.

* Kurtarma planları hakkında [daha fazla bilgi edinin.](site-recovery-create-recovery-plans.md)
* Failover hakkında [daha fazla bilgi edinin.](site-recovery-failover.md)
* VMware VM'leri ve fiziksel sunucuları geri başarısız hakkında [daha fazla bilgi edinin](site-recovery-failback-azure-to-vmware.md)

### <a name="if-my-on-premises-host-is-not-responding-or-crashed-can-i-fail-back-to-a-different-host"></a>Şirket içi ana bilgisayarım yanıt vermiyorsa veya çöküyorsa, başka bir ana bilgisayara geri dönebilir miyim?
Evet, Azure'dan farklı bir ana bilgisayara geri dönmek için alternatif konum kurtarmayı kullanabilirsiniz.

* [VMware sanal makineleri için](concepts-types-of-failback.md#alternate-location-recovery-alr)
* [Hyper-V sanal makineler için](hyper-v-azure-failback.md#fail-back-to-an-alternate-location)

## <a name="automation"></a>Automation

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Site Kurtarma senaryolarını Bir SDK ile otomatikleştirebilir miyim?
Evet. Rest API'si, PowerShell veya Azure SDK kullanarak Site Recovery iş akışlarını otomatikleştirebilirsiniz. PowerShell kullanarak Site Kurtarma dağıtma için şu anda desteklenen senaryolar:

* [VMM bulutlarında Hyper-V VM'leri Azure PowerShell Kaynak Yöneticisi'ne çoğaltma](hyper-v-vmm-powershell-resource-manager.md)
* [VMM olmadan Hyper-V VM'leri Azure PowerShell Kaynak Yöneticisi'ne çoğaltma](hyper-v-azure-powershell-resource-manager.md)
* [PowerShell Kaynak Yöneticisi ile VMware'i Azure'a çoğaltma](vmware-azure-disaster-recovery-powershell.md)

## <a name="componentprovider-upgrade"></a>Bileşen/sağlayıcı yükseltmesi

### <a name="where-can-i-find-the-release-notesupdate-rollups-of-site-recovery-upgrades"></a>Site Kurtarma yükseltmelerinin sürüm notlarını/güncelleme toplamalarını nerede bulabilirim?

Yeni güncelleştirmeler hakkında [bilgi edinin](site-recovery-whats-new.md) ve toplama [bilgileri alın.](service-updates-how-to.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Site Recovery'ye genel bakış](site-recovery-overview.md) başlığını okuyun.

