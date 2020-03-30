---
title: Çevrenizi Power BI ' ye bağlayın - Azure Time Series Öngörüleri | Microsoft Dokümanlar
description: Verileri kuruluşunuz genelinde paylaşmak, grafiklemek ve görüntülemek için Azure Time Series Öngörülerini Power BI'ye nasıl bağlayabilirsiniz öğrenin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
services: time-series-insights
ms.service: time-series-insights
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: 22053bdc3a9836b76aa92303234a095cac6448ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75863851"
---
# <a name="visualize-data-from-time-series-insights-in-power-bi"></a>Power BI'de Time Series Insights'tan verileri görselleştirin

Azure Time Series Öngörüleri, zaman serisi verilerini bulutta depolamak, yönetmek, sorgulamak ve görselleştirmek için bir platformdur. [Power BI,](https://powerbi.microsoft.com) kuruluşunuz genelinde öngörüleri ve sonuçları paylaşmanızı sağlayan zengin görselleştirme özelliklerine sahip bir iş analitiği aracıdır. Her iki hizmet de artık Time Series Insights'ın doğal görselleştirme yeteneklerinin yanı sıra Power BI'lerin en iyi özelliklerini elde etmek için entegre edilebilir.

Şunları öğrenirsiniz:

* Bulut konektörünü kullanarak Zaman Serisi Öngörülerini Power BI'ye bağlayın
* Power BI'de verilerinizle görseller oluşturun
* Raporu Power BI'ye yayınlayın ve kuruluşunuzun geri kalanıyla paylaşın

Sonunda, Zaman serisi verileri Azure Time Series Insights aracılığıyla nasıl görselleştireceğinizi ve Power BI'nin güçlü veri görselleştirmesi ve kolay paylaşım özellikleriyle nasıl geliştireceğinizi öğreneceksiniz.

Zaten bir Azure aboneliğiniz yoksa ücretsiz bir [Azure aboneliğine](https://azure.microsoft.com/free/) kaydolduğunuzdan emin olun.

## <a name="prerequisites"></a>Ön koşullar

* [Power BI Desktop'ın](https://powerbi.microsoft.com/downloads/) en son sürümünü indirin ve kurun
* Azure Zaman [Serisi Öngörüleri Önizleme örneğine](time-series-insights-update-how-to-manage.md) sahip olun veya oluşturun

> [!IMPORTANT]
> Power BI konektörü şu anda Zaman Serisi Öngörüleri Önizleme Sıcak **Mağaza**için yapılandırılan *bir anda gidilen ödeme* ortamlarında desteklenir.

## <a name="connect-data-from-time-series-insights-to-power-bi"></a>Zaman Serisi Öngörüleri'nden Power BI'ye veri bağlama

Time Series Öngörüleri ortamınızı Power BI'ye bağlamak için aşağıdaki adımları izleyin:

1. Açık Zaman Serisi Insights Explorer
1. Verileri sorgu veya ham veri olarak dışa aktarma
1. Açık Güç BI Masaüstü
1. Özel Sorgudan Yük

### <a name="export-data-into-power-bi-desktop"></a>Verileri Power BI masaüstüne aktarma

Başlamak için:

1. Zaman Serisi Öngörüler Önizleme Gezgini'ni açın ve verilerinizi yeniden düzenlemek.
1. Memnun olduğunuz bir görünüm oluşturduktan sonra, **Daha fazla eylem** açılır menüsüne gidin ve **Power BI'ye Bağlan'ı**seçin.

    [![Zaman Serisi Öngörüler Önizleme Explorer dışa aktarma](media/how-to-connect-power-bi/time-series-insights-export-option.png)](media/how-to-connect-power-bi/time-series-insights-export-option.png#lightbox)

1. Parametrelerinizi bu sekmede ayarlayın:

   1. Görüntülemek için göreceli bir zaman dilimi belirtin. Mevcut görünümünüzden memnunsanız, bunu **Varolan zaman dilimi**olarak bırakın.
   
   1. **Toplu** ve **Ham Etkinlikler**arasında seçim yapın. 
   
       > [!NOTE]
       > Verilerinizi daha sonra Power BI'de her zaman toplayabilirsiniz, ancak toplama dan sonra ham verilere geri dönemezsiniz. 
       
       > [!NOTE]
       > Raw Event düzeyi verileri için 100-K olay sayısı sınırı vardır.

       [![Bağlamak](media/how-to-connect-power-bi/connect-to-power-bi.png)](media/how-to-connect-power-bi/connect-to-power-bi.png#lightbox)

   1. Zaman Serisi Öngörüleri örneğini **Warm Store**için yapılandırmadıysanız, bir uyarı alırsınız.

       [![Sıcak mağaza uyarısı](media/how-to-connect-power-bi/connect-to-power-bi-warning.png)](media/how-to-connect-power-bi/connect-to-power-bi-warning.png#lightbox)

       > [!TIP]
       > Mevcut örneğini Azure portalında **Warm Store** için yapılandırabilirsiniz.

1. **Panoya Sorgula'yı**seçin.
1. Şimdi, Power BI Desktop'ı başlatın.
1. **Ana sayfa** sekmesindeki Power BI Desktop'da sol üst köşede **Veri Al'ı** ve ardından **Daha Fazla'yı**seçin.

    [![Ev açılır](media/how-to-connect-power-bi/power-bi-home-drop-down.png)](media/how-to-connect-power-bi/power-bi-home-drop-down.png#lightbox)

1. Zaman **Serisi Öngörüleri**arayın, **Azure Zaman Serisi Öngörüleri 'ni (Beta)** seçin, ardından **bağlanın.**

    [![Power BI'yi Time Series Öngörülerine Bağlayın](media/how-to-connect-power-bi/connect-to-time-series-insights.png)](media/how-to-connect-power-bi/connect-to-time-series-insights.png#lightbox)

    Alternatif olarak, **Azure** sekmesine gidin, **Azure Zaman Serisi Öngörüleri 'ni (Beta)** seçin ve ardından **Bağlanın.**
    
1. İleti iletişim kutusu, üçüncü taraf kaynaklarına bağlanmak için izin isteyen görüntüler. **Devam'ı**seçin.

    [![Özel Sorgu Oluştur'u Seçin](media/how-to-connect-power-bi/confirm-the-connection.png)](media/how-to-connect-power-bi/confirm-the-connection.png#lightbox)

1. **Veri Kaynağı'nın**altındaki açılır menüde özel **sorgu oluştur'u**seçin. Panonuzdan aşağıdaki isteğe bağlı **Özel Sorgu (isteğe bağlı)** alanına yapıştırın ve **tamam tuşuna**basın.

    [![Özel sorguda geçirin ve Tamam'ı seçin](media/how-to-connect-power-bi/custom-query-load.png)](media/how-to-connect-power-bi/custom-query-load.png#lightbox)  

1. Veri tablosu şimdi yüklenir. Power BI'ye yüklemek için **Yük** tuşuna basın.

    [![Tablodaki yüklenen verileri gözden geçirin ve Yükle'yi seçin](media/how-to-connect-power-bi/review-the-loaded-data-table.png)](media/how-to-connect-power-bi/review-the-loaded-data-table.png#lightbox)  

Bu adımları tamamladıysanız, bir sonraki bölüme geçin.

## <a name="create-a-report-with-visuals"></a>Görsel içeren rapor oluşturma

Artık verileri Power BI'ye aktardığınıza göre, görsellerle birlikte bir rapor oluşturmanın zamanı gelmiştir.

1. Pencerenin sol tarafında, **Rapor** görünümünü seçtiğinizden emin olun.

    [![Rapor Görünümü'nü seçin](media/how-to-connect-power-bi/select-the-report-view.png)](media/how-to-connect-power-bi/select-the-report-view.png#lightbox)

1.  **Görseller** sütununda, seçtiğiniz görseli seçin. Örneğin, **Satır grafiğini**seçin. Bu, tuvalinize boş bir çizgi grafiği ekler.
 
1.  **Alanlar** listesinde, **Zaman Damgası'nı** seçin ve öğeleri X ekseni boyunca görüntülemek için **Eksen** alanına sürükleyin.

1.  Yine, **Alanlar** listesinde **TimeSeriesId'i** seçin ve öğeleri Y ekseni boyunca görüntülemek için **Değerler** alanına sürükleyin.

    [![Çizgi grafik oluşturma](media/how-to-connect-power-bi/power-bi-line-chart.png)](media/how-to-connect-power-bi/power-bi-line-chart.png#lightbox)

1.  Tuvalinize başka bir grafik eklemek için, grafik dışında tuvalüzerinde herhangi bir yeri seçin ve bu işlemi yineleyin.

    [![Paylaşmak için ek grafikler oluşturma](media/how-to-connect-power-bi/power-bi-additional-charts.png)](media/how-to-connect-power-bi/power-bi-additional-charts.png#lightbox)

Raporunuzu oluşturduktan sonra, raporuPower BI Reporting Services'da yayınlayabilirsiniz.

## <a name="advanced-editing"></a>Gelişmiş düzenleme

Power BI'de zaten bir veri kümesi yüklediyseniz ancak sorguyu değiştirmek istiyorsanız (tarih/saat veya Çevre Kimliği parametreleri gibi), bunu Power BI'nin Gelişmiş Düzenleyici işlevi aracılığıyla yapabilirsiniz. Daha fazla bilgi edinmek için [Power BI belgelerine](https://docs.microsoft.com/power-bi/desktop-query-overview) bakın.

Genel bir bakış olarak:

1. Power BI Masaüstünde **Sorguları Edit'i**seçin.
1. **İleri Editör**basın .

    [![Gelişmiş Düzenleyici'de sorguları edin](media/how-to-connect-power-bi/power-bi-advanced-query-editing.png)](media/how-to-connect-power-bi/power-bi-advanced-query-editing.png#lightbox)

1. JSON yükünü istediğiniz gibi değiştirin.
1. **Bitti'yi** seçin ve ardından **Power Query Düzenleyicisi Penceresinde** **uygula&'yi kapatın.**

Arabirim artık uyguladığınız istediğiniz değişiklikleri yansıtacaktır.  

## <a name="next-steps"></a>Sonraki Adımlar

* Azure Time Series Insights için [Power BI konektör kavramları](https://docs.microsoft.com/power-bi/desktop-query-overview) hakkında bilgi edinin.

* [Power BI masaüstü hakkında](https://docs.microsoft.com/power-bi/desktop-query-overview)daha fazla bilgi edinin.

* Oku [Time Serisi Insights GA Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-quickstart) ve Zaman Serisi [Insights Önizleme Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart).
