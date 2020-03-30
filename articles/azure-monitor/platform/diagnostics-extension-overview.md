---
title: Azure Tanılama uzantısına genel bakış
description: Bulut hizmetlerinde, sanal makinelerde ve hizmet kumaşında hata ayıklama, performansı ölçme, izleme, trafik analizi için Azure tanılamayı kullanın
ms.subservice: diagnostic-extension
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/14/2020
ms.openlocfilehash: 6cb514312db525ffd2ccf9f7b70968daaa94f322
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672387"
---
# <a name="azure-diagnostics-extension-overview"></a>Azure Tanılama uzantısına genel bakış
Azure Tanılama uzantısı, [Azure Monitor'da](agents-overview.md) sanal makineler de dahil olmak üzere Azure bilgi işlem kaynaklarının konuk işletim sisteminden izleme verileri toplayan bir aracıdır. Bu makalede, desteklediği belirli işlevler ve yükleme ve yapılandırma seçenekleri de dahil olmak üzere Azure Tanılama uzantısına genel bir bakış sağlar. 

> [!NOTE]
> Azure Tanılama uzantısı, bilgi işlem kaynaklarının konuk işletim sisteminden izleme verilerini toplamak için kullanılabilen aracılardan biridir. Farklı aracıların açıklaması için [Azure Monitor aracılarının genel görünümü](agents-overview.md) ve gereksinimleriniz için uygun aracıların seçilmesi yle ilgili kılavuza bakın.

## <a name="comparison-to-log-analytics-agent"></a>Log Analytics aracısı ile karşılaştırma
Azure Monitor'daki Log Analytics aracısı, sanal makinelerin konuk işletim sisteminden izleme verileri toplamak için de kullanılabilir. Gereksinimlerinize bağlı olarak her ikisini veya her ikisini de kullanmayı seçebilirsiniz. Azure Monitörü aracılarının ayrıntılı bir karşılaştırması için [Azure Monitor aracılarının genel görünümüne](agents-overview.md) bakın. 

Göz önünde bulundurulması gereken temel farklar şunlardır:

- Azure Tanı Uzantısı yalnızca Azure sanal makineleriyle kullanılabilir. Log Analytics aracısı Azure'daki sanal makinelerle, diğer bulutlarda ve şirket içinde kullanılabilir.
- Azure Tanılama uzantısı verileri Azure Depolama, [Azure Monitör Ölçümleri](data-platform-metrics.md) (yalnızca Windows) ve Etkinlik Hub'larına gönderir. Log Analytics aracısı [Azure Monitör Günlükleri](data-platform-logs.md)için veri toplar.
- Log Analytics aracısı [çözümler,](../monitor-reference.md#insights-and-core-solutions) [VM'ler için Azure Monitör](../insights/vminsights-overview.md)ve [Azure Güvenlik Merkezi](/azure/security-center/)gibi diğer hizmetler için gereklidir.

## <a name="costs"></a>Maliyetler
Azure Tanı Uzantısı için herhangi bir ücret yoktur, ancak alınan veriler için ücrete tabi olabilirsiniz. Veri topladığınız hedef için [Azure Monitörfiyatlandırmasını](https://azure.microsoft.com/pricing/details/monitor/) kontrol edin.

## <a name="data-collected"></a>Toplanan veriler
Aşağıdaki tablolar, Windows ve Linux tanılama uzantısı tarafından toplanabilecek verileri listeleyiştir.

### <a name="windows-diagnostics-extension-wad"></a>Windows tanılama uzantısı (WAD)

| veri kaynağı | Açıklama |
| --- | --- |
| Windows Olay günlükleri   | Windows olay günlüğünden olaylar. |
| Performans sayaçları | İşletim sisteminin ve iş yüklerinin farklı yönlerinin performansını ölçen sayısal değerler. |
| IIS Günlükleri             | Konuk işletim sisteminde çalışan IIS web sitelerinin kullanım bilgileri. |
| Uygulama günlükleri     | Uygulamanız tarafından yazılan iletileri izleme. |
| .NET EventSource günlükleri |.NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) sınıfını kullanarak kod yazma olayları |
| [Manifest tabanlı ETW günlükleri](https://docs.microsoft.com/windows/desktop/etw/about-event-tracing) |Herhangi bir işlem tarafından oluşturulan Windows olayları için Olay İzleme. |
| Kilitlenme dökümleri (günlükler)   | Bir uygulama çökerse işlemin durumu hakkında bilgi. |
| Dosya tabanlı günlükler    | Uygulamanız veya hizmetiniz tarafından oluşturulan günlükler. |
| Aracı tanılama günlükleri | Azure Tanılama'nın kendisi hakkında bilgi. |


### <a name="linux-diagnostics-extension-lad"></a>Linux tanılama uzantısı (LAD)

| veri kaynağı | Açıklama |
| --- | --- |
| Syslog | Linux olay günlüğü sistemine gönderilen olaylar.   |
| Performans sayaçları  | İşletim sisteminin ve iş yüklerinin farklı yönlerinin performansını ölçen sayısal değerler. |
| Günlük dosyaları | Dosya tabanlı bir günlük gönderilen girişler.  |

## <a name="data-destinations"></a>Veri hedefleri
Hem Windows hem de Linux için Azure Tanılama uzantısı her zaman bir Azure Depolama hesabında veri toplar. Bkz. [Windows Azure tanılama uzantısını (WAD) yükle ve yapılandırın](diagnostics-extension-windows-install.md) ve bu verilerin toplandığı belirli tabloların ve lekelerin listesi için [ölçümleri ve günlükleri izlemek için Linux Tanı uzantısını kullanın.](../../virtual-machines/extensions/diagnostics-linux.md)

Diğer ek hedeflere veri göndermek için bir veya daha fazla *veri lavabosu* yapılandırın. Aşağıdaki bölümlerde Windows ve Linux tanılama uzantısı için kullanılabilir lavabolar listelenmiştir.

### <a name="windows-diagnostics-extension-wad"></a>Windows tanılama uzantısı (WAD)

| Hedef | Açıklama |
|:---|:---|
| Azure Monitör Ölçümleri | Azure Monitör Ölçümleri için performans verileri toplayın. [Bkz. Azure Monitor metrik veritabanına Konuk İşletim Sistemi ölçümleri gönderin.](collect-custom-metrics-guestos-resource-manager-vm.md)  |
| Event Hubs | Azure dışında veri göndermek için Azure Etkinlik Hub'larını kullanın. Bkz. [Azure Tanılama verilerini Olay Hub'larına Akış](diagnostics-extension-stream-event-hubs.md) |
| Azure Depolama lekeleri | Tablolara ek olarak Azure Depolama'daki blob'lara verilere yazın. |
| Application Insights | Diğer uygulama izleme ile tümleştirmek için VM'inizde çalışan uygulamalardan Application Insights'a kadar veri toplayın. Bkz. [Tanılama verilerini Uygulama Öngörülerine Gönder.](diagnostics-extension-to-application-insights.md) |

Log Analytics aracısı genellikle bu işlevsellik için kullanılsa da, azure monitör günlükleriyle analiz etmek için depolama alanından bir Log Analytics çalışma alanına WAD verileri de toplayabilirsiniz. Verileri doğrudan bir Log Analytics çalışma alanına gönderebilir ve ek işlevsellik sağlayan çözümleri ve öngörüleri destekler.  Bkz. [Azure Depolama'dan Azure tanı günlüklerini topla.](diagnostics-extension-logs.md) 


### <a name="linux-diagnostics-extension-lad"></a>Linux tanılama uzantısı (LAD)
LAD, Azure Depolama'daki tablolara veri yazar. Aşağıdaki tablodaki lavaboları destekler.

| Hedef | Açıklama |
|:---|:---|
| Event Hubs | Azure dışında veri göndermek için Azure Etkinlik Hub'larını kullanın. |
| Azure Depolama lekeleri | Tablolara ek olarak Azure Depolama'daki blob'lara verilere yazın. |
| Azure Monitör Ölçümleri | LAD'e ek olarak Telegraf aracısını yükleyin. Bkz. [InfluxData Telegraf agent ile bir Linux VM için özel ölçümler ilerler.](collect-custom-metrics-linux-telegraf.md)


## <a name="installation-and-configuration"></a>Yükleme ve yapılandırma
Tanılama uzantısı Azure'da [sanal makine uzantısı](../../virtual-machines/extensions/overview.md) olarak uygulandığından, Kaynak Yöneticisi şablonları, PowerShell ve CLI'yi kullanarak aynı yükleme seçeneklerini destekler. Sanal [makine uzantılarının](../../virtual-machines/extensions/features-windows.md) yüklenmesi ve bakımı yla ilgili genel ayrıntılar için Windows [ve Sanal makine uzantıları ve Linux için Sanal makine](../../virtual-machines/extensions/features-linux.md) uzantıları ve özellikleri ne kadar özel liğe bakın.

Ayrıca, sanal makine menüsünün **İzleme** bölümünde tanı **ayarları** altında Azure portalındaki Windows ve Linux tanıuzantısını yükleyebilir ve yapılandırabilirsiniz.

Windows ve Linux için tanılama uzantısını yükleme ve yapılandırma hakkında ayrıntılar için aşağıdaki makalelere bakın.

- [Windows Azure tanılama uzantısını yükleme ve yapılandırma (WAD)](diagnostics-extension-windows-install.md)
- [Ölçümleri ve günlükleri izlemek için Linux Tanılama Uzantısı’nı kullanma](../../virtual-machines/extensions/diagnostics-linux.md)

## <a name="other-documentation"></a>Diğer belgeler

###  <a name="azure-cloud-service-classic-web-and-worker-roles"></a>Azure Bulut Hizmeti (klasik) Web ve Çalışan Rolleri
- [Bulut Hizmeti İzlemeye Giriş](../../cloud-services/cloud-services-how-to-monitor.md)
- [Azure Bulut Hizmetlerinde Azure Tanılama'yı Etkinleştirme](../../cloud-services/cloud-services-dotnet-diagnostics.md)
- [Azure bulut hizmetleri için Uygulama Öngörüleri](../app/cloudservices.md)<br>[Azure Tanılama ile Bulut Hizmetleri uygulamasının akışını izleme](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md) 

### <a name="azure-service-fabric"></a>Azure Service Fabric
- [Yerel makine geliştirme kurulumundaki hizmetleri izleme ve tanılama](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

## <a name="next-steps"></a>Sonraki adımlar


* Azure [Tanılama'da Performans Sayaçları kullanmayı](../../cloud-services/diagnostics-performance-counters.md)öğrenin.
* Azure depolama tablolarında verilerinizi başlatma veya bulma konusunda sorun yaşıyorsanız, [Bkz. Sorun Giderme Azure Tanılama](diagnostics-extension-troubleshooting.md)

