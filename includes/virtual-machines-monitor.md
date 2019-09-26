---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 01/27/2019
ms.author: cynthn
ms.openlocfilehash: 11c9b2ea3ea054415f25f864651df28288aa0025
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266849"
---
Tanılama ve günlük verilerini toplayarak, görüntüleyerek ve çözümleyerek sanal makinelerinizi izlemek için birçok fırsatın avantajlarından yararlanabilirsiniz. VM 'nizi basit bir şekilde [izlemek](../articles/azure-monitor/overview.md) IÇIN Azure Portal VM Için genel bakış ekranını kullanabilirsiniz. Ek ölçüm verileri toplamak üzere sanal makinelerinizdeki tanılamayı yapılandırmak için [uzantıları](../articles/virtual-machines/windows/extensions-features.md) kullanabilirsiniz. [Application Insights](../articles/azure-monitor/app/app-insights-overview.md) ve [Log Analytics](../articles/azure-monitor/log-query/log-query-overview.md)gibi daha gelişmiş izleme seçenekleri de kullanabilirsiniz.

## <a name="diagnostics-and-metrics"></a>Tanılama ve ölçümler 

Azure portal, Azure CLı, Azure PowerShell ve programlama uygulamaları programlama arabirimleri (API 'Ler) ' nde [ölçümleri](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md) kullanarak [Tanılama verilerinin](https://docs.microsoft.com/cli/azure/vm/diagnostics) toplanmasını ayarlayabilir ve izleyebilirsiniz. Örneğin, şunları yapabilirsiniz:

- **VM için temel ölçümleri gözlemleyin.** Azure portal genel bakış ekranında, gösterilen temel ölçümler CPU kullanımı, ağ kullanımı, disk baytlarının toplamı ve saniye başına disk işlemleri içerir.

- **Önyükleme tanılaması koleksiyonunu etkinleştirin ve Azure portal kullanarak görüntüleyin.** Kendi görüntünüzü Azure 'a getirirken veya platform görüntülerinden birini önyüklerken, bir sanal makinenin önyüklenebilir olmayan bir duruma kaç nedenden kaynaklanabilir. Ayarlar ekranının Izleme bölümünde önyükleme tanılaması için **Etkinleştir** ' i TıKLATARAK bir VM oluşturduğunuzda, önyükleme tanılamayı kolayca etkinleştirebilirsiniz.

    VM 'Ler önyüklemesinde, önyükleme tanılama Aracısı önyükleme çıkışını yakalar ve Azure Storage 'da depolar. Bu veriler VM önyükleme sorunlarını gidermek için kullanılabilir. Komut satırı araçlarından bir VM oluşturduğunuzda önyükleme tanılaması otomatik olarak etkinleştirilmez. Önyükleme tanılamalarını etkinleştirmeden önce, önyükleme günlüklerini depolamak için bir depolama hesabı oluşturulmalıdır. Azure portal önyükleme tanılamayı etkinleştirirseniz, sizin için otomatik olarak bir depolama hesabı oluşturulur.

    VM oluşturulduğunda önyükleme tanılamayı etkinleştirmediyseniz, daha sonra [Azure CLI](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics), [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.compute/set-azvmbootdiagnostic)veya bir [Azure Resource Manager şablonu](../articles/virtual-machines/windows/extensions-diagnostics-template.md)kullanarak her zaman etkinleştirebilirsiniz.

- **Konuk işletim sistemi Tanılama verileri koleksiyonunu etkinleştirin.** Bir VM oluşturduğunuzda, Konuk işletim sistemi tanılamayı etkinleştirmek için Ayarlar ekranında fırsat alırsınız. Tanılama verileri koleksiyonunu etkinleştirdiğinizde, [Linux Için ıaasdiagnostics uzantısı](../articles/virtual-machines/linux/diagnostic-extension.md) veya [Windows Için ıaasdiagnostics](../articles/virtual-machines/windows/ps-extensions-diagnostics.md) uzantısı, ek disk, CPU ve bellek VERILERI toplamanıza olanak sağlayan sanal makineye eklenir.

    Toplanan tanılama verilerini kullanarak, sanal makinelerinize yönelik otomatik ölçeklendirmeyi yapılandırabilirsiniz. Ayrıca, verileri depolamak için günlükleri yapılandırabilir ve performansı gerçekten doğru olmadığında size bildirmek üzere uyarılar ayarlayabilirsiniz.

## <a name="alerts"></a>Uyarılar

Belirli performans ölçümlerine göre [Uyarılar](../articles/azure-monitor/platform/alerts-overview.md) oluşturabilirsiniz. Ortalama CPU kullanımı belirli bir eşiği aştığında veya kullanılabilir boş disk alanı belirli bir miktarın altına düştüğünde, sorun hakkında uyarı alabilirsiniz. Uyarılar [Azure Portal](../articles/azure-monitor/platform/alerts-classic-portal.md), [Azure POWERSHELL](../articles/azure-monitor/platform/alerts-classic-portal.md#with-powershell)veya [Azure CLI](../articles/azure-monitor/platform/alerts-classic-portal.md#with-azure-cli)kullanılarak yapılandırılabilir.

## <a name="azure-service-health"></a>Azure Hizmet Durumu

Azure [hizmet durumu](../articles/service-health/service-health-overview.md) , Azure hizmetlerindeki sorunlardan etkilendiğiniz durumlarda kişiselleştirilmiş rehberlik ve destek sağlar ve yaklaşan planlı bakımın hazırlanmasına yardımcı olur. Azure hizmet durumu, hedeflenen ve esnek bildirimleri kullanarak sizi ve takımlarınızı uyarır.

## <a name="azure-resource-health"></a>Azure Kaynak Durumu

[Azure Kaynak durumu](../articles/service-health/resource-health-overview.md) , kaynaklarınızı etkileyen bir Azure sorunu tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.

## <a name="azure-activity-log"></a>Azure etkinlik günlüğü

[Azure etkinlik günlüğü](../articles/azure-monitor/platform/activity-logs-overview.md) , Azure 'da oluşan abonelik düzeyindeki olaylara ilişkin Öngörüler sağlayan bir abonelik günlüğü. Günlük, Azure Resource Manager işletimsel verilerden hizmet durumu olaylarında güncelleştirmelere kadar bir veri aralığı içerir. VM 'nizin günlüğünü görüntülemek için Azure portal etkinlik günlüğü ' ne tıklayabilirsiniz.

Etkinlik günlüğünde yapabileceğiniz bazı şeyler şunlardır:

- [Etkinlik günlüğü olayında uyarı](../articles/azure-monitor/platform/activity-logs-overview.md)oluşturma.
- Bir üçüncü taraf hizmeti veya PowerBI gibi özel bir analiz çözümünü [almak için bir olay hub 'ına akış](../articles/azure-monitor/platform/activity-logs-stream-event-hubs.md) yapın.
- PowerBI [içerik paketini](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/)kullanarak PowerBI 'da analiz edin.
- Arşiv veya el ile inceleme için [bir depolama hesabına kaydedin](../articles/azure-monitor/platform/archive-activity-log.md) . Günlük profilini kullanarak saklama süresini (gün olarak) belirtebilirsiniz.

Etkinlik günlüğü verilerine [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights/), [Azure CLı](https://docs.microsoft.com/cli/azure/monitor)veya [izleme REST API 'lerini](https://docs.microsoft.com/rest/api/monitor/)kullanarak da erişebilirsiniz.

[Azure Kaynak günlükleri](../articles/azure-monitor/platform/resource-logs-overview.md) , kendi işlemi hakkında zengin ve sık veriler sağlayan, VM 'niz tarafından yayılan günlüklerdir. Kaynak günlükleri, VM 'de gerçekleştirilen işlemler hakkında Öngörüler sunarak etkinlik günlüğünden farklıdır.

Tanılama günlükleri ile gerçekleştirebileceğiniz işlemlerden bazıları şunlardır:

- Denetim veya el ile inceleme için [bunları bir depolama hesabına kaydedin](../articles/azure-monitor/platform/archive-diagnostic-logs.md) . Kaynak tanılama ayarlarını kullanarak saklama süresini (gün olarak) belirtebilirsiniz.
- [Bunları](../articles/azure-monitor/platform/resource-logs-stream-event-hubs.md) bir üçüncü taraf hizmeti veya PowerBI gibi özel bir analiz çözümü tarafından içeri aktarmak için Event Hubs.
- [Log Analytics](../articles/log-analytics/log-analytics-azure-storage.md)ile çözümleyin.

## <a name="advanced-monitoring"></a>Gelişmiş izleme

- [Azure izleyici](../articles/azure-monitor/overview.md) , kullanılabilirliğini ve performansını korumak için bulutunuzu ve şirket içi ortamlarınızı izleyen bir hizmettir. Bulut ve şirket içi ortamlarınızdaki telemetri toplama, çözümleme ve üzerinde işlem yapmaya yönelik kapsamlı bir çözüm sunar. Uygulamalarınızın performansını anlamanıza ve uygulamalarla bağlı oldukları kaynakları etkileyen sorunları önceden tespit etmenize yardımcı olur. Bir [LINUX VM](../articles/virtual-machines/linux/extensions-oms.md) 'ye veya günlük verilerini toplamak ve bir Log Analytics çalışma alanında depolamak üzere Log Analytics aracısını yükleyen BIR [Windows sanal](../articles/virtual-machines/windows/extensions-oms.md) makinesine uzantı yükleyebilirsiniz.

    Windows ve Linux VM 'Leri için, günlüklerin toplanması için önerilen yöntem Log Analytics aracısını yüklemedir. Bir VM 'ye Log Analytics aracısını yüklemenin en kolay yolu, [log ANALYTICS VM Uzantısı](../articles/log-analytics/log-analytics-azure-vm-extension.md)kullanmaktır. Uzantıyı kullanmak yükleme işlemini kolaylaştırır ve aracıyı belirttiğiniz Log Analytics çalışma alanına veri göndermek üzere otomatik olarak yapılandırır. Ayrıca aracı otomatik olarak yükseltilerek her zaman en yeni özellik ve düzeltmelere sahip olmanız sağlanır.

- [Ağ İzleyicisi](../articles/network-watcher/network-watcher-monitoring-overview.md) , VM 'nizi ve ilişkili kaynaklarını, içinde bulundukları ağla ilişkili olarak izlemenize olanak sağlar. Ağ Izleyicisi Aracısı uzantısını bir [LINUX VM](../articles/virtual-machines/linux/extensions-nwa.md) veya [Windows VM](../articles/virtual-machines/windows/extensions-nwa.md)'ye yükleyebilirsiniz.

- [VM'ler için Azure izleyici](../articles/azure-monitor/insights/vminsights-overview.md) , Windows ve Linux sanal makinelerinizin farklı işlemleri ve diğer kaynaklardaki ve dış bağlantılı bağımlılıklar dahil olmak üzere, Windows ve Linux VM 'lerinizin performansını ve sistem durumunu analiz ederek, Azure sanal makinelerinizi (VM) lerse. 

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLı ile [bir Windows sanal makinesini Azure PowerShell](../articles/virtual-machines/windows/tutorial-monitoring.md) veya [Linux sanal makinesini izleme](../articles/virtual-machines/linux/tutorial-monitoring.md)bölümündeki adımları izleyin.
- [İzleme ve tanılama](https://docs.microsoft.com/azure/architecture/best-practices/monitoring)etrafında en iyi uygulamalar hakkında daha fazla bilgi edinin.
