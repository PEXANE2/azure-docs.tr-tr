---
title: Azure özel bağlantısı için Azure Güvenlik temeli
description: Azure özel bağlantı güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: private-link
ms.topic: conceptual
ms.date: 09/08/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 654fc453f0b4167ae91afcab811de321925c6bf5
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89614676"
---
# <a name="azure-security-baseline-for-azure-private-link"></a>Azure özel bağlantısı için Azure Güvenlik temeli

Bu güvenlik temeli [Azure Güvenlik kıyaslayıcılarından](../security/benchmarks/overview.md) Azure özel bağlantısına kılavuzluk uygular. Azure Güvenlik kıyaslaması, bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimlerine** ve Azure özel bağlantısı için geçerli olan ilgili kılavuza göre gruplandırılır. Azure özel bağlantısı için geçerli olmayan **denetimler** dışlandı. Azure özel bağlantısının Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam Azure sanal ağ güvenlik temeli eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ağ güvenliği](../security/benchmarks/security-control-network-security.md).*

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Rehberlik**: kaynak yapılandırmasını Izlemek ve özel bağlantıyla ilgili ağ kaynaklarınızda yapılan değişiklikleri algılamak Için Azure etkinlik günlüğünü kullanın. 

Kritik kaynaklardaki değişiklikler gerçekleşirken tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

- [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](/azure/azure-monitor/platform/activity-log-view)

- [Azure Izleyici 'de uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: özel bağlantı uç noktaları, sanal ağlar ve ağ güvenlik grupları gibi ağ kaynakları tarafından oluşturulan güvenlik verilerini toplamak Için Azure izleyici aracılığıyla günlükleri alma. 

Azure Izleyici 'de, Log Analytics çalışma alanlarını kullanarak Analytics 'i sorgulayın ve gerçekleştirin ve uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

Ayrıca kurumsal iş gereksinimlerine bağlı olarak, Azure Sentinel veya bir üçüncü taraf SıEM için de verileri etkinleştirin.

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md)

- [Özel bağlantı için günlüğe kaydetme ve izleme](private-link-overview.md#logging-and-monitoring)

- [Ağ güvenlik grubu için tanılama günlüğü](../virtual-network/virtual-network-nsg-manage-log.md)

- [Azure Izleyici ve üçüncü taraf SıEM tümleştirmesi ile çalışmaya başlama](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: Azure izleyici etkinlik günlüklerini etkinleştirin; Örneğin, işlemi kimin başlattığı, işlem ne zaman gerçekleştiği, işlemin durumu ve diğer yararlı denetim bilgileri gibi özel bağlantı kaynaklarında gerçekleştirilen işlem. 

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md)

- [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](/azure/azure-monitor/platform/activity-log-view)

- [Özel bağlantı için günlüğe kaydetme ve Izleme](private-link-overview.md#logging-and-monitoring)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: özel bağlantıyla ilgili Günlükler için, Log Analytics çalışma alanı saklama dönemini kuruluşunuzun Azure izleyici içindeki uyumluluk düzenlemelerine göre ayarlayın. Günlüklerin uzun süreli/arşiv depolaması için Azure depolama hesaplarını kullanın.

- [Log Analytics veri saklama süresini değiştirme](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

- [Azure depolama hesabı günlükleri için bekletme ilkesini yapılandırma](../storage/common/storage-monitor-storage-account.md#configure-logging)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: anormal davranış için günlükleri çözümleyin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics çalışma alanını kullanın.

Diğer bir seçenek de Azure Sentinel veya üçüncü taraf bir SıEM üzerinde veri etkinleştirmek ve bu verileri açmak.

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

- [Log Analytics çalışma alanını anlayın](../azure-monitor/log-query/get-started-portal.md)

- [Azure Izleyici 'de özel sorgular gerçekleştirme](../azure-monitor/log-query/get-started-queries.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Rehberlik**: güvenlik günlükleri ve olayları 'nda bulunan anormal etkinlikleri izlemek ve uyarmak için bir Log Analytics çalışma alanıyla yapılandırılan güvenlik merkezini kullanın.

Kurumsal iş gereksinimlerinize bağlı olarak Azure Sentinel 'e ve şirket içi verileri bir üçüncü taraf SıEM 'e etkinleştirin.

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

- [Azure Güvenlik Merkezi 'nde uyarıları yönetme](../security-center/security-center-managing-and-responding-alerts.md)

- [Log Analytics günlük verilerinde uyarı alma](../azure-monitor/learn/tutorial-response.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kimlik ve erişim denetimi](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: açık olarak atanabilen ve sorgulanabilen Azure Active Directory (Azure AD) yerleşik yönetici rollerini kullanın. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü çalıştırın.

- [Azure AD 'de PowerShell ile dizin rolü alma](/powershell/module/azuread/get-azureaddirectoryrole)

- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](/powershell/module/azuread/get-azureaddirectoryrolemember)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Güvenlik Merkezi 'nin kimlik ve erişim yönetimi özelliklerini kullanın.

Ayrıca, Microsoft Hizmetleri için ayrıcalıklı rolleri Privileged Identity Management Azure Active Directory (Azure AD) kullanarak tam zamanında/tam erişimi etkinleştirin ve Azure Resource Manager.

- [Privileged Identity Management hakkında daha fazla bilgi edinin](/azure/active-directory/privileged-identity-management/)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3,4: Azure Active Directory çoklu oturum açma kullan (SSO)

**Rehberlik**: mümkün olan yerlerde, tek başına bağımsız kimlik bilgilerini hizmet başına yapılandırmak yerine Azure Active Directory ile çoklu oturum açma kullanın.

- [Azure Active Directory uygulamalarda çoklu oturum açma](../active-directory/manage-apps/what-is-single-sign-on.md)

- [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Active Directory (Azure AD) Multi-Factor Authentication 'ı (MFA) etkinleştirin ve güvenlik merkezi kimlik ve erişim yönetimi önerilerini izleyin.

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

- [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3,6: yönetim görevleri için güvenli, Azure tarafından yönetilen iş istasyonları kullanın

**Kılavuz**: Azure ağ kaynaklarını açmak ve yapılandırmak için yapılandırılmış Multi-Factor Authentication bir ayrıcalıklı erişim iş istasyonu (Paw) kullanın.

- [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

**Rehberlik**: riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure Active Directory (Azure AD) risk algılamaları özelliğini kullanın. Güvenlik Merkezi risk algılama, Azure Izleyici 'ye uyarı verebilir ve eylem gruplarını kullanarak özel uyarı/bildirim yapılandırır.

- [Azure Güvenlik Merkezi risk algılamalarını anlama (şüpheli etkinlik)](/azure/active-directory/reports-monitoring/concept-risk-events)

- [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Özel uyarı ve bildirim için eylem gruplarını yapılandırma](../azure-monitor/platform/action-groups.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek için adlandırılmış konumlar kullanın.

- [Azure 'da adlandırılmış konumları yapılandırma](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (Azure AD) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifreleme kullanarak verileri korur ve ayrıca, salları, karmaları ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.  

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory-domain-services/tutorial-create-instance.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure Active Directory eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemeleri kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir.

- [Azure AD raporlamayı anlama](/azure/active-directory/reports-monitoring/)

- [Azure kimlik erişimi Incelemelerini kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Rehberlik**: herhangi bir SIEM/izleme aracıyla tümleştirilecek Azure Active Directory (Azure AD) oturum açma etkinliğini, denetimini ve risk olay günlüğü kaynaklarını kullanın.

Azure AD Kullanıcı hesapları için Tanılama ayarları oluşturup Log Analytics çalışma alanına denetim günlüklerini ve oturum açma günlüklerini göndererek bu işlemi kolaylaştırın. Log Analytics çalışma alanı içinde istenen uyarıları yapılandırın.

- [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapması üzerinde uyarı

**Kılavuz**: otomatik yanıtları yapılandırmak için Azure Active Directory (Azure AD) risk ve kimlik koruma özelliklerini kullanın. 

Daha fazla araştırma için Azure Sentinel 'e veri alma.

- [Azure AD riskli oturum açma işlemlerini görüntüleme](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri koruma](../security/benchmarks/security-control-data-protection.md).*

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: ortam türü ve veri duyarlılığı düzeyi gibi bireysel güvenlik etki alanları için ayrı abonelikler ve yönetim grupları kullanarak yalıtım uygulayın. 

İş gereksinimlerine bağlı olarak uygulamalarınızın ve kurumsal ortamlarınızla Azure kaynaklarınıza erişim düzeyini kısıtlayın. 

Azure Active Directory rol tabanlı erişim denetimi aracılığıyla Azure kaynaklarına erişimi denetleyin.

- [Ek Azure abonelikleri oluşturma](/azure/billing/billing-create-subscription)

- [Yönetim Grupları oluşturma](/azure/governance/management-groups/create)

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: yoldaki tüm hassas bilgileri şifreleyin. Sanal ağlardaki Azure kaynaklarınıza bağlanan tüm istemcilerin TLS 1,2 veya daha yüksek bir bağlantı kurabildiğinden emin olun. Özel bağlantı, temeldeki protokollerle karışmaz. Müşteriler tarafından kullanılan diğer teklifler için en iyi uygulamaları kullanın.

Uygun olduğunda, bekleyen ve geçişte şifreleme için Güvenlik Merkezi önerilerini izleyin.

- [Azure ile iletim sırasında şifrelemeyi anlama](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için rol tabanlı erişim denetimi kullanma

**Rehberlik**: veri ve kaynaklara erişimi denetlemek için Azure rol tabanlı erişim denetimi 'Ni (Azure RBAC) kullanın, aksi takdirde hizmete özel erişim denetimi yöntemlerini kullanın.

- [Burada Azure yerleşik rollerinin kısa bir açıklaması ve benzersiz KIMLIĞI hakkında bilgi edinin](../role-based-access-control/built-in-roles.md)

Ortamınızdaki rol listesini almak için PowerShell cmd "Get-AzRoleDefinition" veya "az role Definition List" komutunu çağırın.

"Görünürlük" ayarı aracılığıyla hizmetinizin görünürlüğünü denetlemek için seçeneklerinizi gözden geçirin. Özel bağlantı. Bu görünürlük ayarları, bir tüketicinin hizmetinize bağlanıp bağlanamacağına karar verir. 

Hizmetinizi diğer sanal ağlarınızdan (yalnızca Azure RBAC izinleri) tüketim açısından özel hale getirin. Sınırlı bir güvenilen abonelik kümesiyle pozlamayı kısıtlayın veya tüm Azure aboneliklerinin özel bağlantı hizmetinde bağlantı isteyebilmesi için ortak hale getirin.

- [Azure RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md)

- [Özel bağlantı hizmeti özellikleri](private-link-service-overview.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Rehberlik**: Azure izleyici etkinlik günlüğü uyarılarını kullanarak, özel bağlantı Hizmetleri ve uç noktalar gibi kritik Azure kaynaklarında değişiklik gerçekleşirken uyarılar oluşturun. 

- [Ağ güvenlik grubu için tanılama günlüğü](private-link-overview.md#logging-and-monitoring)

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: envanter ve varlık yönetimi](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Kılavuz**: özel bağlantı Hizmetleri ve aboneliklerinizin içindeki uç noktalar gibi tüm ağ kaynaklarını sorgulamak ve öğrenmek Için Azure Kaynak grafiğini kullanın. 

Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun.

- [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

- [Azure aboneliklerinizi görüntüleme](/powershell/module/az.accounts/get-azsubscription)

- [Azure RBAC 'yi anlama](../role-based-access-control/overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: verileri bir taksonomi halinde mantıksal olarak düzenlemek için meta verileri kullanarak Azure kaynaklarına Etiketler uygulayın.

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: özel bağlantıyı ve ilgili kaynakları düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. 

Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

- [Ek Azure abonelikleri oluşturma](/azure/billing/billing-create-subscription)

- [Yönetim Grupları oluşturma](/azure/governance/management-groups/create)

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynaklarının envanterini tanımlayın ve saklayın

**Rehberlik**: Kurumsal gereksinimlerinize göre işlem kaynakları Için onaylanan Azure kaynakları ve yazılımlarının envanterini oluşturun.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerinde oluşturulabilen kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Ayrıca, aboneliklerdeki kaynakları sorgulamak/saptamak için Azure Kaynak grafiğini kullanın. Bu, depolama hesaplarıyla ilgili olanlar gibi yüksek güvenlik tabanlı ortamlarda yardımcı olabilir.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

- [Özel bağlantı için Azure ilke örneği yerleşik bileşenleri](../governance/policy/samples/built-in-policies.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="67-remove-unapproved-azure-resources"></a>6,7: onaylanmamış Azure kaynaklarını kaldır

**Rehberlik**: müşteri, Azure ilkesiyle müşterinin Şirket ilkelerinin gerektirdiği şekilde kaynak oluşturulmasını veya kullanımını engelleyebilir. Yetkisiz kaynakları kaldırmak için kendi işleminizi uygulayabilirsiniz.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerinde oluşturulabilen kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](/azure/governance/policy/samples/not-allowed-resource-types)

- [Özel bağlantı için Azure ilke örneği yerleşik bileşenleri](../governance/policy/samples/built-in-policies.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimi kullanın.

- [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: güvenli yapılandırma](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: yerleşik Azure ilke tanımlarıyla birlikte Azure ağ kaynaklarınızın yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak üzere Azure ilke diğer adlarını kullanın.

Azure Resource Manager şablonu JavaScript Nesne Gösterimi (JSON) ' de dışarı aktarma özelliğine sahiptir. Bu yapıt, yapılandırmaların kuruluşunuzun güvenlik gereksinimlerini karşıladığından veya bu gereksinimleri aştığından emin olmak için gözden geçirilmesi gerekir.

Azure kaynaklarınız için güvenli bir yapılandırma temeli olarak güvenlik merkezi 'nden öneriler uygulayın.

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](/powershell/module/az.resources/get-azpolicyalias)

- [Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Özel bağlantı için Azure ilke örneği yerleşik bileşenleri](../governance/policy/samples/built-in-policies.md)

- [Azure portal bir şablona tek ve çoklu kaynak verme](../azure-resource-manager/templates/export-template-portal.md)

- [Güvenlik önerileri-bir başvuru kılavuzu](../security-center/recommendations-reference.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: özel bağlantıyla ve ilgili kaynaklarla ilişkili Azure kaynaklarını güvenli bir şekilde yapılandırmak için Azure Resource Manager şablonlarını ve Azure ilkesini kullanın.  

Azure Resource Manager şablonlar, Azure kaynaklarını dağıtmak için kullanılan JavaScript Nesne Gösterimi (JSON) tabanlı dosyalardır, tüm özel şablonların bir kod deposunda güvenli bir şekilde depolanması ve saklanması gerekir. 

Azure kaynaklarınızın tamamında güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

- [Azure Resource Manager şablonları oluşturma hakkında bilgi](../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md)

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

- [Özel uç noktalar için şablon örnekleri Azure Resource Manager](/azure/templates/microsoft.network/2019-11-01/privateendpoints)

- [Özel bağlantı için Azure ilke örneği yerleşik bileşenleri](../governance/policy/samples/built-in-policies.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilkeleri, Azure Resource Manager şablonları ve Istenen durum yapılandırması betikleri gibi kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure DevOps kullanın. 

Erişim için Azure DevOps ile tümleşikse veya Team Foundation Server ile tümleşikse Active Directory, belirli kullanıcılara, yerleşik güvenlik gruplarına veya Azure Active Directory (Azure AD) tanımlanmış gruplara izin verin veya reddedin.

- [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow)

- [Azure DevOps 'da izinler ve gruplar hakkında](/azure/devops/organizations/security/about-permissions)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Kılavuz**: Azure Ilkesini kullanarak Azure kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. 

Azure kaynaklarınızın ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak üzere Azure Ilke diğer adlarını kullanın. 

Ayrıca abonelikleriniz altında yönetilen belirli kaynaklarla ilgili yerleşik ilke tanımlarından yararlanın.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Diğer adları kullanma](../governance/policy/concepts/definition-structure.md#aliases)

- [Özel bağlantı için Azure ilke örneği yerleşik bileşenleri](../governance/policy/samples/built-in-policies.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Kılavuz**: Azure Kaynak konfigürasyonları 'nı denetlemek Için Azure ilkesini kullanın. Örneğin, Azure Ilkesi, özel bir uç noktayla yapılandırılmayan kaynakları algılamak için kullanılabilir.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Özel bağlantı için Azure ilke örneği yerleşik bileşenleri](../governance/policy/samples/built-in-policies.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri kurtarma](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli otomatik arka pencerelere emin olun

**Kılavuz**: özel bağlantı Hizmetleri, uç noktalar ve ilgili kaynakları dağıtmak için Azure Resource Manager kullanın. Azure Resource Manager, özel bağlantı uç noktalarını ve ilgili kaynakları geri yüklemek için yedeklemeler olarak kullanılabilecek şablonları dışarı aktarma özelliğini sağlar. 

Azure Resource Manager şablonu dışarı aktarma API 'sini düzenli aralıklarla çağırmak için Azure Otomasyonu 'nu kullanın.

- [Azure Resource Manager'a genel bakış](../azure-resource-manager/management/overview.md)

- [Özel uç noktalar için şablon örnekleri Azure Resource Manager](/azure/templates/microsoft.network/2019-11-01/privateendpoints)

- [Azure portal bir şablona tek ve çoklu kaynak verme](../azure-resource-manager/templates/export-template-portal.md)

- [Kaynak grupları-şablonu dışarı aktarma](/rest/api/resources/resourcegroups/exporttemplate)

- [Azure Otomasyonu 'na giriş](../automation/automation-intro.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Kılavuz**: özel bağlantı Hizmetleri, uç noktalar ve ilgili kaynakları dağıtmak için Azure Resource Manager kullanın. Azure Resource Manager, özel bağlantı uç noktalarını ve ilgili kaynakları geri yüklemek için yedeklemeler olarak kullanılabilecek şablonları dışarı aktarma yeteneği sağlar.  

Azure Resource Manager şablonu dışarı aktarma API 'sini düzenli aralıklarla çağırmak için Azure Otomasyonu 'nu kullanın.  

Azure Key Vault içinde müşteri tarafından yönetilen anahtarları yedekleyin.

- [Azure Resource Manager'a genel bakış](../azure-resource-manager/management/overview.md)

- [Özel uç noktalar için şablon örnekleri Azure Resource Manager](/azure/templates/microsoft.network/2019-11-01/privateendpoints)

- [Azure portal bir şablona tek ve çoklu kaynak verme](../azure-resource-manager/templates/export-template-portal.md)

- [Kaynak grupları-şablonu dışarı aktarma](/rest/api/resources/resourcegroups/exporttemplate)

- [Azure Otomasyonu 'na giriş](../automation/automation-intro.md)

- [Azure 'da Anahtar Kasası anahtarlarını yedekleme](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: iş gereksinimlerinize göre yalıtılmış bir aboneliğe düzenli olarak Azure Resource Manager şablonlarının dağıtımını düzenli olarak gerçekleştirme yeteneğini doğrulayın. 

Ayrıca, yedeklenen müşteri tarafından yönetilen anahtarların geri yüklemelerini doğrulayın.

- [ARM şablonları ve Azure portal kaynak dağıtma](../azure-resource-manager/templates/deploy-portal.md)

- [Azure 'da Anahtar Kasası anahtarlarını geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: kodunuzu Azure Resource Manager şablonlar gibi güvenli bir şekilde depolamak ve yönetmek Için Azure DevOps kullanın. 

Bu kaynaklara erişim için Azure DevOps ile tümleşikse veya Team Foundation Server ile tümleşikse Active Directory, belirli kullanıcılara, yerleşik güvenlik gruplarına veya Azure Active Directory (Azure AD) tanımlanmış gruplara izin verin veya reddedin.

- [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow)

- [Azure DevOps 'da izinler ve gruplar hakkında](/azure/devops/organizations/security/about-permissions)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: olay yanıtı](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. 

Tüm personel rollerini ve olay işleme veya yönetim aşamalarını, algılama aşamasından olay sonrası incelemeye göre tanımlayan yazılı olay yanıt planlarını doğrulayın.

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu da kullanabilir](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her uyarıya önem derecesi atar. Önem derecesi, güvenlik merkezi 'nin uyarıyı vermek için kullanılan bulma veya analizte ne kadar önemli olduğunu ve uyarıya yönelik etkinliğin arkasında kötü amaçlı bir amaç olduğunu bilmenin yanı sıra güven düzeyini temel alır.

Etiketleri kullanarak abonelikleri (örneğin, üretim, üretim dışı) açık bir şekilde işaretleyin ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun.  

Müşteri, olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak uyarıların düzeltilme önceliklendirmesinden sorumludur.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: Azure kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

- [NıST 'nin Yayımlama Kılavuzu, BT planları ve becerileri için programları test etme, eğitim ve alıştırma Kılavuzu](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. 

Sorunların çözümlendiğinden emin olmak için olayları gözden geçirin, oluşumu gönderin.

- [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Kılavuz**: Azure kaynaklarına riskleri belirlemenize yardımcı olmak Için sürekli dışarı aktarma özelliğini kullanarak güvenlik merkezi uyarılarını ve önerilerini dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. 

Ayrıca, iş işlemlerinizin gerektirdiği gibi uyarıları Azure Sentinel 'e akışa almak için Güvenlik Merkezi veri bağlayıcısını kullanın.

- [Sürekli dışarı aktarmayı yapılandırma](../security-center/continuous-export.md)

- [Uyarıları Azure Sentinel 'e akış](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanarak Azure kaynaklarınızı korumaya yönelik güvenlik uyarılarındaki Logic Apps ve önerilerle yanıtları otomatik olarak tetikleyin.

- [Iş akışı otomasyonu ve Logic Apps yapılandırma](../security-center/workflow-automation.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: Penetme testleri ve Red ekibi alıştırmaları](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin. 

Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın.

- [Katılım test kurallarını oyma](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Bulut Kırmızı ekip oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
