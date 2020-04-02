---
title: Azure Kapsayıcı Kayıt Defteri için Azure Güvenlik Taban Çizgisi
description: Azure Kapsayıcı Kayıt Defteri için Azure Güvenlik Taban Çizgisi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 9225cfd9793a84f371387d6450a3dfa80ba74de3
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80547533"
---
# <a name="azure-security-baseline-for-azure-container-registry"></a>Azure Kapsayıcı Kayıt Defteri için Azure Güvenlik Taban Çizgisi

Azure Kapsayıcı Kayıt Defteri için Azure Güvenlik Taban Çizgisi, dağıtımınızın güvenlik duruşunu geliştirmenize yardımcı olacak öneriler içerir.

Bu hizmetin temeli, en iyi uygulamalar kılavuzumuzla Azure'da bulut çözümlerinizi nasıl güvenebileceğinize ilişkin öneriler sunan [Azure Güvenlik Kıyaslama sürümü 1.0'dan](https://docs.microsoft.com/azure/security/benchmarks/overview)alınmıştır.

Daha fazla bilgi için [Azure Güvenlik Taban Çizgilerini genel olarak](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)görün.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için [Güvenlik Denetimi: Ağ Güvenliği'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)bakın.*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Sanal Ağınızdaki Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'nı kullanarak kaynakları koruyun

**Yönlendirme**: Azure Sanal Ağı, Azure ve şirket içi kaynaklarınız için güvenli ve özel ağ sağlar. Azure sanal ağındaki özel Azure kapsayıcı kayıt defterinize erişimi sınırlandırarak, yalnızca sanal ağdaki kaynakların kayıt defterine erişmesini sağlarsınız. Binalar arası senaryolar için, yalnızca belirli IP adreslerinden kayıt defteri erişimine izin verecek şekilde güvenlik duvarı kurallarını da yapılandırabilirsiniz. Güvenlik duvarının arkasından, konteyner kayıt defterinize erişmek için güvenlik duvarı erişim kurallarını ve hizmet etiketlerini yapılandırın.

Azure sanal ağı veya güvenlik duvarı kurallarını kullanarak Bir Azure kapsayıcı kayıt defterine erişimi kısıtlayın:https://docs.microsoft.com/azure/container-registry/container-registry-vnet 

Güvenlik duvarının arkasındaki Azure kapsayıcı kayıt defterine erişmek için kuralları yapılandırın:https://docs.microsoft.com/azure/container-registry/container-registry-firewall-access-rules


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Vnet, Subnet ve NIC'lerin yapılandırmasını ve trafiğini izleyin ve kaydedin

**Kılavuz**: Azure'daki ağ kaynaklarınızı korumaya yardımcı olmak için Azure Güvenlik Merkezi'ni kullanın ve ağ koruma önerilerini düzeltin. NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Depolama Hesabına gönderin.

NSG Akış Günlükleri nasıl etkinleştirilir:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Ağ kaynaklarınızı koruyun:https://docs.microsoft.com/azure/security-center/security-center-network-recommendations



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1.3: Kritik web uygulamalarını koruyun

**Rehberlik**: Geçerli değildir. Kıyaslama, Azure Uygulama Hizmeti veya web uygulamalarını barındıran bilgi işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Bilinen kötü amaçlı IP adresleriyle iletişimi reddetme

**Kılavuz**: DDoS saldırılarına karşı koruma sağlamak için sanal ağlarınızda DDoS Standart korumasını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimi reddetmek için Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı kullanın.  Tehdit İstihbaratı etkin leştirilmiş ve kötü amaçlı ağ trafiği için "Uyarı ve reddet" olarak yapılandırılan kuruluşun ağ sınırlarının her birinde Azure Güvenlik Duvarı'nı dağıtın.

Son noktaların onaylanan IP adreslerine sınırlı bir süre için maruz kalmasını sınırlamak için NSG'leri yapılandırmak için Azure Güvenlik Merkezi Just In Time Network erişimini kullanabilirsiniz. Ayrıca, gerçek trafik ve tehdit istihbaratına dayalı Bağlantı Noktaları ve Kaynak IP'leri sınırlayan NSG yapılandırmalarını önermek için Azure Güvenlik Merkezi Uyarlamalı Ağ Sertleştirme'yi kullanın.

DDoS koruması nasıl yapılandırılabilen:https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Azure Güvenlik Duvarı nasıl dağıtılır:https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı Anlayın:https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

Azure Güvenlik Merkezi Uyarlamalı Ağ Sertleştirmeyi Anlayın:https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening

Azure Güvenlik Merkezi Tam Zamanında Ağ Erişim Denetimi:https://docs.microsoft.com/azure/security-center/security-center-just-in-time


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Ağ paketlerini ve akış günlüklerini kaydetme

**Kılavuz**: Azure konteyner kayıt defterinizi korumak için kullanılan alt ağa bağlı NSG için ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin. Akış kayıtları oluşturmak için NSG akış günlüklerini bir Azure Depolama Hesabına kaydedebilirsiniz. Anormal etkinliği araştırmak için gerekirse, Azure Ağ İzleyicipaket yakalamayı etkinleştirin.

NSG Akış Günlükleri nasıl etkinleştirilir:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Ağ İzleyicisi nasıl etkinleştirilir:https://docs.microsoft.com/azure/network-watcher/network-watcher-create


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Ağ tabanlı izinsiz giriş algılama/saldırı önleme sistemlerini (IDS/IPS) dağıtma

**Kılavuz**: Azure Marketi'nden, yük denetimi özellikleriyle IDS/IPS işlevselliğini destekleyen bir teklif seçin. Yük denetimine dayalı izinsiz giriş algılama ve/veya önleme bir gereklilik değilse, Tehdit İstihbaratı içeren Azure Güvenlik Duvarı kullanılabilir. Azure Güvenlik Duvarı Tehdit İstihbarat tabanlı filtreleme, bilinen kötü amaçlı IP adreslerine ve etki alanlarındaki trafiği uyarabilir ve reddedebilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklanmaktadır.

Kötü amaçlı trafiği algılamak ve/veya reddetmek için kuruluşunuzun ağ sınırlarının her birinde seçtiğiniz güvenlik duvarı çözümünün dağıtılanın.

Azure Marketi:https://azuremarketplace.microsoft.com/marketplace/?term=Firewall 

Azure Güvenlik Duvarı nasıl dağıtılır:https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Azure Güvenlik Duvarı ile uyarıları yapılandırma:https://docs.microsoft.com/azure/firewall/threat-intel


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Web uygulamalarına trafik yönetme

**Rehberlik**: Geçerli değildir. Kıyaslama, Azure Uygulama Hizmeti veya bilgi işlem kaynakları üzerinde çalışan web uygulamaları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Ağ güvenliği kurallarının karmaşıklığını ve yönetim ek lerini en aza indirmek

**Kılavuz**: Kapsayıcı kayıt defterinize erişimi gereken kaynaklar için, Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'ndaki ağ erişim denetimlerini tanımlamak için Azure Kapsayıcı Kayıt Defteri hizmeti için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında "AzureContainerRegistry" hizmet etiketi adını belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz. Microsoft, hizmet etiketinin kapsadığı adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.

Hizmet etiketine göre erişime izin verin:https://docs.microsoft.com/azure/container-registry/container-registry-firewall-access-rules#allow-access-by-service-tag


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Ağ aygıtları için standart güvenlik yapılandırmalarını koruyun

**Yönerge**: Azure Kapsayıcı kayıtlarınızla ilişkili ağ kaynakları için Standart güvenlik yapılandırmalarını Azure İlkesi ile tanımlayın ve uygulayın. Kapsayıcı kayıt defterlerinizin ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.ContainerRegistry" ve "Microsoft.Network" ad alanlarında Azure İlkesi takma adlarını kullanın. 

Azure Kaynak Yöneticisi şablonları, RBAC denetimleri ve ilkeleri gibi önemli ortam yapılarını tek bir plan tanımında paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure Planları'nı kullanabilirsiniz. Sürümü aracılığıyla planı yeni aboneliklere ve ince ayar denetimi ne kadar hassas bir şekilde uygulayın.

Azure İlkesi'ni kullanarak Azure kapsayıcı kayıt defterlerinin denetim uyumluluğunu denetleyin:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy

Azure Blueprint nasıl oluşturulur:https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1.10: Belge trafiği yapılandırma kuralları

**Yönerge**: Müşteri, Azure Kaynak Yöneticisi şablonları, RBAC denetimleri ve ilkeleri gibi önemli ortam yapılarını tek bir plan tanımında paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure Planları'nı kullanabilir. Sürümü aracılığıyla planı yeni aboneliklere ve ince ayar denetimi ne kadar hassas bir şekilde uygulayın.

Azure Blueprint nasıl oluşturulur:https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Ağ kaynağı yapılandırmalarını izlemek ve değişiklikleri algılamak için otomatik araçlar kullanın

**Yönerge**: Ağ kaynak yapılandırmalarını izlemek ve konteyner kayıtlarınız ile ilgili ağ kaynaklarının değişikliklerini algılamak için Azure Etkinlik Günlüğü'ne kullanın. Azure Monitor'da kritik ağ kaynaklarında değişiklikler olduğunda tetikleyecek uyarılar oluşturun.

Azure Etkinlik Günlüğü etkinliklerini görüntüleme ve alma:https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Monitor'da uyarı oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için [Güvenlik Denetimi: Günlüğe kaydetme ve izleme.](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Onaylanmış zaman senkronizasyon kaynaklarını kullanma

**Kılavuz**: Microsoft, Azure kaynakları için zaman kaynakları tutar, ancak bilgi işlem kaynaklarınız için zaman eşitleme ayarlarını yönetme seçeneğiniz vardır.

Azure bilgi işlem kaynakları için zaman eşitleme yapılandırması nasıl yapılandırılır:https://docs.microsoft.com/azure/virtual-machines/windows/time-sync


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Yönerge**: Azure kapsayıcı kayıt defteri tarafından oluşturulan güvenlik verilerini toplamak için günlükleri Azure Monitor üzerinden alın. Azure Monitor'da, analitiği sorgulamak ve gerçekleştirmek için Log Analytics Workspace(ler)'i kullanın ve uzun vadeli/arşivdepolama için Azure Depolama Hesapları'nı kullanın.

Tanılama değerlendirme ve denetim için Azure Kapsayıcı Kayıt Defteri günlükleri:https://docs.microsoft.com/azure/container-registry/container-registry-diagnostics-audit-logs



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

**Yönerge**: Azure Monitor, kayıt defterinizdeki kullanıcı tarafından yönlendirilen olaylar için kaynak günlükleri (eski adıyla tanılama günlükleri olarak adlandırılır) toplar. Kayıt defteri kimlik doğrulama olaylarını denetlemek için bu verileri toplayın ve tüketin ve kayıt defterinizdeki güvenlik sorunlarını tanılayabilmeniz için çekme ve itme olayları gibi kayıt defteri yapılarında tam bir etkinlik izi sağlayın.

Tanılama değerlendirme ve denetim için Azure Kapsayıcı Kayıt Defteri günlükleri:https://docs.microsoft.com/azure/container-registry/container-registry-diagnostics-audit-logs


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: İşletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: Geçerli değildir. Kıyaslama, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="25-configure-security-log-storage-retention"></a>2.5: Güvenlik günlüğü depolama tutma yapılandırma

**Kılavuz**: Azure Monitor'da, Log Analytics Çalışma Alanı saklama sürenizi kuruluşunuzun uyumluluk yönetmeliklerine göre ayarlayın. Uzun vadeli/arşivdepolama için Azure Depolama Hesapları'nı kullanın.

Log Analytics Çalışma Alanları için günlük bekletme parametreleri nasıl ayarlanır:https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2.6: Günlükleri izleme ve inceleme

**Kılavuz**: Anormal davranışlar için Azure Konteyner Kayıt Defteri günlüklerini analiz edin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Monitor'un Günlük Analizi Çalışma Alanını kullanın.

Tanılama değerlendirme ve denetim için Azure Kapsayıcı Kayıt Defteri günlükleri:https://docs.microsoft.com/azure/container-registry/container-registry-diagnostics-audit-logs

Günlük Analizi Çalışma Alanını Anlayın:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Monitor'da özel sorgular nasıl gerçekleştirililir:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Anormal aktivite için uyarıları etkinleştirme

**Kılavuz :** Güvenlik günlüklerinde ve Azure konteyner kayıt defterinizle ilgili olaylardaki anormal etkinlikleri izlemek ve uyarmak için Azure Log Analytics çalışma alanını kullanın.

Tanılama değerlendirme ve denetim için Azure Kapsayıcı Kayıt Defteri günlükleri:https://docs.microsoft.com/azure/container-registry/container-registry-diagnostics-audit-logs

Günlük analitiği günlük verilerinde nasıl uyarı olun:https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2.8: Kötü amaçlı yazılımdan koruma günlüğe kaydetmeyi merkezileştirin

**Rehberlik**: Geçerli değildir. Azure Kapsayıcı Kayıt Defteri, kötü amaçlı yazılımdan koruma yla ilgili günlükleri işlemez veya üretmez.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="29-enable-dns-query-logging"></a>2.9: DNS sorgu günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir. Azure Kapsayıcı Kayıt Defteri bir bitiş noktasıdır ve iletişimi başlatmaz ve hizmet DNS sorgusu yapmaz.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="210-enable-command-line-audit-logging"></a>2.10: Komut satırı denetim günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir. Kıyaslama, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için [güvenlik denetimi: kimlik ve erişim denetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)bakın.*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: İdari hesapların envanterini korumak

**Yol gösterici**: Azure Active Directory (Azure AD) açıkça atanması gereken ve sorgulanabilen yerleşik rollere sahiptir. Yönetim gruplarına üye hesapları bulmak için geçici sorgular gerçekleştirmek için Azure AD PowerShell modüllerini kullanın.

Her Azure kapsayıcı kayıt defteri için yerleşik yönetici hesabının etkin mi yoksa devre dışı mı olduğunu izleyin. Kullanılmadığında hesabı devre dışı edin.

PowerShell ile Azure AD'de dizin rolü nasıl elde elabilirsiniz:https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

PowerShell ile Azure AD'de dizin rolünün üyelerini nasıl edinire bilgili olabilirsiniz:https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

Azure Konteyner Kayıt Defteri yönetici hesabı:https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Varsayılan parolaları varsa değiştirme

**Kılavuz :** Azure Active Directory (Azure AD) varsayılan parola kavramına sahip değildir. Parola gerektiren diğer Azure kaynakları, hizmete bağlı olarak farklılık gösteren karmaşıklık gereksinimleri ve minimum parola uzunluğuyla bir parola oluşturulmasını zorlar. Varsayılan parolaları kullanabilen üçüncü taraf uygulamalardan ve Market hizmetlerinden siz sorumlusunuz.

Bir Azure kapsayıcı kayıt defterinin varsayılan yönetici hesabı etkinleştirilirse, karmaşık parolalar otomatik olarak oluşturulur ve döndürülmelidir. Kullanılmadığında hesabı devre dışı edin.

Azure Konteyner Kayıt Defteri yönetici hesabı:https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Özel yönetim hesaplarını kullanma

**Rehberlik**: Özel yönetim hesaplarının kullanımı yla ilgili standart işletim prosedürleri oluşturun. Yönetim hesabı sayısını izlemek için Azure Güvenlik Merkezi Kimliği ve Erişim Yönetimi'ni kullanın.

Ayrıca, bir kapsayıcı kayıt defterinin yerleşik yönetici hesabını etkinleştirmek için yordamlar oluşturun. Kullanılmadığında hesabı devre dışı edin.

Azure Güvenlik Merkezi Kimliğini ve Erişimini Anlayın:https://docs.microsoft.com/azure/security-center/security-center-identity-access

Azure Konteyner Kayıt Defteri yönetici hesabı:https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Azure Active Directory ile tek oturum açma (SSO) kullanma

**Kılavuz**: Mümkün olan her yerde, hizmet başına tek tek kimlik bilgilerini yapılandırmak yerine Azure Active Directory SSO'nu kullanın. Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerini kullanın.

Konteyner kayıt defterine tek tek erişim için Azure Active Directory ile tümleşik tek tek oturum açma'yı kullanın.

Azure AD ile SSO'yi anlayın:https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on

Konteyner kayıt defterine tek tek giriş:https://docs.microsoft.com/azure/container-registry/container-registry-authentication#individual-login-with-azure-ad


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulamayı kullanın

**Kılavuz :** Azure Etkin Dizin (Azure AD) çok faktörlü kimlik doğrulamasını (MFA) etkinleştirin ve Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerini izleyin.

Azure'da MFA nasıl etkinleştirilir:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi'nde kimlik ve erişim nasıl izlenir:https://docs.microsoft.com/azure/security-center/security-center-identity-access


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Tüm idari görevler için özel makineleri (Ayrıcalıklı Erişim İş İstasyonları) kullanın

**Kılavuz :** Azure kaynaklarında oturum açmak ve yapılandırmak için MFA yapılandırılmış PAW'ları (ayrıcalıklı erişim iş istasyonları) kullanın.

Ayrıcalıklı Erişim İş İstasyonları hakkında bilgi edinin:https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure'da MFA nasıl etkinleştirilir:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted


**Azure Güvenlik Merkezi izleme**: N/A

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: İdari hesaplardan şüpheli etkinliklere ilişkin günlük ve uyarı

**Kılavuz :** Çevrede şüpheli veya güvensiz etkinlik oluştuğunda günlük ve uyarı oluşturma için Azure Active Directory (Azure AD) güvenlik raporlarını kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi'ni kullanın.

Riskli etkinlikler için işaretlenen Azure AD kullanıcılarını nasıl tanımlarım:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk

Azure Güvenlik Merkezi'nde kullanıcıların kimlik ve erişim etkinliklerini izleme:https://docs.microsoft.com/azure/security-center/security-center-identity-access


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Yönerge**: Yalnızca IP adresi aralıklarının veya ülkelerin/bölgelerin inbelirli mantıksal gruplandırmalarından erişime izin vermek için Koşullu Erişim Adlandırılmış Konumlar'ı kullanın.

Azure'da Adlandırılmış Konumlar nasıl yapılandırılabilen:https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3.9: Azure Etkin Dizini Kullanma

**Kılavuz :** Merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory 'i (Azure AD) kullanın. Azure AD, veri için güçlü şifreleme kullanarak verileri istirahatte ve aktarım sırasında korur. Azure AD ayrıca kullanıcı kimlik bilgilerini tuzlar, iş lerle ve güvenli bir şekilde depolar.

Azure AD örneğini oluşturma ve yapılandırma:https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Kullanıcı erişimini düzenli olarak gözden geçirin ve uzlaştırın

**Rehberlik**: Azure Active Directory (Azure AD), eski hesapları keşfetmeye yardımcı olmak için günlükler sağlar. Ayrıca, grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarını verimli bir şekilde yönetmek için Azure Kimlik Erişim İncelemeleri'ni kullanın. Kullanıcı erişimi, yalnızca doğru Kullanıcıların sürekli erişime sahip olduğundan emin olmak için düzenli olarak gözden geçirilebilir.

Azure AD raporlamalarını anlayın:https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Azure kimlik erişim incelemeleri nasıl kullanılır:https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Devre dışı bırakılmış hesaplara erişim denemelerini izleme

**Kılavuz**: Herhangi bir Güvenlik Bilgisi ve Olay Yönetimi (SIEM) /İzleme aracıyla tümleştirmenize olanak tanıyan Azure Active Directory (Azure AD) Oturum Açma Etkinliği, Denetim ve Risk Olayı günlük kaynaklarına erişebilirsiniz.

Azure Active Directory kullanıcı hesapları için Tanılama Ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics Çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. Log Analytics Çalışma Alanı içinde istediğiniz Uyarıları yapılandırabilirsiniz.

Azure Etkinlik Günlükleri Azure Monitör'e nasıl entegre eleştirilir:https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Hesap giriş davranış sapması konusunda uyarı

**Kılavuz :** Kullanıcı kimlikleri ile ilgili algılanan şüpheli eylemlere otomatik yanıtları yapılandırmak için Azure Active Directory (Azure AD) Risk ve Kimlik Koruması özelliklerini kullanın. 

Azure AD riskli oturum açmaları nasıl görüntülenebilen:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik Koruması risk ilkelerinin nasıl yapılandırılabilen ve etkinleştirilir:https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Destek senaryoları sırasında Microsoft'a ilgili müşteri verilerine erişim sağlama

**Rehberlik**: Mevcut değil; Müşteri Kilit Kutusu şu anda Azure Kapsayıcı Kayıt Defteri için desteklenmez.

Müşteri Lockbox desteklenen hizmetler listesi:https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için [güvenlik denetimi: veri koruma.](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Hassas Bilgilerin envanterini koruyun

**Yönerge**: Hassas bilgileri depolayan veya işleyen Azure kapsayıcı kayıt şirketlerini izlemeye yardımcı olmak için kaynak etiketlerini kullanın.

Hassas bilgileri depolayan veya işleyen görüntüleri izlemeye yardımcı olmak için, bir kayıt defterindeki kapsayıcı görüntüleri veya diğer yapıları etiketleyin ve kilitleyin ve görüntüleri veya depoları kilitleyin.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Kapsayıcı görüntülerinin etiketlemesi ve sürüm lanması için öneriler:https://docs.microsoft.com/azure/container-registry/container-registry-image-tag-version

Kapsayıcı görüntüsünü Azure kapsayıcı kayıt defterine kilitleme:https://docs.microsoft.com/azure/container-registry/container-registry-image-lock



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Hassas bilgileri depolama veya işleme sistemlerini yalıtma

**Rehberlik**: Geliştirme, test ve üretim için ayrı konteyner kayıtları, abonelikler ve/veya yönetim grupları uygulayın. Hassas verilerin depolanması veya işlenmesi gereken kaynaklar yeterince izole edilmelidir.

Kaynaklar sanal ağ veya alt ağ tarafından ayrılmalı, uygun şekilde etiketlenmelidir ve bir ağ güvenlik grubu (NSG) veya Azure Güvenlik Duvarı tarafından güvence altına alınmalıdır.

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim grupları nasıl oluşturulur:https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Azure sanal ağı veya güvenlik duvarı kurallarını kullanarak Bir Azure kapsayıcı kayıt defterine erişimi kısıtlayın:https://docs.microsoft.com/azure/container-registry/container-registry-vnet

Nasıl bir güvenlik config ile bir NSG oluşturmak için:https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

Azure Güvenlik Duvarı nasıl dağıtılır:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Azure Güvenlik Duvarı ile uyarı veya uyarı yapılandırma ve reddetme:

https://docs.microsoft.com/azure/firewall/threat-intel



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Hassas bilgilerin yetkisiz aktarımının izlenmesi ve engellenmesi

**Kılavuz**: Hassas bilgilerin yetkisiz aktarılmasını izleyen ve bilgi güvenliği profesyonellerini uyarırken bu tür aktarımları engelleyen ağ çevrelerine otomatik bir araç dağıtın.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Tüm hassas bilgileri taşıma sırasında şifreleme

**Kılavuz**: Azure Konteyner Kayıt Defteri'nize bağlanan tüm istemcilerin TLS 1,2 veya daha büyük bir anlaşma sağlayabilmesini sağlayın. Microsoft Azure kaynakları TLS 1.2'yi varsayılan olarak görüşür.

Mümkün olduğu durumlarda, aktarım sırasında istirahat te şifreleme ve şifreleme için Azure Güvenlik Merkezi önerilerine uyun.

Azure ile aktarım sırasında şifrelemeyi anlayın:https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Hassas verileri tanımlamak için etkin bir bulma aracı kullanın

**Kılavuz**: Azure Konteyner Kayıt Defteri için veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz kullanılamıyor. Uyumluluk amacıyla gerekirse üçüncü taraf çözümlerini uygulayın.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Kaynaklara erişimi denetlemek için Azure RBAC'ı kullanın

**Kılavuz**: Azure kapsayıcı kayıt defterindeki verilere ve kaynaklara erişimi denetlemek için Azure Active Directory (Azure AD) RBAC'ı kullanın. 

Azure'da RBAC nasıl yapılandırılmaz:https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

Azure Kapsayıcı Kayıt Defteri rolleri ve izinleri:https://docs.microsoft.com/azure/container-registry/container-registry-roles



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Erişim denetimini uygulamak için ana bilgisayar tabanlı veri kaybı önlemeyi kullanın

**Kılavuz**: Bilgi işlem kaynaklarına uyumluluk için gerekliyse, veriler sistemden kopyalandığında bile verilere erişim denetimlerini zorlamak için otomatik ana bilgisayar tabanlı veri kaybı önleme çözümü gibi üçüncü taraf bir araç uygulayın.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Hassas bilgileri istirahatte şifreleme

**Yönerge**: Tüm Azure kaynaklarında şifrelemeyi kullanın. Varsayılan olarak, bir Azure kapsayıcı kayıt defterindeki tüm veriler Microsoft tarafından yönetilen anahtarlar kullanılarak istirahatte şifrelenir.

Azure'da dinlenilerde şifrelemeyi anlayın:https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest

Azure Konteyner Kayıt Defteri'nde müşteri tarafından yönetilen anahtarlar:https://aka.ms/acr/cmk



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Kritik Azure kaynaklarında yapılan değişiklikleri kaydedin ve uyarı

**Yönerge**: Azure Monitor, kayıt defterinizdeki kullanıcı tarafından yönlendirilen olaylar için kaynak günlükleri (eski adıyla tanılama günlükleri olarak adlandırılır) toplar. Kayıt defteri kimlik doğrulama olaylarını denetlemek için bu verileri toplayın ve tüketin ve kayıt defterinizle operasyonel sorunları tanılayabilmeniz için çekme ve çekme olayları gibi kayıt defteri yapılarında tam bir etkinlik izi sağlayın.

Tanılama değerlendirme ve denetim için Azure Kapsayıcı Kayıt Defteri günlükleri:https://docs.microsoft.com/azure/container-registry/container-registry-diagnostics-audit-logs


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için [Güvenlik Denetimi: Güvenlik Açığı Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)bakın.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Otomatik güvenlik açığı tarama araçlarını çalıştırın

**Yönerge**: Azure Güvenlik Merkezi'nin konteyner resimlerinizde güvenlik açığı değerlendirmeleri gerçekleştirmekonusundaki önerileri izleyin. Görüntü güvenlik açığı değerlendirmeleri gerçekleştirmek için Azure Marketi'nden isteğe bağlı olarak üçüncü taraf çözümleri dağıtın.

Azure Güvenlik Merkezi güvenlik açığı değerlendirme önerileri nasıl uygulanır:https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations

Güvenlik Merkezi ile Azure Kapsayıcı Kayıt Defteri tümleştirmesi (Önizleme):https://docs.microsoft.com/azure/security-center/azure-container-registry-integration



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Otomatik işletim sistemi yama yönetimi çözümlerini dağıtma

**Kılavuz**: Microsoft, Azure Konteyner Kayıt Defteri'ni destekleyen temel sistemlerde yama yönetimi gerçekleştirir.

İşletim sisteminden ve diğer düzeltme emarelerinden temel görüntülere güncelleştirmeler algılandığında kapsayıcı görüntü güncelleştirmelerini otomatikleştirin.

Azure Kapsayıcı Kayıt Defteri görevleri için temel resim güncelleştirmeleri hakkında:https://docs.microsoft.com/azure/container-registry/container-registry-tasks-base-images


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Otomatik üçüncü taraf yazılım yama yönetimi çözümlerini dağıtma

**Kılavuz**: Uygulama görüntülerini yamalamak için üçüncü taraf çözümlerini kullanabilirsiniz.  Ayrıca, güvenlik düzeltme ekine veya temel görüntülerdeki diğer güncelleştirmeleri temel olarak bir kapsayıcı kayıt defterindeki uygulama resimlerigüncelleştirmelerini otomatikleştirmek için Azure Kapsayıcı Kayıt Defteri görevlerini çalıştırabilirsiniz.

ACR Görevleri için temel görüntü güncelleştirmeleri hakkında:https://docs.microsoft.com/azure/container-registry/container-registry-tasks-base-images



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Arka arkaya güvenlik açığı taramalarını karşılaştırın

**Kılavuz**: Güvenlik açıkları için konteyner görüntülerinin periyodik olarak taranmasını etkinleştirmek için Azure Kapsayıcı Kayıt Defteri'ni (ACR) Azure Güvenlik Merkezi ile tümleştirin. İsteğe bağlı olarak, dönemsel görüntü güvenlik açığı taramaları gerçekleştirmek için Azure Marketi'nden üçüncü taraf çözümleri dağıtın.

Güvenlik Merkezi ile Azure Kapsayıcı Kayıt Defteri tümleştirmesi (Önizleme):https://docs.microsoft.com/azure/security-center/azure-container-registry-integration


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Keşfedilen güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Kılavuz**: Güvenlik açıkları için konteyner görüntülerinin periyodik olarak taranmasını ve riskleri sınıflandırmasını etkinleştirmek için Azure Konteyner Kayıt Defteri'ni (ACR) Azure Güvenlik Merkezi ile tümleştirin. İsteğe bağlı olarak, dönemsel görüntü güvenlik açığı taramaları ve risk sınıflandırması gerçekleştirmek için Azure Marketi'nden üçüncü taraf çözümleri dağıtın.

Güvenlik Merkezi ile Azure Kapsayıcı Kayıt Defteri tümleştirmesi (Önizleme):https://docs.microsoft.com/azure/security-center/azure-container-registry-integration



**Azure Güvenlik Merkezi izleme**: N/A

**Sorumluluk**: Müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için [güvenlik denetimi: Envanter ve Varlık Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)bakın.*

### <a name="61-use-azure-asset-discovery"></a>6.1: Azure Varlık Bulma'yı kullanma

**Yönerge**: Aboneliğinizdeki tüm kaynakları (bilgi işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın.  Kiracınızda uygun (okundu) izinler sağlayın ve aboneliklerinizdeki tüm Azure aboneliklerinin yanı sıra kaynakları sırala.

Kaynak Grafiği aracılığıyla klasik Azure kaynakları keşfedilebilir, ancak ileriye dönük Olarak Azure Kaynak Yöneticisi kaynaklarının oluşturulması ve kullanılması önerilir.

Azure Kaynak Grafiği ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure Aboneliklerinizi görüntüleme:https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0 

Azure RBAC'ı anlayın:https://docs.microsoft.com/azure/role-based-access-control/overview



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6.2: Varlık meta verilerini koruma

**Kılavuz**: Azure Kapsayıcı Kayıt Defteri, bir kayıt defterindeki resimler için etiketler ve bildirimler de dahil olmak üzere meta verileri tutar. Yapıları etiketlemek için önerilen uygulamaları izleyin.

Kayıt defterleri, depolar ve görüntüler hakkında:https://docs.microsoft.com/azure/container-registry/container-registry-concepts

Kapsayıcı görüntülerinin etiketlemesi ve sürüm lanması için öneriler:https://docs.microsoft.com/azure/container-registry/container-registry-image-tag-version


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Yetkisiz Azure kaynaklarını silme

**Kılavuz**: Azure Kapsayıcı Kayıt Defteri, bir kayıt defterindeki resimler için etiketler ve bildirimler de dahil olmak üzere meta verileri tutar. Yapıları etiketlemek için önerilen uygulamaları izleyin.

Kayıt defterleri, depolar ve görüntüler hakkında:https://docs.microsoft.com/azure/container-registry/container-registry-concepts

Kapsayıcı görüntülerinin etiketlemesi ve sürüm lanması için öneriler:https://docs.microsoft.com/azure/container-registry/container-registry-image-tag-version



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Onaylanan Azure kaynaklarının ve yazılım başlıklarının envanterini koruyun

**Kılavuz**: Kuruluş gereksinimlerinize göre onaylanmış Azure kaynaklarının bir envanterini oluşturmanız gerekir.  

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Onaylanmamış Azure kaynakları için izleme

**Yönerge**: Aboneliğinizde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure İlkesi'ni kullanın.

Abonelik(ler) içindeki kaynakları sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın.  Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

Azure İlkesi'ni kullanarak Azure kapsayıcı kayıt defterlerinin denetim uyumluluğunu denetleyin:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Bilgi işlem kaynakları içindeki onaylanmamış yazılım uygulamalarını izleme

**Kılavuz**: Anormal davranışlar için Azure Konteyner Kayıt Defteri günlüklerini analiz edin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Monitor'un Günlük Analizi Çalışma Alanını kullanın.

Tanılama değerlendirme ve denetim için Azure Kapsayıcı Kayıt Defteri günlükleri:https://docs.microsoft.com/azure/container-registry/container-registry-diagnostics-audit-logs

Günlük Analizi Çalışma Alanını Anlayın:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Monitor'da özel sorgular nasıl gerçekleştirililir:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Yönlendirme**: Azure Otomasyonu, iş yüklerinin ve kaynakların dağıtımı, işlemleri ve devre dışı bırakılması sırasında tam denetim sağlar.  Yetkisiz Azure kaynaklarını kaldırmak için kendi çözümünüzü uygulayabilirsiniz. Azure Otomasyonuna giriş:https://docs.microsoft.com/azure/automation/automation-intro


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="68-use-only-approved-applications"></a>6.8: Yalnızca onaylı uygulamaları kullanın

**Rehberlik**: Geçerli değildir. Kıyaslama, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="69-use-only-approved-azure-services"></a>6.9: Yalnızca onaylanmış Azure hizmetlerini kullanma

**Kılavuz**: Ortamınızda hangi hizmetleri sağabileceğinizi kısıtlamak için Azure İlke'den yararlanın.

Azure İlkesi'ni kullanarak Azure kapsayıcı kayıt defterlerinin denetim uyumluluğunu denetleyin:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlkesi ile belirli bir kaynak türü nasıl reddedilecek:https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="610-implement-approved-application-list"></a>6.10: Onaylı başvuru listesini uygulama

**Rehberlik**: Geçerli değildir. Kıyaslama, hesaplama kaynakları için tasarlanmıştır.



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="611-limit-users-ability-to-interact-with-azureresources-manager-via-scripts"></a>6.11: Kullanıcıların komut dosyaları aracılığıyla AzureResources Yöneticisi ile etkileşim deşmelerini sınırlandırın

**Yönerge**: Kullanıcıların Azure bilgi işlem kaynakları içinde komut dosyalarını yürütme yeteneğini sınırlamak için işletim sistemine özgü yapılandırmaları veya üçüncü taraf kaynaklarını kullanın.

Koşullu Erişim'i Azure Kaynakları Yöneticisi'ne erişimi engellemek için yapılandırma:https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Kullanıcıların bilgi işlem kaynakları içinde komut dosyalarını yürütme yeteneğini sınırlama

**Yönerge**: Kullanıcıların Azure bilgi işlem kaynakları içinde komut dosyalarını yürütme yeteneğini sınırlamak için işletim sistemine özgü yapılandırmaları veya üçüncü taraf kaynaklarını kullanın.

Örneğin, Windows Ortamlarında PowerShell komut dosyası yürütmenasıl denetilir:https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Yüksek riskli uygulamaları fiziksel veya mantıksal olarak ayırmak

**Yönlendirme**: İş işlemleri için gerekli olan ancak kuruluş için daha yüksek risk lere yol açabilen yazılım, kendi sanal makinesi ve/veya sanal ağı içinde izole edilmeli ve bir Azure Güvenlik Duvarı veya Ağ Güvenliği Grubu ile yeterince güvenli hale alınmalıdır.

Sanal ağ nasıl oluşturulur:https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Nasıl bir güvenlik config ile bir NSG oluşturmak için:https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için güvenlik [denetimi: Güvenli Yapılandırma'ya](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)bakın.*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Tüm Azure kaynakları için güvenli yapılandırmalar oluşturma

**Yönerge**: Tüm Azure Kaynakları için güvenlik yapılandırmalarını korumak için Azure İlkesi veya Azure Güvenlik Merkezi'ni kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlkesi'ni kullanarak Azure kapsayıcı kayıt defterlerinin denetim uyumluluğunu denetleyin:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Güvenli işletim sistemi yapılandırmaları oluşturma

**Rehberlik**: Tüm bilgi işlem kaynaklarındaki güvenlik yapılandırmalarını korumak için Azure Güvenlik Merkezi önerisinden yararlanın "Sanal Makinelerinizdeki Güvenlik Yapılandırmalarında Güvenlik Açıklarını Düzeltin" önerisinden yararlanın.

Azure Güvenlik Merkezi önerileri nasıl izlenir:https://docs.microsoft.com/azure/security-center/security-center-recommendations

Azure Güvenlik Merkezi önerilerini nasıl düzeltiresınız:https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Güvenli Azure kaynak yapılandırmalarını koruyun

**Yol gösterici**: Azure kaynaklarınız arasında güvenli ayarlar uygulamak için Azure ilkesini [reddet] ve [varsa dağıt] kullanın.

Azure İlkesi'ni kullanarak Azure kapsayıcı kayıt defterlerinin denetim uyumluluğunu denetleyin:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlkesi efektlerini anlayın:https://docs.microsoft.com/azure/governance/policy/concepts/effects



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Güvenli işletim sistemi yapılandırmalarını koruyun

**Rehberlik**: Geçerli değildir. Kıyaslama, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure kaynaklarının yapılandırmayı güvenli bir şekilde depolama

**Kılavuz**: Özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos'u kullanın.

Azure DevOps'lerde kod depolama:https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Depobelgeleri:https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Özel işletim sistemi görüntülerini güvenli bir şekilde saklayın

**Rehberlik**: Geçerli değildir. Kıyaslama hesaplama kaynakları için geçerlidir.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Sistem yapılandırma yönetim araçlarını dağıtma

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için Azure İlkesi'ni kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve ardışık hatlar geliştirin.

Azure İlkesi'ni kullanarak Azure kapsayıcı kayıt defterlerinin denetim uyumluluğunu denetleyin:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: İşletim sistemleri için sistem yapılandırma yönetim araçlarını dağıtma

**Rehberlik**: Geçerli değildir. Kıyaslama hesaplama kaynakları için geçerlidir.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Azure hizmetleri için otomatik yapılandırma izleme uygulaması

**Yönerge**: Azure Kaynaklarınız için temel tarama yapmak için Azure Güvenlik Merkezi'ni kullanın.

Aboneliğinizde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure İlkesi'ni kullanın.

Azure Güvenlik Merkezi'nde öneriler nasıl düzeltilir:https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations

Azure İlkesi'ni kullanarak Azure kapsayıcı kayıt defterlerinin denetim uyumluluğunu denetleyin:https://docs.microsoft.com/azure/container-registry/container-registry-azure-policy



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: İşletim sistemleri için otomatik yapılandırma izleme

**Rehberlik**: Geçerli değildir. Kıyaslama hesaplama kaynakları için geçerlidir.


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="711-manage-azure-secrets-securely"></a>7.11: Azure sırlarını güvenli bir şekilde yönetme

**Rehberlik**: Bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvence altına almak için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliğini kullanın.

Azure Yönetilen Kimliklerle nasıl tümleştirilir:https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Anahtar Kasası nasıl oluşturulur:https://docs.microsoft.com/azure/key-vault/quick-create-portal

Key Vault kimlik doğrulaması yönetilen bir kimlikle nasıl sağlayacaksınız:https://docs.microsoft.com/azure/key-vault/managed-identity

Azure Kapsayıcı Kayıt Defteri görevlerinde Azure tarafından yönetilen bir kimlik kullanın:https://docs.microsoft.com/azure/container-registry/container-registry-tasks-authentication-managed-identity


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Kimlikleri güvenli ve otomatik olarak yönetme

**Yol gösterici**: Azure hizmetlerine Azure AD'de otomatik olarak yönetilen bir kimlik sağlamak için Yönetilen Kimlikler'i kullanın. Yönetilen Kimlikler, anahtarınızda herhangi bir kimlik belgesi olmadan Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlere kimlik doğrulamanızı sağlar.

Yönetilen Kimlikler nasıl yapılandırılır:https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

Azure kapsayıcı kayıt defterine kimlik doğrulamak için yönetilen bir kimlik kullanın:https://docs.microsoft.com/azure/container-registry/container-registry-authentication-managed-identity


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: İstenmeden kimlik bilgisi maruziyetini ortadan kaldırın

**Yönerge**: Kod içindeki kimlik bilgilerini tanımlamak için Kimlik Bilgisi Tarayıcısı'nı uygulayın. Kimlik Bilgisi Tarayıcısı, keşfedilen kimlik bilgilerinin Azure Anahtar Kasası gibi daha güvenli konumlara taşınmasını da teşvik edecektir.

Kimlik Bilgisi Tarayıcı nasıl kurulturur:https://secdevtools.azurewebsites.net/helpcredscan.html


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için [Güvenlik Kontrolü: Malware Defense](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)' e bakın.*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımlarını kullanın

**Kılavuz :** Kaynaklarınızı sürekli olarak izlemek ve savunmak için Azure Bulut Hizmetleri ve Sanal Makineler için Microsoft Kötü Amaçlı Yazılımdan Koruma'yı kullanın. Linux için üçüncü taraf kötü amaçlı yazılımdan koruma çözümlerini kullanın.

Bulut Hizmetleri ve Sanal Makineler için Microsoft Antimalware nasıl yapılandırılır:https://docs.microsoft.com/azure/security/fundamentals/antimalware


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: İşlem yapmayan Azure kaynaklarına yüklenecek dosyaları önceden taraya

**Kılavuz :** Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure Kapsayıcı Kayıt Defteri) Microsoft Antimalware etkindir, ancak müşteri içeriğinde çalışmaz.

Uygulama Hizmeti, Veri Gölü Depolama, Blob Depolama vb. gibi bilgi işlem dışı Azure kaynaklarına yüklenen dosyaları önceden tarayabilirsiniz.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Kötü amaçlı yazılımdan koruma yazılımının ve imzalarının güncelleştirilmesini sağlayın

**Rehberlik**: Geçerli değildir. Kıyaslama, hesaplama kaynakları için tasarlanmıştır. Microsoft, altta yatan platform için kötü amaçlı yazılımdan koruma yı işler.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Düzenli otomatik yedeklemeler sağlayın

**Kılavuz**: Microsoft Azure kapsayıcı kayıt defterinizdeki veriler, dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman otomatik olarak çoğaltılır. Azure Kapsayıcı Kayıt Defteri, verilerinizi planlı ve planlanmamış olaylardan korunacak şekilde kopyalar

Birden çok Azure bölgesinde kayıt defteri yinelemelerini korumak için isteğe bağlı olarak bir kapsayıcı kayıt defterini coğrafi olarak çoğaltma. 

Azure Kapsayıcı Kayıt Defteri'nde coğrafi çoğaltma:https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Komple sistem yedeklemelerini gerçekleştirin ve müşteri yönetilen anahtarları yedekleme

**Yönerge**: İsteğe bağlı olarak bir kayıt defterinden diğerine aktarArak konteyner görüntülerini yedekleyin.

Azure komut satırı araçlarını veya SDK'ları kullanarak Azure Key Vault'ta müşteri tarafından yönetilen anahtarları yedekle.

Kapsayıcı görüntülerini bir kapsayıcı kayıt defterine aktarın:https://docs.microsoft.com/azure/container-registry/container-registry-import-images

Azure'da anahtar kasa sıyrık anahtarlarını yedekleme:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Müşteri yönetilen anahtarlar dahil tüm yedeklemeleri doğrulayın

**Kılavuz**: Azure komut satırı araçlarını veya SDK'ları kullanarak Azure Key Vault'ta yedeklenen müşteri yönetilen anahtarlarının test geri yüklemesi.

Azure'da Azure Anahtar Kasası anahtarlarını geri yükleme:https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Yedeklerin ve müşteri yönetilen anahtarlarının korunmasını sağlayın

**Kılavuz**: Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Azure Key Vault'ta Soft-Delete'i etkinleştirebilirsiniz.

Anahtar Kasasında Soft-Delete nasıl etkinleştirilir:https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için [Güvenlik Denetimi: Olay Yanıtı'na](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)bakın.*

### <a name="101-create-an-incident-response-guide"></a>10.1: Olay yanıt kılavuzu oluşturma

**Rehberlik**: Kuruluşunuz için bir olay yanıt kılavuzu oluşturun. Personelin tüm rollerini ve olay işleme/yönetiminin algılamadan olay sonrası incelemeye kadar olan aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun.

Azure Güvenlik Merkezi'nde İş Akışı Otomasyonları nasıl yapılandırılabilen:https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

Kendi güvenlik olayı yanıt sürecinizi oluşturma kılavuzu:https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Microsoft Güvenlik Yanıt Merkezi'nin Bir Olayın Anatomisi:https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Müşteri ayrıca, kendi olay yanıt planının oluşturulmasına yardımcı olmak için NIST'nin Bilgisayar Güvenliği Olay İşlemleri Kılavuzu'ndan da yararlanabilir:https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Olay puanlama ve öncelik lendirme prosedürü oluşturma

**Yönlendirme**: Azure Güvenlik Merkezi, önce hangi uyarıların araştırılması gerektiğine öncelik vermenize yardımcı olmak için her uyarıya önem verir. Önem derecesi, Güvenlik Merkezi'nin bulguda ne kadar emin olduğuna veya uyarıyı vermek için kullanılan analize ve uyarıya yol açan etkinliğin arkasında kötü amaçlı niyet olduğuna dair güven düzeyine dayanır.

Ayrıca, abonelikleri açıkça işaretleyin (eski için. azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10.3: Test güvenlik yanıt yordamları

**Rehberlik**: Sistemlerinizin olay yanıt yeteneklerini düzenli olarak test etmek için alıştırmalar yapın. Zayıf noktaları ve boşlukları belirleyin ve planı gerektiği gibi gözden geçirin.

NIST'in yayınına bakın: BT Planları ve Yetenekleri için Test, Eğitim ve Egzersiz Programları Kılavuzu:https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Güvenlik olayı iletişim bilgilerini sağlayın ve güvenlik olayları için uyarı bildirimlerini yapılandırma

**Kılavuz :** Microsoft Güvenlik Yanıt Merkezi (MSRC), müşterinin verilerine yasadışı veya yetkisiz bir taraf tarafından erişildiğini tespit ederse, güvenlik olayı iletişim bilgileri Microsoft tarafından sizinle iletişim kurmak için kullanılır.  Sorunların çözülmesini sağlamak için olaydan sonraki olayları gözden geçirin.

Azure Güvenlik Merkezi güvenlik kişisini ayarlama:https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Olay yanıt sisteminize güvenlik uyarıları dahil edin

**Kılavuz**: Sürekli Dışa Aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışa aktarın. Sürekli Dışa Aktarma, uyarı ve önerileri el ile veya sürekli olarak dışa aktarmanıza olanak tanır. Sentinel uyarılarını yayınlamak için Azure Güvenlik Merkezi veri konektörünü kullanabilirsiniz.

Sürekli dışa aktarma nasıl yapılandırılabilen:https://docs.microsoft.com/azure/security-center/continuous-export

Uyarıları Azure Sentinel'e nasıl aktarın:https://docs.microsoft.com/azure/sentinel/connect-azure-security-center


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Güvenlik uyarılarına yanıtı otomatikleştirin

**Yönerge**: Güvenlik uyarıları ve önerileri nde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek için Azure Güvenlik Merkezi'ndeki İş Akışı Otomasyonu özelliğini kullanın.

İş Akışı Otomasyonu ve Mantık Uygulamaları nasıl yapılandırılabilen:https://docs.microsoft.com/azure/security-center/workflow-automation


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için [Güvenlik Kontrolü: Penetrasyon Testleri ve Kırmızı Takım Egzersizleri'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises)bakın.*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1: Azure kaynaklarınızı düzenli olarak delme testini gerçekleştirin ve tüm kritik güvenlik bulgularının 60 gün içinde düzeltilmesini sağlayın

**Kılavuz**: Penetrasyon Testlerinizin Microsoft ilkelerini ihlal etmediğini sağlamak için Microsoft Etkileşim Kuralları'na uyun:https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

Microsoft'un Stratejisi ve Red Teaming ve Microsoft tarafından yönetilen bulut altyapısı, hizmetler ve uygulamalara karşı canlı site penetrasyon testi nin uygulanması hakkında daha fazla bilgiyi burada bulabilirsiniz:https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Güvenlik Kıyaslama'ya](https://docs.microsoft.com/azure/security/benchmarks/overview) bakın
- [Azure Güvenlik Taban Çizgileri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
