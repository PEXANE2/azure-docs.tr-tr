---
title: Azure izleme aracılarına genel bakış | Microsoft Docs
description: Bu makalede, Azure 'da veya karma ortamda barındırılan sanal makineleri izlemeyi destekleyen Azure aracılarına yönelik ayrıntılı bir genel bakış sunulmaktadır.
services: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/14/2020
ms.openlocfilehash: 75394768cc41d3ca73398c2691b9ec0feb8c17f8
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86202564"
---
# <a name="overview-of-azure-monitor-agents"></a>Azure Izleyici aracılarına genel bakış

Sanal makineler ve diğer işlem kaynakları, Konuk işletim sistemi ve iş yüklerinin performansını ve kullanılabilirliğini ölçmek üzere bir aracının izleme verileri toplamasını gerektirir. Bu makalede, Azure Izleyici tarafından kullanılan aracılar açıklanmakta ve belirli ortamınızın gereksinimlerini karşılamanız gerektiğini belirlemenize yardımcı olur.

> [!NOTE]
> Azure Izleyici 'nin şu anda birden çok Aracısı vardır çünkü Azure Izleyici 'nin son birleştirme ve Log Analytics. Özelliklerinde örtüşme olabilir, ancak her birinin benzersiz özellikleri vardır. Gereksinimlerinize bağlı olarak, sanal makinelerinizdeki bir veya daha fazla aracıya ihtiyacınız olabilir. 

Belirli bir sanal makine için tek bir aracıda tamamen karşılanmayan belirli bir gereksinim kümesine sahip olabilirsiniz. Örneğin, Azure tanılama uzantısı gerektiren ölçüm uyarılarını kullanmak ve ayrıca Log Analytics Aracısı ve bağımlılık Aracısı gerektiren VM'ler için Azure İzleyici işlevlerinden yararlanmak isteyebilirsiniz. Bu gibi durumlarda, birden çok aracı kullanabilirsiniz ve bu, her birinden işlevselliği gerektiren müşteriler için yaygın bir senaryodur.

## <a name="summary-of-agents"></a>Aracıların Özeti

Aşağıdaki tablolarda, Windows ve Linux için Azure Izleyici aracılarıyla ilgili hızlı bir karşılaştırma sağlanmaktadır. Aşağıdaki bölümde, her biri hakkında daha fazla ayrıntı sağlanır. 

### <a name="windows-agents"></a>Windows aracıları

| | Tanılama<br>Uzantı (WAD) | Log Analytics<br>aracı | Bağımlılık<br>aracı |
|:---|:---|:---|:---|
| **Desteklenen ortamlar** | Azure | Azure<br>Diğer bulut<br>Şirket içi | Azure<br>Diğer bulut<br>Şirket içi | 
| **Aracı gereksinimleri**  | Hiçbiri | Hiçbiri | Log Analytics Aracısı gerektirir |
| **Toplanan veriler** | Olay Günlükleri<br>ETW olayları<br>Performans<br>Dosya tabanlı Günlükler<br>IIS günlükleri<br>.NET uygulama günlükleri<br>Kilitlenme bilgi dökümleri<br>Aracı tanılama günlükleri | Olay Günlükleri<br>Performans<IIS logs><br>Dosya tabanlı Günlükler<br>Öngörüler ve çözümler<br>Diğer hizmetler | İşlem ayrıntıları ve bağımlılıklar<br>Ağ bağlantısı ölçümleri |
| **Gönderilen veriler** | Azure Depolama<br>Azure Izleyici ölçümleri<br>Olay Hub'ı | Azure İzleyici Günlükleri | Azure İzleyici Günlükleri |


### <a name="linux-agents"></a>Linux aracıları

| | Tanılama<br>Uzantı (LAD) | Telegraf<br>aracı | Log Analytics<br>aracı | Bağımlılık<br>aracı |
|:---|:---|:---|:---|:---|
| **Desteklenen ortamlar** | Azure | Azure<br>Diğer bulut<br>Şirket içi | Azure<br>Diğer bulut<br>Şirket içi | Azure<br>Diğer bulut<br>Şirket içi |
| **Aracı gereksinimleri**  | Hiçbiri | Hiçbiri | Hiçbiri | Log Analytics Aracısı gerektirir |
| **Toplanan veriler** | Syslog<br>Performans | Performans | Syslog<br>Performans| İşlem ayrıntıları ve bağımlılıklar<br>Ağ bağlantısı ölçümleri |
| **Gönderilen veriler** | Azure Depolama<br>Olay Hub'ı | Azure Izleyici ölçümleri | Azure İzleyici Günlükleri | Azure İzleyici Günlükleri |

## <a name="log-analytics-agent"></a>Log Analytics aracısı

[Log Analytics Aracısı](log-analytics-agent.md) , Azure 'da, diğer bulut sağlayıcılarında ve Şirket içindeki sanal makinelerin Konuk işletim sisteminden ve iş yüklerinden izleme verilerini toplar. Log Analytics çalışma alanına veri toplar. Log Analytics Aracısı System Center Operations Manager tarafından kullanılan aracıdır ve aynı anda yönetim grubunuz ve Azure Izleyiciyle iletişim kurmak için çok giriş aracı bilgisayarları kullanabilirsiniz. Bu aracı, Azure Izleyici 'deki belirli Öngörüler ve çözümler için de gereklidir.


> [!NOTE]
> Windows için Log Analytics Aracısı genellikle Microsoft Monitoring Agent (MMA) olarak adlandırılır. Linux için Log Analytics Aracısı genellikle OMS Aracısı olarak adlandırılır.



Şunları yapmanız gerekirse Log Analytics aracısını kullanın:

* Azure dışındaki sanal veya fiziksel makinelerden gelen günlükleri ve performans verilerini toplayın. 
* [Günlük sorguları](../log-query/log-query-overview.md)gibi [Azure izleyici günlükleri](data-platform-logs.md#what-can-you-do-with-azure-monitor-logs) tarafından desteklenen özelliklerden yararlanmak için bir Log Analytics çalışma alanına veri gönderin.
* Sanal makinelerinizi ölçekli olarak izlemenize ve diğer kaynaklardaki ve dış süreçlerdeki işlem ve bağımlılıklarını izleyicmenize olanak tanıyan [VM'ler için Azure izleyici](../insights/vminsights-overview.md) kullanın.  
* [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md) veya [Azure Sentinel](../../sentinel/overview.md)kullanarak sanal makinelerinizin güvenliğini yönetin.
* Azure sanal makinelerinizin kapsamlı bir şekilde yönetilmesini sağlamak için [Azure Otomasyonu güncelleştirme yönetimi](../../automation/automation-update-management.md), [Azure Otomasyonu durum yapılandırması](../../automation/automation-dsc-overview.md)veya [Azure Otomasyonu değişiklik izleme ve envanterini](../../automation/change-tracking.md) kullanın
* Belirli bir hizmeti veya uygulamayı izlemek için farklı [çözümler](../monitor-reference.md#insights-and-core-solutions) kullanın.

Log Analytics aracısının sınırlamaları şunlardır:

- Azure Izleyici ölçümleri, Azure depolama veya Azure Event Hubs veri gönderilemez.

## <a name="azure-diagnostics-extension"></a>Azure tanılama uzantısı

[Azure tanılama uzantısı](diagnostics-extension-overview.md) , Konuk işletim sisteminden ve Azure sanal makinelerinin ve diğer işlem kaynaklarının iş yüklerinden izleme verilerini toplar. Birincil olarak Azure Storage 'da veri toplar, ancak Azure Izleyici ölçümleri ve Azure Event Hubs gibi diğer hedeflere veri göndermek için de veri havuzları tanımlamanızı sağlar.

Şunları yapmanız gerekirse Azure tanılama uzantısı 'nı kullanın:

- Arşivleme için verileri Azure depolama 'ya gönderin veya [Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md)gibi araçlarla çözümleyin.
- Verileri, [Ölçüm Gezgini](metrics-getting-started.md) ile analiz etmek ve neredeyse gerçek zamanlı [ölçüm uyarıları](../../azure-monitor/platform/alerts-metric-overview.md) ve [Otomatik ölçeklendirme](autoscale-overview.md) (yalnızca Windows) gibi özelliklerden yararlanmak için [Azure izleyici ölçümlerine](data-platform-metrics.md) veri gönderme.
- [Azure Event Hubs](diagnostics-extension-stream-event-hubs.md)kullanarak üçüncü taraf araçlara veri gönderme.
- VM önyükleme sorunlarını araştırmak için [önyükleme tanılamayı](../../virtual-machines/troubleshooting/boot-diagnostics.md) toplayın.

Azure tanılama uzantısı sınırlamaları şunlardır:

- Yalnızca Azure kaynaklarıyla kullanılabilir.
- Azure Izleyici günlüklerine veri gönderme yeteneği sınırlıdır.

## <a name="telegraf-agent"></a>Telegraf Aracısı

[Etkileyen Azure Data telegraf Aracısı](collect-custom-metrics-linux-telegraf.md) , Linux bilgisayarlardan Azure Izleyici ölçümlerine performans verileri toplamak için kullanılır.

Şunları yapmanız gerekirse telegraf aracısını kullanın:

* Verileri, [Ölçüm Gezgini](metrics-getting-started.md) ile analiz etmek ve neredeyse gerçek zamanlı [ölçüm uyarıları](../../azure-monitor/platform/alerts-metric-overview.md) ve [Otomatik ölçeklendirme](autoscale-overview.md) (yalnızca Linux) gibi özelliklerden yararlanmak için [Azure izleyici ölçümlerine](data-platform-metrics.md) veri gönderme. 



## <a name="dependency-agent"></a>Bağımlılık aracısı

Bağımlılık Aracısı, sanal makine ve dış işlem bağımlılıkları üzerinde çalışan işlemler hakkında bulunan verileri toplar. 

Şunları yapmanız gerekirse bağımlılık aracısını kullanın:

* Eşleme özelliği [VM'ler için Azure izleyici](../insights/vminsights-overview.md) veya [hizmet eşlemesi](../insights/service-map.md) çözümünü kullanın.

Bağımlılık aracısını kullanırken aşağıdakileri göz önünde bulundurun:

- Bağımlılık Aracısı, Log Analytics aracısının aynı sanal makinede yüklü olmasını gerektirir.
- Linux VM 'lerinde, Azure tanılama uzantısından önce Log Analytics aracısının yüklenmesi gerekir.

## <a name="extensions-compared-to-agents"></a>Aracılarla karşılaştırılan uzantılar

[Windows](../../virtual-machines/extensions/oms-windows.md) ve [Linux](../../virtual-machines/extensions/oms-linux.md) Için Log Analytics uzantısı, Azure sanal makinelerine Log Analytics Aracısı 'nı yükler. [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) ve [Linux](../../virtual-machines/extensions/agent-dependency-linux.md) için Azure Izleyici bağımlılığı uzantısı, Azure sanal makinelerine bağımlılık aracısını yükler. Bunlar yukarıda açıklanan aracılardır, ancak bunları [sanal makine uzantıları](../../virtual-machines/extensions/overview.md)aracılığıyla yönetmenizi sağlar. Mümkün olan her durumda aracıları yüklemek ve yönetmek için uzantıları kullanmanız gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Aracılardan her biri hakkında daha fazla ayrıntı için aşağıdaki adresten yararlanın:

- [Log Analytics aracısına genel bakış](log-analytics-agent.md)
- [Azure Tanılama uzantıya genel bakış](diagnostics-extension-overview.md)
- [Etkileyen bir Linux VM için özel ölçümler toplama telegraf Aracısı](collect-custom-metrics-linux-telegraf.md)
