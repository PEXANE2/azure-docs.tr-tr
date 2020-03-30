---
title: Azure Günlük Analizi verilerini Power BI'ye aktarın | Microsoft Dokümanlar
description: Power BI, Microsoft'un farklı veri kümelerinin analizi için zengin görselleştirmeler ve raporlar sağlayan bulut tabanlı bir iş analizi hizmetidir.  Bu makalede, Log Analytics verilerinin Power BI'ye nasıl yapılandırılabildiğini ve içe aktarılabildiğini ve otomatik olarak yenilenebilmek için nasıl yapılandırılabildiğini açıklamaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/01/2019
ms.openlocfilehash: 8ff24d508eb35c4f2a04c7d024254fa6f1875da8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77659294"
---
# <a name="import-azure-monitor-log-data-into-power-bi"></a>Azure Monitor günlük verilerini Power BI'ye aktarma


[Power BI,](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) Microsoft'un farklı veri kümelerinin analizi için zengin görselleştirmeler ve raporlar sağlayan bulut tabanlı bir iş analizi hizmetidir.  Azure Monitor günlük sorgusunun sonuçlarını Power BI veri kümesine aktararak farklı kaynaklardan gelen verileri birleştirme ve web ve mobil cihazlarda rapor paylaşma gibi özelliklerinden yararlanabilirsiniz.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="overview"></a>Genel Bakış
Azure Monitor'daki Bir [Günlük Analizi çalışma alanından](manage-access.md) Power BI'ye veri almak için, Azure Monitor'daki [günlük sorgusuna](../log-query/log-query-overview.md) dayalı olarak Power BI'de bir veri kümesi oluşturursunuz.  Sorgu, veri kümesi her yenilendirinde çalıştırılır.  Daha sonra veri kümesinden veri kullanan Power BI raporları oluşturabilirsiniz.  Power BI'de veri kümesioluşturmak için, sorgunuzu Log Analytics'ten [Power Query (M) diline](https://docs.microsoft.com/powerquery-m/power-query-m-language-specification)dışa aktarın.  Ardından, Power BI Desktop'da bir sorgu oluşturmak ve ardından veri kümesi olarak Power BI'de yayımlamak için bunu kullanırsınız.  Bu işlemin ayrıntıları aşağıda açıklanmıştır.

![Power BI'ye Günlük Analitik](media/powerbi/overview.png)

## <a name="export-query"></a>Dışa aktarma sorgusu
Power BI veri kümesini doldurmak istediğiniz verileri döndüren bir [günlük sorgusu](../log-query/log-query-overview.md) oluşturarak başlayın.  Daha sonra bu sorguydan Power Bi Desktop tarafından kullanılabilecek [Güç Sorgusu (M) diline](https://docs.microsoft.com/powerquery-m/power-query-m-language-specification) dışa aktarırsınız.

1. Veri kümeniz için verileri ayıklamak için [Log Analytics'te günlük sorgusunu oluşturun.](../log-query/get-started-portal.md)
2. **Dışa Aktarma** > **Gücü BI Sorgusu (M) seçeneğini**belirleyin.  Bu, sorguyu **PowerBIQuery.txt**adlı bir metin dosyasına aktarır. 

    ![Günlük aramayı dışa aktarma](media/powerbi/export-analytics.png)

3. Metin dosyasını açın ve içeriğini kopyalayın.

## <a name="import-query-into-power-bi-desktop"></a>Sorguyı Power BI Desktop' a aktar
Power BI Desktop, Power BI'de yayımlanabilen veri kümeleri ve raporlar oluşturmanıza olanak tanıyan bir masaüstü uygulamasıdır.  Azure Monitor'dan dışa aktarılan Güç Sorgusu dilini kullanarak sorgu oluşturmak için de kullanabilirsiniz. 

1. Zaten yoksa [Power BI Desktop'ı](https://powerbi.microsoft.com/desktop/) yükleyin ve uygulamayı açın.
2. Yeni bir sorgu açmak için Veri > Boş**Sorgu** **Al'ı**seçin.  Ardından **Gelişmiş Düzenleyici'yi** seçin ve dışa aktarılan dosyanın içeriğini sorguya yapıştırın. **Bitti**’ye tıklayın.

    ![Power BI Masaüstü sorgusu](media/powerbi/desktop-new-query.png)

5. Sorgu çalışır ve sonuçları görüntülenir.  Kimlik bilgilerinin Azure'a bağlanması isden edilebilir.  
6. Sorgu için açıklayıcı bir ad yazın.  Varsayılan **sorgu1'dir.** Veri kümesini rapora eklemek için **Kapat ve Uygula'yı** tıklatın.

    ![Power BI Masaüstü adı](media/powerbi/desktop-results.png)



## <a name="publish-to-power-bi"></a>Power BI'da yayımla
Power BI'de yayımladığınızda, bir veri kümesi ve rapor oluşturulur.  Power BI Desktop'da bir rapor oluşturursanız, bu rapor verilerinizle birlikte yayınlanacaktır.  Değilse, boş bir rapor oluşturulur.  Raporu Power BI'de değiştirebilir veya veri kümesini temel alan yeni bir rapor oluşturabilirsiniz.

1. Verilerinize dayalı bir rapor oluşturun.  Aşina değilseniz [Power BI Desktop belgelerini](https://docs.microsoft.com/power-bi/desktop-report-view) kullanın.  
1. Power BI'ye göndermeye hazır olduğunuzda **Yayımla'yı**tıklatın.  
1. İstendiğinde, Power BI hesabınızda bir hedef seçin.  Aklınızda belirli bir hedef yoksa, **Çalışma Alanım'ı**kullanın.

    ![Power BI Desktop yayınlamak](media/powerbi/desktop-publish.png)

1. Yayımlama tamamlandığında, yeni veri kümenizle Power BI'yi açmak için **Power BI'de Aç'ı** tıklatın.


### <a name="configure-scheduled-refresh"></a>Zamanlanmış yenileme yapılandırma
Power BI'de oluşturulan veri kümesi, Power BI Desktop'da daha önce gördüğünüz verilerle aynı olacaktır.  Sorguyu yeniden çalıştırmak ve Azure Monitor'dan gelen en son verilerle doldurmak için veri kümesini düzenli aralıklarla yenilemeniz gerekir.  

1. Raporunuzu yüklediğiniz çalışma alanına tıklayın ve **Datasets** menüsünü seçin. 
1. Yeni veri setinizin yanındaki bağlam menüsünü seçin ve **Ayarlar'ı**seçin. 
1. **Veri kaynağı kimlik bilgilerinin** altında kimlik bilgilerinin geçersiz olduğuna dair bir iletiniz olmalıdır.  Bunun nedeni, veri kümesiverilerini yenilerken kullanmak üzere henüz kimlik bilgileri sağlamamış olmasıdır.  
1. Azure **Monitor'da** Günlük Analizi çalışma alanına erişerek kimlik bilgilerini edin ve kimlik bilgilerini belirtin'i tıklatın. İki faktörlü kimlik doğrulamaistiyorsanız, kimlik bilgilerinizle oturum açmak için **Kimlik Doğrulama yönteminin** istenmesi için **OAuth2'yi** seçin.

    ![Güç BI programı](media/powerbi/powerbi-schedule.png)

5. **Zamanlanmış yenileme** altında **verilerinizi güncel tutma**seçeneğini açın.  Yenilemeyi çalıştırmak için isteğe bağlı olarak **Yenileme sıklığını** ve bir veya daha fazla belirli süreyi değiştirebilirsiniz.

    ![Güç BI yenileme](media/powerbi/powerbi-schedule-refresh.png)



## <a name="next-steps"></a>Sonraki adımlar
* Power BI'ye aktarılabilen sorgular oluşturmak için [günlük aramaları](../log-query/log-query-overview.md) hakkında bilgi edinin.
* Azure Monitor günlük dışa aktarmalarını temel alan görselleştirmeler oluşturmak için [Power BI](https://powerbi.microsoft.com) hakkında daha fazla bilgi edinin.
