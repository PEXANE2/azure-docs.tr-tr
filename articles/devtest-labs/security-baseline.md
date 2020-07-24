---
title: Azure DevTest Labs için Azure Güvenlik temeli
description: Azure DevTest Labs için Azure Güvenlik temeli
ms.topic: conceptual
ms.date: 07/09/2020
ms.openlocfilehash: 4cdfab5df2e97c2bd39c1c104e9552fb940f24d9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100041"
---
# <a name="azure-security-baseline-for-azure-devtest-labs"></a>Azure DevTest Labs için Azure Güvenlik temeli

Azure DevTest Labs için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' den çizilir ve bu, bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın
**Rehberlik:** Microsoft, Azure kaynakları için zaman kaynaklarını korur. Ancak, işlem kaynaklarınız için zaman eşitleme ayarlarını yönetebilirsiniz.

Azure işlem kaynakları için zaman eşitlemesini yapılandırma hakkında bilgi edinmek için aşağıdaki makaleye bakın: [Azure 'Da Windows VM 'leri Için zaman eşitleme](https://docs.microsoft.com/azure/virtual-machines/windows/time-sync). 

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** MICROSOFT

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma
**Rehberlik:** Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Etkinlik günlükleri, yönetim düzlemi düzeyinde Azure DevTest Labs örneklerinizin üzerinde gerçekleştirilen işlemlere ilişkin öngörüler sağlar. Azure etkinlik günlüğü verilerini kullanarak, DevTest Labs örnekleriniz için yönetim düzlemi düzeyinde herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Daha fazla bilgi için bkz. [Platform günlüklerini ve ölçümlerini farklı hedeflere göndermek için Tanılama ayarları oluşturma](../azure-monitor/platform/diagnostic-settings.md).

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme
**Rehberlik:** Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Etkinlik günlükleri, yönetim düzlemi düzeyinde Azure DevTest Labs örneklerinizin üzerinde gerçekleştirilen işlemlere ilişkin öngörüler sağlar. Azure etkinlik günlüğü verilerini kullanarak, DevTest Labs örnekleriniz için yönetim düzlemi düzeyinde herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Daha fazla bilgi için bkz. [Platform günlüklerini ve ölçümlerini farklı hedeflere göndermek için Tanılama ayarları oluşturma](../azure-monitor/platform/diagnostic-settings.md).

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama
**Rehberlik:** Azure DevTest Labs sanal makineler müşteri tarafından oluşturulur ve aittir. Bu nedenle, bu, kuruluşun izleme sorumluluğudur. İşlem işletim sistemini izlemek için Azure Güvenlik Merkezi 'ni kullanabilirsiniz. İşletim sisteminden Güvenlik Merkezi tarafından toplanan veriler işletim sistemi türü ve sürümü, işletim sistemi (Windows olay günlükleri), çalışan süreçler, makine adı, IP adresleri ve oturum açan kullanıcı içerir. Log Analytics Aracısı Ayrıca kilitlenme bilgi döküm dosyalarını da toplar.

Daha fazla bilgi için aşağıdaki makaleleri inceleyin: 

- [Azure Izleyici ile Azure sanal makine iç konak günlüklerini toplama](../azure-monitor/learn/quick-collect-azurevm.md)
- [Azure Güvenlik Merkezi veri toplamayı anlama](../security-center/security-center-enable-data-collection.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma
***Rehberlik:** Azure Izleyici 'de, kuruluşunuzun uyumluluk düzenlemelerine göre Azure DevTest Labs örneklerinizin ilişkilendirildiği Log Analytics çalışma alanları için günlük tutma süresini ayarlayın.

Daha fazla bilgi için şu makaleye bakın: [günlük tutma parametrelerini ayarlama](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme
**Rehberlik:** Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve Azure DevTest Labs için toplanabilecek etkinlik günlüğü verilerine göre birçok diğer öngörü sağlamak için Log Analytics sorguları çalıştırın.

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/diagnostic-settings.md)
- [Azure Izleyici 'de Log Analytics çalışma alanında Azure etkinlik günlüklerini toplama ve çözümleme](../azure-monitor/platform/activity-log.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir
**Rehberlik:** Azure Log Analytics çalışma alanını, Güvenlik günlüklerine ve Azure DevTest Labs ilgili olaylara yönelik anormal etkinlikleri izlemek ve uyarı almak için kullanın.

Daha fazla bilgi için şu makaleye bakın: [Log Analytics günlük verilerinde uyarı](../azure-monitor/learn/tutorial-response.md) alma

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme
**Rehberlik:** Uygulanamaz. Azure DevTest Labs kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir
**Rehberlik:** Uygulanamaz. Azure DevTest Labs DNS ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir
**Rehberlik:** Azure DevTest Labs, müşteri tarafından sahip olunan ve yönetilen Azure Işlem makineleri oluşturur. İşlem oluşturma olayını ve CommandLine alanını günlüğe kaydetmek için desteklenen tüm Azure Windows sanal makinelerinde Microsoft Monitoring Agent kullanın. Desteklenen Azure Linux sanal makineleri için, her düğüm temelinde konsol günlüğünü el ile yapılandırabilir ve verileri depolamak için Syslog kullanabilirsiniz. Ayrıca, Azure Izleyici Log Analytics çalışma alanını kullanarak günlükleri gözden geçirin ve Azure sanal makinelerindeki günlüğe kaydedilen verilerde sorgular çalıştırın.

- [Azure Güvenlik Merkezinde veri toplama](../security-center/security-center-enable-data-collection.md#data-collection-tier)
- [Azure Izleyici 'de özel sorgular çalıştırma](../azure-monitor/log-query/get-started-queries.md)
- [Azure İzleyici'de Syslog veri kaynakları](../azure-monitor/platform/data-sources-syslog.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleye bakın:

- [Azure DevTest Labs ortamlar için güvenlik uyarıları](environment-security-alerts.md)