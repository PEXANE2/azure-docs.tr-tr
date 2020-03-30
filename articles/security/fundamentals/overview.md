---
title: Azure güvenliğine giriş | Microsoft Dokümanlar
description: Azure Güvenlik, hizmetleri ve nasıl çalıştığı hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: rkarlin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2019
ms.author: TomSh
ms.openlocfilehash: a1dbabafe32e013d526ed88a83e446ee765cdb7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76045869"
---
# <a name="introduction-to-azure-security"></a>Azure güvenliğe giriş
## <a name="overview"></a>Genel Bakış
Güvenliğin buluttaki bir iş olduğunu ve Azure güvenliği hakkında doğru ve zamanında bilgi bulmanın ne kadar önemli olduğunu biliyoruz. Azure'u uygulamalarınız ve hizmetleriniz için kullanmanın en iyi nedenlerinden biri, çok çeşitli güvenlik araçlarından ve özelliklerinden yararlanmaktır. Bu araçlar ve özellikler, güvenli Azure platformunda güvenli çözümler oluşturmayı mümkün kılar. Microsoft Azure, müşteri verilerinin gizliliğini, bütünlüğünü ve kullanılabilirliğini sağlarken saydam hesap verebilirliği de sağlar.

Bu makalede, Azure ile kullanılabilir güvenlik kapsamlı bir görünüm sağlar.

### <a name="azure-platform"></a>Azure platformu
Azure, işletim sistemleri, programlama dilleri, çerçeveler, araçlar, veritabanları ve aygıtlardan oluşan geniş bir seçkiyi destekleyen genel bir bulut hizmeti platformudur. Docker entegrasyonu ile Linux konteynerleri çalıştırabilirsiniz; JavaScript, Python, .NET, PHP, Java ve Node.js ile uygulamalar oluşturmak; iOS, Android ve Windows aygıtları için arka uçlar oluşturun.

Azure genel bulut hizmetleri, milyonlarca geliştirici ve BT uzmanının zaten güvendiği ve güvendiği aynı teknolojileri destekler. BT varlıklarını oluşturduğunuzda veya bir genel bulut hizmeti sağlayıcısına aktardığınızda, bulut tabanlı varlıklarınızın güvenliğini yönetmek için sağladıkları hizmetler ve denetimlerle uygulamalarınızı ve verilerinizi koruma yeteneklerine güvendiğiniz bir genel bulut hizmeti sağlayıcısı.

Azure'un altyapısı, milyonlarca müşteriyi aynı anda barındırmak için tesisten uygulamalara kadar tasarlanmıştır ve işletmelerin güvenlik gereksinimlerini karşılayabildiği güvenilir bir temel sağlar.

Buna ek olarak Azure, kuruluşunuzun dağıtımlarının benzersiz gereksinimlerini karşılayacak şekilde güvenliği özelleştirebilmeniz için size çok çeşitli yapılandırılabilir güvenlik seçenekleri ve bunları denetleme olanağı sağlar. Bu belge, Azure güvenlik özelliklerinin bu gereksinimleri karşılamanıza nasıl yardımcı olabileceğini anlamanıza yardımcı olur.

> [!Note]
> Bu belgenin birincil odak noktası, uygulamalarınız ve hizmetleriniz için güvenliği özelleştirmek ve artırmak için kullanabileceğiniz müşteriye yönelik denetimlerdir.
>
> Microsoft'un Azure platformunun kendisini nasıl güvence altına aldığı hakkında bilgi için [Azure altyapı güvenliğine](infrastructure.md)bakın.

## <a name="summary-of-azure-security-capabilities"></a>Azure güvenlik özelliklerinin özeti

### <a name="features-to-secure-the-azure-platform"></a>Azure platformlarını güvenli hale getirmek için özellikler
Aşağıdaki özellikler, Azure Platformu'nun güvenli bir şekilde yönetildiğine dair güvence sağlamak için gözden geçirebileceğiniz özelliklerdir. Bağlantılar, Microsoft'un müşteri güven sorularını dört alanda nasıl ele aldığı konusunda daha ayrıntılı bilgi için sağlanmıştır: güvenli platform, gizlilik & denetimleri, uyumluluk ve şeffaflık.

| [Güvenli Platform](https://www.microsoft.com/trustcenter/Security/default.aspx)  | [Gizlilik & Denetimleri](https://www.microsoft.com/trustcenter/Privacy/default.aspx)  |[Uyumluluk](https://www.microsoft.com/trustcenter/Compliance/default.aspx)   | [Saydamlık](https://www.microsoft.com/trustcenter/Transparency/default.aspx) |
| :-- | :-- | :-- | :-- |
| [Güvenlik Geliştirme Döngüsü](https://www.microsoft.com/sdl/), İç denetimler | [Verilerinizi her zaman yönetme](https://www.microsoft.com/trustcenter/Privacy/You-own-your-data) | [Güven Merkezi](https://www.microsoft.com/trustcenter/default.aspx) |[Microsoft, Azure hizmetlerinde müşteri verilerini nasıl güvence altına alar](https://www.microsoft.com/trustcenter/Transparency/default.aspx) |
| [Zorunlu Güvenlik eğitimi, arka plan kontrolleri](https://downloads.cloudsecurityalliance.org/star/self-assessment/StandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx) |  [Veri konumu denetimi](https://www.microsoft.com/trustcenter/Privacy/Where-your-data-is-located) |  [Ortak Denetimler Merkezi](https://www.microsoft.com/trustcenter/Common-Controls-Hub) |[Microsoft, Azure hizmetlerinde veri konumunu nasıl yönetir?](https://azuredatacentermap.azurewebsites.net/)|
| [Penetrasyon testi,](https://downloads.cloudsecurityalliance.org/star/self-assessment/StandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx) [izinsiz giriş tespiti, DDoS,](https://www.microsoft.com/trustcenter/Security/ThreatManagement) [Denetimler & günlüğe kaydetme](https://www.microsoft.com/trustcenter/Security/AuditingAndLogging) | [Koşullarınızda veri erişimi sağlayın](https://www.microsoft.com/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms) |  [Bulut Hizmetleri Durum Tespiti Kontrol Listesi](https://www.microsoft.com/trustcenter/Compliance/Due-Diligence-Checklist) |[Microsoft'ta kimler verilerinize hangi şartlarda erişebilir?](https://www.microsoft.com/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)|
| [State of the art veri merkezi](https://www.microsoft.com/cloud-platform/global-datacenters), fiziksel güvenlik, Güvenli [Ağ](network-overview.md) | [Kolluk kuvvetlerine yanıt verme](https://www.microsoft.com/trustcenter/Privacy/Responding-to-govt-agency-requests-for-customer-data) |  [Hizmete uygunluk, konum & Endüstri](https://www.microsoft.com/trustcenter/Compliance/default.aspx) |[Microsoft, Azure hizmetlerinde müşteri verilerini nasıl güvence altına alar](https://www.microsoft.com/trustcenter/Transparency/default.aspx)|
|  [Güvenlik Olayı tepkisi](https://aka.ms/SecurityResponsepaper), [Paylaşılan Sorumluluk](https://aka.ms/sharedresponsibility) |[Sıkı gizlilik standartları](https://www.microsoft.com/TrustCenter/Privacy/We-set-and-adhere-to-stringent-standards) |  | [Azure hizmetleri için sertifikayı gözden geçirin, Saydamlık merkezi](https://www.microsoft.com/trustcenter/Compliance/default.aspx)|

### <a name="features-to-secure-data-and-application"></a>Verileri ve uygulamayı güvence altına almak için özellikler
Bulut hizmeti modeline bağlı olarak, uygulama nın veya hizmetin güvenliğini yönetmekten kimin sorumlu olduğu konusunda değişken sorumluluk vardır. Azure Platformu'nda, yerleşik özellikler ve Azure aboneliğine dağıtılabilen iş ortağı çözümleri aracılığıyla bu sorumlulukları karşılamanıza yardımcı olacak özellikler mevcuttur.

Yerleşik özellikler altı işlevsel alanda düzenlenir: Operasyonlar, Uygulamalar, Depolama, Ağ, İşlem ve Kimlik. Bu altı alanda Azure Platformu'nda bulunan özellikler ve özellikler le ilgili ek ayrıntılar özet bilgiler aracılığıyla sağlanır.

## <a name="operations"></a>İşlemler
Bu bölümde, güvenlik işlemlerindeki temel özellikler ve bu özellikler le ilgili özet bilgiler le ilgili ek bilgiler sağlar.

### <a name="security-and-audit-dashboard"></a>Güvenlik ve Denetim Panosu
[Güvenlik ve Denetim çözümü,](../../security-center/security-center-intro.md) dikkatinizi gerektiren önemli sorunlar için yerleşik arama [sorgularıyla](https://blogs.technet.microsoft.com/msoms/2016/01/21/easy-microsoft-operations-management-suite-search-queries/) kuruluşunuzun BT güvenlik duruşuna kapsamlı bir görünüm sağlar. [Güvenlik ve Denetim](https://technet.microsoft.com/library/mt484091.aspx) panosu, Azure Monitor günlüklerinde güvenlikle ilgili her şeyin ana ekranıdır. Bilgisayarlarınızın Güvenlik durumu hakkında üst düzey bilgiler sağlar. Ayrıca son 24 saat, 7 gün veya herhangi bir özel zaman dilimine ait tüm olayları görüntüleme becerisine sahiptir.

Ayrıca, belirli bir olay algılandığında Güvenlik & Uyumluluğu [otomatik olarak belirli eylemleri gerçekleştirecek](https://blogs.technet.microsoft.com/robdavies/2016/04/20/simple-look-at-oms-alert-remediation-with-runbooks-part-1/) şekilde yapılandırabilirsiniz.

### <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Kaynak Yöneticisi,](../../azure-resource-manager/management/deployment-models.md) grup olarak çözümünüzdeki kaynaklarla çalışmanızı sağlar. Çözümünüzdeki tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilirsiniz. Dağıtım için bir [Azure Kaynak Yöneticisi şablonu](https://blogs.technet.microsoft.com/canitpro/2015/06/29/devops-basics-infrastructure-as-code-arm-templates/) kullanıyorsunuz ve bu şablon sınama, hazırlama ve üretim gibi farklı ortamlar için çalışabilir. Resource Manager kaynaklarınızı dağıttıktan sonra yönetmenize yardımcı olmak için güvenlik, denetleme ve etiketleme özellikleri sunar.

Azure Kaynak Yöneticisi şablon tabanlı dağıtımlar, standart güvenlik denetimi ayarları nedeniyle Azure'da dağıtılan çözümlerin güvenliğini artırmaya yardımcı olur ve standart şablon tabanlı dağıtımlara entegre edilebilir. Bu, el ile dağıtımlar sırasında gerçekleşebilecek güvenlik yapılandırma hataları riskini azaltır.

### <a name="application-insights"></a>Application Insights
[Application Insights,](https://docs.microsoft.com/azure/application-insights/) web geliştiricileri için genişletilebilir bir Uygulama Performans Yönetimi (APM) hizmetidir. Application Insights ile canlı web uygulamalarınızı izleyebilir ve performans anormalliklerini otomatik olarak algılayabilirsiniz. Sorunları tanılamanıza ve kullanıcıların uygulamalarınızla gerçekte ne yaptığını anlamanıza yardımcı olacak güçlü analiz araçları içerir. Uygulamanızı hem sınama sırasında hem de yayımladıktan veya dağıttıktan sonra, her zaman çalışır durumda izler.

Application Insights, örneğin, çoğu kullanıcıyı günün hangi saatlerinde aldığınızı, uygulamanın ne kadar duyarlı olduğunu ve bağlı olduğu herhangi bir harici hizmet tarafından ne kadar iyi sunulduğunu gösteren grafikler ve tablolar oluşturur.

Kilitlenmeler, hatalar veya performans sorunları varsa, nedenini tanılamak için telemetri verilerini ayrıntılı olarak arayabilirsiniz. Ayrıca, uygulamanızın kullanılabilirliği ve performansında herhangi bir değişiklik olması durumunda hizmet size e-posta gönderir. Böylece Application Insight, gizlilik, bütünlük ve kullanılabilirlik güvenlik üçlüsündeki kullanılabilirliğe yardımcı olduğu için değerli bir güvenlik aracı haline gelir.

### <a name="azure-monitor"></a>Azure İzleyici
[Azure Monitor,](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) hem Azure altyapısından[(Etkinlik Günlüğü)](../../azure-monitor/platform/platform-logs-overview.md)hem de her bir Azure kaynağından[(Tanılama Günlükleri)](../../azure-monitor/platform/platform-logs-overview.md)veriler üzerinde görselleştirme, sorgu, yönlendirme, uyarı, otomatik ölçek ve otomasyon sunar. Azure günlüklerinde oluşturulan güvenlikle ilgili olaylar konusunda sizi uyarmak için Azure Monitor'u kullanabilirsiniz.

### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri
[Azure Monitor günlükleri](https://azure.microsoft.com/documentation/services/log-analytics/) – Azure kaynaklarına ek olarak hem şirket içi hem de üçüncü taraf bulut tabanlı altyapı (AWS gibi) için BT yönetimi çözümü sağlar. Azure Monitor'dan gelen veriler doğrudan Azure Monitor günlüklerine yönlendirilebilir, böylece tüm ortamınız için ölçümleri ve günlükleri tek bir yerde görebilirsiniz.

Azure Monitor günlükleri, esnek bir sorgu yaklaşımıyla güvenlikle ilgili büyük miktarda girişi hızlı bir şekilde aramanıza olanak sağladığından, adli tıp ve diğer güvenlik çözümlemelerinde yararlı bir araç olabilir. Ayrıca, şirket içi [güvenlik duvarı ve proxy günlükleri Azure'a aktarılabilir ve Azure Monitor günlüklerini kullanarak analiz edilebilebilir.](../../log-analytics/log-analytics-agent-windows.md)

### <a name="azure-advisor"></a>Azure Danışmanı
[Azure Danışmanı,](../../advisor/index.yml) Azure dağıtımlarınızı optimize etmenize yardımcı olan kişiselleştirilmiş bir bulut danışmanıdır. Kaynak yapılandırmanızı ve kullanım telemetrinizi çözümler. Daha [sonra, genel Azure harcamanızı azaltmak](../../advisor/advisor-cost-recommendations.md)için fırsatlar ararken, kaynaklarınızın [performansını,](../../advisor/advisor-performance-recommendations.md) [güvenliğini](../../advisor/advisor-security-recommendations.md)ve [yüksek kullanılabilirliğini](../../advisor/advisor-high-availability-recommendations.md) artırmaya yardımcı olacak çözümler önerir. Azure Danışmanı, Azure'da dağıttEttiğiniz çözümler için genel güvenlik duruşunuzu önemli ölçüde iyileştirebilen güvenlik önerileri sağlar. Bu öneriler, Azure Güvenlik [Merkezi](../../security-center/security-center-intro.md) tarafından gerçekleştirilen güvenlik çözümlemesi ile çizilir.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
[Güvenlik Merkezi,](../../security-center/security-center-intro.md) Azure kaynaklarınızın güvenliğini daha fazla görünürlük ve denetimle tehditleri önlemenize, algılamanıza ve yanıt vermeye yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Buna ek olarak, Güvenlik Merkezi, uyarıları ve önerileri hemen üzerine gerçekleştirebileceğiniz tek bir pano sağlayarak güvenlik operasyonlarına yardımcı olur. Genellikle, Güvenlik Merkezi konsolu içinde tek bir tıklamayla sorunları düzeltebilirsiniz.
## <a name="applications"></a>Uygulamalar
Bu bölümde, uygulama güvenliğindeki temel özellikler ve bu özellikler le ilgili özet bilgiler le ilgili ek bilgiler sağlar.

### <a name="web-application-vulnerability-scanning"></a>Web Uygulaması güvenlik açığı tarama
[Uygulama Hizmeti uygulamanızdaki](../../app-service/overview.md) güvenlik açıklarını test etmeye başlamanın en kolay yollarından biri, uygulamanızda tek tıklamayla güvenlik açığı taraması yapmak için [Tinfoil Security ile tümleştirmeyi](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) kullanmaktır. Test sonuçlarını anlaşılması kolay bir raporda görüntüleyebilir ve her güvenlik açığını adım adım yönergelerle nasıl düzeltebileceğinizi öğrenebilirsiniz.

### <a name="penetration-testing"></a>Penetrasyon Testi
Kendi penetrasyon testlerinizi gerçekleştirmeyi tercih ediyorsanız veya başka bir tarayıcı paketi veya sağlayıcısı kullanmak istiyorsanız, [Azure penetrasyon test onay işlemini](https://docs.microsoft.com/azure/security/fundamentals/pen-testing ) izlemeniz ve istenen penetrasyon testlerini gerçekleştirmek için önceden onay almanız gerekir.

### <a name="web-application-firewall"></a>Web Uygulaması güvenlik duvarı
[Azure Application Gateway'deki](https://azure.microsoft.com/services/application-gateway/) web uygulaması güvenlik duvarı (WAF), web uygulamalarını SQL enjeksiyonu, site ler arası komut dosyası saldırıları ve oturum kaçırma gibi yaygın web tabanlı saldırılara karşı korumaya yardımcı olur. [Açık Web Uygulama Güvenliği Projesi (OWASP)](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)tarafından en yaygın 10 güvenlik açığı olarak tanımlanan tehditlere karşı koruma ile önceden yapılandırılmış olarak gelir.

### <a name="authentication-and-authorization-in-azure-app-service"></a>Azure App Service’de kimlik doğrulaması ve yetkilendirme
[Uygulama Hizmeti Kimlik Doğrulaması / Yetkilendirme,](../../app-service/overview-authentication-authorization.md) uygulamanızın arka ucunda ki kodu değiştirmek zorunda kalmamak için uygulamanızın kullanıcılarda oturum açmasını sağlayan bir özelliktir. Uygulamanızı korumanın ve kullanıcı başına verilerle çalışmanın kolay bir yolunu sağlar.

### <a name="layered-security-architecture"></a>Katmanlı Güvenlik Mimarisi
[Uygulama Hizmet Ortamları](../../app-service/environment/app-service-app-service-environment-intro.md) bir [Azure Sanal Ağı'na](../../virtual-network/virtual-networks-overview.md)dağıtılan yalıtılmış bir çalışma zamanı ortamı sağladığından, geliştiriciler her uygulama katmanı için farklı ağ erişim düzeyleri sağlayan katmanlı bir güvenlik mimarisi oluşturabilir. Yaygın bir istek, API arka uçlarını genel Internet erişiminden gizlemek ve yalnızca API'ların upstream web uygulamaları tarafından çağrılmasını sağlamaktır. [Ağ Güvenliği grupları (NSG'ler),](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/) API uygulamalarına genel erişimi kısıtlamak için Uygulama Hizmeti Ortamları içeren Azure Sanal Ağ alt ağlarında kullanılabilir.

### <a name="web-server-diagnostics-and-application-diagnostics"></a>Web sunucusu tanılama ve uygulama tanılama
App Service web uygulamaları, hem web sunucusundan hem de web uygulamasından bilgi günlüğe kaydetme için tanılama işlevselliği sağlar. Bunlar mantıksal olarak [web sunucusu tanılama](../../app-service/troubleshoot-diagnostic-logs.md) ve [uygulama tanılama](https://technet.microsoft.com/library/hh530058(v=sc.12).aspx)ayrılır. Web sunucusu, tanılama ve sorun giderme sitelerinde ve uygulamalarda iki önemli ilerleme içerir.

İlk yeni özellik, uygulama havuzları, çalışan işlemler, siteler, uygulama etki alanları ve çalışan istekler hakkında gerçek zamanlı durum bilgileridir. İkinci yeni avantaj, tüm istek ve yanıt süreci boyunca bir isteği izleyen ayrıntılı izleme olaylarıdır.

Bu izleme olaylarının toplanmasını etkinleştirmek için, IIS 7, geçen süre veya hata yanıt kodlarına dayalı herhangi bir istek için XML formatında tam izleme günlüklerini otomatik olarak yakalayacak şekilde yapılandırılabilir.

#### <a name="web-server-diagnostics"></a>Web sunucusu tanılama
Aşağıdaki günlük türlerini etkinleştirebilir veya devre dışı kullanabilirsiniz:

-   Ayrıntılı Hata Günlüğü - Bir hatayı gösteren HTTP durum kodları için ayrıntılı hata bilgileri (durum kodu 400 veya daha büyük). Bu, sunucunun hata kodunu neden döndürebileceğini belirlemeye yardımcı olabilecek bilgiler içerebilir.

-   Başarısız İstek İzleme - İstek ve her bileşende alınan süreyi işlemek için kullanılan IIS bileşenlerinin izini içeren başarısız istekler hakkında ayrıntılı bilgi. Bu, site performansını artırmaya veya belirli bir HTTP hatasının döndürülmesine neden olan şeyi yalıtmaya çalışıyorsanız yararlı olabilir.

-   Web Server Logging - W3C genişletilmiş günlük dosya biçimini kullanarak HTTP işlemleri hakkında bilgi. Bu, işlenen istek sayısı veya belirli bir IP adresinden kaç istek olduğu gibi genel site ölçümlerini belirlerken yararlıdır.

#### <a name="application-diagnostics"></a>Uygulama tanılama
[Uygulama tanılama,](../../app-service/troubleshoot-diagnostic-logs.md) bir web uygulaması tarafından üretilen bilgileri yakalamanızı sağlar. ASP.NET uygulamaları, bilgileri uygulama tanılama günlüğüne günlüğe kaydetmek için [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) sınıfını kullanabilir. Uygulama Tanılamada, başlıca iki türde olay vardır: uygulama performansı ile ilgili olanlar ve uygulama arızaları ve hataları ile ilgili olanlar. Arızalar ve hatalar, bağlantı, güvenlik ve arıza sorunları olarak daha da ayrılabilir. Arıza sorunları, genellikle uygulama kodundaki bir sorunla ilgilidir.

Uygulama Tanılamada, bu şekilde gruplandırılmış olayları görebilirsiniz:

-   Tümü (tüm olayları görüntüler)
-   Uygulama Hataları (özel durumları görüntüler)
-   Performans (performans olaylarını görüntüler)

## <a name="storage"></a>Depolama
Bu bölümde, Azure depolama güvenliğindeki temel özellikler ve bu özelliklerle ilgili özet bilgiler le ilgili ek bilgiler sağlanmaktadır.

### <a name="role-based-access-control-rbac"></a>Rol Tabanlı Erişim Denetimi (RBAC)
Rol Tabanlı Erişim Denetimi (RBAC) ile depolama hesabınızı n güvenliğini sağlayabilirsiniz. Veri erişimi için Güvenlik ilkelerini uygulamak isteyen kuruluşlar için, [erişimi bilme ve](https://en.wikipedia.org/wiki/Need_to_know) en az ayrıcalık [güvenlik](https://en.wikipedia.org/wiki/Principle_of_least_privilege) ilkelerine göre kısıtlamak zorunludur. Bu erişim hakları, belirli bir kapsamdaki gruplara ve uygulamalara uygun RBAC rolü atayarak verilir. Kullanıcılara ayrıcalık atamak için Depolama Hesabı Oluşturucusu gibi [yerleşik RBAC rollerini](../../role-based-access-control/built-in-roles.md)kullanabilirsiniz. [Azure Kaynak Yöneticisi](../../storage/blobs/security-recommendations.md) modelini kullanarak bir depolama hesabının depolama anahtarlarına erişim, Role Tabanlı Erişim Denetimi (RBAC) aracılığıyla denetlenebilir.

### <a name="shared-access-signature"></a>Paylaşılan Erişim İmzası
[Paylaşılan erişim imzası (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md), depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. SAS, bir istemciye depolama hesabınızdaki nesnelere belirli bir süre ve belirli bir izin kümesiyle sınırlı izinler verebileceğiniz anlamına gelir. Bu sınırlı izinleri, hesap erişim anahtarlarınızı paylaşmak zorunda kalmadan verebilirsiniz.

### <a name="encryption-in-transit"></a>Aktarımda Şifreleme
Aktarım sırasında şifreleme, ağlar arasında aktarıldığında verileri koruma mekanizmasıdır. Azure Depolama ile verileri şu şekilde güvene alabilirsiniz:
-   Azure Depolama'ya veri aktarırken veya Azure Depolama'nın dışına aktarırken HTTPS gibi [aktarım düzeyinde şifreleme.](../../storage/blobs/security-recommendations.md)

-   Azure Dosya paylaşımları için [SMB 3.0 şifrelemesi](../../storage/blobs/security-recommendations.md) gibi [kablo şifrelemesi.](../../storage/blobs/security-recommendations.md) [Azure File shares](../../storage/files/storage-dotnet-how-to-use-files.md)

-   İstemci tarafı şifreleme, depolamaya aktarılmadan önce verileri şifrelemek ve depolama dışına aktarıldıktan sonra verilerin şifresini çözmek için.

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
Birçok kuruluş için veri şifreleme, veri gizliliği, uyumluluk ve veri egemenliği yolunda zorunlu bir adımdır. "Beklemede" verilerin şifrelemesini sağlayan üç Azure depolama güvenlik özelliği vardır:

-   [Depolama Hizmeti](../../storage/common/storage-service-encryption.md) Şifrelemesi, depolama hizmetinin verileri Azure Depolama'ya yazarken otomatik olarak şifrelemesini istemenize olanak tanır.

-   [İstemci tarafı Şifreleme](../../storage/common/storage-client-side-encryption.md) de istirahatşifreleme özelliği sağlar.

-   [Azure Disk Şifreleme,](../azure-security-disk-encryption-overview.md) IaaS sanal makinesi tarafından kullanılan işletim sistemi disklerini ve veri disklerini şifrelemenize olanak tanır.

### <a name="storage-analytics"></a>Depolama Analizi
[Azure Depolama Analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) günlüğe kaydetme yapar ve bir depolama hesabı için metrik verileri sağlar. Bu verileri istekleri izlemek, kullanım eğilimlerini çözümlemek ve depolama hesabınızla ilgili sorunları tanılamak için kullanabilirsiniz. Depolama Analizi, bir depolama cihazına gönderilen başarılı ve başarısız isteklerle ilgili ayrıntılı bilgileri günlüğe kaydeder. Bu bilgileri kullanarak istekleri ayrı ayrı izleyebilir ve depolama hizmetiyle ilgili sorunları tanılayabilirsiniz. İstekler en iyi çaba temelinde günlüğe kaydedilir. Aşağıdaki türden kimliği doğrulanmış istekler kaydedilir:
-   Başarılı istekler.

-   Zaman acısı, azaltma, ağ, yetkilendirme ve diğer hatalar da dahil olmak üzere başarısız istekler.

-   Başarısız ve başarılı istekler de dahil olmak üzere Paylaşılan Erişim İmzası (SAS) kullanan istekler.

-   Analitik verilere istekler.

### <a name="enabling-browser-based-clients-using-cors"></a>CORS Kullanarak Tarayıcı Tabanlı İstemleri Etkinleştirme
[Çapraz Kaynak Paylaşımı (CORS),](https://docs.microsoft.com/rest/api/storageservices/fileservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) etki alanlarının birbirlerinin kaynaklarına erişmelerine izin vermesini sağlayan bir mekanizmadır. Kullanıcı Aracısı, belirli bir etki alanından yüklenen JavaScript kodunun başka bir etki alanında bulunan kaynaklara erişmesine izin verilmesi için ek üstbilgiler gönderir. İkinci etki alanı daha sonra, özgün etki alanının kaynaklarına erişmesine izin veren veya reddeden ek üstbilgilerle yanıt verir.

Azure depolama hizmetleri artık CORS'u destekler, böylece hizmet için CORS kurallarını ayarladığınızda, hizmete karşı farklı bir etki alanından yapılan doğru kimlik doğrulaması isteği, belirttiğiniz kurallara göre izin verilip verilmediğini belirlemek için değerlendirilir.

## <a name="networking"></a>Ağ Oluşturma
Bu bölümde, Azure ağ güvenliğindeki temel özellikler ve bu özelliklerle ilgili özet bilgiler le ilgili ek bilgiler sağlanmaktadır.

### <a name="network-layer-controls"></a>Ağ Katmanı Denetimleri
Ağ erişim denetimi, belirli aygıtlara veya alt ağlara ve bağlantının sınırlandırma eylemidir ve ağ güvenliğinin çekirdeğini temsil eder. Ağ erişimi denetiminin amacı, sanal makinelerinizin ve hizmetlerinizin yalnızca erişilebilir olmasını istediğiniz kullanıcılar ve aygıtlar tarafından erişilebilir olduğundan emin olmaktır.

#### <a name="network-security-groups"></a>Ağ Güvenlik Grupları
[Ağ Güvenlik Grubu (NSG),](../../virtual-network/virtual-network-vnet-plan-design-arm.md) güvenlik duvarLarını filtreleyen temel bir durum lu pakettir ve [5-tuple'a](https://www.techopedia.com/definition/28190/5-tuple)dayalı erişimi denetlemenizi sağlar. NSG'ler uygulama katmanı denetimi veya kimlik doğrulaması erişim denetimleri sağlamaz. Azure Sanal Ağı'ndaki alt ağlar arasında hareket eden trafiği ve Bir Azure Sanal Ağı ile Internet arasındaki trafiği denetlemek için kullanılabilirler.

#### <a name="route-control-and-forced-tunneling"></a>Rota Kontrolü ve Zorunlu Tünel
Azure Sanal Ağlarınızda yönlendirme davranışını denetleme yeteneği, kritik bir ağ güvenliği ve erişim denetimi yeteneğidir. Örneğin, Azure Sanal Ağınıza gelen ve gelen tüm trafiğin bu sanal güvenlik cihazından geçtiğinden emin olmak istiyorsanız, yönlendirme davranışını kontrol edebilmeniz ve özelleştirebilmeniz gerekir. Bunu, Azure'da Kullanıcı Tanımlı Rotalar'ı yapılandırarak yapabilirsiniz.

[Kullanıcı Tanımlı Rotalar,](../../virtual-network/virtual-networks-udr-overview.md) mümkün olan en güvenli rotayı sigortalamak için tek tek sanal makinelere veya alt ağlara giren ve çıkan trafik için gelen ve giden yolları özelleştirmenize olanak tanır. [Zorunlu tünelleme,](https://www.petri.com/azure-forced-tunneling) hizmetlerinizin Internet'teki aygıtlara bağlantı başlatmasına izin verilmemesini sağlamak için kullanabileceğiniz bir mekanizmadır.

Bu, gelen bağlantıları kabul edip bunlara yanıt verebilmekten farklıdır. Ön uç web sunucuları Internet ana bilgisayarlarından gelen isteklere yanıt vermek gerekir, bu nedenle Internet kaynaklı trafik bu web sunucuları gelen izin verilir ve web sunucuları yanıt verebilir.

Zorunlu tünelleme genellikle internete giden trafiği şirket içi güvenlik vekilleri ve güvenlik duvarlarından geçmeye zorlamak için kullanılır.

#### <a name="virtual-network-security-appliances"></a>Sanal Ağ Güvenlik Aletleri
Ağ Güvenlik Grupları, Kullanıcı Tanımlı Rotalar ve zorunlu tünelleme, [OSI modelinin](https://en.wikipedia.org/wiki/OSI_model)ağ ve aktarım katmanlarında bir güvenlik düzeyi sağlarken, yığının daha yüksek düzeylerinde güvenliği etkinleştirmek istediğiniz zamanlar olabilir. Azure iş ortağı ağ güvenlik cihazı çözümlerini kullanarak bu gelişmiş ağ güvenliği özelliklerine erişebilirsiniz. [Azure Marketi'ni](https://azure.microsoft.com/marketplace/) ziyaret ederek ve "güvenlik" ve "ağ güvenliği" araması yaparak en güncel Azure iş ortağı ağ güvenliği çözümlerini bulabilirsiniz.

### <a name="azure-virtual-network"></a>Azure Sanal Ağ
Azure Virtual Network (VNet) buluttaki kendi ağınızın bir gösterimidir. Aboneliğinize özel Azure ağ dokusunun mantıksal bir yalıtımıdır. Bu ağ içindeki IP adres bloklarını, DNS ayarlarını, güvenlik ilkelerini ve yol tablolarını tam olarak denetleyebilirsiniz. VNet'inizi alt ağlara bölebilir ve Azure Sanal Ağlar'a Azure IaaS sanal makineleri (VM' ler) ve/veya [Bulut hizmetlerini (PaaS rol örnekleri)](../../cloud-services/cloud-services-choose-me.md) yerleştirebilirsiniz.

Bunun yanı sıra, Azure'ın sunduğu [bağlantı seçeneklerinden](../../vpn-gateway/index.yml) birini kullanarak sanal ağı şirket içi ağınıza bağlayabilirsiniz. Özetle, IP adres blokları üzerinde tam bir kontrol sahibi olarak ve Azure'ın sunduğu kurumsal ölçek avantajıyla, ağınızı Azure'a genişletebilirsiniz.

Azure ağ çeşitli güvenli uzaktan erişim senaryolarını destekler. Bunlardan bazıları:

-   [Tek tek iş istasyonlarını azure sanal ağına bağlama](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

-   [Şirket içi ağı VPN ile Azure Sanal Ağına bağlayın](../../vpn-gateway/vpn-gateway-about-vpngateways.md)

-   [Şirket içi ağı özel bir WAN bağlantısıyla Azure Sanal Ağına bağlayın](../../expressroute/expressroute-introduction.md)

-   [Azure Sanal Ağları birbirine bağlayın](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

### <a name="vpn-gateway"></a>VPN Gateway
Azure Sanal Ağınız ile şirket içi siteniz arasında ağ trafiği göndermek için Azure Sanal Ağınız için bir VPN ağ geçidi oluşturmanız gerekir. [VPN ağ geçidi,](../../vpn-gateway/vpn-gateway-about-vpngateways.md) genel bağlantı üzerinden şifreli trafik gönderen bir sanal ağ ağ geçidi türüdür. Azure Sanal Ağlar arasında Azure ağ dokusu üzerinden trafik göndermek için VPN ağ geçitlerini de kullanabilirsiniz.

### <a name="express-route"></a>Express Route
Microsoft Azure [ExpressRoute,](../../expressroute/expressroute-introduction.md) bir bağlantı sağlayıcısı tarafından kolaylaştırılabilen özel bir özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenize olanak tanıyan özel bir WAN bağlantısıdır.

![Express Route](./media/overview/azure-security-fig1.png)

ExpressRoute ile Microsoft Azure, Office 365 ve CRM Online gibi Microsoft bulut hizmetlerine bağlantı kurabilirsiniz. Ortak yerleşim tesisinde bağlantı sağlayıcısı üzerinden herhangi bir ağdan herhangi bir ağa (IP VPN), noktadan noktaya Ethernet ağı veya sanal çapraz bağlantısından bağlantı olabilir. 

ExpressRoute bağlantıları genel Internet üzerinden gitmez ve bu nedenle VPN tabanlı çözümlerden daha güvenli olarak kabul edilebilir. Bu, ExpressRoute bağlantılarına İnternet üzerindeki sıradan bağlantılara göre daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantılardan daha yüksek güvenlik sağlar.


### <a name="application-gateway"></a>Application Gateway
Microsoft [Azure Uygulama Ağ Geçidi,](../../application-gateway/overview.md) uygulamanız için çeşitli katman 7 yük dengeleme özellikleri sunan bir Uygulama Teslim Denetleyicisi [(ADC)](https://en.wikipedia.org/wiki/Application_delivery_controller) hizmeti sağlar.

![Application Gateway](./media/overview/azure-security-fig2.png)

CPU yoğun SSL sonlandırmasını Uygulama Ağ Geçidi'ne ("SSL boşaltma" veya "SSL köprüleme" olarak da bilinir) boşaltarak web çiftliği üretkenliğini optimize etmenizi sağlar. Ayrıca, gelen trafiğin round-robin dağıtımı, çerez tabanlı oturum yakınlığı, URL yol tabanlı yönlendirme ve tek bir Uygulama Ağ Geçidi arkasında birden çok web sitesi barındırma olanağı da dahil olmak üzere diğer Katman 7 yönlendirme özellikleri sağlar. Azure Application Gateway, bir katman 7 yük dengeleyicidir.

Bulutta veya şirket içinde olmalarından bağımsız olarak, farklı sunucular arasında yük devretme ile HTTP istekleri için performans amaçlı yönlendirme sağlar.

Uygulama, HTTP yük dengeleme, çerez tabanlı oturum yakınlığı, [Güvenli Soketkatmanı (SSL)](../../application-gateway/tutorial-restrict-web-traffic-powershell.md) boşaltma, özel sistem sondaları, çoklu site desteği ve diğerleri dahil olmak üzere birçok Uygulama Teslim Denetleyicisi (ADC) özellikleri sağlar.

### <a name="web-application-firewall"></a>Web Uygulaması Güvenlik Duvarı
Web Application Firewall, standart Uygulama Teslim Denetimi (ADC) işlevleri için uygulama ağ geçidi kullanan web uygulamalarına koruma sağlayan [Azure Uygulama Ağ Geçidi'nin](../../application-gateway/overview.md) bir özelliğidir. Web uygulaması güvenlik duvarı bunu, uygulamaları OWASP tarafından sunulan en yaygın 10 web güvenlik açığının çoğuna karşı koruyarak gerçekleştirir.

![Web Uygulaması Güvenlik Duvarı](./media/overview/azure-security-fig1.png)

-   SQL ekleme koruması

-   Komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme saldırıcı gibi Yaygın Web Saldırıları Koruması

-   HTTP protokolü ihlallerine karşı koruma

-   Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma

-   Robotlar, gezginler ve tarayıcıları önleme

-   Yaygın uygulama yanlış yapılandırmalarının algılanması (diğer bir şekilde Apache, IIS, vb.)

Web saldırılarına karşı korunacak merkezi bir web uygulaması, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim tehditlerine karşı uygulamayı daha güvende tutar. Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.

### <a name="traffic-manager"></a>Traffic Manager
Microsoft [Azure Trafik Yöneticisi,](../../traffic-manager/traffic-manager-overview.md) farklı veri merkezlerinde hizmet bitiş noktaları için kullanıcı trafiğinin dağıtımını denetlemenize olanak tanır. Trafik Yöneticisi tarafından desteklenen hizmet bitiş noktaları arasında Azure VM'leri, Web Uygulamaları ve Bulut hizmetleri yer almaktadır. Traffic Manager’ı Azure olmayan dış uç noktalarla da kullanabilirsiniz. Trafik Yöneticisi, [trafik yönlendirme yöntemini](../../traffic-manager/traffic-manager-routing-methods.md) ve uç noktaların durumunu temel alan, istemci isteklerini en uygun bitiş noktasına yönlendirmek için Alan Adı Sistemi'ni (DNS) kullanır.

Trafik Yöneticisi, farklı uygulama gereksinimlerine, uç nokta sağlık [denetimine](../../traffic-manager/traffic-manager-monitoring.md)ve otomatik arızaya uygun bir dizi trafik yönlendirme yöntemi sağlar. Traffic Manager, bir Azure bölgesinin tamamının devre dışı kalması dahil olmak üzere hatalara dayanıklıdır.

### <a name="azure-load-balancer"></a>Azure Load Balancer
[Azure Yük Dengeleyici,](../../load-balancer/load-balancer-overview.md) uygulamalarınız için yüksek kullanılabilirlik ve ağ performansı sağlar. Gelen trafiği, yük dengeli bir kümede tanımlanan sağlıklı hizmet örnekleri arasında dağıtan katman 4 (TCP, UDP) yük dengeleyicisidir. Azure Yük Dengeleyicisi şu şekilde yapılandırılabilir:

-   Sanal makinelere gelen internet trafiğini yükleyin. Bu [yapılandırma, Internet'e bakan yük dengelemesi](../../load-balancer/concepts-limitations.md#publicloadbalancer)olarak bilinir.

-   Sanal ağdaki sanal makineler arasındaki trafiği, bulut hizmetlerindeki sanal makineler arasında veya şirket içi bilgisayarlar ile sanal makineler arasında çapraz sanal ağdaki trafiği yükleyin. Bu yapılandırma [iç yük dengeleme](../../load-balancer/concepts-limitations.md#internalloadbalancer)olarak bilinir.

- Dış trafiği belirli bir sanal makineye iletme

### <a name="internal-dns"></a>Dahili DNS
Yönetim Portalı'ndaki bir VNet'te veya ağ yapılandırma dosyasında kullanılan DNS sunucularının listesini yönetebilirsiniz. Müşteri her VNet için en fazla 12 DNS sunucusu ekleyebilir. DNS sunucularını belirtirken, müşterinin DNS sunucularını müşterinin ortamı için doğru sırada listelediğinizi doğrulamanız önemlidir. DNS sunucu listeleri round-robin çalışmaz. Belirtilen sırayla kullanılırlar. Listedeki ilk DNS sunucusuna ulaşılabiliyorsa, istemci, DNS sunucusunun düzgün çalışıp çalışmadığına bakılmaksızın bu DNS sunucusunu kullanır. Müşterinin sanal ağı için DNS sunucu siparişini değiştirmek için, DNS sunucularını listeden kaldırın ve müşterinin istediği sıraya geri ekleyin. DNS "CIA" güvenlik üçlüsü kullanılabilirlik yönünü destekler.

### <a name="azure-dns"></a>Azure DNS
[Alan Adı Sistemi](https://technet.microsoft.com/library/bb629410.aspx)veya DNS, bir web sitesinin veya hizmet adının IP adresine çevrilme (veya çözümlenmesinden) sorumludur. [Azure DNS,](../../dns/dns-overview.md) Microsoft Azure altyapılarını kullanarak ad çözümlemesi sağlayan DNS etki alanları için bir barındırma hizmetidir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz. DNS "CIA" güvenlik üçlüsü kullanılabilirlik yönünü destekler.

### <a name="azure-monitor-logs-nsgs"></a>Azure Monitör, NSG'leri günlükler
NSG'ler için aşağıdaki tanılama günlüğü kategorilerini etkinleştirebilirsiniz:

-   Olay: NSG kurallarının MAC adresine dayalı VM'lere ve örnek rollerine uygulandığı girişleri içerir. Bu kuralların durumu her 60 saniyede bir toplanır.

-   Kurallar sayacı: Trafiği reddetmek veya izin vermek için her NSG kuralının kaç kez uygulandığına yönelik girişleri içerir.

### <a name="security-center"></a>Güvenlik Merkezi
[Azure Güvenlik Merkezi,](../../security-center/security-center-intro.md) ağ güvenliği en iyi uygulamaları için Azure kaynaklarınızın güvenlik durumunu sürekli olarak analiz eder. Güvenlik Merkezi olası güvenlik açıklarını tanımladığında, kaynaklarınızı sertleştirmek ve korumak için gerekli denetimleri yapılandırma sürecinde size rehberlik eden [öneriler](../../security-center/security-center-recommendations.md) oluşturur.

## <a name="compute"></a>İşlem
Bölüm, bu alandaki temel özellikler le ilgili ek bilgiler ve bu özellikler hakkında özet bilgiler sağlar.

### <a name="antimalware--antivirus"></a>Antimalware & Antivirüs
Azure IaaS ile, sanal makinelerinizi kötü amaçlı dosyalara, reklam yazılımlarına ve diğer tehditlerden korumak için Microsoft, Symantec, Trend Micro, McAfee ve Kaspersky gibi güvenlik satıcılarının kötü amaçlı yazılımdan koruma yazılımlarını kullanabilirsiniz. Azure Bulut Hizmetleri ve Sanal Makineler için [Microsoft Kötü Amaçlı Yazılımdan Koruma,](antimalware.md) virüsleri, casus yazılımları ve diğer kötü amaçlı yazılımları tanımlamaya ve kaldırmaya yardımcı olan bir koruma yeteneğidir. Microsoft Antimalware, bilinen kötü amaçlı veya istenmeyen yazılımlar azure sistemlerinizde kendisini yüklemeye veya çalıştırmaya çalıştığında yapılandırılabilir uyarılar sağlar. Microsoft Antimalware, Azure Güvenlik Merkezi kullanılarak da dağıtılabilir

### <a name="hardware-security-module"></a>Donanım Güvenliği Modülü
Şifreleme ve kimlik doğrulama, anahtarlar korunmadıkça güvenliği artırmaz. Kritik sırlarınızın ve anahtarlarınızın yönetimini ve güvenliğini Azure [Key Vault'ta](../../key-vault/key-vault-overview.md)saklayarak basitleştirebilirsiniz. Key Vault, anahtarlarınızı FIPS 140-2 Düzey 2 standartlarına göre onaylanmış donanım Güvenlik modüllerinde (HSM' ler) saklama seçeneği sunar. Yedekleme veya saydam veri [şifreleme](https://msdn.microsoft.com/library/bb934049.aspx) için SQL Server şifreleme anahtarlarınızın tümü, uygulamalarınızdan gelen tüm anahtarlar veya sırlar ile Birlikte Key Vault'ta saklanabilir. İzinler ve bu korumalı öğelere erişim [Azure Etkin Dizin](https://azure.microsoft.com/documentation/services/active-directory/)aracılığıyla yönetilir.

### <a name="virtual-machine-backup"></a>Sanal makine yedekleme
[Azure Yedekleme,](../../backup/backup-overview.md) uygulama verilerinizi sıfır sermaye yatırımı ve minimum işletme maliyetiyle koruyan bir çözümdür. Uygulama hataları verilerinizi bozabilir ve insan hataları uygulamalarınıza güvenlik sorunlarına yol açabilecek hatalar alabilirsiniz. Azure Yedekleme ile Windows ve Linux çalıştıran sanal makineleriniz korunur.

### <a name="azure-site-recovery"></a>Azure Site Recovery
Kuruluşunuzun [iş sürekliliği/olağanüstü durum kurtarma (BCDR)](../../best-practices-availability-paired-regions.md) stratejisinin önemli bir bölümü, planlı ve planlanmamış kesintiler meydana geldiğinde kurumsal iş yüklerini ve uygulamaları nasıl çalışır durumda tutacağınızı bulmaktır. [Azure Site Kurtarma,](../../site-recovery/site-recovery-overview.md) birincil konumunuz çöktürülmede ikincil bir konumdan kullanılabilmeleri için iş yüklerinin ve uygulamaların çoğaltılması, başarısız olma ve kurtarma işleminin düzenlenmesine yardımcı olur.

### <a name="sql-vm-tde"></a>SQL VM TDE
[Saydam veri şifreleme (TDE)](../../virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault.md) ve sütun düzeyi şifreleme (CLE) SQL sunucu şifreleme özellikleridir. Bu şifreleme biçimi, müşterilerin şifreleme için kullandığınız şifreleme anahtarlarını yönetmesini ve depolamasını gerektirir.

Azure Key Vault (AKV) hizmeti, bu anahtarların güvenliğini ve yönetimini güvenli ve yüksek kullanılabilir bir konumda geliştirmek için tasarlanmıştır. SQL Server Bağlayıcısı, SQL Server'ın bu anahtarları Azure Key Vault'tan kullanmasını sağlar.

Sql Server'ı şirket içi makinelerle çalıştırıyorsanız, şirket içi SQL Server makinenizden Azure Key Vault'a erişmek için izleyebileceğiniz adımlar vardır. Ancak Azure VM'lerde SQL Server için Azure Anahtar Kasası Tümleştirme özelliğini kullanarak zamandan tasarruf edebilirsiniz. Bu özelliği etkinleştirmek için birkaç Azure PowerShell cmdlet ile, bir SQL VM'nin anahtar kasanıza erişebilmesi için gerekli yapılandırmayı otomatikleştirebilirsiniz.

### <a name="vm-disk-encryption"></a>VM Disk Şifreleme
[Azure Disk Şifreleme,](../azure-security-disk-encryption-overview.md) Windows ve Linux IaaS sanal makine disklerinizi şifrelemenize yardımcı olan yeni bir özelliktir. İşletim sistemi ve veri diskleri için birim şifreleme sağlamak için Windows'un endüstri standardı BitLocker özelliğini ve Linux'un DM-Crypt özelliğini uygular. Çözüm, Key Vault aboneliğinizdeki disk şifreleme anahtarlarını ve sırlarını kontrol etmenize ve yönetmenize yardımcı olmak için Azure Key Vault ile entegre edilmiştir. Çözüm ayrıca, sanal makine diskleri üzerindeki tüm verilerin Azure depolama alanınızda dinlenime de şifrelenmesini sağlar.

### <a name="virtual-networking"></a>Sanal ağ
Sanal makinelerin ağ bağlantısına ihtiyacı vardır. Bu gereksinimi desteklemek için Azure, sanal makinelerin bir Azure Sanal Ağına bağlanmasını gerektirir. Azure Sanal Ağ, fiziksel Azure ağ dokusunun üzerine inşa edilmiş mantıksal bir yapıdır. Her mantıksal [Azure Sanal Ağı](../../virtual-network/virtual-networks-overview.md) diğer tüm Azure Sanal Ağlarından izole edilmiştir. Bu yalıtım, dağıtımlarınızdaki ağ trafiğinin diğer Microsoft Azure müşterileri tarafından erişilemeyeceğini niçin sigortalatamanıza yardımcı olur.

### <a name="patch-updates"></a>Yama Güncellemeleri
Yama Güncelleştirmeleri, hem işletmenizde dağıtmanız gereken yazılım güncelleştirmelerinin sayısını azaltarak hem de uyumluluğu izleme yeteneğinizi artırarak olası sorunları bulma ve düzeltme ve yazılım güncelleştirme yönetimi sürecini basitleştirme temelini oluşturur.

### <a name="security-policy-management-and-reporting"></a>Güvenlik politikası yönetimi ve raporlama
[Güvenlik Merkezi,](../../security-center/security-center-intro.md) tehditleri önlemenize, algılamanıza ve yanıt vermenize yardımcı olur ve Azure kaynaklarınızın güvenliği nde daha fazla görünürlük ve denetim sağlar. Azure aboneliklerinizde tümleşik Güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde fark edilmeyecek tehditleri algılamaya yardımcı olur ve geniş bir güvenlik çözümleri ekosistemiyle çalışır.

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi
Sistemleri, uygulamaları ve verileri güvence altına almak kimlik tabanlı erişim denetimleriyle başlar. Microsoft iş ürünleri ve hizmetlerinde yerleşik olan kimlik ve erişim yönetimi özellikleri, kuruluş ve kişisel bilgilerinizi yetkisiz erişimden korurken, yasal kullanıcıların her zaman ve her nerede olurlarsa olsunlar kullanımına sunulmasını sağlar buna ihtiyacım var.

### <a name="secure-identity"></a>Güvenli Kimlik
Microsoft, kimlik ve erişimi yönetmek için ürünleri ve hizmetleri genelinde birden çok güvenlik uygulaması ve teknolojisi kullanır.

-   [Çok Faktörlü Kimlik Doğrulama,](https://azure.microsoft.com/services/multi-factor-authentication/) kullanıcıların çevrimiçi ve buluta erişmek için birden çok yöntem kullanmasını gerektirir. Bir dizi kolay doğrulama seçeneğiyle güçlü kimlik doğrulaması sağlarken, kullanıcıları basit bir oturum açma işlemiyle karşılar.

-   [Microsoft Authenticator,](https://aka.ms/authenticator) hem Microsoft Azure Etkin Dizini hem de Microsoft hesaplarıyla çalışan ve giyilebilir cihazlar ve parmak izi tabanlı onaylar için destek içeren kullanıcı dostu Çok Faktörlü Kimlik Doğrulama deneyimi sağlar.

-   [Parola ilkesi zorlama,](https://azure.microsoft.com/documentation/articles/active-directory-passwords-policy/) parola ve karmaşıklık gereksinimlerini, zorunlu periyodik döndürmeyi ve başarısız kimlik doğrulama denemelerinden sonra hesap kilitlemeyi dayatarak geleneksel parolaların güvenliğini artırır.

-   [Belirteç tabanlı kimlik doğrulama,](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/) Azure Active Directory aracılığıyla kimlik doğrulamasına olanak tanır.

-   [Rol tabanlı erişim denetimi (RBAC),](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/) kullanıcının atanan rolüne bağlı olarak erişim izni vermenizi sağlayarak kullanıcılara yalnızca iş görevlerini yerine getirmeleri için gereken erişim miktarını vermenizi kolaylaştırır. RBAC'ı kuruluşunuzun iş modeline ve risk toleransına göre özelleştirebilirsiniz.

-   [Tümleşik kimlik yönetimi (karma kimlik),](https://azure.microsoft.com/documentation/articles/active-directory-hybrid-identity-design-considerations-overview/) dahili veri merkezleri ve bulut platformları arasında kullanıcıların erişimini kontrol etmenizi sağlayarak kimlik doğrulama ve tüm kaynaklara yetkilendirme için tek bir kullanıcı kimliği oluşturmanıza olanak tanır.

### <a name="secure-apps-and-data"></a>Güvenli Uygulamalar ve veriler
Kapsamlı bir kimlik ve erişim yönetimi bulut çözümü olan [Azure Active Directory,](https://azure.microsoft.com/services/active-directory/)sitedeki ve buluttaki uygulamalardaki verilere erişimi güvence altına almaya yardımcı olur ve kullanıcıların ve grupların yönetimini kolaylaştırır. Temel dizin hizmetleri, gelişmiş kimlik yönetimi, güvenlik ve uygulama erişim yönetimini bir araya getirir ve geliştiricilerin uygulamalarında ilke tabanlı kimlik yönetimi oluşturmalarını kolaylaştırır. Azure Active Directory'nizi geliştirmek için Azure Active Directory Temel, Premium P1 ve Premium P2 sürümleriyle ücretli özellikler ekleyebilirsiniz.

| Ücretsiz / Ortak Özellikler     | Temel Özellikler    |Premium P1 Özellikleri |Premium P2 Özellikleri | Azure Active Directory Join – Windows 10 yalnızca ilgili özellikler|
| :------------- | :------------- |:------------- |:------------- |:------------- |
|   [Dizin Nesneleri](../../active-directory/active-directory-whatis.md), [Kullanıcı/Grup Yönetimi (ekle/güncelleme/silme)/ Kullanıcı tabanlı sağlama, Aygıt kaydı](../../active-directory/active-directory-whatis.md), Tek Oturum Açma [(SSO)](../../active-directory/active-directory-whatis.md), [Bulut kullanıcıları için Self Servis Şifre Değişikliği](../../active-directory/active-directory-whatis.md), Connect [(Şirket içi dizinleri Azure Active Directory'ye kadar uzanan eşitleme motoru)](../../active-directory/active-directory-whatis.md), [Güvenlik / Kullanım Raporları](../../active-directory/active-directory-whatis.md)       |   [Grup tabanlı erişim yönetimi / sağlama](../../active-directory/active-directory-whatis.md), Bulut kullanıcıları için [Self-Servis Şifre Sıfırlama](../../active-directory/active-directory-whatis.md), Şirket [Markalaşma (Logon Sayfalar / Erişim Masası özelleştirme)](../../active-directory/active-directory-whatis.md), [Uygulama Proxy](../../active-directory/active-directory-whatis.md), [SLA 99,9%](../../active-directory/active-directory-whatis.md) |  [Self Servis Grubu ve uygulama Yönetimi/Self Servis uygulama eklemeleri/Dinamik Gruplar,](../../active-directory/active-directory-whatis.md) [Self Servis Şifre Sıfırlama/Değiştirme/Açma ile şirket içi geri yazma](../../active-directory/active-directory-whatis.md), Çok [Faktörlü Kimlik Doğrulama (Bulut ve Şirket İçi (MFA Server))](../../active-directory/active-directory-whatis.md), [MIM CAL + MIM Server](../../active-directory/active-directory-whatis.md), Bulut Uygulaması [Bulma](../../active-directory/active-directory-whatis.md), [Connect Health](../../active-directory/active-directory-whatis.md), Grup hesapları için otomatik [şifre rollover](../../active-directory/active-directory-whatis.md)|    [Kimlik Koruması](../../active-directory/identity-protection/overview.md), [Ayrıcalıklı Kimlik Yönetimi](../../active-directory/privileged-identity-management/pim-configure.md)|   [Azure AD, Desktop SSO, Azure AD için Microsoft Passport, Administrator BitLocker kurtarma,](../../active-directory/active-directory-whatis.md) [MDM otomatik kayıt, Self Servis BitLocker kurtarma, Azure AD Join ile Windows 10 cihazlarına ek yerel yöneticilere](../../active-directory/active-directory-whatis.md) bir aygıta katılın|


- [Bulut Uygulaması Bulma,](../../active-directory/cloudappdiscovery-get-started.md) Azure Active Directory'nin kuruluşunuzdaki çalışanlar tarafından kullanılan bulut uygulamalarını tanımlamanızı sağlayan birinci sınıf bir özelliğidir.

- [Azure Active Directory Identity Protection,](https://azure.microsoft.com/documentation/articles/active-directory-identityprotection/) kuruluşunuzun kimliklerini etkileyebilecek risk algılamalarına ve olası güvenlik açıklarına birleşik bir görünüm sağlamak için Azure Active Directory anomali algılama özelliklerini kullanan bir güvenlik hizmetidir.

- [Azure Active Directory Etki Alanı Hizmetleri,](https://azure.microsoft.com/services/active-directory-ds/) etki alanı denetleyicilerini dağıtmaya gerek kalmadan Azure VM'lerine bir etki alanında katılmanızı sağlar. Kullanıcılar bu VM'lerde kurumsal Active Directory kimlik bilgilerini kullanarak oturum açabilir ve kaynaklara sorunsuz bir şekilde erişebilir.

- [Azure Active Directory B2C,](https://azure.microsoft.com/services/active-directory-b2c/) yüz milyonlarca kimlik le ölçeklendirebilen ve mobil ve web platformları arasında entegre edilebilen tüketiciye dönük uygulamalar için son derece kullanılabilir, küresel bir kimlik yönetimi hizmetidir. Müşterileriniz, mevcut sosyal medya hesaplarını kullanan özelleştirilebilir deneyimler aracılığıyla tüm uygulamalarınızda oturum açabilir veya yeni bağımsız kimlik bilgileri oluşturabilirsiniz.

- [Azure Active Directory B2B Collaboration,](https://aka.ms/aad-b2b-collaboration) iş ortaklarının kendi kendilerini yöneten kimliklerini kullanarak şirket uygulamalarınıza ve verilerinize seçici olarak erişmesini sağlayarak şirketler arası ilişkilerinizi destekleyen güvenli bir iş ortağı tümleştirme çözümüdür.

- [Azure Active Directory Join,](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/) merkezi yönetim için bulut özelliklerini Windows 10 aygıtlarına genişletmenizi sağlar. Kullanıcıların Azure Active Directory aracılığıyla kurumsal veya kuruluş bulutuna bağlanmasını mümkün kılar ve uygulamalara ve kaynaklara erişimi kolaylaştırır.

- [Azure Active Directory Application Proxy,](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/) şirket içinde barındırılan web uygulamaları için SSO ve güvenli uzaktan erişim sağlar.

## <a name="next-steps"></a>Sonraki Adımlar

- [Bulutta paylaşılan sorumluluğunuzu](shared-responsibility.md)anlayın.

- Azure [Güvenlik](https://azure.microsoft.com/services/security-center/) Merkezi'nin, Azure kaynaklarınızın güvenliği üzerinde daha fazla görünürlük ve denetimle tehditleri önlemenize, algılamanıza ve yanıt vermenize nasıl yardımcı olabileceğini öğrenin.
