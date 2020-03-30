---
title: Hizmet Sağlayıcılar için Azure Monitör Günlükleri | Microsoft Dokümanlar
description: Azure Monitör Günlükleri, Yönetilen Hizmet Sağlayıcıları (MSP'ler), büyük işletmeler, Bağımsız Yazılım Satıcıları (ISV' ler) ve barındırma hizmeti sağlayıcılarının müşterinin şirket içi veya bulut altyapısındaki sunucuları yönetmesine ve izlemesine yardımcı olabilir.
ms.subservice: logs
ms.topic: conceptual
author: MeirMen
ms.author: meirm
ms.date: 02/03/2020
ms.openlocfilehash: ed398e12ee90f2eef2cfa78e2ed02701e6012517
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658889"
---
# <a name="azure-monitor-logs-for-service-providers"></a>Hizmet Sağlayıcılar için Azure Monitör Günlükleri

Azure Monitor'daki Günlük Analizi çalışma alanları, yönetilen hizmet sağlayıcıların (MSP'ler), büyük işletmelerin, bağımsız yazılım satıcılarının (ISV'ler) ve barındırma hizmet sağlayıcılarının müşterinin şirket içi veya bulut altyapısındaki sunucuları yönetmesine ve izlemesine yardımcı olabilir.

Büyük işletmeler, özellikle birçok farklı iş birimi için BT yönetiminden sorumlu merkezi bir BT ekibi olduğunda, hizmet sağlayıcılarla birçok benzerliği paylaşır. Basitlik için, bu belge *hizmet sağlayıcısı* terimini kullanır, ancak aynı işlevsellik işletmeler ve diğer müşteriler için de kullanılabilir.

[Bulut Çözüm Sağlayıcısı (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programının bir parçası olan iş ortakları ve hizmet sağlayıcıları için Azure Monitor'daki Log Analytics, Azure CSP aboneliklerinde kullanılabilen Azure hizmetlerinden biridir.

Azure Monitor'daki Günlük Analitiği, [Azure Deniz Feneri'ndeki](https://docs.microsoft.com/azure/lighthouse/overview)Azure temsilci kaynak yönetimi özelliği aracılığıyla müşteri kaynaklarını yöneten bir hizmet sağlayıcısı tarafından da kullanılabilir.

## <a name="architectures-for-service-providers"></a>Hizmet Sağlayıcılar için Mimariler

Günlük Analizi çalışma alanları, yöneticinin [günlük](data-platform-logs.md) verilerinin akışını ve yalıtımını denetlemesi ve belirli iş gereksinimlerini karşılayan bir mimari oluşturması için bir yöntem sağlar. [Bu makalede,](design-logs-deployment.md) bir çalışma alanı için tasarım, dağıtım ve geçiş konuları açıklanır ve veri günlüğe kaydetme izinlerinin nasıl uygulanacağı ve yönetilen [erişim](manage-access.md) makalesi açıklanır. Hizmet sağlayıcıların ek değerlendirmeleri vardır.

Log Analytics çalışma alanlarıyla ilgili olarak hizmet sağlayıcılar için üç olası mimari vardır:

### <a name="1-distributed---logs-are-stored-in-workspaces-located-in-the-customers-tenant"></a>1. Dağıtılan - Günlükler müşterinin kiracısında bulunan çalışma alanlarında saklanır

Bu mimaride, müşterinin kiracısında, müşterinin tüm günlükleri için kullanılan bir çalışma alanı dağıtılır.

Hizmet sağlayıcısı yöneticilerinin bir müşteri kiracısında Günlük Analizi çalışma alanına erişmesinin iki yolu vardır:

- Bir müşteri, hizmet sağlayıcısından tek tek kullanıcıları [Azure Active Directory konuk kullanıcıları (B2B)](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)olarak ekleyebilir. Hizmet sağlayıcı yöneticilerin, bu çalışma alanlarına erişebilmek için Azure portalındaki her müşterinin dizininde oturum açmaları gerekir. Bu, müşterilerin her hizmet sağlayıcı yöneticisi için tek tek erişimi yönetmesini de gerektirir.
- Daha fazla ölçeklenebilirlik ve esneklik için, hizmet sağlayıcıları müşterinin kiracısına erişmek için [Azure Deniz Feneri'nin](https://docs.microsoft.com/azure/lighthouse/overview) [Azure temsilci kaynak yönetimi](https://docs.microsoft.com/azure/lighthouse/concepts/azure-delegated-resource-management) özelliğini kullanabilir. Bu yöntemle, hizmet sağlayıcı yöneticiler hizmet sağlayıcısının kiracısında bir Azure AD kullanıcı grubuna dahil edilir ve bu gruba her müşteri için onboarding işlemi sırasında erişim verilir. Bu yöneticiler daha sonra her müşterinin kiracısına ayrı ayrı oturum açmak zorunda kalmak yerine, her müşterinin çalışma alanlarına kendi hizmet sağlayıcısı kiracısından erişebilir. Müşterilerinizin Log Analytics çalışma alanlarına bu şekilde erişmek, müşteri tarafında gereken çalışmayı azaltır ve [Azure Monitor Çalışma Kitapları](https://docs.microsoft.com/azure//azure-monitor/platform/workbooks-overview)gibi araçlar aracılığıyla aynı hizmet sağlayıcısı tarafından yönetilen birden çok müşteri arasında veri toplamayı ve analiz etmeyi kolaylaştırabilir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/lighthouse/how-to/monitor-at-scale)

Dağıtılmış mimarinin avantajları şunlardır:

* Müşteri, [Azure temsilcikaynak yönetimi](https://docs.microsoft.com/azure/lighthouse/concepts/azure-delegated-resource-management)aracılığıyla belirli izin düzeylerini onaylayabilir veya kendi rol [tabanlı erişimini](https://docs.microsoft.com/azure/role-based-access-control/overview)kullanarak günlüklere erişimi yönetebilir.
* Günlükler, yalnızca aracı tabanlı VM verilerinden değil, her türlü kaynaktan toplanabilir. Örneğin, Azure Denetim Günlükleri.
* Her müşterinin çalışma alanı için saklama ve veri kapama gibi farklı ayarları olabilir.
* Mevzuat ve uyumluluk için müşteriler arasında yalıtım.
* Her çalışma alanının ücreti müşterinin aboneliğine kaydedilir.

Dağıtılmış mimarinin dezavantajları şunlardır:

* Azure Monitör Çalışma Kitapları gibi araçlarla müşteri kiracıları arasında verileri merkezi olarak görselleştirmek ve çözümleme, özellikle 50'den fazla çalışma alanında verileri analiz ederken daha yavaş deneyimlere neden olabilir.
* Müşteriler Azure temsilci kaynak yönetimi için yerleşik değilse, hizmet sağlayıcı yöneticilerin müşteri dizininde sağlanması gerekir ve hizmet sağlayıcısının aynı anda çok sayıda müşteri kiracısını yönetmesi daha zordur.

### <a name="2-central---logs-are-stored-in-a-workspace-located-in-the-service-provider-tenant"></a>2. Merkezi - Günlükler, hizmet sağlayıcı kiracıda bulunan bir çalışma alanında saklanır

Bu mimaride, günlükler müşterinin kiracılarında değil, yalnızca servis sağlayıcısının aboneliklerinden birinde merkezi bir konumda depolanır. Müşterinin VM'lerine yüklenen aracılar, çalışma alanı kimliği ve gizli anahtar kullanılarak günlüklerini bu çalışma alanına gönderecek şekilde yapılandırılır.

Merkezi mimarinin avantajları şunlardır:

* Çok sayıda müşteriyi yönetmek ve bunları çeşitli arka uç sistemlerine entegre etmek kolaydır.
* Hizmet sağlayıcısı günlükleri ve işlevler ve kaydedilmiş sorgular gibi çeşitli yapılar üzerinde tam mülkiyete sahiptir.
* Servis sağlayıcı tüm müşterilerine analitik gerçekleştirebilir.

Merkezi mimarinin dezavantajları şunlardır:

* Bu mimari yalnızca aracı tabanlı VM verileri için geçerlidir, PaaS, SaaS ve Azure kumaş veri kaynaklarını kapsamaz.
* Tek bir çalışma alanıyla birleştirildiğinde verileri müşteriler arasında ayırmak zor olabilir. Bunu yapmak için tek iyi yöntem bilgisayarın tam nitelikli etki alanı adı (FQDN) veya Azure abonelik kimliği üzerinden kullanmaktır. 
* Tüm müşterilerden gelen tüm veriler tek bir fatura ve aynı bekletme ve yapılandırma ayarlarıyla aynı bölgede depolanır.
* Azure Tanılama ve Azure Denetim Günlükleri gibi Azure kumaşı ve PaaS hizmetleri, çalışma alanının kaynakla aynı kiracıda olmasını gerektirir, bu nedenle günlükleri merkezi çalışma alanına gönderemezler.
* Tüm müşterilerden gelen tüm VM aracıları, aynı çalışma alanı kimliği ve anahtarı kullanılarak merkezi çalışma alanına doğrulanır. Diğer müşterileri kesintiye uğratmadan belirli bir müşteriden günlükleri engellemek için bir yöntem yoktur.

### <a name="3-hybrid---logs-are-stored-in-workspace-located-in-the-customers-tenant-and-some-of-them-are-pulled-to-a-central-location"></a>3. Hibrit - Günlükler müşterinin kiracısında bulunan çalışma alanında saklanır ve bazıları merkezi bir konuma çekilir.

İki seçenek arasında üçüncü mimari karışımı. Günlüklerin her müşteri için yerel olduğu ancak günlüklerin merkezi bir depo oluşturmak için bazı mekanizmalar kullanılarak ilk dağıtılmış mimariye dayanır. Günlüklerin bir kısmı raporlama ve analiz için merkezi bir konuma çekilir. Bu bölüm az sayıda veri türü veya günlük istatistikler gibi etkinliğin bir özeti olabilir.

Günlükleri merkezi bir konumda uygulamak için iki seçenek vardır:

1. Merkezi çalışma alanı: Hizmet sağlayıcısı kiracısında bir çalışma alanı oluşturabilir ve çeşitli çalışma alanlarındaki verileri bu merkezi konuma getirmek için [Veri Toplama API'si](../../azure-monitor/platform/data-collector-api.md) ile Sorgu [API'sini](https://dev.loganalytics.io/) kullanan bir komut dosyası kullanabilir. Komut dosyası dışında başka bir seçenek de [Azure Logic Apps'ı](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)kullanmaktır.

2. Merkezi bir konum olarak Güç BI: Power BI, çeşitli çalışma alanları veri aktarDığında, Log Analytics çalışma alanı ve [Power BI](../../azure-monitor/platform/powerbi.md)arasındaki tümleştirmeyi kullanarak merkezi konum olarak hareket edebilir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak Yöneticisi şablonlarını](template-workspace-configuration.md) kullanarak çalışma alanlarıoluşturma ve yapılandırmayı otomatikleştirin

* [PowerShell'i](../../azure-monitor/platform/powershell-workspace-configuration.md) kullanarak çalışma alanları oluşturmayı otomatikleştirin 

* Varolan sistemlerle tümleştirmek için [Uyarıları](../../azure-monitor/platform/alerts-overview.md) kullanma

* [Power BI](../../azure-monitor/platform/powerbi.md) kullanarak özet raporlar oluşturma

* Yerleşik müşterilerden [Azure'a yetki nisabı olan kaynak yönetimi.](https://docs.microsoft.com/azure/lighthouse/concepts/azure-delegated-resource-management)
