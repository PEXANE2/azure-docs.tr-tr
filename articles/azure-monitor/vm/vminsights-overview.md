---
title: VM öngörüleri nedir?
description: Azure VM 'lerinin sistem durumunu ve performansını izleyen ve uygulama bileşenlerini ve bağımlılıklarını otomatik olarak bulan ve eşleyen VM öngörülerine genel bakış.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/22/2020
ms.openlocfilehash: 967ff9672f55af3e9c5d2e7c3c3dbc7d2ff6cb3c
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101707379"
---
# <a name="overview-of-vm-insights"></a>VM öngörülerine genel bakış

VM öngörüleri, sanal makinelerinizin ve sanal makine ölçek kümelerinin, çalışan işlemleri ve diğer kaynaklardaki bağımlılıklar dahil olmak üzere performansını ve sistem durumunu izler. Performans sorunlarını ve ağ sorunlarını tanımlayarak önemli uygulamaların öngörülebilir performans ve kullanılabilirlik sağlanmasına yardımcı olabilir ve ayrıca bir sorunun diğer bağımlılıklarla ilgili olup olmadığını anlamanıza yardımcı olabilir.

VM öngörüleri aşağıdakiler üzerinde Windows ve Linux işletim sistemlerini destekler:

- Azure sanal makineleri
- Azure sanal makine ölçek kümeleri
- Azure Arc ile bağlantılı karma sanal makineler
- Şirket içi sanal makineler
- Başka bir bulut ortamında barındırılan sanal makineler
  

VM öngörüleri verileri Azure Izleyici günlüklerinde depolar ve bu sayede güçlü toplama ve filtreleme olanağı sunar ve zaman içinde veri eğilimlerini analiz edebilir. Bu verileri doğrudan sanal makineden tek bir VM 'de görüntüleyebilir veya birden çok VM 'nin toplanmış bir görünümünü sunmak için Azure Izleyici 'yi kullanabilirsiniz.

![Azure portal sanal makine öngörüleri perspektifi](media/vminsights-overview/vminsights-azmon-directvm.png)


## <a name="pricing"></a>Fiyatlandırma
VM öngörüleri için doğrudan maliyet yoktur, ancak Log Analytics çalışma alanındaki etkinliği için ücretlendirilirsiniz. [Azure izleyici fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/monitor/)yayınlanan fiyatlandırmaya bağlı olarak, VM öngörüleri için faturalandırılır:

- Aracılardan alınan ve çalışma alanında depolanan veriler.
- Konuk sistem durumundan toplanan sistem durumu verileri (Önizleme)
- Günlük ve sistem durumu verilerine göre uyarı kuralları.
- Uyarı kurallarından gönderilen bildirimler.

Günlük boyutu performans sayaçlarının dize uzunluklarına göre farklılık gösterir ve VM 'ye ayrılan mantıksal disk ve ağ bağdaştırıcısı sayısıyla artabilir. Zaten Hizmet Eşlemesi kullanıyorsanız, göreceğiniz tek değişiklik Azure Izleyici veri türüne gönderilen ek performans vereceğiz `InsightsMetrics` .


## <a name="configuring-vm-insights"></a>VM öngörülerini yapılandırma
VM öngörülerini yapılandırma adımları aşağıdaki gibidir. Her bir adımla ilgili ayrıntılı yönergeler için her bir bağlantıyı izleyin:

- [Log Analytics çalışma alanı oluşturun.](./vminsights-configure-workspace.md#create-log-analytics-workspace)
- [Çalışma alanına Vminsıghts çözümü ekleyin.](./vminsights-configure-workspace.md#add-vminsights-solution-to-workspace)
- [Sanal makineye aracıları ve sanal makine ölçek kümesini izlenecek şekilde kurun.](./vminsights-enable-overview.md)



## <a name="next-steps"></a>Sonraki adımlar

- Bkz. sanal makineleriniz için izlemeyi etkinleştiren gereksinimler ve yöntemler için [VM öngörülerini dağıtma](./vminsights-enable-overview.md) .
