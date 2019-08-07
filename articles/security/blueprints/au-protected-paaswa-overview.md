---
title: Avustralya için Azure Güvenlik ve Uyumluluk Şeması-PaaS Web uygulaması KORUMALı
description: Avustralya için Azure Güvenlik ve Uyumluluk Şeması-PaaS Web uygulaması KORUMALı
services: security
author: meladie
ms.assetid: 708aa129-b226-4e02-85c6-1f86e54564e4
ms.service: security
ms.topic: article
ms.date: 08/23/2018
ms.author: meladie
ms.openlocfilehash: 294716052869dac03db42feea9ade15d610551e6
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68781114"
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-australia-protected"></a>Avustralya için Azure Güvenlik ve Uyumluluk Şeması-PaaS Web uygulaması KORUMALı

## <a name="overview"></a>Genel Bakış

Bu Azure Güvenlik ve Uyumluluk Şeması, bir hizmet olarak platform (PaaS) ortamının dağıtımı için, AU ile korunan kamu verilerinin koleksiyon, depolama ve alma için uygun, Avustralya sinyalleri (ASD) tarafından üretilen Avustralya kamu bilgi güvenliği el kitabı (ıSM). Bu şema, ortak bir başvuru mimarisini gösterir ve hassas kamu verilerinin güvenli, uyumlu, çok katmanlı bir ortamda doğru işlenmesini göstermeye yardımcı olur.

Bu başvuru mimarisi, uygulama kılavuzu ve tehdit modeli, müşterilerin kendi planlama ve sistem actatsyon süreçlerini benimsemesi için bir temel sağlar ve müşterilerin iş yüklerini ASD ile uyumlu bir şekilde Azure 'a dağıtmalarına yardımcı olur. Müşteriler, Federasyon hizmetlerini kullanmak ve şirket içi kaynakları Azure kaynaklarıyla bütünleştirmek için bir Azure VPN Gateway veya ExpressRoute uygulamayı tercih edebilir. Müşteriler, şirket içi kaynakları kullanmanın güvenlik etkilerine yönelik etkileri göz önünde bulundurmalıdır. Her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebileceğinden, tüm gereksinimleri karşılamak için ek yapılandırma gerekir.

ASD 'nin uyumlu olması için bir bilgi güvenliği kayıtlı denetçisi, sistemi denetler. Müşteriler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebileceğinden, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmelerinden sorumludur.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, Azure SQL veritabanı arka ucu ile PaaS Web uygulaması için bir başvuru mimarisi sağlar. Web uygulaması, bir Azure veri merkezi 'nde özel ve adanmış bir ortam olan yalıtılmış bir Azure App Service Ortamı barındırılır. Ortam yükü, Web uygulaması için trafiği Azure tarafından yönetilen sanal makineler arasında dengeler. Tüm Web uygulaması bağlantıları en düşük 1,2 sürümüne sahip TLS gerektirir. Bu mimaride ağ güvenlik grupları, bir Application Gateway, Azure DNS ve Load Balancer de bulunur.

Mimari, şirket içi ağı Azure 'a genişleten güvenli bir karma ortam sunarak, Web tabanlı iş yüklerinin bir kuruluşun özel yerel ağı veya internet 'ten Şirket kullanıcıları tarafından güvenli bir şekilde erişilmesini sağlar. Şirket içi çözümler için, müşterinin her ikisi de tüm güvenlik, işlem ve uyumluluktan sorumlu ve sorumludur.

Bu çözüme dahil edilen Azure kaynakları, bir VPN Gateway ve ExpressRoute aracılığıyla IPSec VPN aracılığıyla bir şirket içi ağa veya veri merkezi birlikte bulundurma tesisine (örn. Canbere) bağlanabilir. Bir VPN kullanma kararı, iletilen verilerin sınıflandırmasıyla ve ağ yolunun göz önünde bulundurularak yapılmalıdır. Büyük veri gereksinimleriyle büyük ölçekli, görev açısından kritik iş yüklerini çalıştıran müşteriler, Azure hizmetlerine özel ağ bağlantısı için ExpressRoute kullanan bir karma ağ mimarisini düşünmelidir. Azure 'a yönelik bağlantı mekanizmaları hakkında daha fazla bilgi için [rehberlik ve öneriler](#guidance-and-recommendations) bölümüne bakın.

Azure Active Directory Federasyon, kullanıcıların şirket içi Active Directory Federasyon Hizmetleri (AD FS) altyapısını kullanarak kimlik doğrulaması yapmasını ve buluttaki tüm kaynaklara erişmesini sağlamak için kullanılmalıdır. Active Directory Federasyon Hizmetleri (AD FS), bu karma ortam için Basitleştirilmiş, güvenli kimlik Federasyonu ve Web 'de çoklu oturum açma özellikleri sağlayabilir. Kurulum Azure Active Directory daha fazla ayrıntı için [rehberlik ve öneriler](#guidance-and-recommendations) bölümüne bakın.

Çözüm, müşterilerin bekleyen verilerin gizliliğini korumak için Depolama Hizmeti Şifrelemesi kullanmak üzere yapılandırabileceği Azure depolama hesaplarını kullanır. Azure, esneklik için müşterinin seçili bölgesi içinde verilerin üç kopyasını depolar. Azure bölgeleri dayanıklı bölge çiftlerine dağıtılır ve coğrafi olarak yedekli depolama, verilerin üç kopya ile ikinci bölgeye çoğaltılmasını sağlar. Bu, müşterinin birincil veri konumundaki olumsuz bir olayı, veri kaybına neden olur.

Gelişmiş güvenlik için, bu Çözümdeki tüm Azure kaynakları Azure Resource Manager aracılığıyla bir kaynak grubu olarak yönetilir. Azure Active Directory rol tabanlı erişim denetimi, Azure Key Vault dağıtılan kaynak ve anahtarlara erişimi denetlemek için kullanılır. Sistem durumu, Azure Güvenlik Merkezi ve Azure Izleyici aracılığıyla izlenir. Müşteriler günlükleri yakalamak ve sistem durumunu tek ve kolayca gezinebilir bir panoda göstermek için her iki izleme hizmetini de yapılandırır. Azure Application Gateway, önleme modunda bir güvenlik duvarı olarak yapılandırılır ve TLS v 1.2 veya üzeri olmayan trafiğe izin vermez. Çözüm, Web katmanını çok kiracılı olmayan bir ortamda yalıtmak için Azure Application Service Environment v2 'yi kullanır.

![Au korumalı başvuru mimarisi Için PaaS Web uygulaması](images/au-protected-paaswa-architecture.png?raw=true "Au korumalı başvuru mimarisi diyagramı Için PaaS Web uygulaması")

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Daha fazla ayrıntı, [dağıtım mimarisi](#deployment-architecture) bölümünde bulunur.

- Application Gateway
    - Web uygulaması güvenlik duvarı
        - Güvenlik duvarı modu: önleme
        - Kural kümesi: OWASP
        - Dinleyici bağlantı noktası: 443
- Application Insights
- Azure Active Directory
- Azure uygulama hizmeti ortamı v2
- Azure Otomasyonu
- Azure DNS
- Azure Key Vault
- Azure Load Balancer
- Azure İzleyici
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure SQL Database
- Azure Storage
- Azure İzleyici günlükleri
- Azure Sanal Ağı
    - (1)/16 ağ
    - (4)/24 ağ
    - Ağ güvenlik grupları
- Ağ güvenlik grupları
- Kurtarma Hizmetleri Kasası
- Azure Web Uygulaması

Bu Blueprint, Avustralya Cyber Güvenlik Merkezi (ACSC) tarafından korunan sınıflandırmada kullanılmak üzere sertifikalı Azure hizmetlerini içerir. Microsoft, müşterilerin bu Azure hizmetleriyle ilgili yayımlanan güvenlik ve Denetim raporlarını gözden geçirmesini ve risk yönetimi çatılarını kullanarak Azure hizmetinin kendi iç actatsyon için uygun olup olmadığını ve bu noktada kullanılması gerektiğini öneriyor Korunan sınıflandırma.

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğelerinin ayrıntıları verilmiştir.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) , müşterilerin çözümdeki kaynaklarla bir grup olarak çalışmasını sağlar. Müşteriler çözüme yönelik tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilir. Müşteriler dağıtım için bir şablon kullanır ve bu şablon test, hazırlık ve üretim gibi farklı ortamlarda çalışabilir. Kaynak Yöneticisi, müşterilerin kaynaklarını dağıtımdan sonra yönetmesine yardımcı olmak için güvenlik, denetleme ve etiketleme özellikleri sağlar.

Savunma **ana bilgisayarı**: Savunma ana bilgisayarı, kullanıcıların bu ortamdaki dağıtılan kaynaklara erişmesine olanak tanıyan tek giriş noktasıdır. Savunma ana bilgisayarı, yalnızca güvenli bir listedeki genel IP adreslerinden gelen uzak trafiğe izin vererek, dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için, trafiğin kaynağının ağ güvenlik grubunda tanımlanması gerekir.

Bu çözüm, aşağıdaki yapılandırmalara sahip etki alanına katılmış bir savunma ana bilgisayarı olarak bir sanal makine oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/fundamentals/antimalware)
-   [Azure Tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   Azure Key Vault kullanarak [Azure disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption)
-   Kullanımda olmadığında sanal makine kaynaklarının tüketimini azaltmak için [otomatik olarak kapatılmış ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/)

**App Service ortamı v2**: [Azure App Service ortamı](https://docs.microsoft.com/azure/app-service/environment/intro) , yüksek ölçekte App Service uygulamaları güvenli bir şekilde çalıştırmak için tamamen yalıtılmış ve ayrılmış bir ortam sağlayan bir App Service özelliğidir.

App Service ortamlar yalnızca tek bir müşterinin uygulamalarını çalıştırmak için yalıtılmıştır ve her zaman bir sanal ağa dağıtılır. Müşteriler hem gelen hem de giden uygulama ağ trafiği üzerinde ayrıntılı denetime sahiptir ve uygulamalar, sanal ağlar üzerinde şirket içi kurumsal kaynaklara yüksek hızlı güvenli bağlantılar kurabilir.

Bu mimari için App Service ortamların kullanımı, aşağıdaki denetimlere/yapılandırmalara izin verir:

- App Service ortamların yalıtılmış hizmet planını kullanacak şekilde yapılandırılması gerekir
    - Farklı sınıflandırmalardaki uygulamalar için farklı App Service ortamları yapılandırma
- Güvenli bir Azure sanal ağı ve ağ güvenlik kuralları içinde barındırma
- HTTPS iletişimi için otomatik olarak imzalanan bir iç yük dengeleyici sertifikasıyla yapılandırılmış App Service ortamları. En iyi uygulama olarak, Microsoft, gelişmiş güvenlik için güvenilir bir sertifika yetkilisinin kullanılmasını önerir.
- [İç Yük Dengeleme modu](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer) (mod 3)
- [TLS v 1.0 ve v 1.1 'yi](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings) devre dışı bırak
- [TLS şifre](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings) değiştirme
- [Gelen trafiği denetle N/W bağlantı noktaları](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Web uygulaması güvenlik duvarı – verileri kısıtla](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- [Azure SQL veritabanı trafiğine](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview) izin ver

**Azure Web uygulaması**: [Azure App Service](https://docs.microsoft.com/azure/app-service/) , müşterilerin altyapıyı yönetmeksizin seçtikleri programlama dilinde Web uygulamaları oluşturup barındırmalarını sağlar. Otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar, hem Windows hem de Linux’ı destekler ve GitHub, Azure DevOps Services veya herhangi bir Git deposundan otomatik dağıtımlar sağlar.

### <a name="virtual-network"></a>Sanal Ağ
Mimari, adres alanı 10.200.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) , bir sanal ağ içindeki trafiğe izin veren veya bu trafiği reddeden erişim denetimi listelerini içerir. Ağ güvenlik grupları, trafiği bir alt ağda veya tek bir sanal makine düzeyinde güvenli hale getirmek için kullanılabilir. Aşağıdaki ağ güvenlik grupları var:
- Application Gateway için 1 ağ güvenlik grubu
- App Service Ortamı için 1 ağ güvenlik grubu
- Azure SQL veritabanı için 1 ağ güvenlik grubu
- Savunma ana bilgisayarı için 1 ağ güvenlik grubu

Ağ güvenlik gruplarının her biri, çözümün güvenli ve doğru bir şekilde çalışabilmesi için belirli bağlantı noktaları ve protokoller açar. Ayrıca, her bir ağ güvenlik grubu için aşağıdaki yapılandırma etkinleştirilir:

  - [Tanılama günlükleri ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) , bir depolama hesabında etkinleştirilir ve depolanır
  - Azure Izleyici günlükleri [ağ güvenlik grubunun](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json) tanılamalarına bağlı

**Alt ağlar**: Her alt ağ, karşılık gelen ağ güvenlik grubuyla ilişkilendirilir.

**Azure DNS**: Etki alanı adı sistemi veya DNS, IP adresine bir Web sitesi veya hizmet adı çevirmekten (veya çözümlemeden) sorumludur. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) , Azure altyapısını kullanarak ad ÇÖZÜMLEMESI sağlayan DNS etki alanları için bir barındırma hizmetidir. Kullanıcılar, Azure 'da etki alanlarını barındırarak DNS kayıtlarını, diğer Azure hizmetleriyle aynı kimlik bilgilerini, API 'Leri, araçları ve faturalandırmayı kullanarak yönetebilir. Ayrıca, özel DNS etki alanlarını da destekler Azure DNS.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) , müşterilerin uygulamalarını ölçeklendirmesine ve hizmetler için yüksek kullanılabilirlik oluşturmalarına olanak tanır. Load Balancer, gelen ve giden senaryoları destekler ve düşük gecikme süresi, yüksek aktarım hızı ve tüm TCP ve UDP uygulamaları için milyonlarca akışa kadar ölçeklendirme yapar.

### <a name="data-in-transit"></a>Aktarım durumundaki veriler
Azure, Azure veri merkezlerinden gelen ve varsayılan olarak tüm iletişimleri şifreler. 

Müşterinin sahip olduğu ağlardan alınan korumalı veriler için, mimaride Azure, Internet veya ExpressRoute 'u ıPSEC ile yapılandırılmış bir VPN Gateway kullanır.

Ayrıca, Azure yönetim portalı üzerinden Azure 'a yönelik tüm işlemler, TLS v 1.2 kullanan HTTPS aracılığıyla gerçekleştirilir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, verileri şifreleme, veritabanı denetimi ve diğer ölçülerle korur.

**Azure depolama**: Rest gereksinimlerinde şifrelenmiş verileri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption)kullanır. Bu, Avustralya kamu ıSM tarafından tanımlanan kurumsal güvenlik taahhütleri ve uyumluluk gereksinimleri desteğiyle verilerin korunmasını ve korunmasını sağlar.

**Azure disk şifrelemesi**: [Azure disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) , veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğinden yararlanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Azure Key Vault ile tümleşir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-AAD-authentication) , veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin tek bir merkezi konumda kimlik yönetimine izin verebilir.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) , veritabanı olaylarını izler ve bunları Azure Storage hesabındaki bir denetim günlüğüne yazar.
-   Azure SQL veritabanı, bekleyen bilgileri korumak üzere veritabanı, ilişkili yedeklemeler ve işlem günlüğü dosyalarını gerçek zamanlı şifrelemeyi ve şifre çözmeyi gerçekleştiren [Saydam veri şifrelemesini](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)kullanacak şekilde yapılandırılmıştır. Saydam veri şifrelemesi, depolanan verilerin yetkisiz erişime maruz olmadığından emin olmak için güvence sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) , şüpheli veritabanı etkinlikleri, olası güvenlik AÇıKLARı, SQL ekleme saldırıları ve anormal veritabanı erişim desenleri için güvenlik uyarıları sunarak meydana gelebilecek olası tehditlere yönelik algılama ve yanıt verir. SQL tehdit algılama uyarıları [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ile tümleştirir, bu da şüpheli etkinliğin ayrıntılarını ve tehdidi araştırmak ve hafifletmek için önerilen eylemi içerir.
-   [Always Encrypted sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) , gizli verilerin veritabanı sisteminde hiçbir şekilde düz metin olarak göründüğünden emin olun. Veri şifrelemeyi etkinleştirdikten sonra, yalnızca anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) , verileri ayrıcalıklı olmayan kullanıcılara veya uygulamalara maskeleyerek hassas veri pozlamasını sınırlar. Dinamik veri maskeleme, potansiyel olarak duyarlı verileri otomatik olarak bulabilir ve uygun maskeleri uygulanmasını önerebilir. Bu, gizli verilerin yetkisiz erişim aracılığıyla veritabanından çıkmayacağı gibi erişimi azaltmaya yardımcı olur. Müşterilerin, veritabanı şemasına uyacak şekilde dinamik veri maskeleme ayarlarını ayarlaması gerekir.

### <a name="identity-management"></a>Kimlik yönetimi
Müşteriler, Microsoft 'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmeti olan [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)federasyona eklemek için şirket Içi Active Directory federe hizmetleri kullanabilir. [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) , şirket içi dizinleri Azure Active Directory ile tümleştirir. Bu Çözümdeki tüm kullanıcılar Azure SQL veritabanına erişen kullanıcılar dahil olmak üzere Azure Active Directory hesapları gerektirir. Federasyon oturum açma ile, kullanıcılar Azure Active Directory oturum açabilirler ve şirket içi kimlik bilgilerini kullanarak Azure kaynaklarında kimlik doğrulaması yapabilir.

Ayrıca, aşağıdaki Azure Active Directory özellikleri Azure ortamındaki verilere erişimi yönetmeye yardımcı olur:
- Uygulamanın kimlik doğrulaması, Azure Active Directory kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [uygulamaları Azure Active Directory tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, veritabanı sütun şifrelemesi uygulamanın kimliğini Azure SQL veritabanı 'nda doğrulamak için Azure Active Directory kullanır. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'nda hassas verileri koruma](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- [Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) , yöneticilerin yalnızca kullanıcıların işlerini gerçekleştirmesi için ihtiyaç duyduğu erişim miktarını vermek üzere hassas erişim izinleri tanımlamasına olanak sağlar. Yöneticiler, her kullanıcıya Azure kaynakları için Kısıtlanmamış izin vermek yerine, verilere erişim için yalnızca belirli eylemlere izin verebilir. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) , müşterilerin belirli bilgilere erişimi olan kullanıcı sayısını en aza indirmesine olanak sağlar. Yöneticiler, ayrıcalıklı kimlikleri ve kaynaklara erişimlerini bulma, kısıtlama ve izleme işlemleri için Azure Active Directory Privileged Identity Management kullanabilir. Bu işlev, gerektiğinde isteğe bağlı, tam zamanında yönetim erişimi sağlamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) , kuruluş kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleriyle ilişkili şüpheli eylemleri algılanan otomatik yanıtları yapılandırır ve araştırır şüpheli olaylar, bunları çözümlemek için uygun eylemi gerçekleştirebilir.

**Azure Multi-Factor Authentication**: Kimlikleri korumak için çok faktörlü kimlik doğrulamasının uygulanması gerekir. [Azure Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) , kullanıcıları korumak için ikinci bir kimlik doğrulama yöntemi sağlayan kullanımı kolay, ölçeklenebilir ve güvenilir bir çözümdür. Azure Multi-Factor Authentication, bulutun gücünü kullanır ve şirket içi Active Directory ve özel uygulamalarla tümleşir. Bu koruma, yüksek hacimli, görev açısından kritik senaryolara genişletilir.

### <a name="security"></a>Güvenlik
**Gizli dizi yönetimi**: Çözüm, anahtarların ve parolaların yönetimi için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) kullanır. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşterilerin verileri korumalarına yardımcı olur:
- Gelişmiş erişim ilkeleri, gereksinim temelinde yapılandırılır.
- Key Vault erişim ilkeleri anahtarlar ve gizlilikler için gereken en düşük izinlerle tanımlanır.
- Key Vault tüm anahtarlar ve gizli dizileri için süre sonu tarihleri vardır.
- Key Vault tüm anahtarlar özel donanım güvenliği modülleri tarafından korunur. Anahtar türü, korunan bir donanım güvenlik modülü 2048 bit RSA anahtarıdır.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanılarak gerekli olan en düşük izinlere sahiptir.
- Key Vault için tanılama günlükleri, en az 365 günlük bir bekletme dönemi ile etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri, gerekli olanlarla kısıtlıdır.

**Azure Güvenlik Merkezi**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)ile, müşteriler iş yükleri genelinde güvenlik ilkelerini merkezi olarak uygulayabilir ve yönetebilir, tehditlere maruz kalma olasılığını sınırlayabilir, saldırıları algılayıp yanıtlayabilir. Ayrıca, Azure Güvenlik Merkezi, güvenlik duruşunu artırmaya ve verileri korumaya yardımcı olmak için yapılandırma ve hizmet önerileri sağlamak üzere Azure hizmetleri 'nin mevcut yapılandırmalarına erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen olası saldırılardan müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi, bir tehdit ya da şüpheli etkinlik gerçekleştiğinde tetiklenen bir dizi [önceden tanımlanmış güvenlik uyarısı](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)içerir. Azure Güvenlik Merkezi 'ndeki [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) , müşterilerin ortamlarından zaten toplanmış olan verileri temel alarak yeni güvenlik uyarıları tanımlamasına olanak tanır.

Azure Güvenlik Merkezi, öncelik veren güvenlik uyarıları ve olayları sunarak müşterilerin olası güvenlik sorunlarını bulmasını ve adreslerini daha kolay hale getirmesini sağlar. Algılanan her tehdit için [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) , tehditleri İnceleme ve düzeltme konusunda olay yanıtı ekiplerine yardımcı olacak şekilde oluşturulmuştur.

**Application Gateway**: Mimari, Web uygulaması güvenlik duvarı ile bir Application Gateway kullanan güvenlik açıklarına karşı risk düzeyini azaltır ve OWASP kural kümesi etkindir. Ek yetenekler şunlardır:

- [Uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [SSL yük boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal) 'yı etkinleştir
- [TLS v 1.0 ve v 1.1 'yi](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell) devre dışı bırak
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (önleme modu)
- OWASP 3,0 RuleSet ile [önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)
- [Tanılama günlüğünü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics) etkinleştirme
- [Özel durum araştırmaları](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimler sağlar. Azure Güvenlik Merkezi ayrıca bir saygınlık sistem sağlar.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

Azure Hizmetleri, sistem durumunun yanı sıra sistem durumu ve Kullanıcı etkinliklerini de yoğun olarak günlüğe kaydeder:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşma süresinin ve durumunun belirlenmesine yardımcı olabilir.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından yayılan tüm günlükleri içerir. Bu Günlükler Windows olay sistemi günlükleri, Azure depolama günlükleri, Key Vault denetim günlükleri ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri, arşivleme için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Bekletme, kuruluşa özgü saklama gereksinimlerini karşılamak için Kullanıcı tarafından yapılandırılabilir ve 730 güne kadar.

**Azure izleyici günlükleri**: Bu Günlükler, işleme, depolama ve Pano raporlama için [Azure izleyici günlüklerinde](https://azure.microsoft.com/services/log-analytics/) birleştirilir. Toplandığında, veriler her bir veri türü için ayrı tablolar halinde düzenlenir ve böylece özgün kaynağına bakılmaksızın tüm verilerin birlikte analiz edilmesi sağlanır. Ayrıca, Azure Güvenlik Merkezi, müşterilerin güvenlik olay verilerine erişmek ve diğer hizmetlerdeki verilerle birleştirmek için kusto sorguları kullanmasına izin veren Azure Izleyici günlükleri ile tümleşir.

Aşağıdaki Azure [izleme çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) , bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü, düzenli bir aralıkta sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [Aracı durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve coğrafi dağıtımını, yanıt vermeyen aracı sayısını ve işletimsel verileri gönderen aracıların sayısını bildirir.
-   [Etkinlik günlüğü Analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.

**Azure Otomasyonu**: [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook 'ları depolar, çalıştırır ve yönetir. Bu çözümde, runbook 'lar Azure SQL veritabanından günlükleri toplamaya yardımcı olur. Otomasyon [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) çözümü, müşterilerin ortamdaki değişiklikleri kolayca belirlemesine olanak sağlar.

**Azure izleyici**: [Azure izleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) , kuruluşların Azure kaynaklarında API çağrılarını izleme de dahil olmak üzere performansı izlemelerine, uyarıları oluşturmalarına ve verileri arşivlemesini sağlayarak eğilimleri belirlemesine yardımcı olur.

Azure ağ Izleyicisi: [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) , bir Azure sanal ağındaki kaynakların günlüklerini izlemek, tanılamak, görüntülemek ve etkinleştirmek ya da devre dışı bırakmak için araçlar sağlar.  Uluslar için varlıkların, NSG 'ler ve sanal makineler için ağ Izleyicisi akış günlüklerini uygulaması gerekir. Bu günlüklerin, yalnızca güvenlik günlüklerinin depolandığı ve depolama hesabına erişimin, rol tabanlı erişim denetimleriyle güvenli hale getirilmesi gereken ayrılmış bir depolama hesabında depolanması gerekir.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı [indirilebilir](https://aka.ms/au-protected-paaswa-tm) veya aşağıda bulunabilir. Bu model, müşterilerin değişiklik yaparken sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![Au korumalı tehdit modeli Için PaaS Web uygulaması](images/au-protected-paaswa-threat-model.png?raw=true "Au korumalı tehdit modeli diyagramı Için PaaS Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
Bu uyumluluk belgeleri, Microsoft tarafından sunulan platformlar ve hizmetler temelinde Microsoft tarafından oluşturulur. Bu belgede, çok çeşitli müşteri dağıtımları nedeniyle, yalnızca Azure ortamında barındırılan bir çözüme yönelik genelleştirilmiş bir yaklaşım sunulmaktadır. Müşteriler, kendi işletim ortamları ve iş sonuçları temelinde alternatif ürün ve Hizmetleri tanımlayabilir ve kullanabilir. Şirket içi kaynakları kullanmayı tercih eden müşteriler, bu şirket içi kaynaklara yönelik güvenlik ve işlemleri ele almalıdır. Belgelenmiş çözüm, müşteriler tarafından belirli şirket içi ve güvenlik gereksinimlerini karşılamak üzere özelleştirilebilir.

[Azure Güvenlik ve uyumluluk şeması-au korumalı müşteri sorumluluğu MATRISI](https://aka.ms/au-protected-crm) au-Prot için gereken tüm güvenlik denetimlerini listeler. Bu matris, her denetim uygulamasının Microsoft 'un, müşterinin sorumluluğunun veya ikisi arasında paylaşılıp 'nin sorumluluğundadır.

[Azure Güvenlik ve uyumluluk şeması-au korumalı PaaS Web uygulaması uygulama matrisi](https://aka.ms/au-protected-paaswa-cim) , ' nin ayrıntılı açıklamaları dahil olmak üzere PaaS Web uygulaması mimarisi tarafından HANGI au korumalı denetimlerin giderildiği hakkında bilgi sağlar uygulamanın, kapsanan her denetimin gereksinimlerini nasıl karşıladığı.

## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute

Sınıflandırılmış bilgiler için güvenli bir IPSec VPN tünelinin, bu IaaS Web uygulaması başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kurması için yapılandırılması gerekir. Bir IPSec VPN 'yi uygun şekilde ayarlayarak, müşteriler aktarım sırasında veriler için bir koruma katmanı ekleyebilir.

Azure ile güvenli bir IPSec VPN tüneli uygulayarak, şirket içi ağ ve Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı Internet üzerinden yapılabilir ve müşterilerin, müşterinin ağı ile Azure arasındaki şifrelenmiş bir bağlantı içindeki bilgileri güvenli bir şekilde "tünelleye" sağlar. Siteden siteye VPN, delikler için tüm boyutlardaki kuruluşlar tarafından dağıtılan güvenli, Olgun bir teknolojidir. 

VPN tünelinin içindeki trafik, siteden siteye VPN ile Internet 'e geçiş yaptığından, Microsoft özel bir bağlantı seçeneği sunar. Azure ExpressRoute, Azure ile şirket içi bir konum veya bir Exchange barındırma sağlayıcısı arasında ayrılmış bir bağlantıdır ve özel ağ olarak kabul edilir. ExpressRoute bağlantıları Internet üzerinden gitmediğinden, bu bağlantılar Internet üzerinden tipik bağlantılardan daha fazla güvenilirlik, daha hızlı ve daha düşük gecikme süreleri sunar. Ayrıca, bu müşterinin iletişim sağlayıcısının doğrudan bir bağlantısı olduğundan, veriler Internet üzerinden hareket etmez ve bu nedenle bu açık değildir.

Şirket içi bir ağı Azure 'a genişleten güvenli bir karma ağ uygulamak için en iyi yöntemler [mevcuttur](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid). 

Internet veya Azure ExpressRoute 'u kullanarak sınıflandırılan verilerin korunmasına yardımcı olmak için, müşterilerin aşağıdaki ayarları uygulayarak IPSec VPN 'sini yapılandırması gerekir:

• Müşteri VPN başlatıcısı, 14400 saniyeden daha büyük olmayan bir SA yaşam süresi için yapılandırılmış olmalıdır.
• Müşteri VPN başlatıcısı, IKEv1 agresif modunu devre dışı bırakmalıdır.
• Müşteri VPN başlatıcısının kusursuz Iletme gizliliği yapılandırması gerekir.
• Müşteri VPN başlatıcısı HMAC-SHA256 veya daha büyük kullanımını yapılandırmalıdır.

VPN cihazları ve IPSec/ıKE parametrelerinin yapılandırma seçenekleri gözden geçirilmek üzere [kullanılabilir](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices) .

### <a name="azure-active-directory-setup"></a>Azure Active Directory kurulum
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) , dağıtımı yönetmek ve ortamla etkileşime geçen personele erişimi sağlamak için gereklidir. Mevcut bir Windows Server Active Directory, [dört tıklamayla](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express)Azure Active Directory tümleştirilebilir.

Ayrıca, [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) müşterilerin Federasyon 'yi şirket içi [Active Directory Federasyon Hizmetleri (AD FS)]( https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-azure-adfs) ve Azure Active Directory yapılandırmasına izin verir. Federasyon oturum açma ile müşteriler, kullanıcıların şirket ağı sırasında parolalarını yeniden girmesi gerekmeden Azure Active Directory tabanlı hizmetlerde şirket içi parolalarla oturum açmasını sağlayabilir. Active Directory Federasyon Hizmetleri (AD FS) ile Federasyon seçeneğini kullanarak, yeni bir Active Directory Federasyon Hizmetleri (AD FS) yüklemesi dağıtabilir veya var olan bir yüklemeyi Windows Server 2012 R2 grubunda belirtebilirsiniz.

Sınıflandırılan verilerin Azure Active Directory karşı eşitlenmesini engellemek için, müşteriler Azure Active Directory Connect aşağıdaki ayarları uygulayarak Azure Active Directory çoğaltılan öznitelikleri kısıtlayabilir:

- [Filtrelemeyi etkinleştir](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering)
- [Parola karması eşitlemesini devre dışı bırak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)
-   [Parola geri yazmayı devre dışı bırak](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)
-   [Cihaz geri yazmayı devre dışı bırak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-feature-device-writeback)
-   [Yanlışlıkla silmeleri](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-prevent-accidental-deletes) ve [Otomatik yükseltmeyi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-feature-automatic-upgrade) engellemek için varsayılan ayarları bırakın


## <a name="disclaimer"></a>Sorumluluk Reddi

 - Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
 - Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
 - Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
 - Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
 - Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
