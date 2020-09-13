---
title: Azure İzleyici’yi dağıtma
description: Azure aboneliğinizdeki tüm kaynakları izlemek üzere Azure Izleyici 'nin tamamen uygulanması için gereken farklı adımları açıklar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: 0a5c788b4429b5048a1b94fa8adfb2d9367982da
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90033481"
---
# <a name="deploy-azure-monitor"></a>Azure İzleyici’yi dağıtma
Azure izleyici 'nin tüm Azure kaynaklarınızın izlenmesini sağlamak için Azure izleyici bileşenleri yapılandırma ve Azure Izleyici 'nin toplanacak izleme verilerini oluşturmak için Azure kaynaklarını yapılandırma bir birleşimidir. Bu makalede, Azure aboneliğinizdeki tüm kaynakları izlemek için ortak bir yapılandırma kullanarak Azure Izleyici 'nin tamamen uygulanması için gereken farklı adımlar açıklanmaktadır. Her adımın temel açıklamaları, ayrıntılı yapılandırma gereksinimleri için diğer belgelerin bağlantılarıyla birlikte sağlanır.

> [!IMPORTANT]
> Azure Izleyici ve yapılandırma özellikleri, etkin özelliklerin maliyetiyle iş gereksinimlerinize bağlı olarak farklılık gösterir. Aşağıdaki her adım potansiyel bir maliyet olup olmadığını belirler ve bu adıma geçmeden önce bu maliyetleri değerlendirmelisiniz. Fiyatlandırma ayrıntılarının tamamı için bkz. [Azure izleyici fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/) .

## <a name="configuration-goals"></a>Yapılandırma hedefleri
Azure Izleyici 'nin tamamen uygulamanın hedefi, tüm bulut kaynaklarınızın ve uygulamalarınızın tüm kullanılabilir verilerini toplamaktır ve bu verilere bağlı olarak Azure Izleyici 'de çok sayıda özelliği etkinleştirir.

Azure Izleyici tarafından toplanan veriler, [Azure Izleyici ölçümlerine](platform/data-platform-metrics.md) veya [Azure izleyici günlüklerine](platform/data-platform-logs.md)gönderilir. Her biri farklı türdeki verileri depolar ve farklı analiz ve uyarı türlerini sunar. Farklı uyarı türlerinin açıklaması için [Microsoft Azure ' deki uyarılara](platform/alerts-overview.md) ilişkin Iki ve genel bakış karşılaştırması için bkz. [Azure Izleyici ölçümlerini ve günlüklerini karşılaştırın](platform/data-platform.md) . 

Bazı veriler, farklı özellikler kullanılarak kullanabilmesi için hem ölçümler hem de günlüklere gönderilebilir. Bu durumlarda, her birini ayrı ayrı yapılandırmanız gerekebilir. Örneğin, ölçüm verileri Azure kaynakları tarafından Ölçüm Gezgini ve ölçüm uyarılarını destekleyen ölçümlere otomatik olarak gönderilir. Aynı ölçüm verilerini günlüklere göndermek için her kaynak için bir tanılama ayarı oluşturmanız gerekir. Bu, Log Analytics kullanarak performans eğilimlerini diğer günlük verileriyle analiz etmenizi sağlar. Aşağıdaki bölümler, verilerin nereye gönderileceğini belirler ve tüm olası konumlara veri göndermek için gereken her adımı içerir.

Azure dışındaki kaynakları izleme ve Azure Izleyici dışında veri gönderme gibi ek gereksinimleriniz olabilir. Bunlar gibi gereksinimler, bu makalede açıklanan özelliklerin ek yapılandırmasıyla elde edilebilir. Ek yapılandırma seçenekleri için her adımın belgelerine yönelik bağlantıları izleyin.

## <a name="collect-data-from-azure-resources"></a>Azure kaynaklarından veri toplama

> [!NOTE]
> Azure Izleyici ile sanal makineleri izlemeye yönelik kapsamlı bir kılavuz için bkz. Azure [izleyici ile Azure kaynaklarını izleme](insights/monitor-azure-resource.md) .

Bazı Azure kaynaklarını izleme, yapılandırma gerekmeden otomatik olarak kullanılabilir, ancak ek izleme verileri toplamak için yapılandırma adımları gerçekleştirmeniz gerekir. Aşağıdaki tabloda, Azure Izleyici ölçümleri ve Azure Izleyici günlüklerine gönderilen adım verileri de dahil olmak üzere Azure kaynaklarınızdan tüm kullanılabilir verileri toplamak için gereken yapılandırma adımları gösterilmektedir. Aşağıdaki bölümlerde her bir adım daha ayrıntılı olarak açıklanır.

[![Azure Kaynak izlemeyi ](media/deploy/deploy-azure-resources.png) dağıtma](media/deploy/deploy-azure-resources.png#lightbox)

### <a name="no-configuration"></a>Yapılandırma yok
Azure Izleyici 'nin aşağıdaki özellikleri, bir Azure aboneliği oluştururken hiçbir yapılandırma gerekmeden etkinleştirilir. Bu izlemele ilişkili bir maliyet yok.

[Azure Active Directory günlükleri](../active-directory/reports-monitoring/overview-reports.md) -oturum açma etkinliğinin kiracı düzeyinde geçmişini ve Azure Active Directory yapılan değişikliklerin denetim izlerini sağlar. Azure Active Directory günlüklerinin ayrıntıları ve Azure portal nasıl görüntüleneceği hakkında bilgi için Azure Active Directory portalındaki Azure Active Directory Portal ve [oturum açma etkinliği raporlarında](../active-directory/reports-monitoring/concept-sign-ins.md) bkz. [Denetim etkinlik raporları](../active-directory/reports-monitoring/concept-audit-logs.md) .

[Etkinlik günlüğü](platform/platform-logs-overview.md) -Azure 'da oluşan yönetim grubu ve abonelik düzeyi olayları hakkında öngörüler sağlar. Yeni bir Azure kaynağı oluşturduğunuzda, bir kaynağı değiştirirken veya önemli bir etkinlik gerçekleştirdiğinizde olaylar otomatik olarak etkinlik günlüğüne yazılır. Olayları Azure portal görüntüleyebilir ve belirli olaylar oluşturulduğunda etkinlik günlüğü uyarıları oluşturabilirsiniz. Etkinlik günlüğünün ayrıntıları ve Azure portal nasıl görüntüleneceği hakkında bilgi için bkz. [Azure etkinlik günlüğü](platform/activity-log.md) .

[Platform ölçümleri](platform/metrics-supported.md) -Azure hizmetlerinden [Azure izleyici ölçümlerine](platform/data-platform-metrics.md)otomatik olarak toplanır. Bu veriler genellikle farklı hizmetler için Azure portal **genel bakış** sayfasında sunulur. Azure portal platform ölçümlerini çözümleme hakkında daha fazla bilgi için bkz. [Azure Ölçüm Gezgini kullanmaya](platform/metrics-getting-started.md) başlama. 


### <a name="create-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma
Bu tür verilerin Azure kaynaklarından Günlükler olarak toplanması, Azure sanal makinelerinin Konuk işletim sisteminden veri toplanması ve Azure Izleyici öngörülerinin çoğu için gerekli olan [Azure Izleyici günlüklerini](platform/data-platform-logs.md)etkinleştirmek için en az bir Log Analytics çalışma alanı gerekir. Azure Sentinel ve Azure Güvenlik Merkezi gibi diğer hizmetler de Log Analytics çalışma alanı kullanır ve Azure Izleyici için kullandığınız aynı şekilde paylaşabilir. Bu izlemeyi desteklemek için tek bir çalışma alanıyla başlayabilirsiniz, ancak birden çok çalışma alanı ne zaman kullanılacağı konusunda rehberlik için  [Azure Izleyici günlükleri dağıtımını tasarlama](platform/design-logs-deployment.md) konusuna bakın.

Log Analytics çalışma alanı oluşturmak için herhangi bir maliyet yoktur, ancak bu verilere toplanacak veriler yapılandırıldıktan sonra potansiyel bir ücret gönderilir. Ayrıntılar için bkz. [Azure Izleyici günlükleriyle kullanımı ve maliyetleri yönetme](platform/manage-cost-storage.md) .  

İlk Log Analytics çalışma alanı oluşturmak için [Azure portal Log Analytics çalışma alanı oluşturma](learn/quick-create-workspace.md) bölümüne bakın. Erişimi yapılandırmak için bkz. [Azure izleyici 'de günlük verilerine ve çalışma alanlarına erişimi yönetme](platform/manage-access.md) . 

### <a name="create-diagnostic-setting-to-collect-tenant-and-subscription-logs"></a>Kiracı ve abonelik günlüklerini toplamak için tanılama ayarı oluştur
Kiracınız için [Azure Active Directory günlüğe kaydedilir](../active-directory/reports-monitoring/overview-reports.md) ve aboneliğiniz için [etkinlik günlüğü](platform/platform-logs-overview.md) otomatik olarak toplanır, bu olayları bir Log Analytics çalışma alanına göndermek, Log Analytics günlük sorgularını kullanarak bu olayları diğer günlük verileriyle analiz etmenizi sağlar. Bu Ayrıca, Azure Active Directory günlüklerini uyarmak ve etkinlik günlüğü uyarılarından daha karmaşık mantık sağlamak için tek yol olan günlük sorgusu uyarıları oluşturmanıza de olanak tanır.

Etkinlik günlüğünü bir çalışma alanına gönderme maliyeti yoktur, ancak Azure Active Directory Günlükler için bir veri alımı ve bekletme ücreti vardır. 

Bkz. [Azure AD günlüklerini Azure izleyici günlükleri Ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) ve kiracınız için bir tanılama ayarı oluşturmak üzere [farklı hedeflere platform günlükleri ve ölçümleri göndermek Için Tanılama ayarları oluşturma](platform/diagnostic-settings.md) ve Log Analytics çalışma alanınıza günlük girişleri gönderme. 

### <a name="create-diagnostic-setting-to-collect-resource-logs-and-platform-metrics"></a>Kaynak günlüklerini ve platform ölçümlerini toplamak için tanılama ayarı oluştur
Azure 'daki kaynaklar, kaynak içinde gerçekleştirilen işlemlerin ayrıntılarını sağlayan [kaynak günlüklerini](platform/platform-logs-overview.md) otomatik olarak oluşturur. Ancak, platform ölçümlerinden farklı olarak, toplanacak kaynak günlüklerini yapılandırmanız gerekir. Bunları Azure Izleyici günlükleriyle kullanılan diğer verilerle birleştirmek için bir Log Analytics çalışma alanına göndermek üzere bir tanılama ayarı oluşturun. Aynı tanılama ayarı aynı çalışma alanına ait platform ölçümlerini aynı çalışma alanına göndermek için de kullanılabilir, bu da diğer toplanan verilerle günlük sorgularını kullanarak ölçüm verilerini analiz etmenizi sağlar.

Bu koleksiyonun bir maliyeti olduğundan, önemli sayıda kaynağa uygulamadan önce [Azure izleyici fiyatlandırmasına](https://azure.microsoft.com/pricing/details/monitor/) bakın. Ayrıca, günlük koleksiyonunuzun maliyetini en iyi duruma getirme hakkındaki ayrıntılar için bkz. [Azure Izleyici günlükleriyle kullanımı ve maliyetleri yönetme](platform/manage-cost-storage.md) .

Azure kaynağı için bir tanılama ayarı oluşturmak üzere [Azure 'da kaynak günlüklerini ve ölçümleri toplamak için bkz. tanılama ayarı oluşturma](platform/diagnostic-settings.md#create-in-azure-portal) . Her Azure kaynağı için bir tanılama ayarının oluşturulması gerektiğinden, Azure [ilkesi](../governance/policy/overview.md) 'ni kullanma hakkında ayrıntılı bilgi için bkz. Azure [izleyici](deploy-scale.md) 'yi kullanma hakkında ayrıntılar için bkz. Azure 'u kullanarak ayarların otomatik olarak oluşturulmasını sağlamak için.

### <a name="enable-insights-and-solutions"></a>Öngörüleri ve çözümleri etkinleştir
Öngörüler ve çözümler, belirli bir hizmet veya çözüm için özel izleme sağlar. Öngörüler, Azure Izleyici 'nin çalışma kitapları gibi daha yeni özelliklerini kullanır, bu nedenle hizmetiniz için kullanılabiliyorsa bir öngörü kullanmanız gerekir. Bunlar her Azure aboneliğinde otomatik olarak kullanılabilir, ancak tam işlevsellik için bazı yapılandırmalar gerektirebilir. Genellikle, daha önce yapılandırdığınız platform ölçümlerini ve kaynak günlüklerini kullanır ve ek veriler toplayabilirler.

Çözümler her aboneliğe eklenmelidir ve Azure Izleyici günlüklerindeki verilerle özel olarak çalışır ve ek günlük verileri toplayabilir.

Çözümler ve Öngörüler için herhangi bir maliyet yoktur, ancak topladıkları veriler için ücretlendirilirsiniz.

Azure izleyici 'de kullanılabilir Öngörüler ve çözümlerin listesi için bkz. [Azure izleyici tarafından Izlenen nedir?](monitor-reference.md) . Her bir benzersiz yapılandırma veya fiyatlandırma bilgisi için belgelere bakın. 

## <a name="collect-data-from-virtual-machines"></a>Sanal makinelerden veri toplama

> [!NOTE]
> Azure Izleyici ile sanal makineleri izlemeye yönelik kapsamlı bir kılavuz için bkz. Azure [izleyici ile Azure sanal makinelerini izleme](insights/monitor-vm-azure.md) . 

Sanal makineler diğer Azure kaynakları gibi benzer veriler oluşturur, ancak Konuk işletim sisteminden veri toplamak için bir aracıya ihtiyacınız vardır. Azure Izleyici tarafından kullanılan aracıların karşılaştırması için bkz. [Azure izleyici aracılarına genel bakış](platform/agents-overview.md) . 

[VM'ler için Azure izleyici](insights/vminsights-overview.md) , sanal makinelerin Konuk işletim sisteminden veri toplamak için Log Analytics Aracısı ve bağımlılık Aracısı 'nı kullanır, bu nedenle bu aracıları bu öngörü uygulamasının bir parçası olarak dağıtabilirsiniz. Bu, Azure Güvenlik Merkezi gibi diğer hizmetler için Log Analytics aracısına izin vermez.


[![Azure VM ](media/deploy/deploy-azure-vm.png) dağıtma](media/deploy/deploy-azure-vm.png#lightbox)


### <a name="configure-workspace-for-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici çalışma alanını yapılandırma
VM'ler için Azure İzleyici, genellikle diğer Azure kaynaklarından veri toplamak için oluşturulan ile aynı olacak bir Log Analytics çalışma alanı gerektirir. Herhangi bir sanal makineyi etkinleştirmeden önce, VM'ler için Azure İzleyici için gereken çözümü çalışma alanına eklemeniz gerekir.

VM'ler için Azure İzleyici için Log Analytics çalışma alanınızı yapılandırma hakkında ayrıntılı bilgi için bkz. [VM'ler için Azure İzleyici Log Analytics çalışma alanını yapılandırma](insights/vminsights-configure-workspace.md) .

### <a name="enable-azure-monitor-for-vms-on-each-virtual-machine"></a>Her sanal makinede VM'ler için Azure İzleyici etkinleştir
Bir çalışma alanı yapılandırıldıktan sonra, Log Analytics Aracısı ve bağımlılık aracısını yükleyerek her bir sanal makineyi etkinleştirebilirsiniz. Azure Ilkesi de dahil olmak üzere bu aracıları yüklemek için birden çok yöntem vardır. Bu, her sanal makineyi oluşturma sırasında otomatik olarak yapılandırmanıza olanak tanır. VM'ler için Azure İzleyici tarafından toplanan performans verileri ve işlem ayrıntıları Azure Izleyici günlüklerinde saklanır.

Aracıları sanal makinelerinize dağıtmak ve bunları izlemeye etkinleştirmek için seçenekler için bkz. [VM'ler için Azure izleyici genel bakış](insights/vminsights-enable-overview.md) .

### <a name="configure-workspace-to-collect-events"></a>Olayları toplamak için çalışma alanını yapılandırma
VM'ler için Azure İzleyici, her bir sanal makinenin Konuk işletim sistemindeki performans verilerini ve işlemlerin ayrıntılarını ve bağımlılıklarını toplar. Log Analytics Aracısı Ayrıca, Windows 'daki olay günlüğü ve Linux 'tan Syslog gibi konuktaki günlükleri de toplayabilir. Bu günlüklerin yapılandırmasını, bağlandığı Log Analytics çalışma alanından alır. Çalışma alanını yalnızca bir kez yapılandırmanız gerekir ve bir aracı her bağlanışında yapılandırma değişikliklerini indirir. 

Aracı sanal makinelerinizden ek veriler toplamak üzere Log Analytics çalışma alanınızı yapılandırma hakkında ayrıntılı bilgi için bkz. [Azure izleyici 'de aracı veri kaynakları](platform/agent-data-sources.md) .

> [!NOTE]
> Ayrıca, çalışma alanını performans sayaçlarını toplayacak şekilde yapılandırabilirsiniz, ancak bu durum büyük olasılıkla VM'ler için Azure İzleyici tarafından toplanan performans verileriyle yedekli olacaktır. Çalışma alanı tarafından toplanan performans verileri, VM'ler için Azure İzleyici tarafından toplanan performans verileri *ınsightsölçümlerini* tablosunda depolandığından *performans tablosunda depolanır* . Çalışma alanındaki performans toplamayı yalnızca, zaten VM'ler için Azure İzleyici tarafından toplanmamış olan sayaçlara ihtiyacınız varsa yapılandırın.

### <a name="diagnostic-extension-and-telegraf-agent"></a>Tanılama uzantısı ve telegraf Aracısı
VM'ler için Azure İzleyici, performans verilerini bir Log Analytics çalışma alanına Gönderen ancak Azure Izleyici ölçümlerine değil Log Analytics aracısını kullanır. Bu verilerin ölçümlere gönderilmesi, Ölçüm Gezgini ile çözümlenme ve ölçüm uyarıları ile birlikte kullanılmasına olanak tanır. Bu, Linux üzerinde Windows ve telegraf Aracısı için tanılama uzantısı gerektirir.

Bu aracıları yükleme ve yapılandırma hakkında ayrıntılı bilgi için, bkz. [Windows Azure tanılama uzantısı 'nı (WAD) yükleme ve yapılandırma](platform/diagnostics-extension-windows-install.md) ve [etkileyen bir Linux sanal makinesi için etkileyen telegraf aracı](platform/collect-custom-metrics-linux-telegraf.md) .


## <a name="monitor-applications"></a>Uygulamaları izleme
Azure Izleyici, izlemek istediğiniz her uygulama için yapılandırmanız gereken [Application Insights](app/app-insights-overview.md)kullanarak özel uygulamalarınızı izler. Yapılandırma işlemi, izlenen uygulamanın türüne ve gerçekleştirmek istediğiniz izleme türüne göre değişir. Application Insights tarafından toplanan veriler, özelliğe bağlı olarak Azure Izleyici ölçümleri, Azure Izleyici günlükleri ve Azure Blob depolama alanında depolanır. Performans verileri, Azure Izleyici ölçümlerinde ve Azure Izleyici günlüklerinde ek yapılandırma gerekmeden depolanır.

### <a name="create-an-application-resource"></a>Uygulama kaynağı oluşturma
İzlemek istediğiniz her uygulama için Application Insights bir kaynak oluşturmanız gerekir. Application Insights tarafından toplanan günlük verileri, çalışma alanı tabanlı bir uygulama için Azure Izleyici günlüklerinde depolanır. Klasik uygulamalar için günlük verileri, [veri yapısı](platform/data-platform-logs.md#structure-of-data)bölümünde açıklandığı gibi Log Analytics çalışma alanınızdan ayrı bir şekilde depolanır.

 Uygulamayı oluştururken klasik veya çalışma alanı tabanlı ' i kullanmayı seçmeniz gerekir. Bkz. klasik bir uygulama oluşturmak için [Application Insights kaynağı oluşturma](app/create-new-resource.md) . Çalışma alanı tabanlı bir uygulama oluşturmak için [çalışma alanı tabanlı Application Insights kaynaklarına (Önizleme)](app/create-workspace-resource.md) bakın.

### <a name="configure-codeless-or-code-based-monitoring"></a>Kodsuz kullanacaksınız veya kod tabanlı izlemeyi yapılandırma
Bir uygulama için izlemeyi etkinleştirmek üzere, kodsuz kullanacaksınız veya kod tabanlı izleme kullanıp kullanmayacağınızı karar vermelisiniz. Yapılandırma işlemi, bu kararya ve izlediğiniz uygulamanın türüne bağlı olarak farklılık gösterir.

**Codeless izlemenin** uygulanması en kolay yöntemdir ve kod geliştirmeden sonra yapılandırılabilirler. Kodunuzda güncelleştirme gerektirmez. Uygulamanıza bağlı olarak izlemeyi etkinleştirme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

- [Azure Web Apps barındırılan uygulamalar](app/azure-web-apps.md)
- [Java uygulamaları](app/java-in-process-agent.md)
- [Azure VM veya Azure sanal makine ölçek kümesi üzerinde IIS 'de barındırılan ASP.NET uygulamaları](app/azure-vm-vmss-apps.md)
- [ASP.NET IIS şirket içi VM 'de barındırılan uygulamalar](app/monitor-performance-live-website-now.md)


**Kod tabanlı izleme** daha özelleştirilebilir ve ek telemetri toplar, ancak Application Insights SDK NuGet paketlerindeki kodunuza bağımlılık eklemeyi gerektirir. Uygulamanıza bağlı olarak izlemeyi etkinleştirme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

- [ASP.NET uygulamaları](app/asp-net.md)
- [ASP.NET Core uygulamalar](app/asp-net-core.md)
- [.NET konsol uygulamaları](app/console.md)
- [Java](app/java-get-started.md)
- [Node.js](app/nodejs.md)
- [Python](app/opencensus-python.md)
- [Diğer platformlar](app/platforms.md)

### <a name="configure-availability-testing"></a>Kullanılabilirlik testini yapılandırma
Application Insights üzerindeki kullanılabilirlik testleri, uygulamanızın kullanılabilirlik ve yanıt verme hızını dünyanın dört bir yanındaki noktalarından düzenli aralıklarla izleyen yinelenen sınamalardır. Uygun maliyetli olan kullanıcı işlemlerinin benzetimini yapmak için, ücretsiz olarak basit bir ping testi oluşturabilir veya bir dizi Web isteği oluşturabilirsiniz. 

Bkz. farklı test türlerinin Özeti ve bunların oluşturulmasına ilişkin ayrıntılar için [herhangi bir Web sitesinin kullanılabilirliğini izleme](app/monitor-web-app-availability.md) .

### <a name="configure-profiler"></a>Profil oluşturucuyu yapılandırma
Application Insights profil oluşturucu, .NET uygulamaları için performans izlemeleri sağlar. Belirli bir web isteğini işlerken en uzun süreyi alan "sık erişimli" kod yolunu belirlemenize yardımcı olur. Profil oluşturucuyu yapılandırma işlemi, uygulama türüne bağlı olarak değişir. 

Profil oluşturucuyu yapılandırma hakkında ayrıntılı bilgi için bkz. [Azure 'Daki profil üretim uygulamaları Application Insights](app/profiler-overview.md) .

### <a name="configure-snapshot-debugger"></a>Snapshot Debugger Yapılandır
Application Insights Snapshot Debugger, .NET uygulamanızdan özel durum telemetrisini izler ve üretim aşamasındaki sorunları tanılamak için ihtiyaç duyduğunuz bilgilere sahip olmanız için en iyi oluşturma özel durumlarınızın anlık görüntülerini toplar. Snapshot Debugger yapılandırma işlemi, uygulama türüne bağlı olarak değişir. 

Snapshot Debugger yapılandırma hakkında ayrıntılı bilgi için bkz. [.NET uygulamalarında özel durumlarda hata ayıklama anlık görüntüleri](app/snapshot-debugger.md) .


## <a name="visualize-data"></a>Verileri görselleştirme
Öngörüler ve çözümler, kendi çalışma kitaplarını ve verilerini çözümlemek için görünümlerini içerir. Bunlara ek olarak, Azure izleyici verilerini ve panoları içeren çalışma kitapları da dahil olmak üzere kendi [görselleştirmelerinizi](visualizations.md) oluşturabilir ve Azure izleyici verilerini Azure 'daki diğer hizmetlerden alınan verilerle birleştirebilirsiniz.


### <a name="create-workbooks"></a>Çalışma kitapları oluşturma
Azure Izleyici 'deki [çalışma kitapları](platform/workbooks-overview.md) Azure Portal zengin görsel raporlar oluşturmanıza imkan tanır. Birleşik etkileşimli deneyimler oluşturmak için, Azure Izleyici ölçümleri ve Azure Izleyici günlüklerinden farklı veri kümelerini birleştirebilirsiniz. Çalışma kitaplarının galerisine Azure Izleyici menüsünün **çalışma kitapları** sekmesinden erişebilirsiniz. 

Özel çalışma kitapları oluşturma hakkında ayrıntılı bilgi için bkz. [Azure Izleyici çalışma kitapları](platform/workbooks-overview.md) .

### <a name="create-dashboards"></a>Panoları oluşturma
Azure [panoları](../azure-portal/azure-portal-dashboards.md) , Azure için birincil sıra oluşturma teknolojisidir ve Azure altyapınız üzerinde Azure izleyici verilerini diğer hizmetlerden alınan verilerle birleştirerek Azure altyapınızda tek bir cam bölmesi sağlar. Azure Izleyici günlüklerinden veri içeren bir pano oluşturma hakkında ayrıntılı bilgi için bkz. [Log Analytics veri panoları oluşturma ve paylaşma](learn/tutorial-logs-dashboards.md) . 

Application Insights verileri içeren bir pano oluşturma hakkında ayrıntılı bilgi için bkz. [Azure Application Insights kullanarak özel KPI panoları oluşturma](learn/tutorial-app-dashboards.md) . 

## <a name="alerts"></a>Uyarılar
Azure Izleyici 'deki uyarılar, izleme verilerinizde tanımlanan önemli verileri veya desenleri önceden bilgilendirir. Bazı Öngörüler, yapılandırma olmadan uyarılar oluşturur. Diğer senaryolarda, analiz edilecek verileri ve bir uyarının ne zaman oluşturulacağı ölçütlerini ve bir uyarı oluşturulduğunda gerçekleştirilecek eylemi tanımlayan eylem gruplarını içeren [Uyarı kuralları](platform/alerts-overview.md) oluşturmanız gerekir. 


### <a name="create-action-groups"></a>Eylem grubu oluşturma
[Eylem grupları](platform/action-groups.md) , uyarı kuralları tarafından bir uyarı tetiklendiğinde gerçekleştirilecek eylemi belirlemede kullanılan bir bildirim tercihleri koleksiyonudur. Eylem örnekleri arasında bir posta veya metin gönderilmesi, Web kancası çağrılması veya bir ıTSM aracına veri gönderilmesi dahildir. Her uyarı kuralı en az bir eylem grubu gerektirir ve tek bir eylem grubu birden çok uyarı kuralı tarafından kullanılabilir.

Bir eylem grubu oluşturma ve dahil olduğu farklı eylemlerin açıklaması hakkında ayrıntılar için [Azure Portal eylem grupları oluşturma ve yönetme](platform/action-groups.md) konusuna bakın.


### <a name="create-alert-rules"></a>Uyarı kuralları oluşturma
Kullandıkları veri türü tarafından tanımlanan birden çok uyarı kuralı türü vardır. Her birinin farklı özellikleri ve farklı bir maliyeti vardır. İzlemeniz gereken temel strateji, uyarı kuralı türünü, ihtiyacınız olan mantığı sağlayan en düşük maliyetle kullanmaktır.

- [Etkinlik günlüğü kuralları](platform/activity-log-alerts.md). Belirtilen koşullara uyan yeni bir etkinlik günlüğü olayına yanıt olarak bir uyarı oluşturur. Bu uyarıların bir maliyeti yoktur, bu nedenle ilk seçiminiz olmaları gerekir. Etkinlik günlüğü uyarısı oluşturma hakkında ayrıntılı bilgi için bkz. [Azure izleyici kullanarak etkinlik günlüğü uyarılarını oluşturma, görüntüleme ve yönetme](platform/alerts-activity-log.md) .
- [Ölçüm uyarısı kuralları](platform/alerts-metric-overview.md). Bir eşiği aşan bir veya daha fazla ölçüm değerine yanıt olarak bir uyarı oluşturur. Ölçüm uyarıları durum bilgisi anlamına gelir ve değer eşiğin altına düştüğünde uyarının otomatik olarak kapatılmasını ve durum değiştiğinde yalnızca bildirim gönderecektir. Ölçüm uyarılarına ilişkin bir ücret vardır ancak bu, günlük uyarılarından önemli ölçüde küçüktür. Ölçüm uyarısı oluşturma hakkında ayrıntılı bilgi için bkz. [Azure izleyici kullanarak ölçüm uyarıları oluşturma, görüntüleme ve yönetme](platform/alerts-metric.md) .
- [Uyarı kurallarını günlüğe kaydedin](platform/alerts-unified-log.md). Zamanlama sorgusunun sonuçları belirtilen ölçütlerle eşleştiğinde bir uyarı oluşturur. Bunlar, uyarı kurallarının en pahalıdır, ancak en karmaşık ölçütlere izin verir. Günlük sorgusu uyarısı oluşturma hakkında ayrıntılı bilgi için bkz. [Azure izleyici kullanarak günlük uyarıları oluşturma, görüntüleme ve yönetme](platform/alerts-log.md) .
- [Uygulama uyarıları](app/monitor-web-app-availability.md) , Web uygulamanızın proaktif performans ve kullanılabilirlik testlerini gerçekleştirmenize olanak tanır. Herhangi bir ücret ödemeden basit bir ping testi gerçekleştirebilirsiniz, ancak daha karmaşık testler için bir maliyet vardır. Bkz. farklı testlerin açıklaması ve bunların oluşturulmasına ilişkin ayrıntılar için [herhangi bir Web sitesinin kullanılabilirliğini izleme](app/monitor-web-app-availability.md) .


## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure İzleyicisini kullanarak Azure izleyici 'yi ölçekli olarak dağıtma](deploy-scale.md).