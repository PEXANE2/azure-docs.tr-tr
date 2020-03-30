---
title: Explorer'ı kullanarak verileri keşfedin - Azure Time Series Insights | Microsoft Dokümanlar
description: IoT verilerinizi görüntülemek için Azure Zaman Serisi Öngörüler gözden kaçıklarını nasıl kullanacağınızı öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 01/15/2020
ms.custom: seodec18
ms.openlocfilehash: b8c2ba54fcc69ba126bf5f68aed99b25f8156155
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76046184"
---
# <a name="azure-time-series-insights-explorer"></a>Azure Zaman Serisi Öngörüler explorer

Bu makalede, Azure Zaman Serisi Öngörüler [explorer web uygulaması](https://insights.timeseries.azure.com/)için genel kullanılabilirlik özellikleri ve seçenekleri açıklanmaktadır. Zaman Serisi Öngörüleri gezgini, hizmet tarafından sağlanan güçlü veri görselleştirme özelliklerini gösterir ve kendi ortamınızda erişilebilir.

Azure Time Series Insights, milyarlarca IoT olayını aynı anda keşfedip analiz etmeyi kolaylaştıran ve tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmetidir. Verilerinizin küresel bir görünümünü sağlar ve bu da IoT çözümünüzü hızlı bir şekilde doğrulamanızı ve görev açısından kritik aygıtlara pahalıya mal olan kapalı kalma sürelerinden kaçınmanızı sağlar. Gizli eğilimleri keşfedebilir, anomalileri tespit edebilir ve kök neden analizlerini neredeyse gerçek zamanlı olarak gerçekleştirebilirsiniz. Zaman Serisi Öngörüleri gezgini şu anda genel önizlemede.

> [!TIP]
> Gösteri ortamında rehberli bir tur için [Azure Zaman Serisi Öngörüleri'ni hızlı bir şekilde başlatın.](time-series-quickstart.md)

## <a name="video"></a>Video

### <a name="learn-about-querying-data-by-using-the-time-series-insights-explorer-br"></a>Zaman Serisi Öngörüleri gezginini kullanarak verileri sorgulama hakkında bilgi edinin. </br>

> [!VIDEO https://www.youtube.com/embed/SHFPZvrR71s]

>[!NOTE]
>Bir önceki videoyu <a href="https://www.youtube.com/watch?v=6ehNf6AJkFo">oynatın "Azure IoT Çözüm Hızlandırıcısı kullanarak Time Series Insights ile başlarken."</a>

## <a name="prerequisites"></a>Ön koşullar

Time Series Insights explorer'ı kullanmadan önce şunları

- Zaman Serisi Öngörüleri ortamı oluşturun. Daha fazla bilgi için [Time Series Insights ile nasıl başlarılokunur'](./time-series-insights-get-started.md)okuyun.
- Hesabınıza ortamda [erişim sağlayın.](time-series-insights-data-access.md)
- Bir [IoT hub'ı](time-series-insights-how-to-add-an-event-source-iothub.md) veya [olay hub'ı](time-series-insights-how-to-add-an-event-source-eventhub.md) olay kaynağı ekleyin.

## <a name="explore-and-query-data"></a>Verileri keşfetme ve sorgulama

Etkinlik kaynağınızı Time Series Insights ortamınıza bağladıktan birkaç dakika sonra, zaman serisi verilerinizi keşfedebilir ve sorgulayabilirsiniz.

1. Başlamak için, Web tarayıcınızda [Time Series Insights explorer'ı](https://insights.timeseries.azure.com/) açın. Pencerenin sol tarafında bir ortam seçin. Erişiminiz olan tüm ortamlar alfabetik sıraya göre listelenir.

1. Bir ortam seçtikten sonra, en üstteki **"Anlardan** **Ve"** yapılandırmalarını kullanın veya istediğiniz zaman açıklığı üzerinde seçim yapıp sürükleyin. Sağ üst köşedeki büyüteç'i seçin veya seçili zaman başına sağ tıklayın ve **Ara'yı**seçin.

1. Ayrıca **Otomatik Açma** düğmesini seçerek kullanılabilirliği her dakika otomatik olarak yenileyebilirsiniz. **Otomatik Açma** düğmesi yalnızca ana görselleştirmenin içeriğine değil, kullanılabilirlik grafiğine uygulanır.

1. Azure bulut simgesi sizi Azure portalındaki ortamınıza götürür.

   [![Zaman Serisi Insights ortam seçimi](media/time-series-insights-explorer/tsi-ga-explorer-environments.png)](media/time-series-insights-explorer/tsi-ga-explorer-environments.png#lightbox)

1. Ardından, seçili zaman çizelgesi sırasında tüm olayların sayısını gösteren bir grafik görüntülenir. Burada bir dizi denetim var:

    - **Terimler Düzenleyici paneli**: Terim alanı, ortamınızı sorguladığınız yerdir. Ekranın sol tarafında bulunur:
      - **ÖLÇÜ**: Bu açılır liste tüm sayısal**sütunları**gösterir ( Çiftler).
      - **SPLIT BY**: Bu açılır liste kategorik sütunları (**Dizeleri)** gösterir.
      - Adım enterpolasyonunu etkinleştirebilir, minimum ve maksimum gösterebilirsiniz ve **MEASURE'ın**yanındaki kontrol panelinden y eksenini ayarlayabilirsiniz. Ayrıca, gösterilen verilerin bir sayı, ortalama veya verilerin toplamı olup olmadığını da ayarlayabilirsiniz.
      - Aynı x ekseninde görüntülemek için en fazla beş terim ekleyebilirsiniz. Yeni bir terim eklemek için **Ekle'yi** seçin veya varolan bir terimin kopyasını eklemek için **bu terim düğmesini** klon düğmesini kullanın.

        [![Terim seçimi, filtreleme ve sorgu paneli](media/time-series-insights-explorer/tsi-ga-explorer-add-or-clone.png)](media/time-series-insights-explorer/tsi-ga-explorer-add-or-clone.png#lightbox)

      - **Yüklem**: Aşağıdaki tabloda listelenen operands kümesini kullanarak olayları hızlı bir şekilde filtrelemek için yüklem kullanın. Seçerek veya tıklatarak bir arama yaparsanız, yüklem bu aramaya bağlı olarak otomatik olarak güncellenir. Desteklenen operand türleri şunlardır:

         |İşlem  |Desteklenen türler  |Notlar  |
         |---------|---------|---------|
         |**<**, **>**, **<=**, **>=**    |  **Çift**, **DateTime**, **TimeSpan**       |         |
         |**=**, **!=**,**<>**     | **String**, **Bool**, **Çift**, **DateTime**, **TimeSpan**, **NULL**        |         |
         |**Inç**     | **String**, **Bool**, **Çift**, **DateTime**, **TimeSpan**, **NULL**        |  Tüm operands aynı tip veya **NULL** sabit olmalıdır.        |
         |**Hsa**     | **Dize**        |  Sadece sabit dize literals sağ tarafta izin verilir. Boş dize ve **NULL** izin verilmez.       |

      - **Örnek sorgular**

         [![Örnek GA sorguları](media/time-series-insights-explorer/tsi-ga-example-queries.png)](media/time-series-insights-explorer/tsi-ga-example-queries.png#lightbox)

1. **Aralık Boyutu** kaydırıcısı aracını kullanarak aynı zaman aralığı üzerinde aralıkları yakınlaştırıp uzaklaştırabilirsiniz. Kaydırıcı, verilerinizin parçalı, yüksek çözünürlüklü kesimlerini görüntülemenize ve analiz edebilmenize olanak tanıyan milisaniye kadar küçük dilimlere kadar düzgün eğilimler gösteren büyük zaman dilimleri arasındaki hareketi daha hassas bir şekilde kontrol eder. Kaydırıcının varsayılan başlangıç noktası, seçiminizdeki verilerin en uygun görünümü olarak ayarlanır, çözünürlük, sorgu hızı ve parçalı.

1. **Zaman fırçası** aracı, bir zaman diliminden diğerine gezinmeyi kolaylaştırır.

1. Geçerli sorgunuzu kaydetmek ve ortamın diğer kullanıcılarıyla paylaşmak için **Kaydet** simgesini seçin. **Aç** simgesini seçtiğinizde, kayıtlı tüm sorgularınızı ve erişebildiğiniz ortamlarda diğer kullanıcıların paylaşılan sorgularını gözden geçirebilirsiniz.

   [![Sorgular](media/time-series-insights-explorer/tsi-ga-explorer-saved-queries.png)](media/time-series-insights-explorer/tsi-ga-explorer-saved-queries.png#lightbox)

## <a name="visualize-data"></a>Verileri görselleştirme

1. En fazla dört benzersiz sorgunun eşzamanlı görünümü için **Perspektif Görünümü** aracını kullanın. **Perspektif Görünümü** düğmesi grafiğin sağ üst köşesindedir.

   [![Perspektif bölmesine eklemek için sorguları seçme](media/time-series-insights-explorer/tsi-ga-explorer-perspective-panes.png)](media/time-series-insights-explorer/tsi-ga-explorer-perspective-panes.png#lightbox)

1. Verilerinizi görsel olarak keşfetmek için bir grafik görüntüleyin ve **Grafik** araçlarını kullanın:

    - Belirli bir zaman açıklığı veya tek bir veri serisini **seçin** veya **tıklatın.**
    - Bir zaman alanı seçimi nde, olayları yakınlaştırabilir veya keşfedebilirsiniz.
    - Bir veri serisi içinde, seriyi başka bir sütuna bölebilir, seriyi yeni bir terim olarak ekleyebilir, yalnızca seçilen seriyi gösterebilir, seçilen seriyi hariç tutabilir, bu serileri pingleyebilir veya seçilen serideki olayları keşfedebilirsiniz.
    - Grafiğin solundaki filtre alanında, görüntülenen tüm veri serilerini gözden geçirebilir ve değere veya ada göre yeniden sıralayabilirsiniz. Ayrıca, tüm veri serilerini veya sabitlenmiş veya sabitlenmemiş serileri de görüntüleyebilirsiniz. Tek bir veri serisi seçebilir ve seriyi başka bir sütuna bölebilir, seriyi yeni bir terim olarak ekleyebilir, yalnızca seçilen seriyi gösterebilir, seçilen seriyi hariç tutabilir, bu seriyi pinleyebilir veya seçilen serideki olayları keşfedebilirsiniz.
    - Aynı anda birden çok terim görüntülediğinizde, bir veri serisi yle ilgili ek verileri yığınlayabilir, boşaltabilir, gözden geçirebilir ve tüm terimler de aynı y eksenini kullanabilirsiniz. Grafiğin sağ üst köşesindeki düğmeleri kullanın.

    [![Grafik aracı sağ üst köşe seçeneği ayarları](media/time-series-insights-explorer/tsi-ga-example-chart-options.png)](media/time-series-insights-explorer/tsi-ga-example-chart-options.png#lightbox)

1. Belirli bir sorgudaki benzersiz veya anormal veri serilerini hızla tespit etmek için **ısı haritasını** kullanın. Yalnızca bir arama terimi ısı haritası olarak görüntülenebilir.

    [![GA explorer ısı haritası grafik](media/time-series-insights-explorer/tsi-ga-example-heatmap-charting.png)](media/time-series-insights-explorer/tsi-ga-example-heatmap-charting.png#lightbox)

1. Etkinlikleri seçerek veya sağ tıklatarak keşfettiğiniz zaman, **EVENTS** paneli kullanıma sunulmuştur. Burada, tüm ham etkinliklerigözden geçirebilirsiniz ve JSON veya CSV dosyaları olarak olayları dışa aktarAbilirsiniz. Time Series Insights tüm ham verileri saklar.

    [![Olaylar](media/time-series-insights-explorer/tsi-ga-explorer-events-panel.png)](media/time-series-insights-explorer/tsi-ga-explorer-events-panel.png#lightbox)

1. Desenleri ve sütun istatistiklerini ortaya çıkarmak için olayları keşfettikten sonra **STATS** sekmesini seçin.

    - **Desenler**: Bu özellik, seçili veri bölgesindeki istatistiksel olarak en önemli desenleri proaktif olarak ortaya çıkar. Hangi örüntülerin en fazla zaman ve enerji gerektirdiğini anlamak için binlerce olaya bakmanız gerekmez. Time Series Insights ile, bir analiz yapmaya devam etmek için doğrudan bu istatistiksel olarak anlamlı desenlere atlayabilirsiniz. Bu özellik, geçmiş verilere yönelik ölüm sonrası araştırmalar için de yararlıdır.
    - **Sütun İstatistikleri**: Sütun istatistikleri, seçili veri serisinin her sütunundaki verileri seçili zaman çizelgesi üzerinden ayıran grafikler ve tablolar sağlar.

      [![STATS sütun grafiği ve seçenekleri](media/time-series-insights-explorer/tsi-ga-explorer-stat-column.png)](media/time-series-insights-explorer/tsi-ga-explorer-stat-column.png#lightbox)

Şimdi Zaman Serisi Öngörüler explorer web uygulamasında bulunan temel özellikler, yapılandırma ayarları ve görüntüleme seçenekleri hakkında bilgi edinesiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Time Series Insights ortamınızdaki sorunları nasıl [tanılayıp çözeceğinizi](time-series-insights-diagnose-and-solve-problems.md) öğrenin.

- Rehberli [Azure Time Series Insights hızlı başlat](time-series-quickstart.md) turuna katılın.
