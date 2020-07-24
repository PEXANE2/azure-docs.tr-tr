---
title: Azure Işlevleri için Azure Güvenlik temeli
description: Azure Işlevleri için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: b8e6797d1dbbe4260f7a5b701ada887f68a88a7c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079681"
---
# <a name="azure-security-baseline-for-azure-functions"></a>Azure Işlevleri için Azure Güvenlik temeli

Azure Işlevleri için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](../security/benchmarks/overview.md)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](../security/benchmarks/security-baselines-overview.md).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: Azure işlevleri uygulamalarınızı bir Azure sanal ağı ile tümleştirin. Premium planda çalışan işlev uygulamaları, "VNet tümleştirme" özelliği de dahil olmak üzere Azure App Service Web uygulamalarıyla aynı barındırma özelliklerine sahiptir.  Azure sanal ağları, Azure Işlevleri gibi Azure kaynaklarınızın çoğunu internet 'e yönlendirilemeyen bir ağa yerleştirmeniz sağlar.

- [Işlevleri bir Azure sanal ağı ile tümleştirme](./functions-create-vnet.md)

- [Azure Işlevleri ve Azure App Service için VNET tümleştirmesini anlayın](../app-service/web-sites-integrate-with-vnet.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Kılavuz**: Azure Güvenlik Merkezi 'ni kullanın ve Azure işlevleri uygulamalarınızla ilgili ağ kaynaklarının ve ağ yapılandırmalarının güvenliğini sağlamaya yardımcı olmak için ağ koruma önerilerini izleyin.

Azure Işlevleri uygulamasıyla ağ güvenlik grupları (NSG 'ler) kullanıyorsanız, NSG akış günlüklerini etkinleştirin ve trafik denetimleri için günlükleri Azure depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

- [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](../security-center/security-center-network-recommendations.md)

- [NSG akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Trafik Analizi etkinleştirme ve kullanma](../network-watcher/traffic-analytics.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Kılavuz**: Azure işlev uç noktalarınızı üretimde tam olarak güvenli hale getirmek için aşağıdaki işlev uygulama düzeyi güvenlik seçeneklerinden birini uygulamayı göz önünde bulundurmanız gerekir:
- İşlev uygulamanız için App Service kimlik doğrulaması/yetkilendirme 'yi açın,
- İsteklerin kimliğini doğrulamak için Azure API Management (APıM) kullanın veya
- İşlev uygulamanızı bir Azure App Service Ortamı dağıtın.

Ayrıca, uzaktan hata ayıklamanın üretim Azure işlevleriniz için devre dışı bırakıldığından emin olun. Ayrıca, çıkış noktaları arası kaynak paylaşımı (CORS) tüm etki alanlarının Azure Işlev uygulamanıza erişmesine izin vermemelidir. Yalnızca gerekli etki alanlarının Azure Işlev uygulamasıyla etkileşime girmesine izin verin.

Gelen trafiğin ek incelemesi için ağ yapılandırmasının bir parçası olarak Azure Web uygulaması güvenlik duvarı (WAF) dağıtımı yapmayı göz önünde bulundurun. WAF ve alma günlükleri için tanılama ayarını bir depolama hesabı, Olay Hub 'ı veya Log Analytics çalışma alanında etkinleştirin. 

- [Üretimde Azure Işlev uç noktalarının güvenliğini sağlama](./functions-bindings-http-webhook-trigger.md?tabs=csharp#secure-an-http-endpoint-in-production)

- [Azure WAF dağıtma](../web-application-firewall/ag/create-waf-policy-ag.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: DDoS saldırılarına karşı koruma sağlamak için işlevleriniz uygulamalarınızla Ilişkili sanal ağlarda DDoS koruma standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan genel IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.
Ayrıca, tüm gelen isteklerin kimliğini doğrulamak ve kötü amaçlı trafiği filtrelemek için Azure Web uygulaması güvenlik duvarı gibi bir ön uç ağ geçidi yapılandırın. Azure Web uygulaması güvenlik duvarı, SQL 'leri, siteler arası betikleri, kötü amaçlı yazılım yüklemelerini ve DDoS saldırılarını engellemek için gelen Web trafiğini inceleyerek Azure Işlev uygulamalarınızın güvenliğini sağlamaya yardımcı olabilir. Bir WAF 'nin tanıtımı için App Service Ortamı veya özel uç noktalar (Önizleme) kullanılması gerekir. Özel uç noktaların, üretim iş yükleriyle kullanılmadan önce artık (Önizleme) olmadığından emin olun.

- [Azure İşlevleri ağ seçenekleri](./functions-networking-options.md)

- [Azure Işlevleri Premium planı](./functions-scale.md#premium-plan)

- [App Service Ortamlarına giriş](../app-service/environment/intro.md)

- [App Service Ortamında ağ konusunda dikkat edilmesi gerekenler](../app-service/environment/network-info.md)

- [DDoS korumasını yapılandırma](../virtual-network/manage-ddos-protection.md)

- [Azure Güvenlik duvarını dağıtma](../firewall/tutorial-firewall-deploy-portal.md)

- [Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama](../security-center/threat-protection.md)

- [Azure Güvenlik Merkezi Uyarlamalı ağ sağlamlaştırma 'yi anlama](../security-center/security-center-adaptive-network-hardening.md)

- [Azure Güvenlik Merkezi 'Ni tam zamanında ağ Access Control anlama](../security-center/security-center-just-in-time.md)

- [Azure Işlevleri için özel uç noktaları kullanma](../app-service/networking/private-endpoint.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Kılavuz**: Azure Işlevleri uygulamasıyla ağ güvenlik grupları (NSG 'ler) kullanıyorsanız, ağ güvenlik grubu akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir depolama hesabına gönderin. Ayrıca, akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

- [NSG akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Trafik Analizi etkinleştirme ve kullanma](../network-watcher/traffic-analytics.md)

- [Ağ İzleyicisini etkinleştirme](../network-watcher/network-watcher-create.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: tüm gelen isteklerin kimliğini doğrulamak ve kötü amaçlı trafiği filtrelemek Için Azure Web uygulaması güvenlik duvarı gibi bir ön uç ağ geçidi yapılandırın. Azure Web uygulaması güvenlik duvarı, SQL 'leri, siteler arası betikleri, kötü amaçlı yazılım yüklemelerini ve DDoS saldırılarını engellemek için gelen Web trafiğini inceleyerek, işlev uygulamalarınızın güvenliğini sağlamaya yardımcı olabilir. Bir WAF 'nin tanıtımı için App Service Ortamı veya özel uç noktalar (Önizleme) kullanılması gerekir. Özel uç noktaların, üretim iş yükleriyle kullanılmadan önce artık (Önizleme) olmadığından emin olun.

Alternatif olarak, Azure için ID/IP 'ler özellikleri içeren Azure Marketi 'nde bulunan Barbcuda WAF gibi birden çok Market seçeneği vardır.

- [Azure İşlevleri ağ seçenekleri](./functions-networking-options.md)

- [Azure Işlevleri Premium planı](./functions-scale.md#premium-plan)

- [App Service Ortamlarına giriş](../app-service/environment/intro.md)

- [App Service Ortamında ağ konusunda dikkat edilmesi gerekenler](../app-service/environment/network-info.md)

- [Azure Web uygulaması güvenlik duvarını anlama](../web-application-firewall/index.yml)

- [Azure Işlevleri için özel uç noktaları kullanma](../app-service/networking/private-endpoint.md)

- [Barbcuda WAF bulut hizmetini anlama](../app-service/environment/app-service-app-service-environment-web-application-firewall.md#configuring-your-barracuda-waf-cloud-service)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: ağınız için uçtan uca TLS şifrelemesi Ile Azure Web uygulaması güvenlik duvarı gibi bir ön uç ağ geçidi yapılandırın. Bir WAF 'nin tanıtımı için App Service Ortamı veya özel uç noktalar (Önizleme) kullanılması gerekir. Özel uç noktaların, üretim iş yükleriyle kullanılmadan önce artık (Önizleme) olmadığından emin olun.

- [Azure İşlevleri ağ seçenekleri](./functions-networking-options.md)

- [Azure Işlevleri Premium planı](./functions-scale.md#premium-plan)

- [App Service Ortamlarına giriş](../app-service/environment/intro.md)

- [App Service Ortamında ağ konusunda dikkat edilmesi gerekenler](../app-service/environment/network-info.md)

- [Azure Web uygulaması güvenlik duvarını anlama](../web-application-firewall/index.yml)

- [Portal ile Application Gateway kullanarak uçtan uca TLS Yapılandırma](../application-gateway/end-to-end-ssl-portal.md)

- [Azure Işlevleri için özel uç noktaları kullanma](../app-service/networking/private-endpoint.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak Için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örneğin, Azureuygulamahizmeti) belirterek, karşılık gelen hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

- [Hizmet etiketlerini kullanma hakkında daha fazla bilgi için](../virtual-network/service-tags-overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure işlevleriniz ile ilgili ağ ayarları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure işlevlerinizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Web" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. Azure Işlevleri için yerleşik ilke tanımlarından da yararlanabilirsiniz, örneğin:
- CORS, her kaynağın Işlev uygulamalarınıza erişmesine izin vermemelidir
- İşlev Uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- İşlev Uygulaması en son TLS sürümü kullanılmalıdır

Ayrıca, Azure Resource Manager şablonları, rol tabanlı erişim denetimi (RBAC) ve tek bir şema tanımında ilkeler gibi anahtar ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Yeni aboneliklere, ortamlara kolayca şema uygulayabilir ve sürüm oluşturma aracılığıyla denetim ve yönetime yönetim sağlayabilirsiniz.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Blueprint oluşturma](../governance/blueprints/create-blueprint-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Kılavuz**: Azure işlevleri uygulamanız Ile ağ güvenlik grupları (NSG 'ler) kullanıyorsanız, NSG 'ler ve ağ güvenliği ve trafik akışı ile ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için "Açıklama" alanını kullanarak ağa giden/giden trafiğe izin veren kuralların iş gereksinimini ve/veya süresini (vs.) belirtin.

Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz.

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını Izleyin ve Azure işlevleri dağıtımlarınızla ilgili ağ ayarları ve kaynakları için değişiklikleri tespit edin. Kritik ağ ayarlarında veya kaynaklarda yapılan değişiklikler gerçekleştiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun. 

- [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](../azure-monitor/platform/activity-log.md#view-the-activity-log)

- [Azure Izleyici 'de uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, günlüklerdeki zaman damgalarına yönelik Azure Işlevleri gibi Azure kaynakları için kullanılan saat kaynağını korur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: denetim düzlemi denetim günlüğü Için Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Azure etkinlik günlüğü verilerini kullanarak, Azure kaynaklarınızın denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, SILME) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Azure Işlevleri, işlevleri izlemek için Azure Application Insights ile yerleşik tümleştirme de sağlar. Application Insights günlük, performans ve hata verilerini toplar. Performans sorunlarını otomatik olarak algılar ve sorunları tanılamanıza ve işlevlerinizin nasıl kullanıldığını anlamanıza yardımcı olacak güçlü analiz araçları içerir.

Azure Işlev uygulamanızda yerleşik özel güvenlik/denetim günlükleriniz varsa, "FunctionAppLogs" Tanılama ayarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. 

İsteğe bağlı olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz. 

- [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

- [Azure Işlevleri 'ni Azure Application Insights ayarlama](./functions-monitoring.md)

- [Azure Işlevleri için tanılama ayarlarını etkinleştirme (Kullanıcı tarafından oluşturulan Günlükler)](./functions-monitor-log-analytics.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: denetim düzlemi denetim günlüğü Için Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Azure etkinlik günlüğü verilerini kullanarak, Azure kaynaklarınızın denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, SILME) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Azure Işlev uygulamanızda yerleşik özel güvenlik/denetim günlükleriniz varsa, "FunctionAppLogs" Tanılama ayarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. 

- [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

- [Azure Işlevleri için tanılama ayarlarını etkinleştirme (Kullanıcı tarafından oluşturulan Günlükler)](./functions-monitor-log-analytics.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu kılavuz IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, kuruluşunuzun uyumluluk düzenlemelerine göre Azure işlevleri uygulamalarınızla ilişkili Log Analytics çalışma alanları için günlük tutma süresini ayarlayın.

- [Günlük tutma parametrelerini ayarlama](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Kılavuz**: Azure etkinlik günlüğü tanılama ayarlarını ve Azure işlevleri uygulamanız için tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve toplanan verilere göre birçok diğer öngörü sağlamak için Log Analytics sorguları gerçekleştirin.

Günlük, performans ve hata verilerini toplamak için Azure Işlevleri uygulamalarınızın Application Insights etkinleştirin. Application Insights tarafından toplanan telemetri verilerini Azure portal içinde görüntüleyebilirsiniz.

Azure Işlev uygulamanızda yerleşik özel güvenlik/denetim günlükleriniz varsa, "FunctionAppLogs" Tanılama ayarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. 

İsteğe bağlı olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz. 

- [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

- [Azure Işlevleri için tanılama ayarlarını etkinleştirme](./functions-monitor-log-analytics.md)

- [Azure Işlevleri 'ni Azure Application Insights ayarlama ve telemetri verilerini görüntüleme](./functions-monitoring.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Kılavuz**: Azure etkinlik günlüğü tanılama ayarlarını ve Azure işlevleri uygulamanız için tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve toplanan verilere göre birçok diğer öngörü sağlamak için Log Analytics sorguları gerçekleştirin. Log Analytics çalışma alanı sorgularınızı temel alan uyarılar oluşturabilirsiniz.

Günlük, performans ve hata verilerini toplamak için Azure Işlevleri uygulamalarınızın Application Insights etkinleştirin. Application Insights tarafından toplanan telemetri verilerini görüntüleyebilir ve Azure portal içinde uyarı oluşturabilirsiniz.

İsteğe bağlı olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz. 

- [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

- [Azure Işlevleri için tanılama ayarlarını etkinleştirme](./functions-monitor-log-analytics.md)

- [Azure Işlevleri için Application Insights etkinleştirme](./functions-monitoring.md#enable-application-insights-integration)

- [Azure 'da uyarı oluşturma](../azure-monitor/learn/tutorial-response.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; Azure Işlevleri uygulamaları, kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Azure Işlevleri uygulamaları, Kullanıcı tarafından erişilebilen DNS ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu kılavuz IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın. 

- [Azure AD 'de PowerShell ile dizin rolü alma](/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Kılavuz**: Azure işlevlerine denetim düzlemi erişimi, Azure ACTIVE DIRECTORY (ad) ile denetlenir. Azure AD varsayılan parola kavramına sahip değildir.

Veri düzlemi erişimi, yetkilendirme anahtarları, ağ kısıtlamaları ve AAD kimlik doğrulaması dahil olmak üzere çeşitli yollarla denetlenebilir. Yetkilendirme anahtarları, Azure Işlevleri HTTP uç noktalarınıza bağlanan istemciler tarafından kullanılır ve herhangi bir zamanda yeniden oluşturulabilir. Bu anahtarlar varsayılan olarak yeni HTTP uç noktaları için oluşturulur.

Birden çok dağıtım yöntemi, bir dizi oluşturulan kimlik bilgilerinden faydalanabilir işlev uygulamaları için kullanılabilir. Uygulamanız için kullanılacak dağıtım yöntemlerini gözden geçirin.

- [HTTP uç noktası güvenliğini sağlama](./functions-bindings-http-webhook-trigger.md?tabs=csharp#secure-an-http-endpoint-in-production)

- [Yetkilendirme anahtarlarını edinme ve yeniden oluşturma](./functions-bindings-http-webhook-trigger.md?tabs=csharp#obtaining-keys)

- [Azure Işlevlerinde dağıtım teknolojileri](./functions-deployment-technologies.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Ayrıca, özel yönetim hesaplarını izlemenize yardımcı olmak için Azure Güvenlik Merkezi 'nden veya yerleşik Azure Ilkelerinden öneriler kullanabilirsiniz; Örneğin, aboneliğiniz için birden fazla sahip olması gerekir sahip izinlere sahip olan kullanım dışı hesaplar aboneliğinizden çıkarılmalıdır

- [Kimlik ve erişim (Önizleme) izlemek için Azure Güvenlik Merkezi 'ni kullanma](../security-center/security-center-identity-access.md)

- [Azure Ilkesini kullanma](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: mümkün olan yerlerde, işlev uygulamanıza veri erişimi için tek başına bağımsız kimlik bilgilerini yapılandırmak yerıne Azure Active Directory SSO kullanın. Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini kullanın. App Service kimlik doğrulaması/yetkilendirme özelliğini kullanarak Azure Işlevleri uygulamalarınız için çoklu oturum açma uygulayın.

- [Azure Işlevlerinde kimlik doğrulama ve yetkilendirmeyi anlama](../app-service/overview-authentication-authorization.md#identity-providers)

- [Azure AD ile SSO 'yu anlama](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure ACTIVE DIRECTORY (AD) MULTI-Factor AUTHENTICATION (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

- [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve yapılandırmak için yapılandırılmış MULTI-Factor AUTHENTICATION (MFA) ile ayrıcalıklı erişim iş istasyonları (Paw) kullanın.

- [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik oluştuğunda günlükler ve uyarılar oluşturmak için Azure ACTIVE DIRECTORY (AD) PRIVILEGED IDENTITY Management (PIM) kullanın.

Ayrıca, riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın.

- [Privileged Identity Management dağıtma (PıM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Azure AD risk algılamalarını anlama](../active-directory/identity-protection/overview-identity-protection.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından Azure Portal erişimine izin vermek Için, koşullu erişim adlı konum kullanın.

- [Azure 'da adlandırılmış konumları yapılandırma](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Kılavuz**: Azure işlevleri uygulamalarınız için merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

- [Azure Işlevleri uygulamanızı Azure AD oturum açma bilgilerini kullanacak şekilde yapılandırma](../app-service/configure-authentication-provider-aad.md)

- [AAD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) eski hesapları keşfetmenize yardımcı olacak Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir. 

- [Azure AD raporlamayı anlama](../active-directory/reports-monitoring/index.yml)

- [Azure kimlik erişimi Incelemelerini kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Kılavuz**: Azure işlev uygulamalarınız için merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

Azure AD oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişerek Azure Sentinel veya bir üçüncü taraf SıEM ile tümleştirmenize olanak tanır.

Bu işlemi, Azure AD Kullanıcı hesapları için Tanılama ayarları oluşturarak ve Log Analytics çalışma alanına denetim günlüklerini ve oturum açma günlüklerini göndererek kolaylaştırabilirsiniz. Log Analytics içinde, istenen günlük uyarılarını yapılandırabilirsiniz.

- [Azure Işlevleri uygulamanızı Azure AD oturum açma bilgilerini kullanacak şekilde yapılandırma](../app-service/configure-authentication-provider-aad.md)

- [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Yerleşik Azure Sentinel](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Kılavuz**: Azure işlevleri uygulamalarınız için merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Denetim düzleminde hesap oturum açma davranışı sapması (Azure portal) için, otomatik yanıtları Kullanıcı kimlikleriyle ilgili şüpheli eylemlere yönelik olarak yapılandırmak için Azure Active Directory (AD) kimlik koruması ve risk algılama özelliklerini kullanın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz.

- [Azure AD riskli oturum açma işlemlerini görüntüleme](../active-directory/identity-protection/overview-identity-protection.md)

- [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: Şu anda kullanılamıyor; Müşteri Kasası Azure Işlevleri için şu anda desteklenmiyor.

- [Müşteri Kasası tarafından desteklenen hizmetler listesi](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Azure Işlevi uygulamalarının sanal ağ (VNet)/subnet ile ayrılması ve uygun şekilde etiketlenmesi gerekir.

Ayrıca, Özel uç noktaları ağ yalıtımı gerçekleştirmek için de kullanabilirsiniz. Azure özel uç noktası, Azure özel bağlantısı tarafından desteklenen bir hizmete özel ve güvenli bir şekilde (örneğin: Azure Işlevleri uygulaması HTTPs uç noktası) bağlanan bir ağ arabirimidir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor. Özel uç noktalar Premium planda çalışan işlev uygulamalarının (Önizleme) aşamasındadır. Özel uç noktaların, üretim iş yükleriyle kullanılmadan önce artık (Önizleme) olmadığından emin olun.

- [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

- [Yönetim Grupları oluşturma](../governance/management-groups/create.md)

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

- [Azure İşlevleri ağ seçenekleri](./functions-networking-options.md)

- [Azure Işlevleri Premium planı](./functions-scale.md#premium-plan)

- [Özel uç noktayı anlama](../private-link/private-endpoint-overview.md)

- [Azure Işlevleri için özel uç noktaları kullanma](../app-service/networking/private-endpoint.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: ağ engelleyicilerinde, hassas bilgilerin yetkisiz aktarımını izleyen ve bilgi güvenliği uzmanlarına uyarı ederken bu tür aktarımları engelleyen bir otomatik araç dağıtın. 

Microsoft, Azure Işlevleri için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: geçerli değil

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Kılavuz**: Azure işlev uygulamalarınızın Azure Portal, "platform özellikleri: Ağ: SSL" altında "yalnızca https" ayarını etkinleştirin ve en düşük TLS sürümünü 1,2 olarak ayarlayın.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: Şu anda kullanılamıyor; veri tanımlama, sınıflandırma ve kayıp önleme özellikleri şu anda Azure Işlevleri için kullanılamaz. Bu gibi hassas bilgileri işleyen ve uyumluluk amaçları için gerekliyse üçüncü taraf çözümü uygulayan Işlev uygulamalarını etiketleyin.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Kılavuz**: Azure işlev denetim düzlemi (Azure Portal) erişimini denetlemek için Azure ACTIVE DIRECTORY (ad) rol tabanlı erişim denetımı (RBAC) kullanın. 

- [Azure 'da RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

Microsoft, Azure Işlevleri için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: bir işlev uygulaması oluştururken blob, kuyruk ve tablo depolamayı destekleyen genel amaçlı bir Azure depolama hesabı oluşturmanız veya bağlamanız gerekir. Bunun nedeni, Işlevlerin Tetikleyicileri yönetme ve işlev yürütmelerinin günlüğe kaydedilmesini gibi işlemler için Azure Storage 'ı temel aldığından oluşur. Azure depolama, bekleyen bir depolama hesabındaki tüm verileri şifreler. Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, blob ve dosya verilerinin şifrelenmesi için müşteri tarafından yönetilen anahtarlar sağlayabilirsiniz. Bu anahtarların, depolama hesabına erişebilmesi için işlev uygulamasının Azure Key Vault olması gerekir.

- [Azure Işlevleri için depolama konularını anlayın](./storage-considerations.md)

- [Bekleyen veriler için Azure depolama şifrelemesini anlayın](../storage/common/storage-service-encryption.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Izleyici 'Yi Azure etkinlik günlüğü ile birlikte kullanarak, üretim Azure işlev uygulamalarına ve diğer kritik veya ilgili kaynaklara yönelik değişikliklerin ne zaman gerçekleştiği hakkında uyarılar oluşturun.

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Kılavuz**: Azure işlevleri uygulamalarınızın güvende olmasını sağlamak Için bir DevSecOps uygulaması benimseyin ve yaşam döngüsü süresince mümkün olduğunca güvenli olarak kalır. DevSecOps, kuruluşunuzun güvenlik ekibini ve yeteneklerini, ekipteki herkesin sorumluluğunda güvenlik sağlamak için DevOps uygulamalarınıza ekler.

Ayrıca, Azure Işlev uygulamalarınızın güvenliğini sağlamaya yardımcı olmak için Azure Güvenlik Merkezi 'ndeki önerileri izleyin.

- [CI/CD ardışık düzenine sürekli güvenlik doğrulaması ekleme](/azure/devops/migrate/security-validation-cicd-pipeline?view=azure-devops)

- [Azure Güvenlik Merkezi güvenlik açığı değerlendirmesi önerilerini uygulama](../security-center/security-center-vulnerability-assessment-recommendations.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="52-deploy-an-automated-operating-system-patch-management-solution"></a>5,2: otomatik bir işletim sistemi düzeltme eki yönetimi çözümü dağıtın

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: Microsoft, Azure işlevlerini destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir, ancak Azure Güvenlik Merkezi 'ndeki önerilerin önem derecesini ve ortamınızdaki riski ölçmek Için güvenli puanı kullanabilirsiniz. Güvenli puanınız, kaç Güvenlik Merkezi önerisi azaldığından temel alır. İlk çözümleme önerilerini önceliklendirmek için, her birinin önem derecesini göz önünde bulundurun.

- [Güvenlik önerileri başvuru kılavuzu](../security-center/recommendations-reference.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz dahilinde (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) tüm kaynakları sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın.  Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerini numaralandırın.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

- [Azure Kaynak Graf ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

- [Azure aboneliklerinizi görüntüleme](/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Azure RBAC 'yi anlama](../role-based-access-control/overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure kaynaklarını düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ayrıca, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesi 'ni kullanın: izin verilen kaynak türleri izin verilen kaynak türleri

- [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

- [Yönetim Grupları oluşturma](../governance/management-groups/create.md)

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini koruyun

**Rehberlik**: işlem kaynakları Için onaylanan Azure kaynakları ve onaylanan yazılımlar tanımlayın.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: abonelikleriniz içinde oluşturulabilecek kaynak türlerine yönelik kısıtlamalar koymak Için Azure ilkesini kullanın. 

Azure Kaynak Grafiği 'ni kullanarak aboneliklerinde kaynakları sorgulama/bulma.  Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun. 

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: Şu yerleşik ilke tanımlarını kullanarak müşteri aboneliklerde oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın: izin verilen kaynak türleri izin verilen kaynak türleri

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6,11: kullanıcıların betikler aracılığıyla Azure Resources Manager ile etkileşim kurma yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak üzere Azure koşullu erişimini yapılandırın.

- [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: hassas veya yüksek riskli Azure işlevi uygulamaları için yalıtım sağlamak üzere ayrı abonelikler ve/veya yönetim grupları uygulayın.

Yüksek riskli Azure Işlevi uygulamalarını kendi sanal ağına (VNet) dağıtın. Azure Işlevlerinde çevre güvenliği sanal ağlar aracılığıyla sağlanır. Premium planda veya App Service Ortamı (Ao) üzerinde çalışan işlevler VNET 'ler ile tümleştirilebilir. Kullanım durumu için en iyi mimariyi seçin.

- [Azure İşlevleri ağ seçenekleri](./functions-networking-options.md)

- [Azure Işlevleri Premium planı](./functions-scale.md#premium-plan)

- [App Service Ortamında ağ konusunda dikkat edilmesi gerekenler](../app-service/environment/network-info.md)

- [Dış Ao oluşturma](../app-service/environment/create-external-ase.md)

İç ATıCı oluşturma:

- [https://docs.microsoft.com/azure/app-service/environment/create-ilb-as](../virtual-network/quick-create-portal.md)

- [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: Azure Ilkesi Ile Azure işlev uygulamanız için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure Işlevleri uygulamalarınızın yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Web" ad alanındaki Azure Ilke diğer adlarını kullanın. Ayrıca, gibi yerleşik ilke tanımlarından da yararlanabilirsiniz:
- Yönetilen kimlik İşlev Uygulaması kullanılmalıdır
- Işlev uygulamaları için uzaktan hata ayıklama kapatılmalıdır
- İşlev Uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu kılavuz IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Şirket içi işlevleri dağıtmak mümkün olsa da, bu kılavuz IaaS işlem kaynaklarına yöneliktir. Şirket içi işlevlerde dağıtım yaparken ortamınızın güvenli yapılandırmasından siz sorumlusunuz.

- [Şirket içi işlevleri anlama](./functions-runtime-install.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Rehberlik**: kaynak denetiminde ARM şablonlarını ve özel Azure ilke tanımlarını güvenli bir şekilde depolayın ve yönetin.

- [Kod olarak altyapı nedir?](/azure/devops/learn/what-is-infrastructure-as-code)

- [Kod iş akışları olarak tasarım ilkesi](../governance/policy/concepts/policy-as-code.md)

- [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure Repos belgeleri](/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu kılavuz IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Rehberlik**: yerleşik Azure ilke tanımlarını ve "Microsoft. Web" ad alanındaki Azure ilkesi diğer adlarını kullanarak uyarı, denetim ve sistem yapılandırmalarının uygulanmasını sağlayacak özel ilkeler oluşturun. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu kılavuz IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

**Rehberlik**: yerleşik Azure ilke tanımlarını ve "Microsoft. Web" ad alanındaki Azure ilkesi diğer adlarını kullanarak uyarı, denetim ve sistem yapılandırmalarının uygulanmasını sağlayacak özel ilkeler oluşturun. Azure kaynaklarınızın yapılandırmasını otomatik olarak zorlamak için [Denetim], [reddetme] ve [dağıtım yoksa dağıt] Azure ilkesini kullanın.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu kılavuz IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvenli hale getirmek üzere Azure Key Vault Ile birlikte yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmasını sağlar.

- [Key Vault oluşturma](../key-vault/secrets/quick-create-portal.md)

- [App Service ve Azure Işlevleri için Yönetilen kimlikler kullanma](../app-service/overview-managed-identity.md)

- [Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama](../key-vault/general/managed-identity.md)

- [App Service ve Azure Işlevleri için Key Vault başvurularını kullanma](../app-service/app-service-key-vault-references.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: Azure Işlev UYGULAMANıZı Azure AD 'de otomatik olarak yönetilen bir kimlikle sağlamak Için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

- [App Service ve Azure Işlevleri için Yönetilen kimlikler kullanma](../app-service/overview-managed-identity.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder. 

- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu kılavuz IaaS işlem kaynaklarına yöneliktir.

Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel alınan konakta (örneğin, Azure Işlevleri) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: uygulanamaz; Bu öneri, verileri depolamak için tasarlanan işlem dışı kaynaklar için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, verileri depolamak için tasarlanan işlem dışı kaynaklar için tasarlanmıştır.

Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel alınan konakta (örneğin, Azure Işlevleri) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: uygulamanızın düzenli yedeklemelerini zamanlamak için yedekleme ve geri yükleme özelliğini kullanın. Premium planda çalışan işlev uygulamalarının, "yedekleme ve geri yükleme" özelliğini içeren Azure App Service Web uygulamalarıyla aynı barındırma özelliklerine sahip olması gerekir.

Ayrıca, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos ve Azure DevOps gibi bir kaynak denetimi çözümünü de kullanın. Azure DevOps Services, donanım hatası, hizmet kesintisi veya bölge olağanüstü durum durumunda veri kullanılabilirliği sağlamak için Azure depolama özelliklerinin çoğunun yararlanır. Ayrıca, Azure DevOps ekibi, verileri yanlışlıkla veya kötü amaçlı olarak silinmeye karşı korumak için prosedürleri izler.

- [Uygulamanızı Azure’a yedekleme](../app-service/manage-backup.md)

- [Azure DevOps 'da veri kullanılabilirliğini anlama](/azure/devops/organizations/security/data-protection?view=azure-devops#data-availability)

- [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure Repos belgeleri](/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: uygulamanızın düzenli yedeklemelerini zamanlamak için yedekleme ve geri yükleme özelliğini kullanın. Premium planda çalışan işlev uygulamalarının, "yedekleme ve geri yükleme" özelliğini içeren Azure App Service Web uygulamalarıyla aynı barındırma özelliklerine sahip olması gerekir. Müşteri tarafından yönetilen anahtarları Azure Key Vault içinde yedekleyin.

Ayrıca, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos ve Azure DevOps gibi bir kaynak denetimi çözümünü de kullanın. Azure DevOps Services, donanım hatası, hizmet kesintisi veya bölge olağanüstü durum durumunda veri kullanılabilirliği sağlamak için Azure depolama özelliklerinin çoğunun yararlanır. Ayrıca, Azure DevOps ekibi, verileri yanlışlıkla veya kötü amaçlı olarak silinmeye karşı korumak için prosedürleri izler.

- [Uygulamanızı Azure’a yedekleme](../app-service/manage-backup.md)

- [Azure 'da Anahtar Kasası anahtarlarını yedekleme](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

- [Azure DevOps 'da veri kullanılabilirliğini anlama](/azure/devops/organizations/security/data-protection?view=azure-devops#data-availability)

- [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure Repos belgeleri](/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: yedekleme ve geri yükleme özelliğinden düzenli aralıklarla geri yükleme gerçekleştirme olanağı sağlayın. Kodunuzu yedeklemek için başka bir çevrimdışı konum kullanılıyorsa, düzenli aralıklarla tüm geri yüklemeler gerçekleştirme yeteneği sağlar. Yedeklenen müşteri tarafından yönetilen anahtarların test geri yüklenmesi.

- [Azure 'da bir uygulamayı yedekten geri yükleme](../app-service/web-sites-restore.md)

- [Azure 'da bir uygulamayı bir anlık görüntüden geri yükleme](../app-service/app-service-web-restore-snapshots.md)

- [Azure 'da Anahtar Kasası anahtarlarını geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: yedekleme ve geri yükleme özelliğinden yedeklemeler, aboneliğinizde bir Azure depolama hesabı kullanır. Azure depolama, bekleyen bir depolama hesabındaki tüm verileri şifreler. Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, depolama verilerinin şifrelenmesi için müşteri tarafından yönetilen anahtarlar sağlayabilirsiniz.

Müşteri tarafından yönetilen anahtarlar kullanıyorsanız, Key Vault ' deki geçici silme özelliğinin yanlışlıkla veya kötü amaçlı silme ile karşı korunması için etkinleştirildiğinden emin olun.

- [Azure Depolama bekleyen verileri şifreleme](../storage/common/storage-service-encryption.md)

- [Key Vault 'da geçici silme özelliğini etkinleştirme](../storage/blobs/soft-delete-overview.md?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

- [Azure Güvenlik Merkezi 'nde Iş akışı otomasyonlarını yapılandırma](../security-center/security-center-planning-and-operations-guide.md)

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu da kullanabilir](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

- [NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.  Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

- [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz.

- [Sürekli dışarı aktarmayı yapılandırma](../security-center/continuous-export.md)

- [Uyarıları Azure Sentinel 'e akış](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Kılavuz**: Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanarak güvenlik uyarılarına yönelik yanıtları ve Logic Apps önerileri otomatik olarak tetikleyin.

- [Iş akışı otomasyonu ve Logic Apps yapılandırma](../security-center/workflow-automation.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin. Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın.

- [Katılım test kurallarını oyma](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Bulut Kırmızı ekip oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin
