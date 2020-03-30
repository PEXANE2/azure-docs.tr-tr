---
title: Azure Monitör veri platformu | Microsoft Dokümanlar
description: Azure Monitor tarafından toplanan izleme verileri, hafif ve gelişmiş analiz için kullanılan gerçek zamanlı senaryolara ve günlüklere yakın destekyeteneğine sahip ölçümlere ayrılır.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2019
ms.author: bwren
ms.openlocfilehash: a1b5859341237c1b177ee8deaf636a67f4824948
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77666556"
---
# <a name="azure-monitor-data-platform"></a>Azure Monitör veri platformu

Günümüzün karmaşık bilgi işlem ortamlarında hem bulut hem de şirket içi hizmetlere dayanan dağıtılmış uygulamalar çalıştıran gözlemlenebilirliği etkinleştirmek, dağıtılmış sistemin her katmanından ve her bileşeninden operasyonel verilerin toplanmasını gerektirir. Bu veriler hakkında derin bilgiler gerçekleştirebilmeniz ve kuruluşunuzdaki çok sayıda paydaşları desteklemek için farklı bakış açılarına sahip tek bir cam bölmede birleştirebilmeniz gerekir.

[Azure Monitor,](../overview.md) çeşitli kaynaklardan gelen verileri analiz, görselleştirme ve uyarı için kullanılabildiği ortak bir veri platformuna toplar ve toplar. Birden çok kaynaktan gelen verilerin üzerinde tutarlı bir deneyim sağlar ve bu da size tüm izlenen kaynaklarınızda ve hatta verilerini Azure Monitor'da depolayan diğer hizmetlerden gelen verilerle ilgili derin bilgiler sağlar.


![Azure İzleyici'ye genel bakış](media/data-platform/overview.png)

## <a name="observability-data-in-azure-monitor"></a>Azure Monitör'de gözlemlenebilirlik verileri
Ölçümler, günlükler ve dağıtılmış izlemeler genellikle gözlemlenebilirliğin üç ayağı olarak adlandırılır. Bunlar, izlenen bir sistemin yeterli şekilde gözlemlenebilirliğini sağlamak için bir izleme aracının toplaması ve analiz etmesi gereken farklı veri türleridir. Gözlemlenebilirlik, birden çok sütundan gelen verileri ilişkilendirerek ve izlenen tüm kaynak kümesinde verileri bir araya getirerek elde edilebilir. Azure Monitor birden çok kaynaktan gelen verileri birlikte depoladığı için, veriler ortak bir araç kümesi kullanılarak ilişkilendirilebilir ve çözümlenebilir. Ayrıca, diğer hizmetler için veri barındırmanın yanı sıra birden çok Azure aboneliği ve kiracı arasındaki verileri de ilişkilendirir.

Azure kaynakları önemli miktarda izleme verisi oluşturur. Azure Monitor, bu verileri diğer kaynaklardan gelen verileri izlemeyle birlikte bir Metrikler veya Günlükler platformunda birleştirir. Her biri belirli izleme senaryoları için optimize edilmiş ve her biri Azure Monitor'da farklı özellikleri destekler. Veri çözümlemesi, görselleştirme veya uyarı gibi özellikler, gerekli senaryonuzu en verimli ve uygun maliyetli şekilde uygulayabilmeniz için farklılıkları anlamanızı gerektirir. [Uygulama Öngörüleri](../app/app-insights-overview.md) veya Sanal Taşıtlar için [Azure Monitörü](../insights/vminsights-overview.md) gibi Azure Monitor'daki öngörüler, iki veri türü arasındaki farkları anlamak zorunda kalmadan belirli izleme senaryosuna odaklanmanızı sağlayan analiz araçlarına sahiptir. 


### <a name="metrics"></a>Ölçümler
[Ölçümler,](data-platform-metrics.md) bir sistemin belirli bir zaman dilimindeki bazı yönünü açıklayan sayısal değerlerdir. Düzenli aralıklarla toplanır ve bir zaman damgası, bir ad, bir değer ve bir veya daha fazla tanımlayıcı etiketle tanımlanır. Ölçümler, diğer ölçümlerle karşılaştırıldığında çeşitli algoritmalar kullanılarak toplanabilir ve zaman daki eğilimler için analiz edilebilir. 

Azure Monitor'daki ölçümler, zaman damgalı verileri analiz etmek için optimize edilmiş bir zaman serisi veritabanında depolanır. Bu, ölçümleri özellikle uyarıları ve sorunları hızlı algılamak için uygun hale getirir. Sisteminizin nasıl performans gösterdiğini size söyleyebilirler, ancak sorunların temel nedenini belirlemek için genellikle günlüklerle birleştirilmesi gerekir.

Azure [Ölçümleri Gezgini](../platform/metrics-getting-started.md)ile Azure portalında etkileşimli analiz ler için ölçümler mevcuttur. Diğer verilerle birlikte görselleştirme için azure [panosuna](../learn/tutorial-app-dashboards.md) eklenebilir ve neredeyse gerçek zamanlı [uyarı için](alerts-metric.md)kullanılabilirler.

[Azure Monitör'deki Ölçümlerdeki](data-platform-metrics.md)veri kaynakları da dahil olmak üzere Azure Monitör Ölçümleri hakkında daha fazla bilgi edinin.

### <a name="logs"></a>Günlükler
[Günlükler,](data-platform-logs.md) sistem içinde meydana gelen olaylardır. Bunlar farklı türde veriler içerebilir ve zaman damgası ile yapılandırılmış veya serbest biçimli metin olabilir. Ortamdaki olaylar günlük girişleri oluşturdukça ara sıra oluşturulabilir ve ağır yük altındaki bir sistem genellikle daha fazla günlük hacmi oluşturur.

Azure Monitor'daki günlükler, güçlü bir analiz motoru ve [zengin sorgu dili](/azure/kusto/query/)sağlayan Azure Veri [Gezgini'ni](/azure/data-explorer/) temel alan Bir Günlük Analizi çalışma alanında depolanır. Günlükler genellikle tanımlanan sorunun tam bağlamını sağlamak için yeterli bilgi sağlar ve sorunların kök durumunu tanımlamak için değerlidir.

> [!NOTE]
> Azure Monitör Günlükleri ile Azure'daki günlük veri kaynakları arasında ayrım yapmak önemlidir. Örneğin, Azure'daki abonelik düzeyi olayları, Azure Monitörü menüsünden görüntülediğiniz bir [etkinlik günlüğüne](platform-logs-overview.md) yazılır. Kaynakların çoğu, farklı konumlara iletebileceğiniz bir [kaynak günlüğüne](platform-logs-overview.md) operasyonel bilgiler yazar. Azure Monitor Günlükleri, tüm kaynak kümenizde derin analiz sağlamak için etkinlik günlüklerini ve kaynak günlüklerini diğer izleme verileriyle birlikte toplayan bir günlük veri platformudur.


 Azure portalında [Log Analytics](../log-query/portals.md) ile [günlük sorgularıyla](../log-query/log-query-overview.md) etkileşimli olarak çalışabilir veya sonuçları diğer verilerle birlikte görselleştirme için [azure panosuna](../learn/tutorial-app-dashboards.md) ekleyebilirsiniz. Ayrıca, zamanlama sorgusunun sonuçlarına göre bir uyarı tetikleyecek [günlük uyarıları](alerts-log.md) da oluşturabilirsiniz.

[Azure Monitör'deki Günlüklerde](data-platform-logs.md)veri kaynakları da dahil olmak üzere Azure Monitör Günlükleri hakkında daha fazla bilgi edinin.

### <a name="distributed-traces"></a>Dağıtılmış izlemeler
İzlemeler, dağıtılmış bir sistem aracılığıyla kullanıcı isteğini izleyen ilgili olaylar dizisidir. Bunlar, uygulama kodunun davranışını ve farklı işlemlerin performansını belirlemek için kullanılabilir. Günlükler genellikle dağıtılmış bir sistemin tek tek bileşenleri tarafından oluşturulacak olsa da, izleme, uygulamanızın tüm bileşen kümesindeki çalışmasını ve performansını ölçer.

Azure Monitor'da dağıtılmış izleme, [Application Insights SDK](../app/distributed-tracing.md)ile etkinleştirilir ve izleme verileri Application Insights tarafından toplanan diğer uygulama günlüğü verileriyle birlikte depolanır. Bu, günlük sorguları, panolar ve uyarılar da dahil olmak üzere diğer günlük verileriyle aynı çözümleme araçlarına kullanılabilir hale getirir.

[Dağıtılmış İzleme Nedir?'nde](../app/distributed-tracing.md)dağıtılmış izleme hakkında daha fazla bilgi edinin?


## <a name="compare-azure-monitor-metrics-and-logs"></a>Azure Monitör Ölçümleri ve Günlükleri Karşılaştırın

Aşağıdaki tablo, Azure Monitor'daki Ölçümler i ve Günlükleri karşılaştırın.

| Öznitelik  | Ölçümler | Günlükler |
|:---|:---|:---|
| Avantajlar | Hafif ve uyarı gibi neredeyse gerçek zamanlı senaryolar yeteneğine sahip. Sorunların hızlı tespiti için idealdir. | Zengin sorgu dili ile analiz edilir. Derin analiz ve kök nedenini belirlemek için idealdir. |
| Veri | Yalnızca sayısal değerler | Metin veya sayısal veriler |
| Yapı | Örnek zaman, izlenen kaynak, sayısal değer gibi standart özellik kümesi. Bazı ölçümler, daha fazla tanım için birden çok boyut içerir. | Günlük türüne bağlı olarak benzersiz özellikler kümesi. |
| Koleksiyon | Düzenli aralıklarla toplanır. | Olaylar oluşturulacak bir kaydı tetikledikçe ara sıra toplanabilir. |
| Azure portalında görünüm | Ölçüm Gezgini | Log Analytics |
| Veri kaynakları şunlardır: | Azure kaynaklarından toplanan platform ölçümleri.<br>Uygulamalar Application Insights tarafından izlenir.<br>Uygulama veya API tarafından tanımlanan özel. | Uygulama ve kaynak günlükleri.<br>İzleme çözümleri.<br>Aracılar ve VM uzantıları.<br>Uygulama istekleri ve özel durumlar.<br>Azure Güvenlik Merkezi.<br>Veri Toplayıcı API. |

## <a name="collect-monitoring-data"></a>İzleme verilerini toplama
Azure Monitor için farklı [veri kaynakları,](data-sources.md) Log Analytics çalışma alanına (Günlükler) veya Azure Monitor ölçüm veritabanına (Ölçümler) veya her ikisine de yazar. Bazı kaynaklar doğrudan bu veri depolarına yazarken, diğerleri Azure depolama gibi başka bir konuma yazabilir ve günlükleri veya ölçümleri doldurmak için bazı yapılandırmalar gerektirebilir. 

Her türü dolduran farklı veri kaynaklarının listesi için [Azure Monitörü'nde Ölçümler'e](data-platform-metrics.md) ve [Azure Monitör'de Günlükler'e](data-platform-logs.md) bakın.


## <a name="stream-data-to-external-systems"></a>Verileri dış sistemlere aktarın
İzleme verilerini çözümlemek için Azure'daki araçları kullanmanın yanı sıra, güvenlik bilgileri ve olay yönetimi (SIEM) ürünü gibi harici bir araca iletme gereksiniminiz de olabilir. Bu iletme genellikle doğrudan izlenen kaynaklardan [Azure Olay Hub'ları](/azure/event-hubs/)üzerinden yapılır. Bazı kaynaklar, gerekli verileri almak için Mantık Uygulaması gibi başka bir işlemi kullanabilirsiniz iken, doğrudan bir olay hub'ına veri göndermek için yapılandırılabilir. Ayrıntılar [için harici bir araç tarafından tüketim için bir etkinlik merkezine Azure izleme verilerini akış](stream-monitoring-data-event-hubs.md) bakın.



## <a name="next-steps"></a>Sonraki adımlar

- [Azure Monitör'de Ölçümler](data-platform-metrics.md)hakkında daha fazla bilgi edinin.
- [Azure Monitor'daki Günlükler](data-platform-logs.md)hakkında daha fazla bilgi edinin.
- Azure'daki farklı kaynaklar için [kullanılabilen izleme verileri](data-sources.md) hakkında bilgi edinin.
