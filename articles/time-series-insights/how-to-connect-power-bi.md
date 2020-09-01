---
title: Ortamınızı Power BI Azure Time Series Insights bağlama | Microsoft Docs
description: Kuruluşunuz genelinde verileri paylaşmak, grafik ve görüntüleme Power BI Azure Time Series Insights nasıl bağlayacağınızı öğrenin.
author: deepakpalled
ms.author: dpalled
manager: diviso
services: time-series-insights
ms.service: time-series-insights
ms.topic: conceptual
ms.date: 08/26/2020
ms.openlocfilehash: f15686cf07bae4aee41095c970cd8a474724c2ed
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89230985"
---
# <a name="visualize-data-from-azure-time-series-insights-in-power-bi"></a>Power BI Azure Time Series Insights verileri görselleştirme

Azure Time Series Insights, bulutta zaman serisi verilerini depolamak, yönetmek, sorgulamak ve görselleştirmek için bir platformdur. [Power BI](https://powerbi.microsoft.com) , kuruluşunuz genelinde Öngörüler ve sonuçlar paylaşmanıza olanak sağlayan zengin görselleştirme özelliklerine sahip bir iş analizi aracıdır. Her iki hizmet de Azure Time Series Insights ' devralınan görselleştirme özellikleri ve Power BI da en iyi şekilde yararlanmak üzere tümleştirilebilir.

Şunları öğrenirsiniz:

* Bulut bağlayıcısını kullanarak Power BI Azure Time Series Insights bağlama
* Power BI verileriniz ile görseller oluşturun
* Power BI için raporu yayımlayın ve kuruluşunuzun geri kalanıyla paylaşabilirsiniz

Son olarak, Azure Time Series Insights aracılığıyla zaman serisi verileri görselleştirmeyi ve Power BI güçlü veri görselleştirmesi ve kolay paylaşım yeteneklerini nasıl geliştirebileceğinizi öğreneceksiniz.

Henüz yoksa, [ücretsiz bir Azure aboneliğine](https://azure.microsoft.com/free/) kaydolduğunuzdan emin olun.

## <a name="prerequisites"></a>Ön koşullar

* [Power BI Desktop](https://powerbi.microsoft.com/downloads/) en son sürümünü indirin ve yükleyin
* [Azure Time Series Insights Gen2 ortamı](time-series-insights-update-how-to-manage.md) oluşturun veya oluşturun

> [!IMPORTANT]
>
> * Bağlayıcı şu an için **yalnızca ısınma depolarıyla**yapılandırılmış Azure Time Series Insights Gen2 ortamlarında desteklenir.
> * Başka bir Azure AD Kiracısından Azure Time Series Insights Gen2 ortamına Konuk erişiminiz varsa, bağlayıcıya erişemeyeceksiniz. [Ortam erişim ilkeleri](./concepts-access-policies.md)hakkında bilgi edinin.

## <a name="connect-data-from-azure-time-series-insights-to-power-bi"></a>Azure Time Series Insights verileri Power BI 'ye bağlama

Azure Time Series Insights ortamınızı Power BI bağlamak için şu adımları izleyin:

1. Azure Time Series Insights Gezginini aç
1. Verileri sorgu veya ham veri olarak dışarı aktarma
1. Power BI Desktop'ı açma
1. Özel sorgudan yükle

### <a name="export-data-into-power-bi-desktop"></a>Power BI Desktop 'a veri aktarma

Başlamak için:

1. Azure Time Series Insights Gezginini açın ve verilerinizi seçin.
1. Memnun olduğunuz bir görünümü oluşturduktan sonra, **diğer eylemler** açılan menüsüne gidin ve **Power BI Bağlan**' ı seçin.

    [![Gezgin dışarı aktarma Azure Time Series Insights](media/how-to-connect-power-bi/time-series-insights-export-option.png)](media/how-to-connect-power-bi/time-series-insights-export-option.png#lightbox)

1. Parametrelerinizi bu sekmenin içinde ayarlayın:

   1. Görüntülenecek göreli zaman dilimini belirtin. Mevcut görünümümüz varsa, bunu **var olan zaman dilimi**olarak bırakın.

   1. **Toplanan** ve **Ham olaylar**arasında seçim yapın.

       > [!NOTE]
       > Verilerinizi Power BI daha sonra istediğiniz zaman toplayabilirsiniz, ancak toplandıktan sonra ham verilere dönemezsiniz.

       > [!NOTE]
       > Ham olay düzeyi verileri için 250.000 olay sayısı sınırı vardır.

       [![Bağlan](media/how-to-connect-power-bi/connect-to-power-bi.png)](media/how-to-connect-power-bi/connect-to-power-bi.png#lightbox)

   1. Azure Time Series Insights ortamınızı **Isınma deposu**ile yapılandırmadıysanız bir uyarı alırsınız.

       [![Sıcak mağaza uyarısı](media/how-to-connect-power-bi/connect-to-power-bi-warning.png)](media/how-to-connect-power-bi/connect-to-power-bi-warning.png#lightbox)

       > [!TIP]
       > Var olan örneğinizi Azure portal **yarı bir depolama** için yapılandırabilirsiniz.

1. **Sorguyu panoya kopyala**' yı seçin.
1. Şimdi Power BI Desktop başlatın.
1. **Giriş** sekmesinde Power BI Desktop ' de, sol üst köşedeki **verileri al** ' ı seçin ve daha sonra **daha fazlasını**yapın.

    [![Giriş açılan kutusu](media/how-to-connect-power-bi/power-bi-home-drop-down.png)](media/how-to-connect-power-bi/power-bi-home-drop-down.png#lightbox)

1. **Azure Time Series Insights**arayın, **Azure Time Series Insights (Beta)** öğesini seçin ve sonra **bağlanın**.

    [![Power BI Azure Time Series Insights bağlama](media/how-to-connect-power-bi/connect-to-time-series-insights.png)](media/how-to-connect-power-bi/connect-to-time-series-insights.png#lightbox)

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

1. **Görsel öğeler** sütununda, istediğiniz görseli seçin. Örneğin, **çizgi grafik**' i seçin. Bu, Tuvalinize boş bir çizgi grafik ekler.

1. **Alanlar** listesinde **_Timestamp** seçin ve öğeleri X ekseni üzerinde göstermek için **eksen** alanına sürükleyin. **Eksenin** değeri olarak **_Timestamp** ' ye geçdiğinizden emin olun (varsayılan **Tarih hiyerarşisi**).

    [![Rapor görünümünü seçin](media/how-to-connect-power-bi/select-timestamp.png)](media/how-to-connect-power-bi/select-timestamp.png#lightbox)

1. Yine, **alanlar** listesinde **Timeseriesıd** öğesini seçin ve öğeleri Y ekseni üzerinde göstermek için **değerler** alanına sürükleyin.

    [![Çizgi grafik oluşturma](media/how-to-connect-power-bi/power-bi-line-chart.png)](media/how-to-connect-power-bi/power-bi-line-chart.png#lightbox)

1. Tuvalinize başka bir grafik eklemek için, çizgi grafik dışında tuval üzerinde herhangi bir yeri seçin ve bu işlemi tekrarlayın.

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
