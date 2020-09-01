---
title: Redsıs için Azure önbelleği için Azure Güvenlik temeli
description: Redsıs için Azure önbelleği için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 215d31ee4797dd3e7eef88d3a6679f2256f0ffae
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89225987"
---
# <a name="azure-security-baseline-for-azure-cache-for-redis"></a>Redsıs için Azure önbelleği için Azure Güvenlik temeli

Redde Azure önbelleği için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: bir sanal ağ (VNet) içinde redsıs örneği Için Azure önbelleğinizi dağıtın. VNet, buluttaki özel bir ağ. Redsıs örneği için bir Azure önbelleği bir sanal ağ ile yapılandırıldığında, bu, genel olarak adreslenebilir değildir ve yalnızca VNet içindeki sanal makineler ve uygulamalardan erişilebilir.

Ayrıca, bir başlangıç ve bitiş IP adresi aralığı ile güvenlik duvarı kuralları belirtebilirsiniz. Güvenlik duvarı kuralları yapılandırıldığında, yalnızca belirtilen IP adresi aralıklarından gelen istemci bağlantıları önbelleğe bağlanabilir.

Redsıs için Premium Azure önbelleği için sanal ağ desteğini yapılandırma:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-vnet

Redsıs güvenlik duvarı kuralları için Azure önbelleğini yapılandırma:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-configure#firewall

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Rehberlik**: sanal makineler redsıs örneği Için Azure önbelleğiniz ile aynı sanal ağda dağıtıldığında, veri sızdırma riskini azaltmak için ağ güvenlik grupları (NSG) kullanabilirsiniz. NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri Azure depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

NSG akış günlüklerini etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Trafik Analizi etkinleştirme ve kullanma:

https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: Azure sanal ağ (VNet) dağıtımı, Azure önbelleğiniz için gelişmiş güvenlik ve yalıtımın yanı sıra alt ağlar, erişim denetim ilkeleri ve diğer özellikler için erişimi daha da kısıtlamak amacıyla sağlar. Bir sanal ağa dağıtıldığında Redsıs için Azure önbelleği genel olarak adreslenebilir ve yalnızca sanal makine ve VNet içindeki uygulamalardan erişilebilir.

Reddo örnekleri için Azure önbelleğiyle ilişkili sanal ağlarda, dağıtılmış hizmet reddi (DDoS) saldırılarına karşı koruma sağlamak için DDoS koruma standardı 'nı etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

Redsıs için Premium Azure önbelleği için sanal ağ desteğini yapılandırma:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-vnet

Azure DDoS koruması standardını Azure portal kullanarak yönetin:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Rehberlik**: sanal makineler redsıs örneği Için Azure önbelleğiniz ile aynı sanal ağda dağıtıldığında, veri sızdırma riskini azaltmak için ağ güvenlik grupları (NSG) kullanabilirsiniz. NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri Azure depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

NSG akış günlüklerini etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Trafik Analizi etkinleştirme ve kullanma:

https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: redsıs Için Azure önbelleğini kullanırken Azure App Service veya işlem örneklerinde çalışan Web uygulamalarınız varsa, bir Azure sanal ağı (VNet) içindeki tüm kaynakları dağıtın ve Web Application Gateway bir Azure Web uygulaması güvenlik duvarı (WAF) ile güvenli hale getirin. WAF 'yi "önleme modunda" çalışacak şekilde yapılandırın. Önleme modu, kuralların algılamadığı yetkisiz ve saldırıları engeller. Saldırgan bir "403 yetkisiz erişim" özel durumu alır ve bağlantı kapatılır. Önleme modu WAF günlüklerinde bu tür saldırıları kaydeder.

Alternatif olarak, Azure Marketi 'nden, yük İnceleme ve/veya anomali algılama özellikleri ile KIMLIKLER/IP 'ler işlevselliğini destekleyen bir teklif seçebilirsiniz.

Azure WAF yeteneklerini anlayın:

https://docs.microsoft.com/azure/web-application-firewall/ag/ag-overview

Azure WAF 'yi dağıtma:

https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag

Azure Marketi:

https://azuremarketplace.microsoft.com/marketplace/?term=Firewall

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: ağ güvenlik grupları (NSG) veya Azure Güvenlik duvarında ağ erişim denetimlerini tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

Ayrıca, karmaşık güvenlik yapılandırmasını basitleştirmeye yardımcı olması için uygulama güvenlik grupları 'nı (ASG) de kullanabilirsiniz. ASGs, ağ güvenliğini bir uygulamanın yapısının doğal bir uzantısı olarak yapılandırmanıza olanak tanıyarak, sanal makineleri gruplandırmanızı ve bu gruplara göre ağ güvenlik ilkelerini tanımlamanızı sağlar.

Sanal ağ hizmeti Etiketleri:

https://docs.microsoft.com/azure/virtual-network/service-tags-overview

Uygulama güvenlik grupları:

https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure ilkesiyle redsıs örnekleri Için Azure önbelleğiniz ile ilgili ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Redsıs örnekleri için Azure önbelleğinizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için, "Microsoft. cache" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. Ayrıca, gibi yerleşik ilke tanımlarından da yararlanabilirsiniz:

Yalnızca Redis Cache güvenli bağlantılar etkinleştirilmelidir

DDoS koruma standardı etkinleştirilmelidir

Ayrıca, tek bir şema tanımında Azure Resource Manager (ARM) şablonları, Azure rol tabanlı erişim denetimi (Azure RBAC) ve ilkeler gibi anahtar ortam yapıtları sunarak büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Şema 'i yeni abonelikler ve ortamlara kolayca uygulayın ve sürüm oluşturma aracılığıyla denetimi ve yönetimi ayrıntılı olarak ayarlayın.

Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Blueprint oluşturma:

https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: bir taksonomi halinde mantıksal olarak organize etmek için redsıs dağıtımı Için Azure önbelleğiniz ile ilişkili ağ kaynakları için Etiketler kullanın.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğünü kullanarak ağ kaynak yapılandırmasını Izleyin ve redsıs örnekleri Için Azure önbelleğiniz ile ilgili ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

Azure etkinlik günlüğü olaylarını görüntüleme ve alma:

https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Izleyici 'de uyarı oluşturma:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, günlüklerdeki zaman damgalarına yönelik Azure önbelleği gibi Azure kaynakları için kullanılan saat kaynağını korur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Kılavuz**: Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Etkinlik günlükleri, denetim düzlemi düzeyinde Redsıs örnekleri için Azure önbelleğiniz üzerinde gerçekleştirilen işlemlere ilişkin öngörüler sağlar. Azure etkinlik günlüğü verilerini kullanarak, Redsıs örnekleri için Azure önbelleğiniz için denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" ı belirleyebilirsiniz.

Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme: https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Etkinlik günlükleri, denetim düzlemi düzeyinde Redsıs örnekleri için Azure önbelleğiniz üzerinde gerçekleştirilen işlemlere ilişkin öngörüler sağlar. Azure etkinlik günlüğü verilerini kullanarak, Redsıs örnekleri için Azure önbelleğiniz için denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" ı belirleyebilirsiniz.

Tanılama ayarlarını etkinleştirerek ölçümler kullanılabilir olsa da, veri düzleminde denetim günlüğü Redsıs için Azure önbelleği için henüz kullanılamıyor.

Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme: https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, kuruluşunuzun uyumluluk düzenlemelerine göre redsıs örnekleri Için Azure önbelleğiniz ile ilişkili Log Analytics çalışma alanları için günlük tutma süresini ayarlayın.

Veri düzlemine yönelik denetim günlüğü kaydının, Redsıs için Azure önbelleği için henüz kullanılabilir olmadığına unutmayın.

Günlük tutma parametrelerini ayarlama:

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Kılavuz**: Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve Reda için Azure önbelleği için toplanabilecek etkinlik günlüğü verilerine göre birçok diğer öngörü sağlamak için Log Analytics sorguları gerçekleştirin.

Veri düzlemine yönelik denetim günlüğü kaydının, Redsıs için Azure önbelleği için henüz kullanılabilir olmadığına unutmayın.

Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme: https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

Azure Izleyici 'de Log Analytics çalışma alanında Azure etkinlik günlüklerini toplama ve çözümleme: https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-collect

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Rehberlik**: redsıs örnekleri Için Azure önbelleğiniz ile ilgili ölçümleri ve etkinlik günlüklerini temel alarak uyarı alacak şekilde yapılandırabilirsiniz. Azure Izleyici, bir e-posta bildirimi göndermek, Web kancası çağırmak veya bir Azure mantıksal uygulaması çağırmak için bir uyarı yapılandırmanızı sağlar.

Tanılama ayarlarını etkinleştirerek ölçümler kullanılabilir olsa da, veri düzleminde denetim günlüğü Redsıs için Azure önbelleği için henüz kullanılamıyor.

Redsıs için Azure önbelleği uyarılarını yapılandırma: https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-monitor#alerts

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; Redsıs için Azure önbelleği, kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Redsıs için Azure önbelleği, DNS ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

Azure AD 'de PowerShell ile dizin rolü alma: https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma: https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Redsıs Için Azure önbelleğine denetim düzlemi erişimi, Azure ACTIVE DIRECTORY (ad) ile denetlenir. Azure AD varsayılan parola kavramına sahip değildir. 

Redo için Azure önbelleğine veri düzlemi erişimi, erişim anahtarları aracılığıyla denetlenir. Bu anahtarlar, önbelleğinize bağlanan istemciler tarafından kullanılır ve herhangi bir zamanda yeniden oluşturulabilir.

Uygulamanızda varsayılan parolalar oluşturmanız önerilmez. Bunun yerine, parolalarınızı Azure Key Vault kaydedebilir ve sonra bunları almak için Azure Active Directory kullanabilirsiniz.

Redsıs erişim anahtarları için Azure önbelleğini yeniden üretme: https://docs.microsoft.com/azure/azure-cache-for-redis/cache-configure#settings

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Ayrıca, özel yönetim hesaplarını izlemenize yardımcı olmak için Azure Güvenlik Merkezi veya yerleşik Azure Ilkeleri önerilerini kullanabilirsiniz; örneğin:

- Aboneliğinize birden fazla sahip atanmalıdır

- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır

- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

Kimlik ve erişim (Önizleme) izlemek için Azure Güvenlik Merkezi 'ni kullanma: https://docs.microsoft.com/azure/security-center/security-center-identity-access

Azure Ilkesini kullanma: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: redsıs Için Azure Cache, kullanıcıların kimliğini doğrulamak için erişim anahtarlarını kullanır ve veri düzlemi düzeyinde çoklu oturum AÇMAYı (SSO) desteklemez. Redde için Azure önbelleği denetim düzlemine erişim, REST API aracılığıyla kullanılabilir ve SSO 'yu destekler. Kimlik doğrulaması yapmak için isteklerinizin yetkilendirme üst bilgisini Azure Active Directory aldığınız JSON Web Token ayarlayın.

Redsıs REST API için Azure önbelleğini anlayın: https://docs.microsoft.com/rest/api/redis/

Azure AD ile SSO 'yu anlayın: https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure ACTIVE DIRECTORY (AD) MULTI-Factor AUTHENTICATION (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

Azure 'da MFA 'yı etkinleştirme: https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme: https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve yapılandırmak için yapılandırılmış MULTI-Factor AUTHENTICATION (MFA) ile ayrıcalıklı erişim iş istasyonları (Paw) kullanın.

Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin:

https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure 'da MFA 'yı etkinleştirme:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik oluştuğunda günlükler ve uyarılar oluşturmak için Azure ACTIVE DIRECTORY (AD) PRIVILEGED IDENTITY Management (PIM) kullanın.

Ayrıca, riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın.

Privileged Identity Management dağıtma (PıM): https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan

Azure AD risk algılamalarını anlama: https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek için Azure ACTIVE DIRECTORY (ad) koşullu erişim içindeki adlandırılmış konumları yapılandırın.

Azure 'da adlandırılmış konumlar nasıl yapılandırılır: https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

Azure AD kimlik doğrulaması, Redsıs ' veri düzlemi için Azure önbelleğine doğrudan erişim için kullanılamaz, ancak Azure AD kimlik bilgileri, denetim düzlemi düzeyinde yönetim için kullanılabilir (örneğin, Azure portal) Redsıs erişim anahtarlarına yönelik Azure önbelleğini denetlemek için kullanılabilir.


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) eski hesapları keşfetmenize yardımcı olacak Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir. 

Azure AD raporlamayı anlayın: https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Azure kimlik erişimi Incelemelerini kullanma: https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Kılavuz**: Azure Sentinel veya üçüncü taraf SIEM ile tümleştirmenize olanak tanıyan Azure ACTIVE DIRECTORY (ad) oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişiminiz vardır.

Bu işlemi, Azure AD Kullanıcı hesapları için Tanılama ayarları oluşturarak ve Log Analytics çalışma alanına denetim günlüklerini ve oturum açma günlüklerini göndererek kolaylaştırabilirsiniz. Log Analytics içinde, istenen günlük uyarılarını yapılandırabilirsiniz.

Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme: https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

Yerleşik Azure Sentinel: https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: denetim düzleminde hesap oturum açma davranışı sapması için, Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırmak üzere Azure ACTIVE DIRECTORY (ad) kimlik koruması ve risk algılama özelliklerini kullanın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz.

Azure AD riskli oturum açma işlemlerini görüntüleme: https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme: https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

Azure Sentinel 'i ekleme: https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: henüz kullanılamıyor; Müşteri Kasası, Redsıs için Azure önbelleği için henüz desteklenmiyor.

Müşteri Kasası tarafından desteklenen hizmetler listesi:

https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: geçerli değil

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Redsıs örnekleri için Azure önbelleği, sanal ağ/alt ağ ile ayrılmalıdır ve uygun şekilde etiketlenemez. İsteğe bağlı olarak, yalnızca belirtilen IP adresi aralıklarından gelen istemci bağlantılarının önbelleğe bağlanabilmesi için kurallar tanımlamak üzere Redsıs güvenlik duvarı için Azure önbelleğini kullanın.

Ek Azure abonelikleri oluşturma:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları oluşturma:

https://docs.microsoft.com/azure/governance/management-groups/create

Redsıs için Azure önbelleğini VNET 'e dağıtma:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-vnet

Redsıs güvenlik duvarı kuralları için Azure önbelleğini yapılandırma:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-configure#firewall

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: henüz kullanılamıyor; veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz redin için Azure önbelleği için kullanılabilir değil.

Microsoft, Redsıs için Azure önbelleği için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

Azure 'da müşteri veri korumasını anlayın: https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: redsıs Için Azure önbelleği varsayılan olarak TLS şifreli iletişimler gerektirir. TLS sürümleri 1,0, 1,1 ve 1,2 Şu anda desteklenmektedir. Ancak, bu durumda TLS 1,0 ve 1,1, sektör genelinde kullanımdan kalkmaya yönelik bir yoldur. bu nedenle, tüm mümkünse TLS 1,2 kullanın. İstemci kitaplığınız veya aracınız TLS desteklemiyorsa, şifrelenmemiş bağlantıları etkinleştirmek Azure portal veya yönetim API 'Leri aracılığıyla yapılabilir. Şifrelenmiş bağlantıların mümkün olmadığı durumlarda, önbelleğinizi ve istemci uygulamanızı bir sanal ağa yerleştirmek önerilir.

Redsıs için Azure önbelleği için iletimde şifrelemeyi anlayın:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-best-practices

VNET önbellek senaryolarında kullanılan gerekli bağlantı noktalarını anlayın:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-vnet#outbound-port-requirements

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: veri tanımlama, sınıflandırma ve kayıp önleme özellikleri, Redsıs Için Azure önbelleği için henüz kullanılamıyor. Gizli bilgiler içeren örnekleri Etiketler ve uyumluluk amaçları için gerekliyse üçüncü taraf çözümü uygular.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

Azure 'da müşteri veri korumasını anlayın: https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Rehberlik**: redsıs denetim düzlemi Için Azure önbelleğine erişimi denetlemek için Azure rol tabanlı erişim denetimi 'Ni (Azure RBAC) kullanın (örneğin, Azure Portal). 

Azure RBAC 'yi yapılandırma:

https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft, Redsıs için Azure önbelleği için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

Azure 'da müşteri veri korumasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: redsıs Için Azure Cache, müşteri verilerini bellekte depolar ve Microsoft tarafından uygulanan çok sayıda denetim tarafından güçlü bir şekilde korunuyorsa, bellek varsayılan olarak şifrelenmez. Kuruluşunuz tarafından gerekliyse, Redsıs için Azure önbelleğinde depolamadan önce içeriği şifreleyin.

Redsıs özelliği için Azure önbellek "Redsıs veri kalıcılığı" kullanılıyorsa, veriler sahip olduğunuz ve yönettiğiniz bir Azure depolama hesabına gönderilir. Önbellek oluşturma sırasında ve mevcut Premium önbellekler için kaynak menüsünde, "Redsıs için yeni Azure önbelleği" dikey penceresinden kalıcılığı yapılandırabilirsiniz.

Azure depolama 'daki veriler, 256 bit AES şifrelemesi kullanılarak şifrelenmiş ve şifresi çözülür, en güçlü blok şifrelemeleri kullanılabilir ve FIPS 140-2 uyumludur. Azure depolama şifrelemesi devre dışı bırakılamıyor. Depolama hesabınızın şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya kendi anahtarınızla şifrelemeyi yönetebilirsiniz.

Redsıs için Azure önbelleğinde kalıcılığı yapılandırma: https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-persistence

Azure depolama hesapları için şifrelemeyi anlayın: https://docs.microsoft.com/azure/storage/common/storage-service-encryption

Azure müşteri verileri korumasını anlayın: https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Reda ve diğer kritik veya ilgili kaynaklar Için Azure Cache 'in üretim örneklerine değişiklik gerçekleşirken uyarı oluşturmak üzere Azure etkinlik günlüğü Ile Azure izleyici 'yi kullanın.

Azure etkinlik günlüğü olayları için uyarı oluşturma:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: redsıs örnekleri ve ilgili kaynaklar Için Azure önbelleğinizi güvenli hale getirmenin Azure Güvenlik Merkezi önerilerini izleyin.

Microsoft, Redsıs için Azure önbelleğini destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

Azure Güvenlik Merkezi önerilerini anlayın: https://docs.microsoft.com/azure/security-center/recommendations-reference

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: Microsoft, redsıs Için Azure önbelleğini destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz dahilinde (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) tüm kaynakları sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın.  Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerini numaralandırın.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

Azure Kaynak Graf ile sorgu oluşturma: https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure aboneliklerinizi görüntüleme: https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure RBAC 'yi anlama: https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: uygun yerlerde, redsıs örnekleri ve ilgili kaynaklar Için Azure önbelleğini düzenlemek ve izlemek için etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ayrıca, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri abonelikleri içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure Ilkesi 'ni kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Ek Azure abonelikleri oluşturma: https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları oluşturma: https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini koruyun

**Rehberlik**: uygulanamaz; Bu öneri, bir bütün olarak işlem kaynaklarına ve Azure 'a yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:

İzin verilmeyen kaynak türleri

İzin verilen kaynak türleri

Ayrıca, Azure Kaynak Grafiği 'ni kullanarak abonelikler içindeki kaynakları sorgular/bulur.

Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

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

İzin verilmeyen kaynak türleri

İzin verilen kaynak türleri

Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilkesiyle belirli bir kaynak türünü reddetme:

https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6,11: kullanıcıların betikler aracılığıyla Azure Resources Manager ile etkileşim kurma yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure Resource Manager (ARM) ile etkileşime geçmesini sınırlamak Için Azure koşullu erişimini yapılandırın.

ARM 'e erişimi engellemek için koşullu erişimi yapılandırma:

https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

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

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: Azure Ilkesi ile redsıs örnekleri Için Azure önbelleğiniz için standart güvenlik yapılandırması tanımlayın ve uygulayın. Redsıs örnekleri için Azure önbelleğinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için, "Microsoft. cache" ad alanındaki Azure Ilke diğer adlarını kullanın. Ayrıca, şu gibi Redsıs örnekleri için Azure önbelleğiniz ile ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz:

Yalnızca Redis Cache güvenli bağlantılar etkinleştirilmelidir

Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme: https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilke efektlerini anlayın: https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Rehberlik**: redsıs örnekleri ve ilgili kaynaklar Için Azure önbelleğiniz Için özel Azure ilke tanımları veya Azure Resource Manager şablonları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın.

Azure DevOps 'da kod depolama: https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Repos belgeleri: https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft. cache" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft. cache" ad alanındaki Azure ilke diğer adlarını kullanın. Redsıs örnekleri ve ilgili kaynaklar için Azure önbelleğiniz için otomatik olarak yapılandırmaların uygulanmasını sağlamak üzere Azure Ilkesi [audit], [Deny] ve [dağıtım yok] kullanın.

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: redsıs örnekleri Için Azure önbelleğinize erişmek üzere kullanılan Azure App Service üzerinde çalışan Azure sanal makineleri veya Web uygulamaları Için, redsıs gizli yönetimi Için Azure önbelleğini basitleştirmek ve güvenli hale getirmek üzere Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın. Key Vault geçici silmenin etkinleştirildiğinden emin olun.

Azure yönetilen kimliklerle tümleştirme:

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Key Vault oluşturma:

https://docs.microsoft.com/azure/key-vault/quick-create-portal

Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama:

https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: redsıs örnekleri Için Azure önbelleğinize erişmek üzere kullanılan Azure App Service üzerinde çalışan Azure sanal makineleri veya Web uygulamaları Için, redsıs gizli yönetimi Için Azure önbelleğini basitleştirmek ve güvenli hale getirmek üzere Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın. Key Vault geçici silmenin etkinleştirildiğinden emin olun.

Azure Active Directory ' de otomatik olarak yönetilen kimlik ile Azure hizmetleri sağlamak için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Azure Key Vault dahil olmak üzere AAD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

Yönetilen kimlikleri yapılandırma:

https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

Azure yönetilen kimliklerle tümleştirme:

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

Kimlik bilgisi tarayıcısını ayarlama: https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure App Service) etkinleştirilir, ancak müşteri içeriklerde çalıştırılmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, redin Için Azure önbelleği) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.

App Service, Data Lake Storage, BLOB depolama, PostgreSQL için Azure veritabanı vb. gibi işlem dışı Azure kaynaklarına karşıya yüklenen tüm içerikleri önceden tarayın. Microsoft bu örneklerdeki verilerinize erişemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayar üzerinde etkinleştirilmiştir (örneğin, Reddır için Azure önbelleği), ancak müşteri içeriği üzerinde çalışmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: redsıs kalıcılığını etkinleştirin. Redsıs kalıcılığı, redin içinde depolanan verileri kalıcı hale getirebilmeniz için izin verir. Ayrıca, anlık görüntüler alabilir ve verileri yedekleyebilir ve bu da donanım arızası durumunda yükleyebilirsiniz. Bu, tüm verilerin bellekte depolandığı temel veya standart katmandan çok büyük bir avantajdır ve önbellek düğümlerinin kapatılmasında oluşabilecek bir hata olması durumunda olası veri kaybı olabilir.

Redsıs dışarı aktarma için Azure önbelleği de kullanabilirsiniz. Dışarı aktarma, redin için Azure önbelleğinde depolanan verileri Redsıs uyumlu RDB dosyaları 'na dışarı aktarıp verbırakmanıza olanak tanır. Bu özelliği, Redsıs örneği için bir Azure önbelleğinden diğerine veya başka bir Redsıs sunucusuna veri taşımak için kullanabilirsiniz. Dışarı aktarma işlemi sırasında, Redsıs sunucu örneği için Azure önbelleğini barındıran sanal makinede geçici bir dosya oluşturulur ve dosya belirtilen depolama hesabına yüklenir. Dışarı aktarma işlemi başarı veya başarısızlık durumu ile tamamlandığında geçici dosya silinir.

Redsıs kalıcılığını etkinleştirme:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-persistence

Redsıs dışarı aktarma için Azure önbelleğini kullanma:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-import-export-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: redsıs kalıcılığını etkinleştirin. Redsıs kalıcılığı, redin içinde depolanan verileri kalıcı hale getirebilmeniz için izin verir. Ayrıca, anlık görüntüler alabilir ve verileri yedekleyebilir ve bu da donanım arızası durumunda yükleyebilirsiniz. Bu, tüm verilerin bellekte depolandığı temel veya standart katmandan çok büyük bir avantajdır ve önbellek düğümlerinin kapatılmasında oluşabilecek bir hata olması durumunda olası veri kaybı olabilir.

Redsıs dışarı aktarma için Azure önbelleği de kullanabilirsiniz. Dışarı aktarma, redin için Azure önbelleğinde depolanan verileri Redsıs uyumlu RDB dosyaları 'na dışarı aktarıp verbırakmanıza olanak tanır. Bu özelliği, Redsıs örneği için bir Azure önbelleğinden diğerine veya başka bir Redsıs sunucusuna veri taşımak için kullanabilirsiniz. Dışarı aktarma işlemi sırasında, Redsıs sunucu örneği için Azure önbelleğini barındıran sanal makinede geçici bir dosya oluşturulur ve dosya belirtilen depolama hesabına yüklenir. Dışarı aktarma işlemi başarı veya başarısızlık durumu ile tamamlandığında geçici dosya silinir.

Redsıs örnekleri için Azure önbelleğiniz kimlik bilgilerini depolamak üzere Azure Key Vault kullanıyorsanız, anahtarlarınızın düzenli otomatik yedeklemelerini sağlayın.

Redsıs kalıcılığını etkinleştirme:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-persistence

Redsıs dışarı aktarma için Azure önbelleğini kullanma:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-import-export-data

Key Vault anahtarlarını yedekleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: Redsıs içeri aktarma Için Azure önbelleğini kullanın. İçeri aktarma, Linux, Windows veya Amazon Web Services gibi herhangi bir bulut sağlayıcısında çalışan bir bulut veya ortamda çalışan redsıs sunucusundan, Redsıs uyumlu RDB dosyalarını getirmek için kullanılabilir. Verilerin içeri aktarılması, önceden doldurulmuş verilerle bir önbellek oluşturmanın kolay bir yoludur. İçeri aktarma işlemi sırasında Redsıs için Azure önbelleği, RDB dosyalarını Azure depolama alanından belleğe yükler ve ardından anahtarları önbelleğe ekler.

Azure Key Vault Sırlarınızın düzenli olarak veri geri yüklemesini test edin.

Redsıs Içeri aktarma için Azure önbelleğini kullanma:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-import-export-data

Key Vault gizli dizileri geri yükleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultsecret?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: Redsıs dışa aktarma ve redsıs kalıcılığı Için Azure önbelleği, seçili Azure depolama hesabınızda depolanır. Azure depolama 'daki veriler, 256 bit AES şifrelemesi kullanılarak şifrelenmiş ve şifresi çözülür, en güçlü blok şifrelemeleri kullanılabilir ve FIPS 140-2 uyumludur. Azure depolama şifrelemesi devre dışı bırakılamıyor. Depolama hesabınızın şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya kendi anahtarınızla şifrelemeyi yönetebilirsiniz.

Azure depolama hesapları için şifrelemeyi anlayın: https://docs.microsoft.com/azure/storage/common/storage-service-encryption

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Microsoft

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

Azure Güvenlik Merkezi 'nde Iş akışı Otomatiklamalar nasıl yapılandırılır:

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu:

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın Anatomisi:

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu de kullanabilir:

https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin:

https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.  Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarılar Sentinel 'i akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

Sürekli dışarı aktarmayı yapılandırma:

https://docs.microsoft.com/azure/security-center/continuous-export

Uyarıları Azure Sentinel 'e aktarma:

https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

Iş akışı otomasyonunu yapılandırma ve Logic Apps:

https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve 60 gün içinde tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin:

https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz: 

https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
