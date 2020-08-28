---
title: VM'ler için Azure İzleyici nedir?
description: Azure VM 'lerinin sistem durumunu ve performansını izleyen VM'ler için Azure İzleyici genel bakış ve uygulama bileşenlerini ve bunların bağımlılıklarını otomatik olarak bulur ve eşleştirir.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/22/2020
ms.openlocfilehash: 5c3cb13d0b2da9370f402083d82397679f2c9343
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89022505"
---
# <a name="what-is-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici nedir?

VM'ler için Azure İzleyici, sanal makinelerinizin ve sanal makine ölçek kümelerinin, çalışan işlemleri ve diğer kaynaklardaki bağımlılıklar dahil olmak üzere performansını ve sistem durumunu izler. Performans sorunlarını ve ağ sorunlarını tanımlayarak önemli uygulamaların öngörülebilir performans ve kullanılabilirlik sağlanmasına yardımcı olabilir ve ayrıca bir sorunun diğer bağımlılıklarla ilgili olup olmadığını anlamanıza yardımcı olabilir.

VM'ler için Azure İzleyici, Windows ve Linux işletim sistemlerini aşağıdakiler için destekler:

- Azure sanal makineleri
- Azure sanal makine ölçek kümeleri
- Azure Arc ile bağlantılı karma sanal makineler
- Şirket içi sanal makineler
- Başka bir bulut ortamında barındırılan sanal makineler
  



VM'ler için Azure İzleyici, verileri Azure Izleyici günlüklerinde depolar ve bu sayede güçlü toplama ve filtreleme olanağı sunar ve zaman içinde veri eğilimlerini analiz edebilir. Bu verileri doğrudan sanal makineden tek bir VM 'de görüntüleyebilir veya birden çok VM 'nin toplanmış bir görünümünü sunmak için Azure Izleyici 'yi kullanabilirsiniz.

![Azure portal sanal makine öngörüleri perspektifi](media/vminsights-overview/vminsights-azmon-directvm.png)


## <a name="pricing"></a>Fiyatlandırma
VM'ler için Azure İzleyici için doğrudan maliyet yoktur, ancak Log Analytics çalışma alanındaki etkinliği için ücretlendirilirsiniz. [Azure izleyici fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/monitor/)yayınlanan fiyatlandırmaya göre, VM'ler için Azure izleyici için faturalandırılır:

- Aracılardan alınan ve çalışma alanında depolanan veriler.
- Günlük ve sistem durumu verilerine göre uyarı kuralları.
- Uyarı kurallarından gönderilen bildirimler.

Günlük boyutu performans sayaçlarının dize uzunluklarına göre farklılık gösterir ve VM 'ye ayrılan mantıksal disk ve ağ bağdaştırıcısı sayısıyla artabilir. Zaten Hizmet Eşlemesi kullanıyorsanız, göreceğiniz tek değişiklik Azure Izleyici veri türüne gönderilen ek performans vereceğiz `InsightsMetrics` .


## <a name="configuring-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici yapılandırma
VM'ler için Azure İzleyici yapılandırma adımları aşağıdaki gibidir. Her bir adımla ilgili ayrıntılı yönergeler için her bir bağlantıyı izleyin:

- [Log Analytics çalışma alanı oluşturun.](vminsights-configure-workspace.md#create-log-analytics-workspace)
- [Çalışma alanına Vminsıghts çözümü ekleyin.](vminsights-configure-workspace.md#add-vminsights-solution-to-workspace)
- [Sanal makineye aracıları ve sanal makine ölçek kümesini izlenecek şekilde kurun.](vminsights-enable-overview.md)



## <a name="next-steps"></a>Sonraki adımlar

- Sanal makineleriniz için izlemeyi etkinleştiren gereksinimler ve yöntemler için bkz. [dağıtım VM'ler için Azure izleyici](vminsights-enable-overview.md) .

