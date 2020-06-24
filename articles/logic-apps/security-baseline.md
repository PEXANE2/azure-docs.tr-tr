---
title: Logic Apps için Azure Güvenlik temeli
description: Logic Apps için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 06/22/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: a33d3e8b330f5ee8549f536bbfba958d96d8e59b
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85256058"
---
# <a name="azure-security-baseline-for-logic-apps"></a>Logic Apps için Azure Güvenlik temeli

Logic Apps için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

**Rehberlik**: "genel," çok kiracılı Logic Apps hizmetinde çalışan bağlayıcılar Microsoft tarafından dağıtılır ve yönetilir. Bu bağlayıcılar; Office 365, Azure Blob depolama, SQL Server, Dynamics, Salesforce, SharePoint ve daha fazlası dahil olmak üzere bulut hizmetlerine, şirket içi sistemlere veya her ikisine de erişmek için Tetikleyiciler ve eylemler sağlar. Ağ güvenlik grubunuzdaki veya Azure Güvenlik duvarınızdaki kuralları belirtirken ilgili kaynaklara erişime izin vermek için AzureConnectors Service etiketini kullanabilirsiniz.

Bir Azure sanal ağındaki kaynaklara doğrudan erişmesi gereken Logic Apps için, mantıksal uygulamalarınızı özel kaynaklarda oluşturabileceğiniz, dağıtabileceğiniz ve çalıştırabileceğiniz bir tümleştirme hizmeti ortamı (ıSE) oluşturabilirsiniz. Azure depolama, Azure Cosmos DB, Azure SQL veritabanı, iş ortağı hizmetleri veya Azure 'da barındırılan müşteri hizmetleri gibi Azure PaaS hizmetlerine erişim sağlamak için bazı Azure sanal ağları özel uç noktalar (Azure özel bağlantı) kullanır. Mantıksal uygulamalarınızın özel uç noktaları kullanan sanal ağlara erişmesi gerekiyorsa, bu mantık uygulamalarını bir ıSE içinde oluşturmanız, dağıtmanız ve çalıştırmanız gerekir.

ISE 'yi oluşturduğunuzda, iç veya dış erişim uç noktaları kullanmayı tercih edebilirsiniz. Seçiminiz, işinizdeki Logic Apps 'teki istek veya Web kancasının, sanal ağınızın dışından çağrı alıp alamayacağını belirler. İç ve dış erişim uç noktaları Ayrıca, sanal ağınızın içinden veya dışından bir çalıştırmanın giriş ve çıkışları dahil olmak üzere mantıksal uygulamanızın çalıştırma geçmişini görüntüleyip görüntüleyemeyeceğinizi de etkiler.

ISE ile ilgili tüm sanal ağ alt ağ dağıtımlarının, uygulamanızın güvenilen bağlantı noktalarına ve kaynaklarına özel ağ erişim denetimleriyle uygulanmış bir ağ güvenlik grubu olduğundan emin olun. Logic Apps 'i bir ıSE dağıtırken, özel bağlantı kullanın. Azure özel bağlantısı, sanal ağınızdaki özel bir uç nokta üzerinden Azure PaaS hizmetlerine ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine erişmenizi sağlar. Alternatif olarak, belirli bir kullanım durumu varsa, Azure Güvenlik Duvarı 'Nı uygulayarak bu gereksinimi karşılayabilirsiniz. Güvenlik kurallarını ayarlarken karmaşıklığı azaltmaya yardımcı olmak için, belirli bir Azure hizmeti için IP adresi ön eklerinin gruplarını temsil eden hizmet etiketlerini kullanın.

* [Logic Apps için bağlayıcıları anlayın](https://docs.microsoft.com/azure/connectors/apis-list)

* [Azure 'da hizmet etiketlerini anlama](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

* [Tümleştirme hizmeti ortamlarını (sesleri) kullanarak Azure Logic Apps Azure sanal ağ kaynaklarına erişimi anlayın](https://docs.microsoft.com/azure/logic-apps/connect-virtual-network-vnet-isolated-environment-overview)

* [Sanal ağ hizmeti uç noktalarını anlama](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)

* [Azure özel bağlantısını anlama](https://docs.microsoft.com/azure/private-link/private-link-overview)

* [ISE uç noktası erişimini anlama](https://docs.microsoft.com/azure/logic-apps/connect-virtual-network-vnet-isolated-environment-overview#ise-endpoint-access)

* [Sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

* [Güvenlik Yapılandırması ile NSG oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

* [Azure Güvenlik duvarını dağıtma ve yapılandırma](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

* [ISE için erişimi etkinleştirme](https://docs.microsoft.com/azure/logic-apps/connect-virtual-network-vnet-isolated-environment#enable-access-for-ise)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: paylaşılan

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: sanal ağların, alt ağların ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Kılavuz**: bir dış erişim noktası kullanan bir tümleştirme hizmeti ortamında (ISE) Logic Apps çalıştırırsanız, veri sızdırma riskini azaltmak için bir ağ güvenlik grubu (NSG) kullanabilirsiniz. Trafik denetimi için NSG akış günlüklerini etkinleştirin ve günlükleri bir Azure depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

* [ISE uç noktası erişimini anlama](https://docs.microsoft.com/azure/logic-apps/connect-virtual-network-vnet-isolated-environment-overview#ise-endpoint-access)

* [NSG akış günlüklerini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Trafik Analizi etkinleştirme ve kullanma](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: mantıksal uygulamanız, Istek veya Web kancası tetikleyicisi gibi gelen çağrıları veya istekleri alan istek tabanlı bir tetikleyici kullanıyorsa, yalnızca yetkili istemcilerin mantıksal uygulamanızı çağırabilmesi için erişimi sınırlayabilirsiniz.

Bir tümleştirme hizmeti ortamında (ıSE) Logic Apps çalıştırıyorsanız, DDoS saldırılarına karşı koruma sağlamak için ıSE ile ilişkili sanal ağ üzerinde DDoS koruma standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

Tehdit zekası etkinleştirilmiş ve kötü amaçlı ağ trafiği için "uyarı ve reddetme" olarak yapılandırılmış bir kuruluşun ağ sınırlarının her birinde Azure Güvenlik Duvarı 'Nı dağıtın.

NSG 'leri, sınırlı bir süre için bitiş noktaları onaylı IP adresleriyle sınırlamak üzere Azure Güvenlik Merkezi 'ni tam zamanında ağ erişimi ile kullanın.

Bağlantı noktalarını ve kaynak IP 'Leri gerçek trafik ve tehdit zeka göre sınırlayan NSG yapılandırmalarının kullanılması için Azure Güvenlik Merkezi Uyarlamalı ağ sağlamlaştırma kullanın.

* [Logic Apps gelen çağrıları güvenli hale getirme](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-request-based-triggers)

* [Gelen IP adreslerini kısıtlama](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#restrict-inbound-ip-addresses)

* [DDoS korumasını yapılandırma](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

* [Azure Güvenlik duvarını dağıtma](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

* [Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer)

* [Azure Güvenlik Merkezi Uyarlamalı ağ sağlamlaştırma 'yi anlama](https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening)

* [Azure Güvenlik Merkezi 'Ni tam zamanında ağ Access Control anlama](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="15-record-network-packets"></a>1,5: ağ paketlerini kaydetme

**Kılavuz**: bir dış erişim noktası kullanan bir tümleştirme hizmeti ortamında (ISE) Logic Apps çalıştırırsanız, veri sızdırma riskini azaltmak için bir ağ güvenlik grubu (NSG) kullanabilirsiniz. Trafik denetimi için NSG akış günlüklerini etkinleştirin ve günlükleri bir Azure depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

Ağ trafiğiyle ilgili daha fazla koruma ve bilgi sağlamak için, yalnızca bunları her bir Application Gateway örneği üzerinde etkinleştirdiyseniz oluşturulan erişim günlüklerine başvurabilirsiniz. Bu günlüğü, Application Gateway erişim düzenlerini görüntülemek ve önemli bilgileri çözümlemek için kullanabilirsiniz. Bu, arayanın IP, istenen URL, yanıt gecikmesi, dönüş kodu ve gelen ve giden baytları içerir.

Aksi takdirde, bu gereksinimi karşılamak için Market 'ten bir üçüncü taraf çözümünü kullanabilirsiniz.

* [ISE uç noktası erişimini anlama](https://docs.microsoft.com/azure/logic-apps/connect-virtual-network-vnet-isolated-environment-overview#ise-endpoint-access)

* [NSG akış günlüklerini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Trafik Analizi etkinleştirme ve kullanma](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [Application Gateway ile iç VNET 'te API Management tümleştirme](https://docs.microsoft.com/azure/api-management/api-management-howto-integrate-internal-vnet-appgateway)

* [WAF erişim günlüklerini anlama](https://docs.microsoft.com/azure/web-application-firewall/ag/web-application-firewall-logs#access-log)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Kılavuz**: Azure Marketi 'nden, yük İnceleme ÖZELLIKLERI ile KIMLIKLER/IP işlevlerini destekleyen bir teklif seçin. Yük incelemesini temel alan yetkisiz giriş algılama ve/veya önleme bir gereksinim değilse, tehdit zekasıyla Azure Güvenlik Duvarı kullanılabilir. Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen ve giden trafiği uyarabilir ve reddedebilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklıdır.

Kötü amaçlı trafiği algılamak ve/veya reddetmek için kuruluşunuzun ağ sınırlarının her birinde tercih ettiğiniz güvenlik duvarı çözümünü dağıtın.

* [Azure Market](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

* [Azure Güvenlik duvarını dağıtma](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

* [Azure Güvenlik Duvarı ile uyarıları yapılandırma](https://docs.microsoft.com/azure/firewall/threat-intel)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: bir tümleştirme hizmeti ortamında (ISE) Logic Apps çalıştırıyorsanız Azure Application Gateway dağıtın.

* [Application Gateway ile iç VNET 'te API Management tümleştirme](https://docs.microsoft.com/azure/api-management/api-management-howto-integrate-internal-vnet-appgateway)

* [Application Gateway HTTPS kullanacak şekilde yapılandırma](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)

* [Azure Web uygulaması ağ geçitleri ile katman 7 yük dengelemesini anlama](https://docs.microsoft.com/azure/application-gateway/overview)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: Azure Logic Apps örneklerine erişmesi gereken kaynaklar için, ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak üzere sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örneğin, LogicApps, LogicAppsManagement) belirterek, karşılık gelen hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

* [Hizmet etiketlerini kullanma hakkında daha fazla bilgi için](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure ilkesiyle Azure Logic Apps örneklarınızla ilgili ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure Logic Apps örneklerinizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için, "Microsoft. Logic" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. Ayrıca, gibi yerleşik ilke tanımlarından da yararlanabilirsiniz:

Logic Apps tanılama günlükleri etkinleştirilmelidir

DDoS koruma standardı etkinleştirilmelidir

Ayrıca, tek bir şema tanımında Azure Resource Manager şablonları, rol tabanlı erişim denetimi (RBAC) ve ilkeler gibi anahtar ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Şema 'i yeni abonelikler ve ortamlara kolayca uygulayın ve sürüm oluşturma aracılığıyla denetimi ve yönetimi ayrıntılı olarak ayarlayın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Blueprint oluşturma](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: bireysel NSG kuralları Için "Açıklama" alanını kullanarak ağa giden/giden trafiğe izin veren kuralların iş gereksinimini ve/veya süresini (vs.) belirtin.

Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure Ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

* [Güvenlik Yapılandırması ile NSG oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

* [Logic Apps için Azure Ilke tanımlarının listesi](https://docs.microsoft.com/azure/logic-apps/policy-samples)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Rehberlik**: ağ kaynak yapılandırmasını izlemek ve Azure Logic Apps örneklarınızla ilgili ağ kaynaklarına yönelik değişiklikleri algılamak Için Azure etkinlik günlüğünü kullanın. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

* [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Azure Izleyici 'de uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, günlüklerde zaman damgalarına yönelik Azure Logic Apps gibi Azure kaynakları için kullanılan saat kaynağını korur.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Kılavuz**: çalışma zamanı sırasında mantıksal uygulamalarınız hakkında daha zengin hata ayıklama bilgileri almak Için Azure izleyici günlüklerini ayarlayıp kullanarak, olayları tetikleme, çalıştırma olayları ve Log Analytics çalışma alanında eylem olayları gibi çalışma zamanı verileri ve olayları hakkında bilgi kaydedebilirsiniz. Azure Izleyici, kullanılabilirliğini ve performansını daha kolay koruyabilmeniz için bulutunuzu ve şirket içi ortamlarınızı izlemenize yardımcı olur. Azure Izleyici günlüklerini kullanarak, bu bilgileri toplayıp gözden geçirmenize yardımcı olan günlük sorguları oluşturabilirsiniz. Bu tanılama verilerini Azure depolama ve Azure Event Hubs gibi diğer Azure hizmetleriyle de kullanabilirsiniz.

Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Azure Izleyici günlüklerini ayarlama ve Azure Logic Apps için tanılama verilerini toplama](https://docs.microsoft.com/azure/logic-apps/monitor-logic-apps-log-analytics)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: çalışma zamanı sırasında mantıksal uygulamalarınız hakkında daha zengin hata ayıklama bilgileri almak Için Azure izleyici günlüklerini ayarlayıp kullanarak, olayları tetikleme, çalıştırma olayları ve Log Analytics çalışma alanında eylem olayları gibi çalışma zamanı verileri ve olayları hakkında bilgi kaydedebilirsiniz. Azure Izleyici, kullanılabilirliğini ve performansını daha kolay koruyabilmeniz için bulutunuzu ve şirket içi ortamlarınızı izlemenize yardımcı olur. Azure Izleyici günlüklerini kullanarak, bu bilgileri toplayıp gözden geçirmenize yardımcı olan günlük sorguları oluşturabilirsiniz. Bu tanılama verilerini Azure depolama ve Azure Event Hubs gibi diğer Azure hizmetleriyle de kullanabilirsiniz.

Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Azure Izleyici günlüklerini ayarlama ve Azure Logic Apps için tanılama verilerini toplama](https://docs.microsoft.com/azure/logic-apps/monitor-logic-apps-log-analytics)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Rehberlik**: bir mantıksal uygulama oluşturup çalıştırdıktan sonra, bu mantıksal uygulamanın çalıştırma durumunu, çalışma geçmişi, tetikleme geçmişini ve performansı kontrol edebilirsiniz. Gerçek zamanlı olay izleme ve daha zengin hata ayıklama için, Azure Izleyici günlüklerini kullanarak mantıksal uygulamanız için tanılama günlüğü ayarlayın. Bu Azure hizmeti, kullanılabilirliğini ve performansını daha kolay koruyabilmeniz için bulutunuzu ve şirket içi ortamlarınızı izlemenize yardımcı olur. Daha sonra olayları tetikleme, olayları çalıştırma ve eylem olayları gibi olayları bulabilir ve görüntüleyebilirsiniz. Bu bilgileri Azure Izleyici günlüklerinde depolayarak, bu bilgileri bulmanıza ve çözümlemenize yardımcı olan günlük sorguları oluşturabilirsiniz. Bu tanılama verilerini Azure depolama ve Azure Event Hubs gibi diğer Azure hizmetleriyle de kullanabilirsiniz.

Azure Izleyici 'de, kuruluşunuzun uyumluluk düzenlemelerine göre Azure Logic Apps örnekleriyle ilişkili Günlükler için günlük tutma süresini ayarlayın.

* [Çalışma durumunu izleme, tetikleyici geçmişini inceleme ve Azure Logic Apps için uyarıları ayarlama](https://docs.microsoft.com/azure/logic-apps/monitor-logic-apps)

* [Günlük tutma parametrelerini ayarlama](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Kılavuz**: mantıksal uygulamanız için günlük kaydı ayarlamak için, mantıksal uygulamanızı oluştururken Log Analytics etkinleştirebilir veya mevcut Logic Apps için Log Analytics çalışma alanınıza Logic Apps yönetimi çözümünü yükleyebilirsiniz. Bu çözüm, mantıksal uygulama çalışmalarınız için toplu bilgiler sağlar ve durum, yürütme süresi, yeniden gönderme görevi durumu ve bağıntı kimlikleri gibi belirli ayrıntıları içerir. Daha sonra, bu bilgiler için günlüğe kaydetmeyi etkinleştirmek ve sorgular oluşturmak için Azure Izleyici günlüklerini ayarlayın.

Azure etkinlik günlüğü tanılama ayarlarını da etkinleştirebilir ve günlükleri bir Log Analytics çalışma alanına gönderebilirsiniz. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve Azure Logic Apps için toplanabilecek etkinlik günlüğü verilerine göre birçok diğer öngörü sağlamak için Log Analytics sorguları gerçekleştirin.

Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

* [Azure Izleyici günlüklerini ayarlama ve Azure Logic Apps için tanılama verilerini toplama](https://docs.microsoft.com/azure/logic-apps/monitor-logic-apps-log-analytics)

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Azure Izleyici 'de Log Analytics Azure etkinlik günlüklerini toplama ve çözümleme](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-collect)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Rehberlik**: güvenlik günlükleri ve olayları 'nda bulunan anormal etkinlikleri izlemek ve uyarmak için Log Analytics Ile Azure Güvenlik Merkezi 'ni kullanın.

Alternatif olarak, Azure Sentinel 'de ve yerleşik verileri etkinleştirebilir.

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Azure Güvenlik Merkezi 'nde uyarıları yönetme](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

* [Log Analytics günlük verilerinde uyarı alma](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; Azure Logic Apps kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Azure Logic Apps DNS ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

Azure Active Directory (Azure AD) tarafından korunan diğer kaynaklara kolayca erişmek ve oturum açmadan kimliğinizi doğrulamak için, mantıksal uygulamanız kimlik bilgileri veya gizli dizileri yerine yönetilen bir kimlik (eski adıyla Yönetilen Hizmet Kimliği veya MSI) kullanabilir. Azure bu kimliği sizin için yönetir ve gizli dizileri sağlamanız veya döndürmenize gerek olmadığından kimlik bilgilerinizin güvenliğini sağlamaya yardımcı olur.

Bir mantıksal uygulamadaki her istek uç noktası, uç noktanın URL 'sinde bir paylaşılan erişim Imzasına (SAS) sahiptir. Diğer kişilerle istek tabanlı bir tetikleyici için uç nokta URL 'SI paylaşırsanız, belirli anahtarları kullanan ve sona erme tarihleri olan geri çağırma URL 'Leri oluşturabilirsiniz. Bu şekilde, belirli bir TimeSpan temelinde, mantıksal uygulamanızı tetikleyerek anahtarları sorunsuzca alabilir veya erişimi kısıtlayabilirsiniz.

* [Azure AD 'de PowerShell ile dizin rolü alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

* [Azure Logic Apps içindeki yönetilen kimlikleri kullanarak Azure kaynaklarına erişimi kimlik doğrulaması](https://docs.microsoft.com/azure/logic-apps/create-managed-service-identity)

* [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

* [SAS kullanarak Azure Logic Apps erişimi ve verileri güvenli hale getirme](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-request-based-triggers)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Azure Active Directory ve Azure Logic Apps varsayılan parola kavramına sahip değildir.

Temel kimlik doğrulaması kullanılıyorsa, bir Kullanıcı adı ve parola belirtmeniz gerekir. Bu kimlik bilgilerini oluştururken kimlik doğrulaması için güçlü bir parola yapılandırmadiğinizden emin olun.

Kod olarak altyapı kullanıyorsanız, parolaları kodda depolamaktan kaçının ve bunun yerine kimlik bilgilerini depolamak ve almak için Azure Key Vault kullanın.

* [Logic Apps verileri güvenli hale getirme ve verilere erişme](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app)

* [Azure Key Vault bir gizli dizi ayarlama ve alma](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Ayrıca, özel yönetim hesaplarını izlemenize yardımcı olmak için Azure Güvenlik Merkezi veya yerleşik Azure Ilkeleri önerilerini kullanabilirsiniz; örneğin:
- Aboneliğinize birden fazla sahip atanmalıdır
- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

* [Kimlik ve erişim (Önizleme) izlemek için Azure Güvenlik Merkezi 'ni kullanma](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Azure Ilkesini kullanma](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: API çağrıları aracılığıyla kurtarma hizmetleri kasalarınızla etkileşim kurmak için kullanılabilecek bir belirteç almak üzere bir Azure uygulama kaydı (hizmet sorumlusu) kullanın.

Birçok bağlayıcı, mantıksal uygulamanızda bir tetikleyici veya eylem kullanabilmeniz için öncelikle hedef hizmet veya sisteme bir bağlantı oluşturmanızı ve kimlik doğrulama kimlik bilgilerini ya da diğer yapılandırma ayrıntılarını sağlamanızı gerektirir. Örneğin, verilere erişim veya sizin adınıza gönderi için Twitter hesabına bir bağlantı yetkilendirmelisiniz.]

Azure Active Directory (Azure AD) OAuth kullanan bağlayıcılar için bağlantı oluşturmak, erişim belirtecinizin şifreli ve güvenli bir şekilde bir Azure gizli deposunda depolandığı Office 365, Salesforce veya GitHub gibi hizmette oturum açma anlamına gelir. FTP ve SQL gibi diğer bağlayıcılar, sunucu adresi, Kullanıcı adı ve parola gibi yapılandırma ayrıntılarına sahip bir bağlantı gerektirir. Bu bağlantı yapılandırma ayrıntıları da şifrelenir ve güvenli bir şekilde depolanır.

* [Azure REST API 'Lerini çağırma](https://docs.microsoft.com/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

* [Azure AD ile istemci uygulamanızı (hizmet sorumlusu) kaydetme](https://docs.microsoft.com/rest/api/azure/#register-your-client-application-with-azure-ad)

* [İş akışı API bilgilerini tetikler](https://docs.microsoft.com/rest/api/logic/workflowtriggers)

* [Bağlayıcı yapılandırmasını anlama](https://docs.microsoft.com/azure/connectors/apis-list])

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure ACTIVE DIRECTORY (AD) MULTI-Factor AUTHENTICATION (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

* [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve yapılandırmak için yapılandırılmış MULTI-Factor AUTHENTICATION (MFA) ile ayrıcalıklı erişim iş istasyonları (Paw) kullanın.

* [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik oluştuğunda günlükler ve uyarılar oluşturmak için Azure ACTIVE DIRECTORY (AD) PRIVILEGED IDENTITY Management (PIM) kullanın.

Ayrıca, riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın.

* [Privileged Identity Management dağıtma (PıM)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

* [Azure AD risk algılamalarını anlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından Azure Portal erişimine izin vermek Için, koşullu erişim adlı konum kullanın.

Ayrıca, bir mantıksal uygulamadaki her istek uç noktası, uç noktanın URL 'sinde bir paylaşılan erişim Imzasına (SAS) sahiptir. Mantıksal uygulamanızı yalnızca belirli IP adreslerinden gelen istekleri kabul edecek şekilde kısıtlayabilirsiniz.

* [Azure 'da adlandırılmış konumları yapılandırma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

* [Logic Apps gelen IP adreslerini nasıl kısıtlayacağınızı anlayın](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#restrict-inbound-ip-addresses)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: Azure Logic Apps örneklerinizin merkezi kimlik doğrulaması ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

Logic Apps desteklendiği yerlerde, Azure Active Directory (Azure AD) tarafından korunan diğer kaynaklara kolayca erişmek ve kimlik bilgileri veya gizli dizileri yerine oturum açmadan kimliğinizi doğrulamak için yönetilen bir kimlik kullanın. Azure bu kimliği sizin için yönetir ve gizli dizileri sağlamanız veya döndürmenize gerek olmadığından kimlik bilgilerinizin güvenliğini sağlamaya yardımcı olur.

Azure Logic Apps hem sistem tarafından atanan hem de Kullanıcı tarafından atanan yönetilen kimlikleri destekler. Mantıksal uygulamanız, sistem tarafından atanan kimlik veya tek bir kullanıcı tarafından atanan kimlik kullanabilir, bu da bir Logic Apps grubu genelinde paylaşabilir, ancak ikisini birden kullanamazsınız. Şu anda yalnızca belirli yerleşik Tetikleyiciler ve Eylemler, yönetilmeyen bağlayıcılar veya bağlantılar değil, yönetilen kimlikleri destekler, örneğin:
- HTTP
- Azure İşlevleri
- Azure API Management
- Azure Uygulama Hizmetleri

* [Azure AD örneği oluşturma ve yapılandırma](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

* [Azure Logic Apps içindeki yönetilen kimlikleri kullanarak Azure kaynaklarına erişimi kimlik doğrulaması](https://docs.microsoft.com/azure/logic-apps/create-managed-service-identity)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) eski hesapları keşfetmenize yardımcı olacak Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir.

* [Azure AD raporlamayı anlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [Azure kimlik erişimi Incelemelerini kullanma](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Rehberlik**: Azure Logic Apps örneklerinizin merkezi kimlik doğrulaması ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

Azure AD oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişerek Azure Sentinel veya bir üçüncü taraf SıEM ile tümleştirmenize olanak tanır.

Bu işlemi, Azure AD Kullanıcı hesapları için Tanılama ayarları oluşturarak ve Log Analytics çalışma alanına denetim günlüklerini ve oturum açma günlüklerini göndererek kolaylaştırabilirsiniz. Log Analytics içinde, istenen günlük uyarılarını yapılandırabilirsiniz.

* [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

* [Yerleşik Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırmak IÇIN Azure AD risk ve kimlik koruması özelliklerini kullanın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz.

* [Azure AD riskli oturum açma işlemlerini görüntüleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: Şu anda kullanılamıyor; Müşteri Kasası, Azure Logic Apps için henüz desteklenmiyor.

* [Müşteri Kasası tarafından desteklenen hizmetler listesi](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: "genel," çok kiracılı Logic Apps hizmetinde çalışan bağlayıcılar Microsoft tarafından dağıtılır ve yönetilir. Bu bağlayıcılar; Office 365, Azure Blob depolama, SQL Server, Dynamics, Salesforce, SharePoint ve daha fazlası dahil olmak üzere bulut hizmetlerine, şirket içi sistemlere veya her ikisine de erişmek için Tetikleyiciler ve eylemler sağlar.

Bir Azure sanal ağındaki kaynaklara doğrudan erişmesi gereken Logic Apps için, mantıksal uygulamalarınızı özel kaynaklarda oluşturabileceğiniz, dağıtabileceğiniz ve çalıştırabileceğiniz bir tümleştirme hizmeti ortamı (ıSE) oluşturabilirsiniz. Azure depolama, Azure Cosmos DB veya Azure SQL veritabanı, iş ortağı hizmetleri veya Azure 'da barındırılan müşteri hizmetleri gibi Azure PaaS hizmetlerine erişim sağlamak için bazı Azure sanal ağları özel uç noktalar (Azure özel bağlantı) kullanır. Mantıksal uygulamalarınızın özel uç noktaları kullanan sanal ağlara erişmesi gerekiyorsa, bu mantık uygulamalarını bir ıSE içinde oluşturmanız, dağıtmanız ve çalıştırmanız gerekir.

ISE 'yi oluşturduğunuzda, iç veya dış erişim uç noktaları kullanmayı tercih edebilirsiniz. Seçiminiz, işinizdeki Logic Apps 'teki istek veya Web kancasının, sanal ağınızın dışından çağrı alıp alamayacağını belirler.

Ayrıca, ortam türü ve veri duyarlılığı düzeyi gibi bireysel güvenlik etki alanları için ayrı abonelikler ve yönetim grupları kullanarak yalıtım uygulayın. Uygulamalarınızın ve kurumsal ortamların talep ettiği Azure kaynaklarınıza erişim düzeyini kısıtlayabilirsiniz. Azure kaynaklarına erişimi, Azure Active Directory rol tabanlı erişim denetimi aracılığıyla denetleyebilirsiniz.

* [Logic Apps için bağlayıcıları anlayın](https://docs.microsoft.com/azure/connectors/apis-list)

* [Tümleştirme hizmeti ortamlarını (sesleri) kullanarak Azure Logic Apps Azure sanal ağ kaynaklarına erişim](https://docs.microsoft.com/azure/logic-apps/connect-virtual-network-vnet-isolated-environment-overview)

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: Şu anda kullanılamıyor; veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz Azure Logic Apps için kullanılamaz.

Hassas bilgilerin yetkisiz aktarımını izleyen ve bilgi güvenliği uzmanlarına uyarı ederken bu tür aktarımları engelleyen bir üçüncü taraf çözümünü Azure Marketi 'nden yararlanın.

Microsoft, Azure Logic Apps için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: yoldaki tüm hassas bilgileri şifreleyin. Azure Logic Apps, mantıksal uygulama çalıştırma sırasındaki tüm veriler Aktarım Katmanı Güvenliği (TLS) ve bekleyen ' i kullanarak geçiş sırasında şifrelenir. Mantıksal uygulamanızın çalıştırma geçmişini görüntülediğinizde Logic Apps erişiminizin kimliğini doğrular ve ardından her bir çalıştırmaya yönelik istekler ve yanıtlara yönelik giriş ve çıkışlara bağlantılar sağlar. Ancak, herhangi bir parolayı, parolayı, anahtarı veya diğer hassas bilgileri işleyen eylemler için başkalarının bu verilere bakmasını ve erişimini engellemek isteyebilirsiniz. Örneğin, mantıksal uygulamanız bir HTTP eyleminin kimlik doğrulaması sırasında kullanmak üzere Azure Key Vault bir gizli dizi alırsa, bu gizli anahtarı görünümden gizlemek istersiniz.

Istek tetikleyicisi, gelen istekler için yalnızca Aktarım Katmanı Güvenliği (TLS) 1,2 ' i destekler. Azure kaynaklarınıza bağlanan tüm istemcilerin TLS 1,2 veya üzerini anlaşamadığından emin olun. HTTP bağlayıcısını kullanan giden çağrılar Aktarım Katmanı Güvenliği (TLS) 1,0, 1,1 ve 1,2 ' i destekler.

Azure Güvenlik Merkezi önerilerini, varsa, bekleyen ve geçişte şifreleme için kullanın.

* [Azure Logic Apps 'da güvenli erişim ve veriler](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app)

* [Azure Logic Apps gelen HTTPS isteklerini alın ve bunlara yanıt verin](https://docs.microsoft.com/azure/connectors/connectors-native-reqres#tls-support)

* [Azure Logic Apps 'den HTTP veya HTTPS üzerinden hizmet uç noktalarını çağır](https://docs.microsoft.com/azure/connectors/connectors-native-http#tls-support)

* [Azure ile iletim sırasında şifrelemeyi anlama](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

* [Azure ile bekleyen veri şifrelemesini anlama](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)

* [Azure Logic Apps içindeki tümleştirme hizmeti ortamları (sesleri) için bekleyen verileri şifrelemek üzere müşteri tarafından yönetilen anahtarlar ayarlayın](https://docs.microsoft.com/azure/logic-apps/customer-managed-keys-integration-service-environment)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: Azure Logic Apps, birçok tetikleyici ve eylemin, girdileri, çıkışları veya her ikisini de bir mantıksal uygulamanın çalıştırma geçmişinden obscuring göre güvenli hale getirmek için etkinleştirebileceğiniz ayarlar vardır.

Microsoft, Azure Logic Apps için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Çalışma geçmişi verilerine güvenli erişim](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-run-history-data)

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: paylaşılan

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için rol tabanlı erişim denetimi kullanma

**Rehberlik**: mantıksal uygulamaları yönetme, düzenleme ve görüntüleme gibi belirli görevleri yalnızca belirli kullanıcıların veya grupların çalıştırmasına izin verebilirsiniz. İzinlerini denetlemek için, Azure aboneliğinizdeki üyelere özelleştirilmiş veya yerleşik roller atayabilmeniz için Azure rol tabanlı Access Control (RBAC) kullanın:
- Mantıksal uygulama katılımcısı: mantıksal uygulamaları yönetmenizi sağlar, ancak bunlara erişimi değiştiremezsiniz.
- Logic App operatörü: Logic Apps 'i okumanızı, etkinleştirmenizi ve devre dışı bırakmanızı sağlar, ancak bunları düzenleyemez veya güncelleştiremezsiniz.

Başkalarının mantıksal uygulamanızı değiştirmesini veya silmesini engellemek için Azure Kaynak kilidi ' ni kullanabilirsiniz. Bu özellik başkalarının üretim kaynaklarını değiştirmesini veya silmesini engeller.

* [Azure Logic Apps işlemlerine güvenli erişim](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-logic-app-operations)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft, Azure Logic Apps için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Azure müşteri verileri koruması](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: Microsoft

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: Azure Logic Apps bekleyen verileri depolamak ve otomatik olarak şifrelemek Için Azure Storage 'ı kullanır. Bu şifreleme, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur. Azure depolama, verilerinizi şifrelemek için varsayılan olarak Microsoft tarafından yönetilen anahtarları kullanır.

Logic Apps 'i barındırmak için bir tümleştirme hizmeti ortamı (ıSE) oluşturduğunuzda ve Azure Storage tarafından kullanılan şifreleme anahtarları üzerinde daha fazla denetim istiyorsanız, Azure Key Vault kullanarak kendi anahtarınızı ayarlayabilir, kullanabilir ve yönetebilirsiniz. Bu özellik "Kendi Anahtarını Getir" (BYOK) olarak da bilinir ve anahtarınız "müşteri tarafından yönetilen anahtar" olarak adlandırılır.

* [Azure Logic Apps içindeki tümleştirme hizmeti ortamları için bekleyen verileri şifreleme](https://docs.microsoft.com/azure/logic-apps/customer-managed-keys-integration-service-environment)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Rehberlik**: değişikliklerin Azure Logic Apps ve diğer önemli veya ilgili kaynakların yanı sıra değişiklikler gerçekleştiği hakkında uyarı oluşturmak Için Azure etkinlik günlüğü Ile Azure izleyici 'yi kullanın.

* [Azure etkinlik günlüğü olayları için uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: [uygulanamaz; Microsoft, Azure Logic Apps destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.]

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: üçüncü taraf yazılım başlıkları için otomatik düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Microsoft, Azure Logic Apps destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: uygulanamaz; Microsoft, Azure Logic Apps destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: abonelikleriniz dahilinde (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) tüm kaynakları sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerini numaralandırın.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanmanız önemle tavsiye edilir.

* [Azure Kaynak Graf ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [Azure aboneliklerinizi görüntüleme](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Azure RBAC 'yi anlama](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure kaynaklarını düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ayrıca, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri abonelikleri içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure Ilkesi 'ni kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynakları envanterini tanımlama ve sürdürme

**Rehberlik**: Kurumsal gereksinimleriniz uyarınca, işlem kaynakları Için onaylanan Azure kaynaklarının (bağlayıcılar gibi) envanterini ve onaylanan yazılımları oluşturun.

Note: Google 'ın veri ve gizlilik ilkeleri nedeniyle Gmail bağlayıcısını yalnızca Google tarafından onaylanan hizmetlerle kullanabilirsiniz. Bu durum gelişmekte ve gelecekte diğer Google bağlayıcıları etkileyebilir.

* [Tüm Logic Apps bağlayıcıları listesi](https://docs.microsoft.com/connectors/connector-reference/connector-reference-logicapps-connectors)

* [Gmail bağlayıcılarına yönelik sorunları ve sınırlamaları anlayın](https://docs.microsoft.com/connectors/gmail/#known-issues-and-limitations)

* [Google 'ın Gizlilik ilkesi hakkında daha fazla bilgi](https://docs.microsoft.com/azure/connectors/connectors-google-data-security-privacy-policy)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: abonelikleriniz içinde oluşturulabilecek kaynak türlerine yönelik kısıtlamalar koymak Için Azure ilkesini kullanın.

Azure Kaynak Grafiği 'ni kullanarak aboneliklerinde kaynakları sorgulama/bulma. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Kaynak Graf ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilkesiyle belirli bir kaynak türünü reddetme](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: onaylanan yazılım başlıkları envanterini koruyun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak üzere Azure koşullu erişimini yapılandırın.

* [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: iş işlemlerinde gerekli olan, ancak kuruluşa daha fazla risk doğurabilecek Logic Apps ilgili kaynaklar, kendi sanal makinesi ve/veya sanal ağı içinde yalıtılmalı ve bir Azure Güvenlik Duvarı veya ağ güvenlik grubuyla yeterince güvenli hale getirilir.

İş işlemleri için gerekli olan, ancak kuruluş için daha yüksek risk doğurabilecek Logic Apps, belirli izinlere ve RBAC sınırlarına sahip ayrı kaynak grupları aracılığıyla mümkün olan her yerde yalıtılmalıdır.

* [Sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

* [Güvenlik Yapılandırması ile NSG oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [RBAC aracılığıyla Logic Apps erişimi güvenli hale getirme](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-logic-app-operations)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: Azure ilkesiyle Azure Logic Apps örneklerinizin standart güvenlik yapılandırmasını tanımlayın ve uygulayın. Logic Apps örneklerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Logic" ad alanındaki Azure Ilke diğer adlarını kullanın. Örneğin, kullanıcıların erişimi kısıtlamak istediğiniz kaynaklara bağlantı oluşturmasını veya bu bağlantıları kullanmasını engelleyebilirsiniz.

Ayrıca, Azure Resource Manager, yapılandırmanın kuruluşunuzun güvenlik gereksinimlerini karşıladığından/aştığından emin olmak için gözden geçirilmesi gereken JavaScript Nesne Gösterimi (JSON) içinde şablonu dışarı aktarma özelliğine sahiptir.

Ayrıca hassas verileri ve gizli dizileri korumak için güvenli parametreleri kullanın.

* [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Logic Apps içinde bağlayıcılar tarafından oluşturulan bağlantıları engelle](https://docs.microsoft.com/azure/logic-apps/block-connections-connectors)

* [Azure portal bir şablona tek ve çoklu kaynak verme](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

* [Azure Logic Apps için Azure Resource Manager şablonlarını dağıtma](https://docs.microsoft.com/azure/logic-apps/logic-apps-deploy-azure-resource-manager-templates)

* [Güvenli eylem parametrelerini anlama](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#secure-action-parameters)

* [Parametreler için güvenlik önerileri](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-best-practices#security-recommendations-for-parameters)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

Azure Ilkesiyle Azure Logic Apps örneklerinizin standart güvenlik yapılandırmasını tanımlayın ve uygulayın. Logic Apps örneklerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Logic" ad alanındaki Azure Ilke diğer adlarını kullanın. Örneğin, kullanıcıların erişimi kısıtlamak istediğiniz kaynaklara bağlantı oluşturmasını veya bu bağlantıları kullanmasını engelleyebilirsiniz.

Ayrıca, Azure Resource Manager, yapılandırmanın kuruluşunuzun güvenlik gereksinimlerini karşıladığından/aştığından emin olmak için gözden geçirilmesi gereken JavaScript Nesne Gösterimi (JSON) içinde şablonu dışarı aktarma özelliğine sahiptir.

Ayrıca, gizleme kullanarak verileri çalıştırma geçmişinde güvende olduğunuzdan emin olun.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilke efektlerini anlama](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

* [Azure Logic Apps içinde bağlayıcılar tarafından oluşturulan bağlantıları engelle](https://docs.microsoft.com/azure/logic-apps/block-connections-connectors)

* [Azure portal bir şablona tek ve çoklu kaynak verme](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

* [Azure Logic Apps için Azure Resource Manager şablonlarını dağıtma](https://docs.microsoft.com/azure/logic-apps/logic-apps-deploy-azure-resource-manager-templates)

* [Çalışma geçmişi girişleri ve çıkışları için güvenli erişim](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#obfuscate)

* [Parametre girdilerine güvenli erişim](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#secure-action-parameters)

* [Parametreler için güvenlik önerileri](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-best-practices#security-recommendations-for-parameters)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: paylaşılan

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure devops veya Azure Repos kullanın.

Ayrıca, Azure Resource Manager, yapılandırmanın kuruluşunuzun güvenlik gereksinimlerini karşıladığından/aştığından emin olmak için gözden geçirilmesi gereken JavaScript Nesne Gösterimi (JSON) içinde şablonu dışarı aktarma özelliğine sahiptir.

* [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure Repos belgeleri](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

* [Azure portal bir şablona tek ve çoklu kaynak verme](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: yerleşik Azure ilke tanımlarını ve "Microsoft. Logic" ad alanındaki Azure ilkesi diğer adlarını kullanarak uyarı, denetim ve sistem yapılandırmalarının uygulanmasını sağlayacak özel ilkeler oluşturun. Azure kaynaklarınızın ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak üzere Azure Ilke diğer adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Rehberlik**: yerleşik Azure ilke tanımlarını ve "Microsoft. Logic" ad alanındaki Azure ilkesi diğer adlarını kullanarak uyarı, denetim ve sistem yapılandırmalarının uygulanmasını sağlayacak özel ilkeler oluşturun. Azure kaynaklarınızın yapılandırmasını otomatik olarak zorlamak için [Denetim], [reddetme] ve [dağıtım yoksa dağıt] Azure Ilkesini kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: gizleme kullanarak mantıksal uygulama çalıştırma geçmişindeki giriş ve çıkışları güvenli hale getirin. Farklı ortamlarda dağıtım yaparsanız, mantıksal uygulamanızın iş akışı tanımındaki bu ortamlara göre farklılık gösteren değerleri parametreleştirmeyi göz önünde bulundurun. Bu şekilde, mantıksal uygulamanızı dağıtmak, güvenli parametreleri tanımlayarak gizli verileri korumak ve bir parametre dosyası kullanarak bu verileri şablon parametreleri aracılığıyla ayrı girişler olarak geçirmek için Azure Resource Manager şablonu kullanarak sabit kodlanmış verilerden kaçınabilirsiniz. Hassas verileri depolamak ve bu değerleri dağıtımda Key Vault alan güvenli şablon parametrelerini kullanmak için Key Vault kullanabilirsiniz. Daha sonra, parametre dosyanızdaki anahtar kasasına ve gizli anahtarlara başvurabilirsiniz.

Logic Apps 'i barındırmak için bir tümleştirme hizmeti ortamı (ıSE) oluşturduğunuzda ve Azure Storage tarafından kullanılan şifreleme anahtarları üzerinde daha fazla denetim istiyorsanız, Azure Key Vault kullanarak kendi anahtarınızı ayarlayabilir, kullanabilir ve yönetebilirsiniz. Bu özellik "Kendi Anahtarını Getir" (BYOK) olarak da bilinir ve anahtarınız "müşteri tarafından yönetilen anahtar" olarak adlandırılır.

* [Azure Logic Apps çalışma geçmişinde güvenli giriş ve çıkış çıkışları](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#obfuscate)

* [Parametreler için güvenlik önerileri](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-best-practices#security-recommendations-for-parameters)

* [Azure Logic Apps içindeki parametre girdilerine güvenli erişim](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-parameter-inputs)

* [Azure Key Vault kullanarak dağıtım sırasında güvenli parametre değerlerini geçirme](https://docs.microsoft.com/azure/azure-resource-manager/templates/key-vault-parameter)

* [Azure Logic Apps içindeki tümleştirme hizmeti ortamları (sesleri) için bekleyen verileri şifrelemek üzere müşteri tarafından yönetilen anahtarlar ayarlayın](https://docs.microsoft.com/azure/logic-apps/customer-managed-keys-integration-service-environment)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Kılavuz**: Azure Active Directory (Azure AD) tarafından korunan diğer kaynaklara kolayca erişmek ve oturum açmadan kimliğinizi doğrulamak için, mantıksal uygulamanız kimlik bilgileri veya gizli dizileri yerine yönetilen bir kimlik (eski adıyla YÖNETILEN HIZMET KIMLIĞI veya MSI) kullanabilir. Azure bu kimliği sizin için yönetir ve gizli dizileri sağlamanız veya döndürmenize gerek olmadığından kimlik bilgilerinizin güvenliğini sağlamaya yardımcı olur.

Şu anda yalnızca belirli yerleşik Tetikleyiciler ve Eylemler, yönetilmeyen bağlayıcılar veya bağlantılar değil, yönetilen kimlikleri destekler, örneğin:
- HTTP
- Azure İşlevleri
- Azure API Management
- Azure Uygulama Hizmetleri

* [Azure Logic Apps içindeki yönetilen kimlikleri kullanarak Azure kaynaklarına erişimin kimliğini doğrulama](https://docs.microsoft.com/azure/logic-apps/create-managed-service-identity)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: gizleme kullanarak mantıksal uygulama çalıştırma geçmişindeki giriş ve çıkışları güvenli hale getirin. Farklı ortamlarda dağıtım yaparsanız, mantıksal uygulamanızın iş akışı tanımındaki bu ortamlara göre farklılık gösteren değerleri parametreleştirmeyi göz önünde bulundurun. Bu şekilde, mantıksal uygulamanızı dağıtmak, güvenli parametreleri tanımlayarak gizli verileri korumak ve bir parametre dosyası kullanarak bu verileri şablon parametreleri aracılığıyla ayrı girişler olarak geçirmek için Azure Resource Manager şablonu kullanarak sabit kodlanmış verilerden kaçınabilirsiniz. Hassas verileri depolamak ve bu değerleri dağıtımda Key Vault alan güvenli şablon parametrelerini kullanmak için Key Vault kullanabilirsiniz. Daha sonra, parametre dosyanızdaki anahtar kasasına ve gizli anahtarlara başvurabilirsiniz.

Kod içinde kimlik bilgilerini tanımlamak için kimlik bilgisi tarayıcısı da uygulayabilirsiniz. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

* [Azure Logic Apps çalışma geçmişinde güvenli giriş ve çıkış çıkışları](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#obfuscate)

* [Parametreler için güvenlik önerileri](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-best-practices#security-recommendations-for-parameters)

* [Azure Logic Apps içindeki parametre girdilerine güvenli erişim](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-parameter-inputs)

* [Azure Key Vault kullanarak dağıtım sırasında güvenli parametre değerlerini geçirme](https://docs.microsoft.com/azure/azure-resource-manager/templates/key-vault-parameter)

* [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure Logic Apps) etkinleştirilir, ancak müşteri içeriklerde çalıştırılmaz.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft 'un kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure Backup) etkinleştirilir, ancak içeriğiniz üzerinde çalışmaz.

App Service, Data Lake Storage, BLOB depolama vb. gibi işlem dışı Azure kaynaklarına yüklenen tüm dosyaları önceden tarayın.

Depolama hesaplarına yüklenen kötü amaçlı yazılımları algılamak için veri Hizmetleri için Azure Güvenlik Merkezi 'nin tehdit algılamasını kullanın.

* [Azure Cloud Services ve sanal makineler için Microsoft kötü amaçlı yazılımdan koruma 'yı anlayın](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

* [Azure Güvenlik Merkezi 'nin veri Hizmetleri tehdit algılamasını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-data-services)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: bir olağanüstü durum kurtarma (Dr) çözümü uygulayarak verileri koruyabilir, kritik iş işlevlerini destekleyen kaynakları hızlıca geri yükleyebilir ve iş SÜREKLILIĞI (BC) sürdürmek için işlemleri çalıştırmaya devam edebilirsiniz.

Bu olağanüstü durum kurtarma stratejisi, bir bekleme veya yedekleme mantıksal uygulamasına yük devretme için birincil mantıksal uygulamanızı, Azure Logic Apps de kullanılabildiği alternatif bir konuma ayarlamaya odaklanır. Bu şekilde, birincil, kayıpları, kesintiler veya hatalara sahip olursa ikincil iş üzerinde işlem yapabilir. Bu strateji, ikincil mantıksal uygulamanızın ve bağımlı kaynaklarınızın zaten dağıtılması ve alternatif konuma hazırlanmasının gerekli olmasını gerektirir.

Ayrıca, mantıksal uygulamanızın temel alınan iş akışı tanımını bir Azure Resource Manager şablonuna genişletmeniz gerekir. Bu şablon, mantıksal uygulamanızı sağlamak ve dağıtmak için altyapıyı, kaynakları, parametreleri ve diğer bilgileri tanımlar.

* [Azure Logic Apps için iş sürekliliği ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/logic-apps/business-continuity-disaster-recovery-guidance)

* [Azure Resource Manager şablonları kullanarak Azure Logic Apps dağıtımını otomatikleştirme](https://docs.microsoft.com/azure/logic-apps/logic-apps-azure-resource-manager-templates-overview)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: bir olağanüstü durum kurtarma (Dr) çözümü uygulayarak verileri koruyabilir, kritik iş işlevlerini destekleyen kaynakları hızlıca geri yükleyebilir ve iş SÜREKLILIĞI (BC) sürdürmek için işlemleri çalıştırmaya devam edebilirsiniz.

Bu olağanüstü durum kurtarma stratejisi, bir bekleme veya yedekleme mantıksal uygulamasına yük devretme için birincil mantıksal uygulamanızı, Azure Logic Apps de kullanılabildiği alternatif bir konuma ayarlamaya odaklanır. Bu şekilde, birincil, kayıpları, kesintiler veya hatalara sahip olursa ikincil iş üzerinde işlem yapabilir. Bu strateji, ikincil mantıksal uygulamanızın ve bağımlı kaynaklarınızın zaten dağıtılması ve alternatif konuma hazırlanmasının gerekli olmasını gerektirir.

Ayrıca, mantıksal uygulamanızın temel alınan iş akışı tanımını bir Azure Resource Manager şablonuna genişletmeniz gerekir. Bu şablon, mantıksal uygulamanızı sağlamak ve dağıtmak için altyapıyı, kaynakları, parametreleri ve diğer bilgileri tanımlar.

Bir mantıksal uygulamadaki her istek uç noktası, uç noktanın URL 'sinde bir paylaşılan erişim Imzasına (SAS) sahiptir. Gizliliklerinizi depolamak için Azure Key Vault kullanıyorsanız, anahtarlarınızın ve URL 'nizin düzenli otomatik yedeklemelerini sağlayın.

* [Azure Logic Apps için iş sürekliliği ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/logic-apps/business-continuity-disaster-recovery-guidance)

* [Azure Resource Manager şablonları kullanarak Azure Logic Apps dağıtımını otomatikleştirme](https://docs.microsoft.com/azure/logic-apps/logic-apps-azure-resource-manager-templates-overview)

* [SAS kullanarak Azure Logic Apps erişimi ve verileri güvenli hale getirme](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-request-based-triggers)

* [Key Vault anahtarlarını yedekleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: olağanüstü durum kurtarma stratejiniz, Azure Logic Apps de kullanılabilir olan alternatif bir konumda bir bekleme veya yedekleme mantıksal uygulamasına yük devretme için birincil mantıksal uygulamanızı ayarlamaya odaklanmalıdır. Bu şekilde, birincil, kayıpları, kesintiler veya hatalara sahip olursa ikincil iş üzerinde işlem yapabilir. Bu strateji, ikincil mantıksal uygulamanızın ve bağımlı kaynaklarınızın zaten dağıtılması ve alternatif konuma hazırlanmasının gerekli olmasını gerektirir.

Yedeklenen müşteri tarafından yönetilen anahtarların test geri yüklemesi. Bu, yalnızca tümleştirme hizmeti ortamlarında (ıSE) çalışan Logic Apps için geçerli olduğunu unutmayın.

* [Azure Logic Apps için iş sürekliliği ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/logic-apps/business-continuity-disaster-recovery-guidance)

* [Azure Logic Apps içindeki tümleştirme hizmeti ortamları (sesleri) için bekleyen verileri şifrelemek üzere müşteri tarafından yönetilen anahtarlar ayarlayın](https://docs.microsoft.com/azure/logic-apps/customer-managed-keys-integration-service-environment)

* [Azure 'da Anahtar Kasası anahtarlarını geri yükleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: olağanüstü durum kurtarma stratejiniz, Azure Logic Apps de kullanılabilir olan alternatif bir konumda bir bekleme veya yedekleme mantıksal uygulamasına yük devretme için birincil mantıksal uygulamanızı ayarlamaya odaklanmalıdır. Bu şekilde, birincil, kayıpları, kesintiler veya hatalara sahip olursa ikincil iş üzerinde işlem yapabilir. Bu strateji, ikincil mantıksal uygulamanızın ve bağımlı kaynaklarınızın zaten dağıtılması ve alternatif konuma hazırlanmasının gerekli olmasını gerektirir.

Yedeklenen müşteri tarafından yönetilen anahtarları koruyun. Bu, yalnızca tümleştirme hizmeti ortamlarında (ıSE) çalışan Logic Apps için geçerli olduğunu unutmayın.

Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault ' de geçici silme ve Temizleme korumasını etkinleştirin.

* [Azure Logic Apps için iş sürekliliği ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/logic-apps/business-continuity-disaster-recovery-guidance)

* [Azure Logic Apps içindeki tümleştirme hizmeti ortamları (sesleri) için bekleyen verileri şifrelemek üzere müşteri tarafından yönetilen anahtarlar ayarlayın](https://docs.microsoft.com/azure/logic-apps/customer-managed-keys-integration-service-environment)

* [Key Vault 'da geçici silme ve Temizleme korumasını etkinleştirme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

* [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

* [Kendi olay yanıtı planınızı oluşturmaya yardımcı olmak için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzuyla yararlanın](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) etiketleri kullanarak Azure kaynaklarını açıkça tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun. Olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak, uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır.

* [Azure Güvenlik Merkezi'nde güvenlik uyarıları](https://docs.microsoft.com/azure/security-center/security-center-alerts-overview)

* [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: Azure kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

* [NıST 'nin Yayımlama Kılavuzu, BT planları ve becerileri için programları test etme, eğitim ve alıştırma Kılavuzu](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

* [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Kılavuz**: Azure kaynaklarına yönelik riskleri belirlemenize yardımcı olmak Için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz.

* [Sürekli dışarı aktarmayı yapılandırma](https://docs.microsoft.com/azure/security-center/continuous-export)

* [Uyarıları Azure Sentinel 'e akış](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Kılavuz**: Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanarak, güvenlik uyarılarındaki "Logic Apps" aracılığıyla yanıtları otomatik olarak tetikleyin ve Azure kaynaklarınızı korumaya yönelik öneriler alın.

* [Iş akışı otomasyonu ve Logic Apps yapılandırma](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin. Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın.

* [Katılım test kurallarını oyma](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Microsoft Bulut Kırmızı ekip oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
