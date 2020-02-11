---
title: Hizmet sağlayıcıları için Azure Izleyici günlükleri | Microsoft Docs
description: Azure Izleyici günlükleri, yönetilen hizmet sağlayıcılarının (MSPs), büyük kuruluşların, bağımsız yazılım satıcılarının (ISV 'Ler) ve barındırma hizmeti sağlayıcılarının, müşterinin Şirket içi veya bulut altyapısındaki sunucuları yönetmesine ve izlemesine yardımcı olabilir.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: MeirMen
ms.author: meirm
ms.date: 02/03/2020
ms.openlocfilehash: 1b5c181569ea569ecc4808284683501cb20f7bf5
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77117066"
---
# <a name="azure-monitor-logs-for-service-providers"></a>Hizmet sağlayıcıları için Azure Izleyici günlükleri

Azure Izleyici 'deki Log Analytics çalışma alanları, yönetilen hizmet sağlayıcılarının (MSPs), büyük kuruluşların, bağımsız yazılım satıcılarının (ISV) ve barındırma hizmeti sağlayıcılarının, müşterinin Şirket içi veya bulut altyapısındaki sunucuları yönetmesine ve izlemesine yardımcı olabilir.

Büyük kuruluşlar özellikle yönetmekten sorumlu merkezi bir BT ekibiniz olduğunda bu benzer hizmet sağlayıcıları ile paylaşmak için birçok farklı iş birimleri BT. Kolaylık olması için, bu belge *hizmet sağlayıcısını* kullanır, ancak kuruluşlar ve diğer müşteriler için aynı işlevsellik de mevcuttur.

[Bulut çözümü sağlayıcısı (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programının parçası olan iş ortakları ve hizmet sağlayıcıları Için Azure izleyici 'de log ANALYTICS Azure CSP aboneliklerinde bulunan Azure hizmetlerinden biridir.

Azure Izleyici 'de Log Analytics Ayrıca, Azure açık [Thouse](https://docs.microsoft.com/azure/lighthouse/overview)'daki Azure Temsilcili kaynak yönetimi özelliği aracılığıyla müşteri kaynaklarını yöneten bir hizmet sağlayıcısı tarafından da kullanılabilir.

## <a name="architectures-for-service-providers"></a>Hizmet sağlayıcılarına yönelik mimariler

Log Analytics çalışma alanları, yöneticinin [günlük](data-platform-logs.md) verilerinin akışını ve yalıtımının denetlenmesi ve belirli iş ihtiyaçlarını karşılayan bir mimari oluşturması için bir yöntem sağlar. [Bu makalede](design-logs-deployment.md) bir çalışma alanı için tasarım, dağıtım ve geçiş konuları açıklanmakta ve [erişim yönetimi](manage-access.md) makalesi, günlük verilerine yönelik izinlerin nasıl uygulanacağını ve yönetileceğini açıklar. Hizmet sağlayıcıları, ek hususlar vardır.

Log Analytics çalışma alanları ile ilgili hizmet sağlayıcıları için üç olası mimari vardır:

### <a name="1-distributed---logs-are-stored-in-workspaces-located-in-the-customers-tenant"></a>1. dağıtılmış Günlükler müşterinin kiracısında bulunan çalışma alanlarında depolanır

Bu mimaride, bir çalışma alanı söz konusu müşterinin tüm günlükleri için kullanılan bir müşterinin Kiracı dağıtılır.

Hizmet sağlayıcı yöneticilerinin bir müşteri kiracısında bir Log Analytics çalışma alanına erişim sağlayabilmenin iki yolu vardır:

- Müşteri, hizmet sağlayıcısından [Azure Active Directory Konuk Kullanıcı (B2B)](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)olarak bireysel kullanıcılar ekleyebilir. Hizmet sağlayıcı yöneticilerinin, bu çalışma alanlarına erişebilmek için Azure portal her bir müşterinin dizininde oturum açması gerekir. Bu, müşterilerin her bir hizmet sağlayıcı Yöneticisi için bireysel erişimi yönetmesini de gerektirir.
- Hizmet sağlayıcılar, daha fazla ölçeklenebilirlik ve esneklik sağlamak için [Azure](https://docs.microsoft.com/azure/lighthouse/overview) açık [kaynak yönetimi](https://docs.microsoft.com/azure/lighthouse/concepts/azure-delegated-resource-management) özelliğini kullanarak müşterinin kiracısına erişim sağlayabilir. Bu yöntemde, hizmet sağlayıcı yöneticileri hizmet sağlayıcısının kiracısındaki bir Azure AD kullanıcı grubuna dahil edilir ve bu gruba her müşteri için ekleme işlemi sırasında erişim verilir. Bu Yöneticiler daha sonra her bir müşterinin kiracısında tek tek oturum açmasını sağlamak yerine, her bir müşterinin çalışma alanına kendi hizmet sağlayıcısı kiracının içinden erişmesini sağlayabilir. Müşterilerinizin Log Analytics çalışma alanları kaynaklarına bu şekilde erişmek, müşteri tarafında gereken işi azaltır ve [Azure Izleyici çalışma kitapları](https://docs.microsoft.com/azure//azure-monitor/platform/workbooks-overview)gibi araçlarla aynı hizmet sağlayıcısı tarafından yönetilen birden fazla müşteriye veri toplayıp analiz etmeyi kolaylaştırır. Daha fazla bilgi için bkz. [Müşteri kaynaklarını ölçeklendirerek izleme](https://docs.microsoft.com/azure/lighthouse/how-to/monitor-at-scale).

Dağıtılmış mimarinin avantajları şunlardır:

* Müşteri, [Azure tarafından atanan kaynak yönetimi](https://docs.microsoft.com/azure/lighthouse/concepts/azure-delegated-resource-management)aracılığıyla belirli düzeylerde izinleri belirtebilir veya kendi [rol tabanlı erişimini](https://docs.microsoft.com/azure/role-based-access-control/overview)kullanarak günlüklere erişimi yönetebilir.
* Günlükler yalnızca aracı tabanlı VM verilerini değil, tüm kaynak türleri için toplanabilir. Örneğin, Azure denetim günlükleri.
* Her müşteri için kendi çalışma alanı tutma ve veri capping gibi farklı ayarlara sahip olabilir.
* Müşteriler için arasında yalıtım yasal ve uymalarını zorunlu tutar.
* Müşterinin hizmet aboneliği içinde her çalışma alanı için ücret alınacaktır.

Dağıtılmış mimarinin dezavantajları şunlardır:

* Azure Izleyici çalışma kitapları gibi araçlarla verilerin müşteri kiracılarında merkezi olarak görselleştirilmesi ve çözümlenmesi, özellikle 50 ' den fazla çalışma alanındaki verileri analiz edilirken daha yavaş deneyimler oluşmasına neden olabilir.
* Müşteriler, Azure tarafından yetkilendirilen kaynak yönetimi için eklendi değilse, servis sağlayıcı yöneticilerinin müşteri dizininde sağlanması gerekir ve hizmet sağlayıcının aynı anda çok sayıda müşteri kiracıyı yönetmesi daha zordur.

### <a name="2-central---logs-are-stored-in-a-workspace-located-in-the-service-provider-tenant"></a>2. orta Günlükler, hizmet sağlayıcı kiracısında bulunan bir çalışma alanında depolanır

Bu mimaride, günlükler, müşterinin kiracılar, ancak yalnızca bir merkezi konumda hizmet sağlayıcısının aboneliklerden biri içinde depolanmaz. Müşterinin Vm'lere yüklü aracıları çalışma alanı kimliği ve gizli anahtarı kullanarak bu çalışma alanına kendi günlükleri göndermek için yapılandırılır.

Merkezi mimarinin avantajları şunlardır:

* Çok sayıda müşteriler yönetmek ve bunları için çeşitli arka uç sistemleri tümleştirmek kolay bir işlemdir.
* Hizmet sağlayıcısı, günlükler ve işlevleri gibi çeşitli yapıları üzerinde tam sahiplik sahiptir ve sorguları kaydedilir.
* Hizmet sağlayıcısı tüm müşterilerine analiz gerçekleştirebilirsiniz.

Merkezi mimarinin dezavantajları şunlardır:

* Bu mimari yalnızca aracı tabanlı VM verileri için geçerlidir, PaaS, SaaS ve Azure fabric veri kaynakları kapsayan değil.
* Tek bir çalışma alanına birleştirildiğinde müşteriler arasındaki verileri ayrı zor olabilir. Bunu yapmak için en iyi yöntem bilgisayarın tam etki alanı adı (FQDN) kullanmak veya Azure aboneliği kimliği kullanmaktır 
* Tüm müşterilerden gelen tüm veriler tek bir fatura ve aynı saklama ve yapılandırma ayarları ile aynı bölgede depolanır.
* Azure yapısı ve PaaS Hizmetleri kaynak ile aynı kiracıda bu nedenle yönetim çalışma alanına günlükler gönderilemiyor olması için çalışma gibi Azure tanılama ve Azure denetim günlükleri gerektirir.
* Tüm müşterilerin tüm VM aracılarının, aynı çalışma alanı KIMLIĞI ve anahtarı kullanılarak merkezi çalışma alanında kimlik doğrulaması yapılır. Belirli bir müşteri günlüklerinden diğer müşterilerin kesintiye uğratmadan engellemek için bir yöntem yoktur.

### <a name="3-hybrid---logs-are-stored-in-workspace-located-in-the-customers-tenant-and-some-of-them-are-pulled-to-a-central-location"></a>3. karma Günlükler müşterinin kiracısında bulunan çalışma alanında depolanır ve bazıları merkezi bir konuma çekilir.

İki seçenek arasındaki üçüncü mimarisi karışımı. Günlükleri nerededir her müşteri için yerel ilk dağıtılmış mimarisi dayanır ancak günlüklerinin merkezi bir depo oluşturmak için bazı mekanizmasını kullanma. Günlükleri bir kısmı, raporlama ve analiz için merkezi bir konuma çekilir. Bu bölümü, az sayıda veri türleri veya günlük istatistikler gibi etkinliğinin özetini olabilir.

Günlükleri merkezi bir konumda uygulamak için iki seçenek vardır:

1. Merkezi çalışma alanı: hizmet sağlayıcı kiracısında bir çalışma alanı oluşturabilir ve verileri çeşitli çalışma alanlarından bu merkezi konuma getirmek için [veri toplama API](../../azure-monitor/platform/data-collector-api.md) 'Siyle bırlıkte [sorgu API](https://dev.loganalytics.io/) 'sini kullanan bir komut dosyası kullanabilir. Bir komut dosyası dışında başka bir seçenek [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)kullanmaktır.

2. Merkezi bir konum olarak Power BI: Power BI, Log Analytics çalışma alanı ve [Power BI](../../azure-monitor/platform/powerbi.md)arasındaki tümleştirmeyi kullanarak verileri dışa aktardığınızda merkezi konum olarak görev yapabilir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak Yöneticisi şablonlarını](template-workspace-configuration.md) kullanarak çalışma alanlarının oluşturulmasını ve yapılandırılmasını otomatikleştirin

* [PowerShell](../../azure-monitor/platform/powershell-workspace-configuration.md) kullanarak çalışma alanlarının oluşturulmasını otomatikleştirme 

* Mevcut sistemlerle tümleştirme için [uyarıları](../../azure-monitor/platform/alerts-overview.md) kullanma

* [Power BI](../../azure-monitor/platform/powerbi.md) kullanarak özet raporlar oluşturma

* [Azure tarafından atanan kaynak yönetimine](https://docs.microsoft.com/azure/lighthouse/concepts/azure-delegated-resource-management)müşteri ekleme.
