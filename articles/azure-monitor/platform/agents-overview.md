---
title: Azure izleme aracıları genel bakış| Microsoft Dokümanlar
description: Bu makalede, Azure veya karma ortamda barındırılan sanal makinelerin izlenmesini destekleyen kullanılabilir Azure aracılarına ayrıntılı bir genel bakış sunulmaktadır.
services: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/14/2020
ms.openlocfilehash: a7e6a3a299df8112fe4fbcf457516894c1766b8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249093"
---
# <a name="overview-of-azure-monitor-agents"></a>Azure Monitör ütemeleri için genel bakış

Sanal makineler ve diğer bilgi işlem kaynakları, konuk işletim sisteminin ve iş yüklerinin performansını ve kullanılabilirliğini ölçmek için izleme verilerini toplamak için bir aracı gerektirir. Bu makalede, Azure Monitor tarafından kullanılan aracılar açıklanır ve belirli ortamınız için gereksinimleri karşılamak için hangisinin olması gerektiğini belirlemenize yardımcı olur.

> [!NOTE]
> Azure Monitor'un şu anda Azure Monitörü ve Log Analytics'in yakın zamanda konsolidasyonu nedeniyle birden çok aracısı bulunuyor. Özelliklerinde çakışma olsa da, her birinin benzersiz yetenekleri vardır. Gereksinimlerinize bağlı olarak, sanal makinelerinizdeki aracılardan bir veya daha fazlasına ihtiyacınız olabilir. 

Belirli bir sanal makine için tek bir aracıyla tamamen karşılanamayan belirli bir gereksinim ler kümesiniz olabilir. Örneğin, Azure tanılama uzantısı gerektiren metrik uyarıları kullanmak, ancak Aynı zamanda Log Analytics aracısı ve Bağımlılık aracısı gerektiren VM'ler için Azure Monitor işlevselliğinden yararlanmak isteyebilirsiniz. Bu gibi durumlarda, birden çok aracı kullanabilirsiniz ve bu her işlevsellik gerektiren müşteriler için yaygın bir senaryodur.

## <a name="summary-of-agents"></a>Aracıların özeti

Aşağıdaki tablolar, Windows ve Linux için Azure Monitor aracılarının hızlı bir karşılaştırmasını sağlar. Her biri hakkında daha fazla ayrıntı aşağıdaki bölümde verilmiştir. 

### <a name="windows-agents"></a>Windows aracıları

| | Tanılama<br>uzantısı (WAD) | Log Analytics<br>Aracısı | Bağımlılık<br>Aracısı |
|:---|:---|:---|:---|
| Desteklenen ortamlar | Azure | Azure<br>Diğer bulut<br>Şirket içi | Azure<br>Diğer bulut<br>Şirket içi | 
| Aracı gereksinimleri  | None | None | Log Analytics aracısı gerektirir |
| Toplanan veriler | Olay Günlükleri<br>ETW olayları<br>Performans<br>Dosya tabanlı günlükler<br>IIS günlükleri<br>.NET uygulama günlükleri<br>Kilitlenme bilgi dökümleri<br>Aracı tanılama günlükleri | Olay Günlükleri<br>Performans<IIS logs><br>Dosya tabanlı günlükler<br>Öngörüler ve çözümler<br>Diğer hizmetler | İşlem ayrıntıları ve bağımlılıklar<br>Ağ bağlantısı ölçümleri |
| Gönderilen veriler | Azure Storage<br>Azure Monitör Ölçümleri<br>Olay Hub'ı | Azure İzleyici Günlükleri | Azure İzleyici Günlükleri |


### <a name="linux-agents"></a>Linux aracıları

| | Tanılama<br>uzantısı (LAD) | Telegraf<br>Aracısı | Log Analytics<br>Aracısı | Bağımlılık<br>Aracısı |
|:---|:---|:---|:---|:---|
| Desteklenen ortamlar | Azure | Azure<br>Diğer bulut<br>Şirket içi | Azure<br>Diğer bulut<br>Şirket içi | Azure<br>Diğer bulut<br>Şirket içi |
| Aracı gereksinimleri  | None | None | None | Log Analytics aracısı gerektirir |
| Toplanan veriler | Syslog<br>Performans | Performans | Syslog<br>Performans| İşlem ayrıntıları ve bağımlılıklar<br>Ağ bağlantısı ölçümleri |
| Gönderilen veriler | Azure Storage<br>Olay Hub'ı | Azure Monitör Ölçümleri | Azure İzleyici Günlükleri | Azure İzleyici Günlükleri |

## <a name="log-analytics-agent"></a>Log Analytics aracısı

[Log Analytics aracısı,](log-analytics-agent.md) Azure'daki, diğer bulut sağlayıcılarındaki ve şirket içi sanal makinelerin konuk işletim sisteminden izleme verilerini ve iş yüklerini toplar. Bir Log Analytics çalışma alanına veri toplar. Log Analytics aracısı, System Center Operations Manager tarafından kullanılan aracıyla aynıdır ve yönetim grubunuz ve Azure Monitor ile aynı anda iletişim kurmak için çok ev sahibi aracı bilgisayarlarda kullanılabilir. Bu aracı, Azure Monitor'daki belirli öngörüler ve çözümler tarafından da gereklidir.


> [!NOTE]
> Windows için Log Analytics aracısı genellikle Microsoft İzleme Aracısı (MMA) olarak adlandırılır. Linux için Log Analytics aracısı genellikle OMS aracısı olarak adlandırılır.



Aşağıdakiler için log Analytics aracısını kullanın:

* Azure dışındaki sanal veya fiziksel makinelerden günlükleri ve performans verileri toplayın. 
* Azure [Monitör Günlükleri](data-platform-logs.md#what-can-you-do-with-azure-monitor-logs) tarafından desteklenen günlük [sorguları](../log-query/log-query-overview.md)gibi özelliklerden yararlanmak için Günlük Analizi çalışma alanına veri gönderin.
* Sanal makinelerinizi ölçekte izlemenize ve süreçlerini ve diğer kaynaklara ve dış işlemlere olan bağımlılıklarını izlemenize olanak tanıyan [Sanal Makineler için Azure Monitörünü](../insights/vminsights-overview.md) kullanın...  
* [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md) veya [Azure Sentinel'i](../../sentinel/overview.md)kullanarak sanal makinelerinizin güvenliğini yönetin.
* Azure VM'lerinizin kapsamlı yönetimini sağlamak için [Azure Otomasyon Güncelleme yönetimini,](../../automation/automation-update-management.md) [Azure Otomasyon Durumu Yapılandırmasını](../../automation/automation-dsc-overview.md)veya [Azure Otomasyon Değişikliği İzleme ve Envanterini](../../automation/change-tracking.md) kullanın
* Belirli bir hizmeti veya uygulamayı izlemek için farklı [çözümler](../monitor-reference.md#insights-and-core-solutions) kullanın.

Log Analytics aracısının sınırlamaları şunlardır:

- Azure Monitör Ölçümleri, Azure Depolama veya Azure Etkinlik Hub'larına veri gönderemezsiniz.

## <a name="azure-diagnostics-extension"></a>Azure tanılama uzantısı

[Azure Tanılama uzantısı,](diagnostics-extension-overview.md) konuk işletim sisteminden izleme verileri ve Azure sanal makinelerin ve diğer bilgi işlem kaynaklarının iş yüklerini toplar. Öncelikle Azure Depolama'ya veri toplar, ancak Azure Monitör Ölçümleri ve Azure Etkinlik Hub'ları gibi diğer hedeflere de veri göndermek için veri lavabolarını tanımlamanıza olanak tanır.

Aşağıdakiler için gerekirse Azure tanılama uzantısını kullanın:

- Verileri arşivlemek veya [Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md)gibi araçlarla çözümlemek için Azure Depolama'ya gönderin.
- Verileri azure monitör üttülleriyle [metrics explorer](metrics-getting-started.md) analiz etmek ve neredeyse gerçek zamanlı metrik [uyarıları](../../azure-monitor/platform/alerts-metric-overview.md) ve [otomatik ölçeklendirme](autoscale-overview.md) (yalnızca Windows) gibi özelliklerden yararlanmak için [Azure Monitor Ölçümleri'ne](data-platform-metrics.md) gönderin.
- [Azure Etkinlik Hub'larını](diagnostics-extension-stream-event-hubs.md)kullanarak üçüncü taraf araçlarına veri gönderin.
- VM önyükleme sorunlarını araştırmak için [Önyükleme Tanılama'yı](../../virtual-machines/troubleshooting/boot-diagnostics.md) toplayın.

Azure tanılama uzantısı sınırlamaları şunlardır:

- Yalnızca Azure kaynaklarıyla kullanılabilir.
- Azure Monitör Günlükleri'ne veri gönderme yeteneği sınırlıdır.

## <a name="telegraf-agent"></a>Telegraf ajan

[InfluxData Telegraf aracısı,](collect-custom-metrics-linux-telegraf.md) Linux bilgisayarlarından Azure Monitör Ölçümleri'ne performans verileri toplamak için kullanılır.

Gerekirse Telegraf ajan Kullanın:

* Verileri Azure [Monitor Ölçümleri'ne](data-platform-metrics.md) [metrikler ile](metrics-getting-started.md) analiz etmek ve neredeyse gerçek zamanlı metrik [uyarıları](../../azure-monitor/platform/alerts-metric-overview.md) ve [otomatik ölçeklendirme](autoscale-overview.md) (yalnızca Linux) gibi özelliklerden yararlanmak için gönderin. 



## <a name="dependency-agent"></a>Bağımlılık aracısı

Bağımlılık aracısı, sanal makinede çalışan işlemler ve dış işlem bağımlılıkları hakkında keşfedilen verileri toplar. 

Aşağıdakileri yapmanız gerekiyorsa Bağımlılık aracısını kullanın:

* [VM'ler için](../insights/vminsights-overview.md) Harita özelliği Azure Monitörünü veya [Hizmet Haritası](../insights/service-map.md) çözümünü kullanın.

Bağımlılık aracısını kullanırken aşağıdakileri göz önünde bulundurun:

- Bağımlılık aracısı, Log Analytics aracısının aynı sanal makineye yüklenmesini gerektirir.
- Linux VM'lerinde, Log Analytics aracısının Azure Tanı uzantısından önce yüklenmesi gerekir.

## <a name="extensions-compared-to-agents"></a>Aracılara göre uzantılar

[Windows](../../virtual-machines/extensions/oms-windows.md) ve [Linux](../../virtual-machines/extensions/oms-linux.md) için Log Analytics uzantısı, Log Analytics aracısını Azure sanal makinelerine yükler. [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) ve [Linux](../../virtual-machines/extensions/agent-dependency-linux.md) için Azure Monitör Bağımlılığı uzantısı Bağımlılık aracısını Azure sanal makinelerine yükler. Bunlar yukarıda açıklanan aynı [aracılar](../../virtual-machines/extensions/overview.md)ama sanal makine uzantıları ile bunları yönetmek için izin verir. Aracıları mümkün olduğunca yüklemek ve yönetmek için uzantıları kullanmalısınız.


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki ajanlar her biri hakkında daha fazla bilgi alın:

- [Log Analytics acentesine genel bakış](log-analytics-agent.md)
- [Azure Tanılama uzantısına genel bakış](diagnostics-extension-overview.md)
- [InfluxData Telegraf aracısı ile bir Linux VM için özel ölçümler toplayın](collect-custom-metrics-linux-telegraf.md)
