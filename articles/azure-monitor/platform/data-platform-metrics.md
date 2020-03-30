---
title: Azure Monitör'de Ölçümler | Microsoft Dokümanlar
description: Azure Monitor'da, neredeyse gerçek zamanlı senaryoları destekleyebilecek hafif izleme verileri olan ölçümleri açıklar.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2019
ms.author: bwren
ms.openlocfilehash: 80bbf83da17d833c4f8bb1abac9610d70e9a23cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274833"
---
# <a name="metrics-in-azure-monitor"></a>Azure İzleyicisi'nde ölçümler

> [!NOTE]
> Azure Monitor veri platformu iki temel veri türüne dayanır: Ölçümler ve Günlükler. Bu makalede Ölçümler açıklanmaktadır. Günlüklerin ayrıntılı bir açıklaması için [Azure Monitor'daki Günlükler'e](data-platform-logs.md) ve ikisinin karşılaştırması için [Azure Monitor veri platformuna](data-platform.md) bakın.

Azure Monitor'daki ölçümler hafiftir ve neredeyse gerçek zamanlı senaryoları destekleyebilir ve bu senaryoları özellikle sorunları uyarmak ve hızlı bir şekilde algılamak için kullanışlı hale getirir. Bu makalede, ölçümlerin nasıl yapılandırıldığı, bunlarla neler yapabileceğiniz açıklanmaktadır ve verileri ölçümlerde depolayan farklı veri kaynaklarını tanımlar.

## <a name="what-are-metrics"></a>Ölçümler nedir?
Ölçümler, bir sistemin belirli bir zamanda bazı yönlerini açıklayan sayısal değerlerdir. Ölçümler düzenli aralıklarla toplanır ve sık sık örneklenebildikleri için uyarı için yararlıdır ve nispeten basit bir mantıkla bir uyarı hızlı bir şekilde ateşlenebilir.

## <a name="what-can-you-do-with-azure-monitor-metrics"></a>Azure Monitör Ölçümleri ile ne yapabilirsiniz?
Aşağıdaki tablo, Azure Monitor'da metrik verileri kullanmanın farklı yollarını listeler.

|  |  |
|:---|:---|
| Çözümleme | Grafikte toplanan ölçümleri çözümlemek ve farklı kaynaklardan gelen ölçümleri karşılaştırmak için [ölçümler gezginini](metrics-charts.md) kullanın. |
| Görselleştirme | Grafiği metrikler gezgininden [Azure panosuna](../learn/tutorial-app-dashboards.md)sabitle.<br>Etkileşimli bir raporda birden çok veri kümesiyle birleştirmek için bir [çalışma kitabı](../app/usage-workbooks.md) oluşturun. Bir sorgunun sonuçlarını [grafana'ya](grafana-plugin.md) aktararak panolarından yararlanın ve diğer veri kaynaklarıyla birleştirin. |
| Uyarı | Bir bildirim gönderen veya metrik değer bir eşiği geçtiğinde [otomatik eylemde](action-groups.md) olan bir [metrik uyarı kuralını](alerts-metric.md) yapılandırın. |
| Otomatikleştirme |  Bir eşikten geçen bir metrik değere göre kaynakları artırmak veya azaltmak için [Otomatik Ölçek'i](autoscale-overview.md) kullanın. |
| Dışarı Aktarma | Azure Monitör Günlükleri'ndeki verilerle birlikte Azure Monitor Ölçümleri'ndeki verileri analiz etmek ve metrik değerleri 93 günden uzun süre depolamak için [Ölçümleri Günlüklere Yönlendirin.](resource-logs-collect-storage.md)<br>Ölçümleri dış sistemlere yönlendirmek için [Bir Olay Hub'ına](stream-monitoring-data-event-hubs.md) aktarın. |
| Almak | [PowerShell cmdlets](https://docs.microsoft.com/powershell/module/az.applicationinsights) kullanarak bir komut satırından metrik değerlere erişin<br>REST API'yi kullanarak özel uygulamadan metrik [değerlere erişin.](rest-api-walkthrough.md)<br>[CLI](/cli/azure/monitor/metrics)kullanarak bir komut satırından metrik değerlere erişin. |
| Arşiv | Uyumluluk, denetim veya çevrimdışı raporlama amacıyla kaynağınızın performans veya sistem durumu geçmişini [arşivleyin.](..//learn/tutorial-archive-data.md) |

## <a name="how-is-data-in-azure-monitor-metrics-structured"></a>Azure Monitör Ölçümleri'ndeki veriler nasıl yapılandırılır?
Azure Monitor Ölçümleri tarafından toplanan veriler, zaman damgalı verileri analiz etmek için optimize edilmiş bir zaman serisi veritabanında depolanır. Her metrik değer kümesi aşağıdaki özelliklere sahip bir zaman serisidir:

* Değerin toplanma zamanı
* Değerin ilişkili olduğu kaynak
* Metrik için bir kategori gibi davranan bir ad alanı
* Metrik ad
* Değerin kendisi
* Bazı [ölçümler, çok boyutlu ölçümlerde](#multi-dimensional-metrics)açıklandığı gibi birden çok boyuta sahip olabilir. Özel ölçümler en fazla 10 boyuta sahip olabilir.

## <a name="multi-dimensional-metrics"></a>Çok boyutlu ölçümler
Metrik verileriçin zorluklardan biri, toplanan değerler için bağlam sağlamak için genellikle sınırlı bilgilere sahip olmasıdır. Azure Monitor bu zorluğu çok boyutlu ölçümlerle gider. Bir metnin boyutları, metrik değeri açıklamak için ek veri taşıyan ad değeri çiftleridir. Örneğin, kullanılabilir bir _disk alanı,_ C değerlerine sahip _Sürücü_ adlı bir boyuta sahip _olabilir:_, _D:_, tüm sürücülerde veya her sürücü için kullanılabilir disk alanının tek tek görüntülenmesini sağlayacak.

Aşağıdaki _örnekte, Ağ İş İlerletisi_adı verilen varsayımsal bir metrik için iki veri kümesi gösterin. İlk veri kümesinin boyutu yoktur. İkinci veri kümesi iki boyutlu değerleri gösterir, _IP Adresi_ ve _Yönü:_

### <a name="network-throughput"></a>Ağ Throughput

| Zaman damgası     | Metrik Değer |
| ------------- |:-------------|
| 8/9/2017 8:14 | 1,331.8 Kbps |
| 8/9/2017 8:15 | 1,141.4 Kbps |
| 8/9/2017 8:16 | 1,110.2 Kbps |

Bu boyutsal olmayan metrik sadece "belirli bir zamanda ağ iş ımın ne olduğunu" gibi temel bir soruyu yanıtlayabilir.

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>Ağ İşİ + iki boyutlu ("IP" ve "Yön")

| Zaman damgası     | Boyut "IP"   | Boyut "Yön" | Metrik Değer|
| ------------- |:-----------------|:------------------- |:-----------|
| 8/9/2017 8:14 | IP="192.168.5.2" | Yön="Gönder"    | 646,5 Kbps |
| 8/9/2017 8:14 | IP="192.168.5.2" | Yön="Al" | 420.1 Kbps |
| 8/9/2017 8:14 | IP="10.24.2.15"  | Yön="Gönder"    | 150.0 Kbps |
| 8/9/2017 8:14 | IP="10.24.2.15"  | Yön="Al" | 115.2 Kbps |
| 8/9/2017 8:15 | IP="192.168.5.2" | Yön="Gönder"    | 515.2 Kbps |
| 8/9/2017 8:15 | IP="192.168.5.2" | Yön="Al" | 371.1 Kbps |
| 8/9/2017 8:15 | IP="10.24.2.15"  | Yön="Gönder"    | 155.0 Kbps |
| 8/9/2017 8:15 | IP="10.24.2.15"  | Yön="Al" | 100.1 Kbps |

Bu metrik, "her IP adresi için ağ verisi ne kadardı?" ve "alınan adedine göre ne kadar veri gönderildi?" gibi soruları yanıtlayabilir. Çok boyutlu ölçümler, boyutsal olmayan ölçümlerle karşılaştırıldığında ek analitik ve tanısal değer taşır.

## <a name="interacting-with-azure-monitor-metrics"></a>Azure İzleyici Ölçümleri ile etkileşim kurma
Metrik veritabanınızdaki verileri etkileşimli olarak analiz etmek ve zaman içinde birden çok ölçümün değerlerini grafiklemek için [Metrics Explorer'ı](metrics-charts.md) kullanın. Grafikleri diğer görsellerle görüntülemek için bir panoya sabitleyebilirsiniz. Ayrıca [Azure izleme REST API](rest-api-walkthrough.md)kullanarak ölçümleri alabilirsiniz.

![Ölçüm Gezgini](media/data-platform/metrics-explorer.png)

## <a name="sources-of-azure-monitor-metrics"></a>Azure Monitör Ölçümlerinin Kaynakları
Azure Monitor tarafından toplanan üç temel ölçüm kaynağı vardır. Bu ölçümler Azure Monitor metrik veritabanında toplandıktan sonra, kaynakları ne olursa olsun birlikte değerlendirilebilir.

**Platform ölçümleri** Azure kaynakları tarafından oluşturulur ve size sağlık durumları ve performansları hakkında görünürlük sağlar. Her kaynak türü, herhangi bir yapılandırma gerektirmeden farklı bir [ölçüm kümesi](metrics-supported.md) oluşturur. Platform ölçümleri, metrik tanımında aksi belirtilmedikçe Azure kaynaklarından bir dakikalık sıklıkta toplanır. 

**Konuk işletim sistemi ölçümleri** sanal bir makinenin konuk işletim sisteminden toplanır. [Windows Diagnostic Extension (WAD)](../platform/diagnostics-extension-overview.md) ile Windows sanal makineler için konuk işletim sistemi ölçümlerini ve [InfluxData Telegraf Agent](https://www.influxdata.com/time-series-platform/telegraf/)ile Linux sanal makineleri için etkinleştirin.

**Uygulama ölçümleri,** izlenen uygulamalarınız için Application Insights tarafından oluşturulur ve performans sorunlarını algılamanıza ve uygulamanızın nasıl kullanıldığına ilişkin eğilimleri izlemenize yardımcı olur. Bu, Sunucu _yanıt süresi_ ve _Tarayıcı özel durumları_gibi değerleri içerir.

**Özel ölçümler,** otomatik olarak kullanılabilen standart ölçümlere ek olarak tanımladığınız ölçümlerdir. Uygulamaöngörüleri tarafından izlenen [özel ölçümleri tanımlayabilir](../app/api-custom-events-metrics.md) veya [özel ölçümler API'sini](metrics-store-custom-rest-api.md)kullanarak bir Azure hizmeti için özel ölçümler oluşturabilirsiniz.

## <a name="retention-of-metrics"></a>Ölçümlerin Tutulması
Azure'daki çoğu kaynak için ölçümler 93 gün boyunca depolanır. Bazı istisnalar vardır:

**Konuk İşletim Sistemi ölçümleri**
-   **Klasik konuk işletim sistemi ölçümleri.** Bunlar, Windows Diagnostic Extension [(WAD)](../platform/diagnostics-extension-overview.md) veya Linux [Diagnostic Extension (LAD)](../../virtual-machines/extensions/diagnostics-linux.md) tarafından toplanan ve bir Azure depolama hesabına yönlendirilen performans sayaçlarıdır. Bu ölçümler için bekletme 14 gündür.
-   **Azure Monitör Ölçümleri'ne gönderilen konuk İşletim Sistemi ölçümleri.** Bunlar, Windows Diagnostic Extension [(WAD)](diagnostics-extension-overview.md) tarafından toplanan ve [Azure Monitor veri lavabosuna](diagnostics-extension-overview.md#data-destinations)veya Linux makinelerindeki [InfluxData Telegraf Agent](https://www.influxdata.com/time-series-platform/telegraf/) aracılığıyla gönderilen performans sayaçlarıdır. Bu ölçümler için bekletme 93 gündür.
-   **Log Analytics aracısı tarafından toplanan konuk işletim sistemi ölçümleri.** Bunlar Log Analytics aracısı tarafından toplanan ve log analytics çalışma alanına gönderilen performans sayaçlarıdır. Bu ölçümler için bekletme 31 gündür ve 2 yıla kadar uzatılabilir.

**Uygulama Öngörüleri günlük tabanlı ölçümler.** 
- Sahnenin arkasında, [günlük tabanlı ölçümler](../app/pre-aggregated-metrics-log-metrics.md) günlük sorgularına dönüşür. Bekletmeleri, temel günlüklerde olayların tutulmasıyla eşleşir. Application Insights kaynakları için günlükler 90 gün boyunca saklanır.


> [!NOTE]
> Azure [Monitor kaynakları için platform ölçümlerini](resource-logs-collect-storage.md) uzun vadeli trendler için Log Analytics çalışma alanına gönderebilirsiniz.





## <a name="next-steps"></a>Sonraki adımlar

- Azure Monitör [veri platformu](data-platform.md)hakkında daha fazla bilgi edinin.
- Azure [Monitor'da günlük verileri](data-platform-logs.md)hakkında bilgi edinin.
- Azure'daki farklı kaynaklar için [kullanılabilen izleme verileri](data-sources.md) hakkında bilgi edinin.
