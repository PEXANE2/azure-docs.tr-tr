---
title: Event Hubs için Azure Güvenlik temeli
description: Event Hubs için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: e49a6b8000def0fe175930629b33203adec1b0f5
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537063"
---
# <a name="azure-security-baseline-for-event-hubs"></a>Event Hubs için Azure Güvenlik temeli

Event Hubs için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](../security/benchmarks/overview.md)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](../security/benchmarks/security-baselines-overview.md).

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: sanal ağ hizmet uç noktaları ile Olay Hub 'larının tümleştirilmesi, sanal ağlara bağlı sanal makineler gibi iş yüklerinden, her iki uçta da güvenli hale getirilen ağ trafiği yolu ile güvenli erişim sağlar.

En az bir sanal ağ alt ağ hizmeti uç noktasına bağlandıktan sonra, ilgili Event Hubs ad alanı artık trafiği sanal ağlardaki her yerden kabul etmez. Sanal ağ perspektifinden, Event Hubs ad alanınızı bir hizmet uç noktasına bağlamak, sanal ağ alt ağından mesajlaşma hizmetine yalıtılmış bir ağ tüneli yapılandırır. 

Azure özel bağlantı hizmetini kullanarak özel olarak ve güvenli bir şekilde Azure Event Hubs hizmetine bağlayan bir ağ arabirimi olan özel bir uç nokta da oluşturabilirsiniz. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor. Hizmete giden tüm trafik özel uç nokta aracılığıyla yönlendirilebilir, bu nedenle ağ geçitleri, NAT cihazları, ExpressRoute veya VPN bağlantıları ya da genel IP adresleri gerekmez. 

Azure Event Hubs ad alanınızı güvenlik duvarları kullanarak da güvenli hale getirebilirsiniz. Azure Event Hubs, gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini destekler. Azure portal, Azure Resource Manager şablonlarını veya Azure CLı veya Azure PowerShell aracılığıyla güvenlik duvarı kuralları belirleyebilirsiniz.

Azure Event Hubs ile sanal ağ hizmet uç noktalarını kullanma:https://docs.microsoft.com/azure/event-hubs/event-hubs-service-endpoints

Daha fazla bilgi için bkz. Azure Event Hubs Azure özel bağlantısı ile tümleştirme: https://docs.microsoft.com/azure/event-hubs/private-link-service .

Event Hubs ad alanı üzerinde sanal ağlar tümleştirmesini ve güvenlik duvarlarını etkinleştirin:https://docs.microsoft.com/azure/event-hubs/event-hubs-tutorial-virtual-networks-firewalls

Azure Event Hubs ad alanları için IP güvenlik duvarı kurallarını yapılandırma:https://docs.microsoft.com/azure/event-hubs/event-hubs-ip-filtering

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Rehberlik**: Azure Güvenlik Merkezi 'ni kullanın ve azure 'daki Event Hubs kaynaklarınızın güvenliğini sağlamaya yardımcı olmak için ağ koruma önerilerini izleyin. Olay Hub 'larınızı erişmek için Azure sanal makineleri kullanıyorsanız, ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir depolama hesabına gönderin.

NSG akış günlüklerini etkinleştirme:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama:https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: dağıtılmış hizmet reddi (DDoS) saldırılarına karşı koruma sağlamak için, Olay Hub 'larınızla ilişkili sanal ağlarda DDoS koruma standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

DDoS korumasını yapılandırma:[https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection](../virtual-network/manage-ddos-protection.md)

Azure Güvenlik Merkezi tümleşik tehdit bilgileri hakkında daha fazla bilgi için:https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Rehberlik**: Olay Hub 'larınızı erişmek için Azure sanal makinelerini kullanıyorsanız, ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

Anormal etkinlikleri araştırmak için gerekliyse, ağ Izleyicisi paket yakalamayı etkinleştirin.

NSG akış günlüklerini etkinleştirme:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Trafik Analizi etkinleştirme ve kullanma:https://docs.microsoft.com/azure/network-watcher/traffic-analytics

Ağ Izleyicisi 'ni etkinleştirme:https://docs.microsoft.com/azure/network-watcher/network-watcher-create

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: Olay Hub 'larınız için Azure sanal makineleri kullanıyorsanız, yük İnceleme ÖZELLIKLERINE sahip kimlikleri/IP 'Leri destekleyen Azure Marketi 'nden bir teklif seçin. Kuruluşunuz için yük incelemesini temel alan yetkisiz giriş algılama ve/veya önleme gerekmiyorsa Azure Event Hubs ' yerleşik güvenlik duvarı özelliğini kullanabilirsiniz. Güvenlik duvarı kurallarını kullanarak, sınırlı bir IP adresi aralığı veya belirli bir IP adresi için Event Hubs ad uzayına erişimi sınırlayabilirsiniz.

Azure Marketi:

https://azuremarketplace.microsoft.com/marketplace/?term=Firewall

Belirli bir IP adresi için Event Hubs bir güvenlik duvarı kuralı ekleme:

 https://docs.microsoft.com/azure/event-hubs/event-hubs-ip-filtering

**Azure Güvenlik Merkezi izleme**: henüz kullanılamıyor

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: uygulanamaz, bu öneri Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure Ilkesi ile Azure Event Hubs ad boşluklarınızla ilişkili ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Event Hubs ad alanlarınızın ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. EventHub" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. Azure Event Hubs ile ilgili yerleşik ilke tanımlarını da kullanabilirsiniz; örneğin:

- Olay Hub 'ı bir sanal ağ hizmeti uç noktası kullanmalıdır.

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Event Hubs ad alanı için Azure yerleşik Ilkesi:https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#event-hub



Ağ için Azure Ilke örnekleri:https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network



Azure Blueprint oluşturma:https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: sanal ağlar ve Olay Hub 'larınızla ilişkili ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını Izleyin ve Azure Event Hubs ile ilgili ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

Azure etkinlik günlüğü olaylarını görüntüleme ve alma:https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Izleyici 'de uyarı oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: uygulanamaz; Microsoft, günlüklerdeki zaman damgaları için Azure Event Hubs gibi Azure kaynakları için kullanılan saat kaynağını korur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Kılavuz**: Azure izleyici 'de, olayları bir Log Analytics çalışma alanına veya uzun vadeli arşiv depolamaya yönelik bir depolama hesabına göndermek Için etkinlik günlüğü ve Olay Hub 'ı Tanılama ayarları içindeki olay hub 'ları ile ilgili günlükleri yapılandırın.

Azure Event Hubs için tanılama ayarlarını yapılandırma:https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs

Azure etkinlik günlüğünü anlama:https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: Azure Event Hubs ad alanınız için tanılama ayarlarını etkinleştirin. Azure Event Hubs için üç tanılama ayarı kategorisi vardır: Arşiv günlükleri, Işletimsel Günlükler ve otomatik ölçeklendirme günlükleri. İşlem günlüklerini, Olay Hub 'ı oluşturma, kullanılan kaynaklar ve işlem durumu dahil olmak üzere Event Hubs işlemleri sırasında oluşan işlemler, özellikle de işlem türü hakkında bilgi yakalamak için etkinleştirin.

Ayrıca, Azure etkinlik günlüğü tanılama ayarlarını etkinleştirebilir ve bunları bir Azure depolama hesabına, Olay Hub 'ına veya Log Analytics çalışma alanına gönderebilirsiniz. Etkinlik günlükleri, Azure Event Hubs ve diğer kaynaklarda gerçekleştirilen işlemlere ilişkin öngörüler sağlar. Etkinlik günlüklerini kullanarak, Azure Event Hubs ad boşluklarında herhangi bir yazma işlemi (PUT, POST, SILME) için "ne, kim ve ne zaman" i belirleyebilirsiniz.

Azure Event Hubs için tanılama ayarlarını etkinleştirme:https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs

Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme:https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, Olay Hub 'ı ile ilgili olayları yakalamak ve gözden geçirmek için kuruluşunuzun uyumluluk düzenlemelerine göre Log Analytics çalışma alanı saklama süresini ayarlayın.

Log Analytics çalışma alanları için günlük bekletme parametreleri ayarlama:https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: anormal davranış için günlükleri çözümleyin ve izleyin ve Olay Hub 'larıyla ilgili sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics kullanın. Alternatif olarak, Azure Sentinel 'e veya üçüncü taraf SıEM 'ye yönelik verileri etkinleştirebilir.
 

Log Analytics çalışma alanı hakkında daha fazla bilgi için:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Izleyici 'de özel sorgular gerçekleştirme:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries

Azure Sentinel 'i ekleme:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Kılavuz**: Azure izleyici 'de, etkinlik günlüğü içinde Azure Event Hubs ile ilgili günlükleri yapılandırın ve bir Log Analytics çalışma alanına veya uzun vadeli arşiv depolaması için bir depolama hesabına Günlükler göndermek üzere tanılama ayarlarını Event Hubs. Güvenlik günlükleri ve olayları 'nda bulunan anormal etkinlikler için uyarı oluşturmak üzere Log Analytics çalışma alanını kullanın.

Alternatif olarak, Azure Sentinel 'de ve yerleşik verileri etkinleştirebilir. 

Azure etkinlik günlüğünü anlayın:https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview

Azure Event Hubs için tanılama ayarlarını yapılandırma:https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs

Log Analytics çalışma alanı günlük verilerinde uyarı alma:https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response

Azure Sentinel 'i ekleme:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: henüz kullanılamıyor

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; Olay Hub 'ı kötü amaçlı yazılımdan koruma günlüğünü işlemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Event Hubs DNS ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın. 

Azure AD 'de PowerShell ile dizin rolü alma:https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma:https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: denetim düzlemi erişimi Event Hubs Azure ACTIVE DIRECTORY (ad) ile denetlenir. Azure AD varsayılan parola kavramına sahip değildir.

Event Hubs veri düzlemi erişimi, Yönetilen kimlikler veya Uygulama kayıtları ile paylaşılan erişim imzaları ile Azure AD aracılığıyla denetlenir. Paylaşılan erişim imzaları, Olay Hub 'larınızı bağlayan istemciler tarafından kullanılır ve herhangi bir zamanda yeniden oluşturulabilir.

Event Hubs için paylaşılan erişim imzalarını anlayın:https://docs.microsoft.com/azure/event-hubs/authenticate-shared-access-signature

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Ayrıca, özel yönetim hesaplarını izlemenize yardımcı olmak için Azure Güvenlik Merkezi veya yerleşik Azure Ilkeleri önerilerini kullanabilirsiniz; örneğin:

- Aboneliğinize birden fazla sahip atanmalıdır

- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır

- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

Kimlik ve erişim (Önizleme) izlemek için Azure Güvenlik Merkezi 'ni kullanma:https://docs.microsoft.com/azure/security-center/security-center-identity-access

Azure Ilkesini kullanma:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: Microsoft Azure, Azure ACTIVE DIRECTORY (ad) temelinde kaynak ve uygulamalar için tümleşik erişim denetimi yönetimi sağlar. Azure AD 'yi Azure Event Hubs kullanmanın önemli bir avantajı, kimlik bilgilerinizi artık kodda depolamanızı gerektirmez. Bunun yerine, Microsoft Identity platform 'dan bir OAuth 2,0 erişim belirteci isteyebilirsiniz. Belirteç istemek için kaynak adı https: \/ /eventhubs.Azure.net/. Azure AD, uygulamayı çalıştıran güvenlik sorumlusu (bir Kullanıcı, Grup veya hizmet sorumlusu) kimliğini doğrular. Kimlik doğrulaması başarılı olursa, Azure AD uygulamaya bir erişim belirteci döndürür ve uygulama Azure Event Hubs kaynaklarına istek yetkilendirmek için erişim belirtecini kullanabilir.

Event Hubs kaynaklarına erişmek için Azure AD ile bir uygulamanın kimliğini doğrulama:https://docs.microsoft.com/azure/event-hubs/authenticate-application

Azure AD ile SSO 'yu anlama:https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Active Directory MULTI-Factor AUTHENTICATION (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini Izleyerek Olay Hub 'ı özellikli kaynaklarınızın korunmasına yardımcı olun.

Azure 'da MFA 'yı etkinleştirme:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme:https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Rehberlik**: Olay Hub 'ı etkin kaynakları oturum açmak ve yapılandırmak için yapılandırılmış MULTI-Factor AUTHENTICATION (MFA) ile ayrıcalıklı erişim iş istasyonları (Paw) kullanın.

Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin:https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure 'da MFA 'yı etkinleştirme:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik oluştuğunda günlükler ve uyarılar oluşturmak için Azure ACTIVE DIRECTORY (AD) PRIVILEGED IDENTITY Management (PIM) kullanın. Riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın. Ek günlük kaydı için Azure Güvenlik Merkezi risk algılama uyarılarını Azure Izleyicisine gönderin ve eylem gruplarını kullanarak özel uyarı/bildirim yapılandırın.

Privileged Identity Management dağıtma (PıM):https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan

Azure AD risk algılamalarını anlama:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

Özel uyarı ve bildirim için eylem gruplarını yapılandırma:https://docs.microsoft.com/azure/azure-monitor/platform/action-groups

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek Için adlandırılmış konumlar kullanın.



Azure 'da adlandırılmış konumlar nasıl yapılandırılır:https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: Event Hubs gibi Azure kaynakları için merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Bu, yönetim duyarlı kaynaklara rol tabanlı erişim denetimi (RBAC) sağlar.

 Azure AD örneği oluşturma ve yapılandırma:https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant

Azure Event Hubs Azure Active Directory (AAD) ile nasıl tümleştirildiğini öğrenmek için bkz. Azure Active Directory kullanarak Event Hubs kaynaklarına erişimi yetkilendirme:https://docs.microsoft.com/azure/event-hubs/authorize-access-azure-active-directory

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) eski hesapları keşfetmenize yardımcı olacak Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir.

Ek olarak, Event Hubs ' paylaşılan erişim imzalarınızı düzenli olarak döndürün.

Azure AD raporlamayı anlayın:https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Azure kimlik erişimi Incelemelerini kullanma:https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

Event Hubs için paylaşılan erişim imzalarını anlama:https://docs.microsoft.com/azure/event-hubs/authenticate-shared-access-signature

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Rehberlik**: herhangi bir SIEM/izleme aracıyla tümleştirmenize imkan tanıyan Azure ACTIVE DIRECTORY (ad) oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişiminiz vardır.

Bu işlemi, Azure AD Kullanıcı hesapları için Tanılama ayarları oluşturarak ve Log Analytics çalışma alanına denetim günlüklerini ve oturum açma günlüklerini göndererek kolaylaştırabilirsiniz. Log Analytics içinde, istenen günlük uyarılarını yapılandırabilirsiniz.

Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme:https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

Azure Active Directory kullanarak Event Hubs kaynaklarına erişim yetkisi verme:https://docs.microsoft.com/azure/event-hubs/authorize-access-azure-active-directory

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: Event Hubs etkin kaynaklarla ilgili şüpheli eylemler için otomatik yanıtları yapılandırmak üzere Azure Active Directory kimlik koruması ve risk algılama özelliklerini kullanın. Kuruluşunuzun güvenlik yanıtlarını uygulamak için Azure Sentinel aracılığıyla otomatikleştirilmiş yanıtları etkinleştirmeniz gerekir.

Azure AD riskli oturum açma işlemlerini görüntüleme:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme:https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

Azure Sentinel 'i ekleme:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: Şu anda kullanılamıyor; Müşteri Kasası, Event Hubs için henüz desteklenmiyor.

Müşteri Kasası tarafından desteklenen hizmetler listesi:https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Şu anda kullanılamıyor

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olmak için Event Hubs ilişkili kaynaklarda Etiketler kullanın.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Event Hubs ad alanları, hizmet uç noktaları etkinleştirilmiş ve uygun şekilde etiketlenmiş sanal ağ ile ayrılmalıdır.

Azure Event Hubs ad alanınızı güvenlik duvarları kullanarak da güvence altına alabilirsiniz. Azure Event Hubs, gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini destekler. Azure portal, Azure Resource Manager şablonlarını veya Azure CLı veya Azure PowerShell aracılığıyla güvenlik duvarı kuralları belirleyebilirsiniz.

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları oluşturma:https://docs.microsoft.com/azure/governance/management-groups/create

Azure Event Hubs ad alanları için IP güvenlik duvarı kurallarını yapılandırın:https://docs.microsoft.com/azure/event-hubs/event-hubs-ip-filtering

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Sanal ağ oluşturma:https://docs.microsoft.com/azure/virtual-network/quick-create-portal

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: Olay Hub 'larınıza erişmek için sanal makineleri kullanırken, veri kaybı olasılığını azaltmak için sanal ağlar, hizmet uç noktaları, Event Hubs güvenlik duvarı, ağ güvenlik grupları ve hizmet etiketleri kullanın.

Microsoft, Azure Event Hubs için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

Azure Event Hubs ad alanları için IP güvenlik duvarı kurallarını yapılandırın:https://docs.microsoft.com/azure/event-hubs/event-hubs-ip-filtering

Azure Event Hubs ile sanal ağ hizmet uç noktalarını anlayın:https://docs.microsoft.com/azure/event-hubs/event-hubs-service-endpoints

Azure Event Hubs Azure özel bağlantısıyla tümleştirin:https://docs.microsoft.com/azure/event-hubs/private-link-service

Ağ güvenlik gruplarını ve hizmet etiketlerini anlayın:https://docs.microsoft.com/azure/virtual-network/security-overview

Azure 'da müşteri veri korumasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: Azure Event Hubs varsayılan olarak TLS şifreli iletişimleri zorlar. TLS sürümleri 1,0, 1,1 ve 1,2 Şu anda desteklenmektedir. Ancak, bu durumda TLS 1,0 ve 1,1, sektör genelinde kullanımdan kalkmaya yönelik bir yoldur. bu nedenle, tüm mümkünse TLS 1,2 kullanın.

Event Hubs 'nin güvenlik özelliklerini anlamak için bkz. ağ güvenliği:https://docs.microsoft.com/azure/event-hubs/network-security

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: veri tanımlama, sınıflandırma ve kayıp önleme özellikleri Azure Event Hubs için henüz kullanılamıyor. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

Azure 'da müşteri veri korumasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Rehberlik**: Azure Event Hubs, istekleri Event Hubs kaynaklara yetkilendirmek için Azure ACTIVE DIRECTORY (ad) kullanmayı destekler. Azure AD ile rol tabanlı erişim denetimi 'ni (RBAC), bir kullanıcı veya uygulama hizmeti sorumlusu olabilecek bir güvenlik sorumlusu için izin vermek üzere kullanabilirsiniz.

Azure AD RBAC ve Azure Event Hubs için kullanılabilir rolleri anlayın:https://docs.microsoft.com/azure/event-hubs/authorize-access-azure-active-directory

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

Microsoft, Event Hubs için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

Azure 'da müşteri veri korumasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Kılavuz**: Azure Event Hubs, bekleyen verileri Microsoft tarafından yönetilen anahtarlarla veya müşteri tarafından yönetilen anahtarlarla şifreleme seçeneğini destekler. Bu özellik, Azure Event Hubs verilerini bekleyen bir şekilde şifrelemek için kullanılan müşteri tarafından yönetilen anahtarlara erişimi oluşturmanıza, döndürmenize, devre dışı bırakmanızı ve iptal etmenize olanak sağlar.

Azure Event Hubs şifrelemek için müşteri tarafından yönetilen anahtarları yapılandırma:https://docs.microsoft.com/azure/event-hubs/configure-customer-managed-key

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure izleyici 'Yi Azure etkinlik günlüğü ile birlikte kullanarak Azure Event Hubs 'in üretim örneklerine ve diğer önemli veya ilgili kaynaklara yönelik değişikliklerin ne zaman gerçekleştiği hakkında uyarılar oluşturun.

Azure etkinlik günlüğü olayları için uyarı oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: uygulanamaz; Microsoft, Event Hubs destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Microsoft, Event Hubs destekleyen temel sistemlerde Düzeltme Eki Yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Kıyaslama işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Microsoft, Event Hubs destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: uygulanamaz; Microsoft, Event Hubs destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz dahilinde (Azure Event Hubs ad alanları dahil) tüm kaynakları sorgulamak ve saptamak Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun.

Azure Kaynak Graf ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure aboneliklerinizi görüntüleme:https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure RBAC 'yi anlama:https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure Event Hubs ad alanlarını ve ilgili kaynakları düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları oluşturma:https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini koruyun

**Rehberlik**: uygulanamaz; Bu öneri, bir bütün olarak işlem kaynaklarına ve Azure 'a yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Ayrıca, Azure Kaynak Grafiği 'ni kullanarak abonelikler içindeki kaynakları sorgular/bulur.

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

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

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilkesiyle belirli bir kaynak türünü reddetme:https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-divlimit-users-ability-to-interact-with-azure-resource-manager-via-scriptsdiv"></a>6,11: <div>Kullanıcıların betikler aracılığıyla Azure Resource Manager etkileşim kurma yeteneğini sınırlayın</div>

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak üzere Azure koşullu erişimini yapılandırın.

Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma:https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

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

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: Azure Event Hubs dağıtımlarınız için standart güvenlik yapılandırması tanımlayın ve uygulayın. Konfigürasyonları denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. EventHub" ad alanındaki Azure Ilke diğer adlarını kullanın. Azure Event Hubs için yerleşik ilke tanımlarından da yararlanabilirsiniz:

- Olay Hub 'ındaki tanılama günlükleri etkinleştirilmelidir

- Olay Hub 'ı bir sanal ağ hizmeti uç noktası kullanmalıdır

Event Hubs ad alanı için Azure yerleşik Ilkesi:https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#event-hub

Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme:https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Rehberlik**: Event Hubs etkinleştirilmiş kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yok] kullanın. 

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

 
Azure Ilke etkileri hakkında daha fazla bilgi için:https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Rehberlik**: Event Hubs veya ilgili kaynaklarınız Için özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın.

Azure DevOps 'da kod depolama:https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Repos belgeleri:https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft. EventHub" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft. EventHub" ad alanındaki Azure ilke diğer adlarını kullanın. Azure Event Hubs dağıtımlarınız ve ilgili kaynaklarınızın yapılandırmasını otomatik olarak zorlamak için [Denetim], [reddetme] ve [dağıtım yok] Azure Ilkesini kullanın.

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Kılavuz**: Azure sanal makineleri veya Olay Hub 'larınıza erişmek için kullanılan Azure App Service çalışan Web uygulamaları Için, Azure Event Hubs dağıtımlarınız için paylaşılan erişim imza yönetimini basitleştirmek ve güvenli hale getirmek üzere Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın. Key Vault geçici silme özelliğinin etkinleştirildiğinden emin olun.

Event Hubs kaynaklara erişmek için Azure Active Directory bir yönetilen kimliğin kimliğini doğrulayın:https://docs.microsoft.com/azure/event-hubs/authenticate-managed-identity?tabs=latest

Event Hubs için müşteri tarafından yönetilen anahtarları yapılandırın:https://docs.microsoft.com/azure/event-hubs/configure-customer-managed-key 

Azure yönetilen kimliklerle tümleştirme:https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Key Vault oluşturma:https://docs.microsoft.com/azure/key-vault/quick-create-portal

Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama:https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Kılavuz**: Azure sanal makineleri veya Olay Hub 'larınız için kullanılan Azure App Service çalışan Web uygulamaları Için, Azure Event Hubs 'yi basitleştirmek ve güvenli hale getirmek için Azure Key Vault birlikte yönetilen hizmet kimliği kullanın. Key Vault geçici silme özelliğinin etkinleştirildiğinden emin olun.

Azure Active Directory (AD) içinde otomatik olarak yönetilen bir kimlik ile Azure hizmetleri sağlamak için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Azure Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

Event Hubs kaynaklara erişmek için Azure Active Directory bir yönetilen kimliğin kimliğini doğrulayın:https://docs.microsoft.com/azure/event-hubs/authenticate-managed-identity?tabs=latest 

Event Hubs için müşteri tarafından yönetilen anahtarları yapılandırın:https://docs.microsoft.com/azure/event-hubs/configure-customer-managed-key 

Yönetilen kimlikleri yapılandırma:https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

Azure yönetilen kimliklerle tümleştirme:https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

Kimlik bilgisi tarayıcısını ayarlama:https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure App Service) etkinleştirilir, ancak müşteri içeriklerde çalıştırılmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Kılavuz**: Azure Event Hubs, App Service, Data Lake Storage, BLOB depolama, PostgreSQL Için Azure veritabanı vb. gibi Işlem dışı Azure kaynaklarına yüklenen tüm içerikleri önceden tarayın. Microsoft bu örneklerdeki verilerinize erişemez.

Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayar üzerinde etkinleştirilmiştir (örneğin, Reddır için Azure önbelleği), ancak müşteri içeriği üzerinde çalışmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Kılavuz**: Azure Event Hubs için coğrafi olağanüstü durum kurtarmayı yapılandırma. Azure bölgelerinin veya veri merkezlerinin (hiçbir kullanılabilirlik alanı kullanılmıyorsa) çalışma süresi kapalı kalma süresi, veri işlemenin farklı bir bölgede veya veri merkezinde çalışmaya devam edebilmesi için kritik öneme sahiptir. Bu nedenle, coğrafi olağanüstü durum kurtarma ve coğrafi çoğaltma, herhangi bir kuruluş için önemli özelliklerdir. Azure Event Hubs, ad alanı düzeyinde hem coğrafi olağanüstü durum kurtarmayı hem de Coğrafi çoğaltmayı destekler. 

Azure Event Hubs coğrafi olağanüstü durum kurtarmayı anlayın:https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr#availability-zones

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Kılavuz**: Azure Event Hubs, Azure depolama hizmeti şifrelemesi (Azure SSE) ile bekleyen verilerin şifrelenmesini sağlar. Event Hubs, verileri depolamak için Azure depolama 'yı kullanır ve varsayılan olarak, Azure Storage ile depolanan tüm veriler Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Müşteri tarafından yönetilen anahtarları depolamak için Azure Key Vault kullanıyorsanız, anahtarlarınızın düzenli otomatik yedeklemelerini sağlayın.

Aşağıdaki PowerShell komutuyla Key Vault Sırlarınızın düzenli otomatik yedeklemelerini sağlayın: Backup-AzKeyVaultSecret

Bekleyen Azure Event Hubs verilerini şifrelemek için müşteri tarafından yönetilen anahtarları yapılandırma:https://docs.microsoft.com/azure/event-hubs/configure-customer-managed-key

Key Vault gizli dizileri yedekleme:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultsecret

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: yedeklenen müşteri tarafından yönetilen anahtarların sınamasını geri yükleme.

 

Azure 'da Anahtar Kasası anahtarlarını geri yükleme:https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault geçici silme özelliğini etkinleştirin. Azure Event Hubs, müşteri tarafından yönetilen anahtarların geçici silme ve bu şekilde temizlenme gerektirir.

Event Hubs verileri yakalamak için kullanılan Azure depolama hesabı için geçici silme yapılandırın. Bu özelliğin henüz Azure Data Lake Storage Gen 2 için desteklenmediğini unutmayın.

Key Vault 'da geçici silmeyi etkinleştirme:https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

Anahtarlar içeren bir Anahtar Kasası ayarlayın:https://docs.microsoft.com/azure/event-hubs/configure-customer-managed-key

Azure depolama Blobları için geçici silme:https://docs.microsoft.com//azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: personel rollerinin yanı sıra olay işleme/yönetim aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun.

Azure Güvenlik Merkezi 'nde Iş akışı Otomatiklamalar nasıl yapılandırılır:https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, her uyarıya katılma sırasını önceliklendirmenize yardımcı olmak için uyarılara önem derecesi atar. böylece, bir kaynak tehlikeye girdiğinde, hemen bu işe başlayabilirsiniz. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin:https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.  Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin. 

Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama:https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarılar Sentinel 'i akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

Sürekli dışarı aktarmayı yapılandırma:https://docs.microsoft.com/azure/security-center/continuous-export

Uyarıları Azure Sentinel 'e aktarma:https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

Iş akışı otomasyonunu yapılandırma ve Logic Apps:https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve 60 gün içinde tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için lütfen Microsoft katılım kurallarını izleyin: https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1 .
Microsoft 'un yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına yönelik kırmızı ekip oluşturma ve canlı site sızma testini yürütmeye yönelik daha fazla bilgi edinebilirsiniz:https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin
