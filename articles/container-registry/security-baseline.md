---
title: Azure Container Registry için Azure Güvenlik temeli
description: Azure Container Registry için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 95864f932fe255d561eaeb2d803b5fcc79cb2802
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82184104"
---
# <a name="azure-security-baseline-for-azure-container-registry"></a>Azure Container Registry için Azure Güvenlik temeli

Azure Container Registry için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: Azure sanal ağı, Azure ve şirket içi kaynaklarınız için güvenli, özel ağ sağlar. Azure sanal ağından özel Azure Container Registry 'nize erişimi sınırlayarak, yalnızca sanal ağdaki kaynakların kayıt defterine erişebildiğinden emin olursunuz. Şirketler arası senaryolar için, yalnızca belirli IP adreslerinden kayıt defteri erişimine izin vermek üzere güvenlik duvarı kurallarını da yapılandırabilirsiniz. Bir güvenlik duvarının arkasında, kapsayıcı Kayıt defterinize erişmek için güvenlik duvarı erişim kuralları ve hizmet etiketleri yapılandırın.

Azure sanal ağını veya güvenlik duvarı kurallarını kullanarak bir Azure Container Registry 'ye erişimi kısıtlayın:https://docs.microsoft.com/azure/container-registry/container-registry-vnet 

Güvenlik duvarının arkasındaki bir Azure Container Registry 'ye erişmek için kuralları yapılandırın:https://docs.microsoft.com/azure/container-registry/container-registry-firewall-access-rules


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Rehberlik**: Azure Güvenlik Merkezi 'ni kullanarak ağ kaynaklarınızı Azure 'da korumanıza yardımcı olması için ağ koruma önerilerini düzeltin. Trafik denetimi için NSG akış günlüklerini etkinleştirin ve günlükleri bir depolama hesabına gönderin.

NSG akış günlüklerini etkinleştirme:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Ağ kaynaklarınızı koruyun:https://docs.microsoft.com/azure/security-center/security-center-network-recommendations



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz. Kıyaslama, Web uygulamalarını barındıran Azure App Service veya işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: DDoS saldırılarına karşı korumalar için sanal ağlarınızda DDoS standart korumasını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.  Tehdit zekası etkinleştirilmiş ve kötü amaçlı ağ trafiği için "uyarı ve reddetme" olarak yapılandırılmış bir kuruluşun ağ sınırlarının her birinde Azure Güvenlik Duvarı 'Nı dağıtın.

NSG 'leri, sınırlı bir süre için son bitiş noktaları onaylı IP adresleriyle sınırlamak üzere Azure Güvenlik Merkezi 'ni tam zamanında ağ erişimi ile kullanabilirsiniz. Ayrıca, bağlantı noktalarını ve kaynak IP 'Leri gerçek trafik ve tehdit zekası temelinde sınırlayan NSG yapılandırmalarının kullanılması için Azure Güvenlik Merkezi Uyarlamalı ağ sağlamlaştırma kullanın.

DDoS korumasını yapılandırma:https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Azure Güvenlik duvarını dağıtma:https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama:https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

Azure Güvenlik Merkezi Uyarlamalı ağ sağlamlaştırma 'yi anlama:https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening

Azure Güvenlik Merkezi tam zamanında ağ Access Control:https://docs.microsoft.com/azure/security-center/security-center-just-in-time


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Kılavuz**: Azure Container Registry 'nizi korumak için kullanılan alt ağa bağlı NSG için ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin. Flow kayıtları oluşturmak için NSG akış günlüklerini bir Azure depolama hesabına kaydedebilirsiniz. Anormal etkinlikleri araştırmak için gerekliyse, Azure ağ Izleyicisi paket yakalamayı etkinleştirin.

NSG akış günlüklerini etkinleştirme:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Ağ Izleyicisi 'ni etkinleştirme:https://docs.microsoft.com/azure/network-watcher/network-watcher-create


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Kılavuz**: Azure Marketi 'nden, yük İnceleme ÖZELLIKLERI ile KIMLIKLER/IP işlevlerini destekleyen bir teklif seçin. Yük incelemesini temel alan yetkisiz giriş algılama ve/veya önleme bir gereksinim değilse, tehdit zekasıyla Azure Güvenlik Duvarı kullanılabilir. Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen ve giden trafiği uyarabilir ve reddedebilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklıdır.

Kötü amaçlı trafiği algılamak ve/veya reddetmek için kuruluşunuzun ağ sınırlarının her birinde tercih ettiğiniz güvenlik duvarı çözümünü dağıtın.

Azure Marketi:https://azuremarketplace.microsoft.com/marketplace/?term=Firewall 

Azure Güvenlik duvarını dağıtma:https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Azure Güvenlik Duvarı ile uyarıları yapılandırma:https://docs.microsoft.com/azure/firewall/threat-intel


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz. Kıyaslama, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Kılavuz**: kapsayıcı Kayıt defterinize erişmesi gereken kaynaklar Için, ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak üzere Azure Container Registry hizmeti için sanal ağ hizmeti etiketleri kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. "AzureContainerRegistry" hizmet etiketi adını bir kuralın uygun kaynak veya hedef alanında belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

Hizmet etiketine göre erişime izin ver:https://docs.microsoft.com/azure/container-registry/container-registry-firewall-access-rules#allow-access-by-service-tag


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure Ilkesi ile Azure Container Registry 'larınızla ilişkili ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Kapsayıcı kayıt defterlerinizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. ContainerRegistry" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. 

Tek bir şema tanımında Azure Resource Manager şablonları, RBAC denetimleri ve ilkeleri gibi anahtar ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Şema 'i yeni aboneliklere kolayca uygulayın ve sürüm oluşturma aracılığıyla denetim ve yönetime ince ayar yapın.

Azure Ilkesi kullanarak Azure Container Registry 'nin uyumluluğunu denetleme:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy

Azure Blueprint oluşturma:https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: müşteri, tek bir şema tanımında Azure Resource Manager şablonları, RBAC denetimleri ve ilkeleri gibi temel ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları kullanabilir. Şema 'i yeni aboneliklere kolayca uygulayın ve sürüm oluşturma aracılığıyla denetim ve yönetime ince ayar yapın.

Azure Blueprint oluşturma:https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını izleyin ve kapsayıcı kayıt defterlerinden ilgili ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

Azure etkinlik günlüğü olaylarını görüntüleme ve alma:https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Izleyici 'de uyarı oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, Azure kaynakları için zaman kaynaklarını korur, ancak işlem kaynaklarınızın zaman eşitleme ayarlarını yönetme seçeneğiniz vardır.

Azure işlem kaynakları için Saat eşitlemesini yapılandırma:https://docs.microsoft.com/azure/virtual-machines/windows/time-sync


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: bir Azure Container Registry tarafından oluşturulan güvenlik verilerini toplamak Için Azure izleyici aracılığıyla günlükleri alma. Azure Izleyici 'de, Log Analytics çalışma alanı (ler) kullanarak Analizi sorgulayın ve gerçekleştirin ve uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

Tanılama değerlendirmesi ve denetimi için Azure Container Registry günlükleri:https://docs.microsoft.com/azure/container-registry/container-registry-diagnostics-audit-logs



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: Azure izleyici, kayıt defterinizde Kullanıcı odaklı olaylar için kaynak günlüklerini (eski adıyla tanılama günlükleri olarak adlandırılır) toplar. Kayıt defteri kimlik doğrulama olaylarını denetlemek için bu verileri toplayın ve kullanın ve kayıt defterinizdeki güvenlik sorunlarını tanılamanıza olanak tanımak için çekme ve anında iletme olayları gibi kayıt defteri yapıtlarına yönelik tüm etkinlik izini sağlayın.

Tanılama değerlendirmesi ve denetimi için Azure Container Registry günlükleri:https://docs.microsoft.com/azure/container-registry/container-registry-diagnostics-audit-logs


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz. Kıyaslama işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, Log Analytics çalışma alanı saklama dönemini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

Log Analytics çalışma alanları için günlük bekletme parametreleri ayarlama:https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: anormal davranış için Azure Container Registry günlüklerini çözümleyin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics çalışma alanını kullanın.

Tanılama değerlendirmesi ve denetimi için Azure Container Registry günlükleri:https://docs.microsoft.com/azure/container-registry/container-registry-diagnostics-audit-logs

Log Analytics çalışma alanını anlayın:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Izleyici 'de özel sorgular gerçekleştirme:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Kılavuz**: Azure Log Analytics çalışma alanını, Güvenlik günlüklerine ve Azure Container Registry 'niz ile ilgili olaylarda bulunan anormal etkinlikleri izlemek ve uyarmak için kullanın.

Tanılama değerlendirmesi ve denetimi için Azure Container Registry günlükleri:https://docs.microsoft.com/azure/container-registry/container-registry-diagnostics-audit-logs

Log Analytics günlük verilerinde uyarı alma:https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz. Azure Container Registry kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz. Azure Container Registry bir uç noktasıdır ve iletişim başlatmaz ve hizmet DNS 'yi sorgulamaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz. Kıyaslama işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure Active Directory (Azure AD), açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

Her Azure Container Registry için yerleşik yönetici hesabının etkin veya devre dışı olduğunu izleyin. Hesabı kullanımda olmadığında devre dışı bırakın.

Azure AD 'de PowerShell ile dizin rolü alma:https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma:https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

Azure Container Registry yönetici hesabı:https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Azure Active Directory (Azure AD) varsayılan parola kavramına sahip değil. Parola gerektiren diğer Azure kaynakları, karmaşıklık gereksinimleriyle ve hizmete bağlı olarak, en düşük parola uzunluğuna sahip bir parola oluşturulmasını zorlar. Varsayılan parolaları kullanbilen üçüncü taraf uygulamalardan ve Market hizmetlerinden siz sorumlusunuz.

Bir Azure Container Registry 'nin varsayılan yönetici hesabı etkinse, karmaşık parolalar otomatik olarak oluşturulur ve döndürülmelidir. Hesabı kullanımda olmadığında devre dışı bırakın.

Azure Container Registry yönetici hesabı:https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Ayrıca, kapsayıcı kayıt defterinin yerleşik yönetici hesabını etkinleştirmek için yordamlar oluşturun. Hesabı kullanımda olmadığında devre dışı bırakın.

Azure Güvenlik Merkezi kimliğini ve erişimini anlayın:https://docs.microsoft.com/azure/security-center/security-center-identity-access

Azure Container Registry yönetici hesabı:https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: mümkün olan yerlerde, tek başına bağımsız kimlik bilgilerini hizmet başına yapılandırmak yerıne Azure Active Directory SSO kullanın. Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini kullanın.

Kapsayıcı kayıt defterine bireysel erişim için Azure Active Directory ile tümleştirilmiş bireysel oturum açma kullanın.

Azure AD ile SSO 'yu anlayın:https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on

Bir kapsayıcı kayıt defterinde tek tek oturum açma:https://docs.microsoft.com/azure/container-registry/container-registry-authentication#individual-login-with-azure-ad


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Active Directory (Azure AD) Multi-Factor Authentication 'ı (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

Azure 'da MFA 'yı etkinleştirme:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme:https://docs.microsoft.com/azure/security-center/security-center-identity-access


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve YAPıLANDıRMAK için MFA Ile Paws (ayrıcalıklı erişim iş istasyonları) kullanın.

Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin:https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure 'da MFA 'yı etkinleştirme:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted


**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda Günlükler ve uyarılar oluşturmak için Azure Active Directory (Azure AD) güvenlik raporları kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın.

Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk

Azure Güvenlik Merkezi 'nde kullanıcıların kimlik ve erişim etkinliğini izleme:https://docs.microsoft.com/azure/security-center/security-center-identity-access


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek Için adlandırılmış konumlar kullanın.

Azure 'da adlandırılmış konumlar nasıl yapılandırılır:https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (Azure AD) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

Azure AD örneği oluşturma ve yapılandırma:https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure Active Directory (Azure AD) eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir.

Azure AD raporlamayı anlayın:https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Azure kimlik erişimi incelemelerini kullanma:https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Rehberlik**: herhangi bir güvenlik bilgisi ve olay yönetimi (SIEM)/Monitoring aracıyla tümleştirmenize olanak tanıyan Azure Active Directory (Azure AD) oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişiminiz vardır.

Azure Active Directory Kullanıcı hesapları için Tanılama ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. İstenen uyarıları Log Analytics çalışma alanı içinde yapılandırabilirsiniz.

Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme:https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırmak için Azure Active Directory (Azure AD) riskini ve kimlik koruması özelliklerini kullanın. 

Azure AD riskli oturum açma işlemlerini görüntüleme:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme:https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: kullanılamıyor; Müşteri Kasası Azure Container Registry için şu anda desteklenmiyor.

Desteklenen Müşteri Kasası hizmetleri listesi:https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure Container Registry 'leri izlemeye yardımcı olması için kaynak etiketlerini kullanın.

Gizli bilgileri depolayan veya işleyen görüntüleri izlemeye yardımcı olmak için bir kayıt defterindeki bir kayıt defteri ve sürüm kapsayıcısı görüntülerini veya diğer yapıtları ve görüntüleri veya depoları kilitleyin.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Kapsayıcı görüntülerini etiketleme ve sürüm oluşturma önerileri:https://docs.microsoft.com/azure/container-registry/container-registry-image-tag-version

Azure Container Registry 'de kapsayıcı görüntüsünü kilitleme:https://docs.microsoft.com/azure/container-registry/container-registry-image-lock



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı kapsayıcı kayıt defterleri, abonelikler ve/veya yönetim grupları uygulayın. Hassas verileri depolayan veya işleyen kaynaklar yeterince yalıtılmalıdır.

Kaynaklar, sanal ağ veya alt ağ ile ayrılmalıdır, uygun şekilde etiketlenebilir ve bir ağ güvenlik grubu (NSG) veya Azure güvenlik duvarıyla korunmuş olmalıdır.

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim grupları oluşturma:https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Azure sanal ağını veya güvenlik duvarı kurallarını kullanarak bir Azure Container Registry 'ye erişimi kısıtlayın:https://docs.microsoft.com/azure/container-registry/container-registry-vnet

Güvenlik Yapılandırması ile NSG oluşturma:https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

Azure Güvenlik duvarını dağıtma:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Uyarı veya uyarı yapılandırma ve Azure Güvenlik Duvarı ile reddetme:

https://docs.microsoft.com/azure/firewall/threat-intel



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: ağ engelleyicilerinde, hassas bilgilerin yetkisiz aktarımını izleyen ve bilgi güvenliği uzmanlarına uyarı ederken bu tür aktarımları engelleyen bir otomatik araç dağıtın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

Azure 'da müşteri veri korumasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: Azure Container Registry bağlanan istemcilerin TLS 1,2 veya daha büyük bir bağlantı kurabildiğinden emin olun. Microsoft Azure kaynaklar varsayılan olarak TLS 1,2 ' a anlaş.

Azure Güvenlik Merkezi önerilerini, varsa, bekleyen ve geçişte şifreleme için kullanın.

Azure ile iletim sırasında şifrelemeyi anlayın:https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz Azure Container Registry için kullanılabilir değil. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

Azure 'da müşteri veri korumasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Rehberlik**: bir Azure Container Registry 'deki veri ve kaynaklara erişimi denetlemek için Azure Active Directory (Azure AD) RBAC kullanın. 

Azure 'da RBAC 'yi yapılandırma:https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

Azure Container Registry roller ve izinler:https://docs.microsoft.com/azure/container-registry/container-registry-roles



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: işlem kaynaklarında uyumluluk için gerekliyse, verileri bir sistemden kopyalandıklarında bile verilere erişim denetimlerine zorlamak için otomatik ana bilgisayar tabanlı veri kaybı önleme çözümü gibi bir üçüncü taraf aracı uygulayın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

Azure 'da müşteri veri korumasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: tüm Azure kaynaklarında bekleyen şifreleme kullanın. Varsayılan olarak, bir Azure Container Registry 'deki tüm veriler, Microsoft tarafından yönetilen anahtarlar kullanılarak Rest durumunda şifrelenir.

Azure 'da bekleyen şifrelemeyi anlayın:https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest

Azure Container Registry 'de müşteri tarafından yönetilen anahtarlar:https://aka.ms/acr/cmk



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure izleyici, kayıt defterinizde Kullanıcı odaklı olaylar için kaynak günlüklerini (eski adıyla tanılama günlükleri olarak adlandırılır) toplar. Kayıt defteri kimlik doğrulama olaylarını denetlemek için bu verileri toplayın ve kullanın ve kayıt defterinizde işlem sorunlarını tanılamanıza olanak tanımak için çekme ve çekme olayları gibi kayıt defteri yapıtlarına yönelik kapsamlı bir etkinlik izi sağlayın.

Tanılama değerlendirmesi ve denetimi için Azure Container Registry günlükleri:https://docs.microsoft.com/azure/container-registry/container-registry-diagnostics-audit-logs


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: kapsayıcı görüntülerinde güvenlik açığı değerlendirmeleri gerçekleştirerek Azure Güvenlik Merkezi önerilerini izleyin. Azure Market 'ten, görüntü güvenlik açığı değerlendirmelerini gerçekleştirmek için isteğe bağlı olarak üçüncü taraf çözümler dağıtın.

Azure Güvenlik Merkezi güvenlik açığı değerlendirmesi önerilerini uygulama:https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations

Güvenlik Merkezi ile tümleştirme Azure Container Registry (Önizleme):https://docs.microsoft.com/azure/security-center/azure-container-registry-integration



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: Microsoft, Azure Container Registry destekleyen temel sistemlerde Düzeltme Eki Yönetimi gerçekleştirir.

İşletim sisteminden ve diğer düzeltme eklerinden temel görüntülere yönelik güncelleştirmeler algılandığında kapsayıcı görüntüsü güncelleştirmelerini otomatikleştirin.

Azure Container Registry görevler için temel görüntü güncelleştirmeleri hakkında:https://docs.microsoft.com/azure/container-registry/container-registry-tasks-base-images


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulama görüntülerini yama yapmak için üçüncü taraf çözümünü kullanabilirsiniz.  Ayrıca, temel görüntülerdeki güvenlik düzeltme eklerini veya diğer güncelleştirmelere dayalı olarak bir kapsayıcı kayıt defterindeki uygulama görüntülerinin güncelleştirmelerini otomatikleştirmek için Azure Container Registry görevleri çalıştırabilirsiniz.

ACR görevleri için temel görüntü güncelleştirmeleri hakkında:https://docs.microsoft.com/azure/container-registry/container-registry-tasks-base-images



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: güvenlik açıkları için kapsayıcı görüntülerinin düzenli olarak taranmasını sağlamak üzere Azure Güvenlik merkezi ile Azure Container Registry (ACR) tümleştirin. Düzenli olarak görüntü güvenlik açığı taramaları gerçekleştirmek için Azure Marketi 'nden farklı üçüncü taraf çözümleri dağıtın.

Güvenlik Merkezi ile tümleştirme Azure Container Registry (Önizleme):https://docs.microsoft.com/azure/security-center/azure-container-registry-integration


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: güvenlik açıkları için kapsayıcı görüntülerinin düzenli olarak taranmasını ve riskleri sınıflandırmaya olanak tanımak Için Azure Güvenlik merkezi ile Azure Container Registry (ACR) tümleştirin. İsteğe bağlı olarak Azure Marketi 'nden üçüncü taraf çözümler dağıtarak düzenli görüntü güvenlik açığı taramaları ve risk sınıflandırması gerçekleştirin.

Güvenlik Merkezi ile tümleştirme Azure Container Registry (Önizleme):https://docs.microsoft.com/azure/security-center/azure-container-registry-integration



**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz dahilinde (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) tüm kaynakları sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın.  Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerini numaralandırın.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

Azure Kaynak Graf ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure aboneliklerinizi görüntüleme:https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0 

Azure RBAC 'yi anlama:https://docs.microsoft.com/azure/role-based-access-control/overview



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: Azure Container Registry bir kayıt defterindeki görüntüler için Etiketler ve bildirimler dahil meta verileri korur. Yapıtları etiketlemek için önerilen uygulamaları izleyin.

Kayıt defterleri, depolar ve görüntüler hakkında:https://docs.microsoft.com/azure/container-registry/container-registry-concepts

Kapsayıcı görüntülerini etiketleme ve sürüm oluşturma önerileri:https://docs.microsoft.com/azure/container-registry/container-registry-image-tag-version


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure Container Registry bir kayıt defterindeki görüntüler için Etiketler ve bildirimler dahil meta verileri korur. Yapıtları etiketlemek için önerilen uygulamaları izleyin.

Kayıt defterleri, depolar ve görüntüler hakkında:https://docs.microsoft.com/azure/container-registry/container-registry-concepts

Kapsayıcı görüntülerini etiketleme ve sürüm oluşturma önerileri:https://docs.microsoft.com/azure/container-registry/container-registry-image-tag-version



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini koruyun

**Rehberlik**: Kurumsal gereksinimlerinize göre onaylanan Azure kaynakları envanterini oluşturmanız gerekecektir.  

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: abonelikleriniz içinde oluşturulabilecek kaynak türlerine yönelik kısıtlamalar koymak Için Azure ilkesini kullanın.

Azure Kaynak Grafiği 'ni kullanarak aboneliklerinde kaynakları sorgulama/bulma.  Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

Azure Ilkesi kullanarak Azure Container Registry 'nin uyumluluğunu denetleme:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: anormal davranış için Azure Container Registry günlüklerini çözümleyin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics çalışma alanını kullanın.

Tanılama değerlendirmesi ve denetimi için Azure Container Registry günlükleri:https://docs.microsoft.com/azure/container-registry/container-registry-diagnostics-audit-logs

Log Analytics çalışma alanını anlayın:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Izleyici 'de özel sorgular gerçekleştirme:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Kılavuz**: Azure Otomasyonu, dağıtım, işlemler ve iş yüklerinin ve kaynakların yetkisini alma sırasında tamamen denetim sağlar.  Yetkisiz Azure kaynaklarını kaldırmak için kendi çözümünüzü uygulayabilirsiniz. Azure Otomasyonu 'na giriş:https://docs.microsoft.com/azure/automation/automation-intro


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz. Kıyaslama işlem kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: ortamınızda sağlayabileceğiniz hizmetleri kısıtlamak Için Azure ilkesinden yararlanın.

Azure Ilkesi kullanarak Azure Container Registry 'nin uyumluluğunu denetleme:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilkesiyle belirli bir kaynak türünü reddetme:https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz. Kıyaslama işlem kaynakları için tasarlanmıştır.



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="611-limit-users-ability-to-interact-with-azureresources-manager-via-scripts"></a>6,11: kullanıcıların betikler aracılığıyla AzureResources Manager ile etkileşim kurma yeteneğini sınırlayın

**Rehberlik**: kullanıcıların Azure işlem kaynakları içinde betikleri yürütme yeteneğini sınırlamak için işletim sistemine özgü yapılandırma veya üçüncü taraf kaynakları kullanın.

Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma:https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: kullanıcıların Azure işlem kaynakları içinde betikleri yürütme yeteneğini sınırlamak için işletim sistemine özgü yapılandırma veya üçüncü taraf kaynakları kullanın.

Örneğin, Windows ortamlarında PowerShell betiği yürütmeyi denetleme:https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: iş işlemleri için gerekli olan, ancak kuruluşa daha fazla risk doğurabilecek yazılımlar, kendi sanal makinesi ve/veya sanal ağı içinde yalıtılmalıdır ve bir Azure Güvenlik Duvarı veya ağ güvenlik grubuyla yeterince güvenli hale getirilir.

Sanal ağ oluşturma:https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Güvenlik Yapılandırması ile NSG oluşturma:https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: Azure Ilkesi veya Azure Güvenlik Merkezi 'ni kullanarak tüm Azure kaynakları için güvenlik yapılandırmalarının bakımını yapın.

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilkesi kullanarak Azure Container Registry 'nin uyumluluğunu denetleme:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: tüm bilgi işlem kaynaklarında güvenlik yapılandırmalarının bakımını yapmak Için Azure Güvenlik Merkezi önerisini "sanal makinelerinizdeki güvenlik yapılandırmalarında güvenlik açıklarını düzeltme" önerilerini kullanın.

Azure Güvenlik Merkezi önerilerini izleme:https://docs.microsoft.com/azure/security-center/security-center-recommendations

Azure Güvenlik Merkezi önerilerini Düzeltme:https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

Azure Ilkesi kullanarak Azure Container Registry 'nin uyumluluğunu denetleme:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilke efektlerini anlayın:https://docs.microsoft.com/azure/governance/policy/concepts/effects



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz. Kıyaslama işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın.

Azure DevOps 'da kod depolama:https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Repos belgeleri:https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz. Kıyaslama işlem kaynakları için geçerlidir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Kılavuz**: Azure ilkesini kullanarak sistem yapılandırmalarının uyarılarını, denetimini ve zorunlu tutun. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

Azure Ilkesi kullanarak Azure Container Registry 'nin uyumluluğunu denetleme:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz. Kıyaslama işlem kaynakları için geçerlidir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

**Rehberlik**: Azure kaynaklarınız için temel taramalar gerçekleştirmek üzere Azure Güvenlik Merkezi 'ni kullanın.

Abonelikleriniz içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure Ilkesini kullanın.

Azure Güvenlik Merkezi 'nde öneriler nasıl düzeltileceği:https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations

Azure Ilkesi kullanarak Azure Container Registry 'nin uyumluluğunu denetleme:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz. Kıyaslama işlem kaynakları için geçerlidir.


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvenli hale getirmek için Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın.

Azure yönetilen kimliklerle tümleştirme:https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Key Vault oluşturma:https://docs.microsoft.com/azure/key-vault/quick-create-portal

Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama:https://docs.microsoft.com/azure/key-vault/managed-identity

Azure Container Registry görevlerinde Azure tarafından yönetilen bir kimlik kullanın:https://docs.microsoft.com/azure/container-registry/container-registry-tasks-authentication-managed-identity


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Kılavuz**: Azure AD 'de otomatik olarak yönetilen bir kimlik ile Azure hizmetleri sağlamak Için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

Yönetilen kimlikleri yapılandırma:https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

Azure Container Registry 'de kimlik doğrulamak için yönetilen bir kimlik kullanın:https://docs.microsoft.com/azure/container-registry/container-registry-authentication-managed-identity


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

Kimlik bilgisi tarayıcısını ayarlama:https://secdevtools.azurewebsites.net/helpcredscan.html


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: kaynaklarınızı sürekli olarak izlemek ve savunmak için Azure Cloud Services ve sanal makineler Için Microsoft kötü amaçlı yazılımdan koruma kullanın. Linux için üçüncü taraf kötü amaçlı yazılımdan koruma çözümünü kullanın.

Cloud Services ve sanal makineler için Microsoft Antimalware 'i yapılandırma:https://docs.microsoft.com/azure/security/fundamentals/antimalware


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel alınan konakta (örneğin, Azure Container Registry) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.

App Service, Data Lake Storage, BLOB depolama vb. gibi işlem dışı Azure kaynaklarına yüklenen tüm dosyaları önceden tarayın.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz. Kıyaslama işlem kaynaklarına yöneliktir. Microsoft, temel alınan platform için kötü amaçlı yazılımdan koruma uygular.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: Microsoft Azure Container kayıt defterinizde bulunan veriler, dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman otomatik olarak çoğaltılır. Azure Container Registry, planlı ve plansız olaylardan korunabilmesi için verilerinizi kopyalar

İsteğe bağlı olarak, birden çok Azure bölgesindeki kayıt defteri çoğaltmalarını sürdürmek için bir kapsayıcı kayıt defterini çoğaltın. 

Azure Container Registry coğrafi çoğaltma:https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: isteğe bağlı olarak, bir kayıt defterinden diğerine aktararak kapsayıcı görüntülerini yedekleyin.

Azure komut satırı araçlarını veya SDK 'larını kullanarak Azure Key Vault müşteri tarafından yönetilen anahtarları yedekleyin.

Kapsayıcı görüntülerini bir kapsayıcı kayıt defterine içeri aktarma:https://docs.microsoft.com/azure/container-registry/container-registry-import-images

Azure 'da Anahtar Kasası anahtarları nasıl yedekleirsiniz:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Kılavuz**: Azure komut satırı araçlarını veya SDK 'larını kullanarak Azure Key Vault, yedeklenen müşteri tarafından yönetilen anahtarların sınamasını geri yükleme.

Azure 'da Azure Key Vault anahtarlarını geri yükleme:https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Azure Key Vault ' de geçici silme özelliğini etkinleştirebilirsiniz.

Key Vault 'da geçici silmeyi etkinleştirme:https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

Azure Güvenlik Merkezi 'nde Iş akışı Otomatiklamalar nasıl yapılandırılır:https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu:https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın Anatomisi:https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu de kullanabilir:https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Kılavuz**: Azure Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin:https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.  Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

Azure Güvenlik Merkezi güvenlik ilgili kişisini ayarlama:https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details


**Azure Güvenlik Merkezi izleme**: uygulanamaz

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

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve 60 gün içinde tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin:https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz:https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
