---
title: Anahtar Kasası için Azure Güvenlik Taban Çizgisi
description: Anahtar Kasası için Azure Güvenlik Taban Çizgisi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 46fa0160dd8b37e89cdd77ba8acdae294fddbefe
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81616899"
---
# <a name="azure-security-baseline-for-key-vault"></a>Anahtar Kasası için Azure Güvenlik Taban Çizgisi

Anahtar Vault için Azure Güvenlik Taban Çizgisi, dağıtımınızın güvenlik duruşunu geliştirmenize yardımcı olacak öneriler içerir.

Bu hizmetin temeli, en iyi uygulamalar kılavuzumuzla Azure'da bulut çözümlerinizi nasıl güvenebileceğinize ilişkin öneriler sunan [Azure Güvenlik Kıyaslama sürümü 1.0'dan](https://docs.microsoft.com/azure/security/benchmarks/overview)alınmıştır.

Daha fazla bilgi için [Azure Güvenlik Taban Çizgilerini genel olarak](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)görün.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için [Güvenlik Denetimi: Ağ Güvenliği'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)bakın.*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Sanal Ağınızdaki Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'nı kullanarak kaynakları koruyun

**Rehberlik**: Azure Anahtar Kasası'nı Azure Özel Bağlantısıyla Tümleştirin. 

Azure Özel Bağlantı Hizmeti, Azure Hizmetleri'ne (örneğin, Azure Anahtar Kasası) ve Azure barındırılan müşteri/iş ortağı hizmetlerine sanal ağınızdaki Özel Bitiş Noktası üzerinden erişmenizi sağlar.

Azure Özel Bitiş Noktası, sizi Azure Özel Bağlantısı ile çalışan bir hizmete özel ve güvenli bir şekilde bağlayan bir ağ arabirimidir. Özel bitiş noktası, VNet'inizden gelen özel bir IP adresini kullanır ve hizmeti VNet'inize etkin bir şekilde getirir. Hizmete giden tüm trafik özel bitiş noktasından yönlendirilebilir, bu nedenle ağ geçidi, NAT aygıtları, ExpressRoute veya VPN bağlantıları veya genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağıörneğine bağlanarak erişim denetiminde en yüksek parçalılık düzeyini sağlayabilirsiniz.

Key Vault'u Azure Özel Bağlantısıyla nasıl tümleştirilir:

https://docs.microsoft.com/azure/key-vault/private-link-service


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Vnet, Subnet ve NIC'lerin yapılandırmasını ve trafiğini izleyin ve kaydedin

**Kılavuz**: Azure Güvenlik Merkezi'ni kullanın ve Azure'da Key Vault tarafından yapılandırılan kaynaklarınızı güvence altına almaya yardımcı olmak için ağ koruma önerilerini izleyin. 

Azure Güvenlik Merkezi tarafından sağlanan Ağ Güvenliği hakkında daha fazla bilgi için: 

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1.3: Kritik web uygulamalarını koruyun

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Bilinen kötü amaçlı IP adresleriyle iletişimi reddetme

**Kılavuz :** Dağıtılmış hizmet reddi saldırılarına karşı koruma sağlamak için Key Vault örneklerinizle ilişkili Azure Sanal Ağlarda Azure DDoS Koruma Standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimi reddetmek için Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı kullanın.

 
Azure portalını kullanarak Azure DDoS Koruma Standardını yönetin:https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Azure Güvenlik Merkezi'ndeki Azure hizmet katmanı için tehdit algılama:https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Ağ paketlerini ve akış günlüklerini kaydetme

**Kılavuz**: Azure Key Vault ağ güvenlik grupları (NSG) kullanmaz ve Azure Key Vault için akış günlükleri yakalanmaz. Bunun yerine, Azure Anahtar Kasası örneklerinizi güvence altına almak ve ölçümleri kaydetmek ve olayları denetlemek için tanı lama ayarlarına olanak sağlamak için Azure Özel Bağlantısını kullanın.

Anahtar Kasası'nı Azure Özel Bağlantısıyla Tümleştirin:

https://docs.microsoft.com/azure/key-vault/private-link-service

Azure Key Vault günlüğü:https://docs.microsoft.com/azure/key-vault/key-vault-logging



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Ağ tabanlı izinsiz giriş algılama/saldırı önleme sistemlerini (IDS/IPS) dağıtma

**Kılavuz**: Bu gereksinim, Azure Anahtar Kasası için gelişmiş tehdit koruması (ATP) yapılandırılarak karşılanabilir. ATP ek bir güvenlik istihbaratı katmanı sağlar. Bu araç, Azure Anahtar Kasası hesaplarına erişmek veya bunlardan yararlanmak için zararlı olabilecek girişimleri algılar.

Azure Güvenlik Merkezi anormal etkinliği algıladığında, uyarıları görüntüler. Ayrıca, abonelik yöneticisine şüpheli etkinliğin ayrıntılarını ve tanımlanan tehditleri nasıl araştırıp düzelteceklerine ilişkin önerileri içeren e-postalar da e-postada bulunur.

Azure Anahtar Kasası için gelişmiş tehdit koruması ayarlayın:

https://docs.microsoft.com/azure/security-center/advanced-threat-protection-key-vault



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Web uygulamalarına trafik yönetme

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Ağ güvenliği kurallarının karmaşıklığını ve yönetim ek lerini en aza indirmek

**Kılavuz**: Azure Anahtar Kasası örneklerinize erişimi gereken kaynaklar için, ağ güvenlik gruplarında veya Azure Güvenlik Duvarı'ndaki ağ erişim denetimlerini tanımlamak için Azure Anahtar Kasası için Azure hizmet etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında servis etiketi adını (örn. ApiManagement) belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz. Microsoft, hizmet etiketinin kapsadığı adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.

Azure hizmet etiketleri genel bakış:https://docs.microsoft.com/azure/virtual-network/service-tags-overview


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Ağ aygıtları için standart güvenlik yapılandırmalarını koruyun

**Yönerge**: Azure İlkesi ile Azure Anahtar Kasası örneklerinizle ilişkili ağ kaynakları için standart güvenlik yapılandırmaları tanımlayın ve uygulayın. Azure Key Vault örneklerinizin ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.KeyVault" ve "Microsoft.Network" ad alanlarında Azure İlkesi takma adlarını kullanın. Azure Anahtar Kasası ile ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz:

Key Vault sanal ağ hizmeti bitiş noktası kullanmalıdır

Öğretici: Uyumluluğu uygulamak için ilkeler oluşturun ve yönetin:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlkesi Örnekleri:

https://docs.microsoft.com/azure/governance/policy/samples/#networ

Quickstart: Portalda bir plan tanımlayın ve atayın:

https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1.10: Belge trafiği yapılandırma kuralları

**Kılavuz**: Meta veriler ve mantıksal organizasyon sağlamak için Azure Anahtar Kasası örnekleriniz için ağ güvenliği ve trafik akışıyla ilgili kaynaklar için etiketlerkullanın.

Tüm kaynakların etiketlerle oluşturulduğundan emin olmak ve varolan etiketlenmemiş kaynakları size bildirmek için etiketlemeyle ilgili "Etiket ve değeri gerektir" gibi yerleşik Azure ilke tanımlarından herhangi birini kullanın.

Azure PowerShell veya Azure CLI'yi, etiketlerine dayalı kaynaklara bakmak veya eylemlergerçekleştirmek için kullanabilirsiniz.

Azure kaynaklarınızı düzenlemek için etiketleri kullanın:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Ağ kaynağı yapılandırmalarını izlemek ve değişiklikleri algılamak için otomatik araçlar kullanın

**Yönerge**: Ağ kaynak yapılandırmalarını izlemek ve Azure Anahtar Kasası örneklerinizle ilgili ağ kaynaklarındaki değişiklikleri algılamak için Azure Etkinlik Günlüğü'ne kullanın. Azure Monitor'da kritik ağ kaynaklarında değişiklikler olduğunda tetikleyecek uyarılar oluşturun.

Azure Etkinlik Günlüğü olaylarını görüntüleyin ve alın:

https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Monitör'ü kullanarak etkinlik günlüğü uyarıları oluşturun, görüntüleyin ve yönetin:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için [Güvenlik Denetimi: Günlüğe kaydetme ve izleme.](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Onaylanmış zaman senkronizasyon kaynaklarını kullanma

**Rehberlik**: Geçerli değildir; Microsoft, günlüklerde zaman damgaları için Azure Anahtar Kasası gibi Azure kaynakları için kullanılan zaman kaynağını korur.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Yönlendirme**: Azure Key Vault tarafından oluşturulan güvenlik verilerini toplamak için günlükleri Azure Monitor üzerinden alın. Azure Monitor'da, analitiği sorgulamak ve gerçekleştirmek için Azure Log Analytics çalışma alanını kullanın ve uzun vadeli/arşivdepolama için Azure Depolama Hesapları'nı kullanın. Alternatif olarak, Azure Sentinel'e veya bir üçüncü taraf SIEM'e veri etkinleştirebilir ve yerleşik veriler verebilirsiniz. 

Azure Key Vault günlüğü:

https://docs.microsoft.com/azure/key-vault/key-vault-logging

Hızlı başlangıç: Azure Sentinel'de nasıl çalıştırılır:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

**Kılavuz**: Denetim, güvenlik ve tanılama günlüklerine erişmek için Azure Anahtar Kasası örneklerinizde tanılama ayarlarını etkinleştirin. Otomatik olarak kullanılabilen etkinlik günlükleri, olay kaynağı, tarih, kullanıcı, zaman damgası, kaynak adresleri, hedef adresleri ve diğer yararlı öğeleri içerir.

Azure Key Vault günlüğü:

https://docs.microsoft.com/azure/key-vault/key-vault-logging


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: İşletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2.5: Güvenlik günlüğü depolama tutma yapılandırma

**Kılavuz**: Azure Monitörü'nde, Azure Anahtar Kasası günlüklerinizi tutmak için kullanılan Log Analytics çalışma alanı için bekletme süresini kuruluşunuzun uyumluluk yönetmeliklerine göre ayarlayın. Uzun vadeli/arşivdepolama için Azure Depolama Hesapları'nı kullanın.

Veri saklama süresini değiştirme:https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2.6: Günlükleri izleme ve inceleme

**Kılavuz**: Anormal davranışlar için günlükleri analiz edin ve izleyin ve Azure Key Vault korumalı kaynaklarınız için sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Monitor'un Günlük Analizi çalışma alanını kullanın. Alternatif olarak, Azure Sentinel'e veya üçüncü taraf bir SIEM'e veri etkinleştirebilir ve yerleşik veriler verebilirsiniz. 

Hızlı başlangıç: Yerleşik Azure Sentinel:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

Azure Monitor'da Log Analytics ile başlayın:

https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Monitor'da günlük sorgularına başlayın:

https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Anormal aktivite için uyarıları etkinleştirme

**Kılavuz :** Azure Güvenlik Merkezi'nde Key Vault için gelişmiş tehdit koruması (ATP) etkinleştirin. Azure Key Vault'ta tanı ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Bir güvenlik düzenleme otomatik yanıt (SOAR) çözümü sağladığı ndan, Log Analytics çalışma alanınız Azure Sentinel'e dahil olur. Bu, oyun kitaplarının (otomatik çözümler) oluşturulmasına ve güvenlik sorunlarını düzeltmek için kullanılmasına olanak tanır.

Hızlı başlangıç: Yerleşik Azure Sentinel:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını yönetin ve yanıtlayın:

https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts

Azure Monitör Uyarıları ile etkinliklere yanıt verin:

https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2.8: Kötü amaçlı yazılımdan koruma günlüğe kaydetmeyi merkezileştirin

**Rehberlik**: Geçerli değildir; Azure Key Vault, kötü amaçlı yazılımdan koruma yla ilgili günlükleri işlemez veya üretmez.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="29-enable-dns-query-logging"></a>2.9: DNS sorgu günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir; Azure Key Vault, DNS ile ilgili günlükleri işlemez veya üretmez.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2.10: Komut satırı denetim günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için [güvenlik denetimi: kimlik ve erişim denetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)bakın.*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: İdari hesapların envanterini korumak

**Kılavuz**: Azure Active Directory'ye kayıtlı uygulamalarınızın yanı sıra Azure Anahtar Kasası anahtarlarınıza, sırlarınıza ve sertifikalarınıza erişimi olan tüm kullanıcı hesaplarının envanterini koruyun. Key Vault erişimini sorgulamak ve uzlaştırmak için Azure portalını veya PowerShell'i kullanabilirsiniz. PowerShell'de erişimi görüntülemek için aşağıdaki komutu kullanın:

(Get-AzResource -Resourceid [KeyVaultResourceID]). Properties.AccessPolicies

Azure Etkin Dizin'e bir uygulama kaydetme:

https://docs.microsoft.com/azure/key-vault/key-vault-manage-with-cli2#registering-an-application-with-azure-active-directory

Anahtar kasasına güvenli erişim:

https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Varsayılan parolaları varsa değiştirme

**Rehberlik**: Geçerli değildir; Azure Key Vault varsayılan parola kavramına sahip değildir, çünkü kimlik doğrulaması Active Directory tarafından sağlanır ve Rol tabanlı erişim denetimi yle güvence altına alınır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Özel yönetim hesaplarını kullanma

**Kılavuz**: Azure Anahtar Kasası örneklerinize erişimi olan özel yönetim hesaplarının kullanımı yla ilgili standart işletim yordamları oluşturun. Etkin yönetim hesabı sayısını izlemek için Azure Güvenlik Merkezi Kimliği ve Erişim Yönetimi'ni (şu anda önizlemede) kullanın.

Kimlik ve erişimi izleme (önizleme):

https://docs.microsoft.com/azure/security-center/security-center-identity-access


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Azure Active Directory ile tek oturum açma (SSO) kullanma

**Kılavuz :** Uygulamanızın doğruluğunu sorunsuz bir şekilde doğrulamak ve Azure Anahtar Kasası sırlarınıza erişmek için kullanılacak belirteci almak için AppId, TenantID ve ClientSecret ile birlikte bir Azure hizmet müdürü kullanın.

.NET kullanarak Azure Anahtar Kasası'na servis-servis kimlik doğrulaması:

https://docs.microsoft.com/azure/key-vault/service-to-service-authentication



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulamayı kullanın

**Yönerge**: Azure Active Directory Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirin ve Etkinlik Hub'ı tarafından etkinleştirilen kaynaklarınızın korunmasına yardımcı olmak için Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi (şu anda önizlemede) önerilerini izleyin.

Bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulama dağıtımı planlama:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Kimlik ve erişimi izleme (önizleme):

https://docs.microsoft.com/azure/security-center/security-center-identity-access


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Tüm idari görevler için özel makineleri (Ayrıcalıklı Erişim İş İstasyonları) kullanın

**Kılavuz :** Key Vault özellikli kaynaklara giriş yapmak ve yapılandırmak üzere yapılandırılan Azure Çok Faktörlü Kimlik Doğrulama (MFA) ile Ayrıcalıklı Erişim İş İstasyonu (PAW) kullanın. 

Ayrıcalıklı Erişim İş İstasyonları:https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations 

Bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulama dağıtımı planlama:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: İdari hesaplardan şüpheli etkinliklere ilişkin günlük ve uyarı

**Rehberlik**: Ortam içinde şüpheli veya güvensiz etkinlik oluştuğunda günlük ve uyarı oluşturma için Azure Active Directory (AAD) Ayrıcalıklı Kimlik Yönetimi (PIM) kullanın. Riskli kullanıcı davranışı yla ilgili uyarıları ve raporları görüntülemek için AAD risk algılamalarını kullanın. Ek günlüğe kaydetme için Azure Güvenlik Merkezi risk algılama uyarılarını Azure Monitor'a gönderin ve Eylem Grupları'nı kullanarak özel uyarı/bildirimleri yapılandırın.

Şüpheli etkinlikler için uyarılar oluşturmak için Azure Key Vault için gelişmiş tehdit koruması (ATP) etkinleştirin.

Azure AD Ayrıcalıklı Kimlik Yönetimi 'ni (PIM) dağıtın:https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan

Azure Key Vault (önizleme) için gelişmiş tehdit koruması ayarlama:https://docs.microsoft.com/azure/security-center/advanced-threat-protection-key-vault

Azure Anahtar Kasası (Önizleme) için uyarılar:https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurekv

Azure Active Directory risk algılamaları:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

Azure portalında eylem grupları oluşturun ve yönetin:https://docs.microsoft.com/azure/azure-monitor/platform/action-groups



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Yönerge**: Koşullu Erişim ilkesinin konum koşulunu yapılandırın ve adlandırılmış konumlarınızı yönetin. Adlandırılmış konumlarla, IP adresi aralıklarının veya ülkelerin ve bölgelerin mantıksal gruplandırmalarını oluşturabilirsiniz. Anahtar Kasa sırlarınız gibi hassas kaynaklara erişimi, yapılandırılan konumlarınız la sınırlandırabilirsiniz.

Azure Etkin Dizin Koşullu Erişim'de konum durumu nedir?:https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3.9: Azure Etkin Dizini Kullanma

**Yol gösterici**: Key Vault gibi Azure kaynakları için merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory 'i (AAD) kullanın. Bu, Rol tabanlı erişim denetiminin (RBAC) hassas kaynakları yönetmesine olanak tanır.

 

Hızlı başlangıç: Azure Etkin Dizini'nde yeni bir kiracı oluşturun:

https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Kullanıcı erişimini düzenli olarak gözden geçirin ve uzlaştırın

**Kılavuz**: Azure Key Vault yönetim rolleri ile eski hesapları keşfetmeye yardımcı olmak için Azure Active Directory (AAD) günlüklerini gözden geçirin. Ayrıca, grup üyeliklerini verimli bir şekilde yönetmek, Azure Anahtar Kasası'na erişmek için kullanılabilecek kurumsal uygulamalara erişim ve rol atamaları için AAD erişim incelemelerini kullanın. Yalnızca doğru kullanıcıların sürekli erişime sahip olduğundan emin olmak için kullanıcı erişimi her 90 günde bir olduğu gibi düzenli olarak gözden geçirilmelidir.

Azure Active Directory raporları ve izleme belgeleri:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Azure AD erişim değerlendirmeleri nelerdir?:

https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Devre dışı bırakılmış hesaplara erişim denemelerini izleme

**Kılavuz :** Azure Key Vault ve Azure Active Directory için tanı ayarlarını etkinleştirerek tüm günlükleri bir Log Analytics çalışma alanına gönderin. Log Analytics içinde istenen uyarıları (devre dışı bırakılan sırlara erişme girişimleri gibi) yapılandırın.

Azure AD günlüklerini Azure Monitor günlükleriyle tümleştirin:https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

Eski Key Vault çözümünden geçiş:https://docs.microsoft.com/azure/azure-monitor/insights/azure-key-vault#migrating-from-the-old-key-vault-solution



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Hesap giriş davranış sapması konusunda uyarı

**Yönlendirme**: Azure Anahtar Kasası korumalı kaynaklarınız ile ilgili algılanan şüpheli eylemlere otomatik yanıtları yapılandırmak için Azure Active Directory'nin Kimlik Koruması ve risk algılama özelliklerini kullanın. Kuruluşunuzun güvenlik yanıtlarını uygulamak için Azure Sentinel aracılığıyla otomatik yanıtları etkinleştirmelisiniz. 

Azure Etkin Dizin portalında riskli oturum açma raporu:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins 

Nasıl Yapılışı: Risk ilkelerini yapılandırma ve etkinleştirme:https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

Azure Sentinel'e nasıl binilir:https://docs.microsoft.com/azure/sentinel/quickstart-onboard


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Destek senaryoları sırasında Microsoft'a ilgili müşteri verilerine erişim sağlama

**Rehberlik**: Geçerli değildir; Azure Anahtar Kasası için Müşteri Kilit Kutusu desteklenmez.

Genel kullanılabilirlik te desteklenen hizmetler ve senaryolar:https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için [güvenlik denetimi: veri koruma.](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Hassas Bilgilerin envanterini koruyun

**Yönerge**: Azure Anahtar Kasası'nda etkin leştirilmiş kaynakları depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olmak için etiketleri kullanın. 

Azure kaynaklarınızı düzenlemek için etiketleri kullanın:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Hassas bilgileri depolama veya işleme sistemlerini yalıtma

**Kılavuz**: Belirli alt ağlara erişimi kısıtlamak üzere yapılandırılan sanal ağ hizmeti uç noktalarından yararlanarak Azure Anahtar Kasası'na erişimi güvence altına alabilirsiniz.

Güvenlik duvarı kuralları yürürlüğe girsince, Azure Key Vault veri düzlemi işlemlerini yalnızca isteğiniz izin verilen alt ağlardan veya IP adresi aralıklarından geldiğinde gerçekleştirebilirsiniz. Bu, Azure portalındaki Azure Key Vault erişimi için de geçerlidir. Azure portalından önemli bir kasaya göz atabiliyor olsanız da, istemci makineniz izin verilenler listesinde değilse anahtarları, sırları veya sertifikaları listeleyemeyebilirsiniz. Bu, Azure Key Vault Picker ve diğer Azure hizmetlerini de etkiler. Güvenlik duvarı kuralları istemci makinenizin bunu yapmasını engelliyorsa, Anahtar Kasaların listelerini görebilirsiniz, ancak liste anahtarlarını göremeyebilirsiniz.

Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırın:https://docs.microsoft.com/azure/key-vault/key-vault-network-security

Azure Anahtar Kasası için sanal ağ hizmeti bitiş noktaları:https://docs.microsoft.com/azure/key-vault/key-vault-overview-vnet-service-endpoints



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Hassas bilgilerin yetkisiz aktarımının izlenmesi ve engellenmesi

**Kılavuz**: Azure Key Vault'ta depolanan tüm veriler hassas kabul edilir. Azure Key Vault sırlarına erişimi denetlemek için Azure Key Vault veri düzlemi erişim denetimlerini kullanın. Ağ katmanındaki erişimi denetlemek için Key Vault'un yerleşik güvenlik duvarını da kullanabilirsiniz. Azure Key Vault'a erişimi izlemek için, Key Vault Tanı Ayarlarını etkinleştirin ve günlükleri bir Azure Depolama Hesabı veya Log Analytics çalışma alanına gönderin.

Anahtar kasasına güvenli erişim:https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault

Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırın:https://docs.microsoft.com/azure/key-vault/key-vault-network-security

Azure Key Vault günlüğü:https://docs.microsoft.com/azure/key-vault/key-vault-logging



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Tüm hassas bilgileri taşıma sırasında şifreleme

**Yönlendirme**: Kimlik doğrulama, yönetim ve veri düzlemi erişimi için Azure Key Vault'a giden tüm trafik şifrelenir ve HTTPS üzerinden geçer: bağlantı noktası 443. (Ancak, bazen CRL için HTTP [port 80] trafik olacaktır.) 

Güvenlik duvarının arkasındaki Azure Key Vault'a erişin:https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Hassas verileri tanımlamak için etkin bir bulma aracı kullanın

**Rehberlik**: Geçerli değildir; Azure Key Vault'daki tüm veriler (sırlar, anahtarlar ve sertifikalar) hassas kabul edilir.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Kaynaklara erişimi denetlemek için Azure RBAC'ı kullanın

**Kılavuz :** Azure Key Vault örneklerinizin yönetimine ve veri düzlemine güvenli erişim.

Anahtar kasasına güvenli erişim:

https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault


**Azure Güvenlik Merkezi denetimi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Erişim denetimini uygulamak için ana bilgisayar tabanlı veri kaybı önlemeyi kullanın

**Kılavuz**: Microsoft, Azure Key Vault'un altında yatan altyapıyı yönetir ve müşteri verilerinin kaybolmasını veya açığa dökülmesini önlemek için sıkı denetimler uygulamıştır.

Azure Anahtar Kasası nedir?

https://docs.microsoft.com/azure/key-vault/key-vault-overview

Azure müşteri veri koruması:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Hassas bilgileri istirahatte şifreleme

**Kılavuz :** Yönetilen tüm nesneler (anahtar, sertifikalar ve sırlar) Azure Key Vault'ta dinleniyerde şifrelenir.

Destekleyici belgeler:

- [Şifreleme Modeli ve anahtar yönetimi tablosu](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest#encryption-model-and-key-management-table)


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Kritik Azure kaynaklarında yapılan değişiklikleri kaydedin ve uyarı

**Kılavuz**: Azure Key Vault denetim olay günlüğünü incelemek için Azure Monitor'daki Azure Key Vault Analytics çözümünü kullanın.

Azure Monitor'da Azure Key Vault Analytics çözümü:

https://docs.microsoft.com/azure/azure-monitor/insights/azure-key-vault



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için [Güvenlik Denetimi: Güvenlik Açığı Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)bakın.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Otomatik güvenlik açığı tarama araçlarını çalıştırın

**Kılavuz**: Microsoft, Azure Key Vault'u destekleyen temel sistemlerde güvenlik açığı yönetimi gerçekleştirir.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Otomatik işletim sistemi yama yönetimi çözümlerini dağıtma

**Rehberlik**: N/A; Microsoft, Key Vault'u destekleyen temel sistemlerde yama yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Otomatik üçüncü taraf yazılım yama yönetimi çözümlerini dağıtma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Arka arkaya güvenlik açığı taramalarını karşılaştırın

**Kılavuz**: Microsoft, Key Vault'u destekleyen temel sistemlerde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Keşfedilen güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Yönerge**: Azure Güvenlik Merkezi tarafından sağlanan varsayılan risk derecelendirmelerini (Güvenli Puan) kullanın.

Azure Güvenlik Merkezi'nde Güvenli Puanınızı Artırın:

https://docs.microsoft.com/azure/security-center/security-center-secure-score


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için [güvenlik denetimi: Envanter ve Varlık Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)bakın.*

### <a name="61-use-azure-asset-discovery"></a>6.1: Azure Varlık Bulma'yı kullanma

**Yönerge**: Aboneliğinizdeki tüm kaynakları (Azure Anahtar Kasası örnekleri dahil) sorgulamak ve keşfetmek için Azure Kaynak Grafiği'ni kullanın. Kiracınızda uygun (okuma) izinlere sahip olduğundan ve aboneliklerinizdeki tüm Azure aboneliklerinin yanı sıra kaynakları sıralayabildiğinizden emin olun.

Hızlı başlatma: Azure Kaynak Grafiği Gezgini'ni kullanarak ilk Kaynak Grafiği sorgunuzu çalıştırın:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Cari hesabın erişebileceği abonelikleri alın.:

https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure kaynakları için rol tabanlı erişim denetimi (RBAC) nedir?

https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6.2: Varlık meta verilerini koruma

**Yönerge**: Etiketleri, mantıksal olarak taksonomi olarak düzenlemek için meta veriler veren Azure Anahtar Kasası kaynaklarına uygulayın.

Etiketler nasıl oluşturulur ve kullanılır:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Yetkisiz Azure kaynaklarını silme

**Kılavuz**: Azure Anahtar Kasası örneklerini ve ilgili kaynakları düzenlemek ve izlemek için uygun olduğunda etiketleme, yönetim grupları ve ayrı abonelikleri kullanın. Envanteri düzenli olarak uzlaştırın ve yetkisiz kaynakların abonelikten zamanında silindiğinden emin olun.

Ek bir Azure aboneliği oluşturun:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Kaynak organizasyonu ve yönetimi için yönetim grupları oluşturun:

https://docs.microsoft.com/azure/governance/management-groups/create

Azure kaynaklarınızı düzenlemek için etiketleri kullanın:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Onaylanan Azure kaynaklarının ve yazılım başlıklarının envanterini koruyun

**Yönerge**: İşlem kaynaklarınız için onaylanmış Azure kaynaklarının ve onaylı yazılımların listesini tanımlayın

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Onaylanmamış Azure kaynakları için izleme

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure ilkelerini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Ayrıca, abonelik içindeki kaynakları sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın.

Öğretici: Uyumluluğu uygulamak için ilkeler oluşturun ve yönetin:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Hızlı başlatma: Azure Kaynak Grafiği Gezgini'ni kullanarak ilk Kaynak Grafiği sorgunuzu çalıştırın:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Bilgi işlem kaynakları içindeki onaylanmamış yazılım uygulamalarını izleme

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: Geçerli değildir; bu öneri, bir bütün olarak Azure ve hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="68-use-only-approved-applications"></a>6.8: Yalnızca onaylı uygulamaları kullanın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6.9: Yalnızca onaylanmış Azure hizmetlerini kullanma

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure ilkelerini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Öğretici: Uyumluluğu uygulamak için ilkeler oluşturun ve yönetin:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlkesi Örnekleri:https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="610-implement-approved-application-list"></a>6.10: Onaylı başvuru listesini uygulama

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azureresources-manager-via-scripts"></a>6.11: Kullanıcıların komut dosyaları aracılığıyla AzureResources Yöneticisi ile etkileşim deşmelerini sınırlandırın

**Yönerge**: "Microsoft Azure Yönetimi" Uygulaması için "Erişimi engelle" yapılarak kullanıcıların Azure Kaynak Yöneticisi (ARM) ile etkileşim edebilmelerini sınırlamak için Azure Koşullu Erişimi kullanın. Bu, Anahtar Kasa yapılandırması gibi yüksek güvenlik ortamındaki kaynakların oluşturulmasını ve değiştirilmesini engelleyebilir.

Koşullu Erişim ile Azure yönetimine erişimi yönetin:

https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Kullanıcıların bilgi işlem kaynakları içinde komut dosyalarını yürütme yeteneğini sınırlama

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Yüksek riskli uygulamaları fiziksel veya mantıksal olarak ayırmak

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için güvenlik [denetimi: Güvenli Yapılandırma'ya](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)bakın.*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Tüm Azure kaynakları için güvenli yapılandırmalar oluşturma

**Yönerge**: Azure Anahtar Kasası örneklerinizi denetlemek veya yapılandırmayı zorlamak için özel ilkeler oluşturmak için "Microsoft.KeyVault" ad alanında Azure İlkesi takma adlarını kullanın. Azure Anahtar Kasası için yerleşik Azure İlkesi tanımlarını da kullanabilirsiniz:

Key Vault nesneleri kurtarılabilir olmalıdır

Analytics çalışma alanını günlüğe kaydetmek için Anahtar Kasası için Tanılama Ayarlarını Dağıtma

Key Vault'taki tanılama günlükleri etkinleştirilmeli

Key Vault sanal ağ hizmeti bitiş noktası kullanmalıdır

Anahtar Kasası için Tanı Ayarlarını Olay Hub'ına Dağıtma

Azure Anahtar Kasası örnekleriniz için güvenli bir yapılandırma taban çizgisi olarak Azure Güvenlik Merkezi'nden gelen önerileri kullanın.

Kullanılabilir Azure İlkesi Diğer Adları nasıl görüntüleyebilirsiniz:

https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Öğretici: Uyumluluğu uygulamak için ilkeler oluşturun ve yönetin:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Güvenli işletim sistemi yapılandırmaları oluşturma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Güvenli Azure kaynak yapılandırmalarını koruyun

**Yol gösterici**: Azure Tuşu'nu etkinleştiren kaynaklarınızda güvenli ayarları zorlamak için Azure ilkesini [reddet] ve [varsa dağıt] kullanın. 

Öğretici: Uyumluluğu uygulamak için ilkeler oluşturun ve yönetin:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage 

  
Azure İlkesi efektlerini anlayın: 

https://docs.microsoft.com/azure/governance/policy/concepts/effects


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Güvenli işletim sistemi yapılandırmalarını koruyun

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure kaynaklarının yapılandırmayı güvenli bir şekilde depolama

**Kılavuz**: Azure Anahtar Kasası etkin kaynaklarınız için özel Azure İlkesi tanımlarını kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos'u kullanın.

Azure DevOps'lerde kod depolama: 

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops 

Azure Depobelgeleri: 

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Özel işletim sistemi görüntülerini güvenli bir şekilde saklayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Sistem yapılandırma yönetim araçlarını dağıtma

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için özel ilkeler oluşturmak için "Microsoft.KeyVault" ad alanında Azure İlkesi takma adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve ardışık hatlar geliştirin.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: İşletim sistemleri için sistem yapılandırma yönetim araçlarını dağıtma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Azure hizmetleri için otomatik yapılandırma izleme uygulaması

**Kılavuz**: Azure Anahtarı Vault korumalı kaynaklarınız için temel taramaları gerçekleştirmek için Azure Güvenlik Merkezi'ni kullanın 

  

Azure Güvenlik Merkezi'nde öneriler nasıl düzeltilir: 

https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: İşletim sistemleri için otomatik yapılandırma izleme

**Rehberlik**: Geçerli değildir; bu kıyaslama hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7.11: Azure sırlarını güvenli bir şekilde yönetme

**Rehberlik**: Bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvence altına almak için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliğini kullanın. Azure Key Vault yumuşak silme özelliğinin etkin olduğundan emin olun.

Azure Yönetilen Kimliklerle nasıl tümleştirilir:

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Anahtar Kasası nasıl oluşturulur:

https://docs.microsoft.com/azure/key-vault/quick-create-portal

Key Vault kimlik doğrulaması yönetilen bir kimlikle nasıl sağlayacaksınız: 

https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: Bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvence altına almak için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliğini kullanın. 

  

Azure Yönetilen Kimliklerle nasıl tümleştirilir: 

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity  

Anahtar Kasası nasıl oluşturulur: 

https://docs.microsoft.com/azure/key-vault/quick-create-portal    

Key Vault kimlik doğrulaması yönetilen bir kimlikle nasıl sağlayacaksınız:  
https://docs.microsoft.com/azure/key-vault/managed-identity

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

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır. Microsoft, altta yatan platform için kötü amaçlı yazılımdan koruma yı işler.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: İşlem yapmayan Azure kaynaklarına yüklenecek dosyaları önceden taraya

**Yönlendirme**: Azure hizmetlerini destekleyen temel ana bilgisayarda Microsoft kötü amaçlı yazılımdan koruma etkindir (örneğin, Azure Key Vault), müşteri içeriğinde çalışmaz.

Azure Key Vault gibi işlem yapmayan Azure kaynaklarına yüklenen veya gönderilen tüm içerikleri önceden tarayın. Microsoft bu gibi durumlarda verilerinize erişemez.

Azure Bulut Hizmetleri ve Sanal Makineler için Microsoft Kötü Amaçlı Yazılımdan Koruma'yı anlayın:https://docs.microsoft.com/azure/security/fundamentals/antimalware

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Kötü amaçlı yazılımdan koruma yazılımının ve imzalarının güncelleştirilmesini sağlayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır. Microsoft, altta yatan platform için kötü amaçlı yazılımdan koruma yı işler.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Düzenli otomatik yedeklemeler sağlayın

**Kılavuz**: Aşağıdaki PowerShell komutlarıyla Anahtar Kasa Sertifikalarınızın, Anahtarlarınızın, Yönetilen Depolama Hesaplarınızın ve Sırlarınızın düzenli otomatik yedeklemelerini sağlayın:

- Yedekleme-AzKeyVaultCertificate

- Yedekleme-AzKeyVaultKey

- Yedekleme-AzKeyVaultManagedStorageAccount

- Yedekleme-AzKeyVaultSecret

İsteğe bağlı olarak, Key Vault yedeklemelerinizi Azure Yedekleme'de saklayabilirsiniz.

Anahtar Kasa Sertifikaları nasıl yedekilir:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultcertificate

Anahtar Vault Tuşları nasıl yedeklenebilir:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey

Key Vault Yönetilen Depolama Hesapları nasıl yedekilir:https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultmanagedstorageaccount

Anahtar Vault Sırları yedeklemek için nasıl:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultsecret

Azure Yedekleme nasıl etkinleştirilir:https://docs.microsoft.com/azure/backup



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Komple sistem yedeklemelerini gerçekleştirin ve müşteri yönetilen anahtarları yedekleme

**Kılavuz :** Anahtar Kasa Sertifikalarınızın, Anahtarlarınızın, Yönetilen Depolama Hesaplarınızın ve Sırlarınızın yedeklerini aşağıdaki PowerShell komutlarıyla gerçekleştirin:

- Yedekleme-AzKeyVaultCertificate

- Yedekleme-AzKeyVaultKey

- Yedekleme-AzKeyVaultManagedStorageAccount

- Yedekleme-AzKeyVaultSecret

İsteğe bağlı olarak, Key Vault yedeklemelerinizi Azure Yedekleme'de saklayabilirsiniz.

Anahtar Kasa Sertifikaları nasıl yedekilir:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultcertificate

Anahtar Vault Tuşları nasıl yedeklenebilir:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey

Key Vault Yönetilen Depolama Hesapları nasıl yedekilir:https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultmanagedstorageaccount

Anahtar Vault Sırları yedeklemek için nasıl:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultsecret

Azure Yedekleme nasıl etkinleştirilir:https://docs.microsoft.com/azure/backup



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Müşteri yönetilen anahtarlar dahil tüm yedeklemeleri doğrulayın

**Rehberlik**: Anahtar Kasa Sertifikalarınızın, Anahtarlarınızın, Anahtarlarınızın, Yönetilen Depolama Hesaplarınızın ve Sırlarınızın veri geri yüklemesini aşağıdaki PowerShell komutlarıyla düzenli olarak gerçekleştirin:

- Geri Yükleme-AzKeyVaultCertificate

- Geri Yükleme-AzKeyVaultKey

- Geri Yükleme-AzKeyVaultManagedStorageAccount Hesabı

- Geri Yükleme-AzKeyVaultSecret

Anahtar Kasa Sertifikaları nasıl geri yüklenir:https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultcertificate?view=azurermps-6.13.0

Anahtar Kasa Anahtarları nasıl geri yüklenir:https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0 

Key Vault Yönetilen Depolama Hesapları nasıl geri yüklenir:https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultmanagedstorageaccount

Key Vault Secrets nasıl geri yüklenir:https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultsecret?view=azurermps-6.13.0


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Yedeklerin ve müşteri yönetilen anahtarlarının korunmasını sağlayın

**Kılavuz**: Azure Anahtar Kasası için yumuşak silmenin etkinleştirildiğinden emin olun. Yumuşak silme, silinmiş anahtar kasalarının ve anahtarlar, sırlar ve sertifikalar gibi kasa nesnelerinin kurtarılmasına olanak tanır. 

Azure Key Vault'un Yumuşak Silme nasıl kullanılır: 

https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için [Güvenlik Denetimi: Olay Yanıtı'na](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)bakın.*

### <a name="101-create-an-incident-response-guide"></a>10.1: Olay yanıt kılavuzu oluşturma

**Rehberlik**: Kuruluşunuz için bir olay yanıt kılavuzu oluşturun. Personelin tüm rollerini ve olay işleme/yönetiminin algılamadan olay sonrası incelemeye kadar olan aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun. Bu işlemler, Key Vault sırlarını kullananlar gibi hassas sistemleri korumaya odaklanmalıdır.

Azure Güvenlik Merkezi'nde İş Akışı Otomasyonları nasıl yapılandırılabilen: 

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide   

Kendi güvenlik olayı yanıt sürecinizi oluşturma kılavuzu:  

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Microsoft Güvenlik Yanıt Merkezi'nin Bir Olayın Anatomisi:   

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process   

Müşteri ayrıca, kendi olay yanıt planının oluşturulmasına yardımcı olmak için NIST'nin Bilgisayar Güvenliği Olay İşlemleri Kılavuzu'ndan da yararlanabilir: 

https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Olay puanlama ve öncelik lendirme prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, öncelikle hangi uyarıların araştırılması gerektiğine öncelik vermenize yardımcı olmak için her uyarıya önem verir. Önem derecesi, Güvenlik Merkezi'nin bulguda ne kadar emin olduğuna veya uyarıyı vermek için kullanılan analize ve uyarıya yol açan etkinliğin arkasında kötü amaçlı niyet olduğuna dair güven düzeyine dayanır. Ayrıca, abonelikleri açıkça işaretleyin (eski için. azure kaynaklarını, özellikle de Azure Anahtar Kasası sırları gibi hassas verileri işleyenleri net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10.3: Test güvenlik yanıt yordamları

**Kılavuz**: Azure Anahtar Kasası örneklerinizi ve ilgili kaynaklarınızın korunmasına yardımcı olmak için sistemlerinizin olay yanıt yeteneklerini düzenli olarak test etmek için alıştırmalar yapın. Zayıf noktaları ve boşlukları belirleyin ve planı gerektiği gibi gözden geçirin.

NIST'in yayınına bakın: BT Planları ve Yetenekleri için Test, Eğitim ve Egzersiz Programları Kılavuzu: 

https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Güvenlik olayı iletişim bilgilerini sağlayın ve güvenlik olayları için uyarı bildirimlerini yapılandırma

**Kılavuz :** Microsoft Güvenlik Yanıt Merkezi (MSRC) verilerinize yasadışı veya yetkisiz bir taraf tarafından erişildiğini tespit ederse, güvenlik olayı iletişim bilgileri Microsoft tarafından sizinle iletişim kurmak için kullanılır.  Sorunların çözülmesini sağlamak için olaydan sonraki olayları gözden geçirin.

Azure Güvenlik Merkezi Güvenlik Kişisi nasıl ayarlanır:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Olay yanıt sisteminize güvenlik uyarıları dahil edin

**Kılavuz**: Azure Anahtar Vault özellikli kaynaklara yönelik riskleri belirlemeye yardımcı olmak için Sürekli DışAkverme özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışa aktarın. Sürekli Dışa Aktarma, uyarı ve önerileri el ile veya sürekli olarak dışa aktarmanıza olanak tanır.  Uyarıları Azure Sentinel'e aktarmak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz. 

 

Sürekli dışa aktarma nasıl yapılandırılabilen: 

https://docs.microsoft.com/azure/security-center/continuous-export 

  

Uyarıları Azure Sentinel'e nasıl aktarın: 

https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Güvenlik uyarılarına yanıtı otomatikleştirin

**Kılavuz**: Azure Key Vault korumalı kaynaklarınızı korumak için güvenlik uyarıları ve önerilerinde "Mantık Uygulamaları" aracılığıyla yanıtları otomatik olarak tetiklemek için Azure Güvenlik Merkezi'ndeki İş Akışı Otomasyonu özelliğini kullanın. 

 

İş Akışı Otomasyonu ve Mantık Uygulamaları nasıl yapılandırılabilen: 

https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için [Güvenlik Kontrolü: Penetrasyon Testleri ve Kırmızı Takım Egzersizleri'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises)bakın.*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1: Azure kaynaklarınızı düzenli olarak delme testini gerçekleştirin ve tüm kritik güvenlik bulgularının 60 gün içinde düzeltilmesini sağlayın

**Kılavuz**: Azure Key Vault hizmetinde doğrudan kalem testi yapmamanız gerekir, ancak sırların güvenliğini sağlamak için Key Vault'u kullanan Azure kaynaklarınızı test etmeniz tavsiye edilir.

Penetrasyon Testlerinizin Microsoft ilkelerini ihlal etmediğini sağlamak için Microsoft Etkileşim Kuralları'na uymanız gerekir:

https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

Microsoft'un Stratejisi ve Red Teaming ve Microsoft tarafından yönetilen bulut altyapısı, hizmetler ve uygulamalara karşı canlı site penetrasyon testi nin uygulanması hakkında daha fazla bilgiyi burada bulabilirsiniz: 

https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Güvenlik Kıyaslama'ya](https://docs.microsoft.com/azure/security/benchmarks/overview) bakın
- [Azure Güvenlik Taban Çizgileri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
