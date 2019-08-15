---
title: PCI DSS için IaaS Web uygulaması Azure Güvenlik ve Uyumluluk Şeması
description: PCI DSS için IaaS Web uygulaması Azure Güvenlik ve Uyumluluk Şeması
services: security
author: meladie
ms.assetid: 9c825380-2abe-4fdd-800c-59d1fed1780b
ms.service: security
ms.topic: article
ms.date: 07/03/2018
ms.author: meladie
ms.openlocfilehash: 6bff56f70c9ab0ab74e3f30c9571b3e4010b33e0
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946646"
---
# <a name="azure-security-and-compliance-blueprint-iaas-web-application-for-pci-dss"></a>Azure Güvenliği ve Uyumluluğu Şeması: PCI DSS için IaaS Web uygulaması

## <a name="overview"></a>Genel Bakış

Bu Azure Güvenlik ve Uyumluluk Şeması, bir ödeme kartı sektör veri güvenliği standartları (PCI DSS 3,2) dağıtım, depolama ve alma için uygun hizmet olarak uyumlu altyapı (IaaS) ortamının dağıtımına yönelik rehberlik sağlar. kart sahibi verileri. Ortak bir başvuru mimarisine sahiptir ve kredi kartı verilerinin (kart numarası, sona erme ve doğrulama verileri dahil) güvenli, uyumlu, çok katmanlı bir ortamda uygun şekilde işlenmesini gösterir. Bu şema, dağıtımın yükünü ve maliyetini azaltmaya yönelik bulut tabanlı bir yaklaşım arayan kuruluşların ihtiyaçlarını karşılamak için uçtan uca bir çözüm gösterir.

Bu başvuru mimarisi, uygulama kılavuzu ve tehdit modeli, müşterilerin PCI DSS 3,2 gereksinimlerine uyum sağlaması için bir temel sağlar. Bu çözüm, müşterilerin iş yüklerini Azure 'a PCI DSS 3,2 uyumlu bir şekilde dağıtmasını sağlamaya yardımcı olmak için bir temel sağlar. Ancak bu çözüm, ek yapılandırma gerektiğinden, bir üretim ortamında olduğu gibi kullanılmamalıdır.

PCI DSS uyumluluğun sağlanması için, acalacaklandırılan bir güvenlik denetçisi (QSA) bir üretim müşteri çözümünü onaylaması gerekir. Müşteriler, her bir müşterinin uygulamasının özelliklerine göre farklılık gösterebileceğinden, bu mimari kullanılarak oluşturulan herhangi bir çözüme uygun güvenlik ve uyumluluk değerlendirmelerinden sorumludur.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu çözüm bir IaaS Web uygulaması için SQL Server arka uca bir başvuru mimarisi dağıtır. Mimari bir Web katmanı, veri katmanı, Active Directory altyapı, Application Gateway ve Load Balancer içerir. Web ve veri katmanlarına dağıtılan sanal makineler, bir kullanılabilirlik kümesinde yapılandırılır ve SQL Server örnekler, yüksek kullanılabilirlik için her zaman açık kullanılabilirlik grubunda yapılandırılır. Sanal makineler etki alanına katılmış ve Active Directory Grup ilkeleri, işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmalarının uygulanması için kullanılır. Yönetim savunma ana bilgisayarı, yöneticilerin dağıtılan kaynaklara erişmesi için güvenli bir bağlantı sağlar. **Azure, başvuru mimarisi alt ağına yönetim ve veri aktarımı için bir VPN veya ExpressRoute bağlantısı yapılandırılmasını önerir.**

![PCI DSS başvuru mimarisi diyagramı Için IaaS Web uygulaması](images/pcidss-iaaswa-architecture.png "PCI DSS başvuru mimarisi diyagramı Için IaaS Web uygulaması")

Bu çözüm aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisinin ayrıntıları [dağıtım mimarisi](#deployment-architecture) bölümünde bulunur.

- Kullanılabilirlik Kümeleri
    - (1) etki alanı denetleyicileri Active Directory
    - (1) SQL kümesi düğümleri
    - (1) Web/IIS
- Azure Active Directory
- Azure Application Gateway
    - (1) Web uygulaması güvenlik duvarı
        - Güvenlik duvarı modu: önleme
        - Kural kümesi: OWASP 3,0
        - Dinleyici bağlantı noktası: 443
- Azure Key Vault
- Azure Load Balancer
- Azure İzleyici
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure Storage
    - (7) coğrafi olarak yedekli depolama hesapları
- Azure sanal makineleri
    - (1) yönetim/savunma (Windows Server 2016 Datacenter)
    - (2) Active Directory etki alanı denetleyicisi (Windows Server 2016 Datacenter)
    - (2) küme düğümünü SQL Server (Windows Server 2016 üzerinde SQL Server 2017)
    - (2) Web/IIS (Windows Server 2016 Datacenter)
- Azure Sanal Ağı
    - (1)/16 ağ
    - (5)/24 ağ
    - (5) ağ güvenlik grupları
- Bulut tanığı
- Kurtarma Hizmetleri Kasası

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde dağıtım ve uygulama öğelerinin ayrıntıları verilmiştir.

Savunma **ana bilgisayarı**: Savunma ana bilgisayarı, kullanıcıların bu ortamdaki dağıtılan kaynaklara erişmesine olanak tanıyan tek giriş noktasıdır. Savunma ana bilgisayarı, yalnızca güvenli bir listedeki genel IP adreslerinden gelen uzak trafiğe izin vererek, dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için, trafiğin kaynağının ağ güvenlik grubunda tanımlanması gerekir.

Bu çözüm, aşağıdaki yapılandırmalara sahip etki alanına katılmış bir savunma ana bilgisayarı olarak bir sanal makine oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/fundamentals/antimalware)
-   [Azure Tanılama uzantısı](../../virtual-machines/windows/extensions-diagnostics-template.md)
-   Azure Key Vault kullanarak [Azure disk şifrelemesi](../azure-security-disk-encryption-overview.md)
-   Kullanımda olmadığında sanal makine kaynaklarının tüketimini azaltmak için [otomatik olarak kapatılmış ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/)
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) , kimlik bilgilerinin ve diğer parolaların çalışan işletim sisteminden yalıtılmış korunan bir ortamda çalışmasını sağlayacak şekilde etkinleştirildi

### <a name="virtual-network"></a>Sanal ağ

Mimari, adres alanı 10.200.0.0/16 olan bir özel sanal ağ tanımlar.

**Ağ güvenlik grupları**: Bu çözüm, bir sanal ağ içindeki farklı bir Web alt ağı, veritabanı alt ağı, Active Directory alt ağı ve yönetim alt ağı olan bir mimariye kaynak dağıtır. Alt ağlar, alt ağlar arasındaki trafiği yalnızca sistem ve yönetim işlevselliği için gerekli olan ağlara kısıtlamak üzere ayrı alt ağlara uygulanan ağ güvenlik grubu kuralları tarafından mantıksal olarak ayrılır.

Bu çözümle dağıtılan [ağ güvenlik grupları](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) için yapılandırma konusuna bakın. Kuruluşlar, [Bu belgeleri](../../virtual-network/virtual-network-vnet-plan-design-arm.md) kılavuz olarak kullanarak yukarıdaki dosyayı düzenleyerek ağ güvenlik gruplarını yapılandırabilir.

Alt ağların her birinde ayrılmış bir ağ güvenlik grubu vardır:
- Application Gateway için 1 ağ güvenlik grubu (LBNSG)
- Savunma ana bilgisayarı için 1 ağ güvenlik grubu (MGTNSG)
- birincil ve yedek etki alanı denetleyicileri için 1 ağ güvenlik grubu (ADNSG)
- 1 SQL sunucuları ve bulut tanığı için ağ güvenlik grubu (SQLNSG)
- Web katmanı için 1 ağ güvenlik grubu (WEBNSG)

### <a name="data-in-transit"></a>Aktarım durumundaki veriler

Azure, Azure veri merkezlerinden gelen ve varsayılan olarak tüm iletişimleri şifreler. Ayrıca, Azure portal aracılığıyla Azure depolama 'ya yapılan tüm işlemler HTTPS üzerinden gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimari, şifreleme ve veritabanı denetimi de dahil olmak üzere, bekleyen verileri birden çok ölçüyle korur.

**Azure depolama**: Rest gereksinimlerinde şifrelenmiş verileri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) [depolama hizmeti şifrelemesi](../../storage/common/storage-service-encryption.md)kullanır. Bu, PCI DSS 3,2 tarafından tanımlanan kurumsal güvenlik taahhütlerini ve uyumluluk gereksinimlerini desteklemeye karşı, cardş verilerinin korunmasını ve korunmasını sağlar.

**Azure disk şifrelemesi**: Azure disk şifrelemesi, Windows IaaS sanal makine disklerini şifrelemek için kullanılır. [Azure disk şifrelemesi](../azure-security-disk-encryption-overview.md) , işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un BitLocker özelliğinden yararlanır. Çözüm, disk şifreleme anahtarlarının denetlenmesi ve yönetilmesine yardımcı olmak için Azure Key Vault ile tümleşiktir.

**SQL Server**: SQL Server örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
- [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) , veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin tek bir merkezi konumda kimlik yönetimine izin verebilir.
-   [SQL veritabanı denetimi](../../sql-database/sql-database-auditing.md) , veritabanı olaylarını izler ve bunları Azure Storage hesabındaki bir denetim günlüğüne yazar.
-   SQL veritabanı, bekleyen bilgileri korumak için veritabanı, ilişkili yedeklemeler ve işlem günlüğü dosyalarını gerçek zamanlı şifrelemeyi ve şifre çözmeyi gerçekleştiren [Saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)kullanmak üzere yapılandırılmıştır. Saydam Veri Şifrelemesi, depolanan cardş verilerinin yetkisiz erişime maruz olmadığından emin olmak için güvence sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) , uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](../../sql-database/sql-database-threat-detection.md) , şüpheli veritabanı etkinlikleri, olası güvenlik AÇıKLARı, SQL ekleme saldırıları ve anormal veritabanı erişim desenleri için güvenlik uyarıları sunarak meydana gelebilecek olası tehditlere yönelik algılama ve yanıt verir.
-   [Always Encrypted sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) , hassas kart sahibi verilerinin veritabanı sisteminde hiçbir şekilde düz metin olarak göründüğünden emin olun. Veri şifrelemeyi etkinleştirdikten sonra, yalnızca anahtarlara erişimi olan istemci uygulamaları veya uygulama sunucuları düz metin verilerine erişebilir.
- [Genişletilmiş özellikler](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) özelliği, kullanıcıların veritabanı nesnelerine özel özellikler eklemesine ve verileri "Discontinued" olarak etiketlemesine izin verdiğinden, ilişkili işlemler için uygulama mantığını desteklemek için kullanılabilir kart sahibi verileri.
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) , kullanıcıların işleme devam etmek için verilere erişimi kısıtlamak üzere ilkeler tanımlamasına olanak sağlar.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) , verileri ayrıcalıklı olmayan kullanıcılara veya uygulamalara maskeleyerek hassas kart sahibi veri pozlamasını sınırlar. Dinamik veri maskeleme, potansiyel olarak duyarlı verileri otomatik olarak bulabilir ve uygun maskeleri uygulanmasını önerebilir. Bu, kart sahibi verilerine erişimi, yetkisiz erişim yoluyla veritabanından çıkmayacak şekilde tanımanıza ve azaltmaya yardımcı olur. Müşteriler, dinamik veri maskeleme ayarlarının veritabanı şemasına uyacak şekilde ayarlamaktan sorumludur.

### <a name="identity-management"></a>Kimlik yönetimi

Aşağıdaki teknolojiler, Azure ortamındaki verilere erişimi yönetmeye yönelik yetenekler sağlar:

- [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) , Microsoft&#39;s çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüme yönelik tüm kullanıcılar Azure SQL veritabanına erişen kullanıcılar dahil Azure Active Directory oluşturulur.
- Uygulamanın kimlik doğrulaması, Azure Active Directory kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [uygulamaları Azure Active Directory tümleştirme](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). Ayrıca, veritabanı sütun şifrelemesi uygulamanın kimliğini Azure SQL veritabanı 'nda doğrulamak için Azure Active Directory kullanır. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'nda hassas verileri koruma](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- [Azure rol tabanlı erişim denetimi](../../role-based-access-control/role-assignments-portal.md) , yöneticilerin yalnızca kullanıcıların işlerini gerçekleştirmesi için ihtiyaç duyduğu erişim miktarını vermek üzere hassas erişim izinleri tanımlamasına olanak sağlar. Yöneticiler, her kullanıcıya Azure kaynakları için Kısıtlanmamış izin vermek yerine, verilere erişim için yalnızca belirli eylemlere izin verebilir. Abonelik erişimi, abonelik yöneticisiyle sınırlıdır.
- [Azure Active Directory Privileged Identity Management](../../active-directory/privileged-identity-management/pim-getting-started.md) , müşterilerin belirli bilgilere erişimi olan kullanıcı sayısını en aza indirmesine olanak sağlar. Yöneticiler, ayrıcalıklı kimlikleri ve kaynaklara erişimlerini bulma, kısıtlama ve izleme işlemleri için Azure Active Directory Privileged Identity Management kullanabilir. Bu işlev, gerektiğinde isteğe bağlı, tam zamanında yönetim erişimi sağlamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](../../active-directory/identity-protection/overview.md) , kuruluş&#39;kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluş&#39;kimlikleriyle ilgili olarak algılanan şüpheli eylemlere yönelik otomatik yanıtları yapılandırır ve araştırır şüpheli olayları çözmek için uygun eylemi gerçekleştirin.

### <a name="security"></a>Güvenlik

**Gizli dizi yönetimi**: Çözüm, anahtarların ve parolaların yönetimi için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) kullanır. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşterilerin bu verileri korumalarına ve bu verilere erişmesine yardımcı olur:

- Gelişmiş erişim ilkeleri, gereksinim temelinde yapılandırılır.
- Key Vault erişim ilkeleri anahtarlar ve gizlilikler için gereken en düşük izinlerle tanımlanır.
- Key Vault tüm anahtarlar ve gizli dizileri için süre sonu tarihleri vardır.
- Key Vault tüm anahtarlar özel donanım güvenliği modülleri tarafından korunur. Anahtar türü bir HSM korumalı 2048 bit RSA anahtarıdır.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanılarak gerekli olan en düşük izinlere sahiptir.
- Key Vault için tanılama günlükleri, en az 365 günlük bir bekletme dönemi ile etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri, gerekli olanlarla kısıtlıdır.
- Bu çözüm, IaaS sanal makine disk şifreleme anahtarlarını ve gizli dizileri yönetmek için Azure Key Vault ile tümleşiktir.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeler almak için varsayılan olarak yapılandırılır. Bu çözüm Ayrıca, gerektiğinde sanal makinelere yama yapmak üzere güncelleştirilmiş dağıtımların oluşturulabilmesi için [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) hizmetini de içerir.

**Kötü amaçlı yazılımdan koruma**: Sanal makineler için [Microsoft kötü amaçlı yazılımdan](https://docs.microsoft.com/azure/security/fundamentals/antimalware) koruma, bilinen kötü amaçlı veya istenmeyen yazılım denemesi sırasında yapılandırılabilir uyarılarla virüsler, casus yazılım ve diğer kötü amaçlı yazılımların tanımlanmasına ve kaldırılmasına yardımcı olan gerçek zamanlı koruma özelliği sağlar. korumalı sanal makinelerde install veya çalıştırın.

**Azure Güvenlik Merkezi**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)ile, müşteriler iş yükleri genelinde güvenlik ilkelerini merkezi olarak uygulayabilir ve yönetebilir, tehditlere maruz kalma olasılığını sınırlayabilir, saldırıları algılayıp yanıtlayabilir. Ayrıca, Azure Güvenlik Merkezi, güvenlik duruşunu artırmaya ve verileri korumaya yardımcı olmak için yapılandırma ve hizmet önerileri sağlamak üzere Azure hizmetleri 'nin mevcut yapılandırmalarına erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen olası saldırılardan müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi, bir tehdit ya da şüpheli etkinlik gerçekleştiğinde tetiklenen bir dizi [önceden tanımlanmış güvenlik uyarısı](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)içerir. Azure Güvenlik Merkezi 'ndeki [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) , müşterilerin ortamlarından zaten toplanmış olan verileri temel alarak yeni güvenlik uyarıları tanımlamasına olanak tanır.

Azure Güvenlik Merkezi, öncelik veren güvenlik uyarıları ve olayları sunarak müşterilerin olası güvenlik sorunlarını bulmasını ve adreslerini daha kolay hale getirmesini sağlar. Algılanan her tehdit için [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) , tehditleri İnceleme ve düzeltme konusunda olay yanıtı ekiplerine yardımcı olacak şekilde oluşturulmuştur.

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

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

Azure Hizmetleri, sistem durumunun yanı sıra sistem durumu ve Kullanıcı etkinliklerini de yoğun olarak günlüğe kaydeder:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](../../azure-monitor/platform/activity-logs-overview.md) , bir abonelikteki kaynaklarda gerçekleştirilen işlemlerle ilgili öngörüler sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşma süresinin ve durumunun belirlenmesine yardımcı olabilir.
- **Tanılama günlükleri**: [Tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md) her kaynak tarafından yayılan tüm günlükleri içerir. Bu Günlükler Windows olay sistemi günlükleri, Azure depolama günlükleri, Key Vault denetim günlükleri ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri, arşivleme için merkezi ve şifrelenmiş bir Azure depolama hesabına yazar. Bekletme, kuruluşa özgü saklama gereksinimlerini karşılamak için Kullanıcı tarafından yapılandırılabilir ve 730 güne kadar.

**Azure izleyici günlükleri**: Bu Günlükler, işleme, depolama ve Pano raporlama için [Azure izleyici günlüklerinde](https://azure.microsoft.com/services/log-analytics/) birleştirilir. Toplandıktan sonra veriler, özgün kaynağından bağımsız olarak tüm verilerin birlikte çözümlenme olanağı sağlayan Log Analytics çalışma alanları içindeki her bir veri türü için ayrı tablolar halinde düzenlenir. Ayrıca, Azure Güvenlik Merkezi, müşterilerin güvenlik olay verilerine erişmek ve diğer hizmetlerdeki verilerle birleştirmek için kusto sorguları kullanmasına izin veren Azure Izleyici günlükleri ile tümleşir.

Aşağıdaki Azure [izleme çözümleri](../../monitoring/monitoring-solutions.md) , bu mimarinin bir parçası olarak dahil edilmiştir:
-   [Active Directory değerlendirmesi](../../azure-monitor/insights/ad-assessment.md): Active Directory sistem durumu denetimi çözümü, düzenli bir aralıkta sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](../../azure-monitor/insights/sql-assessment.md): SQL sistem durumu denetimi çözümü, düzenli aralıklarla sunucu ortamlarının riskini ve sistem durumunu değerlendirir ve müşterilere dağıtılan Sunucu altyapısına özgü önerilerin öncelikli bir listesini sağlar.
- [Aracı durumu](../../monitoring/monitoring-solution-agenthealth.md): Aracı Durumu çözümü, kaç aracının dağıtıldığını ve coğrafi dağıtımını, yanıt vermeyen aracı sayısını ve işletimsel verileri gönderen aracıların sayısını bildirir.
-   [Etkinlik günlüğü Analizi](../../azure-monitor/platform/collect-activity-logs.md): Etkinlik Günlüğü Analizi çözümü, bir müşterinin tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.

**Azure Otomasyonu**: [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook 'ları depolar, çalıştırır ve yönetir. Bu çözümde, runbook 'lar Azure SQL veritabanından günlükleri toplamaya yardımcı olur. Otomasyon [değişiklik izleme](../../automation/change-tracking.md) çözümü, müşterilerin ortamdaki değişiklikleri kolayca belirlemesine olanak sağlar.

**Azure izleyici**: [Azure izleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) , kuruluşların Azure kaynaklarında API çağrılarını izleme de dahil olmak üzere performansı izlemelerine, uyarıları oluşturmalarına ve verileri arşivlemesini sağlayarak eğilimleri belirlemesine yardımcı olur.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akışı diyagramı (DFD) [indirme](https://aka.ms/pcidss-iaaswa-tm) için kullanılabilir veya aşağıda bulunabilir. Bu model, müşterilerin değişiklik yaparken sistem altyapısında olası risk noktalarını anlamasına yardımcı olabilir.

![PCI DSS başvuru mimarisi diyagramı Için IaaS Web uygulaması](images/pcidss-iaaswa-threat-model.png "PCI DSS başvuru mimarisi diyagramı Için IaaS Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure Güvenlik ve Uyumluluk Şeması PCI DSS müşteri sorumluluğu matrisi](https://aka.ms/pcidss-crm) , tüm PCI DSS 3,2 gereksinimlerinin sorumluluklarını listeler.

[Azure Güvenlik ve uyumluluk şeması-PCI DSS IaaS Web uygulaması Için matris](https://aka.ms/pcidss-iaaswa-cim) , ' ın nasıl yapıldığını açıklayan ayrıntılı açıklamalar dahil olmak üzere IaaS Web uygulaması mimarisi tarafından hangi PCI DSS 3,2 gereksinimlerinin giderildiği hakkında bilgi sağlar. uygulama, kapsanan her bir makalenin gereksinimlerini karşılar.

## <a name="guidance-and-recommendations"></a>Kılavuz ve öneriler

### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute

Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) , bu IaaS Web uygulaması başvuru mimarisinin bir parçası olarak dağıtılan kaynaklarla güvenli bir şekilde bağlantı kuracak şekilde yapılandırılmalıdır. Bir VPN veya ExpressRoute 'u uygun şekilde ayarlayarak, müşteriler aktarım sırasında veriler için bir koruma katmanı ekleyebilir.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı Internet üzerinden gerçekleşir ve müşterilerin, müşterinin ağı ile Azure arasındaki şifrelenmiş bir bağlantı içindeki bilgileri güvenli bir şekilde "tünele" tüneletmesine olanak tanır. Siteden siteye VPN, delikler için tüm boyutlardaki kuruluşlar tarafından dağıtılan güvenli, Olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) Bu seçenekte bir şifreleme mekanizması olarak kullanılır.

VPN tünelinin içindeki trafik, siteden siteye VPN ile Internet 'Te gezintiğinden, Microsoft başka bir daha bile daha güvenli bağlantı seçeneği sunar. Azure ExpressRoute, Azure ile şirket içi bir konum veya bir Exchange barındırma sağlayıcısı arasındaki adanmış bir WAN bağlantıdır. ExpressRoute bağlantıları Internet üzerinden gitmediğinden, bu bağlantılar daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve Internet üzerinden tipik bağlantılardan daha yüksek güvenlik sunar. Ayrıca, bu müşterinin iletişim sağlayıcısının doğrudan bir bağlantısı olduğundan, veriler Internet üzerinden hareket etmez ve bu nedenle bu açık değildir.

Şirket içi bir ağı Azure 'a genişleten güvenli bir karma ağ uygulamak için en iyi yöntemler [mevcuttur](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Sorumluluk Reddi

- Bu belge yalnızca bilgilendirme amaçlıdır. MICROSOFT BU BELGEDEKI BILGILERE GÖRE HIÇBIR GARANTI VERMEZ, AÇIK, ZIMNI VEYA YASAL DEĞILDIR. Bu belge "olduğu gibi" verilmiştir. Bu belgede ifade edilen, URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bilgiler ve görünümler bildirimde bulunmaksızın değiştirilebilir. Bu belgeyi okuyan müşteriler bunu kullanmanın riskini de taşır.
- Bu belge, müşterilere herhangi bir Microsoft ürün veya çözümünden hiçbir fikri mülkiyet hakkı sağlamaz.
- Müşteriler bu belgeyi, iç başvuru amaçları için kopyalayabilir ve kullanabilir.
- Bu belgedeki bazı öneriler, Azure 'da veri, ağ veya işlem kaynağı kullanımının artmasına neden olabilir ve bir müşterinin Azure lisansını veya abonelik maliyetlerini artırabilir.
- Bu mimarinin, müşterilerin belirli gereksinimlerine göre ayarlanmalarına yönelik bir temel işlevi görmesi ve bir üretim ortamında olduğu gibi kullanılmamalıdır.
- Bu belge bir başvuru olarak geliştirilmiştir ve bir müşterinin belirli uyumluluk gereksinimlerini ve düzenlemeleri karşılayabileceği tüm yolları tanımlamak için kullanılmamalıdır. Müşteriler, onaylı müşteri uygulamalarında organizasyonlarından yasal destek almalıdır.
