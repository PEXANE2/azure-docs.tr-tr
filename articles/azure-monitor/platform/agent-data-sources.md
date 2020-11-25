---
title: Azure Izleyici 'de aracı veri kaynaklarını Log Analytics
description: Veri kaynakları, Azure Izleyici 'nin aracılardan ve diğer bağlı kaynaklardan topladığı günlük verilerini tanımlar.  Bu makalede, Azure Izleyici 'nin veri kaynaklarını nasıl kullandığı kavramı açıklanmakta, nasıl yapılandırılacağı hakkında ayrıntılı bilgi verilmektedir ve kullanılabilir farklı veri kaynaklarının bir özeti sağlanır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/21/2020
ms.openlocfilehash: ad5e91a6dcdb61e09a64e61a27f12148ec28168e
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96000751"
---
# <a name="log-analytics-agent-data-sources-in-azure-monitor"></a>Azure Izleyici 'de aracı veri kaynaklarını Log Analytics
Azure Izleyicisinin [Log Analytics](log-analytics-agent.md) aracısına sahip sanal makinelerden topladığı veriler, [Log Analytics çalışma alanında](data-platform-logs.md)yapılandırdığınız veri kaynakları tarafından tanımlanır.   Her veri kaynağı, her tür kendi özellik kümesine sahip olan belirli bir türün kayıtlarını oluşturur.

> [!IMPORTANT]
> Bu makalede, Azure Izleyici tarafından kullanılan aracılardan biri olan [Log Analytics aracısına](log-analytics-agent.md) ait veri kaynakları ele alınmaktadır. Diğer aracılar farklı veriler toplar ve farklı şekilde yapılandırılır. Kullanılabilir aracıların ve toplayabilecekleri verilerin bir listesi için bkz. [Azure izleyici aracılarına genel bakış](agents-overview.md) .

![Günlük verileri toplama](media/agent-data-sources/overview.png)

> [!IMPORTANT]
> Bu makalede açıklanan veri kaynakları yalnızca Log Analytics aracısını çalıştıran sanal makineler için geçerlidir. 

## <a name="summary-of-data-sources"></a>Veri kaynaklarının Özeti
Aşağıdaki tabloda, Log Analytics aracısında Şu anda kullanılabilir olan aracı veri kaynakları listelenmektedir.  Her birinin, bu veri kaynağı için ayrıntı sağlayan ayrı bir makaleye bağlantısı vardır.   Ayrıca, kendi yöntemi ve koleksiyon sıklığı hakkında bilgi de sağlar. 


| Veri kaynağı | Platform | Log Analytics Aracısı | Operations Manager aracısı | Azure depolama | Operations Manager gerekli mi? | Yönetim grubu aracılığıyla gönderilen aracı verileri Operations Manager | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Özel günlükler](data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | varış noktasında |
| [Özel günlükler](data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | varış noktasında |
| [IIS günlükleri](data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |Günlük dosyası geçiş ayarına bağlıdır |
| [Performans sayaçları](data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |zamanlandığı gibi, en az 10 saniye |
| [Performans sayaçları](data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |zamanlandığı gibi, en az 10 saniye |
| [Syslog](data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |Azure depolama 'dan: 10 dakika; aracıda: gelişde |
| [Windows olay günlükleri](data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | varış noktasında |


## <a name="configuring-data-sources"></a>Veri kaynaklarını yapılandırma
Log Analytics aracılarının veri kaynaklarını yapılandırmak için Azure portal **Log Analytics çalışma alanları** menüsüne gidin ve bir çalışma alanı seçin. **Gelişmiş ayarlar** ' a ve ardından **veriler**' e tıklayın. Yapılandırmak istediğiniz veri kaynağını seçin. Yukarıdaki tablodaki bağlantıları, her veri kaynağı ve yapılandırmasındaki ayrıntılar hakkında belgeler için izleyebilirsiniz.

Herhangi bir yapılandırma bu çalışma alanına bağlı tüm aracılara dağıtılır.  Bağlı aracıları bu yapılandırmadan dışlayamazsınız.

![Windows olaylarını yapılandırma](media/agent-data-sources/configure-events.png)



## <a name="data-collection"></a>Veri toplama
Veri kaynağı konfigürasyonları, birkaç dakika içinde Azure Izleyici 'ye doğrudan bağlı aracılara dağıtılır.  Belirtilen veriler aracıdan toplanır ve her bir veri kaynağına özel aralıklarla doğrudan Azure Izleyici 'ye dağıtılır.  Bu bilgiler için her bir veri kaynağına yönelik belgelere bakın.

Bağlı bir yönetim grubundaki System Center Operations Manager aracıları için, veri kaynağı konfigürasyonları yönetim paketlerine çevrilir ve varsayılan olarak her 5 dakikada bir yönetim grubuna dağıtılır.  Aracı yönetim paketini diğer gibi indirir ve belirtilen verileri toplar. Veri kaynağına bağlı olarak, veriler, verileri Azure Izleyicisine ileten bir yönetim sunucusuna gönderilir veya aracı, yönetim sunucusundan geçmeden verileri Azure Izleyici 'ye gönderir. Ayrıntılar için bkz. [Azure 'da çözümleri izlemek Için veri toplama ayrıntıları](../monitor-reference.md) .  Operations Manager ve Azure Izleyicisini bağlama hakkında bilgi edinebilir ve yapılandırmanın yapılandırma [System Center Operations Manager Ile tümleştirilmesine](om-agents.md)göre değişiklik sıklığını değiştirebilirsiniz.

Aracı Azure Izleyici 'ye veya Operations Manager bağlanamazsa, bir bağlantı kurduğunda teslim edileceği verileri toplamaya devam edecektir.  Veri miktarı, istemcinin en büyük önbellek boyutuna ulaşırsa veya aracı 24 saat içinde bir bağlantı kuramazsa, veriler kaybolabilir.

## <a name="log-records"></a>Günlük kayıtları
Azure Izleyici tarafından toplanan tüm günlük verileri çalışma alanında kayıt olarak depolanır.  Farklı veri kaynakları tarafından toplanan kayıtlar kendi özellik kümesine sahip olur ve bunların **tür** özelliklerine göre tanımlanır.  Her bir kayıt türü hakkındaki ayrıntılar için her bir veri kaynağına ve çözüme yönelik belgelere bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Izleyici 'ye işlevsellik ekleyen ve ayrıca çalışma alanına veri toplayacağınız [çözümleri izleme](../insights/solutions.md) hakkında bilgi edinin.
* Veri kaynaklarından ve izleme çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.  
* Veri kaynaklarından ve izleme çözümlerinden toplanan kritik verileri proaktif olarak bildirmek için [uyarıları](alerts-overview.md) yapılandırın.
