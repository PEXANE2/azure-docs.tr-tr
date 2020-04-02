---
title: Microsoft Azure Veri Gezgini Akış bağlayıcısı (Önizleme) kullanım örnekleri
description: Bazı yaygın Microsoft Flow bağlayıcısı kullanım örneklerini öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: dorcohen
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/15/2020
ms.openlocfilehash: 1b9d593b0f0895e2ba75fae7ab7e78ea883c8907
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521725"
---
# <a name="microsoft-flow-connector-preview-usage-examples"></a>Microsoft Flow bağlayıcısı (Önizleme) kullanım örnekleri

Azure Veri Gezgini akış bağlayıcısı, Azure Veri Gezgini'nin, Planlanan veya tetiklenen bir görevin parçası olarak Kusto sorgularını ve komutlarını otomatik olarak çalıştırmak için [Microsoft Power Automate'in Akış özelliklerini](https://flow.microsoft.com/) kullanmasına olanak tanır. Bu belge, birkaç yaygın Microsoft Flow bağlayıcısı kullanım örneği içerir.

Daha fazla bilgi için [Microsoft Flow bağlayıcısı (Önizleme)](flow.md)bakın.

* [Microsoft Flow konektörü ve SQL](#microsoft-flow-connector-and-sql)
* [Verileri Power BI veri kümesine itme](#push-data-to-power-bi-dataset)
* [Koşullu sorgular](#conditional-queries)
* [Birden çok Azure Veri Gezgini Akış grafiğini e-postayla gönder](#email-multiple-azure-data-explorer-flow-charts)

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

1. Yineleme tetikleyicisi ile yeni bir Akış oluşturun ve Akış aralığını ve frekansı tanımlayın. 
1. Bir veya daha fazla Kusto ile yeni bir adım ekleyin - Sorgu çalıştırın ve sonuç eylemlerini görselleştirin. 

    ![Akışta birkaç sorgu çalıştırma](./media/flow-usage/flow-severalqueries.png)
1. Her Kusto - Sorguçalıştır ın ve sonucu görselleştirin, aşağıdaki alanları tanımlayın:
    * Küme URL'si
    * Veritabanı Adı
    * Sorgu ve Grafik Türü (HTML tablosu, pasta grafiği, zaman grafiği, çubuk grafiği veya özel bir değer girin).

    ![Birden çok ekile sonuçları görselleştirin](./media/flow-usage/flow-visualizeresultsmultipleattachments.png)

1. E-posta gönder (v2) eylemi ekleyin: 
    1. Gövde bölümünde kod görünümü simgesini seçin.
    1. **Gövde** alanına, sorgunun görselleştirilmiş sonucunun e-postanın gövdesine dahil olması için gerekli BodyHtml'i ekleyin.
    1. E-postaya bir ek eklemek için Ek Adı ve Ek İçeriği ekleyin.
    
    ![Birden çok eki e-postayla gönder](./media/flow-usage/flow-email-multiple-attachments.png)

    Bir e-posta eylemi oluşturma yla ilgili tam talimatlar için, [Kusto E-posta sorgu sonuçlarına](flow.md#email-kusto-query-results)bakın. 

Sonuçlar:

[![](./media/flow-usage/flow-resultsmultipleattachments.png "Results of multiple attachments")](./media/flow-usage/flow-resultsmultipleattachments.png#lightbox)

[![](./media/flow-usage/flow-resultsmultipleattachments2.png "Results of multiple attachments")](./media/flow-usage/flow-resultsmultipleattachments2.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Planlanan veya tetiklenen bir görevin parçası olarak Kusto sorgularını ve komutlarını otomatik olarak çalıştırmanın başka bir yolu olan [Microsoft Azure Explorer Mantık Uygulaması bağlayıcısı](https://docs.microsoft.com/azure/kusto/tools/logicapps) hakkında bilgi edinin.
