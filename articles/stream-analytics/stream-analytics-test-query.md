---
title: Azure Akış Analizi işini örnek verilerle test edin
description: Bu makalede, bir Azure Akış Analizi işini sınamak, örnek giriş yapmak ve örnek verileri yüklemek için Azure portalının nasıl kullanılacağı açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 3/6/2020
ms.custom: seodec18
ms.openlocfilehash: de2b99666dbed4eaec3db3e56800e9a7352e10e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898397"
---
# <a name="test-an-azure-stream-analytics-job-in-the-portal"></a>Portalda bir Azure Akışı Analizi işini test edin

Azure Akış Analizi'nde, işinizi başlatmadan veya durdurmadan sorgunuzu test edebilirsiniz. Akış kaynaklarınızdan gelen verilerdeki sorguları sınayabilir veya Azure Portalı'ndaki yerel bir dosyadan örnek veriler yükleyebilirsiniz. Ayrıca yerel örnek verilerinizden veya [Visual Studio](stream-analytics-live-data-local-testing.md) ve Visual [Studio Code'da](visual-studio-code-local-run-live-input.md)canlı verilerinizden sorguları yerel olarak test edebilirsiniz.

## <a name="automatically-sample-incoming-data-from-input"></a>Girişten gelen verileri otomatik olarak örnekleme

Azure Akış Analizi, akış girişlerinizden olayları otomatik olarak getirir. Varsayılan örnekte sorguları çalıştırabilir veya örnek için belirli bir zaman çerçevesi ayarlayabilirsiniz.

1. Azure Portal’da oturum açın.

2. Mevcut Stream Analytics işinizi bulun ve seçin.

3. Akış Analizi iş sayfasında, **İş Topolojisi** başlığı altında Sorgu düzenleyicisi penceresini açmak için **Sorgu'yu** seçin. 

4. Gelen olayların örnek listesini görmek için dosya simgesiyle girişi seçin ve örnek olaylar **Giriş önizlemesinde**otomatik olarak görünür.

   a. Verileriniz için serileştirme türü, JSON veya CSV'si varsa otomatik olarak algılanır. Açılan menüdeki seçeneği değiştirerek json, CSV, AVRO olarak da el ile değiştirebilirsiniz.
    
   b. Verilerinizi **Tablo** veya **Ham** formatında görüntülemek için seçiciyi kullanın.
    
   c. Gösterilen verileriniz güncel değilse, en son olayları görmek için **Yenile'yi** seçin.

   Aşağıdaki tablo **Tablo biçiminde**veri bir örnektir:

   ![Tablo formatında Azure Akış Analizi örnek girişi](./media/stream-analytics-test-query/asa-sample-table.png)

   Aşağıdaki tablo **Raw biçiminde**veri bir örnektir:

   ![Azure Akış Analizi örnek girişi ham formatta](./media/stream-analytics-test-query/asa-sample-raw.png)

5. Sorgunuzu gelen verilerle sınamak için **Test sorgusu'nun**. Sonuçlar Test **sonuçları** sekmesinde görünür. Sonuçları indirmek için **sonuçları karşıdan** yüklemeyi de seçebilirsiniz.

   ![Azure Akışı Analizi örnek test sorgu sonuçları](./media/stream-analytics-test-query/asa-test-query.png)

6. Sorgunuzu belirli bir gelen olaylar aralığına göre test etmek **için, zaman aralığını seçin'** i seçin.
   
   ![Gelen örnek etkinlikler için Azure Akışı Analizi zaman aralığı](./media/stream-analytics-test-query/asa-select-time-range.png)

7. Sorgunuzu sınamak için kullanmak istediğiniz olayların zaman aralığını ayarlayın ve **Örnek'i**seçin. Bu süre içinde, hangisi önce gerçekleşirse, en fazla 1000 olay veya 1 MB'yi alabilirsiniz.

   ![Azure Akış Analizi, gelen örnek etkinlikler için zaman aralığını belirlir](./media/stream-analytics-test-query/asa-set-time-range.png)

8. Olaylar seçili zaman aralığı için örnekledikten **sonra, Giriş önizleme** sekmesinde görünür.

   ![Azure Akışı Analizi test sonuçlarını görüntüleme](./media/stream-analytics-test-query/asa-view-test-results.png)

9. Gelen olayların örnek listesini görmek için **Sıfırla'yı** seçin. **Sıfırla'yı**seçerseniz, zaman aralığı seçiminiz kaybolur. Sorgunuzu test etmek ve **Test sonuçları** sekmesinde sonuçları gözden geçirmek için **Test sorgusunu** seçin.

10. Sorgunuzda değişiklik yaptığınızda, yeni sorgu mantığını sınamak için **sorguyu kaydet'i** seçin. Bu, sorgunuzu yinelemeli olarak değiştirmenize ve çıktının nasıl değiştiğini görmek için yeniden sınamanıza olanak tanır.

11. Tarayıcıda gösterilen sonuçları doğruladıktan sonra, işi **başlatmaya** hazırsınız.

## <a name="upload-sample-data-from-a-local-file"></a>Yerel bir dosyadan örnek veri yükleme

Azure Akış Analizi sorgunuzu test etmek için canlı verileri kullanmak yerine yerel bir dosyadan örnek verileri kullanabilirsiniz.

1. Azure Portal’da oturum açın.
   
2. Mevcut Stream Analytics işinizi bulun ve seçin.

3. Akış Analizi iş sayfasında, **İş Topolojisi** başlığı altında Sorgu düzenleyicisi penceresini açmak için **Sorgu'yu** seçin.

4. Sorgunuzu yerel bir dosyayla test etmek için **Giriş önizleme** sekmesinde **örnek girişi yükle'yi** seçin. 

   ![Azure Akışı Analytics yükleme örnek dosya](./media/stream-analytics-test-query/asa-upload-sample-file.png)

5. Sorguyu test etmek için yerel dosyanızı yükleyin. Dosyaları yalnızca JSON, CSV veya AVRO formatlarına yükleyebilirsiniz. **Tamam'ı**seçin.

   ![Azure Akışı Analytics yükleme örnek dosya](./media/stream-analytics-test-query/asa-upload-sample-json-file.png)

6. Dosyayı yükler yüklemez, formdaki dosya içeriğini tablo olarak veya ham biçiminde de görebilirsiniz. **Sıfırla'yı**seçerseniz, örnek veriler önceki bölümde açıklanan gelen giriş verilerine geri döner. Sorguyu istediğiniz zaman sınamak için başka bir dosya yükleyebilirsiniz.

7. Sorgunuzu yüklenen örnek dosyayla karşı test etmek için **Test sorgusunu** seçin.

8. Test sonuçları sorgunuza göre gösterilir. Sorgunuzu değiştirebilir ve yeni sorgu mantığını sınamak için **sorguyu kaydet'i** seçebilirsiniz. Bu, sorgunuzu yinelemeli olarak değiştirmenize ve çıktının nasıl değiştiğini görmek için yeniden sınamanıza olanak tanır.

9. Sorguda birden çok çıktı kullandığınızda, sonuçlar seçili çıktıya göre gösterilir. 

   ![Azure Akış Analizi seçilen çıktı](./media/stream-analytics-test-query/asa-sample-test-selected-output.png)

10. Tarayıcıda gösterilen sonuçları doğruladıktan sonra, işi **başlatabilirsiniz.**

## <a name="next-steps"></a>Sonraki adımlar
* [Stream Analytics'i kullanarak bir IoT çözümü oluşturun:](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics)Bu öğretici, gişedeki trafiği simüle edecek bir veri jeneratörüyle uçtan uca bir çözüm oluşturmanıza rehberlik edecektir.

* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)

* [Yaygın Akış Analizi kullanım desenleri için sorgu örnekleri](stream-analytics-stream-analytics-query-patterns.md)

* [Azure Akış Analizi için girişleri anlama](stream-analytics-add-inputs.md)

* [Azure Akış Analitiği'nden çıktıları anlama](stream-analytics-define-outputs.md)
