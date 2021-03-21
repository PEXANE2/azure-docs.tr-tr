---
title: VPN Gateway için Azure Güvenlik temeli
description: VPN Gateway Güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/01/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 0f1dcb82c3a8d3819e8d316ef8efe2e3256e3db4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102219004"
---
# <a name="azure-security-baseline-for-vpn-gateway"></a>VPN Gateway için Azure Güvenlik temeli

Bu güvenlik temeli, VPN Gateway için [Azure Güvenlik kıyaslama sürüm 1,0](../security/benchmarks/overview-v1.md) ' den rehberlik uygular. Azure Güvenlik Karşılaştırması, Azure üzerindeki bulut çözümlerinizin güvenliğini sağlamaya yönelik öneriler sunar. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimlerine** ve VPN Gateway için geçerli olan ilgili kılavuza göre gruplandırılır. VPN Gateway için geçerli olmayan **denetimler** dışlandı.

VPN Gateway Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam VPN Gateway Güvenlik temeli eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ağ güvenliği](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

**Rehberlik**: VPN Gateway alt ağları ile çalışırken ağ güvenlik grubu (NSG) ağ geçidi alt ağıyla ilişkilendirmekten kaçının. Ağ güvenlik grubunun bu alt ağ ile ilişkilendirilmesi, VPN Gateway’inizin beklendiği gibi çalışmayı durdurmasına neden olabilir.  Ancak, sanal ağınızdaki diğer VPN olmayan ağ geçidi alt ağları için ağ güvenlik grupları 'nı etkinleştirin.

- [Sanal ağ oluşturma](../virtual-network/quick-create-portal.md) 

- [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md) 

- [Azure portal kullanarak rota tabanlı bir VPN ağ geçidi oluşturma](./tutorial-create-gateway-portal.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1,2: sanal ağların, alt ağların ve ağ arabirimlerinin yapılandırma ve trafiğini izleme ve günlüğe kaydetme

**Rehberlik**: Azure Güvenlik Merkezi 'ni kullanarak ağ kaynaklarınızın güvenliğini sağlamaya yardımcı olmak için ağ koruma önerilerini izleyin. 

- [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](../security-center/security-center-network-recommendations.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets"></a>1,5: ağ paketlerini kaydetme

**Rehberlik**: ağ geçidi paketini yakalama veya gereksinimlerinize bağlı olarak belirli bir bağlantı üzerinde VPN Gateway 'i etkinleştirin.

- [VPN ağ geçitleri için paket yakalamaları yapılandırma](packet-capture.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure ilkesiyle ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın.

Ayrıca, tek bir şema tanımında Azure Resource Manager şablonları, Azure RBAC atamaları ve Azure ilke atamaları gibi temel ortam yapıtları sunarak büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Şema 'i yeni veya mevcut aboneliklere uygulayabilir ve sürüm oluşturma aracılığıyla denetim ve yönetime ince ayar yapabilirsiniz.

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md) 

- [Ağ için Azure Ilke örnekleri](../governance/policy/samples/built-in-policies.md#network) 

- [Azure Blueprint oluşturma](../governance/blueprints/create-blueprint-portal.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Rehberlik**: kaynak yapılandırmasını izlemek ve sanal ağ kaynaklarınızda yapılan değişiklikleri algılamak Için Azure etkinlik günlüğü 'nü kullanın. Azure Izleyici 'de, VPN ağ geçidiniz ile ilgili kritik kaynaklardaki değişiklikler gerçekleşirken tetiklenecek uyarılar oluşturun.

- [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](../azure-monitor/essentials/activity-log.md#view-the-activity-log)

- [Azure Izleyici 'de uyarı oluşturma](../azure-monitor/alerts/alerts-activity-log.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: VPN Gateway kaynaklarınız gibi ağ kaynakları tarafından oluşturulan güvenlik verilerini toplamak Için Azure izleyici aracılığıyla alma etkinliği ve tanılama günlükleri. Azure Izleyici 'yi kullanarak günlük verilerinde analizi gerçekleştirin ve bu günlüklerin uzun süreli/arşiv depolaması için Azure depolama hesaplarını kullanın. 

Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

- [VPN Gateway tanılama günlüğü olayları üzerinde uyarı ayarlama](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/essentials/diagnostic-settings.md)

- [Azure Izleyici ve üçüncü taraf SıEM tümleştirmesi ile çalışmaya başlama](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: denetim, güvenlik ve tanılama günlüklerine erişim sağlamak için VPN Gateway kaynaklarınızın tanılama ayarlarını etkinleştirin. Otomatik olarak kullanılabilen etkinlik günlükleri Olay kaynağını, tarihi, kullanıcıyı, zaman damgasını, kaynak adreslerini, hedef adreslerini ve diğer yararlı öğeleri içerir. 

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/essentials/diagnostic-settings.md) 

- [Azure 'da günlüğe kaydetme ve farklı günlük türlerini anlama](../azure-monitor/essentials/platform-logs-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de kuruluşunuzun uyumluluk düzenlemelerine göre Log Analytics çalışma alanı saklama süresini ayarlayın. Uzun süreli ve arşiv depolama için Azure depolama hesaplarını kullanın. 

- [Log Analytics veri saklama süresini değiştirme](../azure-monitor/logs/manage-cost-storage.md#change-the-data-retention-period) 

- [Azure depolama hesabı günlükleri için bekletme ilkesini yapılandırma](../storage/common/manage-storage-analytics-logs.md#configure-logging)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: anormal davranışlar için günlükleri çözümleyin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici 'yi ve bir Log Analytics çalışma alanını kullanın. 

Alternatif olarak, Azure Sentinel 'e veya üçüncü taraf SıEM 'ye yönelik verileri etkinleştirebilir ve bunlar üzerinde ayarlayabilirsiniz. 

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md) 

- [Log Analytics sorguları ile çalışmaya başlama](../azure-monitor/logs/log-analytics-tutorial.md) 

- [Azure Izleyici 'de özel sorgular gerçekleştirme](../azure-monitor/logs/get-started-queries.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Rehberlik**: güvenlik günlükleri ve olayları 'nda bulunan anormal etkinlikleri izlemek ve uyarmak Için Log Analytics çalışma alanıyla Azure Güvenlik Merkezi 'ni kullanın.

Alternatif olarak, Azure Sentinel 'de ve yerleşik verileri etkinleştirebilir.

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

- [Azure Güvenlik Merkezi 'nde uyarıları yönetme](../security-center/security-center-managing-and-responding-alerts.md)

- [Log Analytics günlük verilerinde uyarı alma](../azure-monitor/alerts/tutorial-response.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: Kurumsal gereksinimlerinize göre, DNS günlüğü çözümü Için Azure Marketi 'nden bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kimlik ve erişim denetimi](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Kılavuz**: Azure rol tabanlı erişim denetimi (Azure RBAC), Azure kaynaklarına erişimi rol atamaları aracılığıyla yönetmenizi sağlar. Bu rolleri kullanıcılara atayabilir, hizmet sorumlularını ve yönetilen kimlikleri gruplara atayabilirsiniz. Belirli kaynaklar için önceden tanımlanmış yerleşik roller vardır ve Azure CLI, Azure PowerShell veya Azure portal gibi araçlardan yararlanarak bu rollerin envanteri çıkarılabilir ve bunlar sorgulanabilir.

- [Azure AD 'de PowerShell ile dizin rolü alma](/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0) 

- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. 

Azure AD Privileged Identity Management ve Azure Resource Manager kullanarak tam zamanında erişimi de etkinleştirebilirsiniz. 

- [Privileged Identity Management hakkında daha fazla bilgi edinin](../active-directory/privileged-identity-management/index.yml)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3,4: Azure Active Directory çoklu oturum açma kullan (SSO)

**Rehberlik**: mümkün olan yerlerde, tek başına bağımsız kimlik bilgilerini hizmet başına yapılandırmak yerıne Azure Active Directory SSO kullanın. Azure Güvenlik Merkezi kimlik ve erişim önerilerini kullanın. 

- [Azure AD ile SSO 'yu anlama](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Kılavuz**: Azure AD MFA 'yı etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim önerilerini izleyin. 

- [Azure'da çok faktörlü kimlik doğrulamasını etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md) 

- [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3,6: yönetim görevleri için güvenli, Azure tarafından yönetilen iş istasyonları kullanın

**Rehberlik**: yükseltilmiş ayrıcalıklar gerektiren yönetim görevleri için güvenli, Azure tarafından yönetilen bir iş Istasyonu (ayrıcalıklı erişim iş istasyonu veya Paw olarak da bilinir) kullanın.

- [Güvenli, Azure tarafından yönetilen iş istasyonlarını anlayın](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Azure AD MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan etkinliklerin ne zaman oluştuğunu algılamak için Azure Active Directory güvenlik raporları ve izleme kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın.

- [Riskli etkinlik bayrağıyla işaretlenen Azure AD kullanıcılarını belirleme](../active-directory/identity-protection/overview-identity-protection.md)

- [Azure Güvenlik Merkezi’nde kullanıcıların kimliğini ve erişim etkinliğini izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek IÇIN Azure AD adlandırılmış konumlarını kullanın. 

- [Azure AD adlandırılmış konumlarını yapılandırma](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (Azure AD) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar. 

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure Active Directory (Azure AD) eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini verimli bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure AD kimlik ve erişim gözden geçirmeleri ' nı kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir. 

- [Azure AD raporlamayı anlama](../active-directory/reports-monitoring/index.yml) 

- [Azure AD kimlik ve erişim gözden geçirmelerini kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Rehberlik**: herhangi bir SIEM/izleme aracıyla tümleştirmenize olanak tanıyan Azure AD oturum açma etkinliğine, denetimine ve risk olay günlüğü kaynaklarına erişiminiz vardır. 

Bu işlemi, Azure AD Kullanıcı hesapları için Tanılama ayarları oluşturarak ve Log Analytics çalışma alanına denetim günlüklerini ve oturum açma günlüklerini göndererek kolaylaştırabilirsiniz. İstenen uyarıları Log Analytics çalışma alanı içinde yapılandırabilirsiniz. 

- [Azure Izleyici ile Azure etkinlik günlüklerini tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: otomatik yanıtları yapılandırmak için Azure AD kimlik koruması özellikleri kullanarak Kullanıcı kimlikleriyle ilgili şüpheli eylemleri tespit edin. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz. 

- [Azure AD riskli oturum açma işlemlerini görüntüleme](../active-directory/identity-protection/overview-identity-protection.md)

- [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md) 

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri koruma](../security/benchmarks/security-control-data-protection.md).*

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: VPN ağ geçitlerinin her bir müşteri sanal ağı IÇIN ayrılmış VM örnekleri vardır. Ortam türü ve veri duyarlılığı düzeyi gibi bireysel güvenlik etki alanları için ayrı sanal ağları, abonelikleri ve Yönetim gruplarını kullanarak yalıtım uygulayın. Uygulamalarınızın ve kurumsal ortamların talep ettiği Azure kaynaklarınıza erişim düzeyini kısıtlayabilirsiniz. Azure rol tabanlı erişim denetimi (Azure RBAC) aracılığıyla Azure kaynaklarına erişimi denetleyebilirsiniz.

- [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

- [Yönetim Grupları oluşturma](../governance/management-groups/create-management-group-portal.md)

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: hassas bilgilerin yetkisiz aktarımını izlemek ve bilgi güvenliği uzmanlarına uyarı ederken bu tür aktarımları engellemek için ağ Perimetrelerinde Azure Marketi 'nde üçüncü taraf bir çözüm kullanın. 

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini müşteri veri kaybına ve pozlamaya karşı hassas ve koruyucuları olarak değerlendirir. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar. 

- [Azure’da müşteri verilerinin korunmasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: VPN ağ geçitleri, Azure VPN ağ geçitleri ile müşteri ŞIRKET içi VPN CIHAZLARı (S2S) veya VPN ISTEMCILERI (P2S) arasında müşteri paketlerini şifreler. VPN ağ geçitleri Ayrıca VNet 'ten VNet 'e şifrelemeyi destekler.

Sanal ağınızdaki geçerli kaynaklar için bekleyen ve geçişte şifreleme için Azure Güvenlik Merkezi önerilerini izleyin.

- [VPN türleri hakkında](vpn-gateway-about-vpn-gateway-settings.md#vpntype)

- [Siteler Arası VPN Ağ Geçidi bağlantıları için VPN cihazları ve IPsec/IKE parametreleri hakkında](vpn-gateway-about-vpn-devices.md#ipsec)

- [Şifreleme gereksinimleri ve Azure VPN ağ geçitleri hakkında](vpn-gateway-about-compliance-crypto.md)

- [S2S VPN veya VNet-VNet bağlantıları için IPsec/IKE ilkesi yapılandırma](vpn-gateway-ipsecikepolicy-rm-powershell.md)

- [Azure ile iletim sırasında şifrelemeyi anlama](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: bir üçüncü taraf etkin bulma aracı kullanarak, Şirket içindeki veya uzak bir hizmet sağlayıcılarındaki ve BT 'nin önemli bilgi envanterini güncelleştiren, teknoloji sistemleriniz tarafından depolanan, işlenen veya iletilen tüm hassas bilgileri belirlemek için kullanılır.

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Rehberlik**: veri ve kaynaklara erişimi denetlemek için Azure rol tabanlı erişim denetimi 'Ni (Azure RBAC) kullanın, aksi takdirde hizmete özel erişim denetimi yöntemlerini kullanın. Sahip, katkıda bulunan veya ağ katılımcısı gibi yerleşik rolleri kullanın ve ardından rolü uygun kapsama atayın. Özel bir rol oluşturarak ve sanal ağlar, alt ağlar, VPN ağ geçitleri, ağ arabirimleri, ağ güvenlik grupları ve bu role yönlendirme tabloları için gereken belirli izinleri atayarak sanal ağ özellikleri alt kümesi için belirli izinler atayın.

- [Azure RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md)

- [Sanal ağları planlama](../virtual-network/virtual-network-vnet-plan-design-arm.md#permissions)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Rehberlik**: VPN ağ geçitleriniz gibi kritik Azure kaynaklarına yapılan değişikliklerin gerçekleştiği zamana yönelik Azure etkinlik günlüklerinizi tetiklemek Için Azure izleyici uyarılarını yapılandırın. 

- [VPN Gateway ölçümlerinde uyarıları ayarlama](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md)

- [VPN Gateway tanılama günlüğü olayları üzerinde uyarı ayarlama](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md)

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](../azure-monitor/alerts/alerts-activity-log.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: güvenlik açığı yönetimi](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: ortak bir risk Puanlama programını (örneğin, ortak güvenlik açığı Puanlama sistemi) veya üçüncü taraf tarama aracınız tarafından sunulan varsayılan risk derecelendirmelerini kullanın.

- [NıST yayını--ortak güvenlik açığı Puanlama sistemi](https://www.nist.gov/publications/common-vulnerability-scoring-system)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: envanter ve varlık yönetimi](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: ABONELIKLERINIZDE VPN ağ geçitlerinizle ilgili tüm kaynakları sorgulamak ve saptamak Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinlere sahip olduğunuzdan ve aboneliklerinizdeki tüm kaynakları numaralandırabiliyor olduğunuzdan emin olun. Ayrıca, VPN Gateway kaynaklarınızı listelemek için Azure CLı de kullanabilirsiniz.

- [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

- [VPN ağ geçidi için Azure CLı](/cli/azure/network/vnet-gateway)

- [Azure aboneliklerinizi görüntüleme](/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Azure RBAC 'yi anlama](../role-based-access-control/overview.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: öğeleri tanımlanan bir sınıflandırmaya göre mantıksal olarak düzenlemek için VPN Gateway kaynaklarına Etiketler uygulayın.

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: VPN Gateway kaynaklarını düzenlemek ve izlemek için etiketleme, yönetim grupları ve uygun durumlarda ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun. 

- [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md) 

- [Yönetim grupları oluşturma](../governance/management-groups/create-management-group-portal.md) 

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynaklarının envanterini tanımlayın ve saklayın

**Rehberlik**: Kurumsal gereksinimlerinize göre işlem kaynakları Için onaylanan Azure kaynakları ve onaylanan yazılım envanterini oluşturun.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerinde oluşturulabilen kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Bunlara ek olarak, abonelikler içindeki kaynakları sorgulamak ve saptamak için Azure Kaynak grafiğini kullanın. 

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

- [Sanal ağ için Azure ilke örneği yerleşik bileşenleri](../virtual-network/policy-reference.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: müşteriler, kurumsal güvenlik gereksinimlerinin gerektirdiği şekilde Azure ilke tanımlarını atayarak kaynak oluşturmayı veya kullanımını engelleyebilir. Ancak, onaylanmamış veya yetkisiz kaynakları kaldırmak için kendi işleminizi uygulamanız gerekir.

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak aboneliklerinizde oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](../governance/policy/samples/index.md)

- [Sanal ağ için Azure ilke örneği yerleşik bileşenleri](../virtual-network/policy-reference.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure kaynakları yöneticisiyle etkileşime geçmesini sınırlamak IÇIN Azure AD koşullu erişimi kullanın. 

- [Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: güvenli yapılandırma](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: VPN Gateway dahil olmak üzere Azure ağ kaynaklarınızın yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak üzere Azure ilkesi diğer adlarını kullanın. Yerleşik Azure Ilke tanımlarını da kullanabilirsiniz.

Azure Resource Manager, yapılandırmaların kuruluşunuzun güvenlik gereksinimlerini karşıladığından veya aştığından emin olmak için gözden geçirilmesi gereken Java betik nesne gösterimi (JSON) içinde şablonu dışarı aktarma özelliğine sahiptir.

Azure Güvenlik Merkezi 'nin önerilerini Azure kaynaklarınız için güvenli bir yapılandırma temeli olarak da kullanabilirsiniz.

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Sanal ağ için Azure ilke örneği yerleşik bileşenleri](../virtual-network/policy-reference.md)

- [Azure portal bir şablona tek ve çoklu kaynak verme](../azure-resource-manager/templates/export-template-portal.md)

- [Güvenlik önerileri - başvuru kılavuzu](../security-center/recommendations-reference.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Rehberlik**: VPN Gateway ve ilgili kaynaklarla ilişkili Azure kaynaklarını güvenli şekilde yapılandırmak için Azure Resource Manager şablonları ve Azure ilke atamalarını kullanın. Azure Resource Manager şablonlar, Azure kaynaklarıyla birlikte sanal makine dağıtmak için kullanılan JSON tabanlı dosyalardır ve özel şablonla korunmaları gerekir. Microsoft, temel şablonlarda Bakımı gerçekleştirir.  Azure kaynaklarınızın tamamında güvenli ayarları zorlamak için [reddetme] ve [dağıtım yok] modlarında Azure Ilkesi ' ni kullanın.

- [Azure Resource Manager şablonları oluşturma hakkında bilgi](../virtual-machines/windows/ps-template.md)

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

- [Sanal ağ için Azure Resource Manager şablonu örnekleri](../virtual-network/template-samples.md)

- [Sanal ağ için Azure ilke örneği yerleşik bileşenleri](../virtual-network/policy-reference.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları, Azure Resource Manager şablonları ve istenen durum yapılandırması betikleri gibi kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure DevOps kullanın. Azure DevOps 'da yönettiğiniz kaynaklara erişmek için, Azure DevOps ile tümleşikse veya TFS ile tümleşikse Active Directory belirli kullanıcılara, yerleşik güvenlik gruplarına veya Azure Active Directory (Azure AD) tanımlanmış gruplara izin verebilir veya vermeyebilirsiniz. 

- [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow?view=azure-devops) 

- [Azure DevOps 'da izinler ve gruplar hakkında](/azure/devops/organizations/security/about-permissions)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Kılavuz**: Azure Ilkesini kullanarak Azure kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure kaynaklarınızın diğer adlarını denetlemek veya Azure kaynaklarınızın yapılandırmasını zorlamak için özel ilkeler oluşturmak üzere kullanın. Ayrıca, belirli kaynaklarınızla ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz. Ayrıca, yapılandırma değişikliklerini dağıtmak için Azure Otomasyonu ' nu kullanabilirsiniz. 

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md) 

- [Diğer adları kullanma](../governance/policy/concepts/definition-structure.md#aliases)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Kılavuz**: Azure ilke tanımlarını, sizin VPN Gateway kaynaklarla ilgili kaynak yapılandırmalarına göre ölçmek için atayın. Kaynak yapılandırmalarını denetlemek ve kritik yapılandırma değişiklikleriyle ilgili uyarı almak için Azure Policy Insights 'ı kullanın.

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Sanal ağ için Azure ilke örneği yerleşik bileşenleri](../virtual-network/policy-reference.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: VPN Gateway hizmeti, şifrelenmiş formlarda müşteri önceden paylaşılan anahtarlarını ve sertifikalarını dahili olarak depolar ve iletir. Müşterilerin, önceden paylaşılan anahtarları veya sertifikaları kendi sistemlerinde güvenceye almanız gerekir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Kılavuz**: Azure P2S VPN üç kimlik doğrulama yöntemini destekler:

- Sertifika tabanlı
- RADIUS
- Azure Active Directory (Azure AD)

Yönetilen kimliklerden yararlanmanızı sağladığından Azure AD önerilir.

- [Kiracı yapılandırma](openvpn-azure-ad-tenant.md)

- [Kiracıyı birden çok istemci uygulamasıyla yapılandırma](openvpn-azure-ad-tenant-multi-app.md)

- [Multi-Factor Authentication Yapılandır](openvpn-azure-ad-mfa.md)

- [VPN istemcisi yapılandırma](openvpn-azure-ad-client.md)

- [Yönetilen kimlikleri yapılandırma](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik Bilgisi Tarayıcısı ayrıca bulunan kimlik bilgilerinin Azure Key Vault gibi daha güvenlik konumlara aktarılmasını sağlar. 

- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri kurtarma](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli otomatik arka pencerelere emin olun

**Rehberlik**: VPN Gateway kaynaklarını dağıtmak için Azure Resource Manager kullanın. Azure Resource Manager, VPN Gateway kaynaklarını geri yüklemek için yedeklemeler olarak kullanılabilecek şablonları dışarı aktarma yeteneği sağlar. Azure Resource Manager şablonu dışarı aktarma API 'sini düzenli aralıklarla çağırmak için Azure Otomasyonu 'nu kullanın.

- [Azure Resource Manager'a genel bakış](../azure-resource-manager/management/overview.md)

- [Sanal ağ için Azure Resource Manager şablonu örnekleri](../virtual-network/template-samples.md)

- [Kaynak grupları-şablonu dışarı aktarma](/rest/api/resources/resourcegroups/exporttemplate)

- [Azure Otomasyonu 'na giriş](../automation/automation-intro.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: VPN Gateway kaynaklarını dağıtmak için Azure Resource Manager kullanın. Azure Resource Manager, VPN ağ geçidini ve ilgili kaynakları geri yüklemek için yedeklemeler olarak kullanılabilecek şablonları dışarı aktarma olanağı sağlar. Azure Resource Manager şablonu dışarı aktarma API 'sini düzenli aralıklarla çağırmak için Azure Otomasyonu 'nu kullanın.

- [Azure portal bir şablona tek ve çoklu kaynak verme](../azure-resource-manager/templates/export-template-portal.md)

- [Kaynak grupları-şablonu dışarı aktarma](/rest/api/resources/resourcegroups/exporttemplate)

- [Azure Otomasyonu 'na giriş](../automation/automation-intro.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: gerektiğinde yalıtılmış bir aboneliğe düzenli olarak Azure Resource Manager şablonlarının dağıtımını düzenli olarak gerçekleştirdiğinizden emin olun.

- [ARM şablonları ve Azure portal kaynak dağıtma](../azure-resource-manager/templates/deploy-portal.md)

- [Azure 'da Anahtar Kasası anahtarlarını geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Kılavuz**: özel Azure ilke tanımları ve Azure Resource Manager şablonları gibi kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure DevOps kullanın. Azure DevOps 'da yönettiğiniz kaynakları korumak için, Azure DevOps ile tümleşikse, belirli kullanıcılar, yerleşik güvenlik grupları veya Azure Active Directory (Azure AD) içinde tanımlanan gruplar için izin verebilir veya vermeyebilirsiniz.

- [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure DevOps 'da izinler ve gruplar hakkında](/azure/devops/organizations/security/about-permissions)

- [Azure Depolama blobları için geçici silme](../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: olay yanıtı](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu geliştirin. Tüm personel rollerinin yanı sıra olay işleme ve yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun. 

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/) 

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/) 

- [Kendi olay yanıtı planınızın oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu kullanın](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Kılavuz**: Azure Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri kullanarak abonelikleri işaretleyin ve Azure kaynaklarını tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun.  Olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak, uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır. 

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md) 

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: Azure kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip yanıt planınızı gerektiği gibi düzeltin. 

- [NıST yayını--BT planları ve becerileri için programları test etme, eğitim ve alıştırmaya yönelik kılavuz](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin. 

- [Azure Güvenlik Merkezi güvenlik iletişimini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Kılavuz**: Azure kaynaklarına yönelik riskleri belirlemenize yardımcı olmak için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e aktarabilirsiniz. 

- [Sürekli dışarı aktarmayı yapılandırma](../security-center/continuous-export.md) 

- [Uyarıların Azure Sentinel’e akışını yapma](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: iş akışı Otomasyonu özelliği Azure Güvenlik Merkezi 'Ni kullanarak Azure kaynaklarınızı korumaya yönelik güvenlik uyarılarına ve önerilerine yönelik yanıtları otomatik olarak tetikleyin. 

- [Güvenlik Merkezi 'nde iş akışı Otomasyonu nasıl yapılandırılır](../security-center/workflow-automation.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: Penetme testleri ve Red ekibi alıştırmaları](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft bulut Penme test kurallarını izleyin. Microsoft tarafından yönetilen bulut altyapısına, hizmetlere ve uygulamalara yönelik kırmızı takım ve canlı site sızma testi gerçekleştirmek için Microsoft'un stratejisini ve yürütme sürecini kullanın. 

- [Sızma Testi Etkileşim Kuralları](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Bulut ile Kırmızı Takım Oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin