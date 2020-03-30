---
title: PCI DSS için güvenli web uygulaması | Microsoft Dokümanlar
description: Bu örnek uygulama, Azure'da geliştirdiğinizde uygulamanızı ve kuruluşunuzun güvenlik duruşunu iyileştiren en iyi güvenlik uygulamalarını uygular.
keywords: yok
services: security
documentationcenter: na
author: fehase
manager: rkarlin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2019
ms.author: terrylan
ms.openlocfilehash: 4fe612db65d985be2f1f1c81d03c3ee735c03889
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69992620"
---
# <a name="develop-a-secure-infrastructure-for-a-pci-app"></a>PCI uygulaması için güvenli bir altyapı geliştirme

## <a name="overview"></a>Genel Bakış

Ödeme Kartı Endüstrisi (PCI) uygulaması için bu güvenli altyapı, kart sahibi verilerinin toplanması, saklanması ve alınması için uygun bir hizmet (PaaS) ortamı olarak bir Ödeme Kartı Endüstrisi platformunun dağıtımı için kılavuz sağlar. Bu çözüm, ortak bir başvuru mimarisi için Azure kaynaklarının dağıtımını ve yapılandırmasını otomatikleştirerek müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecekleri ve müşterilerin oluşturması ve oluşturması için bir temel görevi göstermeleri Azure'da kendi çözümlerini yapılandırın. Çözüm, Ödeme Kartı Endüstrisi Veri Güvenliği Standartları 'na (PCI DSS 3.2) benzer bir alt gereksinim kümesini uygular.

Bu örnek, müşterilerin PCI DSS 3.2 gereksinimlerine benzer uyumluluk elde edebilmesine yardımcı olmak için önceden yapılandırılmış güvenlik denetimleri içeren bir PaaS web uygulama başvuru mimarisini otomatik olarak dağıtır. Çözüm, kaynak dağıtımı ve yapılandırmasını yönlendiren Azure Kaynak Yöneticisi şablonları ve PowerShell komut dosyalarından oluşur.

Uygulama bileşenlerinin düzgün şekilde yapılandırıldığından emin olmak için bu makalede açıklanan adımları sırayla izlemeniz gerekir. Veritabanı, Azure Uygulama Hizmeti, Azure Anahtar Kasası örneği ve Azure Uygulama Ağ Geçidi örneği birbirine bağlıdır.

Dağıtım komut dosyaları altyapıyı ayarlar. Dağıtım komut dosyalarını çalıştırdıktan sonra, bileşenleri ve hizmetleri birbirine bağlamak için Azure portalında bazı el ile yapılandırma yapmanız gerekir.

Bu örnek, Azure'da perakende sektöründe çalışan ve güvenli Azure altyapısına sahip bir perakende uygulaması oluşturmak isteyen deneyimli geliştiricilere yöneliktir.

> [!NOTE]
> Bu mimari, müşterilerin kendi özel gereksinimlerine uyum sağlamaları için bir temel görevi görmek için tasarlanmıştır ve üretim ortamında olduğu gibi kullanılmamalıdır.

Bir uygulamayı değişiklik yapmadan bu ortama yerleştirmek PCI DSS 3.2'nin gereksinimlerini tamamen karşılamak için yeterli değildir. Şunlara dikkat edin:

- Bu mimari, müşterilerin Azure'u PCI DSS 3.2 uyumlu bir şekilde kullanmasına yardımcı olmak için bir temel sağlar.
- Gereksinimler her müşterinin uygulamasının özelliklerine bağlı olarak değişebileceğinden, müşteriler bu mimari kullanılarak oluşturulmuş herhangi bir çözümün uygun güvenlik ve uyumluluk değerlendirmesini yapmakla yükümlüdür.

Bu uygulamayı geliştirirken ve dağıtAbiliyorsanız, şunları öğrenin:

- Bir Azure Anahtar Kasası örneği oluşturun ve ondan sırlar saklayın ve alın.
- Azure SQL için Azure Veritabanı'nı dağıtın, güvenli veriler ayarlayın ve bu veritabanına erişim yetkisi verin.
- Azure web uygulamasını, ön uç güvenlik duvarı aEcess ile özel bir izole edilmiş Olan App Service ortamıyla dağıtın.
- [OWASP Top 10 Ruleset](https://coreruleset.org/)kullanan bir güvenlik duvarıyla bir Azure Uygulama Ağ Geçidi örneği oluşturun ve yapılandırın.
- Azure hizmetlerini kullanarak aktarım sırasında ve istirahatte veri şifrelemesini etkinleştirin.
- Uyumlulukları değerlendirmek için Azure ilkesini ve mavi baskıları ayarlama

Bu uygulamayı geliştirip dağıttıktan sonra, açıklanan yapılandırma ve güvenlik önlemleriyle birlikte aşağıdaki örnek web uygulamasını kurmuş olabilirsiniz.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Uygulama, üç katmanlı tipik bir n katmanı uygulamasıdır. İzleme ve gizli yönetim bileşenleritümleşik ön uç, arka uç ve veritabanı katmanı burada gösterilmiştir:

![Uygulama mimarisi](./media/secure-pci-web-app/architecture.png)

Mimari şu bileşenlerden oluşur:

- [Uygulama Servis Ortamı v2](https://docs.microsoft.com/azure/app-service/environment/intro/). Uygulama mimarimiz için Uygulama Hizmet Ortamı ve yük dengeleyicisi sağlar.
- [Azure Uygulama Ağ Geçidi](https://docs.microsoft.com/azure/application-gateway/). Uygulama mimarimiz için ağ geçidi ve güvenlik duvarı sağlar.
- [Uygulama Öngörüleri](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview). Birden çok platformda genişletilebilir Bir Uygulama Performans Yönetimi (APM) hizmeti sağlar.
- [Azure Anahtar Kasası](https://docs.microsoft.com/azure/key-vault/). Uygulamamızın sırlarını saklar ve şifreler ve bunların etrafında erişim politikaları oluşturulmasını yönetir.
- [Azure Etkin Dizin.](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis/) Bulut tabanlı kimlik ve erişim yönetimi hizmeti, oturum açma ve erişim kaynakları sağlar.
- [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview). Etki alanını barındırmak için hizmet sağlar.
- [Azure Yük Dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview/). Uygulamalarınızı ölçeklendirin ve hizmetleriniz için yüksek kullanılabilirlik oluşturur
- [Azure Depolama](https://docs.microsoft.com/azure/storage/common/storage-introduction/). Modern veri depolama senaryoları için çözüm sağlar.
- [Azure Web Uygulaması](https://docs.microsoft.com/azure/app-service/overview/).  Web uygulamalarını barındırmak için HTTP tabanlı bir hizmet sağlar.
- [AzureSQL için Azure Veritabanı.](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview/) Uygulamamızın verilerini güvenli bir şekilde saklar.
- [Azure Planları](https://docs.microsoft.com/azure/governance/blueprints/overview/). Spesifikasyonlar ve belirli standartlar ve gereksinimlere uygunluk sağlar.
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/)
- [Azure İlkesi](https://docs.microsoft.com/azure/governance/policy/overview). Kaynaklarınızı atanan ilkelere uymayarak değerlendirir.

## <a name="threat-model"></a>Tehdit modeli
Tehdit modelleme, işletmenize ve uygulamanıza yönelik olası güvenlik tehditlerini belirleme ve ardından uygun bir azaltma planının uygulanmasını sağlama işlemidir.

Bu örnek, güvenli örnek uygulama için tehdit modellemesi uygulamak için [Microsoft Tehdit Modelleme Aracı'nı](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) kullanmıştır. Bileşenleri ve veri akışlarını diyagramlayarak, geliştirme sürecinin başlarında sorunları ve tehditleri tanımlayabilirsiniz. Bu daha sonra zaman ve para tasarrufu sağlar.

Bu örnek uygulama için tehdit modeli:

![Tehdit modeli](./media/secure-pci-web-app/threat-model.png)

Tehdit modelleme aracının oluşturduğu bazı örnek tehditler ve olası güvenlik açıkları aşağıdaki ekran görüntüsünde gösterilir. Tehdit modeli, maruz kalan saldırı yüzeyine genel bir bakış sağlar ve geliştiricileri sorunları nasıl azaltacakları konusunda düşünmeye teşvik eder.

![Tehdit modeli çıktısı](./media/secure-web-app/threat-model-output.png)

Örneğin, önceki tehdit modeli çıkışındaki SQL enjeksiyonu, kullanıcı girişlerinin temizlenmesi ve PostgreSQL için Azure Veritabanı'nda depolanan işlevler kullanılarak azaltılır. Bu azaltma, veri okuma ve yazma sırasında sorguların rasgele yürütülmesini engeller.

Geliştiriciler, tehdit modeli çıkışındaki tehditlerin her birini azaltarak sistemin genel güvenliğini artırır.

## <a name="deployment"></a>Dağıtım
### <a name="prerequisites"></a>Ön koşullar
Uygulamayı çalışır hale getirmek için aşağıdaki araçları yüklemeniz gerekir:

- Uygulama kodunu değiştirmek ve görüntülemek için bir kod düzenleyicisi. [Visual Studio Code](https://code.visualstudio.com/) açık kaynak seçeneğidir.
- Geliştirme bilgisayarınızda [Azure CLI.](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest&viewFallbackFrom=azure-cli-latest)
- Sisteminizde [git.](https://git-scm.com/) Git, kaynak kodunu yerel olarak klonlamak için kullanılır.
- [jq](https://stedolan.github.io/jq/), kullanıcı dostu bir şekilde JSON sorgulama kiçin bir UNIX aracı.

Bu örnek, Azure içindeki kaynakları dağıtmak için Azure Kaynak Yöneticisi'nin API hizmeti tarafından işlenen JSON yapılandırma dosyaları ve PowerShell komut dosyalarından oluşur. Ayrıntılı dağıtım [talimatlarına buradan](https://github.com/Azure-Samples/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms-AzureRM)ulaşabilirsiniz.

#### <a name="quickstart"></a>Hızlı Başlangıç

1.  [Bu](https://github.com/Azure-Samples/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms-AzureRM.git) GitHub deposunu yerel iş istasyonunuza klonla veya indirin.
2.  0-Setup-AdministrativeAccountAndPermission.md gözden geçirin ve sağlanan komutları çalıştırın.
3.  Contoso örnek verileriyle bir test çözümü dağıtın veya ilk üretim ortamını pilot yapın.

    Bu komut dosyası, Contoso örnek verilerini kullanarak bir web mağazasının gösterimi için Azure kaynaklarını dağıtıyor.

Bu örnekte, web uygulamasını App Service'e dağıtan ve kaynakları oluşturan dağıtım komut dosyasını çalıştırırsınız.

Azure'da uygulamaları dağıtmanın birçok yolu vardır:

- Azure Resource Manager şablonları
- PowerShell
- Azure CLI
- Azure portalında
- Azure DevOps

## <a name="guidance-and-recommendations"></a>Rehberlik ve öneriler

### <a name="network"></a>Ağ
Mimari, adres alanı 10.200.0.0/16 olan özel bir Sanal Ağ tanımlar.
 ![Virtual_Network](./media/secure-pci-web-app/virtual-network.png)

### <a name="network-security-groups"></a>Ağ güvenlik grupları
Ağ güvenlikhttps://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) grupları, sanal ağ daki trafiğe izin veren veya reddeden Erişim Denetim Listeleri (ALA)'lar içerir. Ağ güvenlik grupları, trafiği bir alt ağ veya tek tek VM düzeyinde güvenli hale getirmek için kullanılabilir. Aşağıdaki ağ güvenlik grupları vardır:

- Uygulama Ağ Geçidi için 1 ağ güvenlik grubu
- Uygulama Hizmet Ortamı için 1 ağ güvenlik grubu
- Azure SQL Veritabanı için 1 ağ güvenlik grubu
- Burç ana bilgisayar için 1 ağ güvenlik grubu

Ağ güvenlik gruplarının her birinin, çözümün güvenli ve doğru bir şekilde çalışabilmesi için belirli bağlantı noktaları ve protokolleri açıktır. Ayrıca, her ağ güvenlik grubu için aşağıdaki yapılandırmalar etkinleştirilir:

- Tanılama günlüklerihttps://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) ve olaylar(etkin ve bir depolama hesabında depolanır)
- Azure Monitor günlükleri ağ güvenlik grubunun tanılamalarına bağlıdır.https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

### <a name="nsg-config"></a>NSG Config
Uygulama Hizmet Ortamı için NSG config aşağıdaki resimde gösterildiği gibi yapılandırılmalıdır.

 ![NSG_Config](./media/secure-pci-web-app/nsg-config.png)

Her alt ağ, karşılık gelen ağ güvenlik grubuyla ilişkilidir.

### <a name="config"></a>Config
Alt ağlar aşağıdaki resimde gösterildiği gibi yapılandırılır.
 ![Config](./media/secure-pci-web-app/config.png)

### <a name="azure-dns"></a>Azure DNS
Alan Adı Sistemi (DNS), bir web sitesinin veya hizmet adının IP adresine çevrilme (veya çözümlenmesinden) sorumludur. [Azure DNS,](https://docs.microsoft.com/azure/dns/dns-overview) Azure altyapılarını kullanarak ad çözümlemesi sağlayan DNS etki alanları için bir barındırma hizmetidir. Kullanıcılar, Azure'da etki alanlarını barındırarak, diğer Azure hizmetleriyle aynı kimlik bilgilerini, API'leri, araçları ve faturalandırmayı kullanarak DNS kayıtlarını yönetebilir. Azure DNS özel DNS etki alanlarını da destekler.

### <a name="protect-data"></a>Veri koruma
Buluttaki verilerin korunmasına yardımcı olmak için, verilerinizin oluşabileceği olası durumları ve bu durum için hangi denetimlerin kullanılabildiğini hesaba katmanız gerekir. Azure veri güvenliği ve şifreleme için en iyi uygulamalar aşağıdaki veri durumlarla ilgilidir:

- Istirahatte: Bu, manyetik veya optik disk olsun, fiziksel ortamda statik olarak var olan tüm bilgi depolama nesnelerini, kapsayıcıları ve türlerini içerir.
- Aktarım sırasında: Veriler bileşenler, konumlar veya programlar arasında aktarılırken, bu veri aktarım sırasındadır. Örnekler, ağ üzerinden, bir servis veri yolu üzerinden (ExpressRoute gibi karma bağlantılar da dahil olmak üzere şirket içi buluta ve tam tersi) veya giriş/çıkış işlemi sırasında aktarılmasıdır.

### <a name="azure-storage"></a>Azure Storage
Tüm [Azure Depolama,](https://azure.microsoft.com/services/storage/) şifreli verileri dinlenme gereksinimlerinde karşılamak için, verilere erişen ve şifreleyen anahtarların denetimini sağlamak için Azure Key Vault'u kullanır. Bu, PCI DSS 3.2 tarafından tanımlanan kuruluş güvenlik taahhütlerini ve uyumluluk gereksinimlerini desteklemek için kart sahibi verilerinin korunmasına ve korunmasına yardımcı olur.

### <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi
Azure Disk Şifreleme, veri diskleri için birim şifreleme sağlamak için Windows'un BitLocker özelliğinden yararlanır. Çözüm, disk şifreleme anahtarlarını kontrol etmeye ve yönetmeye yardımcı olmak için Azure Key Vault ile tümleşir.

### <a name="azure-sql-database"></a>Azure SQL Veritabanı
Azure SQL Veritabanı örneği aşağıdaki veritabanı güvenlik ölçülerini kullanır:

- [Etkin Dizin kimlik doğrulaması ve](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) yetkilendirmesi, veritabanı kullanıcılarının ve diğer Microsoft hizmetlerinin tek bir merkezi konumda kimlik yönetimine olanak tanır.
- [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started/) veritabanı olaylarını izler ve bunları bir Azure depolama hesabındaki denetim günlüğüne yazar.
- Azure SQL Veritabanı, bilgileri istirahatte korumak için veritabanının, ilişkili yedeklemelerin ve işlem günlüğü dosyalarının gerçek zamanlı şifreleme ve şifre çözme işlemlerini gerçekleştiren [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)kullanacak şekilde yapılandırılmıştır. Saydam veri şifreleme, depolanan verilerin yetkisiz erişime tabi tutulmadığı konusunda güvence sağlar.
- [Güvenlik duvarı kuralları,](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) uygun izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
- [SQL Tehdit Algılama,](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinlikleri, olası güvenlik açıkları, SQL enjeksiyon saldırıları ve anormal veritabanı erişim desenleri için güvenlik uyarıları sağlayarak olası tehditlere karşı algılama ve yanıt verme olanağı sağlar.
- [Şifrelenmiş Sütunlar,](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verilerin veritabanı sisteminde düz metin olarak görünmemesini sağlar. Veri şifrelemesini etkinleştirdikten sonra, yalnızca istemci uygulamaları veya anahtarlara erişimi olan uygulama sunucuları düz metin verilerine erişebilir.
- [SQL Database dinamik veri maskeleme,](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) verileri ayrıcalıklı olmayan kullanıcılara veya uygulamalara maskeleyerek hassas veri maruziyetini sınırlar. Dinamik veri maskeleme, hassas olabilecek verileri otomatik olarak keşfedebilir ve uygulanacak uygun maskeleri önerebilir. Bu, veritabanından yetkisiz erişim yoluyla çıkmaması için verilere erişimi belirlemeye ve azaltmaya yardımcı olur. Müşteriler, dinamik veri maskeleme ayarlarını veritabanı şemalarına uyacak şekilde ayarlamakla yükümlüdür.

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojiler, Azure ortamında kart sahibi verilerine erişimi yönetmek için özellikler sağlar:

- Azure Active Directory, Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözümün tüm kullanıcıları, Azure SQL Veritabanı'na erişen kullanıcılar da dahil olmak üzere Azure Active Directory'de oluşturulur.
- Uygulamanın kimlik doğrulaması Azure Active Directory kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz. [Uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/plan-an-application-integration). Ayrıca, veritabanı sütun şifrelemesi, uygulamayı Azure SQL Veritabanı'na doğrulamak için Azure Active Directory'yi kullanır. Daha fazla bilgi için [Azure SQL Veritabanı'nda hassas verilerin nasıl korunabildiğini](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview)öğrenin.
- Azure rol tabanlı erişim denetimi, yöneticilerin yalnızca kullanıcıların işlerini gerçekleştirmek için gereken erişim miktarını vermek için ince taneli erişim izinlerini tanımlamasına olanak tanır. Yöneticiler, her kullanıcıya Azure kaynakları için sınırsız izin vermek yerine, kart sahibi verilerine erişmek için yalnızca belirli eylemlere izin verebilir. Abonelik erişimi abonelik yöneticisiyle sınırlıdır.
- Azure Active Directory Ayrıcalıklı Kimlik Yönetimi, müşterilerin kart sahibi verileri gibi belirli bilgilere erişimi olan kullanıcı sayısını en aza indirmesini sağlar. Yöneticiler, ayrıcalıklı kimlikleri ve kaynaklara erişimlerini keşfetmek, kısıtlamak ve izlemek için Azure Etkin Dizin Ayrıcalıklı Kimlik Yönetimi'ni kullanabilir. Bu işlevsellik, gerektiğinde isteğe bağlı, tam zamanında yönetim erişimi zorlamak için de kullanılabilir.
- Azure Etkin Dizin Kimlik Koruması, kuruluşun kimliklerini etkileyen olası güvenlik açıklarını algılar, kuruluşun kimliğiyle ilgili algılanan şüpheli eylemlere otomatik yanıtlar yapılandırır ve şüpheli olayları çözmek için uygun eylemi yapmak.

### <a name="secrets-management"></a>Gizli dizi yönetimi
Çözüm, anahtarların ve sırların yönetimi için Azure Key Vault'u kullanır. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Anahtar Kasası özellikleri, müşterilerin bu verileri korumasına ve bunlara erişmemede yardımcı olur:

- Gelişmiş erişim ilkeleri gereksinim bazında yapılandırılır.
- Anahtar Vault erişim ilkeleri, anahtarlar ve sırlar için gereken en az izinle tanımlanır.
- Key Vault'taki tüm anahtarlar ve sırlar son kullanma tarihlerine sahiptir.
- Key Vault'taki tüm anahtarlar özel donanım güvenlik modülleri ile korunmaktadır. Anahtar türü HSM Korumalı 2048 bit RSA Anahtarıdır.
- Key Vault ile anahtarları ve sırları (kimlik doğrulama anahtarları, depolama hesap anahtarları, veri şifreleme anahtarları gibi) şifreleyebilirsiniz. PFX dosyaları ve parolaları) donanım güvenlik modülleri (HSM) tarafından korunan tuşları kullanarak
- Belirli bir kapsamda kullanıcılara, gruplara ve uygulamalara izin atamak için RBAC'ı kullanın.
- TLS sertifikalarınızı otomatik yenileme ile yönetmek için Key Vault'u kullanın.
- Key Vault için tanılama günlükleri en az 365 günlük bir bekletme süresi ile etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanla sınırlıdır.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
Azure Güvenlik Merkezi ile müşteriler iş yükleri arasında güvenlik ilkelerini merkezi olarak uygulayabilir ve yönetebilir, tehditlere maruz kalma sınırını sınırlayabilir ve saldırıları algılayabilir ve yanıtlayabilir. Ayrıca, Azure Güvenlik Merkezi, güvenlik duruşunu iyileştirmeye ve verileri korumaya yardımcı olmak için yapılandırma ve hizmet önerileri sağlamak için Azure hizmetlerinin mevcut yapılandırmalarına da erişir.

Azure Güvenlik Merkezi, müşterileri ortamlarını hedef alan olası saldırılarkonusunda uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi'nde, bir tehdit veya şüpheli etkinlik gerçekleştiğinde tetiklenen önceden tanımlanmış güvenlik uyarıları kümesi vardır. Azure Güvenlik Merkezi'ndeki özel uyarı kuralları, müşterilerin çevrelerinden zaten toplanan verilere dayalı olarak yeni güvenlik uyarıları tanımlamasına olanak tanır.

Azure Güvenlik Merkezi, müşterilerin olası güvenlik sorunlarını keşfetmesini ve busorunları çözmesini kolaylaştırarak öncelikli güvenlik uyarıları ve olayları sağlar. Tespit edilen her tehdit için, tehditleri araştırmave tedavi etmede olay müdahale ekiplerine yardımcı olmak için bir tehdit istihbarat raporu oluşturulur.

### <a name="azure-application-gateway"></a>Azure Application Gateway
Mimari, web uygulama güvenlik duvarı yapılandırılan ve OWASP kural kümesi etkinleştirilmiş bir Azure Uygulama Ağ Geçidi kullanarak güvenlik açıkları riskini azaltır. Ek özellikler şunlardır:

- Uçuç-SSL
- TLS v1.0 ve v1.1'i devre dışı
- TLSv1.2 etkinleştirme
- Web uygulaması güvenlik duvarı (önleme modu)
- OWASP 3.0 kural seti ile önleme modu
- Tanılama günlüğünü etkinleştirme
- Özel sağlık sondaları
- Azure Güvenlik Merkezi ve Azure Danışmanı ek koruma ve bildirimler sağlar. Azure Güvenlik Merkezi ayrıca bir itibar sistemi de sağlar.

### <a name="logging-and-auditing"></a>Günlük kaydı ve denetim
Azure hizmetleri, sistem ve kullanıcı etkinliğinin yanı sıra sistem durumunu kapsamlı bir şekilde kaydeder:

- [Etkinlik günlükleri,](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) abonelikteki kaynaklarda gerçekleştirilen işlemlerhakkında bilgi sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşum zamanının ve durumunun belirlenmesine yardımcı olabilir.
- [Tanılama günlükleri,](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından yayılan tüm günlükleri içerir. Bu günlükler Arasında Windows olay sistem günlükleri, Azure Depolama günlükleri, Key Vault denetim günlükleri ve Uygulama Ağ Geçidi erişimi ve Güvenlik Duvarı günlükleri yer alır. Tüm tanılama günlükleri arşivleme için merkezi ve şifreli bir Azure depolama hesabına yazar. Bekletme, kuruluşa özgü bekletme gereksinimlerini karşılamak için 730 güne kadar kullanıcı tarafından yapılandırılabilir.

### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri
Bu günlükler, işleme, depolama ve pano raporlaması için [Azure Monitor günlüklerinde](https://azure.microsoft.com/services/log-analytics/) birleştirilir. Veriler toplandıktan sonra, Log Analytics çalışma alanlarındaki her veri türü için ayrı tablolar halinde düzenlenir ve bu da tüm verilerin orijinal kaynağından bağımsız olarak birlikte analiz edilmesine olanak tanır. Ayrıca Azure Güvenlik Merkezi, müşterilerin güvenlik olay verilerine erişmek ve diğer hizmetlerden gelen verilerle birleştirmek için Kusto sorgularını kullanmasına olanak tanıyan Azure Monitor günlükleriyle tümleşir.

Aşağıdaki Azure [izleme çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) bu mimarinin bir parçası olarak dahildir:

- [Etkin Dizin Değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory Health Check çözümü, sunucu ortamlarının risk ve sistem durumunu düzenli aralıklarla değerlendirir ve dağıtılan sunucu altyapısına özgü öncelikli bir öneri listesi sağlar.
- [SQL Değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL Sistem Durumu Denetimi çözümü, sunucu ortamlarının risk ve sistem durumunu düzenli aralıklarla değerlendirir ve müşterilere dağıtılan sunucu altyapısına özgü öncelikli bir öneri listesi sağlar.
- [Aracı Durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı Sağlık çözümü kaç aracının dağıtılanını ve coğrafi dağılımlarının yanı sıra kaç aracının yanıt vermediğini ve operasyonel veri gönderen aracı sayısını bildirir.
- [Etkinlik Günlüğü Analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Activity Log Analytics çözümü, bir müşteri için tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.

### <a name="azure-monitor"></a>Azure İzleyici
[Azure Monitor,](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kuruluşların Azure kaynaklarında API çağrılarını izleme de dahil olmak üzere, kuruluşların denetim yapmasını, uyarıları oluşturmasını ve verileri arşivlemesini sağlayarak kullanıcıların performansı izlemesine, güvenliği korumasına ve eğilimleri belirlemesine yardımcı olur.

### <a name="application-insights"></a>Application Insights
[Application Insights,](https://docs.microsoft.com/azure/application-insights/app-insights-overview) web geliştiricileri için birden çok platformda genişletilebilir bir Uygulama Performans Yönetimi hizmetidir. Uygulama Öngörüleri performans anormalliklerini algılar ve müşteriler bunu canlı web uygulamasını izlemek için kullanabilir. Müşterilerin sorunları tanılayabamalarına ve kullanıcıların uygulamalarıyla gerçekte ne yaptıklarını anlamalarına yardımcı olan güçlü analiz araçları içerir. Müşterilerin performansı ve kullanılabilirliği sürekli olarak geliştirmelerine yardımcı olmak için tasarlanmıştır.

### <a name="azure-key-vault"></a>Azure Key Vault
Anahtarları depolayabilmek için kuruluş için bir kasa oluşturun ve aşağıdaki gibi operasyonel görevler için hesap verebilirliği koruyun:

- Key Vault'ta depolanan veriler şunları içerir:

   - Uygulama içgörü anahtarı
   - Veri Depolama Erişim anahtarı
   - Bağlantı dizesi
   - Veri tablosu adı
   - Kullanıcı Kimlik Bilgileri

- Gelişmiş erişim ilkeleri gereksinim bazında yapılandırılır
- Key Vault erişim ilkeleri anahtarlar ve sırlar için en az gerekli izinler ile tanımlanır
- Key Vault'taki tüm anahtarlar ve sırlar son kullanma tarihlerine sahiptir
- Key Vault'taki tüm anahtarlar HSM [Anahtar Türü = HSM Korumalı 2048 bit RSA Tuşu] tarafından korunmaktadır.
- Tüm kullanıcılara/kimliklere Rol Tabanlı Erişim Denetimi (RBAC) kullanılarak minimum gerekli izinverilir
- Başvurular, birbirlerine güvenmedikleri sürece Bir Anahtar Kasası'nı paylaşmaz ve çalışma zamanında aynı sırlara erişmelerine ihtiyaç duyarlar.
- Key Vault için tanılama günlükleri en az 365 günlük bir bekletme süresi ile etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanla sınırlıdır

### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bu PaaS web uygulama başvuru mimarisinin bir parçası olarak dağıtılan kaynaklara bir bağlantı kurmak için yapılandırılmalıdır. Uygun şekilde vpn veya ExpressRoute ayarlayarak, müşteriler aktarım sırasındaki veriler için bir koruma katmanı ekleyebilir.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile Azure Sanal Ağı arasında sanal özel bir bağlantı oluşturulabilir. Bu bağlantı Internet üzerinden gerçekleşir ve müşterilerin müşterinin ağı ile Azure arasındaki şifreli bir bağlantı içinde güvenli bir şekilde "tünel" bilgilerini "tünel" sağlar. Siteden Siteye VPN, on yıllardır her boyuttaki işletmeler tarafından dağıtılan güvenli ve olgun bir teknolojidir. Bu seçenekte IPsec tünel modu şifreleme mekanizması olarak kullanılır.

VPN tüneli içindeki trafik, siteden siteye VPN ile Internet'te geçiş yaptığından, Microsoft başka, daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute, Azure ile şirket içi konum veya Exchange barındırma sağlayıcısı arasında özel bir WAN bağlantısıdır. ExpressRoute bağlantıları Internet üzerinden gitmediğinden, bu bağlantılar Internet üzerinden normal bağlantılara göre daha fazla güvenilirlik, daha yüksek hızlar, daha düşük gecikme süreleri ve daha yüksek güvenlik sunar. Ayrıca, bu müşterinin telekomünikasyon sağlayıcısının doğrudan bir bağlantısı olduğundan, veriler Internet üzerinden seyahat etmez ve bu nedenle buna maruz kalmaz.

Şirket içi ağı Azure'a genişleten güvenli bir karma ağ uygulamak için en iyi uygulamalar [kullanılabilir.](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid)

## <a name="cost-considerations"></a>Maliyetle ilgili konular
Zaten bir Azure hesabınız yoksa, ücretsiz bir hesap oluşturabilirsiniz. Başlamak için [ücretsiz hesap sayfasına](https://azure.microsoft.com/free/) gidin, ücretsiz bir Azure hesabıyla neler yapabileceğinizi görün ve hangi ürünlerin 12 ay boyunca ücretsiz olduğunu öğrenin.

Güvenlik özellikleriyle örnek uygulamadaki kaynakları dağıtmak için bazı premium özellikler için ödeme yapmanız gerekir. Uygulama ölçeklendirildikçe ve Azure tarafından sunulan ücretsiz katmanlar ve denemeler uygulama gereksinimlerini karşılamak için yükseltilmesi gerektiğinden, maliyetleriniz artabilir. Maliyetlerinizi tahmin etmek için Azure [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleler, güvenli uygulamalar tasarlamanıza, geliştirmenize ve dağıtmanıza yardımcı olabilir.

- [Tasarım](secure-design.md)
- [Geliştirme](secure-develop.md)
- [Dağıt](secure-deploy.md)
