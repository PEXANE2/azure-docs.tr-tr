---
title: Ölçümler ile Azure Veri Gezgini performansını, sağlık & kullanımını izleyin
description: Kümenin performansını, sistem durumunu ve kullanımını izlemek için Azure Veri Gezgini ölçümlerini nasıl kullanacağınızı öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/19/2020
ms.openlocfilehash: 1319b8cd6ac8a0eb83381c24bcde9996458e47a7
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77560313"
---
# <a name="monitor-azure-data-explorer-performance-health-and-usage-with-metrics"></a>Ölçümler ile Azure Veri Gezgini performansını, sistem durumunu ve kullanımını izleyin

Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure Veri Gezgini kullanmak için, önce bir küme oluşturun ve bu kümede bir veya daha fazla veritabanı oluşturursunuz. Daha sonra sorguları bu verilere karşı çalıştırmak için bir veritabanına (yükleme) sahip olursunuz. Azure Veri Gezgini ölçümleri, küme kaynaklarının sistem durumu ve performansına göre önemli göstergeler sağlar. Azure Veri Gezgini küme durumunu ve belirli bir senaryonuzdaki performansı tek başına ölçümler olarak izlemek için bu makalede ayrıntılı ölçümleri kullanın. Ölçümleri, işletimsel [Azure panoları](/azure/azure-portal/azure-portal-dashboards) ve [Azure uyarıları](/azure/azure-monitor/platform/alerts-metric-overview)temeli olarak da kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Bir tane yoksa, [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/)oluşturabilirsiniz.
* Bir [küme ve veritabanı](create-cluster-database-portal.md).

## <a name="using-metrics"></a>Ölçümleri kullanma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Azure Veri Gezgini kümenizde **ölçümler** ' i seçerek ölçümler bölmesini açın ve kümenizdeki Analize başlayın.
    Ölçüm](media/using-metrics/select-metrics.png)![seçin.
1. Ölçümler bölmesinde: ölçüm bölmesinde ![](media/using-metrics/metrics-pane.png)
    1. Ölçüm grafiği oluşturmak için **ölçüm adı '** nı ve ölçüm başına ilgili **toplamayı** seçin. **Kaynak** ve **ölçüm ad alanı** seçiciler, Azure Veri Gezgini kümeniz için önceden seçilmiştir. Farklı ölçümler hakkında daha fazla bilgi için bkz. [desteklenen Azure Veri Gezgini ölçümleri](#supported-azure-data-explorer-metrics).
    1. Aynı grafikte çizilen birden çok ölçümü görmek için **ölçüm Ekle** ' yi seçin.
    1. Birden çok grafiği tek bir görünümde görmek için **+ yeni grafik** ' i seçin.
    1. Zaman aralığını değiştirmek için saat seçiciyi kullanın (varsayılan: son 24 saat).
    1. Boyutları olan ölçümler için [ **Filtre Ekle** ve **bölmeyi Uygula** '](/azure/azure-monitor/platform/metrics-getting-started#apply-dimension-filters-and-splitting) yı kullanın.
    1. Grafik yapılandırmanızı Pano 'ya eklemek için **panoya sabitle** ' yi seçerek yeniden görüntüleyebilirsiniz.
    1. Küme ölçütlerini kullanarak ölçümlerinizi görselleştirmek için **Yeni bir uyarı kuralı** ayarlayın. Yeni uyarı kuralı, grafiğinizdeki hedef kaynağı, ölçümü, bölmeyi ve filtre boyutlarını içerir. [Uyarı kuralı oluşturma bölmesinde](/azure/azure-monitor/platform/metrics-charts#create-alert-rules)bu ayarları değiştirin.

[Ölçüm Gezgini](/azure/azure-monitor/platform/metrics-getting-started)kullanımı hakkında ek bilgi.

## <a name="supported-azure-data-explorer-metrics"></a>Desteklenen Azure Veri Gezgini ölçümleri

Desteklenen Azure Veri Gezgini ölçümleri, kullanıma göre çeşitli kategorilere ayrılmıştır. 

### <a name="cluster-health-metrics"></a>Küme durumu ölçümleri

Küme durumu ölçümleri kümenin genel durumunu izler. Bu, kaynak ve Alım kullanımını ve yanıt hızını içerir.

**Ölçüm** | **Birim** | **Toplama** | **Ölçüm açıklaması** | **Boyutlarına** |
|---|---|---|---|---|
| Önbellek kullanımı | Yüzde | Ortalama, en fazla, en az | Küme tarafından şu anda kullanılan ayrılmış önbellek kaynaklarının yüzdesi. Önbellek, tanımlı önbellek ilkesine göre Kullanıcı etkinliği için ayrılan SSD 'nin boyutudur. Ortalama önbellek kullanımı %80 veya daha az bir küme için sürdürülebilir bir durumdur. Ortalama önbellek kullanımı %80 ' den fazla ise, küme, depolama için iyileştirilmiş bir fiyatlandırma katmanına [ölçeklendirilebilir](manage-cluster-vertical-scaling.md) veya daha fazla örneğe [ölçeklenmelidir](manage-cluster-horizontal-scaling.md) . Alternatif olarak, önbellek ilkesini uyarlayın (önbellekte daha az gün). Önbellek kullanımı %100 ' den fazlaysa, önbelleğe alma ilkesine göre önbelleğe alınacak verilerin boyutu, kümedeki toplam önbellek boyutudur. | None |
| CPU | Yüzde | Ortalama, en fazla, en az | Kümedeki makineler tarafından şu anda kullanılmakta olan ayrılmış işlem kaynaklarının yüzdesi. Bir küme için bir ortalama %80 veya daha az CPU, sürdürülebilirlik. En yüksek CPU değeri %100 ' dir, bu da verileri işlemek için ek işlem kaynakları olmadığı anlamına gelir. Bir küme iyi gerçekleştirmediğinde, engellenen belirli CPU 'ların olup olmadığını öğrenmek için CPU 'nun en büyük değerini denetleyin. | None |
| Alım kullanımı | Yüzde | Ortalama, en fazla, en az | Kapasite ilkesinde, alma işlemini gerçekleştirmek için ayrılan toplam kaynaklardan verileri almak için kullanılan gerçek kaynakların yüzdesi. Varsayılan kapasite ilkesi 512 ' den fazla eşzamanlı alım işlemi veya kaynak için yatırılan küme kaynaklarının %75 ' inden fazla değil. %80 veya daha az% ' un ortalama alım kullanımı, bir küme için sürdürülebilir bir durumdur. Alım kullanımının en büyük değeri %100 ' dir, bu da tüm küme alma yeteneğinin kullanıldığı ve bir alım sırasının neden olabileceği anlamına gelir. | None |
| Canlı tut | Sayı | Ort | Kümenin yanıt hızını izler. Tam yanıt veren bir küme 1 değerini döndürür ve Engellenen veya bağlantısı kesik bir küme 0 döndürür. |
| Kısıtlanmış komutların toplam sayısı | Sayı | Ort, Max, min, Sum | İzin verilen en fazla sayıda eşzamanlı (paralel) komuta ulaşıldığından kümedeki kısıtlı (reddedilen) komutların sayısı. | None |
| Toplam kapsam sayısı | Sayı | Ort, Max, min, Sum | Kümedeki toplam veri uzantısı sayısı. Bu ölçümdeki değişiklikler, veri kapsamlarını birleştirme, CPU ağır bir etkinlik olduğundan büyük miktarda veri yapısı değişikliği ve kümede yüksek yük anlamına gelmez. | None |
| | | | |

### <a name="export-health-and-performance-metrics"></a>Durum ve performans ölçümlerini dışarı aktarma

Durum ve performans ölçümlerini dışarı aktarma, genel sistem durumunu ve genel durum, sonuçlar, kayıt sayısı ve kullanım gibi dışarı aktarma işlemlerinin performansını izler.

**Ölçüm** | **Birim** | **Toplama** | **Ölçüm açıklaması** | **Boyutlarına** |
|---|---|---|---|---|
Dışarı aktarılmış kayıtların sürekli dışa aktarma sayısı    | Sayı | Toplam | Tüm sürekli dışa aktarma işlerinde dışarı aktarılmış kayıt sayısı. | None |
Sürekli dışarı aktarma maksimum değeri |    Sayı   | Maks   | Kümedeki sürekli dışarı aktarma işleri tarafından raporlanan süre (dakika cinsinden). | None |
Sürekli dışarı aktarma bekleyen sayısı | Sayı | Maks   | Bekleyen sürekli dışarı aktarma işi sayısı. Bu işler, büyük olasılıkla yetersiz kapasiteden dolayı bir kuyrukta çalışmaya hazırlanıyor ancak bir kuyruk bekleniyor. 
Sürekli dışarı aktarma sonucu    | Sayı |   Sayı   | Her bir sürekli dışarı aktarma çalıştırmasının hata/başarı sonucu. | ContinuousExportName |
Kullanım verme |    Yüzde | Maks   | Küme içindeki toplam dışa aktarma kapasitesinin dışında, kullanılan dışarı aktarma kapasitesi (0 ile 100 arasında). | None |
| | | | |

### <a name="ingestion-health-and-performance-metrics"></a>Alım durumu ve performans ölçümleri

Alım durumu ve performans ölçümleri, gecikme, sonuçlar ve birim gibi alma işlemlerinin genel durumunu ve performansını izler.

**Ölçüm** | **Birim** | **Toplama** | **Ölçüm açıklaması** | **Boyutlarına** |
|---|---|---|---|---|
| İşlenen Olaylar (olay/IoT Hub 'Ları için) | Sayı | Max, min, Sum | Olay Hub 'larından okunan ve küme tarafından işlenen toplam olay sayısı. Olaylar, reddedilen olaylara bölünür ve küme altyapısı tarafından kabul edilen olaylar. | EventStatus |
| Alma gecikmesi | Saniyeden | Ortalama, en fazla, en az | Sorgu için hazırlanana kadar, verilerin kümede alındığı zamandan itibaren alınan veri gecikmesi. Alım gecikmesi dönemi alma senaryosuna bağlıdır. | None |
| Alım sonucu | Sayı | Sayı | Başarısız ve başarılı olan alma işlemlerinin toplam sayısı. Başarı ve başarısızlık sonuçlarının demetlerini oluşturmak ve boyutları çözümlemek için **bölmeyi Uygula** ' yı kullanın (**değer** > **durumu**).| Inestionresultdetails |
| Alım birimi (MB) | Sayı | Maksimum, toplam | Sıkıştırmadan önce kümeye alınan verilerin toplam boyutu (MB cinsinden). | Database |
| | | | |  

### <a name="query-performance"></a>Sorgu performansı

Sorgu performans ölçümleri sorgu süresini ve toplam eşzamanlı veya kısıtlanmış sorgu sayısını izler.

**Ölçüm** | **Birim** | **Toplama** | **Ölçüm açıklaması** | **Boyutlarına** |
|---|---|---|---|---|
| Sorgu süresi | Milisaniye | Ort, min, Max, Sum | Sorgu sonuçları alınana kadar geçen toplam süre (ağ gecikmesi dahil değil). | QueryStatus |
| Toplam eşzamanlı sorgu sayısı | Sayı | Ort, Max, min, Sum | Kümede paralel olarak çalışan sorgu sayısı. Bu ölçüm, kümedeki yükü tahmin etmenin iyi bir yoludur. | None |
| Toplam kısıtlanmış sorgu sayısı | Sayı | Ort, Max, min, Sum | Kümedeki kısıtlanmış (reddedilen) sorguların sayısı. İzin verilen en fazla eşzamanlı (paralel) sorgu sayısı, eşzamanlı sorgu ilkesinde tanımlanmıştır. | None |
| | | | |

### <a name="streaming-ingest-metrics"></a>Akış alma ölçümleri

Akış alma ölçümleri akış alma verilerini ve istek hızını, süresini ve sonuçlarını izler.

**Ölçüm** | **Birim** | **Toplama** | **Ölçüm açıklaması** | **Boyutlarına** |
|---|---|---|---|---|
Akış alma verileri oranı |    Sayı   | Kterequestspersecond | Kümeye alınan toplam veri hacmi. | None |
Akış alma süresi   | Milisaniye  | Ortalama, en fazla, en az | Tüm akış alma isteklerinin toplam süresi. | None |
Akış alma Isteği oranı   | Sayı | Count, AVG, Max, min, Sum | Toplam akış alma isteği sayısı. | None |
Akış alma sonucu | Sayı | Ort   | Sonuç türüne göre akış alımı isteklerinin toplam sayısı. | Sonuç |
| | | | |

[Desteklenen Azure Veri Gezgini kümesi ölçümleri](/azure/azure-monitor/platform/metrics-supported#microsoftkustoclusters)hakkında ek bilgiler.


## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Veri Gezgini veri alma ve sorgu izleme](/azure/data-explorer/ingest-data-no-code)
* [Tanılama günlüklerini kullanarak Azure Veri Gezgini alma işlemlerini izleme](/azure/data-explorer/using-diagnostic-logs)
* [Hızlı başlangıç: Azure Veri Gezgini'ndeki verileri sorgulama](web-query-data.md)
