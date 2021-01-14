---
title: Azure depolama için Azure Güvenlik temeli
description: Azure depolama için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/23/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: f6825fee4cfb78ab54d782cd4d942bb994bedae9
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98195730"
---
# <a name="azure-security-baseline-for-azure-storage"></a>Azure depolama için Azure Güvenlik temeli

Azure depolama için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](../../security/benchmarks/overview.md)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](../../security/benchmarks/security-baselines-overview.md).

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](../../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: belırlı genel IP adresi aralıklarından istemcilere erişimi kısıtlayarak depolama hesabınızın güvenlik duvarını yapılandırın, Azure 'da sanal ağları (VNet) veya belirli Azure kaynaklarını seçin. Ayrıca özel uç noktaları, kuruluşunuzda bulunan depolama hizmetine giden trafik için özel ağlar üzerinden özel olarak da yapılandırabilirsiniz.

Not: klasik depolama hesapları, güvenlik duvarlarını ve sanal ağları desteklemez.

- [Azure Storage güvenlik duvarını yapılandırma](./storage-network-security.md#change-the-default-network-access-rule)

- [Azure depolama için özel uç noktaları yapılandırma](./storage-private-endpoints.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-vnet-subnet-and-nic-configuration-and-traffic"></a>1,2: VNet, alt ağ ve NIC yapılandırmasını ve trafiğini izleme ve günlüğe kaydetme

**Kılavuz**: Azure depolama, katmanlı bir güvenlik modeli sağlar. Depolama hesabınıza erişimi, belirtilen IP adreslerinden, IP aralıklarından veya bir Azure sanal ağındaki (VNet) bir alt ağ listesinden kaynaklanan isteklerle sınırlayabilirsiniz. Azure Güvenlik Merkezi 'ni kullanarak ağ kaynaklarınızın güvenliğini sağlamaya yardımcı olmak için ağ koruma önerilerini izleyebilirsiniz. Ayrıca, depolama hesabı güvenlik duvarı aracılığıyla depolama hesapları için yapılandırılmış sanal ağlar/alt ağ için NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir depolama hesabına gönderin. 

Depolama hesabınıza bağlı özel uç noktalarınız varsa, alt ağlar için ağ güvenlik grubu (NSG) kurallarını yapılandıramayacağınızı unutmayın. 

- [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](./storage-network-security.md)

- [NSG akış günlüklerini etkinleştirme](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](../../security-center/security-center-network-recommendations.md)

- [Azure depolama için özel uç noktaları anlama](./storage-private-endpoints.md#known-issues)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle Iletişimleri reddetme

**Kılavuz**: Azure depolama hesabınız Için Gelişmiş tehdit koruması 'nı etkinleştirin. Azure depolama için Gelişmiş tehdit koruması, depolama hesaplarına yönelik olağan dışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Azure Güvenlik Merkezi tümleşik uyarıları, ağ iletişiminin başarıyla çözümlenen bir IP adresiyle ilişkilendirildiği etkinliklere, IP adresinin bilinen riskli IP adresi (örneğin, bilinen bir cryptominer) veya daha önce riskli olarak tanınmamış bir IP adresi olup olmadığına bağlıdır. Etkinlikte anomali oluştuğunda güvenlik uyarıları tetiklenir. 

- [Gelişmiş tehdit korumasını etkinleştirme](./azure-defender-storage-configure.md?tabs=azure-portal)

- [Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama](../../security-center/azure-defender.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Rehberlik**: Ağ İzleyicisi paket yakalama, depolama hesabı ile sanal makine arasındaki trafiği izlemek için yakalama oturumları oluşturmanızı sağlar. Yalnızca istediğiniz trafiği yakalamanızı sağlamak için yakalama oturumu için filtreler sağlanır. Paket yakalama, ağ anormalilerin hem yeniden etkin hem de proaktif olarak tanımlanmasına yardımcı olur. Diğer kullanımlar arasında ağ istatistiklerini toplama, ağ erişimi ile ilgili bilgi alma, istemci-sunucu iletişiminde hata ayıklama ve çok daha fazlası yer alır. Paket yakalamalarını uzaktan tetikleyebilmek için, istenen bir sanal makinede bir paket yakalamayı çalıştırmanın yükünü kolaylaştırır ve bu da değerli süreyi kaydeder. 

- [Portalı kullanarak paket yakalamalarını Azure ağ Izleyicisi ile yönetme](../../network-watcher/network-watcher-packet-capture-manage-portal.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemlerini dağıtma

**Rehberlik**: Azure depolama için Gelişmiş tehdit koruması, depolama hesaplarına yönelik olağan dışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Etkinlikte anomali oluştuğunda güvenlik uyarıları tetiklenir. Azure Güvenlik Merkezi ile tümleşik olan bu güvenlik uyarıları şüpheli etkinliğin ayrıntılarının yanı sıra tehditlerin incelenmesine ve ortadan kaldırılmasına yönelik önerilerle birlikte abonelik yöneticilerine e-posta yoluyla da gönderilir. 

- [Azure depolama için Gelişmiş tehdit koruması yapılandırma](./azure-defender-storage-configure.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-your-web-applications"></a>1,7: Web uygulamalarınıza giden trafiği yönetme

**Rehberlik**: uygulanamaz; öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Kılavuz**: depolama hesabınıza erişmesi gereken sanal ağlardaki kaynak Için, ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak üzere yapılandırılmış sanal ağ Için sanal ağ hizmeti etiketleri kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örneğin, depolama) belirterek, karşılık gelen hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir. 

Ağ erişiminin belirli depolama hesaplarına kapsama alınması gerektiğinde, sanal ağ hizmeti uç noktası ilkeleri kullanın.

- [Hizmet etiketlerini kullanma hakkında daha fazla bilgi için](../../virtual-network/service-tags-overview.md)

- [Azure depolama için sanal ağ hizmet uç noktası ilkeleri hakkında daha fazla bilgi için](../../virtual-network/virtual-network-service-endpoint-policies-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: hizmet

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure Ilkesi Ile Azure depolama hesabınızla ilişkili ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Depolama hesabı kaynaklarınızın ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Storage" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. 

Depolama hesabı ile ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz, örneğin: depolama hesapları bir sanal ağ hizmeti uç noktası kullanmalıdır 

- [Azure İlkesi'ni yapılandırma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

- [Depolama için Azure Ilke örnekleri](../../governance/policy/samples/built-in-policies.md#storage)

- [Ağ için Azure Ilke örnekleri](../../governance/policy/samples/built-in-policies.md#network)

- [Azure Blueprint oluşturma](../../governance/blueprints/create-blueprint-portal.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: ağ güvenlik grupları (NSG) ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için "Açıklama" alanını kullanarak ağa giden/giden trafiğe izin veren kuralların iş gereksinimini ve/veya süresini (vs.) belirtin. Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure Ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın. Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz. 

- [Etiketler oluşturma ve kullanma](../../azure-resource-manager/management/tag-resources.md)

- [Sanal ağ oluşturma](../../virtual-network/quick-create-portal.md)

- [Güvenlik Yapılandırması ile NSG oluşturma](../../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını Izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Rehberlik**: ağ kaynakları için yapılandırma değişikliklerini günlüğe kaydetmek üzere Azure ilkesini kullanın. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun. 

- [Azure İlkesi'ni yapılandırma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

- [Azure Izleyici 'de uyarı oluşturma](../../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](../../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-resource"></a>2,1: onaylanan zaman eşitleme kaynağını kullan

**Rehberlik**: uygulanamaz; Microsoft, Azure depolama hesapları için zaman kaynaklarını korur.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: uç noktalar cihazları, ağ kaynakları ve diğer güvenlik sistemleri tarafından oluşturulan güvenlik verilerini toplamak Için Azure izleyici aracılığıyla günlükleri alma. Azure Izleyici 'de analiz sorgulamak ve gerçekleştirmek için Log Analytics çalışma alanlarını kullanın ve isteğe bağlı olarak, sabit depolama ve zorlanan bekletme gibi güvenlik özellikleriyle uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../../azure-monitor/platform/diagnostic-settings.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: Azure depolama Analizi Bloblar, kuyruklar ve tablolar için Günlükler sağlar. Hesabınız için hangi günlüklerin kaydedildiğini yapılandırmak üzere Azure portal kullanabilirsiniz. 

- [Azure depolama hesabınız için izlemeyi yapılandırma](./storage-monitor-storage-account.md#configure-monitoring-for-a-storage-account)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: Işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure depolama hesabında veya Log Analytics çalışma alanında güvenlik olay günlüklerini depolarken, saklama ilkesini kuruluşunuzun gereksinimlerine göre ayarlayabilirsiniz. 

- [Azure depolama hesabı günlükleri için bekletme ilkesini yapılandırma](./storage-monitor-storage-account.md#configure-logging)

- [Log Analytics veri saklama süresini değiştirme](../../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Kılavuz**: Azure depolama günlüklerini gözden geçirmek için, Log Analytics teklifiyle sorgu gibi olağan seçenekler ve günlük dosyalarını doğrudan görüntülemenin benzersiz bir seçeneği vardır. Azure depolama 'da Günlükler doğrudan erişilmesi gereken bloblarda depolanır `http://accountname.blob.core.windows.net/$logs` (günlük klasörü varsayılan olarak gizlidir, bu nedenle doğrudan gitmeniz gerekir. Liste komutlarında görüntülenmez) 

Ayrıca, Azure depolama hesabınız için Gelişmiş tehdit koruması 'nı etkinleştirin. Azure depolama için Gelişmiş tehdit koruması, depolama hesaplarına yönelik olağan dışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Etkinlikte anomali oluştuğunda güvenlik uyarıları tetiklenir. Azure Güvenlik Merkezi ile tümleşik olan bu güvenlik uyarıları şüpheli etkinliğin ayrıntılarının yanı sıra tehditlerin incelenmesine ve ortadan kaldırılmasına yönelik önerilerle birlikte abonelik yöneticilerine e-posta yoluyla da gönderilir. 

- [Verileri günlüğe kaydetme ve gözden geçirme](./storage-analytics-logging.md#how-logs-are-stored)

- [Gelişmiş tehdit korumasını etkinleştirme](./azure-defender-storage-configure.md?tabs=azure-portal)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Kılavuz**: Azure Güvenlik Merkezi 'nde depolama hesabı Için Gelişmiş tehdit koruması 'nı etkinleştirin. Depolama hesabı için tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Log Analytics çalışma alanınızı Azure Sentinel 'e ekleyin. Bu, bir güvenlik Orchestration otomatik yanıtı (SOAR) çözümü sağlar. Bu, güvenlik sorunlarını gidermek için PlayBook 'ları (otomatikleştirilmiş çözümlerin) oluşturulmasına ve kullanılmasına olanak tanır. 

- [Azure Sentinel 'i ekleme](../../sentinel/quickstart-onboard.md)

- [Azure Güvenlik Merkezi 'nde uyarıları yönetme](../../security-center/security-center-managing-and-responding-alerts.md)

- [Log Analytics günlük verilerinde uyarı alma](../../azure-monitor/learn/tutorial-response.md)

- [Azure Depolama analizini günlüğe kaydetme](./storage-analytics-logging.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Kılavuz**: Azure Güvenlik Merkezi 'ni kullanarak Azure depolama 'ya yönelik tehdit korumasını etkinleştirerek, karma saygınlığı analizini ve etkin bir Tor çıkış düğümünden şüpheli erişimi (bir anonim hale getirme) kullanın. 

- [Azure depolama için Gelişmiş tehdit koruması yapılandırma](./azure-defender-storage-configure.md?tabs=azure-portal)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Kılavuz**: Azure izleyici 'de Azure DNS Analytics (Önizleme) çözümü, güvenlik, performans ve işlemlerde DNS altyapısına ilişkin Öngörüler toplar. Şu anda bu Azure depolama hesaplarını desteklemez, ancak üçüncü taraf DNS günlüğü çözümünü kullanabilirsiniz. 

- [DNS Analizi Preview çözümüyle DNS altyapınız hakkında Öngörüler toplayın](../../azure-monitor/insights/dns-analytics.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Kıyaslama işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tut

**Kılavuz**: Azure AD 'nin açıkça atanması ve sorgulanabilir olması gereken yerleşik rolleri vardır. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın. 

- [Azure AD 'de PowerShell ile dizin rolü alma](/powershell/module/azuread/get-azureaddirectoryrole)

- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](/powershell/module/azuread/get-azureaddirectoryrolemember)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Azure depolama hesapları veya Azure Active Directory, varsayılan veya boş parola kavramıdır. Azure depolama, Azure rol tabanlı erişim denetimi (Azure RBAC) ve paylaşılan anahtar ve paylaşılan erişim Imzaları (SAS) destekleyen bir erişim denetimi modeli uygular. Paylaşılan anahtar ve SAS kimlik doğrulamasının bir özelliği, çağıran ile hiçbir kimliğin ilişkilendirilmediği ve bu nedenle güvenlik sorumlusu izin tabanlı yetkilendirmenin gerçekleştirilemediği bir özelliğidir. 

- [Azure depolama 'daki verilere erişimi yetkilendirme](./storage-auth.md)

- [Azure depolama hesabı için güvenlik sorumlularını ve erişim denetimini anlama](./storage-introduction.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: depolama hesabınıza erişimi olan adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın. 

Ayrıca, Microsoft Hizmetleri ve Azure ARM için Azure AD Privileged Identity Management ayrıcalıklı rolleri kullanarak, tam zamanında/tam erişimi de etkinleştirebilirsiniz. 

- [Azure Güvenlik Merkezi kimlik ve erişimini anlama](../../security-center/security-center-identity-access.md)

- [Privileged Identity Management genel bakış](../../active-directory/privileged-identity-management/index.yml)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3,4: Azure Active Directory tek Sign-On kullanın (SSO)

**Rehberlik**: mümkün olan yerlerde, tek başına bağımsız kimlik bilgilerini hizmet başına yapılandırmak yerıne Azure Active Directory SSO kullanın. Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini kullanın. 

- [Azure AD ile SSO 'yu anlama](../../active-directory/manage-apps/what-is-single-sign-on.md)

- [Azure depolama 'daki verilere erişimi yetkilendirme](./storage-auth.md)

- [Azure Active Directory kullanarak bloblara ve kuyruklara erişim yetkisi verme](./storage-auth-aad.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="35-use-multifactor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulaması kullanın.

**Rehberlik**: depolama hesabı kaynaklarınızı korumaya yardımcı olmak Için Azure Active Directory çok faktörlü kimlik doğrulamasını etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin. 

- [Azure'da çok faktörlü kimlik doğrulamasını etkinleştirme](../../active-directory/authentication/howto-mfa-getstarted.md)

- [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](../../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Rehberlik**: depolama hesabı kaynaklarını açmak ve YAPıLANDıRMAK için MFA Ile Paws (ayrıcalıklı erişim iş istasyonları) kullanın. 

- [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Azure'da çok faktörlü kimlik doğrulamasını etkinleştirme](../../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Kılavuz**: Azure Güvenlik Merkezi risk algılama uyarılarını Azure izleyici 'ye gönderin ve eylem gruplarını kullanarak özel uyarı/bildirim yapılandırın. Şüpheli etkinlik uyarıları oluşturmak için Azure depolama hesabı için Gelişmiş tehdit koruması 'nı etkinleştirin. Ayrıca, riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın. 

- [Azure depolama hesabı için Gelişmiş tehdit koruması ayarlama](./azure-defender-storage-configure.md)

- [Azure AD risk algılamalarını anlama](../../active-directory/identity-protection/overview-identity-protection.md)

- [Özel uyarı ve bildirim için eylem gruplarını yapılandırma](../../azure-monitor/platform/action-groups.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resource-from-only-approved-locations"></a>3,8: Azure kaynağını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek için adlandırılmış konumlar kullanın. 

- [Azure 'da adlandırılmış konumları yapılandırma](../../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (Azure AD) kullanın. Azure, bir istemcinin depolama hesabındaki kaynaklara erişimi üzerinde ayrıntılı denetim için Azure rol tabanlı erişim denetimi (Azure RBAC) sağlar.  Daha kolay tehlikeye giren hesap anahtarını kullanmak yerine, en iyi güvenlik uygulaması olarak mümkün olduğunda Azure AD kimlik bilgilerini kullanın. Uygulama tasarımınız, blob depolamaya erişim için paylaşılan erişim imzaları gerektirdiğinde, üstün güvenlik için mümkün olduğunda Kullanıcı temsili paylaşılan erişim imzaları (SAS) oluşturmak için Azure AD kimlik bilgilerini kullanın.

- [Azure AD örneği oluşturma ve yapılandırma](../../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Yönetim kaynaklarına erişmek için Azure depolama kaynak sağlayıcısını kullanma](./authorization-resource-provider.md)

- [Azure portal Azure RBAC ile Azure Blob ve kuyruk verilerine erişimi yapılandırma](./storage-auth-aad-rbac-portal.md)

- [Azure depolama 'daki verilere erişimi yetkilendirme](./storage-auth.md)

- [Paylaşılan erişim imzalarını (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](./storage-sas-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: depolama hesabı yönetici rollerine sahip olanlar içerebilen eski hesapların keşfedilmesine yardımcı olmak için Azure Active Directory günlüklerini gözden geçirin. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek için Azure kimlik erişimi Incelemelerini kullanın, depolama hesabı kaynaklarına erişmek için kullanılabilecek kurumsal uygulamalara erişim ve rol atamaları. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için Kullanıcı erişiminin düzenli olarak gözden geçirilmesi gerekir.  

Verilerinizin güvenliğine güvenmeksizin Depolama hesabınızdaki kaynaklara güvenli temsilci erişimi sağlamak için paylaşılan erişim imzasını (SAS) de kullanabilirsiniz. İstemcinin erişebileceği kaynakları, bu kaynaklarda sahip oldukları izinleri ve SAS 'ın ne kadar süreyle geçerli olduğunu (diğer parametreler arasında) denetleyebilirsiniz.

Ayrıca, kapsayıcılara ve bloblara anonim okuma erişimini gözden geçirin. Varsayılan olarak kapsayıcıya ve içindeki bloblara yalnızca uygun izinlerin verilmiş olduğu kullanıcılar erişebilir. Azure Izleyici 'yi, anonim kimlik doğrulama koşulunu kullanarak depolama hesaplarına yönelik anonim erişimi uyarmak için kullanabilirsiniz.

Kullanıcı hesabı erişiminin şüpheli olma riskini azaltmak için etkili bir yol, kullanıcılara verdiğiniz erişim süresini sınırlayamaktır. Zaman sınırlı SAS URI 'Leri, bir depolama hesabına Kullanıcı erişiminin süresinin otomatik olarak dolmasına yönelik tek bir yoldur. Ayrıca, depolama hesabı anahtarlarını sık aralıklarla döndürmek, depolama hesabı anahtarları aracılığıyla beklenmeyen erişimin sınırlı bir süre olduğundan emin olmanın bir yoludur.

- [Azure AD raporlamayı anlama](../../active-directory/reports-monitoring/index.yml)

- [Azure depolama hesabı düzeyinde erişimi görüntüleme ve değiştirme](./storage-auth-aad-rbac-portal.md)

- [Paylaşılan erişim imzalarını (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](./storage-sas-overview.md)

- [Kapsayıcılara ve blob’lara anonim okuma erişimini yönetme](../blobs/anonymous-read-access-configure.md)

- [Azure portalında depolama hesabını izleme](./storage-monitor-storage-account.md)

- [Depolama hesabı erişim anahtarlarını yönetme](./storage-account-keys-manage.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Rehberlik**: bir depolama hizmetine başarılı ve başarısız istekler hakkında ayrıntılı bilgileri günlüğe almak için depolama Analizi kullanın. Tüm Günlükler, bir depolama hesabı için Depolama Analizi etkinleştirildiğinde otomatik olarak oluşturulan $logs adlı kapsayıcıda Blok Bloblarında depolanır.

Azure Active Directory Kullanıcı hesapları için Tanılama ayarları oluşturun, Denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına gönderir. İstenen uyarıları Log Analytics çalışma alanı içinde yapılandırabilirsiniz. Azure depolama hesaplarında kimlik doğrulama başarısızlıklarını izlemek için, depolama kaynak ölçümleri için belirli eşiklere ulaşıldığında size bildirimde bulunan uyarılar oluşturabilirsiniz. Ayrıca, anonim kimlik doğrulama koşulunu kullanarak depolama hesaplarına yönelik anonim erişim hakkında uyarı almak için Azure Izleyici 'yi kullanın.

- [Azure Depolama analizini günlüğe kaydetme](./storage-analytics-logging.md)

- [Azure Etkinlik Günlüklerini Azure İzleyici ile tümleştirme](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Azure depolama hesapları için ölçüm uyarılarını yapılandırma](./storage-monitor-storage-account.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: depolama hesabı kaynaklarınızla ilgili olarak algılanan şüpheli eylemlere yönelik otomatik yanıtları yapılandırmak Için Azure Active Directory risk ve kimlik koruması özelliklerini kullanın. Kuruluşunuzun güvenlik yanıtlarını uygulamak için Azure Sentinel aracılığıyla otomatikleştirilmiş yanıtları etkinleştirmeniz gerekir. 

- [Azure AD riskli oturum açma işlemlerini görüntüleme](../../active-directory/identity-protection/overview-identity-protection.md)

- [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](../../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Azure Sentinel 'i ekleme](../../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Kılavuz**: Microsoft 'un müşteri verilerine erişmesi gereken destek senaryolarında, müşteri kasası (depolama hesabı için Önizleme) müşterilerin müşteri verileri erişim isteklerini gözden geçirmesi ve onaylaması veya reddetmesi için bir arabirim sağlar. Microsoft, kuruluşunuzun depolama hesabı 'nda depolanan gizli dizileri için gerekli değildir ve bu parolalara erişim istemez.

- [Müşteri Kasası anlayın](../../security/fundamentals/customer-lockbox-overview.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](../../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen depolama hesabı kaynaklarını izlemeye yardımcı olması için etiketleri kullanın. 

- [Etiketler oluşturma ve kullanma](../../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: ortam, veri duyarlılığı gibi bireysel güvenlik etki alanları için ayrı abonelikler, yönetim grupları ve depolama hesapları kullanarak yalıtım uygulayın.  Kullanılan ağların türüne ve alt kümelerine göre, uygulamalarınızın ve kurumsal ortamlarınızın talep ettiği depolama hesaplarınıza erişim düzeyini denetlemek için depolama hesabınızı kısıtlayabilirsiniz. Ağ kuralları yapılandırıldığında, yalnızca belirtilen ağ kümesi üzerinde veri isteyen uygulamalar bir depolama hesabına erişebilir. Azure depolama 'ya erişimi Azure RBAC aracılığıyla denetleyebilirsiniz. Ayrıca, sanal ağınız ile hizmet arasındaki trafik Microsoft omurga ağı üzerinden geçiş yaparken, genel Internet 'ten etkilenme olasılığını ortadan kaldırarak güvenliği artırmak için özel uç noktaları yapılandırabilirsiniz. 

- [Ek Azure abonelikleri oluşturma](../../cost-management-billing/manage/create-subscription.md)

- [Yönetim Grupları oluşturma](../../governance/management-groups/create-management-group-portal.md)

- [Etiketler oluşturma ve kullanma](../../azure-resource-manager/management/tag-resources.md)

- [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](./storage-network-security.md)

- [Sanal ağ hizmeti uç noktaları](../../virtual-network/virtual-network-service-endpoints-overview.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleyin ve engelleyin.

**Rehberlik**: hassas bilgileri depolayan veya işleyen depolama hesabı kaynakları Için, etiketleri kullanarak kaynakları hassas olarak işaretleyin. Veri kaybı riskini azaltmak için Azure Güvenlik Duvarı 'nı kullanarak Azure depolama hesapları için giden ağ trafiğini kısıtlayın. 

Ayrıca, çıkış sanal ağ trafiğini hizmet uç noktası üzerinden Azure depolama hesaplarına filtrelemek ve verilerin yalnızca belirli Azure depolama hesaplarına sızmasına izin vermek için sanal ağ hizmet uç noktası ilkelerini kullanın.

- [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](../../virtual-network/virtual-network-service-endpoint-policies-overview.md)

- [Azure Depolama için sanal ağ hizmet uç noktası ilkeleri](../../private-link/tutorial-private-endpoint-storage-portal.md)

- [Azure’da müşteri verilerinin korunmasını anlama](../../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: depolama hesabı için gerekli olan güvenli AKTARıMı etkinleştirerek https kullanımını zorunlu kılabilirsiniz. Bu özellik etkinleştirildikten sonra HTTP kullanan bağlantılar reddedilir. Ayrıca, depolama hesabınız için güvenli aktarımı zorlamak üzere Azure Güvenlik Merkezi 'ni ve Azure Ilkesini kullanın.

- [Azure depolama 'da güvenli aktarım gerektirme](./storage-require-secure-transfer.md)

- [Güvenlik Merkezi tarafından izlenen Azure Güvenlik ilkeleri](../../security-center/policy-reference.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: veri tanımlama özellikleri henüz Azure depolama hesabı ve ilgili kaynaklar için kullanılabilir değil. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın. 

- [Azure’da müşteri verilerinin korunmasını anlama](../../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Rehberlik**: Azure Active Directory (Azure AD), Azure rol tabanlı erişim denetimi (Azure RBAC) aracılığıyla güvenli kaynaklara erişim haklarını yetkilendirir. Azure depolama, blob veya kuyruk verilerine erişmek için kullanılan ortak izin kümelerini çevreleyen bir dizi Azure yerleşik rol tanımlar. 

- [Azure depolama hesabı için Azure rolleri atama](./storage-auth-aad-rbac-portal.md#assign-azure-roles-using-the-azure-portal)

- [Yönetim kaynaklarına erişmek için Azure depolama kaynak sağlayıcısını kullanma](./authorization-resource-provider.md)

- [Azure portal Azure RBAC ile Azure Blob ve kuyruk verilerine erişimi yapılandırma](./storage-auth-aad-rbac-portal.md)

- [AAD örneği oluşturma ve yapılandırma](../../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Azure depolama 'daki verilere erişimi yetkilendirme](./storage-auth.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Kılavuz**: Azure depolama şifrelemesi tüm depolama hesapları için etkin ve devre dışı bırakılamaz. Azure depolama, bulutta kalıcı olduğunda verilerinizi otomatik olarak şifreler. Azure Depolama’dan veri okuduğunuzda veriler döndürülmeden önce Azure Depolama tarafından şifresi çözülür. Azure depolama şifrelemesi, kodun değiştirilmesi veya herhangi bir uygulamaya kod eklemesi gerekmeden verilerinizi Rest 'de korumanıza olanak sağlar. 

- [Rest 'de Azure depolama şifrelemesini anlama](./storage-service-encryption.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Izleyici 'Yi Azure etkinlik günlüğü ile kullanarak, değişikliklerin depolama hesabı kaynaklarına ne zaman gerçekleştiği hakkında uyarılar oluşturun. Ayrıca, Azure depolama 'ya karşı yapılan her isteği nasıl yetkilendirdiğini izlemek için Azure depolama günlüğünü de etkinleştirebilirsiniz. Günlükler, bir isteğin bir OAuth 2,0 belirteci kullanılarak, paylaşılan anahtar kullanılarak veya paylaşılan erişim imzası (SAS) kullanılarak yapılıp yapılmadığını belirtir. Ayrıca, anonim kimlik doğrulama koşulunu kullanarak depolama hesaplarına yönelik anonim erişim hakkında uyarı almak için Azure Izleyici 'yi kullanın.

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](../../azure-monitor/platform/alerts-activity-log.md)

- [Azure Depolama analizini günlüğe kaydetme](./storage-analytics-logging.md)

- [Azure depolama hesapları için ölçüm uyarılarını yapılandırma](./storage-monitor-storage-account.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](../../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: depolama hesaplarınızın yapılandırmasını sürekli olarak denetlemek ve Izlemek Için Azure Güvenlik Merkezi 'nin önerilerini izleyin. 

- [Güvenlik önerileri - başvuru kılavuzu](../../security-center/recommendations-reference.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik Işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Microsoft, depolama hesaplarını destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için bir risk derecelendirme işlemi kullanın.

**Kılavuz**: Azure Güvenlik Merkezi tarafından sunulan varsayılan risk derecelendirmelerini (güvenli puan) kullanın. 

- [Azure Güvenlik Merkezi güvenli Puanını anlama](../../security-center/secure-score-security-controls.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz içindeki tüm kaynakları (depolama hesapları dahil) sorgulamak ve öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun. 

- [Azure Graph ile sorgu oluşturma](../../governance/resource-graph/first-query-portal.md)

- [Azure aboneliklerinizi görüntüleme](/powershell/module/az.accounts/get-azsubscription)

- [Azure RBAC 'yi anlama](../../role-based-access-control/overview.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: mantıksal olarak bunları bir taksonomi halinde düzenlemek için meta veriler sağlayan depolama hesabı kaynaklarına Etiketler uygulayın. 

- [Etiketler oluşturma ve kullanma](../../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: depolama hesaplarını ve ilgili kaynakları düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun. 

Ayrıca, yetkisiz Azure kaynaklarını algılamak için Azure depolama için Gelişmiş tehdit koruması 'nı kullanın. 

- [Ek Azure abonelikleri oluşturma](../../cost-management-billing/manage/create-subscription.md)

- [Yönetim Grupları oluşturma](../../governance/management-groups/create-management-group-portal.md)

- [Etiketler oluşturma ve kullanma](../../azure-resource-manager/management/tag-resources.md)

- [Azure depolama için Gelişmiş tehdit koruması yapılandırma](./azure-defender-storage-configure.md?tabs=azure-portal)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="64-maintain-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini saklayın.

**Rehberlik**: Kurumsal gereksinimlerinize göre onaylanan Azure kaynakları envanterini oluşturmanız gerekecektir. 


**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın: 

 - İzin verilmeyen kaynak türleri 
 - İzin verilen kaynak türleri 

Ayrıca, Azure Kaynak grafiğini kullanarak abonelikler içindeki kaynakları sorgular/bulur. Bu, depolama hesapları gibi yüksek güvenlik tabanlı ortamlarda yardımcı olabilir. 

- [Azure İlkesi'ni yapılandırma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

- [Azure Graph ile sorgu oluşturma](../../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: Işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: müşteri, Azure ilkesiyle müşterinin Şirket ilkelerinin gerektirdiği şekilde kaynak oluşturulmasını veya kullanımını engelleyebilir. 

- [Azure İlkesi'ni yapılandırma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın: 

- İzin verilmeyen kaynak türleri 
- İzin verilen kaynak türleri 

- [Azure İlkesi'ni yapılandırma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](../../governance/policy/samples/index.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager-via-scripts"></a>6,11: kullanıcıların betikler aracılığıyla Azure Resource Manager etkileşim kurma yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimini kullanın. Bu, depolama hesaplarına sahip olanlar gibi yüksek bir güvenlik ortamındaki kaynaklarda oluşturma ve değişiklik yapılmasını önleyebilir. 

- [ARM 'e erişimi engellemek için koşullu erişimi yapılandırma](../../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların Işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](../../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: depolama hesabı örneklerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. Storage" ad alanındaki Azure ilke diğer adlarını kullanın. Azure depolama hesabı için yerleşik Azure Ilke tanımlarını da kullanabilirsiniz, örneğin: 

Depolama hesaplarına Kısıtlanmamış ağ erişimini denetleme  
Depolama hesaplarında Gelişmiş tehdit koruması dağıtma  
Depolama hesaplarının yeni Azure Resource Manager kaynaklarına geçirilmesi gerekir  
Depolama hesaplarına güvenli aktarım etkinleştirilmelidir  

Azure Güvenlik Merkezi 'nden, depolama hesaplarınız için güvenli bir yapılandırma temeli olarak öneriler kullanın. 

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](/powershell/module/az.resources/get-azpolicyalias)

- [Azure İlkesi'ni yapılandırma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-configurations-for-your-operating-system"></a>7,2: Işletim sisteminiz için güvenli yapılandırma oluşturma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-configurations-for-all-azure-resources"></a>7,3: tüm Azure kaynakları için güvenli yapılandırmaların bakımını yapma

**Rehberlik**: depolama hesabı kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yok] kullanın. 

- [Azure İlkesi'ni yapılandırma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilke efektlerini anlama](../../governance/policy/concepts/effects.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-configurations-for-operating-systems"></a>7,4: Işletim sistemleri için güvenli yapılandırmaların bakımını yapma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilkeleri, Azure Resource Manager şablonları, Istenen durum yapılandırma betikleri vb. gibi kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın. Azure DevOps 'da yönettiğiniz kaynaklara erişmek için, Azure DevOps ile tümleşikse veya TFS ile tümleşikse Active Directory belirli kullanıcılara, yerleşik güvenlik gruplarına veya Azure Active Directory (Azure AD) tanımlanmış gruplara izin verebilir veya vermeyebilirsiniz.

- [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow)

- [Azure DevOps 'da izinler ve gruplar hakkında](/azure/devops/organizations/security/about-permissions)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel Işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Rehberlik**: depolama hesabı için sistem yapılandırmasını uyarmak, denetlemek ve zorlamak Için Azure ilkesinden yararlanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin. 

- [Azure İlkesi'ni yapılandırma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: Işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma Izlemeyi uygulayın

**Kılavuz**: Azure depolama hesabı kaynaklarınız için temel taramalar gerçekleştirmek üzere Azure Güvenlik Merkezi 'nden yararlanın. 

- [Azure Güvenlik Merkezi 'nde öneriler nasıl düzeltileceği](../../security-center/security-center-remediate-recommendations.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: Işletim sistemleri için otomatik yapılandırma Izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-securely-manage-azure-secrets"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: Azure Storage, verileri buluta kalıcı hale geldiğinde otomatik olarak şifreler. Depolama hesabının şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya şifrelemeyi kendi anahtarlarıyla yönetebilirsiniz. Müşteri tarafından sağlanmış anahtarlar kullanıyorsanız, anahtarları güvenli bir şekilde depolamak için Azure Key Vault özelliğinden yararlanabilirsiniz. 

Ayrıca, depolama hesabı anahtarlarının kaybının veya açıklanmasının etkilerini sınırlamak için depolama hesabı anahtarlarını sık aralıklarla döndürün.

- [Bekleyen veri için Azure Depolama şifrelemesi](./storage-service-encryption.md)

- [Depolama hesabı erişim anahtarlarını yönetme](./storage-account-keys-manage.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="712-securely-and-automatically-manage-identities"></a>7,12: kimlikleri güvenli ve otomatik olarak Yönet

**Rehberlik**: Azure Active Directory ve yönetilen kimlikler Ile Azure depolama hesapları içindeki bloblara ve kuyruklara erişim yetkisi verin. Azure Blob ve kuyruk depolama, Azure kaynakları için yönetilen kimliklerle Azure Active Directory (Azure AD) kimlik doğrulamasını destekler. Azure kaynakları için Yönetilen kimlikler, Azure sanal makinelerinde (VM), işlev uygulamalarında, sanal makine ölçek kümelerinde ve diğer hizmetlerde çalışan uygulamalardan Azure AD kimlik bilgilerini kullanarak blob ve kuyruk verilerine erişim yetkisi verebilir. Azure AD kimlik doğrulamasıyla birlikte Azure kaynakları için Yönetilen kimlikler kullanarak, kimlik bilgilerini bulutta çalışan uygulamalarınızla depolamaktan kaçınabilirsiniz. 

- [Yönetilen kimlik kullanarak Azure Blob ve kuyruk verilerine erişim verme](./storage-auth-aad-rbac-portal.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik Bilgisi Tarayıcısı ayrıca bulunan kimlik bilgilerinin Azure Key Vault gibi daha güvenlik konumlara aktarılmasını sağlar. 

- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](../../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft, temel alınan platform için kötü amaçlı yazılımdan koruma uygular.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: karma saygınlığı analizini ve etkin bir Tor çıkış düğümünden şüpheli erişimi (bir anonim yönetici) kullanarak Azure depolama 'ya kötü amaçlı yazılım yüklemelerini algılamak için tehdit koruması 'nı kullanın. 

Ayrıca, App Service, Data Lake Storage, BLOB depolama gibi işlem dışı Azure kaynaklarına yüklemeden önce kötü amaçlı yazılımların herhangi bir içeriğini önceden tarayabilirsiniz ve kuruluşunuzun gereksinimlerini karşılayabilirsiniz.

- [Azure depolama için Gelişmiş tehdit koruması yapılandırma](./azure-defender-storage-configure.md?tabs=azure-portal)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve Imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft, temel alınan platform için kötü amaçlı yazılımdan koruma uygular.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](../../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: Microsoft Azure Depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman otomatik olarak çoğaltılır. Azure depolama, verilerinizi geçici donanım arızaları, ağ veya güç kesintileri ve çok büyük doğal olağanüstü durumlar dahil olmak üzere planlı ve planlanmamış olaylardan korunabilecek şekilde kopyalar. Verilerinizi aynı veri merkezinde, aynı bölgedeki ZGen veri merkezlerinde veya coğrafi olarak ayrılmış bölgelerde çoğaltmayı tercih edebilirsiniz. 

Ayrıca, Azure Otomasyonu ' nu Blobların düzenli anlık görüntülerini alacak şekilde etkinleştirebilirsiniz.

- [Azure depolama yedekliliği ve Service-Level sözleşmelerini anlama](./storage-redundancy.md)

- [Blob 'un anlık görüntüsünü oluşturma](/rest/api/storageservices/creating-a-snapshot-of-a-blob)

- [Azure Otomasyonu’na genel bakış](../../automation/automation-intro.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: depolama hesabı tarafından desteklenen hizmetlerden veri yedeklemek için AzCopy veya üçüncü taraf araçlarını kullanma gibi birden çok yöntem mevcuttur. Azure Blob depolama için sabit depolama, kullanıcıların iş açısından kritik veri nesnelerini bir solucan içinde depolamasına olanak sağlar (bir kez yaz, çok oku) durumu. Bu durum, verileri silinebilir olmayan ve Kullanıcı tarafından belirtilen bir Aralık için değiştirilemez hale getirir.

- [AzCopy’yi kullanmaya başlama](./storage-use-azcopy-v10.md)

- [BLOB depolama için dengesde kullanılabilirlik ilkelerini ayarlama ve yönetme](../blobs/storage-blob-immutability-policies-manage.md?tabs=azure-portal)

Müşteri tarafından yönetilen/sunulan anahtarlar Azure CLı veya PowerShell kullanarak Azure Key Vault içinde yönetilebilir. 

- [Azure 'da Anahtar Kasası anahtarlarını yedekleme](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: aşağıdaki PowerShell komutlarıyla, Key Vault sertifikalarınızı, anahtarlarınızı, yönetilen depolama hesaplarınızı ve gizli dizileri düzenli aralıklarla geri yükleme işlemi gerçekleştirin: 

Restore-AzKeyVaultCertificate Restore-AzKeyVaultKey Restore-AzKeyVaultManagedStorageAccount Restore-AzKeyVaultSecret 

- [Key Vault sertifikalarını geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultcertificate)

- [Key Vault anahtarlarını geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey)

- [Key Vault yönetilen depolama hesaplarını geri yükleme](/powershell/module/az.keyvault/backup-azkeyvaultmanagedstorageaccount)

- [Key Vault parolaları geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultsecret)

- [AzCopy, bir depolama hesabına blob, dosya ve tablo verilerini kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır](./storage-use-azcopy-v10.md)

Note: Azure Tablo depolama hizmetinize veri kopyalamak istiyorsanız, AzCopy sürüm 7,3 ' yi de yükleyebilirsiniz.


**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: bir depolama hesabında müşteri tarafından yönetilen anahtarları etkinleştirmek için, anahtarlarınızı depolamak üzere bir Azure Key Vault kullanmanız gerekir. Anahtar kasasında hem geçici silme hem de Temizleme özelliklerini etkinleştirmeniz gerekir. Key Vault geçici silme özelliği, anahtarlar, gizlilikler ve sertifikalar gibi silinen kasaların ve kasa nesnelerinin kurtarılmasına olanak tanır. Depolama hesabı verilerini Azure Storage bloblarına yedekliyorsanız, Bloblar veya blob anlık görüntüleri silindiğinde verilerinizi kaydetmek ve kurtarmak için geçici silme özelliğini etkinleştirin. Yedeklemelerinizi hassas veriler olarak değerlendirmeli ve ilgili erişim ve veri koruma denetimlerini Bu temelin bir parçası olarak uygulamalısınız. Ayrıca, geliştirilmiş koruma için, iş açısından kritik veri nesnelerini bir solucan içinde (bir kez yaz, çok oku) saklayabilir.

- [Azure Key Vault geçici silme kullanma](../../key-vault/general/key-vault-recovery.md)

- [Azure Depolama blobları için geçici silme](../blobs/soft-delete-blob-overview.md?tabs=azure-portal)

- [İş açısından kritik blob verilerini sabit depolama alanıyla depolama](../blobs/storage-blob-immutable-storage.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](../../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-incident-response-guide"></a>10,1: olay yanıtı oluştur Kılavuzu

**Rehberlik**: Kuruluşunuz için bir olay yanıt kılavuzu oluşturun. Tüm personelin rollerine ek olarak algılama aşamasından olay sonrası gözden geçirme aşamasına kadar tüm olay işleme/yönetim aşamalarını tanımlayan yazılı olay yanıt planları bulunduğundan emin olun.

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu da kullanabilir](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi yordamı oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır. 

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) etiketleri kullanarak Azure kaynaklarını açıkça tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun. Azure kaynaklarının önem düzeyine ve olayın oluştuğu ortama bağlı olarak uyarıların çözümünde önceliği belirlemek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../../security-center/security-center-alerts-overview.md)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: Azure kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve açıkları belirleyip planı gerektiği şekilde gözden geçirin.

- [NıST 'nin Yayımlama Kılavuzu, BT planları ve becerileri için programları test etme, eğitim ve alıştırma Kılavuzu](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin Iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

- [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](../../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Kılavuz**: Azure kaynaklarına yönelik riskleri belirlemenize yardımcı olmak Için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz.

- [Sürekli dışarı aktarmayı yapılandırma](../../security-center/continuous-export.md)

- [Uyarıların Azure Sentinel’e akışını yapma](../../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Kılavuz**: Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanarak, güvenlik uyarılarındaki "Logic Apps" aracılığıyla yanıtları otomatik olarak tetikleyin ve Azure kaynaklarınızı korumaya yönelik öneriler alın.

- [Iş akışı otomasyonu ve Logic Apps yapılandırma](../../security-center/workflow-automation.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](../../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin

**Rehberlik**: sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin. Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın.

- [Sızma Testi Etkileşim Kuralları](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Bulut ile Kırmızı Takım Oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](../../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin