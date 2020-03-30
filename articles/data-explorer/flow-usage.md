---
title: Microsoft Azure Veri Gezgini Akış bağlayıcısı (Önizleme) kullanım örnekleri
description: Bazı yaygın Microsoft Flow bağlayıcısı kullanım örneklerini öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: dorcohen
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/15/2020
ms.openlocfilehash: 759ea9800a0ed051fed887adfb10b06f7a53b72f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79529212"
---
# <a name="microsoft-flow-connector-preview-usage-examples"></a>Microsoft Flow bağlayıcısı (Önizleme) kullanım örnekleri

Azure Veri Gezgini akış bağlayıcısı, Azure Veri Gezgini'nin, Planlanan veya tetiklenen bir görevin parçası olarak Kusto sorgularını ve komutlarını otomatik olarak çalıştırmak için [Microsoft Power Automate'in Akış özelliklerini](https://flow.microsoft.com/) kullanmasına olanak tanır. Bu belge, birkaç yaygın Microsoft Flow bağlayıcısı kullanım örneği içerir.

Daha fazla bilgi için [Microsoft Flow bağlayıcısı (Önizleme)](flow.md)bakın.

* [Microsoft Flow konektörü ve SQL](#microsoft-flow-connector-and-sql)
* [Verileri Power BI veri kümesine itme](#push-data-to-power-bi-dataset)
* [Koşullu sorgular](#conditional-queries)
* [Birden çok Azure Veri Gezgini Akış grafiğini e-postayla gönder](#email-multiple-azure-data-explorer-flow-charts)
* [Farklı kişilere farklı bir e-posta gönderme](#send-a-different-email-to-different-contacts)
* [Özel bir HTML tablosu oluşturma](#create-a-custom-html-table)

## <a name="microsoft-flow-connector-and-sql"></a>Microsoft Flow konektörü ve SQL

Verilerinizi sorgulamak ve bir SQL veritabanında toplamak için Microsoft Flow bağlayıcısını kullanın.

> [!Note]
> SQL eklemesi her satır için ayrı ayrı yapılır. Microsoft Flow bağlayıcısını yalnızca küçük miktarda çıktı verisi için kullanın. 

![](./media/flow-usage/flow-sqlexample.png)

> [!IMPORTANT]
> Küme *Adı* alanına küme URL'sini girin.

## <a name="push-data-to-power-bi-dataset"></a>Verileri Power BI veri kümesine itme

Microsoft Flow bağlayıcısı, Kusto sorgularından Power BI akış veri kümelerine veri itmek için Power BI bağlayıcısıyla birlikte kullanılabilir.

1. Yeni bir Çalıştır sorgusu oluşturun ve sonuç eylemini listeleyin.
1. **Yeni adım**'ı seçin.
1. **Eylem ekle** ve Power BI'yi ara'yı seçin.
1. **Power BI'yi**seçin.
1. **Veri kümesine satır ekle'yi**seçin. 

    ![Akış Gücü BI konektörü](./media/flow-usage/flow-powerbiconnector.png)
1. Verilerin itildiği **Çalışma Alanı,** **Veri Kümesi**ve **Tablo'yu** girin.
1. Dinamik içerik iletişim kutusundan, veri kümesi şemasını ve ilgili Kusto sorgu sonuçlarını içeren bir Taşıma Yükü ekleyin.

    ![Akış Gücü BI alanları](./media/flow-usage/flow-powerbifields.png)

Akış, Kusto sorgu sonuç tablosunun her satırı için Power BI eylemini otomatik olarak uygular. 

![Her satır için Akış Gücü BI eylem](./media/flow-usage/flow-powerbiforeach.png)

## <a name="conditional-queries"></a>Koşullu sorgular

Kusto sorgularının sonuçları, bir sonraki akış eylemleri için giriş veya koşul olarak kullanılabilir.

Aşağıdaki örnekte, son gün içinde meydana gelen olaylar için Kusto'yu sorgularız. Çözülen her olay için bir bolluk iletisi deftere nakledilir ve anında iletme bildirimi oluşturulur.
Hala etkin olan her olay için, Kusto benzer olaylar hakkında daha fazla bilgi için sorgulanır. Bu bilgileri e-posta olarak gönderir ve ilgili bir TFS görevini açar.

Benzer bir Akış oluşturmak için aşağıdaki yönergeleri izleyin:

1. Yeni bir Çalıştır sorgusu oluşturun ve sonuç eylemini listeleyin.
1. **Yeni adım**'ı seçin.
1. **Koşul denetimini**seçin.
1. Dinamik içerik penceresinden, sonraki eylemler için koşul olarak kullanmak istediğiniz parametreyi seçin.
1. Verilen parametrede belirli bir koşul ayarlamak için *İlişki* ve *Değer* türünü seçin.

    [![](./media/flow-usage/flow-condition.png "Flow conditions")](./media/flow-usage/flow-condition.png#lightbox)

    Akış, sorgu sonuç tablosunun her satırında bu koşulu uygular.
1. Koşul doğru ve yanlış olduğunda için eylemler ekleyin.

    [![](./media/flow-usage/flow-conditionactions.png "Flow condition actions")](./media/flow-usage/flow-conditionactions.png#lightbox)

Kusto sorgusundaki sonuç değerlerini sonraki eylemler için giriş olarak kullanabilirsiniz. Dinamik içerik penceresinden sonuç değerlerini seçin.
Aşağıdaki örnekte, Bir Bolluk - İleti Sonrası eylem ve Visual Studio - Kusto sorgusundan verileri içeren yeni bir iş öğesi eylemi oluşturma eklendi.

![Bolluk - İleti gönder eylemi](./media/flow-usage/flow-slack.png)

![Visual Studio aksiyon](./media/flow-usage/flow-visualstudio.png)

Bu örnekte, bir olay hala etkinse, aynı kaynaktan gelen olayların geçmişte nasıl çözüldüğu hakkında bilgi almak için Kusto'yu tekrar sorgula.

![Akış koşulu sorgusu](./media/flow-usage/flow-conditionquery.png)

> [!IMPORTANT]
> Küme *Adı* alanına küme URL'sini girin.

Bu bilgileri pasta grafiği olarak görselleştirin ve ekibe e-posta yla gönder.

![Akış durumu e-posta](./media/flow-usage/flow-conditionemail.png)

## <a name="email-multiple-azure-data-explorer-flow-charts"></a>Birden çok Azure Veri Gezgini Akış grafiğini e-postayla gönder

1. "Yineleme" tetikleyicisi ile yeni bir Akış oluşturun ve Akış aralığını ve frekansı tanımlayın. 
1. Bir veya daha fazla Kusto ile yeni bir adım ekleyin - Sorgu çalıştırın ve sonuç eylemlerini görselleştirin. 

    ![Akışta birkaç sorgu çalıştırma](./media/flow-usage/flow-severalqueries.png)
1. Her Kusto - Sorguçalıştır ın ve sonucu görselleştirin, aşağıdaki alanları tanımlayın:
    * Küme URL'si *(Küme Adı* alanında)
    * Veritabanı Adı
    * Sorgu ve Grafik Türü (HTML Tablo/ Pasta Grafiği/ Zaman Grafiği/ Çubuk Grafiği/ Özel Değer Girin).

    ![Birden çok ekile sonuçları görselleştirin](./media/flow-usage/flow-visualizeresultsmultipleattachments.png)

    > [!IMPORTANT]
    > Küme *Adı* alanlarına küme URL'sini girin.

1. E-posta gönder eylemi ekleyin. 
    * *Gövde* alanına, sorgunun görselleştirilmiş sonucunun e-postanın gövdesine dahil olması için gerekli gövdeyi ekleyin.
    * E-postaya bir ek eklemek için Ek Adı ve Ek İçeriği ekleyin.
    
    ![Birden çok eki e-postayla gönder](./media/flow-usage/flow-emailmultipleattachments.png)

Sonuçlar:

[![](./media/flow-usage/flow-resultsmultipleattachments.png "Results of multiple attachments")](./media/flow-usage/flow-resultsmultipleattachments.png#lightbox)

[![](./media/flow-usage/flow-resultsmultipleattachments2.png "Results of multiple attachments")](./media/flow-usage/flow-resultsmultipleattachments2.png#lightbox)

## <a name="send-a-different-email-to-different-contacts"></a>Farklı kişilere farklı bir e-posta gönderme

Farklı kişilere farklı özelleştirilmiş e-postalar göndermek için Microsoft Akışı'ndan yararlanabilirsiniz. E-posta adresleri ve e-posta içeriği Bir Kusto sorgusunun sonucu.

Örnek:

![Kusto sorgusu kullanarak dinamik e-posta](./media/flow-usage/flow-dynamicemailkusto.png)

> [!IMPORTANT]
> Küme *Adı* alanına küme URL'sini girin.

![Akış eyleminde dinamik e-posta](./media/flow-usage/flow-dynamicemail.png)

## <a name="create-a-custom-html-table"></a>Özel bir HTML tablosu oluşturma

Özel HTML tablosu gibi özel HTML öğeleri oluşturmak ve kullanmak için Microsoft Akışı'ndan yararlanabilirsiniz.

Aşağıdaki örnek, özel bir HTML tablosunun nasıl oluşturulabildiğini gösterir. HTML tablosunun satırları günlük düzeyine göre (Azure Veri Gezgini'ndekiyle aynı) göre renklendirilir.

Benzer bir Akış oluşturmak için aşağıdaki yönergeleri izleyin:

1. Yeni bir Kusto oluşturun - Sorgu çalıştırın ve sonuç eylemini listeleyin.

    ![HTML tablosu nun sonuçlarını listele](./media/flow-usage/flow-listresultforhtmltable.png)

> [!IMPORTANT]
> Küme *Adı* alanına küme URL'sini girin.

1. Sorgu sonuçlarını niçin döngüye sok ve HTML tablo gövdesini oluşturun: 
    1. HTML dizesini tutmak için bir değişken oluşturmak için **Yeni adım'ı** seçin
    1. **Eylem ekle'yi** ve Değişkenleri ara'yı seçin. 
    1. Değişkenleri Seçin **- Değişkeni Başlatma**. 
    1. Bir dize değişkenini aşağıdaki gibi başlatma:

    ![Değişkeni başlatma](./media/flow-usage/flow-initializevariable.png)

1. Sonuçlar üzerinde döngü:
    1. **Yeni adım**'ı seçin.
    1. **Eylem ekle**'yi seçin.
    1. Değişkenleri ara. 
    1. **Değişkenler'i seçin - String değişkenine ek.** 
    1. Daha önce başharfe aldığınız değişken adını seçin ve sorgu sonuçlarını kullanarak HTML tablo satırlarını oluşturun. 
    Sorgu sonuçlarını seçerken, Uygula her biri otomatik olarak eklenir.

    Aşağıdaki örnekte, `if` ifade her satırın stilini tanımlamak için kullanılır:

    ```if(equals(items('Apply_to_each')?['Level'], 'Warning'), 'Yellow', if(equals(items('Apply_to_each')?['Level'], 'Error'), 'red', 'white'))```

    [![](./media/flow-usage/flow-createhtmltableloopcontent.png "Create HTML table loop content")](./media/flow-usage/flow-createhtmltableloopcontent.png#lightbox)

1. Tam HTML içeriğini oluşturun: 
    1. Her biri için Uygula dışında yeni bir eylem ekleyin. 
    Aşağıdaki örnekte kullanılan eylem bir e-posta gönder'dir.
    1. Önceki adımlardaki değişkeni kullanarak HTML tablonuzu tanımlayın. 
    1. Bir e-posta gönderiyorsanız, **gelişmiş seçenekleri göster'i** seçin ve HTML altında **Evet'i**seçin.

    ![Özel HTML tablo e-postası](./media/flow-usage/flow-customhtmltablemail.png)

Sonuç:

![Özel HTML tablo e-posta sonucu](./media/flow-usage/flow-customhtmltableresult.png)

## <a name="next-steps"></a>Sonraki adımlar

Planlanan veya tetiklenen bir görevin parçası olarak Kusto sorgularını ve komutlarını otomatik olarak çalıştırmanın başka bir yolu olan [Microsoft Azure Explorer Mantık Uygulaması bağlayıcısı](https://docs.microsoft.com/azure/kusto/tools/logicapps) hakkında bilgi edinin.
