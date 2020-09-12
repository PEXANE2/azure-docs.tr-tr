---
title: Container Instances için Azure Güvenlik temeli
description: Container Instances için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 05/27/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: a26581b61a4b99bd11f48a3d431a1bb85148d66b
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89393437"
---
# <a name="azure-security-baseline-for-container-instances"></a>Container Instances için Azure Güvenlik temeli

Container Instances için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](../security/benchmarks/overview.md)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](../security/benchmarks/security-baselines-overview.md).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: Azure sanal ağı, Azure ve şirket içi kaynaklarınız için güvenli, özel ağ sağlar. Azure sanal ağıyla Azure Container Instances kapsayıcı gruplarınızı tümleştirin. 

* [Sanal ağ senaryoları ve kaynakları-Azure Container Instances](./container-instances-virtual-network-concepts.md)

* [Kapsayıcı örneklerini Azure sanal ağına dağıtma](./container-instances-vnet.md)

* [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

* [Azure Güvenlik duvarını dağıtma ve yapılandırma](../firewall/tutorial-firewall-deploy-portal.md)


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Rehberlik**: Azure Güvenlik Merkezi 'ni kullanarak ağ kaynaklarınızı Azure 'da korumanıza yardımcı olması için ağ koruma önerilerini düzeltin. Trafik denetimi için NSG akış günlüklerini etkinleştirin ve günlükleri bir depolama hesabına gönderin.

* [NSG akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

* [Ağ kaynaklarınızı koruyun](../security-center/security-center-network-recommendations.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Kılavuz**: Azure Web uygulaması güvenlik duvarını (WAF), gelen trafiğin ek incelemesi için Azure Container Instances içinde barındırılan kritik Web uygulamalarının önünde dağıtın. WAF ve alma günlükleri için tanılama ayarını bir depolama hesabı, Olay Hub 'ı veya Log Analytics çalışma alanında etkinleştirin.

* [Azure WAF dağıtma](../web-application-firewall/ag/create-waf-policy-ag.md)



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: DDoS saldırılarına karşı koruma sağlamak Için Azure sanal ağlarınızda DDoS standart korumasını etkinleştirin. Bilinen kötü amaçlı IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın. Tehdit zekası etkinleştirilmiş ve kötü amaçlı ağ trafiği için "uyarı ve reddetme" olarak yapılandırılmış bir kuruluşun ağ sınırlarının her birinde Azure Güvenlik Duvarı 'Nı dağıtın. NSG 'leri, sınırlı bir süre için bitiş noktaları onaylı IP adresleriyle sınırlamak üzere Azure Güvenlik Merkezi 'ni tam zamanında ağ erişimi ile kullanın. Bağlantı noktalarını ve kaynak IP 'Leri gerçek trafik ve tehdit zeka göre sınırlayan NSG yapılandırmalarının kullanılması için Azure Güvenlik Merkezi Uyarlamalı ağ sağlamlaştırma kullanın. 

* [Azure Güvenlik duvarını dağıtma](../firewall/tutorial-firewall-deploy-portal.md)

* [Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama](../security-center/threat-protection.md)

* [Azure Güvenlik Merkezi Uyarlamalı ağ sağlamlaştırma 'yi anlama](../security-center/security-center-adaptive-network-hardening.md)

* [Azure Güvenlik Merkezi tam zamanında ağ Access Control](../security-center/security-center-just-in-time.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Rehberlik**: sanal ağ uygulamanız Ile ağ güvenlik grupları (NSG 'ler) kullanıyorsanız, Azure Container Instances için atanan alt ağa eklenen NSG için NSG akış günlüklerini etkinleştirin. Flow kayıtları oluşturmak için NSG akış günlüklerini bir Azure depolama hesabına kaydedin. Anormal etkinlikleri araştırmak için gerekliyse, Azure ağ Izleyicisi paket yakalamayı etkinleştirin.

* [NSG akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

* [Ağ İzleyicisini etkinleştirme](../network-watcher/network-watcher-create.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Kılavuz**: Azure Marketi 'nden, yük İnceleme ÖZELLIKLERI ile KIMLIKLER/IP işlevlerini destekleyen bir teklif seçin. Yük incelemesini temel alan yetkisiz giriş algılama ve/veya önleme bir gereksinim değilse, tehdit zekasıyla Azure Güvenlik Duvarı kullanılabilir. Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen ve giden trafiği uyarabilir ve reddedebilir. IP adresleri ve etki alanları, Microsoft Tehdit Analizi akışından alınır.

Kötü amaçlı trafiği algılamak ve/veya reddetmek için kuruluşunuzun ağ sınırlarının her birinde tercih ettiğiniz güvenlik duvarı çözümünü dağıtın.

* [Azure Market](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

* [Azure Güvenlik duvarını dağıtma](../firewall/tutorial-firewall-deploy-portal.md)

* [Azure Güvenlik Duvarı ile uyarıları yapılandırma](../firewall/threat-intel.md)

* [Sanal bir ağda dağıtma-Azure Container Instances](./container-instances-vnet.md) 



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: güvenilen SERTIFIKALAR için HTTPS/SSL özellikli Web uygulamaları için Azure Application Gateway dağıtın.

* [Application Gateway dağıtma](../application-gateway/quick-create-portal.md)

* [Application Gateway HTTPS kullanacak şekilde yapılandırma](../application-gateway/create-ssl-portal.md) 

* [Azure Web uygulaması ağ geçitleri ile katman 7 yük dengelemesini anlama](../application-gateway/overview.md)

* [Bir kapsayıcı grubu için statik IP adresi kullanıma sunma](./container-instances-application-gateway.md)

* [Sanal bir ağda dağıtma-Azure Container Instances](./container-instances-vnet.md)



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak Için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir. 

Ayrıca, karmaşık güvenlik yapılandırmasını basitleştirmeye yardımcı olması için uygulama güvenlik gruplarını kullanabilirsiniz. Uygulama güvenlik grupları ağ güvenliğini uygulamanın yapısının doğal bir uzantısı olarak yapılandırmanıza imkan vererek sanal makineleri gruplamanızı ve ağ güvenlik ilkelerini bu gruplara göre tanımlamanızı sağlar. 

* [Hizmet etiketlerini anlama ve kullanma](../virtual-network/service-tags-overview.md) 

* [Uygulama güvenlik gruplarını anlama ve kullanma](../virtual-network/security-overview.md#application-security-groups)

* [Sanal bir ağda dağıtma-Azure Container Instances](./container-instances-vnet.md)



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: tek bir şema tanımında Azure kaynakları Yöneticisi şablonları, Azure RBAC denetimleri ve ilkeleri gibi temel ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları da kullanabilirsiniz. Şema 'i yeni aboneliklere uygulayabilir ve sürüm oluşturma aracılığıyla denetim ve yönetimi ayarlayabilirsiniz. 

* [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Ağ için Azure Ilke örnekleri](/azure/governance/policy/samples/#network)

* [Azure Blueprint oluşturma](../governance/blueprints/create-blueprint-portal.md)



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: NSG 'ler ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için "Açıklama" alanını kullanarak ağa giden/giden trafiğe izin veren kuralların iş gereksinimini ve/veya süresini (vs.) belirtin.

Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz.

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

* [Sanal bir ağda dağıtma-Azure Container Instances](./container-instances-vnet.md)

* [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmalarının yanı sıra kapsayıcı örneklarınızla ilgili ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

* [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](../azure-monitor/platform/activity-log.md#view-the-activity-log)

* [Azure Izleyici 'de uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, Azure kaynakları için zaman kaynaklarını korur, ancak işlem kaynaklarınızın zaman eşitleme ayarlarını yönetme seçeneğiniz vardır. Örneğin, çalışan bir kapsayıcıda bir zaman eşitleme komutu çalıştırın.

* [Azure işlem kaynakları için zaman eşitlemesini yapılandırma](../virtual-machines/windows/time-sync.md)

* [Çalışan bir Azure Container Instance 'da bir komut yürütün](./container-instances-exec.md)



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: bir Azure Container Group tarafından oluşturulan güvenlik verilerini toplamak Için Azure izleyici aracılığıyla günlükleri alma. Azure Izleyici 'de, Log Analytics çalışma alanı (ler) kullanarak Analizi sorgulayın ve gerçekleştirin ve uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

* [Azure Izleyici günlükleri ile kapsayıcı grubu ve örnek günlüğü](./container-instances-log-analytics.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: Azure izleyici, kayıt defterinizde Kullanıcı odaklı olaylar için kaynak günlüklerini (eski adıyla tanılama günlükleri olarak adlandırılır) toplar. Azure Container Instances, Azure Izleyici günlüklerine kapsayıcı grubu günlüklerini ve olay verilerini ve kapsayıcı günlüklerini göndermek için yerleşik destek içerir.

* [Azure Izleyici günlükleri ile kapsayıcı grubu ve örnek günlüğü](../container-registry/container-registry-diagnostics-audit-logs.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz. Bu kılavuz IaaS işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, Log Analytics çalışma alanı saklama dönemini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

* [Log Analytics çalışma alanları için günlük saklama parametrelerini ayarlama](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: anormal davranış için günlükleri çözümleyin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics çalışma alanını kullanın. 

* [Azure Izleyici günlükleri ile kapsayıcı grubu ve örnek günlüğü](./container-instances-log-analytics.md)

* [Log Analytics çalışma alanını anlayın](../azure-monitor/log-query/get-started-portal.md)

* [Azure Izleyici 'de özel sorgular gerçekleştirme](../azure-monitor/log-query/get-started-queries.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Rehberlik**: güvenlik günlükleri ve olayları 'nda bulunan anormal etkinlikleri izlemek ve uyarmak Için Log Analytics çalışma alanını kullanın. 

* [Azure Izleyici günlükleri ile kapsayıcı grubu ve örnek günlüğü](./container-instances-log-analytics.md)

* [Log Analytics günlük verilerinde uyarı alma](../azure-monitor/learn/tutorial-response.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: bir kapsayıcıda çalıştırmak gerekirse kendi kötü amaçlı yazılımdan koruma çözümünüzü ve olay koleksiyonunuzu sağlayın. 


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: BIR kapsayıcıda DNS günlüklerini sorgulamak için gerekiyorsa kendi çözümünüzü sağlayın.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: gerekirse, çalışan bir kapsayıcı örneğinde konsol günlüğünü yapılandırın.

* [Çalışan bir Azure Container Instance 'da bir komut yürütün](./container-instances-exec.md)



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure Active Directory (Azure AD), açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

Azure Container Instances ile bir Azure Container Registry kullanıyorsanız, her Azure Container Registry için yerleşik yönetici hesabının etkin veya devre dışı olduğunu izleyin. Hesabı kullanımda olmadığında devre dışı bırakın.

* [Azure AD 'de PowerShell ile dizin rolü alma](/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

* [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

* [Yönetici hesabı Azure Container Registry](../container-registry/container-registry-authentication.md#admin-account)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Azure Active Directory (Azure AD) varsayılan parola kavramına sahip değil. Parola gerektiren diğer Azure kaynakları, karmaşıklık gereksinimleriyle ve hizmete bağlı olarak, en düşük parola uzunluğuna sahip bir parola oluşturulmasını zorlar. Varsayılan parolaları kullanbilen üçüncü taraf uygulamalardan ve Market hizmetlerinden siz sorumlusunuz.

Azure Container Registry 'yi Azure Container Instances kullanıyorsanız, bir Azure Container Registry 'nin varsayılan yönetici hesabı etkinse, karmaşık parolalar otomatik olarak oluşturulur ve döndürülmelidir. Hesabı kullanımda olmadığında devre dışı bırakın.

* [Yönetici hesabı Azure Container Registry](../container-registry/container-registry-authentication.md#admin-account)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Azure Container Instances ile bir Azure Container Registry kullanıyorsanız, kapsayıcı kayıt defterinin yerleşik yönetici hesabını etkinleştirmek için yordamlar oluşturun. Hesabı kullanımda olmadığında devre dışı bırakın.

* [Azure Güvenlik Merkezi kimlik ve erişimini anlama](../security-center/security-center-identity-access.md)

* [Yönetici hesabı Azure Container Registry](../container-registry/container-registry-authentication.md#admin-account)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: mümkün olan yerlerde, tek başına bağımsız kimlik bilgilerini hizmet başına yapılandırmak yerıne Azure Active Directory SSO kullanın. Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini kullanın.

* [Azure AD ile SSO 'yu anlama](../active-directory/manage-apps/what-is-single-sign-on.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Active Directory (Azure AD) Multi-Factor Authentication 'ı (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

* [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

* [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve YAPıLANDıRMAK için MFA Ile Paws (ayrıcalıklı erişim iş istasyonları) kullanın.

* [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda Günlükler ve uyarılar oluşturmak için Azure Active Directory (Azure AD) güvenlik raporları kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın.

* [Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme](../active-directory/identity-protection/overview-identity-protection.md)

* [Azure Güvenlik Merkezi 'nde kullanıcıların kimlik ve erişim etkinliğini izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek Için adlandırılmış konumlar kullanın.

* [Azure 'da adlandırılmış konumları yapılandırma](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (Azure AD) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

* [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure Active Directory (Azure AD) eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir.

* [Azure AD raporlamayı anlama](../active-directory/reports-monitoring/index.yml)

* [Azure kimlik erişimi incelemelerini kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Rehberlik**: herhangi bir güvenlik bilgisi ve olay yönetimi (SIEM)/Monitoring aracıyla tümleştirmenize olanak tanıyan Azure Active Directory (Azure AD) oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişiminiz vardır.

Azure Active Directory Kullanıcı hesapları için Tanılama ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. İstenen uyarıları Log Analytics çalışma alanı içinde yapılandırabilirsiniz.

* [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırmak için Azure Active Directory (Azure AD) riskini ve kimlik koruması özelliklerini kullanın.

* [Azure AD riskli oturum açma işlemlerini görüntüleme](../active-directory/identity-protection/overview-identity-protection.md)

* [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: Şu anda kullanılamıyor; Müşteri Kasası Azure Container Instances için şu anda desteklenmiyor.

* [Desteklenen Müşteri Kasası hizmetleri listesi](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure Container Instances 'ı izlemeye yardımcı olması için kaynak etiketlerini kullanın. 

Hassas bilgileri depolayan veya işleyen görüntülerin izlenmesine yardımcı olmak için, etiket ve sürüm kapsayıcı görüntüleri.

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

* [Kapsayıcı görüntülerini etiketleme ve sürüm oluşturma önerileri](../container-registry/container-registry-image-tag-version.md)


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Kaynaklar VNet/subnet ile ayrılmalıdır, uygun şekilde etiketlenebilir ve bir NSG veya Azure güvenlik duvarıyla korunmuş olmalıdır. Hassas verileri depolayan veya işleyen kaynaklar yeterince yalıtılmalıdır.

* [Çalışan bir Azure Container Instance 'da bir komut yürütün](./container-instances-exec.md)

* [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

* [Yönetim grupları oluşturma](../governance/management-groups/create.md)

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

* [Sanal bir ağda dağıtma-Azure Container Instances](./container-instances-vnet.md) 
* [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

* [Azure Güvenlik duvarını dağıtma](../firewall/tutorial-firewall-deploy-portal.md)

* [Azure Güvenlik Duvarı ile uyarı veya uyarı yapılandırma ve reddetme](../firewall/threat-intel.md)


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: ağ engelleyicilerinde, hassas bilgilerin yetkisiz aktarımını izleyen ve bilgi güvenliği uzmanlarına uyarı ederken bu tür aktarımları engelleyen bir otomatik araç dağıtın. Azure dosya paylaşımlarından ve kapsayıcı örneklerine takılan diğer birimlerden yetkisiz bilgi aktarımını izleyin ve engelleyin.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

* [Sanal bir ağda dağıtma-Azure Container Instances](./container-instances-vnet.md) 

* [Azure Container Instances'ta Azure dosya paylaşımı bağlama](./container-instances-volume-azure-files.md)


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Kılavuz**: Azure Container gruplarınızı bağlayan tüm istemcilerin TLS 1,2 veya üzeri bir şekilde anlaşabilebileceğine emin olun. Microsoft Azure kaynaklar varsayılan olarak TLS 1,2 ' a anlaş.

Azure Güvenlik Merkezi önerilerini, varsa, bekleyen ve geçişte şifreleme için kullanın.

* [Azure ile iletim sırasında şifrelemeyi anlama](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: veri tanımlama, sınıflandırma ve kayıp önleme özellikleri şu anda Azure Container Instances için kullanılamaz. Bu gibi hassas bilgileri işleyen ve uyumluluk amaçları için gerekliyse üçüncü taraf çözümü uygulayan kapsayıcı gruplarını etiketleyebilir.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Rehberlik**: Azure Container Instances verilerine ve kaynaklara erişimi denetlemek için Azure rol tabanlı erişim denetimi (Azure RBAC) kullanın. 

* [Azure RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Kılavuz**: varsayılan olarak, Azure Container Instances ' deki tüm dağıtım verileri, Microsoft tarafından yönetilen anahtarlar kullanılarak geri kalanında şifrelenir. İsteğe bağlı olarak, şifrelemeyi kendi anahtarınızla yönetin (müşteri tarafından yönetilen anahtar).

* [Azure 'da bekleyen şifrelemeyi anlama](../security/fundamentals/encryption-atrest.md)

* [Dağıtım verilerini Azure Container Instances ile şifreleyin](./container-instances-encrypt-data.md)



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Rehberlik**: kapsayıcı gruplarınız ve kapsayıcı örneklerinizin değişiklik yaptığı durumlar için uyarı oluşturmak üzere Azure etkinlik günlüğü Ile Azure izleyici 'yi kullanın. 

* [Azure etkinlik günlüğü olayları için uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: özel bir kayıt defterinde kapsayıcı görüntülerini taramak ve olası güvenlik açıklarını belirlemek için çözümler uygulayın. Azure Container Registry depolanan kapsayıcı görüntülerinde güvenlik açığı değerlendirmeleri gerçekleştirerek Azure Güvenlik Merkezi önerilerini izleyin. Azure Market 'ten, görüntü güvenlik açığı değerlendirmelerini gerçekleştirmek için isteğe bağlı olarak üçüncü taraf çözümler dağıtın.

* [Azure Container Instances için güvenlik konuları](./container-instances-image-security.md)

* [Güvenlik Merkezi ile Azure Container Registry tümleştirme](../security-center/azure-container-registry-integration.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: Microsoft, çalışan kapsayıcıları destekleyen temel sistemler üzerinde düzeltme eki yönetimi gerçekleştirir.

Kapsayıcı görüntülerine yama yapmak için özel veya üçüncü taraf bir çözüm kullanın. Kapsayıcı görüntülerini Azure Container Registry depoluyoruz, temel işletim sistemi görüntülerinde güvenlik düzeltme eklerine veya diğer güncelleştirmelere dayalı olarak bir kapsayıcı kayıt defterindeki uygulama görüntülerinin güncelleştirmelerini otomatikleştirmek için Azure Container Registry görevlerini çalıştırın.

* [Azure Container Instances için güvenlik konuları](./container-instances-image-security.md)

* [Azure Container Registry görevler için temel görüntü güncelleştirmeleri hakkında](../container-registry/container-registry-tasks-base-images.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: kapsayıcı görüntülerine yama yapmak için özel veya üçüncü taraf bir çözüm kullanın. Kapsayıcı görüntülerini Azure Container Registry depoluyoruz, temel işletim sistemi görüntülerinde güvenlik düzeltme eklerine veya diğer güncelleştirmelere dayalı olarak bir kapsayıcı kayıt defterindeki uygulama görüntülerinin güncelleştirmelerini otomatikleştirmek için Azure Container Registry görevlerini çalıştırın.

* [Azure Container Instances için güvenlik konuları](./container-instances-image-security.md)

* [ACR görevleri için temel görüntü güncelleştirmeleri hakkında](../container-registry/container-registry-tasks-base-images.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: görüntü taramayı sonuçları tutarlı aralıklarla dışarı aktarın ve güvenlik açıklarının düzeltildiğini doğrulamak için sonuçları karşılaştırın. Kapsayıcı görüntülerini Azure Container Registry depolukarşılaşırsanız, güvenlik açıkları için kapsayıcı görüntülerinin düzenli olarak taranmasını sağlamak üzere kayıt defterinizi Azure Güvenlik Merkezi ile tümleştirin. Düzenli olarak görüntü güvenlik açığı taramaları gerçekleştirmek için Azure Marketi 'nden farklı üçüncü taraf çözümleri dağıtın.

* [Azure Container Instances için güvenlik konuları](./container-instances-image-security.md)

* [Güvenlik Merkezi ile Azure Container Registry tümleştirme](../security-center/azure-container-registry-integration.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: kapsayıcı görüntülerini Azure Container Registry depoluıyorsanız, güvenlik açıkları için kapsayıcı görüntülerinin düzenli olarak taranmasını sağlamak ve riskleri sınıflandırmak için kayıt defterinizi Azure Güvenlik Merkezi ile tümleştirin. İsteğe bağlı olarak Azure Marketi 'nden üçüncü taraf çözümler dağıtarak düzenli görüntü güvenlik açığı taramaları ve risk sınıflandırması gerçekleştirin.

* [Azure Container Instances için güvenlik konuları](./container-instances-image-security.md)

* [Güvenlik Merkezi ile Azure Container Registry tümleştirme](../security-center/azure-container-registry-integration.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz dahilinde (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) tüm kaynakları sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerini numaralandırın.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

* [Azure Kaynak Graf ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

* [Azure aboneliklerinizi görüntüleme](/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Azure RBAC 'yi anlama](../role-based-access-control/overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: Azure Container Instances ve ilgili kaynaklara Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: varlıkları düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

* [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

* [Yönetim Grupları oluşturma](../governance/management-groups/create.md)

* [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini koruyun

**Rehberlik**: Kurumsal gereksinimlerinize göre onaylanan Azure kaynakları envanterini oluşturmanız gerekecektir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: abonelikleriniz içinde oluşturulabilecek kaynak türlerine yönelik kısıtlamalar koymak Için Azure ilkesini kullanın.

Azure Kaynak Grafiği 'ni kullanarak aboneliklerinde kaynakları sorgulama/bulma. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

* [Azure Ilkesi kullanarak Azure Container Registry 'nin uyumluluğunu denetleme](../container-registry/container-registry-azure-policy.md)

* [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: kendi çözümünüzü veya bir üçüncü taraf çözümünü, onaylanan Kapsayıcılı uygulamalar için envanter yazılımlarına uygulayın. 

Özel bir kayıt defterinde kapsayıcı görüntülerini taramak ve olası güvenlik açıklarını belirlemek için çözümler uygulayın. Azure Container Registry depolanan kapsayıcı görüntülerinde güvenlik açığı değerlendirmeleri gerçekleştirmek için Azure Güvenlik Merkezi 'ndeki önerileri izleyin. Azure Market 'ten, görüntü güvenlik açığı değerlendirmelerini gerçekleştirmek için isteğe bağlı olarak üçüncü taraf çözümler dağıtın.

Anormal davranış için Azure Container Instances günlüklerini izleyin ve sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics çalışma alanını kullanın.

* [Azure Izleyici günlükleri ile kapsayıcı grubu ve örnek günlüğü](./container-instances-log-analytics.md)

* [Log Analytics çalışma alanını anlayın](../azure-monitor/log-query/get-started-portal.md)

* [Azure Izleyici 'de özel sorgular gerçekleştirme](../azure-monitor/log-query/get-started-queries.md)

* [Azure Container Instances için güvenlik konuları](./container-instances-image-security.md)
* [Güvenlik Merkezi ile Azure Container Registry tümleştirme](../security-center/azure-container-registry-integration.md)



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Kılavuz**: Azure Otomasyonu, dağıtım, işlemler ve iş yüklerinin ve kaynakların yetkisini alma sırasında tamamen denetim sağlar. Yetkisiz Azure kaynaklarını ve yazılım uygulamalarını kaldırmak için kendi çözümünüzü uygulayabilirsiniz.

* [Azure Otomasyonu’na giriş](../automation/automation-intro.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: onaylanan uygulamaları çalıştıran görüntülerin izlenmesine yardımcı olmak için etiket ve sürüm kapsayıcı görüntüleri.
* [Kapsayıcı görüntülerini etiketleme ve sürüm oluşturma önerileri](../container-registry/container-registry-image-tag-version.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: ortamınızda sağlayabileceğiniz hizmetleri kısıtlamak Için Azure ilkesini kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Ilkesiyle belirli bir kaynak türünü reddetme](/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: onaylanan uygulamaları çalıştıran görüntülerin izlenmesine yardımcı olmak için etiket ve sürüm kapsayıcı görüntüleri.
* [Kapsayıcı görüntülerini etiketleme ve sürüm oluşturma önerileri](../container-registry/container-registry-image-tag-version.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-divlimit-users-ability-to-interact-with-azure-resource-manager-via-scriptsbrdiv"></a>6,11: <div>Kullanıcıların betikler aracılığıyla Azure Resource Manager etkileşim kurma yeteneğini sınırlayın<br></div>

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure kaynakları yöneticisiyle etkileşime geçmesini sınırlamak Için Azure koşullu erişimi kullanın. 

* [Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: Azure Container Instances erişimi olan tüm kullanıcılar, kapsayıcılar içinde betikleri yürütebilir.

Farklı Azure abonelikleri veya yönetim grupları kullanarak Azure Container Instances kaynaklarına erişimi yönetin ve gözden geçirin veya sanal ağlar ile NSG 'leri ya da Azure Güvenlik duvarını kullanarak kaynakları yalıtın.

* [Çalışan bir Azure Container Instance 'da bir komut yürütün](./container-instances-exec.md)

* [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

* [Yönetim grupları oluşturma](../governance/management-groups/create.md)

* [Sanal bir ağda dağıtma-Azure Container Instances](./container-instances-vnet.md)

* [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

* [Azure Güvenlik duvarını dağıtma](../firewall/tutorial-firewall-deploy-portal.md)



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: iş işlemleri için gerekli olan, ancak kuruluşa daha fazla risk doğurabilecek yazılımlar, kendi sanal ağı içinde yalıtılmalıdır ve bir Azure Güvenlik Duvarı veya ağ güvenlik grubuyla yeterince güvenli hale getirilir.

* [Sanal bir ağda dağıtma-Azure Container Instances](./container-instances-vnet.md) 

* [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: Azure Resource Manager şablonu kullanarak veya BIR YAML dosyasına vererek onaylanan bir kapsayıcı grubu yapılandırmasını koruyun. Azure Ilkesini kullanarak ilgili Azure kaynakları için güvenlik yapılandırmalarının bakımını yapın.

* [Azure Container Instances’taki kapsayıcı grupları](container-instances-container-groups.md#deployment)

* [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: kapsayıcı görüntülerine yama yapmak için özel veya üçüncü taraf bir çözüm kullanın. Kapsayıcı görüntülerini Azure Container Registry depoluyoruz, temel işletim sistemi görüntülerinde güvenlik düzeltme eklerine veya diğer güncelleştirmelere dayalı olarak bir kapsayıcı kayıt defterindeki uygulama görüntülerinin güncelleştirmelerini otomatikleştirmek için Azure Container Registry görevlerini çalıştırın. 

* [Azure Container Registry görevler için temel görüntü güncelleştirmeleri hakkında](../container-registry/container-registry-tasks-base-images.md)



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

* [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: özel bir kayıt defterinde kapsayıcı görüntülerini taramak ve işletim sistemi yapılandırmalarında olası güvenlik açıklarını belirlemek için çözümler uygulayın. Azure Container Registry depolanan kapsayıcı görüntülerinde güvenlik açığı değerlendirmeleri gerçekleştirmek için Azure Güvenlik Merkezi 'ndeki önerileri izleyin. Azure Market 'ten, görüntü güvenlik açığı değerlendirmelerini gerçekleştirmek için isteğe bağlı olarak üçüncü taraf çözümler dağıtın.

Kapsayıcı görüntülerine yama yapmak için özel veya üçüncü taraf bir çözüm kullanın. Kapsayıcı görüntülerini Azure Container Registry depoluyoruz, temel işletim sistemi görüntülerinde güvenlik düzeltme eklerine veya diğer güncelleştirmelere dayalı olarak bir kapsayıcı kayıt defterindeki uygulama görüntülerinin güncelleştirmelerini otomatikleştirmek için Azure Container Registry görevlerini çalıştırın. 

* [Azure Container Instances için kapsayıcı izleme ve tarama güvenlik önerileri](./container-instances-image-security.md)

* [Güvenlik Merkezi ile Azure Container Registry tümleştirme](../security-center/azure-container-registry-integration.md)
* [Azure Container Registry görevler için temel görüntü güncelleştirmeleri hakkında](../container-registry/container-registry-tasks-base-images.md)



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Rehberlik**: ARM şablonlarını, YAML dosyalarını ve özel Azure ilke tanımlarını kaynak denetiminde güvenli bir şekilde depolayın ve yönetin.

* [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure Repos belgeleri](/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: Azure Container Registry içinde kapsayıcı görüntülerini depolayın ve yalnızca yetkili kullanıcıların görüntülere erişebildiğinden emin olmak IÇIN Azure RBAC 'den yararlanın.

* [Azure RBAC 'yi anlama](../role-based-access-control/rbac-and-directory-admin-roles.md)

* [Container Registry için Azure RBAC 'nı anlama](../container-registry/container-registry-roles.md)

* [Azure RBAC 'yi yapılandırma](../role-based-access-control/quickstart-assign-role-user-portal.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Kılavuz**: Azure ilkesini kullanarak sistem yapılandırmalarının uyarılarını, denetimini ve zorunlu tutun. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

* [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu kılavuz IaaS işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

**Rehberlik**: Azure kaynaklarınız için temel taramalar gerçekleştirmek üzere Azure Güvenlik Merkezi 'ni kullanın.

Abonelikleriniz içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure Ilkesini kullanın.

* [Azure Güvenlik Merkezi 'nde öneriler nasıl düzeltileceği](../security-center/security-center-remediate-recommendations.md)

* [Azure Ilkesi kullanarak Azure Container Registry 'nin uyumluluğunu denetleme](../container-registry/container-registry-azure-policy.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Kılavuz**: kapsayıcı görüntülerini depolamak için Azure Container Registry kullanıyorsanız, kapsayıcılar için işletim sistemi ve Docker ayarlarına yönelik temel taramalar gerçekleştirmek üzere Azure Güvenlik Merkezi 'ni kullanın.

* [Azure Güvenlik Merkezi'ndeki kapsayıcı önerilerini anlama](../security-center/container-security.md)


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvenli hale getirmek için Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın.

* [Azure yönetilen kimliklerle tümleştirme](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

* [Key Vault oluşturma](../key-vault/secrets/quick-create-portal.md)

* [Key Vault kimlik doğrulaması yapma](../key-vault/general/authentication.md)

* [Key Vault erişim ilkesi atama](../key-vault/general/assign-access-policy-portal.md)

* [Azure Container Instances ile yönetilen kimlikleri kullanma](./container-instances-managed-identity.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Kılavuz**: Azure AD 'de otomatik olarak yönetilen bir kimlik ile Azure hizmetleri sağlamak Için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

* [Yönetilen kimlikleri yapılandırma](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

* [Azure Container Instances ile yönetilen kimlikleri kullanma](./container-instances-managed-identity.md)



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

* [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu kılavuz IaaS işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel alınan konakta (örneğin, Azure Container Instances) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.

App Service, Data Lake Storage, BLOB depolama vb. gibi işlem dışı Azure kaynaklarına yüklenen tüm dosyaları önceden tarayın. 


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri IaaS işlem kaynaklarına yöneliktir.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure Container Instances) etkinleştirilir, ancak müşteri içeriklerde çalıştırılmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: Azure Backup etkinleştirin ve yedekleme kaynağını (kapsayıcı gruplarına bağlanmış bir dosya paylaşımının yanı sıra istenen sıklık ve Bekletme dönemi) yapılandırın. 

* [Azure Backup etkinleştirme](../backup/index.yml)

* [Azure Container Instances'ta Azure dosya paylaşımı bağlama](./container-instances-volume-azure-files.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Kılavuz**: Azure komut satırı araçlarını veya SDK 'larını kullanarak Azure Key Vault müşteri tarafından yönetilen anahtarları yedekleyin.

İsteğe bağlı olarak, bir kayıt defterinden diğerine aktararak kapsayıcı görüntülerini yedekleyin.
* [Azure 'da Anahtar Kasası anahtarlarını yedekleme](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

* [Kapsayıcı görüntülerini kapsayıcı kayıt defterine aktarma](../container-registry/container-registry-import-images.md)



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Kılavuz**: Azure komut satırı araçlarını veya SDK 'larını kullanarak Azure Key Vault, yedeklenen müşteri tarafından yönetilen anahtarların sınamasını geri yükleme.

* [Azure 'da Azure Key Vault anahtarlarını geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

* [Dağıtım verilerini şifreleme-Azure Container Instances](./container-instances-encrypt-data.md)



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Azure Key Vault ' de geçici silme özelliğini etkinleştirebilirsiniz.

* [Key Vault 'da geçici silme özelliğini etkinleştirme](../storage/blobs/soft-delete-overview.md?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

Müşteri, kendi olay yanıtı planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu da kullanabilir.

* [Azure Güvenlik Merkezi 'nde Iş akışı otomasyonlarını yapılandırma](../security-center/security-center-planning-and-operations-guide.md)

* [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [NIST Bilgisayar Güvenlik Olayı İşleme Kılavuzu](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Kılavuz**: Azure Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örneğin,. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

* [NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

* [Azure Güvenlik Merkezi güvenlik ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarılar Sentinel 'i akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

* [Sürekli dışarı aktarmayı yapılandırma](../security-center/continuous-export.md)

* [Uyarıları Azure Sentinel 'e akış](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

* [Iş akışı otomasyonu ve Logic Apps yapılandırma](../security-center/workflow-automation.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve 60 gün içinde tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin. Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın.

* [Katılım test kurallarını oyma](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Microsoft Bulut Kırmızı ekip oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin
