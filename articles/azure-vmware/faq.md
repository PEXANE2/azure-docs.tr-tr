---
title: Sık sorulan sorular
description: Azure VMware çözümüyle ilgili bazı yaygın soruların yanıtlarını sağlar.
ms.topic: conceptual
ms.date: 1/27/2021
ms.openlocfilehash: 3ef3d1b9e6fc76b5f09acf8c300dbea901b4aef2
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99072277"
---
# <a name="frequently-asked-questions-about-azure-vmware-solution"></a>Azure VMware çözümü hakkında sık sorulan sorular

Bu makalede, Azure VMware çözümü hakkında sık sorulan soruları yanıtlarız.

## <a name="general"></a>Genel

### <a name="what-is-azure-vmware-solution"></a>Azure VMware Çözümü nedir?

Kuruluşlar, iş çevikliği geliştirmek, maliyetleri azaltmak ve yeniliklerini hızlandırmak için strateji yaparken, hibrit bulut platformları, müşterilerin dijital dönüşümünde önemli etkinleştiriciler olarak ortaya çıktı. Azure VMware çözümü, VMware 'nin Software-Defined veri merkezi (SDDC) yazılımını Microsoft 'un Azure küresel bulut hizmeti ekosistemi ile birleştirir. Azure VMware çözümü, performans, kullanılabilirlik, güvenlik ve uyumluluk gereksinimlerini karşılayacak şekilde yönetilir.

## <a name="azure-vmware-solution-service"></a>Azure VMware çözüm hizmeti

### <a name="where-is-azure-vmware-solution-available-today"></a>Azure VMware çözümü bugün kullanılabilir mi?

Hizmet yeni bölgelere sürekli ekleniyor, bu nedenle daha fazla ayrıntı için [en son hizmet kullanılabilirliği bilgilerini](https://azure.microsoft.com/global-infrastructure/services/?products=azure-vmware) görüntüleyin. 

### <a name="can-workloads-running-in-an-azure-vmware-solution-instance-consume-or-integrate-with-azure-services"></a>Azure VMware Çözüm örneğinde çalışan iş yükleri Azure hizmetleriyle tüketiliyor veya tümleştirilmesine neden olabilir?

Tüm Azure hizmetleri Azure VMware Çözüm müşterileri tarafından kullanılabilir. Belirli hizmetler için performans ve kullanılabilirlik sınırlamaları, büyük/küçük harfe göre değinilmesi gerekecektir.

### <a name="what-guest-operating-systems-are-compatible-with-azure-vmware-solution"></a>Azure VMware çözümüyle hangi Konuk işletim sistemleri uyumlu?

[VMware Uyumluluk Kılavuzu ' nu](https://www.vmware.com/resources/compatibility/search.php?deviceCategory=software&details=1&releases=485&page=1&display_interval=10&sortColumn=Partner&sortOrder=Asc&testConfig=16)kullanarak, vSphere ile Konuk işletim sistemi uyumluluğu hakkında bilgi bulabilirsiniz.  Azure VMware çözümünde çalışan vSphere sürümünü belirlemek için bkz. [VMware yazılım sürümleri](concepts-private-clouds-clusters.md#vmware-software-versions).

### <a name="do-i-use-the-same-tools-that-i-use-now-to-manage-private-cloud-resources"></a>Özel bulut kaynaklarını yönetmek için şu anda kullandığım araçları kullanıyorum?

Evet. Azure portal dağıtım ve çeşitli yönetim işlemleri için kullanılır. vCenter ve NSX Yöneticisi, vSphere ve NSX-T kaynaklarını yönetmek için kullanılır.

### <a name="can-i-manage-a-private-cloud-with-my-on-premises-vcenter"></a>Şirket içi vCenter 'um ile özel bir bulutu yönetebilir miyim?

Başlatma sırasında Azure VMware çözümü, şirket içi ve özel bulut ortamlarında tek bir yönetim deneyimini desteklemez. Özel bulut kümeleri, özel bir bulutta vCenter ve NSX Yöneticisi yerel ile yönetilir.

### <a name="can-i-use-vrealize-suite-running-on-premises"></a>Şirket içinde çalışan VNET paketini kullanabilir miyim? 

Belirli tümleştirmeler ve kullanım durumları, büyük/küçük harf esasına göre değerlendirilebilir.

### <a name="can-i-migrate-vsphere-vms-from-on-premises-environments-to-azure-vmware-solution-private-clouds"></a>Şirket içi ortamlardan vSphere VM 'lerini Azure VMware Çözüm özel bulutlarına geçirebilir miyim?

Evet. Standart çapraz vCenter [vMotion gereksinimleri](https://kb.vmware.com/s/article/2106952?lang=en_US&queryTerm=2106952) karşılanıyorsa VM 'leri özel bir buluta TAŞıMAK için VM geçişi ve vMotion kullanılabilir.

### <a name="is-a-specific-version-of-vsphere-required-in-on-premises-environments"></a>Şirket içi ortamlarda gerekli olan belirli bir vSphere sürümü mi?

Tüm bulut ortamları, vMotion için şirket içi ortamlarda VMware HCX, vSphere 5,5 veya sonraki bir sürümü ile gelir.

### <a name="what-does-the-change-control-process-look-like"></a>Değişiklik denetimi işlemi nasıl görünür?

Hizmette yapılan güncelleştirmelerin kendisi Microsoft Azure Standart değişiklik yönetimi sürecidir. Müşteriler tüm iş yükü yönetim görevlerinden ve ilişkili değişiklik yönetimi işlemlerinden sorumludur.

### <a name="how-is-this-different-from-azure-vmware-solution-by-cloudsimple"></a>Bu, CloudSimple tarafından Azure VMware çözümünden farklı midir?

Yeni Azure VMware çözümüyle, Microsoft ve VMware 'nin doğrudan bir bulut sağlayıcısı ortaklığı vardır. Yeni çözüm, Microsoft tarafından tamamen tasarlanır, oluşturulmuştur ve desteklenir, VMware tarafından onaylanmıştır. Mimari türsel olarak, çözümler, VMware teknoloji yığınının adanmış bir Azure altyapısında çalışıyor olması bakımından tutarlıdır.

### <a name="can-azure-vmware-solution-vms-be-managed-by-vmrc"></a>Azure VMware Çözüm VM 'Leri VMRC tarafından yönetiliyor mu?
Evet. Üzerinde yüklü olan sistem, özel bulut vCenter 'a erişebilir ve ESXi ana bilgisayar adlarını çözümlemek için ortak DNS kullanıyor olabilir.

### <a name="are-there-special-instructions-for-installing-and-using-vmrc-with-azure-vmware-solution-vms"></a>Azure VMware Çözüm VM 'Leri ile VMRC 'yi yüklemek ve kullanmak için özel yönergeler var mı?
Hayır. VM önkoşullarını karşılamak için [VMware tarafından sunulan yönergeleri](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-89E7E8F0-DB2B-437F-8F70-BA34C505053F.html)izleyin. 

### <a name="is-vmware-hcx-supported-on-vpns"></a>Şu VPN 'lerde VMware HCX destekleniyor mu?
Bant genişliği ve gecikme süresi gereksinimleri nedeniyle Hayır.

### <a name="can-azure-bastion-be-used-for-connecting-to-azure-vmware-solution-vms"></a>Azure, Azure VMware Çözüm VM 'lerine bağlanmak için kullanılabilir mi?
Azure 'da, Azure VMware çözümünün Internet 'e sunulmasını engellemek için bağlantı kutusu 'na bağlanmanız önerilir. Azure IaaS nesneleri olmadığından Azure VMware Çözüm VM 'lerine bağlanmak için Azure savunma 'yi kullanamazsınız.

### <a name="can-azure-load-balancer-internal-be-used-for-azure-vmware-solution-vms"></a>Azure VMware Çözüm VM 'Leri için Azure Load Balancer dahili olarak kullanılabilir mi?
Hayır. Azure Load Balancer iç-yalnızca Azure IaaS VM 'lerini destekler. Azure Load Balancer, IP tabanlı arka uç havuzlarını desteklemez; Azure VMware Çözüm VM 'lerinin Azure nesneleri olmayan yalnızca Azure VM 'Leri veya sanal makine ölçek kümesi nesneleri.

### <a name="can-an-existing-expressroute-gateway-be-used-to-connect-to-azure-vmware-solution"></a>Azure VMware çözümüne bağlanmak için mevcut bir ExpressRoute ağ geçidi kullanılabilir mi?
Evet. Sanal ağ başına dört ExpressRoute bağlantı hattı sınırını aşmadığı sürece Azure VMware çözümüne bağlanmak için mevcut bir ExpressRoute ağ geçidini kullanın. Azure VMware çözümüne ExpressRoute aracılığıyla Şirket içinden erişmek için ExpressRoute ağ geçidi, bağlı olan devreler arasında geçişli yönlendirme sağlamadığından ExpressRoute Global Reach sahip olmalısınız.

### <a name="why-does-azure-vmware-solution-use-a-public-4-byte-autonomous-system-number-asn"></a>Azure VMware çözümü neden ortak bir 4 baytlık otonom sistem numarası (ASN) kullanıyor?
Azure VMware çözümü, müşterinin Azure VMware çözümüne yönelik yönlendirme yolundaki özel ASNs 'lerin şirket içi kullanımıyla ilgili hiç bir çakışma olmadığından emin olmak için resmi olarak kaydedilen ortak 4 baytlık ASNs 'yi kullanır.
 
### <a name="how-can-i-use-expressroute-to-connect-to-azure-vmware-solution-if-the-on-premises-expressroute-carrier-partnersisps-dont-support-4-byte-asn"></a>Şirket içi ExpressRoute-taşıyıcı iş ortakları/ISS 'Ler 4 baytlık ASN 'yi desteklemiyorsa Azure VMware çözümüne bağlanmak için ExpressRoute 'u nasıl kullanabilirim?
Azure VMware çözümüne ExpressRoute üzerinden bağlanmak için tek yol, ortamınız için ve şirket içi ExpressRoute-taşıyıcı iş ortakları/ISS 'Ler 4 baytlık ASN 'yi destekler veya BGP ön eki ASN yol tanıtımında 4 bayttan 2 baytlık ASN 'ye geri uyumluluk sağlar.

## <a name="compute-network-storage-and-backup"></a>İşlem, ağ, depolama ve yedekleme

### <a name="is-there-more-than-one-type-of-host-available"></a>Birden fazla konak türü var mı?

Kullanılabilir tek bir konak türü vardır.

### <a name="what-are-the-cpu-specifications-in-each-type-of-host"></a>Her konak türünde CPU belirtimleri nelerdir?

Sunucularda iki adet 18 çekirdekli 2,3 GHz Intel CPU vardır.

### <a name="how-much-memory-is-in-each-host"></a>Her konakta ne kadar bellek var?

Sunucularda 576 GB RAM vardır.

### <a name="what-is-the-storage-capacity-of-each-host"></a>Her Konağın depolama kapasitesi nedir?

Her ESXi konağında, 15,2 TB 'lik kapasite katmanı ve 3,2-TB NVMe önbellek katmanı (her diskgroup 'ta 1,6 TB) içeren iki vSAN diskgroups vardır.

### <a name="how-much-network-bandwidth-is-available-in-each-esxi-host"></a>Her ESXi konağında kullanılabilir ağ bant genişliği miktarı nedir?

Azure VMware çözümünde her ESXi Konağı 4 25-Gbps NIC, ESXi sistem trafiği için sağlanan iki NIC ve iş yükü trafiği için sağlanan iki NIC ile yapılandırılır. 

### <a name="is-data-stored-on-the-vsan-datastores-encrypted-at-rest"></a>Veriler, bekleyen vSAN veri depoları üzerinde mi depolanıyor?

Evet, tüm vSAN verileri, Azure Key Vault depolanan anahtarlar kullanılarak varsayılan olarak şifrelenir.

###  <a name="what-independent-software-vendors-isvs-backup-solutions-work-with-azure-vmware-solution"></a>Hangi bağımsız yazılım satıcıları (ISV) yedekleme çözümleri Azure VMware çözümüyle birlikte çalışıyor?

Commkasası, VERITAS ve Veead, Azure VMware çözümü ile çalışmak için yedekleme çözümlerini genişletti.  Ancak, HotAdd taşıma moduyla VMware VADP kullanan herhangi bir yedekleme çözümü, Azure VMware çözümünde bu kutudan hemen çalışır.

### <a name="what-about-support-for-isv-backup-solutions"></a>ISV yedekleme çözümleri için destek nedir?

Bu yedekleme çözümleri müşteriler tarafından yüklenip yönetildiğinden, destek için ilgili ISV 'ye ulaşabilirler. 

### <a name="what-is-the-correct-storage-policy-for-the-dedupe-setup"></a>Yinelenenleri kaldırma kurulumu için doğru depolama ilkesi nedir?

VM şablonunuz için *thin_provision* depolama ilkesini kullanın.  Varsayılan değer *thick_provision*.

### <a name="are-the-snmp-infrastructure-logs-shared"></a>SNMP altyapı günlükleri paylaşılıyor mu?

Hayır.

## <a name="hosts-clusters-and-private-clouds"></a>Konaklar, kümeler ve özel bulutlar

### <a name="is-the-underlying-infrastructure-shared"></a>Temel alınan altyapı paylaşılıyor mu?

Hayır, özel bulut konakları ve kümeleri ayrılmış ve kullanılmadan önce ve güvenli bir şekilde silinir.

### <a name="what-are-the-minimum-and-maximum-number-of-hosts-per-cluster"></a>Küme başına en düşük ve en yüksek ana bilgisayar sayısı nelerdir?

Kümeler 3 ila 16 ESXi konakları arasında ölçeklendirebilir. Deneme kümeleri üç ana bilgisayar ile sınırlıdır.

### <a name="can-i-scale-my-private-cloud-clusters"></a>Özel bulut kümelerimi ölçeklendirebilir miyim?

Evet, kümeler en düşük ve en fazla ESXi Konağı sayısı arasında ölçeklendirilir. Deneme kümeleri üç ana bilgisayar ile sınırlıdır.

### <a name="what-are-trial-clusters"></a>Deneme kümeleri nedir?

Deneme kümeleri, Azure VMware çözümü özel bulutlarının tek aylık değerlendirmeleri için kullanılan üç ana bilgisayar kümeleridir.

### <a name="can-i-use-high-end-hosts-for-trial-clusters"></a>Deneme kümeleri için yüksek kaliteli Konakları kullanabilir miyim?

Hayır. Yüksek kaliteli ESXi Konakları üretim kümelerinde kullanılmak üzere ayrılmıştır.

## <a name="azure-vmware-solution-and-vmware-software"></a>Azure VMware çözümü ve VMware yazılımı

### <a name="what-versions-of-vmware-software-is-used-in-private-clouds"></a>Özel bulutlarda VMware yazılımının hangi sürümleri kullanılıyor?

[!INCLUDE [vmware-software-versions](includes/vmware-software-versions.md)]


### <a name="do-private-clouds-use-vmware-nsx"></a>Özel bulutlar VMware NSX kullanıyor mu?

Evet, NSX-T 2,5, Azure VMware Çözüm özel bulutlarında yazılım tanımlı ağ için kullanılır.

### <a name="can-i-use-vmware-nsx-v-in-a-private-cloud"></a>VMware NSX-V ' y I özel bir bulutta kullanabilir miyim?

Hayır. NSX-T, NSX 'in desteklenen tek sürümüdür.

### <a name="is-nsx-required-in-on-premises-environments-or-networks-that-connect-to-a-private-cloud"></a>NSX, şirket içi ortamlarda veya özel bir buluta bağlanan ağlarda gereklidir mi?

Hayır, NSX 'i şirket içinde kullanmak zorunda değilsiniz.

### <a name="what-is-the-upgrade-and-update-schedule-for-vmware-software-in-a-private-cloud"></a>Özel buluttaki VMware yazılımı için yükseltme ve güncelleştirme zamanlaması nedir?

Özel bulut yazılım paketi yükseltmeleri, Yazılımı VMware 'den en son yazılım paketi sürümünün bir sürümü içinde tutar. Özel bulut yazılımı sürümleri, bireysel yazılım bileşenlerinin en son sürümlerinden (ESXi, NSX-T, vCenter, vSAN) farklı olabilir.

### <a name="how-often-will-the-private-cloud-software-stack-be-updated"></a>Özel bulut yazılım yığını ne sıklıkla güncelleştirilir?

Özel bulut yazılımı, VMware 'den yazılım paketi yayınını izleyen bir zamanlamaya göre yükseltilir. Özel bulutunuz yükseltmeler için kapalı kalma süresi gerektirmez.

## <a name="connectivity"></a>Bağlantı

### <a name="what-network-ip-address-planning-is-required-to-incorporate-private-clouds-with-on-premises-environments"></a>Şirket içi ortamlarla özel bulutlar eklemek için hangi ağ IP adresi planlaması gerekir?

Bir Azure VMware çözümü özel bulutu dağıtmak için özel bir ağ/22 adres alanı gerekir. Bu özel adres alanı, bir abonelikteki veya şirket içi ağlardaki diğer sanal ağlarla çakışmamalıdır.
 
### <a name="how-do-i-connect-from-on-premises-environments-to-an-azure-vmware-solution-private-cloud"></a>Şirket içi ortamlardan Azure VMware Çözüm özel bulutuna Nasıl yaparım? mı bağlanacak?

Hizmete iki yöntemden birini kullanabilirsiniz: 

- Özel buluta ExpressRoute aracılığıyla eşlenen bir Azure sanal ağı üzerinde dağıtılan bir VM veya uygulama ağ geçidi ile.
- ExpressRoute aracılığıyla şirket içi veri merkezinizden bir Azure ExpressRoute devresine Global Reach.

### <a name="how-do-i-connect-a-workload-vm-to-the-internet-or-an-azure-service-endpoint"></a>Bir iş yükü VM 'sini Internet 'e veya Azure hizmeti uç noktasına bağlamak Nasıl yaparım?.

Azure portal bir özel bulut için internet bağlantısını etkinleştirin. NSX-T Yöneticisi ile bir NSX-T T1 yönlendiricisi ve bir mantıksal anahtar oluşturun. Ardından, mantıksal anahtar tarafından tanımlanan ağ kesimine bir VM dağıtmak için vCenter 'ı kullanırsınız. Bu VM 'nin internet ve Azure hizmetlerine ağ erişimi olur.

### <a name="do-i-need-to-restrict-access-from-the-internet-to-vms-on-logical-networks-in-a-private-cloud"></a>İnternet 'ten özel bir buluttaki mantıksal ağlardaki VM 'lere erişimi kısıtlayacağım mi?

Hayır. İnternet 'ten doğrudan özel bulutlara gelen ağ trafiğine varsayılan olarak izin verilmez.  Ancak, Azure VMware Çözüm özel bulutunuz için Azure portal [genel IP](public-ip-usage.md) seçeneği aracılığıyla Azure VMware Çözüm VM 'lerini Internet 'te kullanıma sunabileceksiniz.

### <a name="do-i-need-to-restrict-internet-access-from-vms-on-logical-networks-to-the-internet"></a>Mantıksal ağlardaki VM 'lerden internet 'e erişimi kısıtlam gerekir mi?

Evet. İnternet 'e yönelik VM erişimini kısıtlamak için bir güvenlik duvarı oluşturmak üzere NSX-T Yöneticisi ' ni kullanmanız gerekir.


### <a name="can-azure-vmware-solution-use-azure-virtual-wan-hosted-expressroute-gateways"></a>Azure VMware çözümü, Azure sanal WAN barındırılan ExpressRoute ağ geçitlerini kullanabilir mi?
Evet.

### <a name="can-transit-connectivity-be-established-between-on-premises-and-azure-vmware-solution-through-azure-virtual-wan-over-expressroute-global-reach"></a>ExpressRoute Global Reach üzerinden Azure sanal WAN aracılığıyla şirket içi ve Azure VMware çözümü arasında geçiş bağlantısı kurulabilir mi?
Azure sanal WAN, iki bağlı ExpressRoute bağlantı hattı ve sanal WAN ExpressRoute ağ geçidi arasında geçişli yönlendirme sağlamaz. ExpressRoute Global Reach kullanmak şirket içi ve Azure VMware çözümü arasında bağlantıya izin verir, ancak sanal WAN hub 'ı yerine Microsoft 'un küresel ağı üzerinden gider.

### <a name="could-i-use-hcx-through-public-internet-communications-as-a-workaround-for-the-non-supportability-of-hcx-when-using-vpn-s2s-with-vwan-for-on-premises-communications"></a>Şirket içi iletişimler için vWAN ile birlikte VPN S2S kullanılırken HCX aracılığıyla genel Internet iletişimlerini desteklenemeyen bir geçici çözüm olarak kullanabilir miyim?

Şu anda, VMware HCX için desteklenen tek yöntem ExpressRoute 'u.

## <a name="accounts-and-privileges"></a>Hesaplar ve ayrıcalıklar

### <a name="what-accounts-and-privileges-will-i-get-with-my-new-azure-vmware-solution-private-cloud"></a>Yeni Azure VMware çözümü özel bulutum ile hangi hesapları ve ayrıcalıkları alabilirim?

NSX-T Manager 'da vCenter ve yönetici erişimi içindeki bir cloudadmin kullanıcısı için kimlik bilgileri sağlıyoruz. Ayrıca, Azure Active Directory birleştirmek için kullanılabilecek bir CloudAdmin grubu da vardır. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).

### <a name="can-have-administrator-access-to-esxi-hosts"></a>ESXi konaklarına yönetici erişimi olabilir mi?

Hayır, ESXi 'ye yönetici erişimi, çözümün güvenlik gereksinimlerini karşılayacak şekilde kısıtlıdır.

### <a name="what-privileges-and-permissions-will-i-have-in-vcenter"></a>VCenter 'da hangi ayrıcalıkların ve izinlerin olması gerekir?

CloudAdmin grup ayrıcalıklarınız olacaktır. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).

### <a name="what-privileges-and-permissions-will-i-have-on-the-nsx-t-manager"></a>NSX-T Manager 'da hangi ayrıcalıkları ve izinleri almam gerekir?

NSX-T üzerinde tam yönetici ayrıcalıklarına sahip olacaksınız ve şirket içi NSX-T veri merkezi 'nde olduğu gibi vSphere rol tabanlı erişim denetimini yönetebilirsiniz. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).

> [!NOTE]
> Bir T0 yönlendiricisi, özel bulut dağıtımının bir parçası olarak oluşturulur ve yapılandırılır. Bu mantıksal yönlendiricide veya NSX-T Edge düğümü VM 'lerinde yapılan herhangi bir değişiklik, özel bulutunuzun bağlantısını etkileyebilir.

## <a name="billing-and-support"></a>Faturalandırma ve destek

### <a name="how-will-pricing-be-structured-for-azure-vmware-solution"></a>Azure VMware çözümü için fiyatlandırma nasıl yapılandırılır?

Fiyatlandırma hakkında genel sorular için bkz. Azure VMware Çözüm [fiyatlandırma](https://azure.microsoft.com/pricing/details/azure-vmware) sayfası. 

### <a name="can-azure-vmware-solution-be-purchased-through-a-microsoft-csp"></a>Azure VMware çözümü bir Microsoft CSP aracılığıyla satın alınabilir mi?

Evet, müşteriler Azure VMware çözümünü bir CSP tarafından yönetilen bir Azure aboneliği içinde dağıtabilir.

### <a name="who-supports-azure-vmware-solution"></a>Azure VMware çözümünü kimler destekler?

Microsoft, Azure VMware çözümü için destek sunar. Bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)gönderebilirsiniz. 

CSP ile yönetilen abonelikler için, ilk destek düzeyi çözüm sağlayıcısını diğer Azure hizmetleri için CSP ile aynı şekilde sağlar.

### <a name="what-accounts-do-i-need-to-create-an-azure-vmware-solution-private-cloud"></a>Azure VMware çözümü özel bulutu oluşturmak için hangi hesaplara ihtiyacım var?

Azure aboneliğinde bir Azure hesabınızın olması gerekir.

### <a name="are-red-hat-solutions-supported-on-azure-vmware-solution"></a>Red hat çözümleri Azure VMware çözümünde destekleniyor mu?

Microsoft ve Red hat, Azure platformunda çalışan Red Hat ekosistemlerine yönelik Birleşik bir iletişim noktası sağlayan tümleşik, birlikte bulunan bir destek ekibini paylaşır.  Red Hat Enterprise Linux ile çalışan diğer Azure platformu hizmetleri gibi Azure VMware çözümü de bulut erişimi ve tümleşik destek şemsiye altındadır. Red Hat Enterprise Linux Azure 'da Azure VMware çözümünün üzerine çalıştırmak için desteklenir.

### <a name="is-vmware-hcx-enterprise-available-and-if-so-how-much-does-it-cost"></a>VMware HCX Enterprise mevcuttur ve bu durumda ne kadar ücret maliyetlidir?

VMware HCX Enterprise, *Önizleme* işlevi/hizmeti olarak Azure VMware çözümü ile kullanılabilir. Azure VMware çözümü için VMware HCX Enterprise önizleme aşamasındadır. Bu, ücretsiz bir işlev/hizmet ve hizmet hüküm ve koşullarına tabidir. VMware HCX Enterprise hizmeti GA olduktan sonra, faturalandırmaya geçiş yapılacak 30 günlük bir bildirim alırsınız. Hizmeti devre dışı bırakabilirsiniz veya hizmetten vazgeçebilirsiniz.

### <a name="how-do-i-request-a-host-quota-increase-for-azure-vmware-solution"></a>Nasıl yaparım? Azure VMware çözümü için bir konak kotası artışı ister misiniz?

CSP ile yönetilen abonelikler için müşterinin isteği iş ortağına göndermesi gerekir. Daha sonra iş ortağı ekibi, abonelik için artan kota artışı sağlamak üzere Microsoft ile ilgilenir. Daha fazla bilgi için bkz. [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md). 

EA abonelikleri için aşağıdaki yordamı kullanın. Öncelikle şunları yapmanız gerekir:

* Microsoft ile [Azure Kurumsal Anlaşma (EA)](../cost-management-billing/manage/ea-portal-agreements.md) .
* Azure aboneliğindeki bir Azure hesabı.

Azure VMware Çözüm kaynağınızı oluşturabilmeniz için önce, konaklarınızın ayrılmaları için bir destek bileti göndereceğiz. İsteğinizi onaylamak ve yerine getirmek için beş adede kadar iş günü sürer. Mevcut bir Azure VMware çözümü özel bulutunuz varsa ve daha fazla ana bilgisayar ayırmak istiyorsanız, aynı işlemden geçmeniz gerekir.

1. Azure portal, **Yardım + Destek** altında, **[Yeni bir destek isteği](https://rc.portal.azure.com/#create/Microsoft.Support)** oluşturun ve bilet için aşağıdaki bilgileri sağlayın:
   - **Sorun türü:** Teknik
   - **Abonelik:** Aboneliğinizi seçin
   - **Hizmet:** Tüm hizmetler Azure VMware çözümüne >
   - **Kaynak:** Genel soru 
   - **Özet:** Kapasiteye ihtiyaç duyuyor
   - **Sorun türü:** Kapasite yönetimi sorunları
   - **Sorun alt türü:** Ek konak kotası/kapasitesi için müşteri Isteği

1. Destek bileti **açıklamasında** , **Ayrıntılar** sekmesinde şunları belirtin:

   - POC veya üretim 
   - Bölge Adı
   - Ana bilgisayar sayısı
   - Diğer ayrıntılar

   >[!NOTE]
   >Azure VMware çözümü, özel bulutunuzu ve artıklığı N + 1 ana bilgisayarlarını çalıştırmak için en az üç ana bilgisayar önerir. 

1. İsteği göndermek için **gözden geçir + oluştur** ' u seçin.

   Bir destek temsilcisinin isteğinizi onaylamasını beş adede kadar iş günü sürer.

   >[!IMPORTANT] 
   >Zaten mevcut bir Azure VMware çözümünüz varsa ve ek konaklar istemeniz durumunda, Konakları ayırmak için beş iş gününe ihtiyacımız olduğunu lütfen unutmayın. 

1. Konaklarınızı sağlayabilmeniz için önce **Microsoft. AVS** kaynak sağlayıcısını Azure Portal kaydettiğinizden emin olun.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Kaynak sağlayıcısını kaydetme hakkında daha fazla yol için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md). 

### <a name="are-reserved-instances-available-for-purchasing-through-the-cloud-solution-provider-csp-program"></a>Ayrılmış örnekler, bulut çözümü sağlayıcısı (CSP) programı aracılığıyla satın alınabilir mi?

Evet. CSP, müşterileri için ayrılmış örnekler satın alabilir. Daha fazla bilgi için bkz. [ayrılmış örnekle maliyetleri kaydetme](reserved-instance.md). 

### <a name="does-azure-vmware-solution-offer-multi-tenancy-for-hosting-csp-partners"></a>Azure VMware çözümü, CSP iş ortaklarını barındırmak için çok kiracılı bir teklif sunuyor mu?

Hayır. Şu anda Azure VMware çözümü çok kiracılı bir sunmaz.

### <a name="will-traffic-between-on-premises-and-azure-vmware-solution-over-expressroute-incur-any-outbound-data-transfer-charge-in-the-metered-data-plan"></a>ExpressRoute üzerinden şirket içi ve Azure VMware çözümü arasındaki trafik, ölçülen veri planındaki giden veri aktarımı ücretlerine neden olur?

Azure VMware Çözüm ExpressRoute bağlantı hattının trafiği hiçbir şekilde tarifeli değildir. ExpressRoute bağlantı hattınızdan şirket içi olarak Azure 'a bağlanan trafik, ExpressRoute fiyatlandırma planlarına göre ücretlendirilir.


## <a name="customer-communication"></a>Müşteri iletişimi

### <a name="how-can-i-receive-an-alert-when-azure-sends-service-health-notifications-to-my-azure-subscription"></a>Azure, Azure aboneliğime hizmet durumu bildirimleri gönderdiğinde nasıl uyarı alabilirim?

Hizmet sorunları, planlı bakım, sistem durumu danışmanları, Güvenlik Danışma belgeleri bildirimleri Azure portal **hizmet durumu** üzerinden yayımlanır.  Bu bildirimler için etkinlik günlüğü uyarılarını ayarlarken zamanında işlem yapabilirsiniz. Daha fazla bilgi için bkz. [Azure Portal kullanarak hizmet durumu uyarıları oluşturma](../service-health/alerts-activity-log-service-notifications-portal.md#create-service-health-alert-using-azure-portal).

:::image type="content" source="media/service-health.png" alt-text="Hizmet durumu bildirimlerinin ekran görüntüsü":::



<!-- LINKS - external -->
[kb2106952]: https://kb.vmware.com/s/article/2106952?lang=en_US&queryTerm=21069522

<!-- LINKS - internal -->
[Access and Identity Concepts]: concepts-identity.md
