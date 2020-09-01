---
title: Bilişsel hizmetler için Azure Güvenlik temeli
description: Bilişsel hizmetler için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 06/22/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 3633afa3ed7d3dc9ba3d85bd7542ef9838881c5b
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89225657"
---
# <a name="azure-security-baseline-for-cognitive-services"></a>Bilişsel hizmetler için Azure Güvenlik temeli

Bilişsel hizmetler için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

**Kılavuz**: Azure bilişsel hizmetler, katmanlı bir güvenlik modeli sağlar. Bu model, Bilişsel Hizmetler hesaplarınızı belirli bir ağ alt kümesinde güvenlik altına almanızı sağlar. Ağ kuralları yapılandırıldığında yalnızca belirli bir ağ kümesi üzerinden veri isteğinde bulunan uygulamalar hesaba erişim sağlayabilir. Yalnızca belirtilen IP adreslerinden, IP aralıklarından veya Azure sanal ağlardaki alt ağlar listesinden kaynaklanan isteklere izin veren istek filtreleme ile kaynaklarınıza erişimi sınırlayabilirsiniz.

Bilişsel hizmetler için sanal ağ ve hizmet uç noktası desteği belirli bir bölge kümesiyle sınırlıdır.

* [Azure bilişsel hizmetler sanal ağlarını yapılandırma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-virtual-networks?tabs=portal)

* [Azure sanal ağlarına genel bakış](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: sanal ağların, alt ağların ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Rehberlik**: sanal makineler, Azure bilişsel hizmetler Kapsayıcınız ile aynı sanal ağda dağıtıldığında veri sızdırma riskini azaltmak için ağ güvenlik grupları (NSG) kullanabilirsiniz. NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri Azure depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

* [NSG akış günlüklerini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Trafik Analizi etkinleştirme ve kullanma](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: bir kapsayıcı içinde bilişsel hizmetler kullanıyorsanız, kapsayıcı dağıtımınızı kötü amaçlı trafiğe filtre uygulayan ve uçtan uca TLS şifrelemesini destekleyen ve kapsayıcı uç noktasının özel ve güvenli kalmasını sağlayan öne bakan bir Web uygulaması güvenlik duvarı çözümü ile dağıtabilirsiniz.

Bilişsel hizmetler kapsayıcılarının faturalandırma amaçlarıyla ölçüm bilgilerini göndermesi gerektiğini göz önünde bulundurun. Tek özel durum, farklı bir faturalandırma metodolojisini izledikleri için çevrimdışı kapsayıcılardır. İzin verme hatası, bilişsel hizmetler kapsayıcılarının bağlı olduğu çeşitli ağ kanallarının kapsayıcının çalışmasını engelleyecek. Ana bilgisayar, liste bağlantı noktası 443 ve aşağıdaki etki alanlarına izin verir:
- *. cognitive.microsoft.com
- *. cognitiveservices.azure.com

Ayrıca, bilişsel hizmetler kapsayıcılarının Microsoft sunucuları için kullandığı güvenli kanallarda güvenlik duvarı çözümünüz için derin paket incelemesini devre dışı bırakmanız gerektiğini unutmayın. Bunun başarısız olması, kapsayıcının düzgün çalışmasını engeller.

* [Azure bilişsel hizmetler kapsayıcı güvenliğini anlama](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: sanal makineler, Azure bilişsel hizmetler Kapsayıcınız ile aynı sanal ağda dağıtıldığında, Azure ilkesiyle ilgili ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Redsıs örnekleri için Azure önbelleğinizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için, "Microsoft. Biliveservices" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. Ayrıca, gibi yerleşik ilke tanımlarından da yararlanabilirsiniz:
- DDoS koruma standardı etkinleştirilmelidir

Ayrıca, tek bir şema tanımında Azure Resource Manager şablonları, Azure rol tabanlı erişim denetimi (Azure RBAC) ve ilkeler gibi temel ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Şema 'i yeni abonelikler ve ortamlara kolayca uygulayın ve sürüm oluşturma aracılığıyla denetimi ve yönetimi ayrıntılı olarak ayarlayın.

Bir kapsayıcı içinde bilişsel hizmetler kullanıyorsanız, kapsayıcı dağıtımınızı kötü amaçlı trafiğe filtre uygulayan ve uçtan uca TLS şifrelemesini destekleyen ve kapsayıcı uç noktasının özel ve güvenli kalmasını sağlayan bir ön taraftaki Web uygulaması güvenlik duvarı çözümü ile dağıtabilirsiniz.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Blueprint oluşturma](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

* [Azure bilişsel hizmetler kapsayıcı güvenliğini anlama](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets"></a>1,5: ağ paketlerini kaydetme

**Rehberlik**: sanal makineler, Azure bilişsel hizmetler Kapsayıcınız ile aynı sanal ağda dağıtıldığında veri sızdırma riskini azaltmak için ağ güvenlik grupları (NSG) kullanabilirsiniz. NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri Azure depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

* [NSG akış günlüklerini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Trafik Analizi etkinleştirme ve kullanma](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Kılavuz**: bir kapsayıcı içinde bilişsel hizmetler kullanıyorsanız, kapsayıcı dağıtımınızı kötü amaçlı trafiğe filtre uygulayan ve uçtan uca TLS şifrelemesini destekleyen ve kapsayıcı uç noktasının özel ve güvenli kalmasını sağlayan bir ön uç Web uygulaması güvenlik duvarı çözümü ile dağıtabilirsiniz. Azure Marketi 'nden, kimlik denetimini devre dışı bırakabilme özelliği ile kimlikler/IP işlevlerini destekleyen bir teklif seçebilirsiniz.

Bilişsel hizmetler kapsayıcılarının faturalandırma amaçlarıyla ölçüm bilgilerini göndermesi gerektiğini göz önünde bulundurun. Tek özel durum, farklı bir faturalandırma metodolojisini izledikleri için çevrimdışı kapsayıcılardır. İzin verme hatası, bilişsel hizmetler kapsayıcılarının bağlı olduğu çeşitli ağ kanallarının kapsayıcının çalışmasını engelleyecek. Ana bilgisayar, liste bağlantı noktası 443 ve aşağıdaki etki alanlarına izin verir:
- *. cognitive.microsoft.com
- *. cognitiveservices.azure.com

Ayrıca, bilişsel hizmetler kapsayıcılarının Microsoft sunucuları için kullandığı güvenli kanallarda güvenlik duvarı çözümünüz için derin paket incelemesini devre dışı bırakmanız gerektiğini unutmayın. Bunun başarısız olması, kapsayıcının düzgün çalışmasını engeller.

* [Azure bilişsel hizmetler kapsayıcı güvenliğini anlama](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

* [Azure Market](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Kılavuz**: bir kapsayıcı içinde bilişsel hizmetler kullanıyorsanız, kapsayıcı dağıtımınızı kötü amaçlı trafiğe filtre uygulayan ve uçtan uca TLS şifrelemesini destekleyen ve kapsayıcı uç noktasının özel ve güvenli kalmasını sağlayan bir ön uç Web uygulaması güvenlik duvarı çözümü ile dağıtabilirsiniz.

Bilişsel hizmetler kapsayıcılarının faturalandırma amaçlarıyla ölçüm bilgilerini göndermesi gerektiğini göz önünde bulundurun. Tek özel durum, farklı bir faturalandırma metodolojisini izledikleri için çevrimdışı kapsayıcılardır. İzin verme hatası, bilişsel hizmetler kapsayıcılarının bağlı olduğu çeşitli ağ kanallarının kapsayıcının çalışmasını engelleyecek. Ana bilgisayar, liste bağlantı noktası 443 ve aşağıdaki etki alanlarına izin verir:
- *. cognitive.microsoft.com
- *. cognitiveservices.azure.com

Ayrıca, bilişsel hizmetler kapsayıcılarının Microsoft sunucuları için kullandığı güvenli kanallarda güvenlik duvarı çözümünüz için derin paket incelemesini devre dışı bırakmanız gerektiğini unutmayın. Bunun başarısız olması, kapsayıcının düzgün çalışmasını engeller.

* [Azure bilişsel hizmetler kapsayıcı güvenliğini anlama](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: ağ güvenlik grupları (NSG) veya Azure Güvenlik duvarında ağ erişim denetimlerini tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

Ayrıca, karmaşık güvenlik yapılandırmasını basitleştirmeye yardımcı olması için uygulama güvenlik grupları 'nı (ASG) de kullanabilirsiniz. ASGs, ağ güvenliğini bir uygulamanın yapısının doğal bir uzantısı olarak yapılandırmanıza olanak tanıyarak, sanal makineleri gruplandırmanızı ve bu gruplara göre ağ güvenlik ilkelerini tanımlamanızı sağlar.

* [Sanal ağ hizmeti etiketleri](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

* [Uygulama güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure ilkesiyle Azure bilişsel hizmetler kapsayıcınıza ilişkin ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Redsıs örnekleri için Azure önbelleğinizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için, "Microsoft. Biliveservices" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın.

Ayrıca, tek bir şema tanımında Azure Resource Manager şablonları, Azure rol tabanlı erişim denetimi (Azure RBAC) ve ilkeler gibi anahtar ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Şema 'i yeni abonelikler ve ortamlara kolayca uygulayın ve sürüm oluşturma aracılığıyla denetimi ve yönetimi ayrıntılı olarak ayarlayın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Blueprint oluşturma](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: Azure bilişsel hizmetler kapsayıcınızda ilişkili ağ kaynakları için Etiketler kullanarak bunları bir taksonomiyle mantıksal olarak organize edin.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğünü kullanarak ağ kaynak yapılandırmasını Izleyin ve Azure bilişsel hizmetler kapsayıcınıza ilişkin ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

* [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Azure Izleyici 'de uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, günlüklerde zaman damgalarına yönelik Azure bilişsel hizmetler gibi Azure kaynakları için kullanılan saat kaynağını korur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Kılavuz**: Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Etkinlik günlükleri, denetim düzlemi düzeyinde Azure bilişsel Hizmetler kapsayıcısında gerçekleştirilen işlemlere ilişkin öngörüler sağlar. Azure etkinlik günlüğü verilerini kullanarak, Redsıs örnekleri için Azure önbelleğiniz için denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" ı belirleyebilirsiniz.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: denetim düzlemi denetim günlüğü Için Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Azure etkinlik günlüğü verilerini kullanarak, Azure kaynaklarınızın denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, SILME) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Ayrıca, Azure bilişsel hizmetler, analiz, uyarı ve raporlama amacıyla toplanabilecek ve kullanılabilecek tanılama olayları gönderir. Bilişsel hizmetler kapsayıcısı için tanılama ayarlarını Azure portal aracılığıyla yapılandırabilirsiniz. Bir depolama hesabına, Olay Hub 'ına veya bir Log Analytics çalışma alanına bir veya daha fazla tanılama olayı gönderebilirsiniz.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Azure bilişsel hizmetler için tanılama ayarlarını kullanma](https://docs.microsoft.com/azure/cognitive-services/diagnostic-logging)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, Log Analytics çalışma alanı saklama dönemini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

* [Log Analytics çalışma alanları için günlük saklama parametrelerini ayarlama](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Kılavuz**: Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Bu Günlükler, sorun tanımlama ve hata ayıklama için kullanılan bir kaynağın çalışması hakkında zengin, sık veriler sağlar. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve Azure bilişsel hizmetler için toplanabilecek etkinlik günlüğü verilerine göre birçok diğer öngörü sağlamak için Log Analytics sorguları gerçekleştirin.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Azure Izleyici 'de Log Analytics çalışma alanında Azure etkinlik günlüklerini toplama ve çözümleme](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-collect)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Kılavuz**: Azure &amp; izleyici 'deki uyarı ölçümleri bölümüne giderek Azure bilişsel hizmetler 'deki desteklenen ölçümler üzerinde uyarılar oluşturabilirsiniz.

Bilişsel hizmetler Kapsayıcınız için tanılama ayarlarını yapılandırın ve günlükleri bir Log Analytics çalışma alanına gönderin. Log Analytics çalışma alanınızda, önceden tanımlanmış bir koşullar kümesi gerçekleşirken yapılacak uyarıları yapılandırın. Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Azure Izleyici 'yi kullanarak günlük uyarıları oluşturma, görüntüleme ve yönetme](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; Azure bilişsel hizmetler, kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Azure bilişsel hizmetler, DNS ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

* [Azure AD 'de PowerShell ile dizin rolü alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

* [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Azure bilişsel hizmetler 'e yönelik denetim düzlemi erişimi, Azure ACTIVE DIRECTORY (ad) ile denetlenir. Azure AD varsayılan parola kavramına sahip değildir.

Azure bilişsel hizmetler 'e veri düzlemi erişimi erişim anahtarları aracılığıyla denetlenir. Bu anahtarlar, önbelleğinize bağlanan istemciler tarafından kullanılır ve herhangi bir zamanda yeniden oluşturulabilir.

Uygulamanızda varsayılan parolalar oluşturmanız önerilmez. Bunun yerine, parolalarınızı Azure Key Vault kaydedebilir ve sonra bunları almak için Azure Active Directory kullanabilirsiniz.

* [Redsıs erişim tuşları için Azure önbelleğini yeniden üretme](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-configure#settings)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Ayrıca, özel yönetim hesaplarını izlemenize yardımcı olmak için Azure Güvenlik Merkezi veya yerleşik Azure Ilkeleri önerilerini kullanabilirsiniz; örneğin:
- Aboneliğinize birden fazla sahip atanmalıdır
- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

* [Kimlik ve erişim (Önizleme) izlemek için Azure Güvenlik Merkezi 'ni kullanma](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Azure Ilkesini kullanma](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Kılavuz**: Azure bilişsel hizmetler, kullanıcıların kimliğini doğrulamak için erişim anahtarları kullanır ve veri düzlemi düzeyinde çoklu oturum AÇMAYı (SSO) desteklemez. Azure bilişsel hizmetler için denetim düzlemine erişim, REST API aracılığıyla kullanılabilir ve SSO 'yu destekler. Kimlik doğrulaması yapmak için isteklerinizin yetkilendirme üst bilgisini Azure Active Directory aldığınız JSON Web Token ayarlayın.

* [Azure bilişsel hizmetler REST API anlayın](https://docs.microsoft.com/rest/api/cognitiveservices/)

* [Azure AD ile SSO 'yu anlama](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure ACTIVE DIRECTORY (AD) MULTI-Factor AUTHENTICATION (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

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

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik oluştuğunda günlükler ve uyarılar oluşturmak için Azure ACTIVE DIRECTORY (AD) PRIVILEGED IDENTITY Management (PIM) kullanın.

Ayrıca, riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın.

* [Privileged Identity Management dağıtma (PıM)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

* [Azure AD risk algılamalarını anlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek için Azure ACTIVE DIRECTORY (ad) koşullu erişim içindeki adlandırılmış konumları yapılandırın.

* [Azure 'da adlandırılmış konumları yapılandırma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar. Kullanım örneği AD kimlik doğrulamasını destekliyorsa, bilişsel hizmetler API 'nize yönelik isteklerin kimliğini doğrulamak için Azure AD 'yi kullanın.

Şu anda yalnızca Görüntü İşleme API'si, Yüz Tanıma API'si, Metin Analizi API'si, derinlikli okuyucu, form tanıyıcı, anomali algılayıcısı ve Azure AD 'yi kullanarak Bing Özel Arama kimlik doğrulaması desteği hariç tüm Bing hizmetleri.

* [Bilişsel hizmetler için isteklerin kimliğini doğrulama](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-azure-active-directory)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Kılavuz**: Azure AD, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, müşteri, grup üyeliklerini verimli bir şekilde yönetmek için Azure kimlik erişimi Incelemelerini kullanmaya, kurumsal uygulamalara erişime ve rol atamalarına de olanak sağlar. Kullanıcının erişimi, yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için düzenli olarak incelenebilir.

API Management Kullanıcı hesaplarının envanterini sürdürmek için müşteri, gerektiğinde erişimi uzlaştırır. API Management, geliştiriciler API Management kullanarak kullanıma sunabileceğiniz API 'lerin kullanıcılardır. Varsayılan olarak, yeni oluşturulan geliştirici hesapları etkindir ve geliştiriciler grubuyla ilişkilendirilir. Etkin durumda olan geliştirici hesapları, aboneliklerine sahip oldukları tüm API 'Lere erişmek için kullanılabilir.

* [Azure API Management'ta kullanıcı hesaplarını yönetme](https://docs.microsoft.com/azure/api-management/api-management-howto-create-or-invite-developers)

* [API Management kullanıcıların listesini alma](https://docs.microsoft.com/powershell/module/az.apimanagement/get-azapimanagementuser?view=azps-3.1.0)

* [Azure kimlik erişimi Incelemelerini kullanma](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Kılavuz**: Azure Sentinel veya üçüncü taraf SIEM ile tümleştirmenize olanak tanıyan Azure ACTIVE DIRECTORY (ad) oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişiminiz vardır.

Bu işlemi, Azure AD Kullanıcı hesapları için Tanılama ayarları oluşturarak ve Log Analytics çalışma alanına denetim günlüklerini ve oturum açma günlüklerini göndererek kolaylaştırabilirsiniz. Log Analytics içinde, istenen günlük uyarılarını yapılandırabilirsiniz.

* [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

* [Yerleşik Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: denetim düzleminde hesap oturum açma davranışı sapması için, Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırmak üzere Azure ACTIVE DIRECTORY (ad) kimlik koruması ve risk algılama özelliklerini kullanın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz.

* [Azure AD riskli oturum açma işlemlerini görüntüleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: henüz kullanılamıyor; Müşteri Kasası henüz Azure bilişsel hizmetler için desteklenmiyor.

* [Müşteri Kasası tarafından desteklenen hizmetler listesi](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Şu anda kullanılamıyor

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Kaynaklar VNet/subnet ile ayrılmalıdır, uygun şekilde etiketlenebilir ve bir NSG veya Azure güvenlik duvarıyla korunmuş olmalıdır. Hassas verileri depolayan veya işleyen kaynaklar yeterince yalıtılmalıdır. Hassas verileri depolayan veya işleyen sanal makineler için, kullanımda olmadığında devre dışı bırakmak üzere ilke ve yordam uygulayın.

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

* [Güvenlik Yapılandırması ile NSG oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

* [Azure Güvenlik duvarını dağıtma](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

* [Azure Güvenlik Duvarı ile uyarı veya uyarı yapılandırma ve reddetme](https://docs.microsoft.com/azure/firewall/threat-intel)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: henüz kullanılamıyor; veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz Azure bilişsel hizmetler için kullanılabilir değil.

Microsoft, Azure bilişsel hizmetler için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Şu anda kullanılamıyor

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: http üzerinden kullanıma sunulan bilişsel hizmetler uç noktaları, TLS 1,2 ' i zorlar. Zorunlu bir güvenlik protokolüyle, bilişsel hizmetler uç noktasını çağırmaya çalışan tüketiciler şu yönergelere uymalıdır:
- İstemci Işletim sistemi (OS), TLS 1,2 ' i desteklemelidir.
- HTTP çağrısının isteğin bir parçası olarak TLS 1,2 ' i belirtmesini sağlamak için kullanılan dil (ve platform). (Dile ve platforma bağlı olarak, TLS belirtmek örtülü olarak veya açık olarak yapılır.)

* [Azure bilişsel hizmetler için Aktarım katmanı güvenliğini anlama](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-security)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz Azure bilişsel hizmetler için kullanılabilir değil. Gizli bilgiler içeren örnekleri Etiketler ve uyumluluk amaçları için gerekliyse üçüncü taraf çözümü uygular.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Kılavuz**: Azure bilişsel hizmetler Denetim düzlemi 'ne erişimi denetlemek için Azure rol tabanlı erişim denetimi 'Ni (Azure RBAC) kullanın (örneğin, Azure Portal).

* [Azure RBAC 'yi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft, Azure bilişsel hizmetler için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: bilişsel hizmetler için bekleyen şifreleme, kullanılan belirli bir hizmete bağımlıdır. Çoğu durumda, veriler şifrelenir ve FIPS 140-2 uyumlu 256 bit AES şifrelemesi kullanılarak çözülür. Şifreleme ve şifre çözme saydamdır, bu da şifreleme ve erişimin sizin için yönetilip yönetilmediğini belirtir. Verileriniz varsayılan olarak güvenlidir ve şifreleme avantajlarından yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez.

Ayrıca, müşteri tarafından yönetilen anahtarlarınızın depolanması için Azure Key Vault de kullanabilirsiniz. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz.

* [Bekleyen bilgileri şifreleyen hizmetlerin listesi](https://docs.microsoft.com/azure/cognitive-services/encryption/cognitive-services-encryption-keys-portal)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure bilişsel hizmetler 'in üretim örneklerine ve diğer kritik veya ilgili kaynaklara yönelik değişikliklerin ne zaman gerçekleştiği hakkında uyarı oluşturmak Için Azure etkinlik günlüğü Ile Azure izleyici 'yi kullanın.

* [Azure etkinlik günlüğü olayları için uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: Microsoft, Azure bilişsel hizmetler 'i destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Microsoft

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: üçüncü taraf yazılım başlıkları için otomatik düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: Microsoft, Azure bilişsel hizmetler 'i destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

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

**Rehberlik**: uygun yerlerde, redsıs örnekleri ve ilgili kaynaklar Için Azure önbelleğini düzenlemek ve izlemek için etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ayrıca, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri abonelikleri içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure Ilkesi 'ni kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynakları envanterini tanımlama ve sürdürme

**Rehberlik**: uygulanamaz; Bu öneri, bir bütün olarak işlem kaynaklarına ve Azure 'a yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Ayrıca, Azure Kaynak Grafiği 'ni kullanarak abonelikler içindeki kaynakları sorgular/bulur.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Kaynak Graf ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: uygulanamaz; Bu öneri, bir bütün olarak işlem kaynaklarına ve Azure 'a yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilkesiyle belirli bir kaynak türünü reddetme](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: onaylanan yazılım başlıkları envanterini koruyun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak üzere Azure koşullu erişimini yapılandırın.

* [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: Azure ilkesiyle Azure bilişsel hizmetler Kapsayıcınız için standart güvenlik yapılandırması tanımlayın ve uygulayın. Redsıs örnekleri için Azure önbelleğinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Biliveservices" ad alanındaki Azure Ilke diğer adlarını kullanın.

* [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilke efektlerini anlama](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: Azure bilişsel hizmetler kapsayıcıları ve ilgili kaynaklarınız Için özel Azure ilke tanımları veya Azure Resource Manager şablonları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın.

* [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure Repos belgeleri](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft. cache" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak üzere özel Azure ilke tanımları oluşturmak Için "Microsoft. Biliveservices" ad alanındaki Azure ilke diğer adlarını kullanın. Redsıs örnekleri ve ilgili kaynaklar için Azure önbelleğiniz için otomatik olarak yapılandırmaların uygulanmasını sağlamak üzere Azure Ilkesi [audit], [Deny] ve [dağıtım yok] kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Kılavuz**: Azure BILIŞSEL hizmetler API 'nize erişmek için kullanılan Azure App Service çalışan Azure sanal makineleri veya Web uygulamaları Için, Azure bilişsel hizmetler anahtar yönetimini basitleştirmek ve güvenli hale getirmek için Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın. Key Vault geçici silmenin etkinleştirildiğinden emin olun.

* [Azure yönetilen kimliklerle tümleştirme](https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity)

* [Key Vault oluşturma](https://docs.microsoft.com/azure/key-vault/quick-create-portal)

* [Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama](https://docs.microsoft.com/azure/key-vault/managed-identity)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Kılavuz**: Azure BILIŞSEL hizmetler API 'nize erişmek için kullanılan Azure App Service çalışan Azure sanal makineleri veya Web uygulamaları Için, Azure bilişsel hizmetler anahtar yönetimini basitleştirmek ve güvenli hale getirmek için Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın. Key Vault geçici silmenin etkinleştirildiğinden emin olun.

Azure Active Directory ' de otomatik olarak yönetilen kimlik ile Azure hizmetleri sağlamak için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Azure Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

* [Yönetilen kimlikleri yapılandırma](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm)

* [Azure yönetilen kimliklerle tümleştirme](https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

* [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure bilişsel hizmetler) etkinleştirilir, ancak müşteri içeriklerde çalıştırılmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, redin Için Azure önbelleği) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.

App Service, Data Lake Storage, BLOB depolama, PostgreSQL için Azure veritabanı vb. gibi işlem dışı Azure kaynaklarına karşıya yüklenen tüm içerikleri önceden tarayın. Microsoft bu örneklerdeki verilerinize erişemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure bilişsel hizmetler) etkinleştirilir, ancak müşteri içeriklerde çalıştırılmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: Microsoft Azure Depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman otomatik olarak çoğaltılır. Azure depolama, verilerinizi geçici donanım arızaları, ağ veya güç kesintileri ve çok büyük doğal olağanüstü durumlar dahil olmak üzere planlı ve planlanmamış olaylardan korunabilecek şekilde kopyalar. Verilerinizi aynı veri merkezinde, aynı bölgedeki ZGen veri merkezlerinde veya coğrafi olarak ayrılmış bölgelerde çoğaltmayı tercih edebilirsiniz.

Ayrıca, verileri arşiv katmanına yedeklemek için yaşam döngüsü yönetimi özelliğini de kullanabilirsiniz. Ayrıca, depolama hesabında depolanan yedeklemeleriniz için geçici silme özelliğini etkinleştirin.

* [Azure depolama yedekliliği ve hizmet düzeyi sözleşmelerini anlama](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

* [Azure Blob depolama yaşam döngüsünü yönetme](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts)

* [Azure Depolama blobları için geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: bilişsel hizmetler ve ilgili kaynakları dağıtmak için Azure Resource Manager kullanın. Azure Resource Manager, çözümünüzü geliştirme yaşam döngüsü boyunca yeniden dağıtmanıza ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olmanızı sağlayan şablonları dışarı aktarma özelliğini sağlar. Azure Resource Manager şablonu dışarı aktarma API 'sini düzenli aralıklarla çağırmak için Azure Otomasyonu 'nu kullanın. Önceden paylaşılan anahtarları Azure Key Vault içinde yedekleyin.

* [Azure Resource Manager'a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/management/overview)

* [Azure Resource Manager şablonu kullanarak bilişsel hizmetler kaynağı oluşturma](https://docs.microsoft.com/azure/cognitive-services/resource-manager-template?tabs=portal)

* [Azure portal bir şablona tek ve çoklu kaynak verme](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

* [Kaynak grupları-şablonu dışarı aktarma](https://docs.microsoft.com/rest/api/resources/resourcegroups/exporttemplate)

* [Azure Otomasyonu 'na giriş](https://docs.microsoft.com/azure/automation/automation-intro)

* [Azure 'da Anahtar Kasası anahtarlarını yedekleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: gerektiğinde yalıtılmış bir aboneliğe düzenli olarak Azure Resource Manager şablonlarının dağıtımını düzenli olarak gerçekleştirdiğinizden emin olun. Yedeklenen önceden paylaşılan anahtarların test geri yüklemesi.

* [ARM şablonları ve Azure portal kaynak dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-portal)

* [Azure 'da Anahtar Kasası anahtarlarını geri yükleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: Azure Resource Manager şablonlarınızı güvenli bir şekilde depolamak ve yönetmek Için Azure DevOps kullanın. Azure DevOps 'da yönettiğiniz kaynakları korumak için, belirli kullanıcılara, yerleşik güvenlik gruplarına veya Azure DevOps ile tümleşikse veya TFS ile tümleşikse Active Directory Azure Active Directory (Azure AD) içinde tanımlanan gruplara izin verebilir veya vermeyebilirsiniz.  Müşteri tarafından yönetilen anahtarları korumak için rol tabanlı erişim denetimi kullanın. Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault ' de geçici silme ve Temizleme korumasını etkinleştirin. 

* [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure DevOps 'da izinler ve gruplar hakkında](https://docs.microsoft.com/azure/devops/organizations/security/about-permissions)

* [Key Vault 'da geçici silme ve Temizleme korumasını etkinleştirme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

* [Azure Güvenlik Merkezi 'nde Iş akışı otomasyonlarını yapılandırma](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

* [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Ayrıca, NıST 'nin bilgisayar güvenliği olay Işleme kılavuzumuzu, kendi olay yanıtı planınızın oluşturulmasına yardımcı olması için de kullanabilirsiniz](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

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

**Rehberlik**: * [Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
