---
title: include dosyası
description: include dosyası
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/30/2019
ms.author: cynthn
ms.openlocfilehash: feea9696316723a2750be6fc1e13001224320324
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81865622"
---
Azure'da barındırılan VM'lerin önemli ölçüde büyümesiyle, destekledikleri uygulamaları ve altyapı hizmetlerini etkileyen performans ve sistem durumu sorunlarını belirlemek önemlidir. Temel izleme, ana bilgisayar hipervizörü tarafından toplanan metrik cpu kullanımı, disk kullanımı, bellek kullanımı ve ağ trafiği tarafından Varsayılan olarak Azure ile teslim edilir. Ek metrik ve günlük verileri, konuk işletim sisteminden VM'lerinizdeki tanılamaları yapılandırmak için [uzantılar](../articles/virtual-machines/windows/extensions-features.md) kullanılarak toplanabilir.

Azure Monitor, vm'ler ve Uygulama Öngörüleri için Azure Monitor gibi kapsamlı özelliklerle, vm'ler içinde çalışan konuk işletim sistemi ,.NET tabanlı veya Java web uygulama bileşenleriyle performans ve sistem durumu sorunlarını algılamaya ve tanılamaya yardımcı olmak için merkezi izleme sağlar.

## <a name="diagnostics-and-metrics"></a>Tanılama ve ölçümler 

Azure portalı, Azure CLI, Azure PowerShell ve programlama Uygulamaları Programlama Arabirimleri (API'ler) [ölçümlerini](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md) kullanarak [tanılama verilerinin](https://docs.microsoft.com/cli/azure/vm/diagnostics) toplanmasını ayarlayabilir ve izleyebilirsiniz. Örneğin, şunları yapabilirsiniz:

- **VM için temel ölçümleri gözlemleyin.** Azure portalının Genel Bakış ekranında, gösterilen temel ölçümler arasında CPU kullanımı, ağ kullanımı, saniyede disk baytlarının toplamı ve disk işlemleri yer almaktadır.

- **Önyükleme tanılama koleksiyonunu etkinleştirin ve Azure portalını kullanarak görüntüleyin.** Kendi resminizi Azure'a getirirken veya platform görüntülerinden birini önyüklemeye çalışırken, VM'nin önyükleme yapılamayan bir duruma girmesinin birçok nedeni olabilir. Ayarlar ekranının İzleme bölümü altında **Önyükleme** Tanılama için Etkin'i tıklatarak bir VM oluşturduğunuzda önyükleme tanılamaözelliğini kolayca etkinleştirebilirsiniz.

    VM önyükleme olarak, önyükleme tanı aracısı önyükleme çıktısını yakalar ve Azure depolama alanında saklar. Bu veriler VM önyükleme sorunlarını gidermek için kullanılabilir. Komut satırı araçlarından bir VM oluşturduğunuzda önyükleme tanılama otomatik olarak etkinleştirilir. Önyükleme tanılamalarını etkinleştirmeden önce, önyükleme günlüklerini depolamak için bir depolama hesabı oluşturulmalıdır. Azure portalında önyükleme tanılamasını etkinleştiriseniz, sizin için otomatik olarak bir depolama hesabı oluşturulur.

    VM oluşturulduğunda önyükleme tanılamasını etkinleştirmediyseniz, [Azure CLI](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics), [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.compute/set-azvmbootdiagnostic)veya Azure [Kaynak Yöneticisi şablonu](../articles/virtual-machines/windows/extensions-diagnostics-template.md)kullanarak bunu her zaman daha sonra etkinleştirebilirsiniz.

- **Konuk işletim sistemi tanılama verilerinin toplanmasını etkinleştirin.** Bir VM oluşturduğunuzda, ayarlar ekranında konuk işletim sistemi tanılamalarını etkinleştirme fırsatına sahip olabilirsiniz. Tanılama verilerinin toplanmasını etkinleştirdiğinizde, [Linux için IaaSDiagnostics uzantısı](../articles/virtual-machines/linux/diagnostic-extension.md) veya Windows için [IaaSDiagnostics uzantısı](../articles/virtual-machines/windows/ps-extensions-diagnostics.md) VM'ye eklenir ve bu da ek disk, CPU ve bellek verileri toplamanızı sağlar.

    Toplanan tanılama verilerini kullanarak, Sanal Cihazlarınız için otomatik ölçekleme yapılandırabilirsiniz. Azure [Monitör Günlükleri'ni](../articles/azure-monitor/platform/data-platform-logs.md) verileri depolamak ve performansın doğru olmadığını bildirmek için uyarılar ayarlamak için de yapılandırabilirsiniz.

## <a name="alerts"></a>Uyarılar

Belirli performans ölçümlerine dayalı [uyarılar](../articles/azure-monitor/platform/alerts-overview.md) oluşturabilirsiniz. Hakkında uyarılabileceğiniz sorunlara örnek olarak, ortalama CPU kullanımının belirli bir eşiği aşması veya kullanılabilir boş disk alanının belirli bir miktarın altına düşmesi verilebilir. Uyarılar Azure [portalında](../articles/azure-monitor/platform/alerts-metric.md#create-with-azure-portal), [Azure Kaynak Yöneticisi şablonları](../articles/azure-monitor/platform/alerts-metric-create-templates.md)veya Azure [CLI](../articles/azure-monitor/platform/alerts-metric.md#with-azure-cli)kullanılarak yapılandırılabilir.

## <a name="azure-service-health"></a>Azure Hizmet Durumu

[Azure Hizmet Durumu,](../articles/service-health/service-health-overview.md) Azure hizmetlerindeki sorunlar sizi etkilediğinde kişiselleştirilmiş rehberlik ve destek sağlar ve yaklaşan planlanan bakıma hazırlanmanıza yardımcı olur. Azure Hizmet Durumu, hedefli ve esnek bildirimler kullanarak sizi ve ekiplerinizi uyarır.

## <a name="azure-resource-health"></a>Azure Kaynak Durumu

[Azure Kaynak durumu,](../articles/service-health/resource-health-overview.md) bir Azure sorunu kaynaklarınızı etkilediğinde tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.

## <a name="azure-activity-log"></a>Azure Etkinlik Günlüğü

[Azure Etkinlik Günlüğü,](../articles/azure-monitor/platform/platform-logs-overview.md) Azure'da gerçekleşen abonelik düzeyi olaylarına ilişkin bilgi sağlayan bir abonelik günlüğüdür. Günlük, Azure Kaynak Yöneticisi operasyonel verilerinden Hizmet Durumu etkinliklerindeki güncelleştirmelere kadar çeşitli verileri içerir. VM'nizin günlüğünü görüntülemek için Azure portalında Etkinlik Günlüğü'ne tıklayabilirsiniz.

Etkinlik günlüğüyle yapabileceğiniz şeylerden bazıları şunlardır:

- Etkinlik [Günlüğü olayında bir uyarı](../articles/azure-monitor/platform/platform-logs-overview.md)oluşturun.
- Power BI gibi bir üçüncü taraf hizmet veya özel analiz çözümü tarafından yutulması için bir Etkinlik Hub'ına [aktarın.](../articles/azure-monitor/platform/activity-logs-stream-event-hubs.md)
- [Power BI içerik paketini](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/)kullanarak Power BI'de analiz edin.
- Arşivleme veya el ile denetim için [bir depolama hesabına kaydedin.](../articles/azure-monitor/platform/archive-activity-log.md) Günlük Profilini kullanarak bekletme süresini (gün içinde) belirtebilirsiniz.

[Ayrıca Azure PowerShell, Azure](https://docs.microsoft.com/powershell/module/azurerm.insights/) [CLI](https://docs.microsoft.com/cli/azure/monitor)veya Monitor REST [API'lerini](https://docs.microsoft.com/rest/api/monitor/)kullanarak etkinlik günlüğü verilerine de erişebilirsiniz.

[Azure Kaynak Günlükleri,](../articles/azure-monitor/platform/platform-logs-overview.md) VM'niz tarafından yayılan ve çalışması hakkında zengin ve sık veri sağlayan günlüklerdir. Kaynak günlükleri, VM içinde gerçekleştirilen işlemler hakkında bilgi sağlayarak etkinlik günlüğünden farklıdır.

Tanılama günlükleri ile yapabileceğiniz şeylerden bazıları şunlardır:

- Bunları denetim veya el ile denetim için [bir depolama hesabına](../articles/azure-monitor/platform/archive-diagnostic-logs.md) kaydedin. Kaynak Tanılama Ayarlarını kullanarak bekletme süresini (gün içinde) belirtebilirsiniz.
- Power BI gibi bir üçüncü taraf hizmet veya özel analiz çözümü tarafından yutulması için [bunları Olay Hub'larına aktarın.](../articles/azure-monitor/platform/resource-logs-stream-event-hubs.md)
- [Log Analytics](../articles/log-analytics/log-analytics-azure-storage.md)ile analiz edin.

## <a name="advanced-monitoring"></a>Gelişmiş izleme

Azure VM ve sanal makine ölçeği kümeleri tarafından desteklenen uygulama veya hizmetin görünürlüğü için, uygulamanın kullanılabilirliğini veya performansını etkileyip etkilemediği veya uygulamayla ilgili bir sorun olup olmadığını anlamak için konuk işletim sistemi veya VM'de çalışan iş yüküyle ilgili sorunların tanımlanması, hem [Sanal Ayarlar için Azure Monitörü'nü](../articles/azure-monitor/insights/vminsights-overview.md) hem de Uygulama [Öngörüleri'ni](../articles/azure-monitor/app/app-insights-overview.md)etkinleştirin.

Sanal Makineler için Azure Monitor, keşfettiği diğer kaynaklara ve dış işlemlere farklı süreçler ve birbirine bağlı bağımlılıklar da dahil olmak üzere Windows ve Linux VM'lerinizin performansını ve sistem durumunu analiz ederek Azure sanal makinelerinizi (VM) ölçekte izler. Sorunların araştırılması na ve VM'lerinizin kapasitesini değerlendirmeye yardımcı olmak için çeşitli eğilim performans çizelgeleri içerir. Bağımlılık haritası, izlenen ve izlenmeyen makineleri, işlemler ve bu makineler arasındaki başarısız ve etkin ağ bağlantılarını ve standart ağ bağlantısı ölçümlerine sahip eğilim grafiklerini gösterir. Uygulama Öngörüleri ile birlikte, UYGULAMANIZla birlikte, UYGULAMANIZ la uygulamanız arasındaki sorunları ilişkilendirebilmeniz için uygulamanızı izler ve HTTP istekleri, özel durumlar vb. gibi telemetrileri yakalarsınız. Azure Monitor tarafından VM'ler için toplanan verilerin izlenmesinden algılanan önemli koşullar hakkında sizi uyarmak için [Azure Monitor uyarılarını](../articles/azure-monitor/platform/alerts-overview.md) yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure PowerShell ile [Windows Sanal Makinesini İzle'deki](../articles/virtual-machines/windows/tutorial-monitoring.md) adımlardan yürüyün veya Azure [CLI ile bir Linux Sanal Makinesini izleyin.](../articles/virtual-machines/linux/tutorial-monitoring.md)

- [İzleme ve tanılama](https://docs.microsoft.com/azure/architecture/best-practices/monitoring)ile ilgili en iyi uygulamalar hakkında daha fazla bilgi edinin.
