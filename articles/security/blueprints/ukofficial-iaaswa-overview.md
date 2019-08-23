---
title: UK RESMI için Azure Güvenlik ve Uyumluluk Şeması-üç katmanlı IaaS Web uygulaması
description: UK RESMI için Azure Güvenlik ve Uyumluluk Şeması-üç katmanlı IaaS Web uygulaması
services: security
author: jomolesk
ms.assetid: 9c32e836-0564-4906-9e15-f070d2707e63
ms.service: security
ms.topic: article
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 602e4356ccd9eb45855462a7a25e0966dc176b4f
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69899936"
---
# <a name="azure-security-and-compliance-blueprint---three-tier-iaas-web-application-for-uk-official"></a>UK RESMI için Azure Güvenlik ve Uyumluluk Şeması-üç katmanlı IaaS Web uygulaması

## <a name="overview"></a>Genel Bakış

 Bu makalede, Birleşik Krallık 'ta RESMI olarak sınıflandırılmış çok sayıda iş yükünü işlemeye uygun Microsoft Azure üç katmanlı bir Web tabanlı mimari sunmaya yönelik rehberlik ve otomasyon betikleri sunulmaktadır.

 Kod yaklaşımı olarak bir altyapı kullandığınızda [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) şablonlar KÜMESI, UK National Cyber güvenlik MERKEZI (NCSC) 14 [bulut güvenlik Ilkelerine](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) ve Internet güvenliği (CIS) [merkezine göre orta olan bir ortam dağıtır Kritik güvenlik denetimleri](https://www.cisecurity.org/critical-controls.cfm).

 NCSC, müşteriler tarafından hizmetin güvenlik özelliklerini değerlendirmek ve müşteri ile tedarikçi arasındaki sorumluluk bölümünü anlamanıza yardımcı olmak için bulut güvenlik Ilkelerini önerir. Sorumlulukların bölünmesini anlamanıza yardımcı olması için bu ilkelerin her birine karşı bilgi sağladık.

 Bu mimari ve karşılık gelen Azure Resource Manager şablonları, [Microsoft Azure kullanılarak Birleşik Krallık bulutu için 14 bulut güvenlik denetimleri](https://gallery.technet.microsoft.com/14-Cloud-Security-Controls-670292c1)Microsoft teknik incelemesi tarafından desteklenir. Bu şekilde, Azure hizmetleri 'nin UK NCSC 14 bulut güvenlik Ilkelerine nasıl izleneceği catalogues, böylece kuruluşların bulut tabanlı Hizmetleri kullanarak uyumluluk yükümlülüklerini, genel olarak ve Microsoft Azure üzerinde UK una.

 Bu şablon, iş yükü için altyapıyı dağıtır. Uygulama kodu ve destekleyici iş katmanı ve veri katmanı yazılımının yüklenmiş ve yapılandırılmış olması gerekir. Ayrıntılı dağıtım yönergelerine [buradan](https://aka.ms/ukwebappblueprintrepo)ulaşabilirsiniz.

 Azure aboneliğiniz yoksa, hızlı bir şekilde kaydolabilir ve [Azure ile kolayca çalışmaya başlayabileceksiniz](https://azure.microsoft.com/get-started/).

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

 Azure şablonları, UK RESMI iş yüklerini destekleyen bir Azure bulut ortamında üç katmanlı bir Web uygulaması mimarisi sunar. Mimari, şirket içi bir ağı Azure 'a genişleten güvenli bir karma ortam sunarak, Web tabanlı iş yüklerine Şirket kullanıcıları veya internet üzerinden güvenli bir şekilde erişilmesini sağlar.

![UK resmi başvuru mimarisi diyagramı Için üç katmanlı IaaS Web uygulaması](images/ukofficial-iaaswa-architecture.png?raw=true "UK resmi başvuru mimarisi diyagramı Için üç katmanlı IaaS Web uygulaması")

 Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisinin ayrıntıları [dağıtım mimarisi](#deployment-architecture) bölümünde bulunur.

(1)/16 sanal ağ operasyonel VNet
- (3)/24 alt ağ-3 katmanlı (Web, biz, veri)
- (1)/27 alt ağ-ekler
- (1)/27 alt ağ geçidi alt ağı
- (1)/29 alt ağ-Application Gateway alt ağ
- Varsayılan (Azure tarafından sağlanmış) DNS kullanır
- Yönetim VNet 'e eşleme etkinleştirildi
- Trafik akışını yönetmek için ağ güvenlik grubu (NSG)

(1)/24 sanal ağ-yönetim VNet
- (1)/27 alt ağ
- (2) kullanır DNS ve (1) Azure DNS girdileri ekler
- Işletimsel VNet 'e eşleme etkinleştirildi
- Trafik akışını yönetmek için ağ güvenlik grubu (NSG)

(1) Application Gateway
- WAF-etkin
- WAF modu-önleme
- Kural kümesi: OWASP 3,0
- 80 numaralı bağlantı noktasında HTTP dinleyicisi
- NSG aracılığıyla düzenlenen bağlantı/trafik
- Genel IP adresi uç noktası tanımlı (Azure)

(1) VPN-rota tabanlı, site-2-site IPSec VPN tüneli
- Genel IP adresi uç noktası tanımlı (Azure)
- NSG aracılığıyla düzenlenen bağlantı/trafik
- (1) yerel ağ geçidi (Şirket içi uç noktası)
- (1) Azure ağ geçidi (Azure uç noktası)

(9) sanal makineler-tüm VM 'Ler Azure IaaS kötü amaçlı yazılımdan koruma DSC ayarları ile dağıtılır

- (2) Active Directory Domain Services etki alanı denetleyicileri (Windows Server 2012 R2)
  - (2) DNS sunucu rolleri-VM başına 1
  - (2) VM başına Işlemsel VNet 'e bağlı NIC 'ler-1
  - Her ikisi de, şablonda tanımlı etki alanına katılmış
    - Dağıtımın bir parçası olarak oluşturulan etki alanı


- (1) atlama kutusu (Savunma ana bilgisayarı) yönetim VM
  - Genel IP adresine sahip yönetim VNet üzerinde 1 NIC
    - NSG, trafiği belirli kaynaklarla sınırlamak için kullanılır
  - Etki alanına katılmamış


- (2) Web katmanı VM 'Leri
  - (2) IIS sunucu rolleri-VM başına 1
  - (2) VM başına Işlemsel VNet 'e bağlı NIC 'ler-1
  - Etki alanına katılmamış


- (2) Biz bir katman VM 'Leri
  - (2) VM başına Işlemsel VNet 'e bağlı NIC 'ler-1
  - Etki alanına katılmamış


- (2) veri katmanı VM 'Leri
  - (2) VM başına Işlemsel VNet 'e bağlı NIC 'ler-1
  - Etki alanına katılmamış

Kullanılabilirlik Kümeleri
- (1) Active Directory Etki Alanı Denetleyicisi VM kümesi-2 VM
- (1) Web katmanı VM kümesi-2 VM
- (1) Biz bir katman VM Set-2 VM
- (1) veri katmanı VM kümesi-2 VM

Yük Dengeleyici
- (1) Web katmanı Load Balancer
- (1) biz katman Load Balancer
- (1) veri katmanı Load Balancer

Depolama
- (14) toplam depolama hesabı
  - Active Directory Etki Alanı denetleyicisi kullanılabilirlik kümesi
    - (2) birincil yerel olarak yedekli depolama (LRS) hesapları-her VM için 1  
    - (1) kullanılabilirlik kümesi ekleme için yerel olarak yedekli depolama (LRS) hesabı
  - Yönetim sıçrama kutusu VM
    - (1) sıçrama kutusu VM 'si için birincil yerel olarak yedekli depolama (LRS) hesabı
    - (1) sıçrama kutusu VM için yerel olarak yedekli depolama (LRS) hesabı
  - Web katmanı VM 'Leri
    - (2) birincil yerel olarak yedekli depolama (LRS) hesapları-her VM için 1  
    - (1) Web katmanı kullanılabilirlik kümesi için yerel olarak yedekli depolama (LRS) hesabı
  - Biz katman VM 'Leri
    - (2) birincil yerel olarak yedekli depolama (LRS) hesapları-her VM için 1  
    - (1) biz için yerel olarak yedekli depolama (LRS) hesap kullanılabilirlik kümesi
  - Veri katmanı VM 'Leri
    - (2) birincil yerel olarak yedekli depolama (LRS) hesapları-her VM için 1  
    - (1) veri katmanı kullanılabilirlik kümesi için yerel olarak yedekli depolama (LRS) hesabı

### <a name="deployment-architecture"></a>Dağıtım mimarisi:

**Şirket Içi ağ**: Bir kuruluşta uygulanan özel bir yerel alan ağı.

**Üretim sanal ağı**: Üretim [VNET](https://docs.microsoft.com/azure/Virtual-Network/virtual-networks-overview) (sanal ağ), Azure 'da çalışan uygulamayı ve diğer işletimsel kaynakları barındırır. Her VNet, ağ trafiğini yalıtmak ve yönetmek için kullanılan birkaç alt ağ içerebilir.

**Web katmanı**: Gelen HTTP isteklerini işler. Yanıtlar bu katman üzerinden döndürülür.

**Iş katmanı**: , Sistem için iş süreçlerini ve diğer işlevsel mantığı uygular.

**Veritabanı katmanı**: Yüksek kullanılabilirlik için [Always on kullanılabilirlik grupları SQL Server](https://msdn.microsoft.com/library/hh510230.aspx) kullanarak kalıcı veri depolama sağlar. Müşteriler, [Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) 'Nı PaaS alternatifi olarak kullanabilir.

**Ağ geçidi**: [VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) , şirket içi ağdaki ve üretim VNET 'teki yönlendiriciler arasında bağlantı sağlar.

**Internet ağ geçidi ve genel IP adresi**: İnternet ağ geçidi, uygulama hizmetlerini internet üzerinden kullanıcılara sunar. Bu hizmetlere erişen trafik, Web uygulaması güvenlik duvarı (WAF) koruması ile katman 7 yönlendirme ve yük dengeleme özellikleri sunan bir [Application Gateway](../../application-gateway/overview.md) kullanılarak güvenli hale getirilir.

**Yönetim VNET**: Bu [sanal](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) ağ, üretim VNET 'te çalışan iş yükleri için yönetim ve izleme yeteneklerini uygulayan kaynaklar içerir.

Sıçrama **kutusu**: Ayrıca, yöneticinin üretim VNet 'teki VM 'lere bağlanmak için kullandığı ağ üzerinde güvenli bir VM olan savunma [ana bilgisayarı](https://en.wikipedia.org/wiki/Bastion_host)da denir. Sıçrama kutusunun sadece güvenli bir listede yer alan genel IP adreslerinden gelen uzak trafiğe izin veren bir NSG’si vardır. Uzak Masaüstü (RDP) trafiğine izin vermek için, trafiğin kaynağının NSG 'de tanımlanması gerekir. Üretim kaynaklarının yönetimi, güvenli bir sıçrama kutusu VM 'si kullanılarak RDP aracılığıyla yapılır.

**Kullanıcı tanımlı rotalar**: [Kullanıcı tanımlı yollar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) , Azure sanal AĞLARı 'nda IP trafiği akışını tanımlamak için kullanılır.

**Ağ eşlenmiş sanal ağlar**: Üretim ve yönetim sanal ağları [VNET eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)kullanılarak bağlanır.
Bu sanal ağlar ayrı kaynaklar olarak yönetilmeye devam eder, ancak bu sanal makinelerin tüm bağlantı amaçları için bir tane olarak görünürler. Bu ağlar, özel IP adresleri kullanarak birbirleriyle doğrudan iletişim kurar. VNet eşlemesi aynı Azure bölgesinde olan sanal ağlara tabidir.

**Ağ güvenlik grupları**: [NSG](../../virtual-network/virtual-network-vnet-plan-design-arm.md) 'Ler, VNET içindeki trafiğe izin veren veya reddeden Access Control listeler içerir. NSG 'ler, trafiği bir alt ağda veya tek bir VM düzeyinde güvenli hale getirmek için kullanılabilir.

**Active Directory Domain Services (AD DS)** : Bu mimari, adanmış bir [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) dağıtımı sağlar.

**Günlüğe kaydetme ve denetleme**: [Azure etkinlik günlüğü](../../azure-monitor/platform/activity-logs-overview.md) , aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemleri, işlemi kimin başlatmadığını, işlem oluştuğunda, işlemin durumunu ve işlemi araştırmanıza yardımcı olabilecek diğer özelliklerin değerlerini yakalar. Azure etkinlik günlüğü, bir abonelikteki tüm eylemleri yakalayan bir Azure platform hizmetidir. Günlükler, gerekirse arşivlenebilir veya verilebilirler.

**Ağ izleme ve uyarı**verme: [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) , sanal ağlarınız içinde ağ traffics için ağ paketi yakalama, akış günlüğü, topoloji araçları ve tanılama özellikleri sağlar.

## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler

### <a name="business-continuity"></a>İş Sürekliliği

**Yüksek kullanılabilirlik**: Sunucu iş yükleri, Azure 'da sanal makinelerin yüksek kullanılabilirliğini sağlamaya yardımcı olmak için bir [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) gruplandırılır. Bu yapılandırma, planlı veya plansız bir bakım olayı sırasında en az bir sanal makinenin kullanılabilir olmasını ve% 99,95 Azure SLA 'sını karşılamasını sağlamaya yardımcı olur.

### <a name="logging-and-audit"></a>Günlüğe kaydetme ve denetleme

**İzleme**: [Azure izleyici](../../azure-monitor/overview.md) , tüm Azure kaynaklarınızın etkinlik günlüğü, ölçümleri ve tanılama günlüklerini izlemek için tek bir kaynak sağlayan platform hizmetidir. Azure Izleyici, Azure 'daki kaynaklardan gelen ölçümleri ve günlükleri görselleştirmek, sorgulamak, yönlendirmek, arşivlemek ve üzerinde işlem yapacak şekilde yapılandırılabilir. Kullanıcıların günlükleri değiştirme yeteneğine sahip olmamasını sağlamaya yardımcı olmak üzere, denetim izini güvenli hale getirmek için kaynak tabanlı Access Control kullanılması önerilir.

**Etkinlik günlükleri**: Aboneliğinizdeki kaynaklarda gerçekleştirilen işlemlere ilişkin Öngörüler sağlamak için [Azure etkinlik günlüklerini](../../azure-monitor/platform/activity-logs-overview.md) yapılandırın.

**Tanılama günlükleri**: [Tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md) , bir kaynak tarafından oluşturulan tüm günlüklerdir. Bu Günlükler Windows olay sistemi günlükleri, blob, tablo ve kuyruk günlüklerini içerebilir.

**Güvenlik duvarı günlükleri**: Application Gateway, tam tanılama ve erişim günlükleri sağlar. Güvenlik duvarı günlükleri, WAF’nin etkin olduğu application gateway kaynakları için kullanılabilir.

**Günlük arşivleme**: Günlük veri depolaması, arşiv için merkezi bir Azure depolama hesabına ve tanımlanan bir saklama süresine yazacak şekilde yapılandırılabilir. Günlükler, Azure Izleyici günlükleri veya üçüncü taraf SıEM sistemleri kullanılarak işlenebilir.

### <a name="identity"></a>Kimlik

**Active Directory Domain Services**: Bu mimari Azure 'da bir Active Directory Domain Services dağıtımı sağlar. Azure’da Active Directory uygulamaya yönelik belirli öneriler için aşağıdaki makalelere bakın:

[Active Directory Domain Services (AD DS) Azure 'A genişletme](/azure/architecture/reference-architectures/identity/adds-extend-domain).

[Azure sanal makinelerinde Windows Server Active Directory dağıtmaya yönelik yönergeler](https://msdn.microsoft.com/library/azure/jj156090.aspx).

**Active Directory tümleştirme**: Özel bir AD DS mimarisine alternatif olarak müşteriler, Azure 'da [Azure Active Directory](/azure/architecture/reference-architectures/identity) tümleştirme veya [Active Directory Şirket içi bir ormana katılmış](/azure/architecture/reference-architectures/identity)olarak kullanmak isteyebilir.

### <a name="security"></a>Güvenlik

**Yönetim güvenliği**: Bu şema, yöneticilerin güvenilen bir kaynaktaki RDP kullanarak yönetim VNet 'e ve atlama kutusuna bağlanmasına olanak sağlar. Yönetim VNet için ağ trafiği NSG 'ler kullanılarak denetlenir. 3389 numaralı bağlantı noktasına erişim, JumpBox içeren alt ağa erişebilen güvenilir bir IP aralığından gelen trafikle kısıtlıdır.

Müşteriler ayrıca yönetim VNet ve sıçrama kutusu 'na bağlanırken ortamın güvenliğini sağlamak için [geliştirilmiş bir güvenlik yönetim modeli](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access) kullanmayı da düşünebiliriz. Gelişmiş güvenlik müşterilerinin [ayrıcalıklı bir erişim Iş istasyonu](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations#what-is-a-privileged-access-workstation-paw) ve RDGateway yapılandırması kullanması önerilir. Ağ sanal gereçlerini ve Public/Private DMZs kullanımı, daha fazla güvenlik geliştirmesi sunar.

**Ağın güvenliğini sağlama**: [Ağ güvenlik grupları](../../virtual-network/virtual-network-vnet-plan-design-arm.md) (NSG 'ler), yanlış yapılandırılmış veya devre dışı bir ağ geçidini atlayarak gelen trafiğe karşı ikinci bir koruma düzeyi sağlamak üzere her alt ağ için önerilir. Örnek- [NSG dağıtmak için şablon kaynak yöneticisi](https://github.com/mspnp/template-building-blocks/tree/v1.0.0/templates/buildingBlocks/networkSecurityGroups).

**Genel uç noktaların güvenliğini sağlama**: İnternet ağ geçidi, uygulama hizmetlerini internet üzerinden kullanıcılara sunar. Bu hizmetlere erişen trafik, Web uygulaması güvenlik duvarı ve HTTPS protokol yönetimi sağlayan bir [Application Gateway](../../application-gateway/overview.md)kullanılarak güvenli hale getirilir.

**IP aralıkları**: Mimarideki IP aralıkları önerilen aralıklardır. Müşterilerin kendi ortamlarını göz önünde bulundurmanız ve uygun aralıkları kullanması önerilir.

**Karma bağlantı**: Bulut tabanlı iş yükleri, Azure VPN Gateway kullanarak ıPSEC VPN aracılığıyla şirket içi veri merkezine bağlanır. Müşteriler, Azure 'a bağlanmak için uygun bir VPN Gateway kullandıklarından emin olmalıdır. Örnek- [VPN Gateway Kaynak Yöneticisi şablonu](https://github.com/mspnp/template-building-blocks/tree/v1.0.0/templates/buildingBlocks/vpn-gateway-vpn-connection). Büyük veri gereksinimleriyle büyük ölçekli, görev açısından kritik iş yüklerini çalıştıran müşteriler, Microsoft bulut hizmetlerine özel ağ bağlantısı için [ExpressRoute](/azure/architecture/reference-architectures/hybrid-networking/expressroute) kullanan bir karma ağ mimarisini düşünmek isteyebilir.

**Kaygıları ayrımı**: Bu başvuru mimarisi, yönetim işlemleri ve iş işlemleri için sanal ağları ayırır. Ayrı sanal ağlar ve alt ağlar, [Microsoft bulut hizmetleri ve ağ güvenliği](/azure/architecture/vdc/networking-virtual-datacenter) en iyi yöntemlerinden sonraki ağ kesimleri arasında NSG 'ler kullanarak trafik giriş ve çıkış kısıtlamaları dahil trafik yönetimine izin verir.

**Kaynak yönetimi**: VM 'Ler, sanal ağlar ve yük dengeleyiciler gibi Azure kaynakları, bunları [Azure Kaynak gruplarında](../../azure-resource-manager/resource-group-overview.md)gruplandırarak yönetilir. Kaynak tabanlı Access Control rolleri, erişimi yalnızca yetkili kullanıcılara kısıtlamak için her bir kaynak grubuna atanabilir.

**Access Control kısıtlamaları**: [Rol tabanlı Access Control](../../role-based-access-control/role-assignments-portal.md) (RBAC) kullanarak uygulamanızdaki kaynakları yönetmek için [özel rolleri](../../role-based-access-control/custom-roles.md) kullanın RBAC, DevOps 'un her katmanda gerçekleştirebileceği işlemleri kısıtlamak için kullanılabilir. İzin verirken, [en az ayrıcalık ilkesini](https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1)kullanın. Tüm yapılandırma değişikliklerinin planlı olduğundan emin olmak için yönetim işlemlerinin tümünü günlüğe kaydedin ve normal denetimler gerçekleştirin.

**Internet erişimi**: Bu başvuru mimarisi, internet 'e yönelik ağ geçidi ve yük dengeleyici olarak [Azure Application Gateway](../../application-gateway/overview.md) kullanır. Bazı müşteriler, [Azure Application Gateway](../../application-gateway/overview.md)alternatif olarak ek ağ güvenliği katmanları için üçüncü taraf ağ sanal gereçlerini kullanmayı da göz önünde bulundurabiliriz.

**Azure Güvenlik Merkezi**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) , abonelikteki kaynakların güvenlik durumunun merkezi bir görünümünü sağlar ve güvenliği aşılmış kaynakların önlenmesine yardımcı olan öneriler sağlar. Ayrıca, daha ayrıntılı ilkeleri etkinleştirmek için de kullanılabilir. Örneğin, ilkeler belirli kaynak gruplarına uygulanabilirler. Bu, kuruluşun kendi durumunu riske göre uyarlayabilmenizi sağlar. Müşterilerin Azure aboneliklerinde Azure Güvenlik Merkezi 'Ni etkinleştirmeleri önerilir.

## <a name="ncsc-cloud-security-principles-compliance-documentation"></a>NCSC bulutu güvenlik Ilkeleri uyumluluk belgeleri

Crown Commercial Service (kamu ile ticari ve tedarik etkinliklerini geliştirmek için kullanılan bir acenteler), Microsoft 'un kapsam içi Kurumsal bulut hizmetleri 'nin, tüm tekliflerini RESMI düzeyde kapsayan G-Cloud V6 ile yeniledi. Azure ve G-Cloud ayrıntıları [Azure UK g-Cloud Security değerlendirmesi özetinde](https://www.microsoft.com/en-us/trustcenter/compliance/uk-g-cloud)bulunabilir.

Bu şema, UK-RESMI olarak sınıflandırılan iş yüklerini destekleyen bir ortamın sağlanmasına yardımcı olmak için NCSC [bulut güvenlik ilkelerine](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) göre belgelenen 14 bulut güvenlik ilkelerine hizalanır.

[Müşteri sorumluluğu matrisi](https://aka.ms/ukofficial-crm) (Excel çalışma kitabı) tüm 14 bulut güvenlik ilkelerini listeler ve matris, her prensibi (veya prensibi bölümü), Ilke uygulamasının Microsoft 'un veya müşterinin sorumluluğunda olup olmadığını belirtir. ikisi arasında paylaşılıyor.

[Ilke uygulama matrisi](https://aka.ms/ukofficial-iaaswa-pim) (Excel çalışma kitabı), tüm 14 bulut güvenlik ilkelerini listeler ve matris, müşteri sorumlulukları matrisinde, 1) bir müşteri sorumluluğu atanan her ilke (veya prensip alt bölümü) için belirtilir. şema Otomasyonu, ilkeyi ve 2) uygulamanın, ilke gereksinimlerle nasıl hizalanacağını gösteren bir açıklama uygular.

Ayrıca, bulut güvenlik Birliği (CSA) bulut sağlayıcılarının değerlendirmesinde müşterileri desteklemek ve bulut hizmetlerine geçmeden önce yanıtlanması gereken soruları belirlemek için bulut denetimi matrisini yayımladı. Yanıt olarak Microsoft Azure, Microsoft 'un önerilen ilkeleri nasıl ele aldığı hakkında CSA Consensus değerlendirmesi girişim girişimi anketini ([CSA CAIQ](https://www.microsoft.com/en-us/TrustCenter/Compliance/CSA)) yanıtladı.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Dağıtım kullanıcıları bu şema otomasyonunu dağıtmak için kullanabileceğiniz iki yöntem vardır. İlk yöntem bir PowerShell betiği kullanır, ancak ikinci yöntem başvuru mimarisini dağıtmak için Azure portal kullanır. Ayrıntılı dağıtım yönergelerine [buradan](https://aka.ms/ukofficial-iaaswa-repo)ulaşabilirsiniz.

## <a name="disclaimer"></a>Sorumluluk Reddi

 - Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
 - Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
 - Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
 - Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
 - Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
