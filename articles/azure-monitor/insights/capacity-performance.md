---
title: Azure Monitör'de Kapasite ve Performans çözümü | Microsoft Dokümanlar
description: Hyper-V sunucularınızın kapasitesini anlamanıza yardımcı olmak için Monitör'deki Kapasite ve Performans çözümünü kullanın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/13/2017
ms.openlocfilehash: 75c65cf9f76e711a3aeed764de8b92ed619bad2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77666952"
---
# <a name="plan-hyper-v-virtual-machine-capacity-with-the-capacity-and-performance-solution-deprecated"></a>Kapasite ve Performans çözümü ile Hyper-V sanal makine kapasitesini planlayın (amortismana uğradı)

![Kapasite ve Performans sembolü](./media/capacity-performance/capacity-solution.png)

> [!NOTE]
> Kapasite ve Performans çözümü amortismana kaldırıldı.  Çözümü zaten yüklemiş olan müşteriler bu çözümü kullanmaya devam edebilir, ancak Kapasite ve Performans yeni çalışma alanlarına eklenemez.

Hyper-V sunucularınızın kapasitesini anlamanıza yardımcı olmak için Monitör'deki Kapasite ve Performans çözümünü kullanabilirsiniz. Çözüm, ana bilgisayarların ve bu Hyper-V ana bilgisayarlarında çalışan VM'lerin genel kullanımını (CPU, bellek ve disk) göstererek Hyper-V ortamınıza ilişkin öngörüler sağlar. Ölçümler, tüm ana bilgisayarlarınızda ve üzerinde çalışan VM'lerde CPU, bellek ve diskler için toplanır.

Çözüm:

-   En yüksek ve en düşük CPU ve bellek kullanımı ile ana bilgisayarları gösterir
-   En yüksek ve en düşük CPU ve bellek kullanımı ile VM'leri gösterir
-   En yüksek ve en düşük IOPS ve iş verme kullanımı yla VM'leri gösterir
-   Hangi VM'lerin hangi ana bilgisayarlarda çalıştığını gösterir
-   Küme paylaşılan birimlerinde yüksek iş hacmi, IOPS ve gecikme li üst diskleri gösterir
- Gruplara göre özelleştirmenizi ve filtrelemenizi sağlar

> [!NOTE]
> Kapasite yönetimi adı verilen Kapasite ve Performans çözümünün önceki sürümünde hem Sistem Merkezi Operasyon Yöneticisi hem de Sistem Merkezi Sanal Makine Yöneticisi gereklidir. Bu güncelleştirilmiş çözüm bu bağımlılıkları yok.


## <a name="connected-sources"></a>Bağlı kaynaklar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Destek | Açıklama |
|---|---|---|
| [Windows aracıları](../../azure-monitor/platform/agent-windows.md) | Evet | Çözüm, Windows aracılarından kapasite ve performans verisi bilgileri toplar. |
| [Linux aracıları](../../azure-monitor/learn/quick-collect-linux-computer.md) | Hayır    | Çözüm, doğrudan Linux ajanlarından kapasite ve performans verisi bilgileri toplamaz.|
| [SCOM yönetim grubu](../../azure-monitor/platform/om-agents.md) | Evet |Çözüm, bağlı bir SCOM yönetim grubundaki aracılardan kapasite ve performans verileri toplar. SCOM temsilcisinden Log Analytics'e doğrudan bağlantı gerekmez.|
| [Azure depolama hesabı](../../azure-monitor/platform/collect-azure-metrics-logs.md) | Hayır | Azure depolama kapasite ve performans verilerini içermez.|

## <a name="prerequisites"></a>Ön koşullar

- Windows veya Operations Manager aracıları sanal makinelere değil, Windows Server 2012 veya daha yüksek Hyper-V ana bilgisayarlarına yüklenmelidir.


## <a name="configuration"></a>Yapılandırma

Çalışma alanınıza Kapasite ve Performans çözümlerini eklemek için aşağıdaki adımı gerçekleştirin.

- [Solutions Gallery'den Log Analytics ekle çözümlerinde](../../azure-monitor/insights/solutions.md)açıklanan işlemi kullanarak Log Analytics çalışma alanınıza Kapasite ve Performans çözümlerini ekleyin.

## <a name="management-packs"></a>Yönetim paketleri

SCOM yönetim grubunuz Log Analytics çalışma alanınıza bağlıysa, bu çözümü eklediğinizde aşağıdaki yönetim paketleri SCOM'a yüklenir. Bu yönetim paketleri için bir yapılandırma veya bakım gerekmez.

- Microsoft.IntelligencePacks.CapacityPerformance

1201 olay benzer:


```
New Management Pack with id:"Microsoft.IntelligencePacks.CapacityPerformance", version:"1.10.3190.0" received.
```

Kapasite ve Performans çözümü güncelleştirildiğinde, sürüm numarası değişecektir.

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../../azure-monitor/platform/om-agents.md).

## <a name="using-the-solution"></a>Çözümü kullanma

Çalışma alanınıza Kapasite ve Performans çözümlerini eklediğinizde, Kapasite ve Performans Genel Bakış panosuna eklenir. Bu döşeme, şu anda etkin olan Hyper-V ana bilgisayarlarının sayısının ve seçilen süre boyunca izlenen etkin sanal makine sayısının sayısını görüntüler.

![Kapasite ve Performans döşemesi](./media/capacity-performance/capacity-tile.png)


### <a name="review-utilization"></a>Kullanımı gözden geçirme

Kapasite ve Performans panosunu açmak için Kapasite ve Performans döşemesine tıklayın. Pano aşağıdaki tabloda gösterilen sütunları içerir. Her sütun, sütunun belirtilen kapsam ve zaman aralığına yönelik kriterleriyle eşleşen en fazla on öğe listeler. Sütunun altındaki **Tümünü gör**’e tıklayarak veya sütun başlığına tıklayarak tüm kayıtları döndüren bir günlük araması gerçekleştirebilirsiniz.

- **Hosts**
    - **Ana Bilgisayar CPU Kullanımı** Seçilen zaman dönemine bağlı olarak, ana bilgisayarların CPU kullanımının grafik eğilimini ve ana bilgisayarların listesini gösterir. Zaman içinde belirli bir noktanın ayrıntılarını görüntülemek için çizgi grafiğinin üzerine takın. Günlük aramasında daha fazla ayrıntı görüntülemek için grafiği tıklatın. Günlük aramasını açmak ve barındırılan VM'ler için CPU sayacı ayrıntılarını görüntülemek için herhangi bir ana bilgisayar adını tıklatın.
    - **Ana Bilgisayar Bellek Kullanımı** Seçilen zaman dönemine bağlı olarak ana bilgisayarların bellek kullanımının grafiksel eğilimini ve ana bilgisayarların listesini gösterir. Zaman içinde belirli bir noktanın ayrıntılarını görüntülemek için çizgi grafiğinin üzerine takın. Günlük aramasında daha fazla ayrıntı görüntülemek için grafiği tıklatın. Barındırılan VM'ler için günlük aramasını açmak ve bellek sayacı ayrıntılarını görüntülemek için herhangi bir ana bilgisayar adını tıklatın.
- **Virtual Machines**
    - **VM CPU Kullanımı** Seçilen zaman dönemine bağlı olarak sanal makinelerin CPU kullanımının grafiksel eğilimini ve sanal makinelerin listesini gösterir. En iyi 3 VM'nin belirli bir zaman diliminin ayrıntılarını görüntülemek için çizgi grafiğinin üzerine takın. Günlük aramasında daha fazla ayrıntı görüntülemek için grafiği tıklatın. Günlük aramasını açmak ve VM için toplu CPU sayacı ayrıntılarını görüntülemek için herhangi bir VM adını tıklatın.
    - **VM Bellek Kullanımı** Seçilen zaman dönemine bağlı olarak sanal makinelerin bellek kullanımının grafiksel eğilimini ve sanal makinelerin listesini gösterir. En iyi 3 VM'nin belirli bir zaman diliminin ayrıntılarını görüntülemek için çizgi grafiğinin üzerine takın. Günlük aramasında daha fazla ayrıntı görüntülemek için grafiği tıklatın. Günlük aramasını açmak ve VM için toplu bellek sayacı ayrıntılarını görüntülemek için herhangi bir VM adını tıklatın.
    - **VM Toplam Disk IOPS** Sanal makineler için toplam disk IOPS'nin grafik eğilimini ve seçilen zaman dilimini temel alan her biri için IOPS'ye sahip sanal makinelerin listesini gösterir. En iyi 3 VM'nin belirli bir zaman diliminin ayrıntılarını görüntülemek için çizgi grafiğinin üzerine takın. Günlük aramasında daha fazla ayrıntı görüntülemek için grafiği tıklatın. Günlük aramasını açmak ve VM için toplu disk IOPS sayacı ayrıntılarını görüntülemek için herhangi bir VM adını tıklatın.
    - **VM Toplam Disk İşlemi** Seçilen zaman dönemine bağlı olarak, sanal makineler için toplam disk iş buzunun grafik eğilimini ve her biri için toplam disk iş buzuna sahip sanal makinelerin listesini gösterir. En iyi 3 VM'nin belirli bir zaman diliminin ayrıntılarını görüntülemek için çizgi grafiğinin üzerine takın. Günlük aramasında daha fazla ayrıntı görüntülemek için grafiği tıklatın. Günlük aramasını açmak ve VM için toplanan toplam disk iş çıktısı sayacı ayrıntılarını görüntülemek için herhangi bir VM adını tıklatın.
- **Kümelenmiş Paylaşılan Birimler**
    - **Toplam İş Mesuliyonu** Kümelenmiş paylaşılan birimlerde hem okuma hem de yazmanın toplamını gösterir.
    - **Toplam IOPS** Kümelenmiş paylaşılan birimlerde saniyede giriş/çıktı işlemlerinin toplamını gösterir.
    - **Toplam Gecikme** Kümelenmiş paylaşılan birimlerdeki toplam gecikme yitirme yi gösterir.
- **Konak Yoğunluğu** Üst döşeme, çözümün kullanabileceği toplam ana bilgisayar ve sanal makine sayısını gösterir. Günlük aramasında ek ayrıntıları görüntülemek için üstteki döşemeyi tıklatın. Ayrıca, tüm ana bilgisayarları ve barındırılan sanal makinelerin sayısını listeler. Günlük aramasında VM sonuçlarını detaylandırmak için bir ana bilgisayara tıklayın.


![pano Hosts bıçak](./media/capacity-performance/dashboard-hosts.png)

![pano sanal makineler bıçak](./media/capacity-performance/dashboard-vms.png)


### <a name="evaluate-performance"></a>Performansı değerlendirme

Üretim bilgi işlem ortamları bir kuruluştan diğerine büyük farklılıklar gösterir. Ayrıca, kapasite ve performans iş yükleri, VM'lerinizin nasıl çalıştığını ve neyi normal olarak gördüğünüze bağlı olabilir. Performansı ölçmenize yardımcı olacak belirli yordamlar büyük olasılıkla ortamınız için geçerli olmayacaktır. Yani, daha genelleştirilmiş açıklayıcı rehberlik daha iyi yardımcı olmak için uygundur. Microsoft, performansı ölçmenize yardımcı olmak için çeşitli açıklayıcı kılavuz makaleleri yayımlar.

Özetlemek gerekirse, çözüm performans sayaçları da dahil olmak üzere çeşitli kaynaklardan kapasite ve performans verileri toplar. Çözümde çeşitli yüzeylerde sunulan bu kapasite ve performans verilerini kullanın ve sonuçlarınızı [Hyper-V](https://msdn.microsoft.com/library/cc768535.aspx) makalesinde Ölçüm Performansı ile karşılaştırın. Makale bir süre önce yayınlanmış olsa da, ölçümler, değerlendirmeler ve yönergeler hala geçerlidir. Makale, diğer yararlı kaynaklara bağlantılar içerir.


## <a name="sample-log-searches"></a>Örnek günlük aramaları

Aşağıdaki tablo, bu çözüm tarafından toplanan ve hesaplanan kapasite ve performans verileri için örnek günlük aramaları sağlar.


| Sorgu | Açıklama |
|:--- |:--- |
| Tüm ana bilgisayar bellek yapılandırmaları | Perf &#124; where ObjectName == "Kapasite ve Performans" ve CounterName == "Host Assigned Memory MB" &#124; INSTANCEName'ye göre MB = avg(CounterValue) özetlemek |
| Tüm VM bellek yapılandırmaları | Perf &#124; nerede ObjectName == "Kapasite ve Performans" ve CounterName == "VM Atanmış Bellek MB" &#124; INSTANCEName göre MB = avg(CounterValue) özetlemek |
| Tüm VM'lerde Toplam Disk IOPS'nin dökümü | Perf &#124; where ObjectName == "Capacity and Performance" ve (CounterName == "VHD Reads/s" veya CounterName == "VHD Writes/s") &#124; özetlemek AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1h), CounterName, InstanceName |
| Tüm VM'lerde Toplam Disk İş Buzunun Dökümü | Perf &#124; nerede ObjectName == "Kapasite ve Performans" ve (CounterName == "VHD Read MB/s" veya CounterName == "VHD Write MB/s") özetlemek &#124; AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1h), CounterName, InstanceName |
| Tüm CSV'lerde Toplam IOPS'nin dökümü | Perf &#124; nerede ObjectName == "Kapasite ve Performans" ve (CounterName == "CSV Reads/s" veya CounterName == "CSV Writes/s") özetlemek &#124; AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1h), CounterName, InstanceName |
| Tüm CSV'lerde Toplam İş Meçla'nın Dökümü | Perf &#124; nerede ObjectName == "Kapasite ve Performans" ve (CounterName == "CSV Reads/s" veya CounterName == "CSV Writes/s") özetlemek &#124; AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1h), CounterName, InstanceName |
| Tüm CSV'lerde Toplam Gecikme Gecikmesinin Dökümü | Perf &#124; where ObjectName == "Capacity and Performance" ve (CounterName == "CSV Read Latency" veya CounterName == "CSV Write Latency") özetlemek &#124; AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1h), CounterName, InstanceName |


## <a name="next-steps"></a>Sonraki adımlar
* Ayrıntılı Kapasite ve Performans verilerini görüntülemek için [Günlük Analizi'ndeki Günlük aramalarını](../../azure-monitor/log-query/log-query-overview.md) kullanın.
