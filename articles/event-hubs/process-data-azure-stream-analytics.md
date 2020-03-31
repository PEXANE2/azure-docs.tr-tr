---
title: Akış Analizini kullanarak Event Hubs Azure'dan verileri işleme | Microsoft Dokümanlar
description: Bu makalede, Azure Akış Analizi işini kullanarak Azure etkinlik merkezinizdeki verileri nasıl işlediğiniz ilerler.
services: event-hubs
author: spelluru
manager: ''
ms.author: spelluru
ms.date: 07/09/2019
ms.topic: article
ms.service: event-hubs
ms.openlocfilehash: 531426656fe833752c9c4685688c00de3894895b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69991940"
---
# <a name="process-data-from-your-event-hub-using-azure-stream-analytics"></a>Azure Akış Analizi'ni kullanarak etkinlik merkezinizdeki verileri işleme 
Azure Akış Analizi hizmeti, Azure Etkinlik Hub'larından akış verilerini yutmayı, işlemeyi ve analiz etmeyi kolaylaştırarak gerçek zamanlı eylemleri yönlendirecek güçlü öngörüler sağlar. Bu tümleştirme, hızlı bir şekilde bir sıcak yol analiz ardışık alanı oluşturmanıza olanak sağlar. Gelen verileri görselleştirmek ve bir Akış Analizi sorgusu yazmak için Azure portalını kullanabilirsiniz. Sorgunuz hazır olduğunda, yalnızca birkaç tıklamayla bunu üretime taşıyabilirsiniz. 

## <a name="key-benefits"></a>Önemli avantajlar
Azure Etkinlik Hub'ları ve Azure Akış Analizi entegrasyonunun temel avantajları şunlardır: 
- **Önizleme verileri** – Azure portalındaki bir etkinlik merkezinden gelen verileri önizleyebilirsiniz.
- **Sorgunuzu test edin** – Bir dönüşüm sorgusu hazırlayın ve doğrudan Azure portalında test edin. Sorgu dili sözdizimi için [Akış Analizi Sorgu Dili](/stream-analytics-query/built-in-functions-azure-stream-analytics) belgelerine bakın.
- **Sorgunuzu üretime dağıtın** – Bir Azure Akışı Analizi işi oluşturup başlatarak sorguyu üretime dağıtabilirsiniz.

## <a name="end-to-end-flow"></a>Uçuça akış

1. [Azure portalında](https://portal.azure.com)oturum açın. 
1. **Olay Hub'larınız ad alanına** gidin ve ardından gelen verilere sahip olan olay **hub'ına**gidin. 
1. Etkinlik merkezi sayfasında **İşlem Verileri'ni** seçin.  

    ![İşlem veri döşemesi](./media/process-data-azure-stream-analytics/process-data-tile.png)
1. **Etkinlik döşemesinden gerçek zamanlı öngörüleri etkinleştir'i** **incele'yi** seçin. 

    ![Akış Analizi'ni seçin](./media/process-data-azure-stream-analytics/process-data-page-explore-stream-analytics.png)
1. Aşağıdaki alanlar için önceden ayarlanmış değerlere sahip bir sorgu sayfası görürsünüz:
    1. Sorgu için bir giriş olarak **olay hub'ınız.**
    1. SELECT deyimi ile örnek **SQL sorgusu.** 
    1. Sorgu test sonuçlarınıza başvurmak için bir **çıktı** diğer adı. 

        ![Sorgu düzenleyicisi](./media/process-data-azure-stream-analytics/query-editor.png)
        
        > [!NOTE]
        >  Bu özelliği ilk kez kullandığınızda, bu sayfa, bir tüketici grubu oluşturmak için izninizi ve olay merkezinizin gelen verileri önizlemesi için bir ilke ister.
1. Önceki resimde gösterildiği gibi **Giriş önizleme** bölmesinde **Oluştur'u** seçin. 
1. Bu sekmede en son gelen verilerin anlık görüntüsünü hemen görürsünüz.
    - Verilerinizdeki serileştirme türü otomatik olarak algılanır (JSON/CSV). El ile JSON/CSV/AVRO olarak da değiştirebilirsiniz.
    - Gelen verileri tablo biçiminde veya ham formatta önizleyebilirsiniz. 
    - Gösterilen verileriniz güncel değilse, en son olayları görmek için **Yenile'yi** seçin. 

        Tablo **biçimindeki**veri örneği : ![Tablo biçimindeki sonuçlar](./media/process-data-azure-stream-analytics/snapshot-results.png)

        Burada **ham formatında**veri bir örnektir: 

        ![Ham formatta sonuçlar](./media/process-data-azure-stream-analytics/snapshot-results-raw-format.png)
1. **Test sonuçları** sekmesinde sorgunuzun test sonuçlarının anlık görüntüsünü görmek için **Test sorgusunu** seçin. Sonuçları da indirebilirsiniz.

    ![Test sorgu sonuçları](./media/process-data-azure-stream-analytics/test-results.png)
1. Verileri dönüştürmek için kendi sorgunuzu yazın. Bkz. [Akış Analitiği Sorgu Dili başvurusu.](/stream-analytics-query/stream-analytics-query-language-reference)
1. Sorguyu sınadıktan ve üretime taşımak istediğinizde **sorguyu dağıt'ı**seçin. Sorguyu dağıtmak için, işiniz için bir çıktı ayarlayıp işe başlayabildiğiniz bir Azure Akışı Analizi işi oluşturun. Akış Analizi işi oluşturmak için, iş için bir ad belirtin ve **Oluştur'u**seçin.

      ![Azure Stream Analytics işi oluşturma](./media/process-data-azure-stream-analytics/create-stream-analytics-job.png)

      > [!NOTE] 
      >  Etkinlik Hub'ları sayfasındaoluşturduğunuz her yeni Azure Akışı Analizi işi için bir tüketici grubu ve bir ilke oluşturmanızı öneririz. Tüketici grupları yalnızca beş eşzamanlı okuyucuya izin verir, bu nedenle her iş için özel bir tüketici grubu sağlamak, bu sınırı aşmaktan doğabilecek hataları önler. Özel bir ilke, anahtarınızı döndürmenize veya diğer kaynakları etkilemeden izinleri iptal etmenizi sağlar. 
1. Akış Analizi işiniz artık sorgunuzun test ettiğinizle aynı olduğu ve girişin olay hub'ınız olduğu bir yerde oluşturulur. 

9.  Ardışık mayı suçlamak için sorgunun üstesini ayarlayın ve işi başlatmak için **Başla'** yı sEçin. **output**

    > [!NOTE]
    > İşe başlamadan önce, Azure Akış Analizi'nde oluşturduğunuz çıktı adı ile çıktı alialarını değiştirmeyi unutmayın.

      ![Çıktıyı ayarlayın ve işe başlayın](./media/process-data-azure-stream-analytics/set-output-start-job.png)


## <a name="known-limitations"></a>Bilinen sınırlamalar
Sorgunuzu sınarken, test sonuçlarının yüklenmesi yaklaşık 6 saniye sürer. Test performansını artırmak için çalışıyoruz. Ancak, üretimde dağıtıldığında, Azure Akışı Analitiği'nin ikinci alt gecikmesi olacaktır.

## <a name="streaming-units"></a>Akış birimleri
Azure Akış Analizi işiniz varsayılan olarak üç akış birimine (SUS) bağlanır. Bu ayarı ayarlamak için, Azure portalındaki **Akış Analizi iş** sayfasında sol menüdeki **Ölçek'i** seçin. Akış birimleri hakkında daha fazla bilgi edinmek için Akış [Birimlerini Anla ve Ayarla'yı 'na](../stream-analytics/stream-analytics-streaming-unit-consumption.md)bakın.

![Ölçek akış birimleri](./media/process-data-azure-stream-analytics/scale.png)

## <a name="next-steps"></a>Sonraki adımlar
Akış Analizi sorguları hakkında daha fazla bilgi edinmek için [Stream Analytics Sorgu Dili](/stream-analytics-query/built-in-functions-azure-stream-analytics) bölümüne bakın
