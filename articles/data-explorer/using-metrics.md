---
title: Azure Veri Gezgini performansını, sistem durumu & kullanımını ölçümlerle izleme
description: Kümenin performansını, durumunu ve kullanımını izlemek için Azure Veri Gezgini ölçümlerini nasıl kullanacağınızı öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/19/2020
ms.openlocfilehash: 1319b8cd6ac8a0eb83381c24bcde9996458e47a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77560313"
---
# <a name="monitor-azure-data-explorer-performance-health-and-usage-with-metrics"></a>Azure Veri Gezgini performansını, sistem durumunu ve kullanımını ölçümlerle izleme

Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure Veri Gezgini'ni kullanmak için öncelikle bir küme ve bu kümenin içinde bir veya daha fazla veritabanı oluşturmanız gerekir. Ardından veritabanına veri alarak (yükleyerek) sorgu çalıştırabilirsiniz. Azure Veri Gezgini ölçümleri, küme kaynaklarının durumu ve performansı yla ilgili önemli göstergeler sağlar. Azure Veri Gezgini küme durumunu ve performansını belirli senaryonuzda tek başına ölçümler olarak izlemek için bu makalede ayrıntılı olarak kullanılan ölçümleri kullanın. Ölçümleri, operasyonel [Azure Panoları](/azure/azure-portal/azure-portal-dashboards) ve [Azure Uyarıları](/azure/azure-monitor/platform/alerts-metric-overview)için temel olarak da kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Hesabınız yoksa, ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/)oluşturabilirsiniz.
* Bir [küme ve veritabanı.](create-cluster-database-portal.md)

## <a name="using-metrics"></a>Ölçümleri kullanma

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Azure Veri Gezgini kümenizde, ölçümler bölmesini açmak ve kümenizde analize başlamak için **Ölçümler'i** seçin.
    ![Ölçümler'i](media/using-metrics/select-metrics.png)seçin.
1. Ölçüler bölmesinde: ![Ölçümler bölmesi](media/using-metrics/metrics-pane.png)
    1. Metrik grafik oluşturmak için **Metrik** adını ve metrik başına ilgili **Toplama'yı** seçin. **Kaynak** ve **Metrik Ad Alanı** toplayıcıları Azure Veri Gezgini kümeniz için önceden seçilir. Farklı ölçümler hakkında daha fazla bilgi için [desteklenen Azure Veri Gezgini ölçümlerine](#supported-azure-data-explorer-metrics)bakın.
    1. Aynı grafikte çizilen birden çok ölçümü görmek için **metrik ekle'yi** seçin.
    1. Tek bir görünümde birden çok grafiği görmek için **+ Yeni grafik** seçin.
    1. Zaman aralığını değiştirmek için zaman seçiciyi kullanın (varsayılan: son 24 saat).
    1. Boyutlara sahip ölçümler için [ **Filtre Ekle** ve **Uygula'yı** ](/azure/azure-monitor/platform/metrics-getting-started#apply-dimension-filters-and-splitting) kullanın.
    1. Grafik yapılandırmanızı yeniden görüntüleyebilmeniz için grafik yapılandırmanızı panolara eklemek için **panoya Pin'i** seçin.
    1. Belirlenen ölçütleri kullanarak ölçümlerinizi görselleştirmek için **Yeni uyarı kuralını** ayarlayın. Yeni uyarı kuralı, hedef kaynağınızı, metrik, bölme ve filtre boyutlarınızı grafiğinizden içerir. Bu ayarları [uyarı kuralı oluşturma bölmesinde değiştirin.](/azure/azure-monitor/platform/metrics-charts#create-alert-rules)

[Ölçümler Explorer'ı](/azure/azure-monitor/platform/metrics-getting-started)kullanma hakkında ek bilgiler.

## <a name="supported-azure-data-explorer-metrics"></a>Desteklenen Azure Veri Gezgini ölçümleri

Desteklenen Azure Veri Gezgini Ölçümleri kullanıma göre çeşitli kategorilere ayrılır. 

### <a name="cluster-health-metrics"></a>Küme durumu ölçümleri

Küme sistem durumu ölçümleri kümenin genel durumunu izler. Bu kaynak ve yutma kullanımı ve yanıt içerir.

**Ölçüm** | **Birim** | **Toplama** | **Metrik açıklama** | **Boyutlar** |
|---|---|---|---|---|
| Önbellek kullanımı | Yüzde | Avg, Max, Min | Küme tarafından kullanılmakta olan ayrılan önbellek kaynaklarının yüzdesi. Önbellek, tanımlanan önbellek ilkesine göre kullanıcı etkinliği için ayrılan SSD boyutudur. %80 veya daha az bir önbellek kullanımı, bir küme için sürdürülebilir bir durumdur. Ortalama önbellek kullanımı %80'in üzerindeyse, küme depolama için optimize edilmiş bir fiyatlandırma katmanına [ölçeklendirilmelidir](manage-cluster-vertical-scaling.md) veya daha fazla örneğe [ölçeklendirilmelidir.](manage-cluster-horizontal-scaling.md) Alternatif olarak, önbellek ilkesini (önbellekte daha az gün) uyarla. Önbellek kullanımı %100'ün üzerindeyse, önbelleğe alma ilkesine göre önbelleğe alınacak verilerin boyutu kümedeki toplam önbelleğin boyutundan daha büyüktür. | None |
| CPU | Yüzde | Avg, Max, Min | Kümedeki makineler tarafından kullanılmakta olan ayrılan işlem kaynaklarının yüzdesi. Bir küme için ortalama %80 veya daha az CPU sürdürülebilirdir. CPU'nun maksimum değeri %100'dür, bu da verileri işlemek için ek bilgi işlem kaynakları olmadığı anlamına gelir. Bir küme iyi performans gösterdiğinde, engellenen belirli CPU'lar olup olmadığını belirlemek için CPU'nun maksimum değerini denetleyin. | None |
| Yutma kullanımı | Yüzde | Avg, Max, Min | Satın alma gerçekleştirmek için kapasite ilkesinde ayrılan toplam kaynaktan veri yutmak için kullanılan gerçek kaynakların yüzdesi. Varsayılan kapasite ilkesi, 512 eşzamanlı yutma işleminden veya alıma yatırılan küme kaynaklarının %75'inden fazla değildir. Ortalama %80 veya daha az alım kullanımı bir küme için sürdürülebilir bir durumdur. Yutma kullanımının maksimum değeri %100'dür, bu da tüm kümeleme yeteneğinin kullanıldığı ve yutma kuyruğunun neden olabileceği anlamına gelir. | None |
| Canlı tutun | Sayı | Ort. | Kümenin yanıt verme yeteneğini izler. Tam yanıt veren bir küme değeri 1'i döndürür ve engellenmiş veya bağlantısı kesilen küme 0 döndürür. |
| Toplam daraltılmış komut sayısı | Sayı | Avg, Max, Min, Toplam | İzin verilen en fazla eşzamanlı (paralel) komut sayısına ulaşıldığından, kümedeki daraltılmış (reddedilen) komutların sayısı. | None |
| Toplam kapsam sayısı | Sayı | Avg, Max, Min, Toplam | Kümedeki toplam veri kapsamı sayısı. Bu metrikteki değişiklikler, veri kapsamlarını birleştirme CPU ağırlıklı bir etkinlik olduğundan, büyük veri yapısı değişiklikleri ve kümeüzerindeki yüksek yük anlamına gelebilir. | None |
| | | | |

### <a name="export-health-and-performance-metrics"></a>Sistem durumu ve performans ölçümlerini dışa aktarma

İhracat sağlık ve performans ölçümleri, gecikme, sonuç, kayıt sayısı ve kullanım gibi ihracat işlemlerinin genel sağlığını ve performansını izler.

**Ölçüm** | **Birim** | **Toplama** | **Metrik açıklama** | **Boyutlar** |
|---|---|---|---|---|
Dışa aktarılan kayıtların sürekli ihracat sayısı    | Sayı | Toplam | Tüm sürekli dışa aktarma işlerinde dışa aktarılan kayıtların sayısı. | None |
Sürekli ihracat max gecikmesi |    Sayı   | Maks   | Kümedeki sürekli dışa aktarım işleri tarafından bildirilen gecikme (dakika içinde). | None |
Sürekli ihracat bekleyen sayısı | Sayı | Maks   | Bekleyen sürekli dışa aktarma işlerinin sayısı. Bu işler çalışmaya hazırdır, ancak büyük olasılıkla yetersiz kapasite nedeniyle kuyrukta bekler). 
Sürekli ihracat sonucu    | Sayı |   Sayı   | Her sürekli dışa aktarım çalışmasının Hata/Başarı sonucu. | SürekliExportName |
İhracat kullanımı |    Yüzde | Maks   | Kullanılan dışa aktarma kapasitesi, kümedeki toplam ihracat kapasitesinin dışında (0 ile 100 arasında). | None |
| | | | |

### <a name="ingestion-health-and-performance-metrics"></a>Yutma durumu ve performans ölçümleri

Yutma sağlığı ve performans ölçümleri, gecikme, sonuç ve hacim gibi yutma işlemlerinin genel sağlığını ve performansını izler.

**Ölçüm** | **Birim** | **Toplama** | **Metrik açıklama** | **Boyutlar** |
|---|---|---|---|---|
| İşlenen olaylar (Olay/IoT Hub'ları için) | Sayı | Max, Min, Toplam | Olay merkezlerinden okunan ve küme tarafından işlenen toplam olay sayısı. Olaylar reddedilen olaylara ve küme altyapısı tarafından kabul edilen olaylara bölünür. | EventStatus |
| Yutma gecikmesi | Saniye | Avg, Max, Min | Verilerin kümede alındığı andan sorgu için hazır olana kadar, verilerin gecikme süresi. Yutma gecikme süresi yutma senaryosuna bağlıdır. | None |
| Yutma sonucu | Sayı | Sayı | Başarısız olan ve başarılı olan toplam yutma işlemi sayısı. Başarı kovaları oluşturmak ve sonuçları başarısız ve boyutları analiz etmek için **splitting uygulayın** **(Değer** > **Durumu)** kullanın.| Yutma Sonucu Ayrıntıları |
| Yutma hacmi (MB'de) | Sayı | Max, Toplam | Sıkıştırmadan önce kümeye (MB olarak) alınan toplam veri boyutu. | Database |
| | | | |  

### <a name="query-performance"></a>Sorgu performansı

Sorgu performans ölçümleri sorgu süresini ve eşzamanlı veya daraltılmış sorguların toplam sayısını izler.

**Ölçüm** | **Birim** | **Toplama** | **Metrik açıklama** | **Boyutlar** |
|---|---|---|---|---|
| Sorgu süresi | Milisaniye | Avg, Min, Max, Toplam | Sorgu sonuçları alınana kadar toplam süre (ağ gecikmesi içermez). | Sorgu Durumu |
| Toplam eşzamanlı sorgu sayısı | Sayı | Avg, Max, Min, Toplam | Kümede paralel olarak çalışan sorgu sayısı. Bu metrik küme üzerindeki yükü tahmin etmek için iyi bir yoldur. | None |
| Toplam daraltılmış sorgu sayısı | Sayı | Avg, Max, Min, Toplam | Kümedeki daraltılmış (reddedilen) sorguların sayısı. İzin verilen en fazla eşzamanlı (paralel) sorgu sayısı eşzamanlı sorgu ilkesinde tanımlanır. | None |
| | | | |

### <a name="streaming-ingest-metrics"></a>Akış ölçümleri

Akış ölçümleri akış veri ve istek oranı, süresi ve sonuçları izler.

**Ölçüm** | **Birim** | **Toplama** | **Metrik açıklama** | **Boyutlar** |
|---|---|---|---|---|
Veri Hızı Akış |    Sayı   | RateRequestsPerSecond | Kümeye alınan toplam veri hacmi. | None |
Akış Alma Süresi   | Milisaniye  | Avg, Max, Min | Tüm akış alma isteklerinin toplam süresi. | None |
Akış Alma İstek Oranı   | Sayı | Kont, Avg, Max, Min, Toplam | Toplam akış alma isteği sayısı. | None |
Akış Ingest Sonucu | Sayı | Ort.   | Sonuç türüne göre toplam akış alma isteği sayısı. | Sonuç |
| | | | |

[Desteklenen Azure Veri Gezgini küme ölçümleri](/azure/azure-monitor/platform/metrics-supported#microsoftkustoclusters)hakkında ek bilgiler.


## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Veri Gezgini'nde izleme verilerini alma ve sorgulama](/azure/data-explorer/ingest-data-no-code)
* [Tanılama günlüklerini kullanarak Azure Veri Gezgini işlemlerine izleme](/azure/data-explorer/using-diagnostic-logs)
* [Hızlı başlangıç: Azure Veri Gezgini'nde verileri sorgulama](web-query-data.md)
