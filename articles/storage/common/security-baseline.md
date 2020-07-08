---
title: Azure depolama için Azure Güvenlik temeli
description: Azure depolama için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/23/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: cd7e7df5c789743cf6bd84c6150fd901490bdedd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84751626"
---
# <a name="azure-security-baseline-for-azure-storage"></a>Azure depolama için Azure Güvenlik temeli

Azure depolama için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: belırlı genel IP adresi aralıklarından istemcilere erişimi kısıtlayarak depolama hesabınızın güvenlik duvarını yapılandırın, Azure 'da sanal ağları (VNet) veya belirli Azure kaynaklarını seçin. Ayrıca özel uç noktaları, kuruluşunuzda bulunan depolama hizmetine giden trafik için özel ağlar üzerinden özel olarak da yapılandırabilirsiniz.

Not: klasik depolama hesapları, güvenlik duvarlarını ve sanal ağları desteklemez.

- [Azure Storage güvenlik duvarını yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security#change-the-default-network-access-rule)

- [Azure depolama için özel uç noktaları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-private-endpoints)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-vnet-subnet-and-nic-configuration-and-traffic"></a>1,2: VNet, alt ağ ve NIC yapılandırmasını ve trafiğini izleme ve günlüğe kaydetme

**Kılavuz**: Azure depolama, katmanlı bir güvenlik modeli sağlar. Depolama hesabınıza erişimi, belirtilen IP adreslerinden, IP aralıklarından veya bir Azure sanal ağındaki (VNet) bir alt ağ listesinden kaynaklanan isteklerle sınırlayabilirsiniz. Azure Güvenlik Merkezi 'ni kullanarak ağ kaynaklarınızın güvenliğini sağlamaya yardımcı olmak için ağ koruma önerilerini izleyebilirsiniz. Ayrıca, depolama hesabı güvenlik duvarı aracılığıyla depolama hesapları için yapılandırılmış sanal ağlar/alt ağ için NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir depolama hesabına gönderin. 

Depolama hesabınıza bağlı özel uç noktalarınız varsa, alt ağlar için ağ güvenlik grubu (NSG) kurallarını yapılandıramayacağınızı unutmayın. 

- [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security)

- [NSG akış günlüklerini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

- [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

- [Azure depolama için özel uç noktaları anlama](https://docs.microsoft.com/azure/storage/common/storage-private-endpoints#known-issues)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle Iletişimleri reddetme

**Kılavuz**: Azure depolama hesabınız Için Gelişmiş tehdit koruması 'nı etkinleştirin. Azure depolama için Gelişmiş tehdit koruması, depolama hesaplarına yönelik olağan dışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Azure Güvenlik Merkezi tümleşik uyarıları, ağ iletişiminin başarıyla çözümlenen bir IP adresiyle ilişkilendirildiği etkinliklere, IP adresinin bilinen riskli IP adresi (örneğin, bilinen bir cryptominer) veya daha önce riskli olarak tanınmamış bir IP adresi olup olmadığına bağlıdır. Güvenlik uyarıları, etkinlik durumunda olan bozukluklar gerçekleştiğinde tetiklenir. 

- [Gelişmiş tehdit korumasını etkinleştirme](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection?tabs=azure-portal)

- [Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Rehberlik**: Ağ İzleyicisi paket yakalama, depolama hesabı ile sanal makine arasındaki trafiği izlemek için yakalama oturumları oluşturmanızı sağlar. Yalnızca istediğiniz trafiği yakalamanızı sağlamak için yakalama oturumu için filtreler sağlanır. Paket yakalama, ağ anormalilerin hem yeniden etkin hem de proaktif olarak tanımlanmasına yardımcı olur. Diğer kullanımlar arasında ağ istatistiklerini toplama, ağ erişimi ile ilgili bilgi alma, istemci-sunucu iletişiminde hata ayıklama ve çok daha fazlası yer alır. Paket yakalamalarını uzaktan tetikleyebilmek için, istenen bir sanal makinede bir paket yakalamayı çalıştırmanın yükünü kolaylaştırır ve bu da değerli süreyi kaydeder. 

- [Portalı kullanarak paket yakalamalarını Azure ağ Izleyicisi ile yönetme](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-manage-portal)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemlerini dağıtma

**Rehberlik**: Azure depolama için Gelişmiş tehdit koruması, depolama hesaplarına yönelik olağan dışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Güvenlik uyarıları, etkinlik durumunda olan bozukluklar gerçekleştiğinde tetiklenir. Bu güvenlik uyarıları Azure Güvenlik Merkezi ile tümleşiktir ve ayrıca, şüpheli etkinliklerin ayrıntıları ve tehditleri İnceleme ve düzeltme önerileri ile abonelik yöneticilerine e-posta yoluyla da gönderilir. 

- [Azure depolama için Gelişmiş tehdit koruması yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-your-web-applications"></a>1,7: Web uygulamalarınıza giden trafiği yönetme

**Rehberlik**: uygulanamaz; öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Kılavuz**: depolama hesabınıza erişmesi gereken sanal ağlardaki kaynak Için, ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak üzere yapılandırılmış sanal ağ Için sanal ağ hizmeti etiketleri kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örneğin, depolama) belirterek, karşılık gelen hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir. 

Ağ erişiminin belirli depolama hesaplarına kapsama alınması gerektiğinde, sanal ağ hizmeti uç noktası ilkeleri kullanın.

- [Hizmet etiketlerini kullanma hakkında daha fazla bilgi için](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

- [Azure depolama için sanal ağ hizmet uç noktası ilkeleri hakkında daha fazla bilgi için](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: hizmet

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure Ilkesi Ile Azure depolama hesabınızla ilişkili ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Depolama hesabı kaynaklarınızın ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Storage" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. 

Depolama hesabı ile ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz, örneğin: depolama hesapları bir sanal ağ hizmeti uç noktası kullanmalıdır 

- [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Depolama için Azure Ilke örnekleri](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#storage)

- [Ağ için Azure Ilke örnekleri](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#network)

- [Azure Blueprint oluşturma](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: ağ güvenlik grupları (NSG) ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için "Açıklama" alanını kullanarak ağa giden/giden trafiğe izin veren kuralların iş gereksinimini ve/veya süresini (vs.) belirtin. Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure Ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın. Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz. 

- [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

- [Sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Güvenlik Yapılandırması ile NSG oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını Izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Rehberlik**: ağ kaynakları için yapılandırma değişikliklerini günlüğe kaydetmek üzere Azure ilkesini kullanın. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun. 

- [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Azure Izleyici 'de uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-resource"></a>2,1: onaylanan zaman eşitleme kaynağını kullan

**Rehberlik**: uygulanamaz; Microsoft, Azure depolama hesapları için zaman kaynaklarını korur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: uç noktalar cihazları, ağ kaynakları ve diğer güvenlik sistemleri tarafından oluşturulan güvenlik verilerini toplamak Için Azure izleyici aracılığıyla günlükleri alma. Azure Izleyici 'de analiz sorgulamak ve gerçekleştirmek için Log Analytics çalışma alanlarını kullanın ve isteğe bağlı olarak, sabit depolama ve zorlanan bekletme gibi güvenlik özellikleriyle uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: Azure depolama Analizi Bloblar, kuyruklar ve tablolar için Günlükler sağlar. Hesabınız için hangi günlüklerin kaydedildiğini yapılandırmak üzere Azure portal kullanabilirsiniz. 

- [Azure depolama hesabınız için izlemeyi yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-monitoring-for-a-storage-account)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: Işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure depolama hesabında veya Log Analytics çalışma alanında güvenlik olay günlüklerini depolarken, saklama ilkesini kuruluşunuzun gereksinimlerine göre ayarlayabilirsiniz. 

- [Azure depolama hesabı günlükleri için bekletme ilkesini yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging)

- [Log Analytics veri saklama süresini değiştirme](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Kılavuz**: Azure depolama günlüklerini gözden geçirmek için, Log Analytics teklifiyle sorgu gibi olağan seçenekler ve günlük dosyalarını doğrudan görüntülemenin benzersiz bir seçeneği vardır. Azure depolama 'da Günlükler, doğrudan $logs erişilmesi gereken bloblarda depolanır http://accountname.blob.core.windows.net/ (günlük klasörü varsayılan olarak gizlidir, bu nedenle doğrudan gitmeniz gerekir. Liste komutlarında görüntülenmez) 

Ayrıca, Azure depolama hesabınız için Gelişmiş tehdit koruması 'nı etkinleştirin. Azure depolama için Gelişmiş tehdit koruması, depolama hesaplarına yönelik olağan dışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Güvenlik uyarıları, etkinlik durumunda olan bozukluklar gerçekleştiğinde tetiklenir. Bu güvenlik uyarıları Azure Güvenlik Merkezi ile tümleşiktir ve ayrıca, şüpheli etkinliklerin ayrıntıları ve tehditleri İnceleme ve düzeltme önerileri ile abonelik yöneticilerine e-posta yoluyla da gönderilir. 

- [Verileri günlüğe kaydetme ve gözden geçirme](https://docs.microsoft.com/azure/storage/common/storage-analytics-logging#how-logs-are-stored)

- [Gelişmiş tehdit korumasını etkinleştirme](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Kılavuz**: Azure Güvenlik Merkezi 'nde depolama hesabı Için Gelişmiş tehdit koruması 'nı etkinleştirin. Depolama hesabı için tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Log Analytics çalışma alanınızı Azure Sentinel 'e ekleyin. Bu, bir güvenlik Orchestration otomatik yanıtı (SOAR) çözümü sağlar. Bu, güvenlik sorunlarını gidermek için PlayBook 'ları (otomatikleştirilmiş çözümlerin) oluşturulmasına ve kullanılmasına olanak tanır. 

- [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Azure Güvenlik Merkezi 'nde uyarıları yönetme](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

- [Log Analytics günlük verilerinde uyarı alma](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)

- [Azure Depolama analizini günlüğe kaydetme](https://docs.microsoft.com/azure/storage/common/storage-analytics-logging)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Kılavuz**: Azure Güvenlik Merkezi 'ni kullanarak Azure depolama 'ya yönelik tehdit korumasını etkinleştirerek, karma saygınlığı analizini ve etkin bir Tor çıkış düğümünden şüpheli erişimi (bir anonim hale getirme) kullanın. 

- [Azure depolama için Gelişmiş tehdit koruması yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Kılavuz**: Azure izleyici 'de Azure DNS Analytics (Önizleme) çözümü, güvenlik, performans ve işlemlerde DNS altyapısına ilişkin Öngörüler toplar. Şu anda bu Azure depolama hesaplarını desteklemez, ancak üçüncü taraf DNS günlüğü çözümünü kullanabilirsiniz. 

- [DNS Analizi Preview çözümüyle DNS altyapınız hakkında Öngörüler toplayın](https://docs.microsoft.com/azure/azure-monitor/insights/dns-analytics)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Kıyaslama işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tut

**Kılavuz**: Azure AD 'nin açıkça atanması ve sorgulanabilir olması gereken yerleşik rolleri vardır. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın. 

- [Azure AD 'de PowerShell ile dizin rolü alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Azure depolama hesapları veya Azure Active Directory, varsayılan veya boş parola kavramıdır. Azure depolama, Azure rol tabanlı erişim denetimi 'ni (RBAC) ve paylaşılan anahtar ve paylaşılan erişim Imzalarını (SAS) destekleyen bir erişim denetimi modeli uygular. Paylaşılan anahtar ve SAS kimlik doğrulamasının bir özelliği, çağıran ile hiçbir kimliğin ilişkilendirilmediği ve bu nedenle güvenlik sorumlusu izin tabanlı yetkilendirmenin gerçekleştirilemediği bir özelliğidir. 

- [Azure depolama 'daki verilere erişimi yetkilendirme](https://docs.microsoft.com/azure/storage/common/storage-auth)

- [Azure depolama hesabı için güvenlik sorumlularını ve erişim denetimini anlama](https://docs.microsoft.com/azure/storage/common/storage-introduction)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: depolama hesabınıza erişimi olan adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın. 

Ayrıca, Microsoft Hizmetleri ve Azure ARM için Azure AD Privileged Identity Management ayrıcalıklı rolleri kullanarak, tam zamanında/tam erişimi de etkinleştirebilirsiniz. 

- [Azure Güvenlik Merkezi kimlik ve erişimini anlama](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

- [Privileged Identity Management genel bakış](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3,4: Azure Active Directory çoklu oturum açma kullan (SSO)

**Rehberlik**: mümkün olan yerlerde, tek başına bağımsız kimlik bilgilerini hizmet başına yapılandırmak yerıne Azure Active Directory SSO kullanın. Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini kullanın. 

- [Azure AD ile SSO 'yu anlama](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure depolama 'daki verilere erişimi yetkilendirme](https://docs.microsoft.com/azure/storage/common/storage-auth)

- [Azure Active Directory kullanarak bloblara ve kuyruklara erişim yetkisi verme](https://docs.microsoft.com/azure/storage/common/storage-auth-aad)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="35-use-multifactor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulaması kullanın.

**Rehberlik**: depolama hesabı kaynaklarınızı korumaya yardımcı olmak Için Azure Active Directory çok faktörlü kimlik doğrulamasını etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin. 

- [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

- [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Rehberlik**: depolama hesabı kaynaklarını açmak ve YAPıLANDıRMAK için MFA Ile Paws (ayrıcalıklı erişim iş istasyonları) kullanın. 

- [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Kılavuz**: Azure Güvenlik Merkezi risk algılama uyarılarını Azure izleyici 'ye gönderin ve eylem gruplarını kullanarak özel uyarı/bildirim yapılandırın. Şüpheli etkinlik uyarıları oluşturmak için Azure depolama hesabı için Gelişmiş tehdit koruması 'nı etkinleştirin. Ayrıca, riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın. 

- [Azure depolama hesabı için Gelişmiş tehdit koruması ayarlama](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection)

- [Azure AD risk algılamalarını anlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events)

- [Özel uyarı ve bildirim için eylem gruplarını yapılandırma](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resource-from-only-approved-locations"></a>3,8: Azure kaynağını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek için adlandırılmış konumlar kullanın. 

- [Azure 'da adlandırılmış konumları yapılandırma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (Azure AD) kullanın. Azure AD, bir istemcinin depolama hesabındaki kaynaklara erişimi üzerinde ayrıntılı denetim için rol tabanlı erişim denetimi (RBAC) sağlar.  Daha kolay tehlikeye giren hesap anahtarını kullanmak yerine, en iyi güvenlik uygulaması olarak mümkün olduğunda Azure AD kimlik bilgilerini kullanın. Uygulama tasarımınız, blob depolamaya erişim için paylaşılan erişim imzaları gerektirdiğinde, üstün güvenlik için mümkün olduğunda Kullanıcı temsili paylaşılan erişim imzaları (SAS) oluşturmak için Azure AD kimlik bilgilerini kullanın.

- [Azure AD örneği oluşturma ve yapılandırma](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

- [Yönetim kaynaklarına erişmek için Azure depolama kaynak sağlayıcısını kullanma](https://docs.microsoft.com/azure/storage/common/authorization-resource-provider)

- [Azure Blob ve kuyruk verilerine erişimi Yapılandırma Azure portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal)

- [Azure depolama 'daki verilere erişimi yetkilendirme](https://docs.microsoft.com/azure/storage/common/storage-auth)

- [Paylaşılan erişim imzalarını (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](https://docs.microsoft.com/azure/storage/common/storage-sas-overview)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: depolama hesabı yönetici rollerine sahip olanlar içerebilen eski hesapların keşfedilmesine yardımcı olmak için Azure Active Directory günlüklerini gözden geçirin. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek için Azure kimlik erişimi Incelemelerini kullanın, depolama hesabı kaynaklarına erişmek için kullanılabilecek kurumsal uygulamalara erişim ve rol atamaları. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için Kullanıcı erişiminin düzenli olarak gözden geçirilmesi gerekir.  

Verilerinizin güvenliğine güvenmeksizin Depolama hesabınızdaki kaynaklara güvenli temsilci erişimi sağlamak için paylaşılan erişim imzasını (SAS) de kullanabilirsiniz. İstemcinin erişebileceği kaynakları, bu kaynaklarda sahip oldukları izinleri ve SAS 'ın ne kadar süreyle geçerli olduğunu (diğer parametreler arasında) denetleyebilirsiniz.

Ayrıca, kapsayıcılara ve bloblara anonim okuma erişimini gözden geçirin. Varsayılan olarak kapsayıcıya ve içindeki bloblara yalnızca uygun izinlerin verilmiş olduğu kullanıcılar erişebilir. Azure Izleyici 'yi, anonim kimlik doğrulama koşulunu kullanarak depolama hesaplarına yönelik anonim erişimi uyarmak için kullanabilirsiniz.

Kullanıcı hesabı erişiminin şüpheli olma riskini azaltmak için etkili bir yol, kullanıcılara verdiğiniz erişim süresini sınırlayamaktır. Zaman sınırlı SAS URI 'Leri, bir depolama hesabına Kullanıcı erişiminin süresinin otomatik olarak dolmasına yönelik tek bir yoldur. Ayrıca, depolama hesabı anahtarlarını sık aralıklarla döndürmek, depolama hesabı anahtarları aracılığıyla beklenmeyen erişimin sınırlı bir süre olduğundan emin olmanın bir yoludur.

- [Azure AD raporlamayı anlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

- [Azure depolama hesabı düzeyinde erişimi görüntüleme ve değiştirme](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal)

- [Paylaşılan erişim imzalarını (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](https://docs.microsoft.com/azure/storage/common/storage-sas-overview)

- [Kapsayıcılara ve blob’lara anonim okuma erişimini yönetme](https://docs.microsoft.com/azure/storage/blobs/storage-manage-access-to-resources)

- [Azure portalında depolama hesabını izleme](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account)

- [Depolama hesabı erişim anahtarlarını yönetme](https://docs.microsoft.com/azure/storage/common/storage-account-keys-manage)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Rehberlik**: bir depolama hizmetine başarılı ve başarısız istekler hakkında ayrıntılı bilgileri günlüğe almak için depolama Analizi kullanın. Tüm Günlükler, bir depolama hesabı için Depolama Analizi etkinleştirildiğinde otomatik olarak oluşturulan $logs adlı kapsayıcıda Blok Bloblarında depolanır.

Azure Active Directory Kullanıcı hesapları için Tanılama ayarları oluşturun, Denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına gönderir. İstenen uyarıları Log Analytics çalışma alanı içinde yapılandırabilirsiniz. Azure depolama hesaplarında kimlik doğrulama başarısızlıklarını izlemek için, depolama kaynak ölçümleri için belirli eşiklere ulaşıldığında size bildirimde bulunan uyarılar oluşturabilirsiniz. Ayrıca, anonim kimlik doğrulama koşulunu kullanarak depolama hesaplarına yönelik anonim erişim hakkında uyarı almak için Azure Izleyici 'yi kullanın.

- [Azure Depolama analizini günlüğe kaydetme](https://docs.microsoft.com/azure/storage/common/storage-analytics-logging)

- [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

- [Azure depolama hesapları için ölçüm uyarılarını yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: depolama hesabı kaynaklarınızla ilgili olarak algılanan şüpheli eylemlere yönelik otomatik yanıtları yapılandırmak Için Azure Active Directory risk ve kimlik koruması özelliklerini kullanın. Kuruluşunuzun güvenlik yanıtlarını uygulamak için Azure Sentinel aracılığıyla otomatikleştirilmiş yanıtları etkinleştirmeniz gerekir. 

- [Azure AD riskli oturum açma işlemlerini görüntüleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

- [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Kılavuz**: Microsoft 'un müşteri verilerine erişmesi gereken destek senaryolarında, müşteri kasası (depolama hesabı için Önizleme) müşterilerin müşteri verileri erişim isteklerini gözden geçirmesi ve onaylaması veya reddetmesi için bir arabirim sağlar. Microsoft, kuruluşunuzun depolama hesabı 'nda depolanan gizli dizileri için gerekli değildir ve bu parolalara erişim istemez.

- [Müşteri Kasası anlayın](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen depolama hesabı kaynaklarını izlemeye yardımcı olması için etiketleri kullanın. 

- [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: ortam, veri duyarlılığı gibi bireysel güvenlik etki alanları için ayrı abonelikler, yönetim grupları ve depolama hesapları kullanarak yalıtım uygulayın.  Kullanılan ağların türüne ve alt kümelerine göre, uygulamalarınızın ve kurumsal ortamlarınızın talep ettiği depolama hesaplarınıza erişim düzeyini denetlemek için depolama hesabınızı kısıtlayabilirsiniz. Ağ kuralları yapılandırıldığında, yalnızca belirtilen ağ kümesi üzerinde veri isteyen uygulamalar bir depolama hesabına erişebilir. Azure depolama erişimini Azure AD RBAC aracılığıyla denetleyebilirsiniz. Ayrıca, sanal ağınız ile hizmet arasındaki trafik Microsoft omurga ağı üzerinden geçiş yaparken, genel Internet 'ten etkilenme olasılığını ortadan kaldırarak güvenliği artırmak için özel uç noktaları yapılandırabilirsiniz. 

- [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

- [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

- [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

- [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security)

- [Sanal ağ hizmeti uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleyin ve engelleyin.

**Rehberlik**: hassas bilgileri depolayan veya işleyen depolama hesabı kaynakları Için, etiketleri kullanarak kaynakları hassas olarak işaretleyin. Veri kaybı riskini azaltmak için Azure Güvenlik Duvarı 'nı kullanarak Azure depolama hesapları için giden ağ trafiğini kısıtlayın. 

Ayrıca, çıkış sanal ağ trafiğini hizmet uç noktası üzerinden Azure depolama hesaplarına filtrelemek ve verilerin yalnızca belirli Azure depolama hesaplarına sızmasına izin vermek için sanal ağ hizmet uç noktası ilkelerini kullanın.

- [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview)

- [Azure depolama için sanal ağ hizmet uç noktası ilkeleri](https://docs.microsoft.com/azure/private-link/create-private-endpoint-storage-portal)

- [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: depolama hesabı için gerekli olan güvenli AKTARıMı etkinleştirerek https kullanımını zorunlu kılabilirsiniz. Bu özellik etkinleştirildikten sonra HTTP kullanan bağlantılar reddedilir. Ayrıca, depolama hesabınız için güvenli aktarımı zorlamak üzere Azure Güvenlik Merkezi 'ni ve Azure Ilkesini kullanın.

- [Azure depolama 'da güvenli aktarım gerektirme](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer)

- [Güvenlik Merkezi tarafından izlenen Azure Güvenlik ilkeleri](https://docs.microsoft.com/azure/security-center/security-center-policy-definitions)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: veri tanımlama özellikleri henüz Azure depolama hesabı ve ilgili kaynaklar için kullanılabilir değil. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın. 

- [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Rehberlik**: Azure Active Directory (Azure AD) rol tabanlı erişim denetımı (RBAC) aracılığıyla güvenli kaynaklara erişim haklarını yetkilendirir. Azure depolama, blob veya kuyruk verilerine erişmek için kullanılan ortak izin kümelerini çevreleyen yerleşik RBAC rollerinin bir kümesini tanımlar. 

- [Azure depolama hesabı için RBAC rolleri atama](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal#assign-rbac-roles-using-the-azure-portal)

- [Yönetim kaynaklarına erişmek için Azure depolama kaynak sağlayıcısını kullanma](https://docs.microsoft.com/azure/storage/common/authorization-resource-provider)

- [Azure Blob ve kuyruk verilerine erişimi Yapılandırma Azure portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal)

- [AAD örneği oluşturma ve yapılandırma](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

- [Azure depolama 'daki verilere erişimi yetkilendirme](https://docs.microsoft.com/azure/storage/common/storage-auth)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Kılavuz**: Azure depolama şifrelemesi tüm depolama hesapları için etkin ve devre dışı bırakılamaz. Azure depolama, bulutta kalıcı olduğunda verilerinizi otomatik olarak şifreler. Azure Depolama’dan veri okuduğunuzda veriler döndürülmeden önce Azure Depolama tarafından şifresi çözülür. Azure depolama şifrelemesi, kodun değiştirilmesi veya herhangi bir uygulamaya kod eklemesi gerekmeden verilerinizi Rest 'de korumanıza olanak sağlar. 

- [Rest 'de Azure depolama şifrelemesini anlama](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Izleyici 'Yi Azure etkinlik günlüğü ile kullanarak, değişikliklerin depolama hesabı kaynaklarına ne zaman gerçekleştiği hakkında uyarılar oluşturun. Ayrıca, Azure depolama 'ya karşı yapılan her isteği nasıl yetkilendirdiğini izlemek için Azure depolama günlüğünü de etkinleştirebilirsiniz. Günlükler, bir isteğin bir OAuth 2,0 belirteci kullanılarak, paylaşılan anahtar kullanılarak veya paylaşılan erişim imzası (SAS) kullanılarak yapılıp yapılmadığını belirtir. Ayrıca, anonim kimlik doğrulama koşulunu kullanarak depolama hesaplarına yönelik anonim erişim hakkında uyarı almak için Azure Izleyici 'yi kullanın.

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

- [Azure Depolama analizini günlüğe kaydetme](https://docs.microsoft.com/azure/storage/common/storage-analytics-logging)

- [Azure depolama hesapları için ölçüm uyarılarını yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: depolama hesaplarınızın yapılandırmasını sürekli olarak denetlemek ve Izlemek Için Azure Güvenlik Merkezi 'nin önerilerini izleyin. 

- [Güvenlik önerileri-bir başvuru kılavuzu](https://docs.microsoft.com/azure/security-center/recommendations-reference)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik Işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Microsoft, depolama hesaplarını destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için bir risk derecelendirme işlemi kullanın.

**Kılavuz**: Azure Güvenlik Merkezi tarafından sunulan varsayılan risk derecelendirmelerini (güvenli puan) kullanın. 

- [Azure Güvenlik Merkezi güvenli Puanını anlama](https://docs.microsoft.com/azure/security-center/security-center-secure-score)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz içindeki tüm kaynakları (depolama hesapları dahil) sorgulamak ve öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun. 

- [Azure Graph ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

- [Azure aboneliklerinizi görüntüleme](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Azure RBAC 'yi anlama](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: mantıksal olarak bunları bir taksonomi halinde düzenlemek için meta veriler sağlayan depolama hesabı kaynaklarına Etiketler uygulayın. 

- [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: depolama hesaplarını ve ilgili kaynakları düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun. 

Ayrıca, yetkisiz Azure kaynaklarını algılamak için Azure depolama için Gelişmiş tehdit koruması 'nı kullanın. 

- [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

- [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

- [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

- [Azure depolama için Gelişmiş tehdit koruması yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-maintain-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini saklayın.

**Rehberlik**: Kurumsal gereksinimlerinize göre onaylanan Azure kaynakları envanterini oluşturmanız gerekecektir. 


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın: 

 - İzin verilmeyen kaynak türleri 
 - İzin verilen kaynak türleri 

Ayrıca, Azure Kaynak grafiğini kullanarak abonelikler içindeki kaynakları sorgular/bulur. Bu, depolama hesapları gibi yüksek güvenlik tabanlı ortamlarda yardımcı olabilir. 

- [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Azure Graph ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: Işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: müşteri, Azure ilkesiyle müşterinin Şirket ilkelerinin gerektirdiği şekilde kaynak oluşturulmasını veya kullanımını engelleyebilir. 

- [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın: 

- İzin verilmeyen kaynak türleri 
- İzin verilen kaynak türleri 

- [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager-via-scripts"></a>6,11: kullanıcıların betikler aracılığıyla Azure Resource Manager etkileşim kurma yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimini kullanın. Bu, depolama hesaplarına sahip olanlar gibi yüksek bir güvenlik ortamındaki kaynaklarda oluşturma ve değişiklik yapılmasını önleyebilir. 

- [ARM 'e erişimi engellemek için koşullu erişimi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların Işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

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

**Rehberlik**: depolama hesabı örneklerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. Storage" ad alanındaki Azure ilke diğer adlarını kullanın. Azure depolama hesabı için yerleşik Azure Ilke tanımlarını da kullanabilirsiniz, örneğin: 

Depolama hesaplarına Kısıtlanmamış ağ erişimini denetleme  
Depolama hesaplarında Gelişmiş tehdit koruması dağıtma  
Depolama hesaplarının yeni Azure Resource Manager kaynaklarına geçirilmesi gerekir  
Depolama hesaplarına güvenli aktarım etkinleştirilmelidir  

Azure Güvenlik Merkezi 'nden, depolama hesaplarınız için güvenli bir yapılandırma temeli olarak öneriler kullanın. 

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="72-establish-secure-configurations-for-your-operating-system"></a>7,2: Işletim sisteminiz için güvenli yapılandırma oluşturma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-configurations-for-all-azure-resources"></a>7,3: tüm Azure kaynakları için güvenli yapılandırmaların bakımını yapma

**Rehberlik**: depolama hesabı kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yok] kullanın. 

- [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Azure Ilke efektlerini anlama](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-configurations-for-operating-systems"></a>7,4: Işletim sistemleri için güvenli yapılandırmaların bakımını yapma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilkeleri, Azure Resource Manager şablonları, Istenen durum yapılandırma betikleri vb. gibi kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın. Azure DevOps 'da yönettiğiniz kaynaklara erişmek için, Azure DevOps ile tümleşikse veya TFS ile tümleşikse Active Directory belirli kullanıcılara, yerleşik güvenlik gruplarına veya Azure Active Directory (Azure AD) tanımlanmış gruplara izin verebilir veya vermeyebilirsiniz.

- [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure DevOps 'da izinler ve gruplar hakkında](https://docs.microsoft.com/azure/devops/organizations/security/about-permissions)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel Işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Rehberlik**: depolama hesabı için sistem yapılandırmasını uyarmak, denetlemek ve zorlamak Için Azure ilkesinden yararlanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin. 

- [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: Işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma Izlemeyi uygulayın

**Kılavuz**: Azure depolama hesabı kaynaklarınız için temel taramalar gerçekleştirmek üzere Azure Güvenlik Merkezi 'nden yararlanın. 

- [Azure Güvenlik Merkezi 'nde öneriler nasıl düzeltileceği](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: Işletim sistemleri için otomatik yapılandırma Izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-securely-manage-azure-secrets"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: Azure Storage, verileri buluta kalıcı hale geldiğinde otomatik olarak şifreler. Depolama hesabının şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya şifrelemeyi kendi anahtarlarıyla yönetebilirsiniz. Müşteri tarafından sağlanmış anahtarlar kullanıyorsanız, anahtarları güvenli bir şekilde depolamak için Azure Key Vault özelliğinden yararlanabilirsiniz. 

Ayrıca, depolama hesabı anahtarlarının kaybının veya açıklanmasının etkilerini sınırlamak için depolama hesabı anahtarlarını sık aralıklarla döndürün.

- [Bekleyen veri için Azure Depolama şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)

- [Depolama hesabı erişim anahtarlarını yönetme](https://docs.microsoft.com/azure/storage/common/storage-account-keys-manage)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-securely-and-automatically-manage-identities"></a>7,12: kimlikleri güvenli ve otomatik olarak Yönet

**Rehberlik**: Azure Active Directory ve yönetilen kimlikler Ile Azure depolama hesapları içindeki bloblara ve kuyruklara erişim yetkisi verin. Azure Blob ve kuyruk depolama, Azure kaynakları için yönetilen kimliklerle Azure Active Directory (Azure AD) kimlik doğrulamasını destekler. Azure kaynakları için Yönetilen kimlikler, Azure sanal makinelerinde (VM), işlev uygulamalarında, sanal makine ölçek kümelerinde ve diğer hizmetlerde çalışan uygulamalardan Azure AD kimlik bilgilerini kullanarak blob ve kuyruk verilerine erişim yetkisi verebilir. Azure AD kimlik doğrulamasıyla birlikte Azure kaynakları için Yönetilen kimlikler kullanarak, kimlik bilgilerini bulutta çalışan uygulamalarınızla depolamaktan kaçınabilirsiniz. 

- [Yönetilen kimlik kullanarak Azure Blob ve kuyruk verilerine erişim verme](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder. 

- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft, temel alınan platform için kötü amaçlı yazılımdan koruma uygular.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: karma saygınlığı analizini ve etkin bir Tor çıkış düğümünden şüpheli erişimi (bir anonim yönetici) kullanarak Azure depolama 'ya kötü amaçlı yazılım yüklemelerini algılamak için tehdit koruması 'nı kullanın. 

Ayrıca, App Service, Data Lake Storage, BLOB depolama gibi işlem dışı Azure kaynaklarına yüklemeden önce kötü amaçlı yazılımların herhangi bir içeriğini önceden tarayabilirsiniz ve kuruluşunuzun gereksinimlerini karşılayabilirsiniz.

- [Azure depolama için Gelişmiş tehdit koruması yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve Imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft, temel alınan platform için kötü amaçlı yazılımdan koruma uygular.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: Microsoft Azure Depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman otomatik olarak çoğaltılır. Azure depolama, verilerinizi geçici donanım arızaları, ağ veya güç kesintileri ve çok büyük doğal olağanüstü durumlar dahil olmak üzere planlı ve planlanmamış olaylardan korunabilecek şekilde kopyalar. Verilerinizi aynı veri merkezinde, aynı bölgedeki ZGen veri merkezlerinde veya coğrafi olarak ayrılmış bölgelerde çoğaltmayı tercih edebilirsiniz. 

Ayrıca, Azure Otomasyonu ' nu Blobların düzenli anlık görüntülerini alacak şekilde etkinleştirebilirsiniz.

- [Azure depolama yedekliliği ve hizmet düzeyi sözleşmelerini anlama](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

- [Blob 'un anlık görüntüsünü oluşturma](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob)

- [Azure Otomasyonu’na genel bakış](https://docs.microsoft.com/azure/automation/automation-intro)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: depolama hesabı tarafından desteklenen hizmetlerden veri yedeklemek için AzCopy veya üçüncü taraf araçlarını kullanma gibi birden çok yöntem mevcuttur. Azure Blob depolama için sabit depolama, kullanıcıların iş açısından kritik veri nesnelerini bir solucan içinde depolamasına olanak sağlar (bir kez yaz, çok oku) durumu. Bu durum, verileri silinebilir olmayan ve Kullanıcı tarafından belirtilen bir Aralık için değiştirilemez hale getirir.

- [AzCopy’yi kullanmaya başlama](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)

- [BLOB depolama için dengesde kullanılabilirlik ilkelerini ayarlama ve yönetme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutability-policies-manage?tabs=azure-portal)

Müşteri tarafından yönetilen/sunulan anahtarlar Azure CLı veya PowerShell kullanarak Azure Key Vault içinde yönetilebilir. 

- [Azure 'da Anahtar Kasası anahtarlarını yedekleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: aşağıdaki PowerShell komutlarıyla, Key Vault sertifikalarınızı, anahtarlarınızı, yönetilen depolama hesaplarınızı ve gizli dizileri düzenli aralıklarla geri yükleme işlemi gerçekleştirin: 

Restore-AzKeyVaultCertificate restore-AzKeyVaultKey geri yükleme-AzKeyVaultManagedStorageAccount geri yükleme-AzKeyVaultSecret 

- [Key Vault sertifikalarını geri yükleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultcertificate?view=azurermps-6.13.0)

- [Key Vault anahtarlarını geri yükleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

- [Key Vault yönetilen depolama hesaplarını geri yükleme](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultmanagedstorageaccount)

- [Key Vault parolaları geri yükleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultsecret?view=azurermps-6.13.0)

- [AzCopy, bir depolama hesabına blob, dosya ve tablo verilerini kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)

Note: Azure Tablo depolama hizmetinize veri kopyalamak istiyorsanız, AzCopy sürüm 7,3 ' yi de yükleyebilirsiniz.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: bir depolama hesabında müşteri tarafından yönetilen anahtarları etkinleştirmek için, anahtarlarınızı depolamak üzere bir Azure Key Vault kullanmanız gerekir. Anahtar kasasında hem geçici silme hem de Temizleme özelliklerini etkinleştirmeniz gerekir. Key Vault geçici silme özelliği, anahtarlar, gizlilikler ve sertifikalar gibi silinen kasaların ve kasa nesnelerinin kurtarılmasına olanak tanır. Depolama hesabı verilerini Azure Storage bloblarına yedekliyorsanız, Bloblar veya blob anlık görüntüleri silindiğinde verilerinizi kaydetmek ve kurtarmak için geçici silme özelliğini etkinleştirin. Yedeklemelerinizi hassas veriler olarak değerlendirmeli ve ilgili erişim ve veri koruma denetimlerini Bu temelin bir parçası olarak uygulamalısınız. Ayrıca, geliştirilmiş koruma için, iş açısından kritik veri nesnelerini bir solucan içinde (bir kez yaz, çok oku) saklayabilir.

- [Azure Key Vault geçici silme kullanma](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell)

- [Azure Depolama blobları için geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

- [Sabit depolamayla iş açısından kritik blob verilerini depolayın](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-incident-response-guide"></a>10,1: olay yanıtı oluştur Kılavuzu

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu da kullanabilir](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi yordamı oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır. 

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) etiketleri kullanarak Azure kaynaklarını açıkça tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun. Olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak, uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](https://docs.microsoft.com/azure/security-center/security-center-alerts-overview)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: Azure kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

- [NıST 'nin Yayımlama Kılavuzu, BT planları ve becerileri için programları test etme, eğitim ve alıştırma Kılavuzu](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin Iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

- [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Kılavuz**: Azure kaynaklarına yönelik riskleri belirlemenize yardımcı olmak Için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz.

- [Sürekli dışarı aktarmayı yapılandırma](https://docs.microsoft.com/azure/security-center/continuous-export)

- [Uyarıları Azure Sentinel 'e akış](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Kılavuz**: Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanarak, güvenlik uyarılarındaki "Logic Apps" aracılığıyla yanıtları otomatik olarak tetikleyin ve Azure kaynaklarınızı korumaya yönelik öneriler alın.

- [Iş akışı otomasyonu ve Logic Apps yapılandırma](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin

**Rehberlik**: sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin. Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın.

- [Katılım test kurallarını oyma](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Bulut Kırmızı ekip oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
