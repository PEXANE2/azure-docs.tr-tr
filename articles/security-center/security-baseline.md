---
title: Güvenlik Merkezi için Azure Güvenlik temeli
description: Güvenlik Merkezi güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: security-center
ms.topic: conceptual
ms.date: 08/05/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 0809c9eb1f64dc6a505ef50e25f973aa041d186d
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88004477"
---
# <a name="azure-security-baseline-for-security-center"></a>Güvenlik Merkezi için Azure Güvenlik temeli

Bu güvenlik temeli [Azure Güvenlik kıyaslayıcılarından](../security/benchmarks/overview.md) Azure Güvenlik Merkezi 'ne kılavuzluk uygular. Azure Güvenlik kıyaslaması, bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimlerine** ve Azure Güvenlik Merkezi 'ne uygulanan ilgili kılavuza göre gruplandırılır. Azure Güvenlik Merkezi için geçerli olmayan **denetimler** dışlandı. Azure Güvenlik Merkezi 'nin Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam Azure Güvenlik Merkezi güvenlik temeli eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ağ güvenliği](/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

**Rehberlik**: Azure Güvenlik Merkezi, temel bir Azure sunumudur. Bir sanal ağ, alt ağ veya ağ güvenlik grubunu doğrudan güvenlik merkezi ile ilişkilendiremezsiniz. İşlem kaynaklarınız için veri toplamayı etkinleştirirseniz, güvenlik merkezi bir Log Analytics çalışma alanı aracılığıyla topladığı verileri depolar, bu çalışma alanını, sanal ağınızdaki özel bir uç nokta üzerinden çalışma alanı verilerinize erişim için özel bağlantı kullanacak şekilde yapılandırabilirsiniz. Ayrıca, veri toplama Güvenlik Merkezi kullanılıyorsa, güvenlik verilerini toplamak ve bu işlem kaynaklarına koruma sağlamak için sunucularınıza dağıtılan Log Analytics aracısını kullanır. Log Analytics Aracısı, güvenlik merkezi ile uygun işlemler için belirli bağlantı noktalarının ve protokollerin açılmasını gerektirir. Ağlarınızı yalnızca gerekli bağlantı noktalarına ve protokollere izin verecek şekilde kilitleyin ve yalnızca uygulamanızın ağ güvenlik grupları aracılığıyla çalışması için gereken ek kurallar ekleyin.

- [Azure Güvenlik Merkezinde veri toplama](security-center-enable-data-collection.md)

- [Ağ güvenlik grubuyla Filer ağ trafiği](../virtual-network/tutorial-filter-network-traffic.md)

- [Log Analytics aracısını kullanmaya yönelik güvenlik duvarı gereksinimleri](../azure-monitor/platform/log-analytics-agent.md#firewall-requirements)

- [Azure özel bağlantısını anlama](../private-link/private-link-overview.md) 

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure Güvenlik Merkezi teklifi doğrudan bir sanal ağla tümleştirilemez, ancak ağlarınızda dağıtılan Log Analytics aracılarıyla yapılandırılmış sunuculardan veri toplayabilir. Güvenlik Merkezi 'ne veri gönderecek şekilde yapılandırılmış sunucularınız, belirli bağlantı noktalarının ve protokollerin düzgün iletişim kurmasına izin verilmesini gerektirir. Bu ağ kaynakları için Azure Ilkesiyle standart güvenlik yapılandırması belirleyin ve uygulayın.

Ayrıca, tek bir şema tanımında Azure Resource Manager şablonları, rol atamaları ve Azure ilke atamaları gibi temel ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Güvenlik Merkezi yapılandırmalarının ve ilgili ağ kaynaklarının tutarlı ve güvenli bir şekilde dağıtılması için şema 'i yeni aboneliklere uygulayabilirsiniz.

- [Azure Güvenlik Merkezinde veri toplama](security-center-enable-data-collection.md)

- [Log Analytics aracısını kullanmaya yönelik güvenlik duvarı gereksinimleri](../azure-monitor/platform/log-analytics-agent.md#firewall-requirements)

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md) 

- [Ağ için Azure Ilke örnekleri](../governance/policy/samples/built-in-policies.md#network)

- [Azure Blueprint oluşturma](../governance/blueprints/create-blueprint-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Kılavuz**: Azure Güvenlik Merkezi teklifi doğrudan bir sanal ağla tümleştirilemez, ancak ağlarınızda dağıtılan Log Analytics aracılarıyla yapılandırılmış sunuculardan veri toplayabilir. Güvenlik Merkezi 'ne veri gönderecek şekilde yapılandırılmış sunucularınız, belirli bağlantı noktalarının ve protokollerin düzgün iletişim kurmasına izin verilmesini gerektirir. Bu ağ kaynakları için Azure Ilkesiyle standart güvenlik yapılandırması belirleyin ve uygulayın.

Azure Güvenlik Merkezi 'ne güvenlik günlükleri gönderecek şekilde yapılandırılmış ağlarınızdaki sunucular gibi ağ güvenlik grupları ve diğer kaynaklar için kaynak etiketleri kullanın. Tek tek ağ güvenlik grubu kuralları için "Açıklama" alanını kullanarak bir ağdan gelen/giden trafiğe izin veren kuralları belgeleyin. 

Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure Ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynakları arayabilir veya bunlarla ilgili eylemler gerçekleştirebilirsiniz. 

- [Azure Güvenlik Merkezinde veri toplama](security-center-enable-data-collection.md)

- [Log Analytics aracısını kullanmaya yönelik güvenlik duvarı gereksinimleri](../azure-monitor/platform/log-analytics-agent.md#firewall-requirements)

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags) 

- [Azure sanal ağı oluşturma](../virtual-network/quick-create-portal.md) 

- [Ağ güvenlik grubu kuralları ile ağ trafiğini filtreleme](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak kaynak yapılandırmasını Izleyin ve Azure Güvenlik Merkezi ile ilgili ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik kaynaklardaki değişiklikler gerçekleşirken size bildirimde bulunan Azure Izleyici 'de uyarılar oluşturun.

- [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](/azure/azure-monitor/platform/activity-log-view) 

- [Azure Izleyici 'de uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: günlüğe kaydetme ve izleme](/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Kılavuz**: Azure Güvenlik Merkezi tarafından oluşturulan Birleşik güvenlik verileri ve merkezi bir Log Analytics çalışma alanı kullanılarak bağlı kaynakları. 

Güvenlik Merkezi 'nin veri toplamayı, bağlı Azure işlem kaynaklarınızdan bir merkezi Log Analytics çalışma alanına güvenlik verileri ve olayları gönderecek şekilde yapılandırın. Veri toplamaya ek olarak, merkezi Log Analytics çalışma alanınıza Güvenlik Merkezi tarafından oluşturulan güvenlik uyarılarını ve önerilerini akışa almak için sürekli dışarı aktarma özelliğini kullanın. Azure Izleyici 'de, güvenlik merkezi 'nden ve bağlı Azure kaynaklarınızdan oluşturulan güvenlik verilerinde analiz sorgulayabilir ve analiz gerçekleştirebilirsiniz. 

Alternatif olarak, Güvenlik Merkezi tarafından üretilen verileri Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye gönderebilirsiniz.

- [Güvenlik uyarılarını ve önerilerini dışarı aktarma](continuous-export.md)

- [Azure Güvenlik Merkezinde veri toplama](security-center-enable-data-collection.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md) 

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md) 

- [Azure Izleyici ile Azure sanal makine iç konak günlüklerini toplama](../azure-monitor/learn/quick-collect-azurevm.md)

- [Azure Izleyici ve üçüncü taraf SıEM tümleştirmesi ile çalışmaya başlama](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: Azure izleyici etkinlik günlükleri otomatik olarak kullanılabilir, bu Günlükler, Azure Güvenlik Merkezi gibi, kaynak için tüm işlemler, kimin hangi işlemleri başlattıkları ve ne zaman meydana getirildikleri gibi tüm yazma işlemlerini içerir. Azure etkinlik günlüklerinizi, günlük birleştirme ve daha fazla bekletme için bir Log Analytics çalışma alanına gönderin.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md) 

- [Azure 'da günlüğe kaydetme ve farklı günlük türlerini anlama](../azure-monitor/platform/platform-logs-overview.md)

- [Log Analytics çalışma alanına etkinlik günlükleri gönderme](../azure-monitor/platform/activity-log.md#send-to-log-analytics-workspace)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de kuruluşunuzun uyumluluk düzenlemelerine göre Log Analytics çalışma alanı saklama süresini ayarlayın. Uzun süreli ve arşiv depolama için Azure depolama hesaplarını kullanın. 

- [Log Analytics veri saklama süresini değiştirme](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period) 

- [Azure depolama hesabı günlükleri için bekletme ilkesini yapılandırma](../storage/common/storage-monitor-storage-account.md#configure-logging)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: Azure Güvenlik Merkezi tarafından oluşturulan günlükleri ve anormal davranışlar için bağlı kaynaklarını çözümleyin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici 'yi ve bir Log Analytics çalışma alanını kullanın.

Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz. 

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md) 

- [Log Analytics sorguları ile çalışmaya başlama](../azure-monitor/log-query/get-started-portal.md) 

- [Azure Izleyici 'de özel sorgular gerçekleştirme](../azure-monitor/log-query/get-started-queries.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Rehberlik**: etkinlik Günlüğiz veya Azure Güvenlik Merkezi tarafından oluşturulan veriler tarafından kaydedilen istenmeyen veya anormal etkinlikleri sorgulamak Için Azure izleyici günlük uyarılarını yapılandırın. Kuruluşunuzun bilgilendirilmesine ve anormal etkinlik için bir günlük uyarısı başlatılmışsa işlem yapabilmesi için eylem gruplarını ayarlayın. Güvenlik uyarıları ve önerilerinde Logic Apps 'i tetiklemek için güvenlik merkezi iş akışı Otomasyonu özelliğini kullanın. Güvenlik Merkezi iş akışları, kullanıcılara olay yanıtı bildirmek için kullanılabilir veya uyarı bilgilerine göre kaynakları düzeltmeye yönelik işlemler gerçekleştirebilir.

Alternatif olarak, Azure Güvenlik Merkezi tarafından Azure Sentinel 'e ilişkin ve ile ilgili ve şirket içi verileri etkinleştirebilir ve bu verilere erişebilirsiniz. Azure Sentinel, güvenlikle ilgili sorunlara yönelik otomatik tehdit yanıtlarına izin veren PlayBook 'ları 'ları destekler.

- [Azure Güvenlik Merkezi iş akışı Otomasyonu](workflow-automation.md)

- [Azure Güvenlik Merkezi 'nde uyarıları yönetme](security-center-managing-and-responding-alerts.md) 

- [Log Analytics günlük verilerinde uyarı alma](../azure-monitor/learn/tutorial-response.md)

- [Azure Sentinel 'de otomatik tehdit yanıtlarını ayarlama](../sentinel/tutorial-respond-threats-playbook.md)

- [Azure Izleyici 'de günlük uyarıları](../azure-monitor/platform/alerts-unified-log.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kimlik ve erişim denetimi](/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Kılavuz**: Azure rol tabanlı erişim denetimi (Azure RBAC), Azure kaynaklarına erişimi rol atamaları aracılığıyla yönetmenizi sağlar. Bu rolleri kullanıcılara atayabilir, hizmet sorumlularını ve yönetilen kimlikleri gruplara atayabilirsiniz. Belirli kaynaklar için önceden tanımlı yerleşik roller vardır ve bu roller Azure CLı, Azure PowerShell veya Azure portal gibi araçlarla envantere alınabilir veya sorgulanabilir. Azure Güvenlik Merkezi ' güvenlik okuyucusu ' veya ' güvenlik Admin' için yerleşik rollere sahiptir ve bu sayede kullanıcıların güvenlik ilkelerini okumasına veya güncelleştirmesine ve uyarıları ve önerileri kapatabilir.

- [Azure Güvenlik Merkezi'nde İzinler](security-center-permissions.md)

- [Azure AD 'de PowerShell ile dizin rolü alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0) 

- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: Azure platformu veya Azure Güvenlik Merkezi teklifine özel yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Azure Active Directory 'deki yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın. Güvenlik Merkezi 'nde Ayrıca, kullanıcıların güvenlik ilkelerini güncelleştirmesine ve uyarıları ve önerileri açmasına olanak tanıyan ' güvenlik Admin' ' yerleşik rolleri vardır. bu rol atamasına sahip tüm kullanıcıları düzenli olarak inceleyip mutabık kılmanız gerekir.

Ayrıca, özel yönetim hesaplarını izlemenize yardımcı olmak için Azure Güvenlik Merkezi veya yerleşik Azure Ilkeleri önerilerini kullanabilirsiniz; örneğin:

- Aboneliğinize birden fazla sahip atanmalıdır
- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

- [Azure Güvenlik Merkezi'nde İzinler](security-center-permissions.md)

- [Kimlik ve erişim (Önizleme) izlemek için Azure Güvenlik Merkezi 'ni kullanma](security-center-identity-access.md)

- [Azure Ilkesini kullanma](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: mümkün olan yerlerde, tek başına bağımsız kimlik bilgilerini hizmet başına yapılandırmak yerıne Azure Active Directory SSO kullanın. Azure Güvenlik Merkezi kimlik ve erişim önerilerini kullanın.

- [Azure AD ile SSO 'yu anlama](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Kılavuz**: Azure Güvenlik merkezi ve Azure Portal erişim IÇIN Azure Active Directory MFA 'yı etkinleştirme, tüm güvenlik merkezi kimliklerini ve erişim önerilerini izleyin. 

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md) 

- [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](security-center-identity-access.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Rehberlik**: yükseltilmiş ayrıcalıklar gerektiren yönetim görevleri için güvenli, Azure tarafından yönetilen bir iş Istasyonu (ayrıcalıklı erişim iş istasyonu veya Paw olarak da bilinir) kullanın.

- [Güvenli, Azure tarafından yönetilen iş istasyonlarını anlayın](../active-directory/devices/concept-azure-managed-workstation.md)

- [Azure AD MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan etkinliklerin ne zaman oluştuğunu algılamak için Azure Active Directory güvenlik raporları ve izleme kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın.

- [Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme](/azure/active-directory/reports-monitoring/concept-user-at-risk) 

- [Azure Güvenlik Merkezi 'nde kullanıcıların kimlik ve erişim etkinliğini izleme](security-center-identity-access.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek IÇIN Azure AD adlandırılmış konumlarını kullanın. 

- [Azure AD adlandırılmış konumlarını yapılandırma](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Kılavuz**: Azure Güvenlik Merkezi 'ni kullanırken merkezi kimlik doğrulaması ve yetkilendirme sistemi olarak Azure Active Directory (Azure AD) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar. Azure Güvenlik Merkezi 'nde, kullanıcıların güvenlik ilkelerini güncelleştirmesine ve uyarıları ve önerileri açmasına olanak tanıyan ' güvenlik Admin' ' gibi atanabilir yerleşik roller vardır.

- [Azure Güvenlik Merkezi'nde İzinler](security-center-permissions.md)

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure Active Directory eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini verimli bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure AD kimlik ve erişim gözden geçirmeleri ' nı kullanın. Azure Güvenlik Merkezi ile ilgili Kullanıcı erişimi, yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için düzenli aralıklarla gözden geçirilebilir. 

- [Azure AD raporlamayı anlama](/azure/active-directory/reports-monitoring/) 

- [Azure AD kimlik ve erişim gözden geçirmeleri kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Rehberlik**: herhangi bir SIEM/izleme aracıyla tümleştirmenize olanak tanıyan Azure AD oturum açma etkinliğine, denetimine ve risk olay günlüğü kaynaklarına erişiminiz vardır. 

Bu işlemi, Azure AD Kullanıcı hesapları için Tanılama ayarları oluşturarak ve Log Analytics çalışma alanına denetim günlüklerini ve oturum açma günlüklerini göndererek kolaylaştırabilirsiniz. İstenen uyarıları Log Analytics çalışma alanı içinde yapılandırabilirsiniz.  

- [Azure Izleyici ile Azure etkinlik günlüklerini tümleştirme](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar 

**Rehberlik**: otomatik yanıtları yapılandırmak için Azure AD kimlik koruması özellikleri kullanarak Kullanıcı kimlikleriyle ilgili şüpheli eylemleri tespit edin. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz. 

- [Azure AD riskli oturum açma işlemlerini görüntüleme](/azure/active-directory/reports-monitoring/concept-risky-sign-ins) 

- [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md) 

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri koruma](/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Kılavuz**: Azure Güvenlik Merkezi 'nden hassas güvenlik bilgilerini depolayan Log Analytics çalışma alanı gibi Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: ortam türü ve veri duyarlılığı düzeyi gibi bireysel güvenlik etki alanları için ayrı abonelikler ve yönetim grupları kullanarak yalıtım uygulayın. Uygulamalarınızın ve kurumsal ortamların talep ettiği Azure kaynaklarınıza erişim düzeyini kısıtlayabilirsiniz. Azure RBAC aracılığıyla Azure kaynaklarına erişimi denetleyebilirsiniz.

Varsayılan olarak, Azure Güvenlik Merkezi verileri, güvenlik merkezi arka uç hizmetinde depolanır. Kuruluşunuzda bu verileri kendi kaynaklarınıza depolamak için gereksinimler eklendiyse, güvenlik merkezi verilerini, uyarılarını ve önerilerini depolamak üzere bir Log Analytics çalışma alanı yapılandırabilirsiniz. Kendi çalışma alanınızı kullanırken, verilerin kaynaklandığı ortama göre farklı çalışma alanlarını yapılandırarak daha fazla ayrım ekleyebilirsiniz.

- [Güvenlik uyarılarını ve önerilerini dışarı aktarma](continuous-export.md)

- [Azure Güvenlik Merkezinde veri toplama](security-center-enable-data-collection.md)

- [Ek Azure abonelikleri oluşturma](/azure/billing/billing-create-subscription) 

- [Yönetim grupları oluşturma](../governance/management-groups/create.md) 

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: yoldaki tüm hassas bilgileri şifreleyin. Azure kaynaklarınıza bağlanan tüm istemcilerin TLS 1,2 veya üzerini anlaşamadığından emin olun. Log Analytics aracısıyla yapılandırılan ve Azure Güvenlik Merkezi 'ne veri gönderecek sanal makineler TLS 1,2 kullanacak şekilde yapılandırılmalıdır.

Azure Güvenlik Merkezi önerilerini, varsa, bekleyen ve geçişte şifreleme için kullanın. 

- [Log Analytics verileri güvenli bir şekilde gönderme](../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12)

- [Azure ile iletim sırasında şifrelemeyi anlama](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma 

**Kılavuz**: Azure Güvenlik Merkezi ile ilgili veri ve kaynaklara erişimi yönetmek için Azure rol tabanlı erişim denetimi (Azure RBAC) kullanın. Azure Güvenlik Merkezi ' güvenlik okuyucusu ' veya ' güvenlik Admin' için yerleşik rollere sahiptir ve bu sayede kullanıcıların güvenlik ilkelerini okumasına veya güncelleştirmesine ve uyarıları ve önerileri kapatabilir. Güvenlik Merkezi tarafından toplanan verileri depolayan Log Analytics çalışma alanında, ' Log Analytics okuyucu ', ' Log Analytics katkıda bulunan ' ve diğerleri gibi atayabileceğiniz yerleşik roller de vardır. Kullanıcıların gerekli görevlerini tamamlaması için gereken en az izin veren rolü atayın. Örneğin, okuyucu rolünü yalnızca bir kaynağın güvenlik durumu ile ilgili bilgileri görüntülemesi gereken ancak öneri veya ilkeleri Düzenle gibi işlemleri olmayan kullanıcılara atayın.

- [Azure Log Analytics çalışma alanı izinleri](../role-based-access-control/built-in-roles.md#log-analytics-reader)

- [Azure Güvenlik Merkezi'nde İzinler](security-center-permissions.md)

- [Azure RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Kılavuz**: Azure Güvenlik Merkezi, oluşturduğu verileri, uyarıları ve önerileri depolamak için yapılandırılmış bir Log Analytics çalışma alanı kullanır. Güvenlik Merkezi veri toplama için yapılandırdığınız çalışma alanı için müşteri tarafından yönetilen bir anahtar (CMK) yapılandırın. CMK, çalışma alanına kaydedilen veya gönderilen tüm verilerin, sizin tarafınızdan oluşturulan ve size ait bir Azure Key Vault anahtarla şifrelenmesini sağlar. 

- [Azure Izleyici müşteri tarafından yönetilen anahtar](../azure-monitor/platform/customer-managed-keys.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Güvenlik Merkezi ile Ilgili kritik Azure kaynaklarında değişiklik yapıldığında uyarılar oluşturmak Için Azure izleyici 'yi kullanın. Bu değişiklikler, güvenlik merkezi ile ilgili, uyarıların veya önerilerin devre dışı bırakılması ya da veri depolarının güncelleştirilmesi ya da silinmesi gibi yapılandırmaların değiştirileceği herhangi bir eylem içerebilir.

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: güvenlik açığı yönetimi](/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: ortak bir risk Puanlama programını (örneğin, ortak güvenlik açığı Puanlama sistemi) veya üçüncü taraf tarama aracınız tarafından sunulan varsayılan risk derecelendirmelerini kullanın.

- [NıST yayını--ortak güvenlik açığı Puanlama sistemi](https://www.nist.gov/publications/common-vulnerability-scoring-system)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: envanter ve varlık yönetimi](/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: aboneliklerinizde Azure Güvenlik Merkezi ile ilgili tüm kaynakları sorgulamak ve öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinlere sahip olun ve Güvenlik Merkezi kaynaklarını öğrenmek için tüm Azure aboneliklerini numaralandırın. 

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md) 

- [Azure aboneliklerinizi görüntüleme](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0) 

- [Azure RBAC 'yi anlama](../role-based-access-control/overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure Güvenlik Merkezi 'nden hassas güvenlik bilgilerini depolayan Log Analytics çalışma alanı gibi Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure Güvenlik Merkezi kaynaklarını düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ayrıca, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerinde oluşturulabilen kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

- [Ek Azure abonelikleri oluşturma](/azure/billing/billing-create-subscription)

- [Yönetim Grupları oluşturma](../governance/management-groups/create.md)

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynaklarının envanterini tanımlayın ve saklayın

**Rehberlik**: Kurumsal gereksinimlerinize göre işlem kaynakları Için onaylanan Azure kaynakları ve onaylanan yazılım envanterini oluşturun.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aboneliklerinizde oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın. 

Azure Kaynak Grafiği 'ni kullanarak aboneliklerindeki kaynakları sorgulama ve bulma.  Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun. 

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md) 

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Kılavuz**: Azure Güvenlik Merkezi Ile ilgili Azure kaynaklarını, kuruluşunuzun envanterinin ve gözden geçirme sürecinin bir parçası olarak artık gerekli olmadığında kaldırın.

- [Azure Kaynak grubu ve kaynak silme](../azure-resource-manager/management/delete-resource-group.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak aboneliklerinizde oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak üzere Azure koşullu erişimini yapılandırın.

- [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: güvenli yapılandırma](/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: Azure Ilkesi aracılığıyla Azure Güvenlik Merkezi ve bağlı çalışma alanı için standart güvenlik yapılandırması tanımlayın ve uygulayın. Güvenlik Merkezi ve Log Analytics çalışma alanı yapılandırmasını denetlemek veya zorlamak üzere özel Azure Ilke tanımları oluşturmak için "Microsoft. Operationalınsights" ve "Microsoft. Security" ad alanlarında Azure Ilke diğer adlarını kullanın.

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak için ' Reddet ' ve ' dağıtım yok ' Için Azure ilke efektlerini kullanın. Ayrıca, kuruluşunuz tarafından gerekli olan Azure kaynaklarınızın güvenlik yapılandırmasını sürdürmek için Azure Resource Manager şablonlarını kullanabilirsiniz. 

- [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md) 

- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md) 

- [Azure Resource Manager şablonlarına genel bakış](../azure-resource-manager/templates/overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları, Azure Resource Manager şablonları ve istenen durum yapılandırması betikleri gibi kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure DevOps kullanın. Azure DevOps 'da yönettiğiniz kaynaklara erişmek için, Azure DevOps ile tümleşikse veya TFS ile tümleşikse Active Directory belirli kullanıcılara, yerleşik güvenlik gruplarına veya Azure Active Directory (Azure AD) tanımlanmış gruplara izin verebilir veya vermeyebilirsiniz. 

- [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops) 

- [Azure DevOps 'da izinler ve gruplar hakkında](/azure/devops/organizations/security/about-permissions)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Kılavuz**: Azure Ilkesini kullanarak Azure kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure Güvenlik Merkezi ile ilgili kaynaklarınızın yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak üzere Azure Ilke diğer adlarını kullanın. Ayrıca, yapılandırma değişikliklerini dağıtmak için Azure Otomasyonu ' nu kullanabilirsiniz. 

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md) 

- [Diğer adları kullanma](../governance/policy/concepts/definition-structure.md#aliases)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Rehberlik**: yerleşik Azure ilke tanımlarını ve "Microsoft. Operationalınsights" ve "Microsoft 'un Azure ilkesi diğer adlarını kullanın. Güvenlik "Azure Kaynak yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmaya yönelik ad alanları. Azure kaynaklarınızın yapılandırmasını otomatik olarak zorlamak için Azure ilke efektlerini "Audit", "Deny" ve "dağıtım yoksa dağıt" kullanın.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Kılavuz**: Azure Güvenlik Merkezi, oluşturduğu verileri, uyarıları ve önerileri depolamak için yapılandırılmış bir Log Analytics çalışma alanı kullanır. Güvenlik Merkezi veri toplama için yapılandırdığınız çalışma alanı için müşteri tarafından yönetilen bir anahtar (CMK) yapılandırın. CMK, çalışma alanına kaydedilen veya gönderilen tüm verilerin, sizin tarafınızdan oluşturulan ve size ait bir Azure Key Vault anahtarla şifrelenmesini sağlar. 

- [Azure Izleyici müşteri tarafından yönetilen anahtar](../azure-monitor/platform/customer-managed-keys.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kötü amaçlı yazılımdan koruma](/azure/security/benchmarks/security-control-malware-defense).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Azure Güvenlik Merkezi, dosyaları depolamak veya işlemek için tasarlanmamıştır. Log Analytics çalışma alanı dahil, işlem dışı Azure kaynaklarına karşıya yüklenen tüm içerikleri önceden taramak sizin sorumluluğunuzdadır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri kurtarma](/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın 

**Rehberlik**: kod olarak altyapı (IAC) yaklaşımını Izleyin ve Azure Güvenlik Merkezi ile ilgili kaynaklarınızı, kaynakla ilgili yapılandırmalara yönelik yedekleme olarak kullanılabilecek bir JAVASCRIPT nesne GÖSTERIMI (JSON) şablonunda dağıtmak için Azure Resource Manager kullanın.

- [Azure portal bir şablona tek ve çoklu kaynak verme](../azure-resource-manager/templates/export-template-portal.md)

- [Güvenlik kaynağı için Azure Resource Manager şablonları](/azure/templates/microsoft.security/allversions)

- [Azure Otomasyonu hakkında](../automation/automation-intro.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Kılavuz**: Azure Güvenlik Merkezi, oluşturduğu verileri, uyarıları ve önerileri depolamak için bir Log Analytics çalışma alanı kullanır. Azure Izleyicisini ve Güvenlik Merkezi 'nin müşterinin yönettiği bir anahtarı etkinleştirmek için kullandığı çalışma alanını yapılandırabilirsiniz. Müşteri tarafından yönetilen anahtarlarınızı depolamak için bir Key Vault kullanıyorsanız, anahtarlarınızın düzenli otomatik yedeklemelerini sağlayın.

- [Key Vault anahtarlarını yedekleme](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: Azure Resource Manager desteklenen şablon dosyalarını kullanarak düzenli aralıklarla geri yükleme gerçekleştirme olanağı sağlayın. Yedeklenen müşteri tarafından yönetilen anahtarların test geri yüklemesi.

- [Azure Resource Manager şablonları kullanarak Log Analytics çalışma alanını yönetme](../azure-monitor/platform/template-workspace-configuration.md)

- [Azure 'da Anahtar Kasası anahtarlarını geri yükleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Kılavuz**: özel Azure ilke tanımları ve Azure Resource Manager şablonları gibi kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure DevOps kullanın. Azure DevOps 'da yönettiğiniz kaynakları korumak için, belirli kullanıcılara, yerleşik güvenlik gruplarına veya Azure DevOps ile tümleşikse veya TFS ile tümleşikse Active Directory Azure Active Directory (Azure AD) içinde tanımlanan gruplara izin verebilir veya vermeyebilirsiniz. Müşteri tarafından yönetilen anahtarları korumak için rol tabanlı erişim denetimi kullanın.

Ayrıca, anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault ' de geçici silme ve Temizleme korumasını etkinleştirin.  Azure Resource Manager şablon yedeklemelerini depolamak için Azure depolama kullanılıyorsa, Bloblar veya blob anlık görüntüleri silindiğinde verilerinizi kaydetmek ve kurtarmak için geçici silme özelliğini etkinleştirin. 

- [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure DevOps 'da izinler ve gruplar hakkında](/azure/devops/organizations/security/about-permissions)

- [Key Vault 'da geçici silme ve Temizleme korumasını etkinleştirme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal) 

- [Azure Depolama blobları için geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: olay yanıtı](/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu geliştirin. Tüm personel rollerinin yanı sıra olay işleme ve yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun. 

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/) 

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/) 

- [Kendi olay yanıtı planınızın oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu kullanın](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Kılavuz**: Azure Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri kullanarak abonelikleri işaretleyin ve Azure kaynaklarını tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun.  Olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak, uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır. 

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](security-center-alerts-overview.md) 

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: Azure kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip yanıt planınızı gerektiği gibi düzeltin. 

- [NıST yayını--BT planları ve becerileri için programları test etme, eğitim ve alıştırmaya yönelik kılavuz](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin. 

- [Azure Güvenlik Merkezi güvenlik ilgili kişisini ayarlama](security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Kılavuz**: Azure kaynaklarına yönelik riskleri belirlemenize yardımcı olmak için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e aktarabilirsiniz. 

- [Sürekli dışarı aktarmayı yapılandırma](continuous-export.md) 

- [Uyarıları Azure Sentinel 'e akış](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: iş akışı Otomasyonu özelliği Azure Güvenlik Merkezi 'Ni kullanarak Azure kaynaklarınızı korumaya yönelik güvenlik uyarılarına ve önerilerine yönelik yanıtları otomatik olarak tetikleyin. 

- [Güvenlik Merkezi 'nde iş akışı Otomasyonu nasıl yapılandırılır](workflow-automation.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: Penetme testleri ve Red ekibi alıştırmaları](/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft bulut Penme test kurallarını izleyin. Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın. 

- [Katılım test kurallarını oyma](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Bulut Kırmızı ekip oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
