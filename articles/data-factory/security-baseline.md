---
title: Azure Data Factory için Azure Güvenlik temeli
description: Azure Data Factory için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 06/05/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 57786aa343fd2ea863b17f65253e5d4a4a6b88ce
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89226837"
---
# <a name="azure-security-baseline-for-azure-data-factory"></a>Azure Data Factory için Azure Güvenlik temeli

Azure Data Factory için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

**Rehberlik**: bir Azure-SSIS INTEGRATION RUNTIME (IR) oluştururken, sanal ağ ile buna ekleme seçeneğiniz vardır. Bu, Azure Data Factory ağ güvenlik grubu (NSG) ve yük dengeleyici gibi belirli ağ kaynaklarını oluşturmalarına olanak tanır. Ayrıca kendi statik genel IP adresinizi sağlayabilir veya sizin için Azure Data Factory oluşturmak için bir tane oluşturabilirsiniz. Azure Data Factory tarafından otomatik olarak oluşturulan NSG 'de, bağlantı noktası 3389 varsayılan olarak tüm trafiğe açıktır. Yalnızca yöneticilerin erişimi olduğundan emin olmak için bunu kilitleyin.

Şirket içinde barındırılan IRS, bir sanal ağ içindeki şirket içi bir makineye veya Azure sanal makinesine dağıtılabilir. Sanal ağ alt ağı dağıtımınızın yalnızca yönetim erişimine izin verecek şekilde yapılandırılmış bir NSG 'ye sahip olduğundan emin olun. Azure-SSIS IR, koruma için her IR düğümündeki Windows güvenlik duvarı kuralında varsayılan olarak giden bağlantı noktası 3389 ' i devre dışı bıraktı. Bir NSG 'yi alt ağla ilişkilendirerek ve katı kuralları ayarlayarak sanal ağ tarafından yapılandırılan kaynaklarınızın güvenliğini sağlayabilirsiniz.

Özel bağlantı kullanılabiliyorsa, Azure SQL Server gibi Azure Data Factory işlem hattınızla bağlantılı olan kaynakların güvenliğini sağlamak için özel uç noktaları kullanın. Özel bağlantıyla, sanal ağınız ile hizmet arasındaki trafik Microsoft omurga ağı üzerinden geçer ve bu da genel Internet 'ten etkilenme olasılığını ortadan kaldırır.

* [Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)

* [Şirket içinde barındırılan IR oluşturma ve yapılandırma](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)

* [Sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

* [Güvenlik Yapılandırması ile NSG oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

* [Bir Azure-SSIS IR sanal ağa ekleme](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network#virtual-network-configuration)

* [Azure özel bağlantısını anlama](https://docs.microsoft.com/azure/private-link/private-link-overview)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: sanal ağların, alt ağların ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Kılavuz**: Azure Güvenlik Merkezi 'ni kullanın ve Integration Runtime dağıtımınızla ilişkili sanal ağ ve ağ güvenlik grubu için ağ koruma önerilerini düzeltin.

Ayrıca, Integration Runtime dağıtımınızı koruyan NSG için ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Azure depolama hesabına gönderin.

Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

* [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

* [NSG akış günlüklerini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

* [Trafik Analizi etkinleştirme ve kullanma](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Web uygulamalarını barındıran Azure Apps hizmeti veya işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: dağıtılmış hizmet reddi saldırılarına karşı koruma için Integration Runtime dağıtımınızla ilişkili sanal ağlarda DDoS koruma standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

* [DDoS korumasını yapılandırma](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

* [Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-data-services)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets"></a>1,5: ağ paketlerini kaydetme

**Rehberlik**: Integration Runtime dağıtımınızı koruyan NSG için ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Azure depolama hesabına gönderin.

Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

* [NSG akış günlüklerini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

* [Trafik Analizi etkinleştirme ve kullanma](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Kılavuz**: Azure-SSIS IR giden trafiği incelemek Istiyorsanız, Azure ExpressRoute Zorlamalı tünel aracılığıyla veya kimlikleri/IP 'Leri destekleyen Azure Marketi 'nden gelen bir ağ sanal gereci (NVA) üzerinden Azure-SSIS IR başlatılan trafiği şirket içi güvenlik duvarı ile yönlendirebilirsiniz. Yük incelemesini temel alan yetkisiz giriş algılama ve/veya önleme bir gereksinim değilse, tehdit zekasıyla Azure Güvenlik Duvarı kullanılabilir.

* [Bir Azure-SSIS Integration Runtime sanal ağa ekleme](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)

* [Azure Market](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

* [Azure Güvenlik duvarını dağıtma](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

* [Azure Güvenlik Duvarı ile uyarıları yapılandırma](https://docs.microsoft.com/azure/firewall/threat-intel)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; Bu öneri, Web uygulamalarını barındıran Azure Apps hizmeti veya işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: ağ güvenlik grubu (NSG) veya Azure Güvenlik Duvarı 'nda ağ erişim denetimlerini tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örneğin, DataFactoryManagement) belirterek, karşılık gelen hizmet için gelen trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

* [Hizmet etiketlerini anlama ve kullanma](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

* [Belirli hizmet etiketlerinin Azure Data Factory anlayın](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure Ilkesi ile Azure Data Factory örneklarınızla ilişkili ağ ayarları ve ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure Data Factory örneklerinizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. DataFactory" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. Ayrıca, ağ veya Azure Data Factory örnekleriyle ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz, örneğin:
- DDoS koruma standardı etkinleştirilmelidir

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Ağ için Azure Ilke örnekleri](https://docs.microsoft.com/azure/governance/policy/samples/#network)

* [Azure Blueprint oluşturma](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: meta veri ve mantıksal kuruluş sağlamak üzere Azure Data Factory örneklerinizin ağ güvenliği ve trafik akışı ile ilgili kaynaklar için Etiketler kullanın.

Tüm kaynakların etiketlerle oluşturulduğundan ve mevcut etiketlenmemiş kaynakları bilgilendirmek için, "etiket ve onun değeri gerektir" gibi etiketlemeyle ilgili yerleşik Azure Ilke tanımlarından herhangi birini kullanın.

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını izleyin ve Azure Data Factory örneklerinizin ilişkili olduğu ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

* [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Azure Izleyici 'de uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, günlüklerde zaman damgalarına yönelik Azure Data Factory gibi Azure kaynakları için kullanılan saat kaynağını korur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: Azure Data Factory tarafından oluşturulan güvenlik verilerini toplamak Için Azure izleyici aracılığıyla günlükleri alma. Azure Izleyici 'de, Azure Data Factory etkinlik günlüklerinizi alacak şekilde yapılandırılmış Log Analytics çalışma alanını sorgulayabilirsiniz. Diğer sistemlere veri aktarmak için uzun süreli/arşiv günlüğü depolaması veya Olay Hub 'ları için Azure depolama hesaplarını kullanın.

Alternatif olarak, Azure Sentinel veya bir üçüncü taraf güvenlik olayı ve olay yönetimi (SıEM) üzerinde veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz. Ayrıca, değişiklikleri izleme/denetleme ve hataları ortaya çıkaracak değişiklikleri alma özelliği gibi çeşitli kaynak denetimi avantajlarından yararlanmak için Azure Data Factory git ile tümleştirebilirsiniz.

* [Tanılama ayarlarını yapılandırma](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings#create-diagnostic-settings-in-azure-portal)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Azure Izleyici ve üçüncü taraf SıEM tümleştirmesi ile çalışmaya başlama](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

* [Azure Data Factory kaynak denetimi](https://docs.microsoft.com/azure/data-factory/source-control)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: denetim düzlemi denetim günlüğü Için Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Event Hubs ya da arşiv Için Azure depolama hesabına gönderin. Azure etkinlik günlüğü verilerini kullanarak, Azure kaynaklarınızın denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, SILME) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Ölçümler ve işlem hattı gibi Azure Data Factory işlem olmayan kaynaklar için tanılama günlüklerini yapılandırmak üzere tanılama ayarlarını kullanın. Azure Data Factory depolar işlem hattı-verileri 45 gün boyunca çalıştırın. Bu verileri daha uzun süre saklamak için, tanılama günlüklerinizi denetim veya el ile inceleme için bir depolama hesabına kaydedin ve bekletme süresini gün olarak belirtin. Ayrıca günlükleri Azure Event Hubs akışa alabilir veya kayıtları analiz için bir Log Analytics çalışma alanına gönderebilirsiniz.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Tanılama günlüklerini Azure Data Factory anlama](https://docs.microsoft.com/azure/data-factory/monitor-using-azure-monitor)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: Integration Runtime bir Azure sanal MAKINESINDE (VM) çalıştırıyorsanız, Azure izleyici 'yi kullanarak sanal makineden veri toplayabilirsiniz. Log Analytics VM uzantısının yüklenmesi, Azure Izleyici 'nin Azure sanal makinelerinizden veri toplamasına izin verir. Azure Güvenlik Merkezi, sanal makineler için güvenlik olay günlüğü izleme sağlayabilir. Güvenlik olay günlüğünün oluşturduğu veri hacmi verildiğinde, varsayılan olarak depolanmaz.

Kuruluşunuzun güvenlik olay günlüğü verilerini bekletmek istiyorsanız, bir veri toplama katmanında depolanabilir ve bu noktada Log Analytics sorgulanabilir.

* [Azure Izleyici 'de Azure sanal makinelerinden veri toplama](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

* [Azure Güvenlik Merkezi 'nde veri toplamayı etkinleştirme](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Rehberlik**: Azure Data Factory için tanılama ayarlarını etkinleştirin. Günlükleri bir Log Analytics çalışma alanında depolamayı seçerseniz, kuruluşunuzun uyumluluk düzenlemelerine göre Log Analytics çalışma alanı saklama süresini ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

* [Azure Data Factory tanılama günlüklerini etkinleştirme](https://docs.microsoft.com/azure/data-factory/monitor-using-azure-monitor#set-up-diagnostic-logs)

* [Log Analytics çalışma alanları için günlük saklama parametrelerini ayarlama](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: Azure Data Factory için tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Günlüklerinizi anormal davranışa göre analiz etmek ve izlemek ve sonuçları düzenli olarak gözden geçirmek için Log Analytics kullanın. Azure Data Factory dağıtımlarınızla ilgili tüm veri depoları için tanılama ayarlarını da etkinleştirdiğinizden emin olun. Tanılama ayarlarının nasıl etkinleştirileceği ile ilgili yönergeler için her hizmetin güvenlik temeline bakın.

Integration Runtime bir Azure sanal makinesinde (VM) çalıştırıyorsanız, VM için tanılama ayarlarını da etkinleştirin.

Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

* [Log Analytics şeması](https://docs.microsoft.com/azure/data-factory/monitor-using-azure-monitor#schema-of-logs-and-events)

* [Azure Izleyici ile Azure sanal makinesinden veri toplama](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Kılavuz**: &amp; Azure izleyici 'de uyarılar ölçümleri bölümüne giderek Data Factory desteklenen ölçümler üzerinde uyarı oluşturabilirsiniz.

Azure Data Factory için tanılama ayarlarını yapılandırın ve günlükleri bir Log Analytics çalışma alanına gönderin. Log Analytics çalışma alanınızda, önceden tanımlanmış bir koşullar kümesi gerçekleşirken yapılacak uyarıları yapılandırın. Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

Ayrıca, veri depolarınız ile ilgili hizmetler için tanılama ayarlarını etkinleştirdiğinizden emin olun. Rehberlik için her hizmetin güvenlik temeline başvurabilirsiniz.

* [Azure Data Factory uyarılar](https://docs.microsoft.com/azure/data-factory/monitor-visually#alerts)

* [Tüm desteklenen ölçümler sayfası](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported)

* [Log Analytics çalışma alanında uyarıları yapılandırma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Kılavuz**: Azure sanal makinesinde Integration Runtime çalıştırıyorsanız, Azure Cloud Services ve sanal makineler Için Microsoft kötü amaçlı yazılımdan koruma kullanabilir ve sanal makinelerinizi olayları Azure depolama hesabına yönelik olarak günlüğe kaydeder şekilde yapılandırabilirsiniz. Depolama hesaplarından olayları almak ve uygun yerlerde uyarı oluşturmak için bir Log Analytics çalışma alanı yapılandırın. Azure Güvenlik Merkezi 'nde önerileri takip edin: "Işlem &amp; Uygulamaları".

* [Cloud Services ve sanal makineler için Microsoft Antimalware 'i yapılandırma](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

* [Sanal makineler için konuk düzeyinde izlemeyi etkinleştirme](https://docs.microsoft.com/azure/cost-management/azure-vm-extended-metrics)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Azure Data Factory, DNS ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: Integration Runtime bir Azure sanal MAKINESINDE (VM) çalıştırıyorsanız, komut satırı denetim günlüğünü etkinleştirebilirsiniz. Azure Güvenlik Merkezi, Azure VM 'Ler için güvenlik olay günlüğü izleme sağlar. Güvenlik Merkezi, desteklenen tüm Azure VM 'lerde ve otomatik sağlama etkinse oluşturulan tüm yeni Microsoft Monitoring Agent sağlar veya aracıyı el ile yükleyebilirsiniz. Aracı, işlem oluşturma olayı 4688 ve olay 4688 ' nin içindeki CommandLine alanı ' na izin vermez. VM üzerinde oluşturulan yeni süreçler, olay günlüğü tarafından kaydedilir ve Güvenlik Merkezi 'nin algılama hizmetleri tarafından izlenir.

* [Azure Güvenlik Merkezinde veri toplama](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure Data Factory içinde, Kullanıcı erişimini düzenli olarak izlemediğinizden ve karşılaştırtığınızdan emin olun. Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, katkıda bulunan, sahip veya yönetici rollerinin üyesi ya da bir Azure aboneliğinin yöneticisi olmalıdır.

Ayrıca, kiracı düzeyinde Azure Active Directory (AD) açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. Azure Data Factory örneklerinizin denetim düzlemine yönetici erişimi olan yönetim gruplarının üyeleri olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

Azure AD, Kullanıcı erişimini yönetmek için önerilen yöntem olsa da, Azure sanal makinesinde (VM) Integration Runtime çalıştırıyorsanız, sanal makinenizin yerel hesaplara de sahip olabileceğini aklınızda bulundurun. Hem yerel hem de etki alanı hesaplarının genellikle en düşük bir ayak izi ile incelenmesi ve yönetilmesi gerekir. Ayrıca, yönetim izinlerinin kullanılabilirliğini azaltmak için, ayrıcalıklı kimlik yöneticisinin tam zamanında özelliği gözden geçirilmesini tavsiye ederiz.

* [Azure Data Factory için roller ve izinler](https://docs.microsoft.com/azure/data-factory/concepts-roles-permissions)

* [Ayrıcalıklı Identity Manager hakkında bilgi](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

* [Azure AD 'de PowerShell ile dizin rolü alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

* [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

* [Yerel hesaplar için bilgiler](https://docs.microsoft.com/azure/active-directory/devices/assign-local-admin#manage-the-device-administrator-role)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Azure Data Factory, Azure portal ve Azure Data Factory konsoluna erişim sağlamak için Azure ACTIVE DIRECTORY (ad) kullanır. Azure AD varsayılan parola kavramına sahip değildir, ancak tüm özel veya üçüncü taraf uygulamalar için varsayılan parolalara izin vermekten veya buna izin verilmez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: Azure denetim düzlemi 'ne (Azure Portal) ve Azure Data Factory konsoluna erişim için adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Azure AD 'de Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Ayrıca, özel yönetim hesaplarını izlemenize yardımcı olmak için Azure Güvenlik Merkezi veya yerleşik Azure Ilkeleri önerilerini kullanabilirsiniz; örneğin:
- Aboneliğinize birden fazla sahip atanmalıdır
- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

Azure sanal makinesinde Integration Runtime çalıştırıyorsanız, Azure sanal makineler üzerindeki yönetici hesapları Azure ayrıcalıklı kimlik yöneticisi (PıM) ile de yapılandırılabilir. Azure ayrıcalıklı Identity Manager, izinlerin yalnızca belirli zaman kareleri için kullanılabilmesi ve ikinci bir kişinin onaylamasını gerektirmek için, tam zamanında yükseltme, Multi-Factor Authentication ve temsili seçenekleri gibi çeşitli seçenekler sunar.

* [Azure Güvenlik Merkezi kimlik ve erişimini anlama](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Azure Ilkesini kullanma](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Ayrıcalıklı Identity Manager hakkında bilgi](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

* [Azure Data Factory için roller ve izinler](https://docs.microsoft.com/azure/data-factory/concepts-roles-permissions)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: uygulamanızın veya Işlevinizin, kurtarma hizmetleri kasalarınıza erişmek ve bunlarla etkileşime geçmek için kullanabileceği bir belirteç almak Için bir Azure uygulama kaydı (hizmet sorumlusu) kullanın.

* [Azure REST API 'Lerini çağırma](https://docs.microsoft.com/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

* [Azure AD ile istemci uygulamanızı (hizmet sorumlusu) kaydetme](https://docs.microsoft.com/rest/api/azure/#register-your-client-application-with-azure-ad)

* [Azure Kurtarma Hizmetleri API bilgileri](https://docs.microsoft.com/rest/api/recoveryservices/)

* [Azure Data Factory REST API hakkında bilgi](https://docs.microsoft.com/rest/api/datafactory/)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Active Directory MULTI-Factor AUTHENTICATION (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

* [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve yapılandırmak için yapılandırılmış MULTI-Factor AUTHENTICATION (MFA) ile ayrıcalıklı erişim iş istasyonları (Paw) kullanın.

* [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda günlüklerin ve uyarıların üretilmesi için Azure Active Directory güvenlik raporlarını kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın.

Integration Runtime bir Azure sanal makinesinde (VM) çalıştırıyorsanız, Ayrıca, sanal makinenizin Azure Sentinel 'e de bağlı olmasını sağlayabilirsiniz. Microsoft Azure Sentinel, ölçeklenebilir, bulutta yerel, güvenlik bilgileri olay yönetimi (SıEM) ve güvenlik Orchestration otomatik yanıtı (SOAR) çözümüdür. Azure Sentinel, bir uyarı algılama, tehdit görünürlüğü, proaktif arama ve tehdit yanıtı için tek bir çözüm sunarak kuruluş genelinde akıllı güvenlik Analizi ve tehdit bilgileri sunar.

* [Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk)

* [Azure Güvenlik Merkezi 'nde kullanıcıların kimlik ve erişim etkinliğini izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Yerleşik Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek Için adlandırılmış konumlar kullanın.

* [Azure 'da adlandırılmış konumları yapılandırma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: bir veri fabrikası, belirli veri fabrikasını temsil eden Azure kaynakları için yönetilen bir kimlikle ilişkilendirilebilir. Bu yönetilen kimliği, Azure SQL veritabanı kimlik doğrulaması için kullanabilirsiniz. Belirlenen fabrika, bu kimliği kullanarak, veritabanınıza veya veritabanına erişebilir ve veri kopyalayabilir.

Azure sanal makinesinde Integration Runtime (IR) çalıştırıyorsanız, kodunuzda herhangi bir kimlik bilgisi olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için Yönetilen kimlikler ' i kullanabilirsiniz. Bir sanal makinede çalışan kodunuz, Azure AD kimlik doğrulamasını destekleyen hizmetlere erişim belirteçleri istemek için yönetilen kimlik kullanabilir.

* [Azure AD örneği oluşturma ve yapılandırma](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

* [Azure kaynakları için Yönetilen kimlikler nelerdir?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

* [Azure Data Factory kullanarak Azure SQL veritabanındaki verileri kopyalama ve dönüştürme](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database#using-service-principal-authentication)

* [Azure SQL veritabanı ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) eski hesapları keşfetmenize yardımcı olacak Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir.

Çalışma zamanı tümleştirmenizi bir Azure sanal makinesinde çalıştırıyorsanız, sistemin güvenliğini tehlikeye atabilecek beklenmeyen hesaplar olmadığından emin olmak için yerel güvenlik gruplarını ve kullanıcıları gözden geçirmeniz gerekir.

* [Azure kimlik erişimi Incelemelerini kullanma](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

* [Azure AD raporlamayı anlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [Azure kimlik erişimi Incelemelerini kullanma](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Rehberlik**: herhangi bir SIEM/izleme aracıyla tümleştirmenize imkan tanıyan Azure ACTIVE DIRECTORY (ad) oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişiminiz vardır. Bu işlemi, Azure AD Kullanıcı hesapları için Tanılama ayarları oluşturarak ve Log Analytics çalışma alanına denetim günlüklerini ve oturum açma günlüklerini göndererek kolaylaştırabilirsiniz. Log Analytics içinde, istenen günlük uyarılarını yapılandırabilirsiniz.

Integration Runtime bir Azure sanal makinesinde (VM) çalıştırıyorsanız, sanal makineyi Azure Sentinel 'e ekleyebilirsiniz. Microsoft Azure Sentinel, ölçeklenebilir, bulutta yerel, güvenlik bilgileri olay yönetimi (SıEM) ve güvenlik Orchestration otomatik yanıtı (SOAR) çözümüdür. Azure Sentinel, bir uyarı algılama, tehdit görünürlüğü, proaktif arama ve tehdit yanıtı için tek bir çözüm sunarak kuruluş genelinde akıllı güvenlik Analizi ve tehdit bilgileri sunar.

* [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

* [Azure Active Directory kullanarak Event Hubs kaynaklarına erişim yetkisi verme](https://docs.microsoft.com/azure/event-hubs/authorize-access-azure-active-directory)

* [Yerleşik Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Kılavuz**: Azure SQL veritabanı veya Azure sanal makineleri gibi Azure Data Factory kaynaklarınız için merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Denetim düzleminde hesap oturum açma davranışı sapması (Azure portal) için, otomatik yanıtları Kullanıcı kimlikleriyle ilgili şüpheli eylemlere yönelik olarak yapılandırmak için Azure AD Kimlik Koruması ve risk algılama özelliklerini kullanın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz.

* [Azure AD riskli oturum açma işlemlerini görüntüleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [SQL ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure?tabs=azure-powershell)

* [Azure-SSIS Integration Runtime için Azure Active Directory kimlik doğrulamasını etkinleştirme](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Kılavuz**: Microsoft 'un müşteri verilerine erişmesi gereken destek senaryolarında, Azure müşteri kasası müşterilerin müşteri verileri erişim isteklerini gözden geçirmesi ve onaylaması veya reddetmesi için bir arabirim sağlar. Azure Kasası Azure Data Factory kendisi için kullanılamadığından Azure Kasası, Azure SQL veritabanı ve Azure sanal makinelerini destekler.

* [Müşteri Kasası anlayın](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

Azure SQL veritabanı veri bulma ve sınıflandırma özelliğini kullanın. Veri bulma ve sınıflandırma, &amp; veritabanlarınızdaki hassas verileri korumak, sınıflandırmak ve etiketleme Için Azure SQL veritabanı 'nda yerleşik olarak bulunan gelişmiş yetenekler sağlar.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Azure SQL Server veri bulma ve sınıflandırma kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Tümleştirme çalışma zamanları, sanal ağ (VNet)/subnet ile ayrılmalıdır ve uygun şekilde etiketlenemez.

 Ayrıca, Özel uç noktaları ağ yalıtımı gerçekleştirmek için de kullanabilirsiniz. Azure özel uç noktası, Azure özel bağlantısı tarafından desteklenen bir hizmete özel ve güvenli bir şekilde bağlanan bir ağ arabirimidir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor.

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Özel bağlantıyı anlama](https://docs.microsoft.com/azure/private-link/private-endpoint-overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: Azure Data Factory dağıtımınız için hassas bilgileri depolamak veya işlemek üzere veri kaynakları (Azure SQL veritabanı gibi) için, etiketleri kullanarak ilgili kaynakları hassas olarak işaretleyin.

Özel bağlantı kullanılabiliyorsa, Azure Data Factory işlem hattınızla bağlantılı olan kaynakların güvenliğini sağlamak için özel uç noktaları kullanın. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Ayrıca, bir ağ güvenlik grubu (NSG) üzerinde sıkı bir giden kuralları kümesi yapılandırarak ve bu NSG 'yi alt ağınızla ilişkilendirerek veri sızdırma riskini azaltabilirsiniz.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Güvenlik Yapılandırması ile NSG oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

* [Azure özel bağlantısını anlama](https://docs.microsoft.com/azure/private-link/private-link-overview)

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: bulut VERI deposu https veya TLS 'yi destekliyorsa, Data Factory ve bulut veri deposundaki veri taşıma hizmetleri arasındaki tüm veri aktarımları, GÜVENLI kanal https veya TLS aracılığıyla yapılır. Kullanılan TLS sürümü 1,2.

Azure SQL veritabanı ve Azure SQL veri ambarı 'na yönelik tüm bağlantılar, veriler veritabanına aktarılırken ve veritabanından aktarılırken şifreleme (SSL/TLS) gerektirir. JSON kullanarak bir işlem hattı yazarken, şifreleme özelliğini ekleyin ve bağlantı dizesinde true olarak ayarlayın. Azure depolama için bağlantı dizesinde HTTPS kullanabilirsiniz.

* [Azure Data Factory geçişte şifrelemeyi anlama](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Kılavuz**: Azure SQL veritabanı örneklerinizi kopyalamak ve dönüştürmek için Azure Data Factory kullanıyorsanız, Azure SQL veritabanı veri bulma ve sınıflandırma özelliğini kullanın. Veri bulma ve sınıflandırma, &amp; veritabanlarınızdaki hassas verileri korumak, sınıflandırmak ve etiketleme Için Azure SQL veritabanı 'nda yerleşik olarak bulunan gelişmiş yetenekler sağlar.

Veri bulma ve sınıflandırma özellikleri henüz diğer Azure hizmetleri için kullanılamaz.

* [Azure SQL Server veri bulma ve sınıflandırma kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Rehberlik**: Azure Data Factory denetim düzlemine erişimi denetlemek için Azure rol tabanlı erişim denetimi (Azure RBAC) kullanın (Azure Portal).

Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, katkıda bulunan, sahip veya yönetici rollerinin üyesi ya da bir Azure aboneliğinin yöneticisi olmalıdır.

Azure SQL veritabanı gibi Data Factory veri kaynaklarınız için, Azure RBAC hakkında daha fazla bilgi için ilgili hizmetin güvenlik temeline bakın.

* [Azure RBAC 'yi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

* [Azure Data Factory için roller ve izinler](https://docs.microsoft.com/azure/data-factory/concepts-roles-permissions)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: veri tanımlama, sınıflandırma ve kayıp önleme özellikleri, Azure depolama veya işlem kaynakları için henüz kullanılamıyor. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: Azure Data Factory dağıtımlarınızla ilgili veri depoları için veri şifreleme mekanizmasını etkinleştirmenizi öneririz. Bekleyen verilerin şifrelenmesi hakkında daha fazla bilgi için ilgili hizmetin güvenlik temeline başvurabilirsiniz.

Integration Runtime bir Azure sanal makinesinde çalıştırıyorsanız, Windows Sanal Makineleri (VM) üzerindeki sanal diskler, sunucu tarafı şifreleme veya Azure disk şifrelemesi (ADE) kullanılarak geri kalanıyla şifrelenir. Azure disk şifrelemesi, yönetilen diskleri, Konuk VM içindeki müşteri tarafından yönetilen anahtarlarla şifrelemek için Windows 'un BitLocker özelliğinden yararlanır. Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme, depolama hizmetindeki verileri şifreleyerek VM 'niz için herhangi bir işletim sistemi türünü ve görüntüsünü kullanmanızı sağlayarak ADE 'yi geliştirir.

Kimlik bilgilerini veya gizli değerleri bir Azure Key Vault saklayabilir ve etkinliklerinize geçirilecek işlem hattı yürütme sırasında kullanabilirsiniz. Ayrıca, veri depoları için kimlik bilgilerini saklayabilir ve bir Azure Key Vault. Azure Data Factory, veri deposu/işlem kullanan bir etkinliği yürütürken kimlik bilgilerini alır.

* [Azure Data Factory bekleyen şifrelemeyi anlama](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)

* [Azure yönetilen disklerinin sunucu tarafı şifrelemesi](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption)

* [Windows VM 'Leri için Azure disk şifrelemesi](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption-overview)

* [Ardışık düzen etkinliklerinde Azure Key Vault gizli dizileri kullanma](https://docs.microsoft.com/azure/data-factory/how-to-use-azure-key-vault-secrets-pipeline-activities)

* [Azure Key Vault kimlik bilgileri](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Izleyici 'Yi Azure etkinlik günlüğü ile birlikte kullanarak, Azure Data Factory ve ilgili kaynaklarda değişiklik gerçekleşirken uyarılar oluşturun.

* [Azure etkinlik günlüğü olayları için uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

* [Azure etkinlik günlüğü olayları için uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

* [Azure Depolama analizi günlüğü](https://docs.microsoft.com/azure/storage/common/storage-analytics-logging)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Kılavuz**: Azure SQL veritabanı 'nı bir veri deposu olarak kullanıyorsanız, Azure SQL veritabanı Için gelişmiş veri güvenliğini etkinleştirin ve Azure SQL sunucularınızda güvenlik açığı değerlendirmelerini gerçekleştirerek Azure Güvenlik Merkezi 'ndeki önerileri izleyin.

Integration Runtime bir Azure sanal makinesinde (VM) çalıştırıyorsanız, sanal makinelerinizde güvenlik açığı değerlendirmeleri gerçekleştirerek Azure Güvenlik Merkezi 'ndeki önerileri izleyin. Sanal makinelerinize yönelik güvenlik açığı değerlendirmelerini gerçekleştirmek için, Azure Güvenlik önerilir veya üçüncü taraf çözümü kullanın.

* [Azure SQL veritabanı 'nda güvenlik açığı değerlendirmeleri nasıl çalıştırılır](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment)

* [Gelişmiş veri güvenliğini etkinleştirme](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)

* [Azure Güvenlik Merkezi güvenlik açığı değerlendirmesi önerilerini uygulama](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: Integration Runtime bir Azure sanal MAKINESINDE (VM) çalıştırıyorsanız, VM 'lerinize yönelik güncelleştirmeleri ve düzeltme eklerini yönetmek için Azure güncelleştirme yönetimi çözümünü kullanın. Güncelleştirme Yönetimi, desteklenen Windows sistemlerine yama yapmak için yerel olarak yapılandırılmış güncelleştirme deposuna bağımlıdır. System Center Updates Publisher (Updates Publisher) gibi araçlar, Windows Server Update Services (WSUS) içinde özel güncelleştirmeler yayımlamanıza olanak sağlar. Bu senaryo Güncelleştirme Yönetimi, üçüncü taraf yazılımlarla güncelleştirme deposu olarak Configuration Manager kullanan makinelere yaması sağlar.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure 'da Güncelleştirme Yönetimi çözümü](https://docs.microsoft.com/azure/automation/automation-update-management)

* [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](https://docs.microsoft.com/azure/automation/automation-tutorial-update-management)

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: üçüncü taraf yazılım başlıkları için otomatik düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: Integration Runtime bir Azure sanal MAKINESINDE (VM) çalıştırıyorsanız, üçüncü taraf bir düzeltme eki yönetimi çözümü kullanabilirsiniz. Sanal makineleriniz için güncelleştirmeleri ve düzeltme eklerini yönetmek üzere Azure Güncelleştirme Yönetimi çözümünü kullanabilirsiniz. Güncelleştirme Yönetimi, desteklenen Windows sistemlerine yama yapmak için yerel olarak yapılandırılmış güncelleştirme deposuna bağımlıdır. System Center Updates Publisher (Updates Publisher) gibi araçlar, Windows Server Update Services (WSUS) içinde özel güncelleştirmeler yayımlamanıza olanak sağlar. Bu senaryo Güncelleştirme Yönetimi, üçüncü taraf yazılımlarla güncelleştirme deposu olarak Configuration Manager kullanan makinelere yaması sağlar.

* [Azure 'da Güncelleştirme Yönetimi çözümü](https://docs.microsoft.com/azure/automation/automation-update-management)

* [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](https://docs.microsoft.com/azure/automation/automation-tutorial-update-management)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: bir Azure sanal makinesinde Integration Runtime çalıştırıyorsanız, tarama sonuçlarını tutarlı aralıklarla dışarı aktarın ve güvenlik açıklarının düzeltildiğini doğrulamak için sonuçları karşılaştırın. Azure Güvenlik Merkezi tarafından önerilen güvenlik açığı yönetimi önerisi kullanılırken, geçmiş tarama verilerini görüntülemek için seçili çözümün portalına de Pivot ekleyebilirsiniz.

* [Sanal makineler için tümleşik güvenlik açığı tarayıcısını anlama](https://docs.microsoft.com/azure/security-center/built-in-vulnerability-assessment)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: Integration Runtime bir Azure sanal makinesinde çalıştırıyorsanız, yerel güvenlik açığı tarayıcısını kullanabilirsiniz. Azure Güvenlik Merkezi 'ne dahil edilen güvenlik açığı tarayıcısı Qualys tarafından desteklenmektedir. Qualys 'in tarayıcısı, Azure sanal makinelerinizdeki güvenlik açıklarının gerçek zamanlı olarak tanımlanması için önde gelen bir araçtır.

Güvenlik Merkezi güvenlik açıklarını belirlediğinde, bulguları ve ilgili bilgileri öneriler olarak gösterir. İlgili bilgiler, düzeltme adımlarını, ilgili, CVSS puanlarını ve daha fazlasını içerir. Bir veya daha fazla abonelik veya belirli bir sanal makine için tanımlanan güvenlik açıklarını görüntüleyebilirsiniz.

* [Sanal makineler için tümleşik güvenlik açığı tarayıcısı](https://docs.microsoft.com/azure/security-center/built-in-vulnerability-assessment)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: abonelikleriniz dahilinde (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) tüm kaynakları sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerini numaralandırın.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

* [Azure Kaynak Graf ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [Azure aboneliklerinizi görüntüleme](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Azure RBAC 'yi anlama](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure kaynaklarını düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ayrıca, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri abonelikleri içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure Ilkesi 'ni kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynakları envanterini tanımlama ve sürdürme

**Rehberlik**: işlem kaynakları Için onaylanan Azure kaynakları ve onaylanan yazılımlar tanımlayın.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: abonelikleriniz içinde oluşturulabilecek kaynak türlerine yönelik kısıtlamalar koymak Için Azure ilkesini kullanın.

Azure Kaynak Grafiği 'ni kullanarak aboneliklerinde kaynakları sorgulama/bulma. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Graph ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: Integration Runtime bir Azure sanal MAKINESINDE (VM) çalıştırıyorsanız, sanal makinelerde tüm yazılımlarla ilgili bilgi toplamayı otomatik hale getirmek Için Azure sanal makine envanterinden yararlanın. Azure Otomasyonu iş yüklerinin ve kaynaklarının dağıtılması, işleme alınması ve yetkilerinin alınması sırasında eksiksiz denetim sağlar.

Note: yazılım adı, sürüm, yayımcı ve yenileme süresi Azure portal kullanılabilir. Yüklemenin tarih ve diğer bilgilerine erişim sağlamak için, müşteri, Konuk düzeyinde tanılamayı etkinleştirmek ve Windows olay günlüklerini bir Log Analytics çalışma alanına getirmelerini sağlamak için gereklidir.

* [Azure Otomasyonu’na giriş](https://docs.microsoft.com/azure/automation/automation-intro)

* [Azure VM envanterini etkinleştirme](https://docs.microsoft.com/azure/automation/automation-tutorial-installed-software)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Kılavuz**: Azure sanal makinesinde Integration Runtime çalıştırıyorsanız Azure Otomasyonu, dağıtım, işlemler ve iş yüklerinin ve kaynakların yetkisini alma sırasında tüm denetimi sağlar. Sanal makinelerde yüklü olan tüm yazılımları tanımlamak için Değişiklik İzleme kullanabilirsiniz. Kendi işleminizi uygulayabilir veya yetkisiz yazılımı kaldırmak için Azure Otomasyonu durum yapılandırması ' nı kullanabilirsiniz.

* [Azure Otomasyonu’na giriş](https://docs.microsoft.com/azure/automation/automation-intro)

* [Değişiklik İzleme çözümü ile ortamınızdaki değişiklikleri izleyin](https://docs.microsoft.com/azure/automation/change-tracking)

* [Azure Otomasyonu durum yapılandırmasına genel bakış](https://docs.microsoft.com/azure/automation/automation-dsc-overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: Integration Runtime bir Azure sanal MAKINESINDE (VM) çalıştırıyorsanız, yalnızca yetkili yazılımın yürütüldüğünden ve tüm yetkisiz yazılımların VM 'lerde yürütülmesini engellediği Için Azure Güvenlik Merkezi Uyarlamalı uygulama denetimleri kullanın.

* [Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanma](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilkesiyle belirli bir kaynak türünü reddetme](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: onaylanan yazılım başlıkları envanterini koruyun

**Rehberlik**: Uyarlamalı uygulama denetimi, Azure Güvenlik Merkezi 'nden, Azure ve Azure dışı makinelerde (Windows ve Linux) hangi uygulamaların çalıştırılacağını denetlemenize yardımcı olan akıllı, otomatik, uçtan uca bir çözümdür. Kuruluşunuzun gereksinimlerini karşılamazsa, üçüncü taraf bir çözüm uygulayın.

Bunun yalnızca Integration Runtime bir Azure sanal makinesinde çalışıyor olması durumunda geçerli olduğunu unutmayın.

* [Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanma](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak üzere Azure koşullu erişimini yapılandırın.

* [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: çalışma zamanı tümleştirmenizi, betiklerin türüne bağlı olarak bir Azure sanal makinesinde çalıştırıyorsanız, kullanıcıların Azure işlem kaynakları içinde betikleri yürütme yeteneğini sınırlamak için işletim sistemine özgü yapılandırma veya üçüncü taraf kaynakları kullanabilirsiniz. Ayrıca, Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerinden yararlanarak yalnızca yetkili yazılımın yürütüldüğünü ve tüm yetkisiz yazılımların Azure sanal makinelerinde yürütülmesini engellemiş olmasını sağlayabilirsiniz.

* [Windows ortamlarında PowerShell betiği yürütmeyi denetleme](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6)

* [Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanma](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Kılavuz**: Azure ortamınızda dağıtılan yüksek riskli uygulamalar, sanal ağ, alt ağ, abonelikler, yönetim grupları vb. ve bir Azure Güvenlik Duvarı, Web uygulaması güvenlik duvarı (WAF) veya ağ güvenlik grubu (NSG) kullanılarak yeterince güvenli hale getirilmiş olabilir.

* [Azure 'da sanal ağlar ve sanal makineler](https://docs.microsoft.com/azure/virtual-machines/windows/network-overview)

* [Azure Güvenlik Duvarı nedir?](https://docs.microsoft.com/azure/firewall/overview)

* [Azure Web Uygulaması Güvenlik Duvarı nedir?](https://docs.microsoft.com/azure/web-application-firewall/overview)

* [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview)

* [Azure sanal ağ nedir?](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)

* [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](https://docs.microsoft.com/azure/governance/management-groups/overview)

* [Abonelik kararı kılavuzu](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/subscriptions/)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: Azure ilkesiyle Azure Data Factory için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure Data Factory örneklerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. DataFactory" ad alanındaki Azure Ilke diğer adlarını kullanın.

* [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: çalışma zamanı tümleştirmenizi bir Azure sanal makinesinde çalıştırıyorsanız, tüm işlem kaynaklarında güvenlik yapılandırmalarının bakımını yapmak Için Azure Güvenlik Merkezi önerisi [sanal makinelerinizdeki güvenlik yapılandırmalarında güvenlik açıklarını düzeltin] kullanın.

* [Azure Güvenlik Merkezi önerilerini izleme](https://docs.microsoft.com/azure/security-center/security-center-recommendations)

* [Azure Güvenlik Merkezi önerilerini Düzeltme](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilke efektlerini anlama](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

* [Azure Resource Manager şablonları oluşturma hakkında bilgi](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: Integration Runtime bir Azure sanal MAKINESINDE (VM) çalıştırıyorsanız, dağıtım Için VM 'ler için güvenli bir yapılandırma sürdürmek üzere birkaç seçenek olduğunu unutmayın:
- Azure Resource Manager şablonlar: Bunlar, Azure portal bir VM dağıtmak için kullanılan JSON tabanlı dosyalardır ve özel şablonun saklanması gerekir. Microsoft, temel şablonlarda Bakımı gerçekleştirir.
- Özel sanal sabit disk (VHD): bazı durumlarda, diğer yollarla yönetilebilecek karmaşık ortamlarla ilgilenirken gibi özel VHD dosyalarının kullanılması gerekebilir. -Azure Otomasyonu durum yapılandırması: temel işletim sistemi dağıtıldıktan sonra, ayarların daha ayrıntılı bir şekilde denetlenmesi ve otomasyon çerçevesi aracılığıyla uygulanması için bu kullanılabilir.

Çoğu senaryoda, Azure Otomasyonu Istenen durum yapılandırması ile birleştirilmiş Microsoft temel VM şablonları, güvenlik gereksinimlerinin karşılanmasına ve bakımında yardımcı olabilir.

* [VM şablonunun nasıl indirileceği hakkında bilgi](https://docs.microsoft.com/azure/virtual-machines/windows/download-template)

* [Azure Resource Manager şablonları oluşturma hakkında bilgi](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)

* [Özel bir VM VHD 'sini Azure 'a yükleme](https://docs.microsoft.com/azure-stack/operator/azure-stack-add-vm-image?view=azs-1910)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure devops veya Azure Repos kullanın.

* [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure Repos belgeleri](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Kılavuz**: özel görüntüler kullanıyorsanız, görüntülere yalnızca yetkili kullanıcıların erişebildiğinden emin olmak için Azure rol tabanlı erişim denetimi 'Ni (Azure RBAC) kullanın. Kapsayıcı görüntüleri için Azure Container Registry depolayın ve yalnızca yetkili kullanıcıların görüntülere erişebildiğinden emin olmak için Azure RBAC 'den yararlanın.

Data Factory katkıda bulunan rolü, veri fabrikalarının yanı sıra bunların içindeki alt kaynakların oluşturulması ve yönetilmesi için kullanılabilir.

* [Azure RBAC 'yi anlama](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)

* [Container Registry için Azure RBAC 'nı anlama](https://docs.microsoft.com/azure/container-registry/container-registry-roles)

* [Azure RBAC 'yi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)

* [Azure Data Factory için roller ve izinler](https://docs.microsoft.com/azure/data-factory/concepts-roles-permissions)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: yerleşik Azure ilke tanımlarını ve "Microsoft. DataFactory" ad alanındaki Azure ilkesi diğer adlarını kullanarak uyarı, denetim ve sistem yapılandırmalarının uygulanmasını sağlayacak özel ilkeler oluşturun. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: Integration Runtime bir Azure sanal makinesinde çalışıyorsa bu öneri uygulanabilir. Azure Otomasyonu durum yapılandırması, herhangi bir bulutta veya şirket içi veri merkezinde Istenen durum yapılandırması (DSC) düğümleri için bir yapılandırma yönetim hizmetidir. Merkezi bir güvenli konumdan binlerce makine genelinde hızla ve kolayca ölçeklenebilirlik sunar. Makineleri kolayca ekleyebilir, bunlara bildirime dayalı yapılandırmalara atayabilir ve her makinenin, belirttiğiniz istenen duruma göre uyumluluğunu gösteren raporları görüntüleyebilirsiniz.

* [Azure Otomasyonu durum yapılandırmasına göre yönetim için makine ekleme](https://docs.microsoft.com/azure/automation/automation-dsc-onboarding)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Rehberlik**: yerleşik Azure ilke tanımlarını ve "Microsoft. DataFactory" ad alanındaki Azure ilkesi diğer adlarını kullanarak uyarı, denetim ve sistem yapılandırmalarının uygulanmasını sağlayacak özel ilkeler oluşturun. Azure kaynaklarınızın yapılandırmasını otomatik olarak zorlamak için [Denetim], [reddetme] ve [dağıtım yoksa dağıt] Azure Ilkesini kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: Integration Runtime bir Azure sanal makinesinde çalışıyorsa bu öneri uygulanabilir. Azure Otomasyonu durum yapılandırması, herhangi bir bulutta veya şirket içi veri merkezinde Istenen durum yapılandırması (DSC) düğümleri için bir yapılandırma yönetim hizmetidir. Merkezi bir güvenli konumdan binlerce makine genelinde hızla ve kolayca ölçeklenebilirlik sunar. Makineleri kolayca ekleyebilir, bunlara bildirime dayalı yapılandırmalara atayabilir ve her makinenin, belirttiğiniz istenen duruma göre uyumluluğunu gösteren raporları görüntüleyebilirsiniz.

* [Azure Otomasyonu durum yapılandırmasına göre yönetim için makine ekleme](https://docs.microsoft.com/azure/automation/automation-dsc-onboarding)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvenli hale getirmek için Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın.

Ayrıca, kimlik bilgilerini veya gizli değerleri bir Azure Key Vault saklayabilir ve etkinliklerinize geçmek üzere işlem hattı yürütme sırasında kullanabilirsiniz. Geçici silme özelliğinin etkinleştirildiğinden emin olun.

* [Azure yönetilen kimliklerle tümleştirme](https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity)

* [Key Vault oluşturma](https://docs.microsoft.com/azure/key-vault/quick-create-portal)

* [Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama](https://docs.microsoft.com/azure/key-vault/managed-identity)

* [İşlem hattı etkinliklerinde Azure Key Vault gizli dizilerini kullanma](https://docs.microsoft.com/azure/data-factory/how-to-use-azure-key-vault-secrets-pipeline-activities)

* [Azure Key Vault geçici silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: bir veri fabrikası oluştururken, fabrika oluşturma ile birlikte yönetilen bir kimlik oluşturulabilir. Yönetilen kimlik, Azure Active Directory kayıtlı bir yönetilen uygulamadır ve bu belirli veri fabrikasını temsil eder.

* [Azure Data Factory için yönetilen kimlik](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

* [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Kılavuz**: Azure sanal makinesinde Integration Runtime çalıştırıyorsanız, kaynaklarınızı sürekli olarak izlemek ve savunmak için Azure Için Microsoft kötü amaçlı yazılımdan koruma Windows sanal makineleri kullanabilirsiniz.

* [Cloud Services ve sanal makineler için Microsoft Antimalware 'i yapılandırma](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel alınan konakta (örneğin, Azure App Service) etkinleştirilir, ancak içeriğiniz üzerinde çalışmaz.

App Service, Data Lake Storage, BLOB depolama vb. gibi işlem dışı Azure kaynaklarına yüklenen tüm dosyaları önceden tarayın.

Depolama hesaplarına yüklenen kötü amaçlı yazılımları algılamak için veri Hizmetleri için Azure Güvenlik Merkezi 'nin tehdit algılamasını kullanın.

* [Azure Cloud Services ve sanal makineler için Microsoft Antimalware 'i anlayın](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

* [Azure Güvenlik Merkezi 'nin veri Hizmetleri tehdit algılamasını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-data-services)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Kılavuz**: dağıtıldığında, Azure Için Microsoft Antimalware, varsayılan olarak en son imza, platform ve altyapı güncelleştirmelerini otomatik olarak yükler. Azure Güvenlik Merkezi 'ndeki önerileri takip edin: "Işlem &amp; Uygulamaları", tüm uç noktaların en son imzalarla güncel olduğundan emin olmak için. Windows işletim sistemi, Azure Güvenlik Merkezi ile tümleşen Microsoft Defender Gelişmiş tehdit koruması hizmeti ile virüs veya kötü amaçlı yazılım tabanlı saldırılar riskini sınırlamak üzere ek güvenlik ile daha da korunabilir.

* [Azure Cloud Services ve sanal makineler için Microsoft Antimalware nasıl dağıtılır](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

* [Microsoft Defender Gelişmiş Tehdit Koruması](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/onboard-configure)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: Integration Runtime bir Azure sanal MAKINESINDE (VM) ÇALıŞTıRıYORSANıZ, sanal makinenin yanı sıra otomatik yedeklemeler için istenen sıklığı ve bekletme süresini etkinleştirip Azure Backup etkinleştirin ve yapılandırın.

Veri depolarınız için, düzenli ve otomatik yedeklemeleri gerçekleştirmeye yönelik öneriler için bu hizmetin güvenlik temeline bakın.

* [Azure VM yedeklemesine genel bakış](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction)

* [VM ayarlarından bir Azure VM 'yi yedekleme](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: Integration Runtime bir Azure sanal MAKINESINDE (VM) çalıştırıyorsanız, Azure Backup ve hedef Azure VM 'lerinin yanı sıra istediğiniz sıklık ve bekletme dönemlerini etkinleştirin. Azure Key Vault içinde müşteri tarafından yönetilen anahtarları yedekleyin.

Veri depolarınız için, düzenli ve otomatik yedeklemeleri gerçekleştirmeye yönelik öneriler için bu hizmetin güvenlik temeline bakın.

* [Azure VM yedeklemesine genel bakış](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction)

* [Azure 'da Anahtar Kasası anahtarlarını yedekleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: Integration Runtime bir Azure sanal makinesinde çalıştırıyorsanız, Azure Backup içindeki içeriğin düzenli aralıklarla veri geri yüklemesini gerçekleştirme yeteneğinin olduğundan emin olun. Gerekirse, içeriği yalıtılmış bir VLAN 'a geri yükleme testi yapın. Yedeklenen müşteri tarafından yönetilen anahtarların düzenli olarak test geri yüklenmesi.

Veri depolarınız için, yedeklemeleri doğrulamaya yönelik yönergeler için bu hizmetin güvenlik temeline bakın.

* [Azure sanal makine yedeklemesinden dosyaları kurtarma](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm)

* [Azure 'da Anahtar Kasası anahtarlarını geri yükleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: Integration Runtime bir Azure sanal MAKINESINDE (VM) çalıştırıyorsanız ve bu vm 'yi Azure Backup ile KAPATıRSANıZ, vm 'niz depolama HIZMETI ŞIFRELEMESI (SSE) ile geri kalanı şifrelenir. Azure Backup, Azure disk şifrelemesi kullanılarak şifrelenmiş Azure VM 'Leri de yedekleyebilir. Azure disk şifrelemesi, bir anahtar kasasında gizli dizi olarak korunmuş olan BitLocker şifreleme anahtarları (BEKs) ile tümleşir. Azure disk şifrelemesi Ayrıca Azure Key Vault anahtar şifreleme anahtarları (KEKs) ile tümleşir. Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault geçici silme özelliğini etkinleştirin.

* [VM 'Ler için geçici silme](https://docs.microsoft.com/azure/backup/backup-azure-security-feature-cloud#soft-delete)

* [Azure Key Vault geçici silmeye genel bakış](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

* [Azure Güvenlik Merkezi 'nde Iş akışı otomasyonlarını yapılandırma](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

* [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu da kullanabilir](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

* [NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

* [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarılar Sentinel 'i akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

* [Sürekli dışarı aktarmayı yapılandırma](https://docs.microsoft.com/azure/security-center/continuous-export)

* [Uyarıları Azure Sentinel 'e akış](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

* [Iş akışı otomasyonu ve Logic Apps yapılandırma](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: 

* [Sızma testlerinizin Microsoft ilkelerini ihlal etmemesini sağlamak için Microsoft katılım kurallarını izleyin](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
