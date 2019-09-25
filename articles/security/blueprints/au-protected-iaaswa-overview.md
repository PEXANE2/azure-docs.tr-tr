---
title: Avustralya için Azure Güvenlik ve Uyumluluk Şeması-IaaS Web uygulaması
description: Avustralya için Azure Güvenlik ve Uyumluluk Şeması-IaaS Web uygulaması
services: security
author: meladie
ms.assetid: f53a25c4-1c75-42d6-a0e7-a91661673891
ms.service: security
ms.topic: article
ms.date: 08/23/2018
ms.author: meladie
ms.openlocfilehash: 5fe9cfe642585c4f5220d79813816e554fdf48ef
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71259193"
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-australia-protected"></a>Avustralya için Azure Güvenlik ve Uyumluluk Şeması-IaaS Web uygulaması

## <a name="overview"></a>Genel Bakış
Bu Azure Güvenlik ve Uyumluluk Şeması, bir hizmet olarak altyapı (IaaS) ortamının, ile uyumlu AU KORUMALı kamu verilerinin toplanmasını, depolanmasını ve alınmasını uygun şekilde dağıtımına yönelik rehberlik sağlar. Avustralya sinyalleri (ASD) tarafından üretilen Avustralya kamu bilgi güvenliği el kitabı (ıSM). Bu şema, ortak bir başvuru mimarisini gösterir ve hassas kamu verilerinin güvenli, uyumlu, çok katmanlı bir ortamda doğru işlenmesini göstermeye yardımcı olur.

Bu başvuru mimarisi, uygulama kılavuzu ve tehdit modeli, müşterilerin kendi planlama ve sistem actatsyon süreçlerini benimsemesi için bir temel sağlar ve müşterilerin iş yüklerini ASD ile uyumlu bir şekilde Azure 'a dağıtmalarına yardımcı olur. Müşteriler, Federasyon hizmetlerini kullanmak ve şirket içi kaynakları Azure kaynaklarıyla bütünleştirmek için bir Azure VPN Gateway veya ExpressRoute uygulamayı tercih edebilir. Müşteriler, şirket içi kaynakları kullanmanın güvenlik etkilerine yönelik etkileri göz önünde bulundurmalıdır. Her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebileceğinden, tüm gereksinimleri karşılamak için ek yapılandırma gerekir.

ASD 'nin uyumlu olması için bir bilgi güvenliği kayıtlı denetçisi, sistemi denetler. Müşteriler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebileceğinden, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmelerinden sorumludur.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm bir IaaS Web uygulaması için SQL Server arka uca bir başvuru mimarisi dağıtır. Mimari bir Web katmanı, veri katmanı, Active Directory altyapı, Application Gateway ve Load Balancer içerir. Web ve veri katmanlarına dağıtılan sanal makineler, bir kullanılabilirlik kümesinde yapılandırılır ve SQL Server örnekler, yüksek kullanılabilirlik için her zaman açık kullanılabilirlik grubunda yapılandırılır. Sanal makineler etki alanına katılmış ve Active Directory Grup ilkeleri, işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmalarının uygulanması için kullanılır. Yönetim savunma ana bilgisayarı, yöneticilerin dağıtılan kaynaklara erişmesi için güvenli bir bağlantı sağlar.

Mimari, şirket içi ağı Azure 'a genişleten güvenli bir karma ortam sunarak, Web tabanlı iş yüklerinin bir kuruluşun özel yerel ağı veya internet 'ten Şirket kullanıcıları tarafından güvenli bir şekilde erişilmesini sağlar. Şirket içi çözümler için, müşteri hem sorumlu hem de tüm güvenlik, işlem ve uyumluluk yönünden sorumludur.

Bu çözüme dahil edilen Azure kaynakları, Azure VPN Gateway veya ExpressRoute aracılığıyla IPSec VPN aracılığıyla şirket içi ağa veya veri merkezi birlikte bulundurma tesisine (örneğin, Canbera) bağlanabilir. Bir VPN kullanma kararı, iletilen verilerin sınıflandırmasıyla ve ağ yolunun göz önünde bulundurularak yapılmalıdır. Büyük veri gereksinimleriyle büyük ölçekli, görev açısından kritik iş yüklerini çalıştıran müşteriler, Azure hizmetlerine özel ağ bağlantısı için ExpressRoute kullanan bir karma ağ mimarisini düşünmelidir. Azure 'a yönelik bağlantı mekanizmaları hakkında daha fazla bilgi için rehberlik ve öneriler bölümüne bakın.

Azure Active Directory Federasyon, kullanıcıların şirket içi Active Directory Federasyon Hizmetleri (AD FS) altyapısını kullanarak kimlik doğrulaması yapmasını ve buluttaki tüm kaynaklara erişmesini sağlamak için kullanılmalıdır. Active Directory Federasyon Hizmetleri (AD FS), bu karma ortam için Basitleştirilmiş, güvenli kimlik Federasyonu ve Web 'de çoklu oturum açma özellikleri sağlayabilir. Kurulum Azure Active Directory daha fazla ayrıntı için rehberlik ve öneriler bölümüne bakın.

Çözüm, müşterilerin bekleyen verilerin gizliliğini korumak için Depolama Hizmeti Şifrelemesi kullanmak üzere yapılandırabileceği Azure depolama hesaplarını kullanır. Azure, esneklik için müşterinin seçili bölgesi içinde verilerin üç kopyasını depolar. Azure bölgeleri dayanıklı bölge çiftlerine dağıtılır ve coğrafi olarak yedekli depolama, verilerin üç kopya ile ikinci bölgeye çoğaltılmasını sağlar. Bu, müşterinin birincil veri konumundaki olumsuz bir olayı, veri kaybına neden olur.

Gelişmiş güvenlik için, bu Çözümdeki tüm kaynaklar Azure Resource Manager aracılığıyla bir kaynak grubu olarak yönetilir. Azure Active Directory rol tabanlı erişim denetimi, Azure Key Vault dağıtılan kaynak ve anahtarlara erişimi denetlemek için kullanılır. Sistem durumu, Azure Güvenlik Merkezi ve Azure Izleyici aracılığıyla izlenir. Müşteriler günlükleri yakalamak ve sistem durumunu tek ve kolayca gezinebilir bir panoda göstermek için her iki izleme hizmetini de yapılandırır. Azure Application Gateway, önleme modunda bir güvenlik duvarı olarak yapılandırılmıştır ve en düşük 1,2 sürümüne sahip TLS trafiği gerektirir.

![Au korumalı başvuru mimarisi Için IaaS Web uygulaması](images/au-protected-iaaswa-architecture.png?raw=true "Au korumalı başvuru mimarisi diyagramı Için IaaS Web uygulaması") 

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Daha fazla ayrıntı, [dağıtım mimarisi](#deployment-architecture) bölümünde bulunur.

- Kullanılabilirlik Kümeleri
    - (1) SQL kümesi düğümleri
    - (1) Web/IIS
- Azure Active Directory
- Azure Application Gateway
    - (1) Web uygulaması güvenlik duvarı
        - Güvenlik duvarı modu: önleme
        - Kural kümesi: OWASP 3,0
        - Dinleyici bağlantı noktası: 443
- Azure bulut tanığı
- Azure Key Vault
- Azure Load Balancer
- Azure İzleyici
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure İzleyici günlükleri
- Azure Storage
- Azure sanal makineleri
    - (1) yönetim/savunma (Windows Server 2016 Datacenter)
    - (2) küme düğümünü SQL Server (Windows Server 2016 üzerinde SQL Server 2017)
    - (2) Web/IIS (Windows Server 2016 Datacenter)
- Azure Sanal Ağı
    - (4) ağ güvenlik grupları
    - Azure ağ Izleyicisi
- Kurtarma Hizmetleri Kasası

Bu Blueprint, Avustralya Cyber Güvenlik Merkezi (ACSC) tarafından korunan sınıflandırmada kullanılmak üzere sertifikalı Azure hizmetlerini içerir. Bu başvuru mimarisine dahil olan tüm hizmetler, ACSC tarafından, ayırt alma sınırlandırma Işaretçileri (DLD) düzeyinde sertifikalandırilmiştir. Microsoft, müşterilerin bu Azure hizmetleriyle ilgili yayımlanan güvenlik ve Denetim raporlarını gözden geçirmesini ve risk yönetimi çatılarını kullanarak Azure hizmetinin kendi iç actatsyon için uygun olup olmadığını ve bu noktada kullanılması gerektiğini öneriyor Korunan sınıflandırma.

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğelerinin ayrıntıları verilmiştir.

Savunma **ana bilgisayarı**: Savunma ana bilgisayarı, kullanıcıların bu ortamdaki dağıtılan kaynaklara erişmesine olanak tanıyan tek giriş noktasıdır. Savunma ana bilgisayarı, yalnızca güvenli bir listedeki genel IP adreslerinden gelen uzak trafiğe izin vererek, dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için, trafiğin kaynağının ağ güvenlik grubunda tanımlanması gerekir.

Bu çözüm, aşağıdaki yapılandırmalara sahip etki alanına katılmış bir savunma ana bilgisayarı olarak bir sanal makine oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/fundamentals/antimalware)
-   [Azure Tanılama uzantısı](../../virtual-machines/windows/extensions-diagnostics-template.md)
-   Azure Key Vault kullanarak [Azure disk şifrelemesi](../azure-security-disk-encryption-overview.md)
-   Kullanımda olmadığında sanal makine kaynaklarının tüketimini azaltmak için [otomatik olarak kapatılmış ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/)

### <a name="virtual-network"></a>Sanal ağ
Mimari, adres alanı 10.200.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: Bu çözüm, bir sanal ağ içindeki farklı bir Web alt ağı, veritabanı alt ağı, Active Directory alt ağı ve yönetim alt ağı olan bir mimariye kaynak dağıtır. Alt ağlar, alt ağlar arasındaki trafiği yalnızca sistem ve yönetim işlevselliği için gerekli olan ağlara kısıtlamak üzere ayrı alt ağlara uygulanan ağ güvenlik grubu kuralları tarafından mantıksal olarak ayrılır.

Bu çözümle dağıtılan [ağ güvenlik grupları](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) için yapılandırma konusuna bakın. Kuruluşlar, [Bu belgeleri](../../virtual-network/virtual-network-vnet-plan-design-arm.md) kılavuz olarak kullanarak yukarıdaki dosyayı düzenleyerek ağ güvenlik gruplarını yapılandırabilir.

Alt ağların her birinde ayrılmış bir ağ güvenlik grubu vardır:
- Application Gateway için 1 ağ güvenlik grubu (LBNSG)
- Savunma ana bilgisayarı için 1 ağ güvenlik grubu (MGTNSG)
- 1 SQL sunucuları ve bulut tanığı için ağ güvenlik grubu (SQLNSG)
- Web katmanı için 1 ağ güvenlik grubu (WEBNSG)

### <a name="data-in-transit"></a>Aktarım durumundaki veriler
Azure, Azure veri merkezlerinden gelen ve varsayılan olarak tüm iletişimleri şifreler. 

Müşterinin sahip olduğu ağlardan alınan korumalı veriler için, mimaride Internet veya ExpressRoute 'u ıPSEC ile yapılandırılmış bir VPN Gateway kullanır.

Ayrıca, Azure yönetim portalı üzerinden Azure 'a yönelik tüm işlemler, TLS 1,2 kullanan HTTPS aracılığıyla gerçekleştirilir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, verileri şifreleme, veritabanı denetimi ve diğer ölçülerle korur.

**Azure depolama**: Rest gereksinimlerinde şifrelenmiş verileri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) [depolama hizmeti şifrelemesi](../../storage/common/storage-service-encryption.md)kullanır. Bu, Avustralya kamu ıSM tarafından tanımlanan kurumsal güvenlik taahhütleri ve uyumluluk gereksinimleri desteğiyle verilerin korunmasını ve korunmasını sağlar.

**Azure disk şifrelemesi**: [Azure disk şifrelemesi](../azure-security-disk-encryption-overview.md) , veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğinden yararlanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Azure Key Vault ile tümleşir.

**SQL Server**: SQL Server örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [SQL Server denetim](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine?view=sql-server-2017) , veritabanı olaylarını izler ve onları denetim günlüklerine yazar.
-   [Saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) , bekleyen bilgileri korumak için veritabanı, ilişkili yedeklemeler ve işlem günlüğü dosyalarını gerçek zamanlı şifreleme ve şifre çözme işlemleri gerçekleştirir. Saydam veri şifrelemesi, depolanan verilerin yetkisiz erişime maruz olmadığından emin olmak için güvence sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [Şifrelenmiş sütunlar](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-wizard?view=sql-server-2017) , gizli verilerin veritabanı sisteminde hiçbir şekilde düz metin olarak göründüğünden emin olmanızı sağlamaktır. Veri şifrelemeyi etkinleştirdikten sonra, yalnızca anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
- [Dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?view=sql-server-2017) , verileri ayrıcalıklı olmayan kullanıcılara veya uygulamalara maskeleyerek hassas veri pozlamasını sınırlar. Dinamik veri maskeleme, potansiyel olarak duyarlı verileri otomatik olarak bulabilir ve uygun maskeleri uygulanmasını önerebilir. Bu, gizli verilerin yetkisiz erişim aracılığıyla veritabanından çıkmayacağı gibi erişimi azaltmaya yardımcı olur. **Müşteriler, dinamik veri maskeleme ayarlarının veritabanı şemasına uyacak şekilde ayarlamaktan sorumludur.**

### <a name="identity-management"></a>Kimlik yönetimi
Müşteriler, Microsoft 'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmeti olan [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)federasyona eklemek için şirket Içi Active Directory federe hizmetleri kullanabilir. [Azure Active Directory Connect](../../active-directory/hybrid/whatis-hybrid-identity.md) , şirket içi dizinleri Azure Active Directory ile tümleştirir. Bu Çözümdeki tüm kullanıcılar Azure Active Directory hesapları gerektirir. Federasyon oturum açma ile, kullanıcılar Azure Active Directory oturum açabilirler ve şirket içi kimlik bilgilerini kullanarak Azure kaynaklarında kimlik doğrulaması yapabilir.

Ayrıca, aşağıdaki Azure Active Directory özellikleri Azure ortamındaki verilere erişimi yönetmeye yardımcı olur:
- Uygulamanın kimlik doğrulaması, Azure Active Directory kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [uygulamaları Azure Active Directory tümleştirme](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md).
- [Azure rol tabanlı erişim denetimi](../../role-based-access-control/role-assignments-portal.md) , yöneticilerin yalnızca kullanıcıların işlerini gerçekleştirmesi için ihtiyaç duyduğu erişim miktarını vermek üzere hassas erişim izinleri tanımlamasına olanak sağlar. Yöneticiler, her kullanıcıya Azure kaynakları için Kısıtlanmamış izin vermek yerine, verilere erişim için yalnızca belirli eylemlere izin verebilir. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır.
- [Azure Active Directory Privileged Identity Management](../../active-directory/privileged-identity-management/pim-getting-started.md) , müşterilerin belirli bilgilere erişimi olan kullanıcı sayısını en aza indirmesine olanak sağlar. Yöneticiler, ayrıcalıklı kimlikleri ve kaynaklara erişimlerini bulma, kısıtlama ve izleme işlemleri için Azure Active Directory Privileged Identity Management kullanabilir. Bu işlev, gerektiğinde isteğe bağlı, tam zamanında yönetim erişimi sağlamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](../../active-directory/identity-protection/overview.md) , kuruluş&#39;kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluş&#39;kimlikleriyle ilgili olarak algılanan şüpheli eylemlere yönelik otomatik yanıtları yapılandırır ve araştırır şüpheli olayları çözmek için uygun eylemi gerçekleştirin.

**Azure Multi-Factor Authentication**: Kimlikleri korumak için çok faktörlü kimlik doğrulamasının uygulanması gerekir. [Azure Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) , kullanıcıları korumak için ikinci bir kimlik doğrulama yöntemi sağlayan kullanımı kolay, ölçeklenebilir ve güvenilir bir çözümdür. Azure Multi-Factor Authentication, bulutun gücünü kullanır ve şirket içi Active Directory ve özel uygulamalarla tümleşir. Bu koruma, yüksek hacimli, görev açısından kritik senaryolara genişletilir.

### <a name="security"></a>Güvenlik
**Gizli dizi yönetimi**: Çözüm, anahtarların ve parolaların yönetimi için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) kullanır. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşterilerin bu verileri korumalarına ve bu verilere erişmesine yardımcı olur:

- Gelişmiş erişim ilkeleri, gereksinim temelinde yapılandırılır.
- Key Vault erişim ilkeleri anahtarlar ve gizlilikler için gereken en düşük izinlerle tanımlanır.
- Key Vault tüm anahtarlar ve gizli dizileri için süre sonu tarihleri vardır.
- Key Vault tüm anahtarlar özel donanım güvenliği modülleri tarafından korunur. Anahtar türü, korunan bir donanım güvenlik modülü 2048 bit RSA anahtarıdır.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanılarak gerekli olan en düşük izinlere sahiptir.
- Key Vault için tanılama günlükleri, en az 365 günlük bir bekletme dönemi ile etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri, gerekli olanlarla kısıtlıdır.
- Bu çözüm, IaaS sanal makine disk şifreleme anahtarlarını ve gizli dizileri yönetmek için Azure Key Vault ile tümleşiktir.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeler almak için varsayılan olarak yapılandırılır. Bu çözüm Ayrıca, gerektiğinde sanal makinelere yama yapmak üzere güncelleştirilmiş dağıtımların oluşturulabilmesi için [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) hizmetini de içerir.

**Kötü amaçlı yazılımdan koruma**: Sanal makineler için [Microsoft kötü amaçlı yazılımdan](https://docs.microsoft.com/azure/security/fundamentals/antimalware) koruma, bilinen kötü amaçlı veya istenmeyen yazılım denemesi sırasında yapılandırılabilir uyarılarla virüsler, casus yazılım ve diğer kötü amaçlı yazılımların tanımlanmasına ve kaldırılmasına yardımcı olan gerçek zamanlı koruma özelliği sağlar. korumalı sanal makinelerde install veya çalıştırın.

**Azure Güvenlik Merkezi**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)ile, müşteriler iş yükleri genelinde güvenlik ilkelerini merkezi olarak uygulayabilir ve yönetebilir, tehditlere maruz kalma olasılığını sınırlayabilir, saldırıları algılayıp yanıtlayabilir. Ayrıca, Azure Güvenlik Merkezi, güvenlik duruşunu artırmaya ve verileri korumaya yardımcı olmak için yapılandırma ve hizmet önerileri sağlamak üzere Azure hizmetleri 'nin mevcut yapılandırmalarına erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen olası saldırılardan müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi, bir tehdit ya da şüpheli etkinlik gerçekleştiğinde tetiklenen bir dizi [önceden tanımlanmış güvenlik uyarısı](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)içerir. Azure Güvenlik Merkezi 'ndeki [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) , müşterilerin ortamlarından zaten toplanmış olan verileri temel alarak yeni güvenlik uyarıları tanımlamasına olanak tanır.

Azure Güvenlik Merkezi, öncelik veren güvenlik uyarıları ve olayları sunarak müşterilerin olası güvenlik sorunlarını bulmasını ve adreslerini daha kolay hale getirmesini sağlar. Algılanan her tehdit için [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) , tehditleri İnceleme ve düzeltme konusunda olay yanıtı ekiplerine yardımcı olacak şekilde oluşturulmuştur.

Ayrıca, bu başvuru mimarisi Azure Güvenlik Merkezi 'ndeki [güvenlik açığı değerlendirmesini](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) kullanır. Bir iş ortağı Aracısı (ör. Qualys) yapılandırıldıktan sonra, güvenlik açığı verilerini ortağın yönetim platformuna bildirir. Buna karşılık, iş ortağının yönetim platformu Azure Güvenlik Merkezi 'ne geri güvenlik açığı ve sistem durumu izleme verileri sağlar ve bu sayede müşterilerin savunmasız sanal makineleri hızla belirlemesine izin verilir.

**Azure Application Gateway**: Mimari, bir Web uygulaması güvenlik duvarı yapılandırılmış bir Azure Application Gateway kullanan güvenlik açıklarına karşı risk düzeyini azaltır ve OWASP kural kümesi etkindir. Ek yetenekler şunlardır:

- [Uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [SSL yük boşaltma](../../application-gateway/create-ssl-portal.md) 'yı etkinleştir
- [TLS v 1.0 ve v 1.1 'yi](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell) devre dışı bırak
- [Web uygulaması güvenlik duvarı](../../application-gateway/waf-overview.md) (önleme modu)
- OWASP 3,0 RuleSet ile [önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)
- [Tanılama günlüğünü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics) etkinleştirme
- [Özel durum araştırmaları](../../application-gateway/quick-create-portal.md)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimler sağlar. Azure Güvenlik Merkezi ayrıca bir saygınlık sistem sağlar.

### <a name="business-continuity"></a>İş sürekliliği
**Yüksek kullanılabilirlik**: Çözüm, tüm sanal makineleri bir [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets)dağıtır. Kullanılabilirlik kümeleri, kullanılabilirliği artırmak için sanal makinelerin birden fazla yalıtılmış donanım kümesi arasında dağıtılmasını sağlar. Planlanmış veya plansız bir bakım olayı sırasında en az bir sanal makine kullanılabilir ve% 99,95 Azure SLA 'sını karşıladınız.

**Kurtarma Hizmetleri Kasası**: [Kurtarma Hizmetleri Kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) , yedekleme verilerini barındırır ve bu mimarideki tüm Azure sanal makineleri için tüm yapılandırmaların korunmasını sağlar. Bir kurtarma hizmetleri kasasıyla, müşteriler tüm sanal makineyi geri yüklemeden bir IaaS sanal makinesinden dosya ve klasörleri geri yükleyebilir, böylece daha hızlı geri yükleme sürelerini etkinleştirir.

**Bulut tanığı**: [Bulut tanığı](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) , Windows Server 2016 ' de yönetim noktası olarak Azure 'dan yararlanan bir yük devretme kümesi çekirdek tanığı türüdür. Diğer çekirdek tanığı gibi bulut tanığı bir oy alır ve çekirdek hesaplamalarına katılabilir, ancak standart genel kullanıma açık Azure Blob depolamayı kullanır. Bu, bir genel bulutta barındırılan sanal makinelerin ek bakım yükünü ortadan kaldırır.

### <a name="logging-and-auditing"></a>Günlük kaydı ve denetim
Azure Hizmetleri, sistem durumunun yanı sıra sistem durumu ve Kullanıcı etkinliklerini de yoğun olarak günlüğe kaydeder:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](../../azure-monitor/platform/activity-logs-overview.md) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşma süresinin ve durumunun belirlenmesine yardımcı olabilir.
- **Tanılama günlükleri**: [Tanılama günlükleri](../../azure-monitor/platform/resource-logs-overview.md) her kaynak tarafından yayılan tüm günlükleri içerir. Bu Günlükler Windows olay sistemi günlükleri, Azure depolama günlükleri, Key Vault denetim günlükleri ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri, arşivleme için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Bekletme, kuruluşa özgü saklama gereksinimlerini karşılamak için Kullanıcı tarafından yapılandırılabilir ve 730 güne kadar.

**Azure izleyici günlükleri**: Bu Günlükler, işleme, depolama ve Pano raporlama için [Azure izleyici günlüklerinde](https://azure.microsoft.com/services/log-analytics/) birleştirilir. Toplandığında, veriler her bir veri türü için ayrı tablolar halinde düzenlenir ve böylece özgün kaynağına bakılmaksızın tüm verilerin birlikte analiz edilmesi sağlanır. Ayrıca, Azure Güvenlik Merkezi, müşterilerin güvenlik olay verilerine erişmek ve diğer hizmetlerdeki verilerle birleştirmek için kusto sorguları kullanmasına izin veren Azure Izleyici günlükleri ile tümleşir.

Aşağıdaki Azure [izleme çözümleri](../../monitoring/monitoring-solutions.md) , bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Active Directory değerlendirmesi](../../azure-monitor/insights/ad-assessment.md): Active Directory sistem durumu denetimi çözümü, düzenli bir aralıkta sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](../../azure-monitor/insights/sql-assessment.md): SQL sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [Aracı durumu](../../monitoring/monitoring-solution-agenthealth.md): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve coğrafi dağıtımını, yanıt vermeyen aracı sayısını ve işletimsel verileri gönderen aracıların sayısını bildirir.
-   [Etkinlik günlüğü Analizi](../../azure-monitor/platform/collect-activity-logs.md): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.

**Azure Otomasyonu**: [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook 'ları depolar, çalıştırır ve yönetir. Bu çözümde runbook 'lar Azure SQL Server günlüklerin toplanmasını sağlamaya yardımcı olur. Otomasyon [değişiklik izleme](../../automation/change-tracking.md) çözümü, müşterilerin ortamdaki değişiklikleri kolayca belirlemesine olanak sağlar.

**Azure izleyici**: [Azure izleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) , kuruluşların Azure kaynaklarında API çağrılarını izleme de dahil olmak üzere performansı izlemelerine, uyarıları oluşturmalarına ve verileri arşivlemesini sağlayarak eğilimleri belirlemesine yardımcı olur.

[Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Azure Ağ İzleyicisi, Azure sanal ağındaki kaynaklarda izleme, tanılama, ölçümleri görüntüleme ve günlükleri etkinleştirme veya devre dışı bırakma işlemleri için araçlar sağlar.  Uluslar için varlıkların, NSG 'ler ve sanal makineler için ağ Izleyicisi akış günlüklerini uygulaması gerekir. Bu günlüklerin, yalnızca güvenlik günlüklerinin depolandığı ve depolama hesabına erişimin, rol tabanlı erişim denetimleriyle güvenli hale getirilmesi gereken ayrılmış bir depolama hesabında depolanması gerekir.

## <a name="threat-model"></a>Tehdit modeli
Bu başvuru mimarisi için veri akışı diyagramı [indirilebilir](https://aka.ms/au-protected-iaaswa-tm) veya aşağıda bulunabilir. Bu model, müşterilerin değişiklik yaparken sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![Au korumalı tehdit modeli Için IaaS Web uygulaması](images/au-protected-iaaswa-threat-model.png?raw=true "Au korumalı tehdit modeli diyagramı Için IaaS Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
Bu uyumluluk belgeleri, Microsoft tarafından sunulan platformlar ve hizmetler temelinde Microsoft tarafından oluşturulur. Bu belgede, çok çeşitli müşteri dağıtımları nedeniyle, yalnızca Azure ortamında barındırılan bir çözüme yönelik genelleştirilmiş bir yaklaşım sunulmaktadır. Müşteriler, kendi işletim ortamları ve iş sonuçları temelinde alternatif ürün ve Hizmetleri tanımlayabilir ve kullanabilir. Şirket içi kaynakları kullanmayı tercih eden müşteriler, bu şirket içi kaynaklara yönelik güvenlik ve işlemleri ele almalıdır. Belgelenmiş çözüm, müşteriler tarafından belirli şirket içi ve güvenlik gereksinimlerini karşılamak üzere özelleştirilebilir.

[Azure Güvenlik ve uyumluluk şeması – au korumalı müşteri sorumluluğu matrisi](https://aka.ms/au-protected-crm) , au korumalı için gereken tüm güvenlik denetimlerini listeler. Bu matris, her denetim uygulamasının Microsoft 'un, müşterinin sorumluluğunun veya ikisi arasında paylaşılıp 'nin sorumluluğundadır.

[Azure Güvenlik ve uyumluluk şeması – au korumalı IaaS Web uygulaması uygulama matrisi](https://aka.ms/au-protected-iaaswa-cim) , ' nin ayrıntılı açıklamaları dahil olmak üzere IaaS Web uygulaması mimarisi tarafından HANGI au korumalı denetimlerin giderildiği hakkında bilgi sağlar uygulamanın, kapsanan her denetimin gereksinimlerini nasıl karşıladığı.

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
[Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md) , dağıtımı yönetmek ve ortamla etkileşime geçen personele erişimi sağlamak için gereklidir. Mevcut bir Windows Server Active Directory, [dört tıklamayla](../../active-directory/hybrid/how-to-connect-install-express.md)Azure Active Directory tümleştirilebilir.

Ayrıca, [Azure Active Directory Connect](../../active-directory/hybrid/whatis-hybrid-identity.md) müşterilerin Federasyon 'yi şirket içi [Active Directory Federasyon Hizmetleri (AD FS)]( ../../active-directory/hybrid/how-to-connect-fed-azure-adfs.md) ve Azure Active Directory yapılandırmasına izin verir. Federasyon oturum açma ile müşteriler, kullanıcıların şirket ağı sırasında parolalarını yeniden girmesi gerekmeden Azure Active Directory tabanlı hizmetlerde şirket içi parolalarla oturum açmasını sağlayabilir. Active Directory Federasyon Hizmetleri (AD FS) ile Federasyon seçeneğini kullanarak, yeni bir Active Directory Federasyon Hizmetleri (AD FS) yüklemesi dağıtabilir veya var olan bir yüklemeyi Windows Server 2012 R2 grubunda belirtebilirsiniz.

Sınıflandırılan verilerin Azure Active Directory karşı eşitlenmesini engellemek için, müşteriler Azure Active Directory Connect aşağıdaki ayarları uygulayarak Azure Active Directory çoğaltılan öznitelikleri kısıtlayabilir:
- [Filtrelemeyi etkinleştir](../../active-directory/hybrid/how-to-connect-sync-configure-filtering.md)
- [Parola karması eşitlemesini devre dışı bırak](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md)
- [Parola geri yazmayı devre dışı bırak](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)
- [Cihaz geri yazmayı devre dışı bırak](../../active-directory/hybrid/how-to-connect-device-writeback.md)
- [Yanlışlıkla silmeleri](../../active-directory/hybrid/how-to-connect-sync-feature-prevent-accidental-deletes.md) ve [Otomatik yükseltmeyi](../../active-directory/hybrid/how-to-connect-install-automatic-upgrade.md) engellemek için varsayılan ayarları bırakın

## <a name="disclaimer"></a>Sorumluluk Reddi
- Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
- Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
- Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
- Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
- Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
- Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
