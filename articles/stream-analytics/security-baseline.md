---
title: Stream Analytics için Azure Güvenlik temeli
description: Stream Analytics için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 06/05/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 9d085ba494ea6bb6e9e80490d85e50f100fc0908
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84485654"
---
# <a name="azure-security-baseline-for-stream-analytics"></a>Stream Analytics için Azure Güvenlik temeli

Stream Analytics için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

**Rehberlik**: Azure Stream Analytics ağ güvenlik grupları (NSG) ve Azure Güvenlik duvarının kullanımını desteklemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: sanal ağların, alt ağların ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Rehberlik**: Azure Stream Analytics sanal ağların ve alt ağların kullanımını desteklemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: Azure Güvenlik Merkezi tehdit koruması 'nı kullanarak bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri algılayabilir ve uyarır.

* [Azure Güvenlik Merkezi 'nde Azure hizmet katmanı için tehdit koruması](https://docs.microsoft.com/azure/security-center/threat-protection)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets"></a>1,5: ağ paketlerini kaydetme

**Rehberlik**: Azure Stream Analytics ağ güvenlik gruplarını kullanmaz ve Azure Key Vault akış günlükleri yakalanmaz.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: Azure abonelik ortamınızda olağandışı veya potansiyel olarak zararlı işlemleri algılamak Için Azure Güvenlik Merkezi tehdit koruması 'nı kullanın.

* [Azure Güvenlik Merkezi 'nde Azure hizmet katmanı için tehdit koruması](https://docs.microsoft.com/azure/security-center/threat-protection)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: Azure Stream Analytics sanal ağların ve ağ kurallarının kullanımını desteklemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure Stream Analytics sanal ağların ve ağ cihazlarının kullanımını desteklemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: Azure Stream Analytics sanal ağların ve trafik yapılandırma kurallarının kullanımını desteklemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü ' ne kaynak yapılandırmasını izlemek ve Stream Analytics kaynaklarınızın değişikliklerini algılamak için kullanın. Kritik kaynaklardaki değişiklikler gerçekleşirken tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

* [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Azure Izleyici 'de uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, Stream Analytics gibi Azure kaynakları için kullanılan zaman kaynağını korur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: denetim olayları ve istekleri gibi güvenlik verilerini toplamak Için Azure izleyici aracılığıyla günlükleri alma. Azure Izleyici 'de, analiz yapmak ve analiz yapmak için Log Analytics çalışma alanlarını kullanın ve isteğe bağlı olarak, sabit depolama ve zorlanan bekletme tutma gibi güvenlik özellikleriyle uzun süreli/arşiv depolama için Azure Storage accountytym kullanın.

* [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: yönetim, güvenlik ve tanılama günlüklerine erişim Için Azure Stream Analytics tanılama ayarlarını etkinleştirin. Ayrıca, Azure etkinlik günlüğü tanılama ayarlarını da etkinleştirebilir ve günlükleri aynı Log Analytics çalışma alanına veya depolama hesabına gönderebilirsiniz.

* [Azure Stream Analytics, gözden geçirme için tanılama günlükleri ve etkinlik verileri sağlar](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure depolama hesabında veya Log Analytics çalışma alanında güvenlik olay günlüklerini depolarken, saklama ilkesini kuruluşunuzun gereksinimlerine göre ayarlayabilirsiniz.

* [Azure Stream Analytics, gözden geçirme için tanılama günlükleri ve etkinlik verileri sağlar](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)

* [Azure depolama hesabı günlükleri için bekletme ilkesini yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging)

* [Log Analytics veri saklama süresini değiştirme](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: anormal davranışlar için günlükleri çözümleyin ve izleyin ve Stream Analytics kaynaklarınızın sonuçlarını düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics çalışma alanını kullanın. Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Log Analytics çalışma alanı hakkında daha fazla bilgi için](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)

* [Azure Izleyici 'de özel sorgular gerçekleştirme](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Rehberlik**: Stream Analytics Için tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Log Analytics çalışma alanınızı Azure Sentinel 'e ekleyin. Bu, bir güvenlik Orchestration otomatik yanıtı (SOAR) çözümü sağlar. Bu, güvenlik sorunlarını gidermek için PlayBook 'ları (otomatikleştirilmiş çözümlerin) oluşturulmasına ve kullanılmasına olanak tanır.

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Log Analytics günlük verilerinde uyarı alma](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)

* [Azure Stream Analytics, gözden geçirme için tanılama günlükleri ve etkinlik verileri sağlar](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; Stream Analytics kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Kılavuz**: Azure izleyici 'de Azure DNS Analytics (Önizleme) çözümü, güvenlik, performans ve işlemlerde DNS altyapısına ilişkin Öngörüler toplar. Şu anda bu Azure Stream Analytics desteklemez, ancak üçüncü taraf DNS günlüğü çözümünü kullanabilirsiniz.

* [DNS Analizi Preview çözümüyle DNS altyapınız hakkında Öngörüler toplayın](https://docs.microsoft.com/azure/azure-monitor/insights/dns-analytics)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Kılavuz**: Azure AD 'nin açıkça atanması gereken yerleşik rolleri vardır. Rolleri, üyeliği bulacak şekilde sorgulabilirler. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

* [Azure AD 'de PowerShell ile dizin rolü alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

* [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Stream Analytics kimlik doğrulaması, Azure Active Directory ile birlikte sağlandığı ve hizmeti yönetmek için rol tabanlı erişim DENETIMLERI (RBAC) tarafından sağlandığı için varsayılan parola kavramına sahip değildir. Ekleme akışı hizmetlerine ve çıkış hizmetlerine bağlı olarak, işlerinde yapılandırılan kimlik bilgilerini döndürmeniz gerekir.

* [Stream Analytics işinin girdileri ve çıkışları için oturum açma kimlik bilgilerini döndürün](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-login-credentials-inputs-outputs)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: yönetici rollerine en az ayrıcalıklı erişim ilkesi dahil olmak üzere en iyi uygulamaları takip eden bir kimlik yönetimi ve rol güvenlik planı oluşturun. Azure AD ve Azure kaynaklarına tam zamanında ayrıcalıklı erişim sağlamak için Azure Privileged Identity Management (PıM) kullanın. Yönetim hesaplarının etkinliğini izlemek için Azure PıM uyarılarını ve denetim geçmişini kullanın. Güvenliği aşılmış olabilecek yönetim hesaplarını belirlemenize yardımcı olması için Azure AD güvenlik raporlarını kullanın.

* [Daha fazla bilgi](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: mümkün olan yerlerde, hizmet başına tek başına kimlik bilgilerini yapılandırmak yerıne Azure Active Directory SSO kullanın. Azure Güvenlik Merkezi kimlik &amp; erişim önerilerini uygulayın.

* [Azure AD ile SSO 'yu anlama](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Active Directory Multi-Factor Authentication 'ı (MFA) etkinleştirin ve Stream Analytics kaynaklarınızın korunmasına yardımcı olmak Için Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

* [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Rehberlik**: çok faktörlü kimlik DOĞRULAMASıYLA (MFA) Stream Analytics kaynakları oturum açmak ve yapılandırmak Için yapılandırılmış Paws (ayrıcalıklı erişim iş istasyonları) kullanın.

* [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda günlüklerin ve uyarıların üretilmesi için Azure Active Directory güvenlik raporlarını kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın.

* [Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk)

* [Azure Güvenlik Merkezi 'nde kullanıcıların kimlik ve erişim etkinliğini izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek için adlandırılmış konumlar kullanın.

* [Azure 'da adlandırılmış konumları yapılandırma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (Azure AD) kullanın. Azure AD, istemcinin Stream Analytics kaynaklara erişimi üzerinde ayrıntılı denetim için rol tabanlı erişim denetimi (RBAC) sağlar.

* [Azure AD örneği oluşturma ve yapılandırma](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: depolama hesabı yönetici rollerine sahip olanlar içerebilen eski hesapların keşfedilmesine yardımcı olmak için Azure Active Directory günlüklerini gözden geçirin. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için Kullanıcı erişiminin düzenli olarak gözden geçirilmesi gerekir.

* [Azure AD raporlamayı anlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [Azure kimlik erişimi Incelemelerini kullanma](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Rehberlik**: tüm günlükleri bir Log Analytics çalışma alanına göndererek Azure Stream Analytics ve Azure Active Directory için tanılama ayarlarını etkinleştirin. Log Analytics içindeki istenen uyarıları (devre dışı parolalara erişim girişimleri gibi) yapılandırın.

* [Azure AD günlüklerini Azure Izleyici günlükleriyle tümleştirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: Stream Analytics kaynaklarınızla ilgili olarak algılanan şüpheli eylemlere yönelik otomatik yanıtları yapılandırmak Için Azure Active Directory risk ve kimlik koruması özelliklerini kullanın. Kuruluşunuzun güvenlik yanıtlarını uygulamak için Azure Sentinel aracılığıyla otomatikleştirilmiş yanıtları etkinleştirmeniz gerekir.

* [Azure AD riskli oturum açma işlemlerini görüntüleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: uygulanamaz; Azure Stream Analytics için Müşteri Kasası desteklenmez.

* [Genel kullanıma yönelik desteklenen hizmetler ve senaryolar](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Stream Analytics kaynaklarını izlemeye yardımcı olması için Etiketler kullanın.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: girişleri, çıkışları ve depolama hesaplarını aynı aboneliğe yerleştirerek Stream Analytics işleri yalıtın. Uygulamalarınızın ve kurumsal ortamların talep ettiği Stream Analytics kaynaklarınıza erişim düzeyini denetlemek için Stream Analytics kısıtlayabilirsiniz. Azure AD RBAC aracılığıyla Azure Stream Analytics erişimini denetleyebilirsiniz.

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Azure Stream Analytics için girişleri anlayın](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-add-inputs)

* [Azure Stream Analytics çıkışlarını anlayın](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-outputs)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: veri kaybı önleme özellikleri, Azure Stream Analytics kaynakları için henüz kullanılamıyor. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini müşteri veri kaybına ve pozlamaya karşı hassas ve koruyucuları olarak değerlendirir. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

* [Azure depolama hesaplarını güvenli hale getirme](https://docs.microsoft.com/azure/storage/common/storage-security-guide)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: Azure Stream Analytics tüm gelen ve giden iletişimleri ŞIFRELER ve TLS 1,2 ' i destekler. Yerleşik kontrol noktaları da şifrelenir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: veri tanımlama özellikleri, Azure Stream Analytics kaynakları için henüz kullanılamıyor. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için rol tabanlı erişim denetimi kullanma

**Rehberlik**: kullanıcıların hizmetle nasıl etkileşime gireceğini denetlemek için rol tabanlı erişim denetımı (RBAC) kullanın.

* [Azure 'da RBAC 'yi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: tüm işlemler bellek içinde yapıldığından, Stream Analytics gelen verileri depolamaz. Stream Analytics tarafından kalıcı olması gereken sorgular ve işlevler dahil tüm özel veriler, yapılandırılan depolama hesabında depolanır. Çıktı verilerinizi depolama hesaplarınızdaki geri kalanında şifrelemek için müşteri tarafından yönetilen anahtarları (CMK) kullanın. CMK olmadan bile, verilerinizi şifrelemek ve güvenli hale getirmek için altyapı genelinde en iyi sınıf şifreleme standartlarını otomatik olarak Stream Analytics.

* [Azure Stream Analytics veri koruma](https://docs.microsoft.com/azure/stream-analytics/data-protection)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Izleyici 'Yi Azure etkinlik günlüğü ile birlikte kullanarak, Azure Stream Analytics kaynakların üretim örneklerine yapılan değişikliklerin ne zaman gerçekleştiği hakkında uyarılar oluşturun.

* [Azure etkinlik günlüğü olayları için uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: Azure Stream Analytics kaynaklarınızı güvenli hale getirmek Için Azure Güvenlik Merkezi önerilerini izleyin.

Microsoft, Azure Stream Analytics destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

* [Azure Güvenlik Merkezi önerilerini anlama](https://docs.microsoft.com/azure/security-center/recommendations-reference)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: üçüncü taraf yazılım başlıkları için otomatik düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Kılavuz**: Azure Güvenlik Merkezi tarafından sunulan varsayılan risk derecelendirmelerini (güvenli puan) kullanın.

* [Azure Güvenlik Merkezi güvenli Puanını anlama](https://docs.microsoft.com/azure/security-center/security-center-secure-score)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: abonelikleriniz dahilinde (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) tüm kaynakları sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerini numaralandırın.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

* [Azure Kaynak Graf ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [Azure aboneliklerinizi görüntüleme](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Azure RBAC 'yi anlama](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure Stream Analytics kaynaklarını düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ayrıca, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri abonelikleri içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure Ilkesi 'ni kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynakları envanterini tanımlama ve sürdürme

**Rehberlik**: uygulanamaz; Bu öneri, bir bütün olarak işlem kaynaklarına ve Azure 'a yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Ayrıca, Azure Kaynak Grafiği 'ni kullanarak abonelikler içindeki kaynakları sorgular/bulur.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Graph ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: uygulanamaz; Bu öneri, bir bütün olarak işlem kaynaklarına ve Azure 'a yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilkesiyle belirli bir kaynak türünü reddetme](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: onaylanan yazılım başlıkları envanterini koruyun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak üzere Azure koşullu erişimini yapılandırın.

* [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: Azure Stream Analytics yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. StreamAnalytics" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, Azure Stream Analytics ile ilgili yerleşik ilke tanımlarını kullanabilirsiniz; Örneğin, Azure Stream Analytics içindeki tanılama günlükleri etkinleştirilmelidir

* [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Azure Ilkesi yerleşik ilke tanımları](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies)

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilke efektlerini anlama](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilkeleri, Azure Resource Manager şablonları, Istenen durum yapılandırma betikleri, Kullanıcı tanımlı işlevler, sorgular dahil olmak üzere kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın. Azure DevOps 'da yönettiğiniz kaynaklara erişmek için, Azure DevOps ile tümleşikse veya TFS ile tümleşikse Active Directory belirli kullanıcılara, yerleşik güvenlik gruplarına veya Azure Active Directory (Azure AD) tanımlanmış gruplara izin verebilir veya vermeyebilirsiniz.

* [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure DevOps 'da izinler ve gruplar hakkında](https://docs.microsoft.com/azure/devops/organizations/security/about-permissions)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. StreamAnalytics" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. StreamAnalytics" ad alanındaki Azure ilke diğer adlarını kullanın. Azure Stream Analytics kaynaklarınızın yapılandırmasını otomatik olarak zorlamak için [Denetim], [reddetme] ve [dağıtım yok] Azure Ilkesini kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: Stream Analytics işiniz tarafından kullanılan giriş veya çıkış kaynaklarının bağlantı ayrıntıları, yapılandırılan depolama hesabında depolanır. Tüm verilerinizi güvenli hale getirmek için depolama hesabınızı şifreleyin. Ayrıca, bir Stream Analytics işinin girişi veya çıkışı için düzenli olarak kimlik bilgilerini döndürün.

* [Azure Stream Analytics veri koruma](https://docs.microsoft.com/azure/stream-analytics/data-protection)

* [Stream Analytics Işinin girdileri ve çıkışları için oturum açma kimlik bilgilerini döndürün](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-login-credentials-inputs-outputs)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: çıkış Için yönetilen kimlik kimlik doğrulaması, bir bağlantı dizesi kullanmak yerine Power BI, depolama hesabı da dahil olmak üzere, Stream Analytics işlerinin hizmete doğrudan erişmesini sağlar.

* [Yönetilen kimlikleri kullanarak Azure Data Lake Storage 1. Stream Analytics kimlik doğrulama](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-managed-identities-adls)

* [Azure Stream Analytics işinizin Azure Blob depolama çıkışına kimliğini doğrulamak için yönetilen kimlik kullanma](https://docs.microsoft.com/azure/stream-analytics/blob-output-managed-identity)

* [Azure Stream Analytics işinizin kimliğini doğrulamak için yönetilen kimliği kullanın Power BI](https://docs.microsoft.com/azure/stream-analytics/powerbi-output-managed-identity)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

* [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft 'un kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure Stream Analytics) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.

App Service, Stream Analytics, BLOB depolama vb. gibi Azure kaynaklarına yüklenen tüm içerikleri önceden tarayın. Microsoft bu örneklerdeki verilerinize erişemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: seçilen çıkış hizmeti türüne göre. çıkış hizmetinize yönelik önerilen yönergelere göre çıkış verilerinin otomatik yedeklemelerini gerçekleştirebilirsiniz. Kullanıcı tanımlı işlevler, sorgular ve veri anlık görüntüleri dahil olmak üzere iç veriler, düzenli olarak yedeklemenizin yapılandırılmış depolama hesabında depolanır.

Microsoft Azure Depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman otomatik olarak çoğaltılır. Azure depolama, verilerinizi geçici donanım arızaları, ağ veya güç kesintileri ve çok büyük doğal olağanüstü durumlar dahil olmak üzere planlı ve planlanmamış olaylardan korunabilecek şekilde kopyalar. Verilerinizi aynı veri merkezinde, aynı bölgedeki ZGen veri merkezlerinde veya coğrafi olarak ayrılmış bölgelerde çoğaltmayı tercih edebilirsiniz.

Ayrıca, verileri arşiv katmanına yedeklemek için yaşam döngüsü yönetimi özelliğini de kullanabilirsiniz. Ayrıca, depolama hesabında depolanan yedeklemeleriniz için geçici silme özelliğini etkinleştirin.

* [Azure Stream Analytics veri koruma](https://docs.microsoft.com/azure/stream-analytics/data-protection#private-data-assets-that-are-stored)

* [Azure depolama yedekliliği ve hizmet düzeyi sözleşmelerini anlama](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

* [Azure Blob depolama yaşam döngüsünü yönetme](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts)

Azure depolama Blobları için geçici silme:https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: Kullanıcı tanımlı işlevler, sorgular ve veri anlık görüntüleri dahil dahili veriler, düzenli olarak yedeklebilmeniz için yapılandırılmış depolama hesabında depolanır.

Depolama hesabı tarafından desteklenen hizmetlerden verileri yedeklemek için AzCopy veya üçüncü taraf araçlarını kullanma dahil olmak üzere birden çok yöntem mevcuttur. Azure Blob depolama için sabit depolama, kullanıcıların iş açısından kritik veri nesnelerini bir solucan içinde depolamasına olanak sağlar (bir kez yaz, çok oku) durumu. Bu durum, verileri silinebilir olmayan ve Kullanıcı tarafından belirtilen bir Aralık için değiştirilemez hale getirir.

* [Azure Stream Analytics veri koruma](https://docs.microsoft.com/azure/stream-analytics/data-protection#private-data-assets-that-are-stored)

* [AzCopy’yi kullanmaya başlama](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)

* [BLOB depolama için dengesde kullanılabilirlik ilkelerini ayarlama ve yönetme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutability-policies-manage?tabs=azure-portal)

Müşteri tarafından yönetilen/sunulan anahtarlar Azure CLı veya PowerShell kullanarak Azure Key Vault içinde yönetilebilir.

* [Azure 'da Anahtar Kasası anahtarlarını yedekleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: verilerin bütünlüğünü sınamak için düzenli olarak yedekleme verilerinizi geri yükleme işlemini gerçekleştirin.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Kılavuz**: Azure depolama alanı içinde depolanan Stream Analytics yedeklemeler, varsayılan olarak şifrelemeyi destekler ve kapatılamaz. Yedeklemelerinizi hassas veriler olarak değerlendirmeli ve ilgili erişim ve veri koruma denetimlerini Bu temelin bir parçası olarak uygulamalısınız.

* [Azure Stream Analytics veri koruma](https://docs.microsoft.com/azure/stream-analytics/data-protection#private-data-assets-that-are-stored)

* [Azure depolama 'daki verilere erişimi yetkilendirme](https://docs.microsoft.com/azure/storage/common/storage-auth)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

* [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

* [Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu da kullanabilir](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) etiketleri kullanarak Azure kaynaklarını açıkça tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun. Olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak, uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır.

* [Azure Güvenlik Merkezi'nde güvenlik uyarıları](https://docs.microsoft.com/azure/security-center/security-center-alerts-overview)

* [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: Azure kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

* [NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

* [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Kılavuz**: Azure kaynaklarına yönelik riskleri belirlemenize yardımcı olmak Için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz.

* [Sürekli dışarı aktarmayı yapılandırma](https://docs.microsoft.com/azure/security-center/continuous-export)

* [Uyarıları Azure Sentinel 'e akış](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Kılavuz**: Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanarak, güvenlik uyarılarındaki "Logic Apps" aracılığıyla yanıtları otomatik olarak tetikleyin ve Azure kaynaklarınızı korumaya yönelik öneriler alın.

* [Iş akışı otomasyonu ve Logic Apps yapılandırma](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: 

* [Sızma testlerinizin Microsoft ilkelerini ihlal etmemesini sağlamak için Microsoft katılım kurallarını izleyin](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
