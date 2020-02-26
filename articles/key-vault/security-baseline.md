---
title: Key Vault için Azure Güvenlik temeli
description: Key Vault için Azure Güvenlik temeli
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 02/22/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 9b767693691557f684bee59aa1764395dc42bffe
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77590045"
---
# <a name="azure-security-baseline-for-key-vault"></a>Key Vault için Azure Güvenlik temeli

Key Vault için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetlerin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: Azure Key Vault Azure özel bağlantısı ile tümleştirin. Azure özel bağlantı hizmeti, Azure hizmetlerine (örneğin, Azure Key Vault) ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine sanal ağınızdaki özel bir uç nokta üzerinden erişmenizi sağlar.
Azure özel uç noktası, Azure özel bağlantısı tarafından desteklenen bir hizmete özel ve güvenli bir şekilde bağlanan bir ağ arabirimidir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor. Hizmete giden tüm trafik özel uç nokta aracılığıyla yönlendirilebilir, bu nedenle ağ geçitleri, NAT cihazları, ExpressRoute veya VPN bağlantıları ya da genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağı örneğine bağlanarak, erişim denetimi için en yüksek düzeyde ayrıntı düzeyi sağlayabilirsiniz.

Key Vault Azure özel bağlantısı ile tümleştirme: https://docs.microsoft.com/azure/key-vault/private-link-service

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Kılavuz**: Azure Güvenlik Merkezi 'ni kullanın ve azure 'da Key Vault yapılandırılmış kaynaklarınızın güvenliğini sağlamaya yardımcı olmak için ağ koruma önerilerini izleyin. 


Azure Güvenlik Merkezi tarafından sunulan ağ güvenliği hakkında daha fazla bilgi için: 

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: dağıtılmış hizmet reddi saldırılarına karşı koruma için Key Vault örneklarınızla Ilişkili Azure sanal ağlarında Azure DDoS koruma standardı 'nı etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

 
Azure Güvenlik Merkezi 'nde Azure hizmet katmanı için Azure portal: https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection tehdit algılamayı kullanarak Azure DDoS koruması standardını yönetin: https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Rehberlik**: Azure Key Vault Azure Key Vault için ağ güvenlik grupları (NSG) ve akış günlükleri kullanmaz. Bunun yerine, Azure Key Vault örneklerinizi güvenli hale getirmek ve ölçümleri ve denetim olaylarını kaydetmek için tanılama ayarlarını etkinleştirmek üzere Azure özel bağlantısı 'nı kullanın.
Key Vault Azure özel bağlantısıyla tümleştirin: https://docs.microsoft.com/azure/key-vault/private-link-service

Günlüğe kaydetme Azure Key Vault: https://docs.microsoft.com/azure/key-vault/key-vault-logging


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: Bu gereksinim, Azure Key Vault için Gelişmiş tehdit KORUMASı (ATP) yapılandırılarak karşılanır. ATP ek bir güvenlik zekası katmanı sağlar. Bu araç Azure Key Vault hesaplara erişmeye veya açıktan yararlanmaya yönelik olabilecek zararlı girişimleri algılar.


Azure Güvenlik Merkezi anormal etkinlik algıladığında uyarıları görüntüler. Ayrıca, şüpheli etkinliğin ayrıntılarını ve belirlenen tehditleri araştırıp düzeltme önerilerini içeren abonelik yöneticisine e-posta gönderir.


Azure Key Vault için Gelişmiş tehdit koruması ayarlama: https://docs.microsoft.com/azure/security-center/advanced-threat-protection-key-vault


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: Azure Key Vault örneklerine erişmesi gereken kaynaklar için, ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak üzere Azure Key Vault için Azure hizmet etiketleri kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.


Azure hizmet etiketlerine genel bakış:

https://docs.microsoft.com/azure/virtual-network/service-tags-overview

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure ilkesiyle Azure Key Vault örneklarınızla ilişkili ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure Key Vault örneklerinizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Keykasası" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. Ayrıca, Azure Key Vault ile ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz, örneğin:


Key Vault bir sanal ağ hizmeti uç noktası kullanmalıdır


Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


Azure Ilke örnekleri:

https://docs.microsoft.com/azure/governance/policy/samples/#networ

Hızlı başlangıç: portalda bir şema tanımlama ve atama:

https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: meta veri ve mantıksal kuruluş sağlamak üzere Azure Key Vault örneklerinizin ağ güvenliği ve trafik akışı ile ilgili kaynaklar için Etiketler kullanın.


Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.


Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynakları arayabilir veya bunlarla ilgili eylemler gerçekleştirebilirsiniz.


Azure kaynaklarınızı düzenlemek için etiketleri kullanın:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını izleyin ve Azure Key Vault örneklerinizin ilişkili olduğu ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

Azure etkinlik günlüğü olaylarını görüntüleyin ve alın:

https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view


Azure Izleyici 'yi kullanarak etkinlik günlüğü uyarıları oluşturun, görüntüleyin ve yönetin:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: uygulanamaz; Microsoft, günlüklerdeki zaman damgaları için Azure Key Vault gibi Azure kaynakları için kullanılan saat kaynağını korur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: Azure Key Vault tarafından oluşturulan güvenlik verilerini toplamak Için Azure izleyici aracılığıyla günlükleri alma. Azure Izleyici 'de Azure Log Analytics çalışma alanını kullanarak Analytics 'i sorgulayın ve gerçekleştirin ve uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın. Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz. 

Günlüğe Azure Key Vault:

https://docs.microsoft.com/azure/key-vault/key-vault-logging

Hızlı başlangıç: yerleşik Azure Sentinel:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: denetim, güvenlik ve tanılama günlüklerine erişim için Azure Key Vault örneklerinizin tanılama ayarlarını etkinleştirin. Otomatik olarak kullanılabilen etkinlik günlükleri Olay kaynağını, tarihi, kullanıcıyı, zaman damgasını, kaynak adreslerini, hedef adreslerini ve diğer yararlı öğeleri içerir.

Günlüğe Azure Key Vault:

https://docs.microsoft.com/azure/key-vault/key-vault-logging

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, Azure Key Vault günlüklerinizi tutmak için kullanılan Log Analytics çalışma alanı için, saklama süresini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

Veri saklama süresini değiştirin: https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: anormal davranışlar için günlükleri çözümleyin ve izleyin ve Azure Key Vault korumalı kaynaklarınızın sonuçlarını düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics çalışma alanını kullanın. Alternatif olarak, Azure Sentinel 'e veya üçüncü taraf SıEM 'ye yönelik verileri etkinleştirebilir. 


Hızlı başlangıç: yerleşik Azure Sentinel: https://docs.microsoft.com/azure/sentinel/quickstart-onboard


Azure Izleyici 'de Log Analytics kullanmaya başlayın: https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Izleyici 'de günlük sorgularını kullanmaya başlama: https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Kılavuz**: Azure Güvenlik merkezi 'nde Key Vault için Gelişmiş tehdit koruması 'Nı (ATP) etkinleştirin. Azure Key Vault tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Log Analytics çalışma alanınızı Azure Sentinel 'e ekleyin. Bu, bir güvenlik Orchestration otomatik yanıtı (SOAR) çözümü sağlar. Bu, güvenlik sorunlarını gidermek için PlayBook 'ları (otomatikleştirilmiş çözümlerin) oluşturulmasına ve kullanılmasına olanak tanır.

Hızlı başlangıç: yerleşik Azure Sentinel:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama:

https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts


Azure Izleyici uyarıları ile olaylara yanıt verme:

https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; Azure Key Vault kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Azure Key Vault DNS ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure Active Directory kayıtlı uygulamalarınızın envanterini ve Azure Key Vault anahtarlarına, sırlarınıza ve sertifikalara erişimi olan tüm Kullanıcı hesaplarını sürdürün. Key Vault erişimi sorgulamak ve mutabık kılmak için Azure portal ya da PowerShell kullanabilirsiniz. PowerShell 'deki erişimi görüntülemek için aşağıdaki komutu kullanın:


(Get-AzResource-RESOURCEID [Keyvaultresourceıd]). Properties. AccessPolicies

Bir uygulamayı Azure Active Directory kaydetme:

https://docs.microsoft.com/azure/key-vault/key-vault-manage-with-cli2#registering-an-application-with-azure-active-directory

Anahtar kasasına güvenli erişim:

https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: uygulanamaz; Azure Key Vault, kimlik doğrulama Active Directory tarafından sağlandığı ve rol tabanlı erişim denetimiyle güvenli hale getirildiği için varsayılan parola kavramına sahip değildir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: Azure Key Vault örneklerinizin erişimine sahip adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Etkin yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi (Şu anda önizleme aşamasında) kullanın.


Kimlik ve erişimi izleme (Önizleme):

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: uygulamanızın kimliğini sorunsuzca doğrulamak ve Azure Key Vault gizli dizilerinize erişmek için kullanılacak belirteci almak için AppID, Tenantıd ve ClientSecret ile birlikte bir Azure hizmet sorumlusu kullanın.


.NET kullanarak Azure Key Vault için hizmetten hizmete kimlik doğrulaması:

https://docs.microsoft.com/azure/key-vault/service-to-service-authentication


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Active Directory Multi-Factor Authentication etkinleştirin ve Olay Hub 'ı özellikli kaynaklarınızın korunmasına yardımcı olmak Için Azure Güvenlik Merkezi kimlik ve erişim yönetimi (Şu anda önizleme aşamasında) önerilerini izleyin.


Bulut tabanlı bir Azure Multi-Factor Authentication dağıtımı planlama:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted


Kimlik ve erişimi izleme (Önizleme):

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Rehberlik**: Key Vault etkin kaynakları açmak ve yapılandırmak Için yapılandırılmış Azure MULTI-Factor AUTHENTICATION (MFA) Ile ayrıcalıklı erişim iş istasyonu (Paw) kullanın.


Ayrıcalıklı erişim Iş Istasyonları: https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations 

Bulut tabanlı bir Azure Multi-Factor Authentication dağıtımı planlama: https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik oluştuğunda günlükler ve uyarılar oluşturmak için Azure ACTIVE DIRECTORY (AAD) PRIVILEGED IDENTITY Management (PIM) kullanın. Riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için AAD risk algılamalarını kullanın. Ek günlük kaydı için Azure Güvenlik Merkezi risk algılama uyarılarını Azure Izleyicisine gönderin ve eylem gruplarını kullanarak özel uyarı/bildirim yapılandırın.


Şüpheli etkinlik uyarıları oluşturmak için Azure Key Vault için Gelişmiş tehdit koruması 'nı (ATP) etkinleştirin.


Dağıtım Azure AD Privileged Identity Management (PıM):

https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan

Azure Key Vault için Gelişmiş tehdit koruması ayarlama (Önizleme): https://docs.microsoft.com/azure/security-center/advanced-threat-protection-key-vault


Azure Key Vault için uyarılar (Önizleme):

https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurekv


Risk algılamalarını Azure Active Directory:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events


Azure portal eylem grupları oluşturun ve yönetin:

https://docs.microsoft.com/azure/azure-monitor/platform/action-groups

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: koşullu erişim ilkesinin konum koşulunu yapılandırın ve adlandırılmış konumlarınızı yönetin. Adlandırılmış konumlarla, IP adresi aralıkları veya ülkeler ve bölgeler için mantıksal gruplandırmaları oluşturabilirsiniz. Key Vault gizli dizileri gibi hassas kaynaklarla erişimi, yapılandırılmış adlandırılmış konumlara kısıtlayabilirsiniz.

Koşullu erişim Azure Active Directory konum koşulu nedir?: https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: Key Vault gibi Azure kaynakları için merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (AAD) kullanın. Bu, gizli kaynakları yönetmek için rol tabanlı erişim denetimi (RBAC) sağlar.

Hızlı başlangıç: Azure Active Directory yeni bir kiracı oluşturun: https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure Key Vault yönetici rollerine sahip eski hesapları bulmaya yardımcı olması için Azure ACTIVE DIRECTORY (AAD) günlüklerini inceleyin. Ayrıca, grup üyeliklerini verimli bir şekilde yönetmek için AAD erişim incelemelerini kullanın, Azure Key Vault erişmek için kullanılabilecek kurumsal uygulamalara erişin ve rol atamalarına erişin. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişiminin her 90 gün gibi düzenli aralıklarla gözden geçirilmesi gerekir.


Raporları ve izleme belgelerini Azure Active Directory: https://docs.microsoft.com/azure/active-directory/reports-monitoring/


Azure AD erişim gözden geçirmeleri nelerdir?: https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Rehberlik**: tüm günlükleri bir Log Analytics çalışma alanına göndererek Azure Key Vault ve Azure Active Directory için tanılama ayarlarını etkinleştirin. Log Analytics içindeki istenen uyarıları (devre dışı parolalara erişim girişimleri gibi) yapılandırın.

Azure AD günlüklerini Azure Izleyici günlükleriyle tümleştirme: https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

Eski Key Vault çözümden geçiş: https://docs.microsoft.com/azure/azure-monitor/insights/azure-key-vault#migrating-from-the-old-key-vault-solution       

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: Azure Key Vault korumalı kaynaklarınızla ilişkili şüpheli eylemleri algılanan otomatik yanıtları yapılandırmak Için Azure Active Directory kimlik koruması ve risk algılama özelliklerini kullanın. Kuruluşunuzun güvenlik yanıtlarını uygulamak için Azure Sentinel aracılığıyla otomatikleştirilmiş yanıtları etkinleştirmeniz gerekir.


Azure Active Directory portalında riskli oturum açma işlemleri raporu: https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins 

Nasıl yapılır: risk ilkelerini yapılandırma ve etkinleştirme: https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies


Azure Sentinel 'i ekleme:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: uygulanamaz; Azure Key Vault için Müşteri Kasası desteklenmez.

Genel kullanıma yönelik desteklenen hizmetler ve senaryolar: https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: Azure Key Vault etkinleştirilmiş kaynaklarda hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.


Azure kaynaklarınızı düzenlemek için etiketleri kullanma: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: belirli alt ağlara erişimi kısıtlamak için yapılandırılmış sanal ağ hizmeti uç noktaları kullanarak Azure Key Vault erişimi güvenli hale getirebilirsiniz.


Güvenlik duvarı kuralları etkin olduktan sonra, isteğiniz izin verilen alt ağlardan veya IP adresi aralıklarından kaynaklanıyorsa yalnızca Azure Key Vault veri düzlemi işlemleri gerçekleştirebilirsiniz. Bu, Azure portal Azure Key Vault erişim için de geçerlidir. Azure portal bir anahtar kasasına gözatabilseniz de, istemci makineniz izin verilenler listesinde değilse anahtarları, parolaları veya sertifikaları listelemeyebilirsiniz. Bu ayrıca Azure Key Vault seçiciyi ve diğer Azure hizmetlerini de etkiler. Güvenlik duvarı kuralları istemci makinenizin bunu yapmasını engelliyorsa, anahtar kasalarının listelerini görebilirsiniz, ancak liste anahtarlarını göremezsiniz.


Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma: https://docs.microsoft.com/azure/key-vault/key-vault-network-security

Azure Key Vault için sanal ağ hizmet uç noktaları: https://docs.microsoft.com/azure/key-vault/key-vault-overview-vnet-service-endpoints

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: Azure Key Vault içinde depolanan tüm veriler hassas olarak kabul edilir. Azure Key Vault gizli dizi erişimini denetlemek için Azure Key Vault veri düzlemi erişim denetimleri kullanın. Ayrıca, Ağ katmanında erişimi denetlemek için Key Vault yerleşik güvenlik duvarını da kullanabilirsiniz. Azure Key Vault erişimi izlemek için, Key Vault tanılama ayarlarını etkinleştirin ve günlükleri bir Azure depolama hesabına veya Log Analytics çalışma alanına gönderin.

Anahtar kasasına güvenli erişim: https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault

Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma: https://docs.microsoft.com/azure/key-vault/key-vault-network-security

Günlüğe kaydetme Azure Key Vault: https://docs.microsoft.com/azure/key-vault/key-vault-logging

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: kimlik doğrulama, yönetim ve veri düzlemi erişimi için Azure Key Vault giden tüm trafik ŞIFRELENIR ve https: bağlantı noktası 443 üzerinden geçer. (Ancak, CRL için HTTP [port 80] trafiği zaman zaman olacaktır.)


Güvenlik duvarının arkasındaki Azure Key Vault erişim: https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: uygulanamaz; Azure Key Vault (gizli diziler, anahtarlar ve sertifikalar) içindeki tüm veriler hassas olarak değerlendirilir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="46-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,6: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: Azure Key Vault örneklerinizin yönetimine ve veri düzlemine güvenli erişim.


Anahtar kasasına güvenli erişim:

https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: Microsoft, Azure Key Vault için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.


Azure Key Vault nedir? https://docs.microsoft.com/azure/key-vault/key-vault-overview

Azure müşteri verileri koruması: https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: tüm yönetilen nesneler (anahtar, sertifikalar ve gizlilikler) Azure Key Vault Rest 'te şifrelenir.


Azure Key Vault için güvenlik denetimleri: https://docs.microsoft.com/azure/key-vault/key-vault-security-controls


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure izleyici 'de Azure Key Vault Analytics çözümünü kullanarak Azure Key Vault denetim olay günlüklerini gözden geçirin.

Azure Izleyici 'de Azure Key Vault Analytics çözümü: https://docs.microsoft.com/azure/azure-monitor/insights/azure-key-vault


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: Microsoft, Azure Key Vault destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: yok; Microsoft, Key Vault destekleyen temel sistemlerde Düzeltme Eki Yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: Microsoft, Key Vault destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Kılavuz**: Azure Güvenlik Merkezi tarafından sunulan varsayılan risk derecelendirmelerini (güvenli puan) kullanın.

Azure Güvenlik Merkezi 'nde güvenli puanınızı geliştirme:

https://docs.microsoft.com/azure/security-center/security-center-secure-score

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: aboneliğinizdeki tüm kaynakları (Azure Key Vault örnekleri dahil) sorgulamak ve bulmayı sağlamak Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun.

Hızlı başlangıç: Azure Kaynak Grafiği gezginini kullanarak ilk kaynak grafik sorgunuzu çalıştırın: https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Geçerli hesabın erişebileceği abonelikler alın.: https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure kaynakları için rol tabanlı erişim denetimi (RBAC) nedir? https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: mantıksal olarak bunları bir taksonomi halinde düzenlemek için meta veriler sağlayan Azure Key Vault kaynaklarına Etiketler uygulayın.


Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure Key Vault örnekleri ve ilgili kaynakları düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.


Ek bir Azure aboneliği oluşturun:

https://docs.microsoft.com/azure/billing/billing-create-subscription


Kaynak organizasyonu ve yönetimi için yönetim grupları oluşturma:

https://docs.microsoft.com/azure/governance/management-groups/create


Azure kaynaklarınızı düzenlemek için etiketleri kullanma: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini koruyun

**Rehberlik**: işlem kaynaklarınız Için onaylanan Azure kaynakları ve onaylanan yazılımın listesini tanımlama

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkelerini kullanın:

- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Ayrıca, Azure Kaynak grafiğini kullanarak abonelikler içindeki kaynakları sorgular/bulur.

Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Hızlı başlangıç: Azure Kaynak Grafiği gezginini kullanarak ilk kaynak grafik sorgunuzu çalıştırın: https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: uygulanamaz; Bu öneri, Azure için bir bütün olarak ve işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkelerini kullanın:

- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilke örnekleri: https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azureresources-manager-via-scripts"></a>6,11: kullanıcıların betikler aracılığıyla AzureResources Manager ile etkileşim kurma yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimini kullanın. Bu, Key Vault yapılandırma ile ilgili olanlar gibi yüksek bir güvenlik ortamındaki kaynaklarda oluşturma ve değişikliklere engel olabilir.

Azure yönetimine erişimi koşullu erişimle yönetme: https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

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

**Rehberlik**: Azure Key Vault örneklerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. keykasası" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, Azure Key Vault için yerleşik Azure Ilke tanımlarını da kullanabilirsiniz:

- Key Vault nesneler kurtarılabilir olmalıdır
- Log Analytics çalışma alanına Key Vault için tanılama ayarlarını dağıt
- Key Vault tanılama günlükleri etkinleştirilmelidir
- Key Vault bir sanal ağ hizmeti uç noktası kullanmalıdır
- Olay Hub 'ına Key Vault için tanılama ayarlarını dağıtma

Azure Güvenlik Merkezi 'ndeki önerileri, Azure Key Vault örneklerinizin güvenli bir yapılandırma temeli olarak kullanın.

Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme: https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Rehberlik**: Azure Key Vault etkinleştirilmiş kaynaklarınız genelinde güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yok] kullanın. 


Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage 


  
Azure Ilke efektlerini anlayın: 

https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Rehberlik**: Azure Key Vault etkinleştirilmiş kaynaklarınız Için özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure DevOps/depolarını kullanın.


Azure DevOps 'da kod depolama: 

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops 


Azure Repos belgeleri: 

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. keykasası" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.



Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

**Rehberlik**: Azure Key Vault korumalı kaynaklarınız için temel taramalar gerçekleştirmek üzere Azure Güvenlik Merkezi 'ni kullanma 

  

Azure Güvenlik Merkezi 'nde öneriler nasıl düzeltileceği: 

https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu kıyaslama, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvenli hale getirmek için Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın. Azure Key Vault geçici silmenin etkinleştirildiğinden emin olun.


Azure yönetilen kimliklerle tümleştirme:

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity


Key Vault oluşturma:

https://docs.microsoft.com/azure/key-vault/quick-create-portal


Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama: 

https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvenli hale getirmek için Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın. 

  

Azure yönetilen kimliklerle tümleştirme: 

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity  



Key Vault oluşturma: 

https://docs.microsoft.com/azure/key-vault/quick-create-portal    

Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama:  
https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.  
  
 Kimlik bilgileri tarayıcısını ayarlama: https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft, temel alınan platform için kötü amaçlı yazılımdan koruma uygular.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayar üzerinde etkinleştirilmiştir (örneğin, Azure Key Vault), ancak müşteri içeriği üzerinde çalışmaz.


Karşıya yüklenen veya Azure Key Vault gibi işlem dışı Azure kaynaklarına gönderilen tüm içerikleri önceden tarayın. Microsoft bu örneklerdeki verilerinize erişemez.


Azure Cloud Services ve sanal makineler için Microsoft Antimalware 'i anlayın: https://docs.microsoft.com/azure/security/fundamentals/antimalware

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft, temel alınan platform için kötü amaçlı yazılımdan koruma uygular.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: aşağıdaki PowerShell komutlarıyla, Key Vault sertifikalarınızın, anahtarlarının, yönetilen depolama hesaplarınızın ve gizli dizilerinizde düzenli otomatik yedeklemelere sahip olun:

- Backup-AzKeyVaultCertificate
- Backup-AzKeyVaultKey
- Backup-AzKeyVaultManagedStorageAccount
- Backup-AzKeyVaultSecret

İsteğe bağlı olarak, Key Vault yedeklerinizi Azure Backup içinde depolayabiliriz.

Key Vault sertifikalarını yedekleme: https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultcertificate

Key Vault anahtarlarını yedekleme: https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey

Key Vault yönetilen depolama hesaplarını yedekleme: https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultmanagedstorageaccount

Key Vault gizli dizileri yedekleme: https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultsecret

Azure Backup etkinleştirme: https://docs.microsoft.com/azure/backup

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: aşağıdaki PowerShell komutlarıyla Key Vault sertifikalarınızı, anahtarlarınızı, yönetilen depolama hesaplarınızı ve gizli dizileri gerçekleştirin:

- Backup-AzKeyVaultCertificate
- Backup-AzKeyVaultKey
- Backup-AzKeyVaultManagedStorageAccount
- Backup-AzKeyVaultSecret

İsteğe bağlı olarak, Key Vault yedeklerinizi Azure Backup içinde depolayabiliriz.

Key Vault sertifikalarını yedekleme: https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultcertificate

Key Vault anahtarlarını yedekleme: https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey

Key Vault yönetilen depolama hesaplarını yedekleme: https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultmanagedstorageaccount

Key Vault gizli dizileri yedekleme: https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultsecret

Azure Backup etkinleştirme: https://docs.microsoft.com/azure/backup

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: aşağıdaki PowerShell komutlarıyla, Key Vault sertifikalarınızı, anahtarlarınızı, yönetilen depolama hesaplarınızı ve gizli dizileri düzenli aralıklarla geri yükleme işlemi gerçekleştirin:

- Restore-AzKeyVaultCertificate
- Restore-AzKeyVaultKey
- Restore-AzKeyVaultManagedStorageAccount
- Restore-AzKeyVaultSecret

Key Vault sertifikalarını geri yükleme: https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultcertificate?view=azurermps-6.13.0

Key Vault anahtarlarını geri yükleme: https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0 

Key Vault yönetilen depolama hesaplarını geri yükleme: https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultmanagedstorageaccount

Key Vault gizli dizileri geri yükleme: https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultsecret?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: Azure Key Vault için geçici silmenin etkinleştirildiğinden emin olun. Geçici silme, silinen anahtar kasalarının ve anahtar, gizli dizi ve sertifika gibi kasa nesnelerinin kurtarılmasına olanak tanır. 

Azure Key Vault 'nin geçici silme kullanımı: 

https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun. Bu işlemlerin, Key Vault gizli dizileri kullanma gibi hassas sistemleri korumasıyla bir odağa sahip olması gerekir.



Azure Güvenlik Merkezi 'nde Iş akışı Otomatiklamalar nasıl yapılandırılır: 

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide   



Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu:  

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/



Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın Anatomisi:   

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process   



Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu de kullanabilir: 

https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır. Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için, özellikle de Azure Key Vault gizli dizileri gibi hassas verileri işlemek üzere bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: Azure Key Vault örneklerinizin ve ilgili kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek üzere alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.


NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin: 

https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.  Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.



Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: Azure Key Vault etkinleştirilmiş kaynaklardaki riskleri belirlemenize yardımcı olması Için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar.  Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz. 

 

Sürekli dışarı aktarmayı yapılandırma: 

https://docs.microsoft.com/azure/security-center/continuous-export 

  

Uyarıları Azure Sentinel 'e aktarma: 

https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın ve Azure Key Vault korumalı kaynaklarınızı korumaya yönelik öneriler alın. 

 

Iş akışı otomasyonunu yapılandırma ve Logic Apps: 

https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve 60 gün içinde tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Azure Key Vault hizmetinde doğrudan kalem testi gerçekleştirmekten, ancak gizli dizi güvenliğini sağlamak için Key Vault kullanan Azure kaynaklarınızı test etmeniz önerilir.


Sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak için Microsoft katılım kurallarını izlemeniz gerekir:

https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1


Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz: 

https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

