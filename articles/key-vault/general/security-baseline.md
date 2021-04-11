---
title: Key Vault için Azure Güvenlik temeli
description: Key Vault güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: key-vault
ms.topic: conceptual
ms.date: 03/29/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 305fdd3a0b8e0c876924c6e1f090424e67571af0
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105968722"
---
# <a name="azure-security-baseline-for-key-vault"></a>Key Vault için Azure Güvenlik temeli

Bu güvenlik temeli, Key Vault için [Azure Güvenlik kıyaslama sürüm 1,0](../../security/benchmarks/overview-v1.md) ' den rehberlik uygular. Azure Güvenlik Karşılaştırması, Azure üzerindeki bulut çözümlerinizin güvenliğini sağlamaya yönelik öneriler sunar. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimlerine** ve Key Vault için geçerli olan ilgili kılavuza göre gruplandırılır. Key Vault için geçerli olmayan **denetimler** veya Microsoft 'un hangi sorumluluğun çıkarılmadığı.

Key Vault Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam Key Vault güvenlik temeli eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Ağ Güvenliği](../../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

**Rehberlik**: Azure Key Vault Azure özel bağlantısı ile tümleştirin. Azure özel bağlantı hizmeti, Azure hizmetlerine (örneğin, Azure Key Vault) ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine sanal ağınızdaki özel bir uç nokta üzerinden erişmenizi sağlar.

Azure özel uç noktası, Azure özel bağlantısı tarafından desteklenen bir hizmete özel ve güvenli bir şekilde bağlanan bir ağ arabirimidir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor. Hizmete giden tüm trafik özel uç nokta aracılığıyla yönlendirilebilir, bu nedenle ağ geçitleri, NAT cihazları, ExpressRoute veya VPN bağlantıları ya da genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağı örneğine bağlanarak, erişim denetimi için en yüksek düzeyde ayrıntı düzeyi sağlayabilirsiniz.

- [Azure özel bağlantısı ile Key Vault tümleştirme](/azure/key-vault/private-link-service)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: [Azure Güvenlik kıyaslaması](/azure/governance/policy/samples/azure-security-benchmark) , Güvenlik Merkezi için varsayılan Ilke girişimidir ve [Güvenlik Merkezi 'nin önerilerine](/azure/security-center/security-center-recommendations)yöneliktir. Bu denetimle ilgili Azure Ilke tanımları, Güvenlik Merkezi tarafından otomatik olarak etkinleştirilir. Bu denetimle ilgili uyarılar, ilgili hizmetler için bir [Azure Defender](/azure/security-center/azure-defender) planı gerektirebilir.

**Azure ilkesi yerleşik tanımları-Microsoft. Keykasası**:

[!INCLUDE [Resource Policy for Microsoft.KeyVault 1.1](../../../includes/policy/standards/asb/rp-controls/microsoft.keyvault-1-1.md)]

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1,2: sanal ağların, alt ağların ve ağ arabirimlerinin yapılandırma ve trafiğini izleme ve günlüğe kaydetme

**Kılavuz**: Azure Güvenlik Merkezi 'ni kullanın ve azure 'da Key Vault yapılandırılmış kaynaklarınızın güvenliğini sağlamaya yardımcı olmak için ağ koruma önerilerini izleyin. 

- [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliği hakkında daha fazla bilgi için](../../security-center/security-center-network-recommendations.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: dağıtılmış hizmet reddi saldırılarına karşı koruma için Key Vault örneklarınızla Ilişkili Azure sanal ağlarında Azure DDoS koruma standardı 'nı etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

 
- [Azure portal kullanarak Azure DDoS koruması standardını yönetme](/azure/virtual-network/manage-ddos-protection)

- [Azure Güvenlik Merkezi 'nde Azure hizmet katmanı için tehdit algılama](/azure/security-center/security-center-alerts-service-layer)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: Bu gereksinim, Azure Key Vault için Gelişmiş tehdit KORUMASı (ATP) yapılandırılarak karşılanır. ATP ek bir güvenlik zekası katmanı sağlar. Bu araç Azure Key Vault hesaplara erişmeye veya açıktan yararlanmaya yönelik olabilecek zararlı girişimleri algılar.

Azure Güvenlik Merkezi anormal etkinlik algıladığında uyarıları görüntüler. Ayrıca, şüpheli etkinliğin ayrıntılarını ve belirlenen tehditleri araştırıp düzeltme önerilerini içeren abonelik yöneticisine e-posta gönderir.

- [Azure Key Vault için gelişmiş tehdit koruması ayarlama](/azure/security-center/advanced-threat-protection-key-vault)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: Azure Key Vault örneklerine erişmesi gereken kaynaklar için, ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak üzere Azure Key Vault için Azure hizmet etiketleri kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

- [Azure hizmet etiketlerine genel bakış](../../virtual-network/service-tags-overview.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure ilkesiyle Azure Key Vault örneklarınızla ilişkili ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure Key Vault örneklerinizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Keykasası" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. Ayrıca, Azure Key Vault ile ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz, örneğin:
- Key Vault bir sanal ağ hizmeti uç noktası kullanmalıdır

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:

- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

- [Azure İlkesi Örnekleri](/azure/governance/policy/samples)

- [Portalda bir şema tanımlama ve atama](../../governance/blueprints/create-blueprint-portal.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: meta veri ve mantıksal kuruluş sağlamak üzere Azure Key Vault örneklerinizin ağ güvenliği ve trafik akışı ile ilgili kaynaklar için Etiketler kullanın.

Tüm kaynakların etiketlerle oluşturulduğundan ve etiketlenmemiş kaynakları bilgilendirdiğinden emin olmak için etiketlemeyle ilgili yerleşik Azure Ilke tanımlarını kullanın ("etiket ve onun değeri gerektir" gibi).

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynakları arayabilir veya bunlarla ilgili eylemler gerçekleştirebilirsiniz.

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını izleyin ve Azure Key Vault örneklerinizin ilişkili olduğu ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

- [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](/azure/azure-monitor/platform/activity-log-view)

- [Azure Izleyici 'yi kullanarak etkinlik günlüğü uyarıları oluşturma, görüntüleme ve yönetme](/azure/azure-monitor/platform/alerts-activity-log)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: günlüğe kaydetme ve izleme](../../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: Azure Key Vault tarafından oluşturulan güvenlik verilerini toplamak Için Azure izleyici aracılığıyla günlükleri alma. Azure Izleyici 'de Azure Log Analytics çalışma alanını kullanarak Analytics 'i sorgulayın ve gerçekleştirin ve uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın. Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz. 

- [Azure Key Vault günlüğü](/azure/key-vault/key-vault-logging)

- [Hızlı başlangıç: Pano içi Azure Sentinel](../../sentinel/quickstart-onboard.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: denetim, güvenlik ve tanılama günlüklerine erişim için Azure Key Vault örneklerinizin tanılama ayarlarını etkinleştirin. Otomatik olarak kullanılabilen etkinlik günlükleri Olay kaynağını, tarihi, kullanıcıyı, zaman damgasını, kaynak adreslerini, hedef adreslerini ve diğer yararlı öğeleri içerir.

- [Azure Key Vault günlüğü](/azure/key-vault/key-vault-logging)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: [Azure Güvenlik kıyaslaması](/azure/governance/policy/samples/azure-security-benchmark) , Güvenlik Merkezi için varsayılan Ilke girişimidir ve [Güvenlik Merkezi 'nin önerilerine](/azure/security-center/security-center-recommendations)yöneliktir. Bu denetimle ilgili Azure Ilke tanımları, Güvenlik Merkezi tarafından otomatik olarak etkinleştirilir. Bu denetimle ilgili uyarılar, ilgili hizmetler için bir [Azure Defender](/azure/security-center/azure-defender) planı gerektirebilir.

**Azure ilkesi yerleşik tanımları-Microsoft. Keykasası**:

[!INCLUDE [Resource Policy for Microsoft.KeyVault 2.3](../../../includes/policy/standards/asb/rp-controls/microsoft.keyvault-2-3.md)]

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, Azure Key Vault günlüklerinizi tutmak için kullanılan Log Analytics çalışma alanı için, saklama süresini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

- [Veri saklama süresini değiştirme](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: anormal davranışlar için günlükleri çözümleyin ve izleyin ve Azure Key Vault korumalı kaynaklarınızın sonuçlarını düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics çalışma alanını kullanın. Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz. 

- [Yerleşik Azure Sentinel](../../sentinel/quickstart-onboard.md)

- [Azure Izleyici 'de Log Analytics kullanmaya başlama](/azure/azure-monitor/log-query/get-started-portal)

- [Azure İzleyici’de günlük sorgularını kullanmaya başlama](/azure/azure-monitor/log-query/get-started-queries)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Kılavuz**: Azure Güvenlik merkezi 'nde Key Vault için Gelişmiş tehdit koruması 'Nı (ATP) etkinleştirin. Azure Key Vault tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Log Analytics çalışma alanınızı Azure Sentinel 'e ekleyin. Bu, bir güvenlik Orchestration otomatik yanıtı (SOAR) çözümü sağlar. Bu, güvenlik sorunlarını gidermek için PlayBook 'ları (otomatikleştirilmiş çözümlerin) oluşturulmasına ve kullanılmasına olanak tanır.

- [Hızlı Başlangıç: Azure Sentinel'i kullanıma alma](../../sentinel/quickstart-onboard.md)

- [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](../../security-center/security-center-managing-and-responding-alerts.md)

- [Azure İzleyici Uyarıları ile olaylara yanıt verme](/azure/azure-monitor/learn/tutorial-response)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kimlik ve Access Control](../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure Active Directory (Azure AD) ile kaydedilen uygulamalarınızın envanterini ve Azure Key Vault Anahtarlarınıza, sırlarınıza ve sertifikalara erişimi olan tüm Kullanıcı hesaplarını sürdürün. Key Vault erişimi sorgulamak ve mutabık kılmak için Azure portal ya da PowerShell kullanabilirsiniz. PowerShell 'deki erişimi görüntülemek için aşağıdaki komutu kullanın:

(Get-AzResource-RESOURCEID [Keyvaultresourceıd]). Properties. AccessPolicies

- [Azure AD ile uygulama kaydetme](/azure/key-vault/key-vault-manage-with-cli2#registering-an-application-with-azure-active-directory)

- [Anahtar kasasına erişimin güvenliğini sağlama](secure-your-key-vault.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: Azure Key Vault örneklerinizin erişimine sahip adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Etkin yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi (Şu anda önizleme aşamasında) kullanın.

- [Kimlik ve erişimi izleme (Önizleme)](../../security-center/security-center-identity-access.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3,4: Azure Active Directory çoklu oturum açma kullan (SSO)

**Rehberlik**: uygulamanızın kimliğini sorunsuzca doğrulamak ve Azure Key Vault gizli dizilerinize erişmek için kullanılacak belirteci almak için AppID, Tenantıd ve ClientSecret ile birlikte bir Azure hizmet sorumlusu kullanın.

- [.NET kullanarak Azure Key Vault için hizmetten hizmete kimlik doğrulaması](/azure/key-vault/service-to-service-authentication)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Active Directory (Azure AD) çok faktörlü kimlik doğrulamasını etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi (Şu anda önizleme aşamasında), Olay Hub 'ı özellikli kaynaklarınızı korumaya yardımcı olma önerilerini izleyin.

- [Azure AD Multi-Factor Authentication dağıtımını planlayın](../../active-directory/authentication/howto-mfa-getstarted.md)

- [Kimlik ve erişimi izleme (Önizleme)](../../security-center/security-center-identity-access.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3,6: yönetim görevleri için güvenli, Azure tarafından yönetilen iş istasyonları kullanın

**Rehberlik**: Key Vault etkinleştirilmiş kaynakları etkinleştirmek ve yapılandırmak Için YAPıLANDıRıLMıŞ Azure AD çok faktörlü kimlik doğrulaması Ile ayrıcalıklı erişim iş istasyonu (Paw) kullanın.

- [Ayrıcalıklı Erişim İş İstasyonları](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/) 

- [Bulut tabanlı bir Azure AD çok faktörlü kimlik doğrulama dağıtımı planlama](../../active-directory/authentication/howto-mfa-getstarted.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik oluştuğunda günlükler ve uyarılar oluşturmak için Azure Active Directory (Azure AD) PRIVILEGED IDENTITY Management (PIM) kullanın. Riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın. Ek günlük kaydı için Azure Güvenlik Merkezi risk algılama uyarılarını Azure Izleyicisine gönderin ve eylem gruplarını kullanarak özel uyarı/bildirim yapılandırın.

Şüpheli etkinlik uyarıları oluşturmak için Azure Key Vault için Gelişmiş tehdit koruması 'nı (ATP) etkinleştirin.

- [Azure AD Privileged Identity Management dağıtma (PıM)](../../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Azure Key Vault için Gelişmiş tehdit koruması ayarlama (Önizleme)](/azure/security-center/advanced-threat-protection-key-vault)

- [Azure Key Vault için uyarılar (Önizleme)](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurekv)

- [Azure AD risk algılamaları](/azure/active-directory/reports-monitoring/concept-risk-events)

- [Azure portalında eylem gruplarını oluşturma ve yönetme](/azure/azure-monitor/platform/action-groups)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: koşullu erişim ilkesinin konum koşulunu yapılandırın ve adlandırılmış konumlarınızı yönetin. Adlandırılmış konumlarla, IP adresi aralıkları veya ülkeler ve bölgeler için mantıksal gruplandırmaları oluşturabilirsiniz. Key Vault gizli dizileri gibi hassas kaynaklarla erişimi, yapılandırılmış adlandırılmış konumlara kısıtlayabilirsiniz.

- [Azure Active Directory (Azure AD) koşullu erişim içindeki konum koşulu nedir?](../../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: Key Vault gibi Azure kaynakları için merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (Azure AD) kullanın. Bu, Azure rol tabanlı erişim denetimi 'nin (Azure RBAC) hassas kaynakları yönetme olanağı sağlar.

- [Azure AD 'de yeni bir kiracı oluşturun](../../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure Key Vault yönetici rollerine sahip eski hesapları bulmaya yardımcı olması için Azure Active Directory (Azure AD) günlüklerini inceleyin. Ayrıca, grup üyeliklerini verimli bir şekilde yönetmek için Azure AD erişim gözden geçirmeleri 'nı kullanın, Azure Key Vault erişmek için kullanılabilecek kurumsal uygulamalara erişin ve rol atamalarına erişin. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişiminin her 90 gün gibi düzenli aralıklarla gözden geçirilmesi gerekir.

- [Azure AD raporları ve izleme belgeleri](/azure/active-directory/reports-monitoring/)

- [Azure AD erişim gözden geçirmeleri nelerdir?](../../active-directory/governance/access-reviews-overview.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Rehberlik**: tüm günlükleri bir Log Analytics çalışma alanına göndererek Azure Key Vault ve Azure Active Directory (Azure AD) için tanılama ayarlarını etkinleştirin. Log Analytics içindeki istenen uyarıları (devre dışı parolalara erişim girişimleri gibi) yapılandırın.

- [Azure AD günlüklerini Azure Izleyici günlükleriyle tümleştirme](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

- [Eski Key Vault çözümünden geçiş](/azure/azure-monitor/insights/azure-key-vault#migrating-from-the-old-key-vault-solution)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapması üzerinde uyarı

**Rehberlik**: Azure Key Vault korumalı kaynaklarınızla ilişkili şüpheli eylemler için otomatik yanıtlar yapılandırmak üzere Azure Active Directory (Azure AD) kimlik koruması ve risk algılama özelliklerini kullanın. Kuruluşunuzun güvenlik yanıtlarını uygulamak için Azure Sentinel aracılığıyla otomatikleştirilmiş yanıtları etkinleştirmeniz gerekir.

- [Azure AD portalındaki riskli oturum açma işlemleri raporu](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Nasıl yapılır: risk ilkelerini yapılandırma ve etkinleştirme](../../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Azure Sentinel 'i ekleme](../../sentinel/quickstart-onboard.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Veri Koruma](../../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: Azure Key Vault etkinleştirilmiş kaynaklarda hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın. 

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: belirli alt ağlara erişimi kısıtlamak için yapılandırılmış sanal ağ hizmeti uç noktaları kullanarak Azure Key Vault erişimi güvenli hale getirebilirsiniz.

Güvenlik duvarı kuralları etkin olduktan sonra, isteğiniz izin verilen alt ağlardan veya IP adresi aralıklarından kaynaklanıyorsa yalnızca Azure Key Vault veri düzlemi işlemleri gerçekleştirebilirsiniz. Bu, Azure portal Azure Key Vault erişim için de geçerlidir. Azure portal bir anahtar kasasına gözatabilseniz de, istemci makineniz izin verilenler listesinde değilse anahtarları, parolaları veya sertifikaları listelemeyebilirsiniz. Bu ayrıca Azure Key Vault seçiciyi ve diğer Azure hizmetlerini de etkiler. Güvenlik duvarı kuralları istemci makinenizin bunu yapmasını engelliyorsa, anahtar kasalarının listelerini görebilirsiniz, ancak liste anahtarlarını göremezsiniz.

- [Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma](/azure/key-vault/key-vault-network-security)

- [Azure Key Vault için sanal ağ hizmet uç noktaları](/azure/key-vault/key-vault-overview-vnet-service-endpoints)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: Azure Key Vault içinde depolanan tüm veriler hassas olarak kabul edilir. Azure Key Vault gizli dizi erişimini denetlemek için Azure Key Vault veri düzlemi erişim denetimleri kullanın. Ayrıca, Ağ katmanında erişimi denetlemek için Key Vault yerleşik güvenlik duvarını da kullanabilirsiniz. Azure Key Vault erişimi izlemek için, Key Vault tanılama ayarlarını etkinleştirin ve günlükleri bir Azure depolama hesabına veya Log Analytics çalışma alanına gönderin.

- [Anahtar kasasına erişimin güvenliğini sağlama](secure-your-key-vault.md)

- [Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma](/azure/key-vault/key-vault-network-security)

- [Azure Key Vault günlüğü](/azure/key-vault/key-vault-logging)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4,6: kaynaklara erişimi yönetmek için Azure RBAC kullanma

**Rehberlik**: Azure Key Vault örneklerinizin yönetimine ve veri düzlemine güvenli erişim.

- [Anahtar kasasına erişimin güvenliğini sağlama](secure-your-key-vault.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure izleyici 'de Azure Key Vault Analytics çözümünü kullanarak Azure Key Vault denetim olay günlüklerini gözden geçirin.

- [Azure Izleyici 'de Azure Key Vault Analytics çözümü](/azure/azure-monitor/insights/azure-key-vault)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: güvenlik açığı yönetimi](../../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Kılavuz**: Azure Güvenlik Merkezi tarafından sunulan varsayılan risk derecelendirmelerini (güvenli puan) kullanın.

- [Azure Güvenlik Merkezi 'nde güvenli puanınızı geliştirme](/azure/security-center/security-center-secure-score)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: envanter ve varlık yönetimi](../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: aboneliğinizdeki tüm kaynakları (Azure Key Vault örnekleri dahil) sorgulamak ve bulmayı sağlamak Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun.

- [Azure Kaynak Grafiği gezginini kullanarak ilk kaynak Graph sorgunuzu çalıştırma](../../governance/resource-graph/first-query-portal.md)

- [Geçerli hesabın erişebileceği abonelikleri al](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [Azure rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: mantıksal olarak bunları bir taksonomi halinde düzenlemek için meta veriler sağlayan Azure Key Vault kaynaklarına Etiketler uygulayın.

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure Key Vault örnekleri ve ilgili kaynakları düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

- [Ek Azure aboneliği oluşturma](/azure/billing/billing-create-subscription)

- [Kaynak organizasyonu ve yönetimi için yönetim grupları oluşturma](/azure/governance/management-groups/create)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkelerini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Ayrıca, Azure Kaynak grafiğini kullanarak abonelikler içindeki kaynakları sorgular/bulur.

- [Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

- [Hızlı başlangıç: Azure Kaynak Grafiği gezginini kullanarak ilk kaynak Graph sorgunuzu çalıştırma](../../governance/resource-graph/first-query-portal.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkelerini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:

- [Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

- [Azure İlkesi Örnekleri](/azure/governance/policy/samples/built-in-policies#general)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimini kullanın. Bu, Key Vault yapılandırma ile ilgili olanlar gibi yüksek bir güvenlik ortamındaki kaynaklarda oluşturma ve değişikliklere engel olabilir.

- [Koşullu erişimle Azure yönetimine erişimi yönetme](../../role-based-access-control/conditional-access-azure-management.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: güvenli yapılandırma](../../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: Azure Key Vault örneklerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. keykasası" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, Azure Key Vault için yerleşik Azure Ilke tanımlarını da kullanabilirsiniz:

- Key Vault nesneler kurtarılabilir olmalıdır

- Log Analytics çalışma alanına Key Vault için tanılama ayarlarını dağıt

- Key Vault tanılama günlükleri etkinleştirilmelidir

- Key Vault bir sanal ağ hizmeti uç noktası kullanmalıdır

- Olay Hub 'ına Key Vault için tanılama ayarlarını dağıtma

- Azure Güvenlik Merkezi 'ndeki önerileri, Azure Key Vault örneklerinizin güvenli bir yapılandırma temeli olarak kullanın.

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Rehberlik**: Azure Key Vault etkinleştirilmiş kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yok] kullanın. 

- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

  
- [Azure Ilke efektlerini anlama](../../governance/policy/concepts/effects.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Rehberlik**: Azure Key Vault etkinleştirilmiş kaynaklarınız Için özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın.

- [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Azure Repos belgeleri](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. keykasası" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

- [Azure İlkesi'ni yapılandırma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Rehberlik**: Azure Key Vault korumalı kaynaklarınız için temel taramalar gerçekleştirmek üzere Azure Güvenlik Merkezi 'ni kullanın.

- [Azure Güvenlik Merkezi 'nde öneriler nasıl düzeltileceği](../../security-center/security-center-remediate-recommendations.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: bulut uygulamalarınız için gizli dizi yönetimini basitleştirmek ve güvenli hale getirmek için Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın. Azure Key Vault geçici silme özelliğinin etkinleştirildiğinden emin olun.

- [Azure yönetilen kimliklerle tümleştirin](../../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Key Vault oluşturma](quick-create-portal.md)

- [Key Vault’a yönelik kimlik doğrulaması](authentication.md)

- [Key Vault erişim ilkesi atama](assign-access-policy-portal.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: [Azure Güvenlik kıyaslaması](/azure/governance/policy/samples/azure-security-benchmark) , Güvenlik Merkezi için varsayılan Ilke girişimidir ve [Güvenlik Merkezi 'nin önerilerine](/azure/security-center/security-center-recommendations)yöneliktir. Bu denetimle ilgili Azure Ilke tanımları, Güvenlik Merkezi tarafından otomatik olarak etkinleştirilir. Bu denetimle ilgili uyarılar, ilgili hizmetler için bir [Azure Defender](/azure/security-center/azure-defender) planı gerektirebilir.

**Azure ilkesi yerleşik tanımları-Microsoft. Keykasası**:

[!INCLUDE [Resource Policy for Microsoft.KeyVault 7.11](../../../includes/policy/standards/asb/rp-controls/microsoft.keyvault-7-11.md)]

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvenli hale getirmek için Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın.

  

- [Azure yönetilen kimliklerle tümleştirme](../../azure-app-configuration/howto-integrate-azure-managed-service-identity.md) 

- [Key Vault oluşturma](quick-create-portal.md)    

- [Key Vault kimlik doğrulaması yapma](authentication.md)

- [Key Vault erişim ilkesi atama](assign-access-policy-portal.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik Bilgisi Tarayıcısı ayrıca bulunan kimlik bilgilerinin Azure Key Vault gibi daha güvenlik konumlara aktarılmasını sağlar.  
  
- [ Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kötü amaçlı yazılımdan koruma](../../security/benchmarks/security-control-malware-defense.md).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayar üzerinde etkinleştirilmiştir (örneğin, Azure Key Vault), ancak müşteri içeriği üzerinde çalışmaz.

Karşıya yüklenen veya Azure Key Vault gibi işlem dışı Azure kaynaklarına gönderilen tüm içerikleri önceden tarayın. Microsoft bu örneklerdeki verilerinize erişemez.

- [Azure Cloud Services ve sanal makineler için Microsoft Antimalware 'i anlayın](../../security/fundamentals/antimalware.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri kurtarma](../../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli otomatik arka pencerelere emin olun

**Rehberlik**: aşağıdaki PowerShell komutlarıyla, Key Vault sertifikalarınızın, anahtarlarının, yönetilen depolama hesaplarınızın ve gizli dizilerinizde düzenli otomatik yedeklemelere sahip olun:

- Backup-AzKeyVaultCertificate

- Backup-AzKeyVaultKey

- Backup-AzKeyVaultManagedStorageAccount

- Backup-AzKeyVaultSecret

İsteğe bağlı olarak, Key Vault yedeklerinizi Azure Backup içinde depolayabiliriz.

- [Key Vault sertifikalarını yedekleme](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultcertificate?view=azps-5.3.0&amp;preserve-view=true)

- [Key Vault anahtarlarını yedekleme](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-5.3.0&amp;preserve-view=true)

- [Key Vault yönetilen depolama hesaplarını yedekleme](/powershell/module/az.keyvault/add-azkeyvaultmanagedstorageaccount)

- [Key Vault gizli dizileri yedekleme](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultsecret?view=azps-5.3.0&amp;preserve-view=true)

- [Azure Backup etkinleştirme](/azure/backup)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: aşağıdaki PowerShell komutlarıyla Key Vault sertifikalarınızı, anahtarlarınızı, yönetilen depolama hesaplarınızı ve gizli dizileri gerçekleştirin:

- Backup-AzKeyVaultCertificate

- Backup-AzKeyVaultKey

- Backup-AzKeyVaultManagedStorageAccount

- Backup-AzKeyVaultSecret

İsteğe bağlı olarak, Key Vault yedeklerinizi Azure Backup içinde depolayabiliriz.

- [Key Vault sertifikalarını yedekleme](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultcertificate?view=azps-5.3.0&amp;preserve-view=true)

- [Key Vault anahtarlarını yedekleme](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-5.3.0&amp;preserve-view=true)

- [Key Vault yönetilen depolama hesaplarını yedekleme](/powershell/module/az.keyvault/add-azkeyvaultmanagedstorageaccount)

- [Key Vault gizli dizileri yedekleme](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultsecret?view=azps-5.3.0&amp;preserve-view=true)

- [Azure Backup etkinleştirme](/azure/backup)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: aşağıdaki PowerShell komutlarıyla, Key Vault sertifikalarınızı, anahtarlarınızı, yönetilen depolama hesaplarınızı ve gizli dizileri düzenli aralıklarla geri yükleme işlemi gerçekleştirin:

- Restore-AzKeyVaultCertificate

- Restore-AzKeyVaultKey

- Restore-AzKeyVaultManagedStorageAccount

- Restore-AzKeyVaultSecret

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:

- [Key Vault sertifikalarını geri yükleme](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultcertificate?view=azps-5.3.0&amp;preserve-view=true)

- [Key Vault anahtarlarını geri yükleme](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-5.3.0&amp;preserve-view=true)

- [Key Vault yönetilen depolama hesaplarını geri yükleme](/powershell/module/az.keyvault/backup-azkeyvaultmanagedstorageaccount)

- [Key Vault parolaları geri yükleme](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultsecret?view=azps-5.3.0&amp;preserve-view=true)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: Azure Key Vault için geçici silmenin etkinleştirildiğinden emin olun. Geçici silme, silinen anahtar kasalarının ve anahtar, gizli dizi ve sertifika gibi kasa nesnelerinin kurtarılmasına olanak tanır. 

- [Azure Key Vault 'nin geçici silme kullanımı](/azure/key-vault/key-vault-soft-delete-powershell)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: [Azure Güvenlik kıyaslaması](/azure/governance/policy/samples/azure-security-benchmark) , Güvenlik Merkezi için varsayılan Ilke girişimidir ve [Güvenlik Merkezi 'nin önerilerine](/azure/security-center/security-center-recommendations)yöneliktir. Bu denetimle ilgili Azure Ilke tanımları, Güvenlik Merkezi tarafından otomatik olarak etkinleştirilir. Bu denetimle ilgili uyarılar, ilgili hizmetler için bir [Azure Defender](/azure/security-center/azure-defender) planı gerektirebilir.

**Azure ilkesi yerleşik tanımları-Microsoft. Keykasası**:

[!INCLUDE [Resource Policy for Microsoft.KeyVault 9.4](../../../includes/policy/standards/asb/rp-controls/microsoft.keyvault-9-4.md)]

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Olay Yanıtı](../../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: Kuruluşunuz için bir olay yanıt kılavuzu oluşturun. Tüm personelin rollerine ek olarak algılama aşamasından olay sonrası gözden geçirme aşamasına kadar tüm olay işleme/yönetim aşamalarını tanımlayan yazılı olay yanıt planları bulunduğundan emin olun. Bu işlemlerin, Key Vault gizli dizileri kullanma gibi hassas sistemleri korumasıyla bir odağa sahip olması gerekir.

- [Azure Güvenlik Merkezi 'nde Iş akışı otomasyonlarını yapılandırma](../../security-center/security-center-planning-and-operations-guide.md)   

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process)   

- [Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu da kullanabilir](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, güvenlik merkezi 'nin, uyarıyı vermek için kullanılan bulma veya ölçümde ne kadar uygun olduğunu ve uyarıya yönelik etkinliğin arkasında kötü amaçlı bir amaç olduğunu bilmenin güven düzeyini temel alır. Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için, özellikle de Azure Key Vault gizli dizileri gibi hassas verileri işlemek üzere bir adlandırma sistemi oluşturun.

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: Azure Key Vault örneklerinizin ve ilgili kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek üzere alıştırmaları gerçekleştirin. Zayıf noktaları ve açıkları belirleyip planı gerektiği şekilde gözden geçirin.

- [NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.  Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

- [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](../../security-center/security-center-provide-security-contact-details.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: Azure Key Vault etkinleştirilmiş kaynaklardaki riskleri belirlemenize yardımcı olması Için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar.  Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz. 

 

- [Sürekli dışarı aktarmayı yapılandırma](../../security-center/continuous-export.md) 

  

- [Uyarıların Azure Sentinel’e akışını yapma](../../sentinel/connect-azure-security-center.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın ve Azure Key Vault korumalı kaynaklarınızı korumaya yönelik öneriler alın. 

 

- [Iş akışı otomasyonu ve Logic Apps yapılandırma](../../security-center/workflow-automation.md)

**Sorumluluk**: Müşteri

**Azure Güvenlik Merkezi izleme**: yok

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: Penetme testleri ve Red ekibi alıştırmaları](../../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft bulut Penme test kurallarını izleyin. Microsoft tarafından yönetilen bulut altyapısına, hizmetlere ve uygulamalara yönelik kırmızı takım ve canlı site sızma testi gerçekleştirmek için Microsoft'un stratejisini ve yürütme sürecini kullanın.

- [Sızma Testi Etkileşim Kuralları](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Bulut ile Kırmızı Takım Oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Sorumluluk**: Paylaşılan

**Azure Güvenlik Merkezi izleme**: yok

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik Karşılaştırması 2.0 sürümüne genel bakış](/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
