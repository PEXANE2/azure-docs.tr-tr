---
title: Ortamınızı Power BI Azure Time Series Insights bağlama | Microsoft Docs
description: Kuruluşunuz genelinde verileri paylaşmak, grafik ve görüntüleme Power BI Azure Time Series Insights nasıl bağlayacağınızı öğrenin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
services: time-series-insights
ms.service: time-series-insights
ms.topic: conceptual
ms.date: 06/01/2020
ms.openlocfilehash: ea46f37b0c09ca655b29ac3cfa2f168e18c85590
ms.sourcegitcommit: ff19f4ecaff33a414c0fa2d4c92542d6e91332f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "85052450"
---
# <a name="visualize-data-from-time-series-insights-in-power-bi"></a>Power BI Time Series Insights verileri görselleştirme

Azure Time Series Insights, bulutta zaman serisi verilerini depolamak, yönetmek, sorgulamak ve görselleştirmek için bir platformdur. [Power BI](https://powerbi.microsoft.com) , kuruluşunuz genelinde Öngörüler ve sonuçlar paylaşmanıza olanak sağlayan zengin görselleştirme özelliklerine sahip bir iş analizi aracıdır. Her iki hizmet de Time Series Insights ' devralınan görselleştirme özellikleri ve Power BI da en iyi şekilde yararlanmak üzere tümleştirilebilir.

Şunları öğrenirsiniz:

* Bulut bağlayıcısını kullanarak Power BI Time Series Insights bağlama
* Power BI verileriniz ile görseller oluşturun
* Power BI için raporu yayımlayın ve kuruluşunuzun geri kalanıyla paylaşabilirsiniz

Son olarak, Azure Time Series Insights aracılığıyla zaman serisi verileri görselleştirmeyi ve Power BI güçlü veri görselleştirmesi ve kolay paylaşım yeteneklerini nasıl geliştirebileceğinizi öğreneceksiniz.

Henüz yoksa, [ücretsiz bir Azure aboneliğine](https://azure.microsoft.com/free/) kaydolduğunuzdan emin olun.

## <a name="prerequisites"></a>Ön koşullar

* [Power BI Desktop](https://powerbi.microsoft.com/downloads/) en son sürümünü indirin ve yükleyin
* [Azure Time Series Insights Preview örneği](time-series-insights-update-how-to-manage.md) oluşturun veya oluşturun

> [!IMPORTANT]
> Power BI Bağlayıcısı, **Isınma mağazalarında**yapılandırılmış olan *kullandıkça öde* ortamlarında Time Series Insights önizleme sürümünde desteklenmektedir.

## <a name="connect-data-from-time-series-insights-to-power-bi"></a>Time Series Insights verileri Power BI 'ye bağlama

Time Series Insights ortamınızı Power BI bağlamak için şu adımları izleyin:

1. Time Series Insights Gezginini aç
1. Verileri sorgu veya ham veri olarak dışarı aktarma
1. Power BI Desktop açın
1. Özel sorgudan yükle

### <a name="export-data-into-power-bi-desktop"></a>Power BI Desktop 'a veri aktarma

Başlamak için:

1. Time Series Insights önizleme Gezginini açın ve verilerinizi seçin.
1. Memnun olduğunuz bir görünümü oluşturduktan sonra, **diğer eylemler** açılan menüsüne gidin ve **Power BI Bağlan**' ı seçin.

    [![Time Series Insights Preview Explorer dışarı aktarma](media/how-to-connect-power-bi/time-series-insights-export-option.png)](media/how-to-connect-power-bi/time-series-insights-export-option.png#lightbox)

1. Parametrelerinizi bu sekmenin içinde ayarlayın:

   1. Görüntülenecek göreli zaman dilimini belirtin. Mevcut görünümümüz varsa, bunu **var olan zaman dilimi**olarak bırakın.
   
   1. **Toplanan** ve **Ham olaylar**arasında seçim yapın. 
   
       > [!NOTE]
       > Verilerinizi Power BI daha sonra istediğiniz zaman toplayabilirsiniz, ancak toplandıktan sonra ham verilere dönemezsiniz. 
       
       > [!NOTE]
       > Ham olay düzeyi verileri için 100-K olay sayısı sınırı vardır.

       [![Bağlan](media/how-to-connect-power-bi/connect-to-power-bi.png)](media/how-to-connect-power-bi/connect-to-power-bi.png#lightbox)

   1. Time Series Insights örneğinizi **Isınma deposu**için yapılandırmadıysanız, bir uyarı alırsınız.

       [![Sıcak mağaza uyarısı](media/how-to-connect-power-bi/connect-to-power-bi-warning.png)](media/how-to-connect-power-bi/connect-to-power-bi-warning.png#lightbox)

       > [!TIP]
       > Var olan örneğinizi Azure portal **yarı bir depolama** için yapılandırabilirsiniz.

1. **Sorguyu panoya kopyala**' yı seçin.
1. Şimdi Power BI Desktop başlatın.
1. **Giriş** sekmesinde Power BI Desktop ' de, sol üst köşedeki **verileri al** ' ı seçin ve daha sonra **daha fazlasını**yapın.

    [![Giriş açılan kutusu](media/how-to-connect-power-bi/power-bi-home-drop-down.png)](media/how-to-connect-power-bi/power-bi-home-drop-down.png#lightbox)

1. **Time Series Insights**arayın, **Azure Time Series Insights (Beta)** öğesini seçin ve sonra **bağlanın**.

    [![Power BI Time Series Insights bağlama](media/how-to-connect-power-bi/connect-to-time-series-insights.png)](media/how-to-connect-power-bi/connect-to-time-series-insights.png#lightbox)

    Alternatif olarak, **Azure** sekmesine gidin, **Azure Time Series Insights (Beta)** öğesini seçin ve sonra **bağlantısını**yapın.
    
1. Üçüncü taraf kaynaklarına bağlanmak için izin isteyen bir ileti iletişim kutusu görüntülenir. **Devam**' ı seçin.

    [![Özel sorgu oluştur ' a tıklayın](media/how-to-connect-power-bi/confirm-the-connection.png)](media/how-to-connect-power-bi/confirm-the-connection.png#lightbox)

1. **Veri kaynağı**altındaki açılan menüde **özel sorgu oluştur**' u seçin. Panodan, aşağıdaki isteğe bağlı **özel sorgu (isteğe bağlı)** alanına yapıştırın ve sonra **Tamam**' a basın.

    [![Özel sorguyu geçirin ve Tamam ' ı seçin.](media/how-to-connect-power-bi/custom-query-load.png)](media/how-to-connect-power-bi/custom-query-load.png#lightbox)  

1. Veri tablosu şimdi yüklenir. Power BI yüklemek için **Yükle** ' ye basın.

    [![Tablodaki yüklenen verileri gözden geçirin ve Yükle ' yi seçin.](media/how-to-connect-power-bi/review-the-loaded-data-table.png)](media/how-to-connect-power-bi/review-the-loaded-data-table.png#lightbox)  

Bu adımları tamamladıysanız bir sonraki bölüme atlayın.

## <a name="create-a-report-with-visuals"></a>Görsel içeren rapor oluşturma

Verileri Power BI 'e aktardığınıza göre, görsellerin bulunduğu bir rapor oluşturmak biraz zaman alabilir.

1. Pencerenin sol tarafında, **rapor** görünümünü seçtiğinizden emin olun.

    [![Rapor görünümünü seçin](media/how-to-connect-power-bi/select-the-report-view.png)](media/how-to-connect-power-bi/select-the-report-view.png#lightbox)

1.  **Görsel öğeler** sütununda, istediğiniz görseli seçin. Örneğin, **çizgi grafik**' i seçin. Bu, Tuvalinize boş bir çizgi grafik ekler.

1.  **Alanlar** listesinde **_Timestamp** seçin ve öğeleri X ekseni üzerinde göstermek için **eksen** alanına sürükleyin. **Eksenin** değeri olarak **_Timestamp** ' ye geçdiğinizden emin olun (varsayılan **Tarih hiyerarşisi**).

    [![Rapor görünümünü seçin](media/how-to-connect-power-bi/select-timestamp.png)](media/how-to-connect-power-bi/select-timestamp.png#lightbox)

1.  Yine, **alanlar** listesinde **Timeseriesıd** öğesini seçin ve öğeleri Y ekseni üzerinde göstermek için **değerler** alanına sürükleyin.

    [![Çizgi grafik oluşturma](media/how-to-connect-power-bi/power-bi-line-chart.png)](media/how-to-connect-power-bi/power-bi-line-chart.png#lightbox)

1.  Tuvalinize başka bir grafik eklemek için, çizgi grafik dışında tuval üzerinde herhangi bir yeri seçin ve bu işlemi tekrarlayın.

    [![Paylaşılacak ek grafikler oluşturma](media/how-to-connect-power-bi/power-bi-additional-charts.png)](media/how-to-connect-power-bi/power-bi-additional-charts.png#lightbox)

Raporunuzu oluşturduktan sonra, Power BI Reporting Services 'a yayımlayabilirsiniz.

## <a name="advanced-editing"></a>Gelişmiş Düzenle

Power BI bir veri kümesini zaten yüklediyseniz ancak sorguyu değiştirmek istiyorsanız (Tarih/saat veya ortam KIMLIĞI parametreleri gibi), bunu Power BI Gelişmiş Düzenleyici işlevleri aracılığıyla yapabilirsiniz. Daha fazla bilgi edinmek için [Power BI belgelerine](https://docs.microsoft.com/power-bi/desktop-query-overview) bakın.

Genel bakış olarak:

1. Power BI Desktop, **sorguları Düzenle**' yi seçin.
1. **Gelişmiş Düzenleyici**tuşuna basın.

    [![Gelişmiş Düzenleyici sorguları düzenleme](media/how-to-connect-power-bi/power-bi-advanced-query-editing.png)](media/how-to-connect-power-bi/power-bi-advanced-query-editing.png#lightbox)

1. JSON yükünü istediğiniz gibi değiştirin.
1. **Bitti** ' yi seçin ve ardından & **Power Query Düzenleyicisi penceresinde**Uygula ' yı **kapatın** .

Arabirim artık uyguladığınız istenen değişiklikleri yansıtır.  

## <a name="next-steps"></a>Sonraki Adımlar

* Azure Time Series Insights için [Power BI bağlayıcı kavramlarını](https://docs.microsoft.com/power-bi/desktop-query-overview) okuyun.

* [Power BI Masaüstü](https://docs.microsoft.com/power-bi/desktop-query-overview)hakkında daha fazla bilgi edinin.

* [TIME SERIES INSIGHTS GA Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-quickstart) ve [Time Series Insights önizleme Gezgini](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart)' ni okuyun.
