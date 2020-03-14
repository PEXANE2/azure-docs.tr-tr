---
title: Azure Izleyici 'de ölçümler | Microsoft Docs
description: Azure Izleyici 'de, neredeyse gerçek zamanlı senaryoları destekleyebilen hafif izleme verileri olan ölçümleri açıklar.
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
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79274833"
---
# <a name="metrics-in-azure-monitor"></a>Azure İzleyicisi'nde ölçümler

> [!NOTE]
> Azure Izleyici veri platformu iki temel veri türünü temel alır: ölçümler ve Günlükler. Bu makalede ölçümler açıklanır. [Azure izleyici 'de](data-platform-logs.md) Günlükler hakkında ayrıntılı bir açıklama ve [Azure izleyici veri platformu](data-platform.md) için bu Iki değerin karşılaştırması için günlüklere bakın.

Azure Izleyici 'de ölçümler hafif ve neredeyse gerçek zamanlı senaryoları desteklemeye yönelik olarak, sorunların uyarı ve hızlı bir şekilde algılanması için oldukça yararlı hale sahiptir. Bu makalede, ölçümlerin nasıl yapılandırıldığı, bunlarla yapabilecekleriniz ve ölçümlerde veri depolayan farklı veri kaynakları tanımlanmaktadır.

## <a name="what-are-metrics"></a>Ölçümler nelerdir?
Sayısal değerleri, belirli bir zamanda bir sistem bazı yönlerini açıklamak ölçümleridir. Ölçümler düzenli aralıklarla toplanır ve sıklıkla örneklenebilir ve bir uyarı görece basit mantığa göre hızla tetiklenebilir.

## <a name="what-can-you-do-with-azure-monitor-metrics"></a>Azure Izleyici ölçümleriyle ne yapabilirsiniz?
Aşağıdaki tabloda, Azure Izleyici 'de ölçüm verilerini kullanmanın farklı yolları listelenmektedir.

|  |  |
|:---|:---|
| Çözümleme | Bir grafikteki toplanan ölçümleri çözümlemek ve farklı kaynaklardaki ölçümleri karşılaştırmak için [Ölçüm Gezgini](metrics-charts.md) 'ni kullanın. |
| Görselleştirme | Ölçüm Gezgini 'nden bir [Azure panosuna](../learn/tutorial-app-dashboards.md)grafik sabitleme.<br>Etkileşimli bir raporda birden fazla veri kümesiyle birleştirilecek bir [çalışma kitabı](../app/usage-workbooks.md) oluşturun. Bir sorgunun sonuçlarını [Grafana](grafana-plugin.md) olarak dışa aktarın ve diğer veri kaynaklarıyla birleştirin. |
| Uyarı | Ölçüm değeri bir eşiği aştığında bir bildirim gönderen veya [otomatik eylem](action-groups.md) alan bir [ölçüm uyarısı kuralı](alerts-metric.md) yapılandırın. |
| Otomatikleştirme |  Bir eşiği aşan ölçüm değerine göre kaynakları artırmak veya azaltmak için [Otomatik ölçeklendirmeyi](autoscale-overview.md) kullanın. |
| Dışarı Aktarma | Azure izleyici günlüklerindeki verilerle birlikte verileri analiz etmek ve 93 günden daha uzun süre için ölçüm değerlerini depolamak üzere [ölçümleri günlüklere yönlendirin](resource-logs-collect-storage.md) .<br>Bir [Olay Hub 'ına](stream-monitoring-data-event-hubs.md) , bunları dış sistemlere yönlendirmek için ölçüm akışı. |
| Almanın | [PowerShell cmdlet 'lerini](https://docs.microsoft.com/powershell/module/az.applicationinsights) kullanarak bir komut satırından ölçüm değerlerine erişin<br>[REST API](rest-api-walkthrough.md)kullanarak özel uygulamadan ölçüm değerlerine erişin.<br>[CLI](/cli/azure/monitor/metrics)kullanarak bir komut satırından ölçüm değerlerine erişin. |
| Arşiv | Uyumluluk, denetim veya çevrimdışı raporlama amacıyla kaynağınızın performans veya sistem durumu geçmişini [arşivleyin](..//learn/tutorial-archive-data.md) . |

## <a name="how-is-data-in-azure-monitor-metrics-structured"></a>Azure Izleyici ölçümleri içindeki veriler nasıl yapılandırılır?
Azure Izleyici ölçümleri tarafından toplanan veriler zaman damgalı verileri çözümlemek için en iyi duruma getirilmiş bir zaman serisi veritabanında depolanır. Her ölçüm değeri kümesi, aşağıdaki özelliklere sahip bir zaman dizisidir:

* Değerin toplandığı saat
* Değerin ilişkilendirildiği kaynak
* Ölçüm için bir kategori gibi davranan bir ad alanı
* Ölçüm adı
* Değerin kendisi
* Bazı ölçümler, çok [boyutlu ölçümler](#multi-dimensional-metrics)bölümünde açıklandığı gibi birden çok boyuta sahip olabilir. Özel ölçümler, en fazla 10 boyuta sahip olabilir.

## <a name="multi-dimensional-metrics"></a>Çok boyutlu ölçümleri
Ölçüm verilerine yönelik güçlüklerden biri, toplanan değerler için bağlam sağlamak üzere genellikle sınırlı bilgi sağlamaktır. Azure Izleyici, çok boyutlu ölçümler ile bu zorluğu ele alınmaktadır. Bir ölçüm boyutlarını ölçüm değeri tanımlamak için ek veri taşıyan ad-değer çiftleridir. Örneğin, bir ölçüm _kullanılabilir disk alanı_ , _C:_ , _D:_ değerlerine sahip _sürücü_ adlı bir boyuta sahip olabilir, bu da tüm sürücülerde veya her sürücü için ayrı ayrı kullanılabilir disk alanı görüntülenmesine izin verir.

Aşağıdaki örnekte, _ağ işleme_adlı bir kuramsal ölçüm için iki veri kümesi gösterilmektedir. İlk veri kümesi herhangi bir boyutu vardır. İkinci veri kümesi, iki boyutlu değerleri, _IP adresini_ ve _yönü_gösterir:

### <a name="network-throughput"></a>Ağ aktarım hızı

| Zaman damgası     | Ölçüm değeri |
| ------------- |:-------------|
| 8/9/2017 8:14 | 1,331.8 KB/sn |
| 8/9/2017 8:15 | 1,141.4 KB/sn |
| 8/9/2017 8:16 | 1,110.2 KB/sn |

Temel bir soru cevap ister yalnızca "my ağ aktarım hızı belirli bir zamanda neydi?" boyutlu Bu ölçüm olabilir.

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>Ağ aktarım hızı + iki boyutlu ("IP" ve "Yönü")

| Zaman damgası     | Boyut "IP"   | Boyut "Yönü" | Ölçüm değeri|
| ------------- |:-----------------|:------------------- |:-----------|
| 8/9/2017 8:14 | IP "192.168.5.2" = | Yön "Gönder" =    | 646.5 KB/sn |
| 8/9/2017 8:14 | IP "192.168.5.2" = | Yön "Al" = | 420.1 KB/sn |
| 8/9/2017 8:14 | IP "10.24.2.15" =  | Yön "Gönder" =    | 150.0 KB/sn |
| 8/9/2017 8:14 | IP "10.24.2.15" =  | Yön "Al" = | 115.2 KB/sn |
| 8/9/2017 8:15 | IP "192.168.5.2" = | Yön "Gönder" =    | 515.2 KB/sn |
| 8/9/2017 8:15 | IP "192.168.5.2" = | Yön "Al" = | 371.1 KB/sn |
| 8/9/2017 8:15 | IP "10.24.2.15" =  | Yön "Gönder" =    | 155.0 KB/sn |
| 8/9/2017 8:15 | IP "10.24.2.15" =  | Yön "Al" = | 100.1 KB/sn |

Bu ölçüm, "ağ aktarım hızı için her bir IP adresi neydi?" ve "karşı gönderilen veri miktarını alındı?" gibi soruları yanıtlayabilirsiniz Çok boyutlu ölçümler, boyutsuz ölçümler için kıyasla ek analiz ve tanılama değer taşır.

## <a name="interacting-with-azure-monitor-metrics"></a>Azure Izleyici ölçümleri ile etkileşim kurma
Ölçüm veritabanınızdaki verileri etkileşimli olarak analiz etmek ve zaman içinde birden çok ölçümün değerlerini grafik olarak eklemek için [Ölçüm Gezgini](metrics-charts.md) kullanın. Grafikleri bir panoya sabitleyebilir ve diğer görselleştirmelerle görüntüleyebilirsiniz. [Azure izleme REST API](rest-api-walkthrough.md)kullanarak ölçümleri de alabilirsiniz.

![Ölçüm Gezgini](media/data-platform/metrics-explorer.png)

## <a name="sources-of-azure-monitor-metrics"></a>Azure Izleyici ölçümlerinin kaynakları
Azure İzleyici tarafından toplanan ölçümleri üç temel kaynakları vardır. Bu ölçümler Azure Izleyici ölçüm veritabanında toplandıktan sonra, kaynağı ne olursa olsun birlikte değerlendirilebilirler.

**Platform ölçümleri** Azure kaynakları tarafından oluşturulur ve bunların sistem durumu ve performansına ilişkin görünürlük sağlar. Her kaynak türü, hiçbir yapılandırma gerekmeden [farklı bir ölçüm kümesi](metrics-supported.md) oluşturur. Platform ölçümleri, ölçüm tanımında aksi belirtilmedikçe, Azure kaynaklarından tek dakikalık bir sıklıkta toplanır. 

**Konuk işletim sistemi ölçümleri** , bir sanal makinenin Konuk işletim sisteminden toplanır. [Windows Tanılama uzantısı (WAD)](../platform/diagnostics-extension-overview.md) ile Windows sanal makineler için konuk işletim sistemi ölçümlerini etkinleştirin ve etkileyen Linux sanal makineleri Için [telegraf aracısını](https://www.influxdata.com/time-series-platform/telegraf/)etkinleştirin.

**Uygulama ölçümleri** , izlenen uygulamalarınız için Application Insights tarafından oluşturulur ve performans sorunlarını tespit etmenize ve uygulamanızın kullanılma sıklığındaki eğilimleri izlemenize yardımcı olur. Buna _sunucu yanıt süresi_ ve _tarayıcı özel durumları_gibi değerler dahildir.

**Özel ölçümler** , otomatik olarak kullanılabilen standart ölçümlere ek olarak tanımladığınız ölçümlerdir. Uygulamanızda Application Insights tarafından izlenen [özel ölçümleri tanımlayabilir](../app/api-custom-events-metrics.md) veya [özel ölçüm API](metrics-store-custom-rest-api.md)'sini kullanarak bir Azure hizmeti için özel ölçümler oluşturabilirsiniz.

## <a name="retention-of-metrics"></a>Ölçüm bekletme
Azure 'daki çoğu kaynak için ölçümler 93 gün süreyle depolanır. Bazı özel durumlar vardır:

**Konuk işletim sistemi ölçümleri**
-   **Klasik Konuk işletim sistemi ölçümleri**. Bunlar, [Windows Tanılama uzantısı (WAD)](../platform/diagnostics-extension-overview.md) veya [Linux Tanılama uzantısı (LAD)](../../virtual-machines/extensions/diagnostics-linux.md) tarafından toplanan performans sayaçlarıdır ve bir Azure depolama hesabına yönlendirilir. Bu ölçümler için bekletme 14 gündür.
-   **Azure Izleyici ölçümlerine gönderilen Konuk işletim sistemi ölçümleri**. Bunlar, [Windows Tanılama uzantısı (WAD)](diagnostics-extension-overview.md) tarafından toplanan ve [Azure izleyici veri havuzuna](diagnostics-extension-overview.md#data-destinations)gönderilen ve Linux makinelerinde [etkileyen bir telegraf Aracısı](https://www.influxdata.com/time-series-platform/telegraf/) aracılığıyla performans sayaçlarıdır. Bu ölçümler için bekletme 93 gündür.
-   **Log Analytics Aracısı tarafından toplanan Konuk işletim sistemi ölçümleri**. Bunlar, Log Analytics Aracısı tarafından toplanan ve bir Log Analytics çalışma alanına gönderilen performans sayaçlarıdır. Bu ölçümler için bekletme 31 gündür ve 2 yıla kadar genişletilebilir.

**Günlük tabanlı ölçümleri Application Insights**. 
- Sahnenin arkasında [günlük tabanlı ölçümler](../app/pre-aggregated-metrics-log-metrics.md) , günlük sorgularına dönüştürülür. Saklama süresi, olayların temel alınan günlüklerde bekletilmesi ile eşleşir. Application Insights kaynaklar için Günlükler 90 gün süreyle depolanır.


> [!NOTE]
> [Azure izleyici kaynakları için platform ölçümlerini, uzun dönem eğilimi için bir Log Analytics çalışma alanına gönderebilirsiniz](resource-logs-collect-storage.md) .





## <a name="next-steps"></a>Sonraki adımlar

- [Azure izleyici veri platformu](data-platform.md)hakkında daha fazla bilgi edinin.
- [Azure izleyici 'de günlük verileri](data-platform-logs.md)hakkında bilgi edinin.
- Azure 'daki farklı kaynaklar için [kullanılabilen izleme verileri](data-sources.md) hakkında bilgi edinin.
