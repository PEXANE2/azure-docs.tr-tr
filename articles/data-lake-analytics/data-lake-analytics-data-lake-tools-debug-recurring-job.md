---
title: Azure Veri Gölü Analitiği'nde yinelenen işleri hata ayıklama
description: Anormal yinelenen bir işi hata ayıklamak için Visual Studio için Azure Veri Göl Araçlarını nasıl kullanacağınızı öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 05/20/2018
ms.openlocfilehash: 5a2935d559a967151c5bdc01c4b0806fe52179b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60629789"
---
# <a name="troubleshoot-an-abnormal-recurring-job"></a>Olağan dışı bir şekilde yinelenen iş sorunlarını giderme

Bu makalede, yinelenen işlerle ilgili sorunları gidermek [için Visual Studio için Azure Veri Göl Araçları'nın](https://aka.ms/adltoolsvs) nasıl kullanılacağı gösterilmektedir. Azure Veri Gölü ve [Azure HDInsight blogundan](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/)boru hattı ve yinelenen işler hakkında daha fazla bilgi edinin.

Yinelenen işler genellikle aynı sorgu mantığını ve benzer giriş verilerini paylaşır. Örneğin, her Pazartesi sabahı saat 8'de çalışan yinelenen bir işiniz olduğunu düşünün. geçen haftanın haftalık aktif kullanıcısını saymak için. Bu işlerin komut dosyaları, sorgu mantığını içeren bir komut dosyası şablonu paylaşır. Bu işlerin girdileri geçen haftanın kullanım verileridir. Aynı sorgu mantığını ve benzer girdiyi paylaşmak genellikle bu işlerin performansının benzer ve kararlı olduğu anlamına gelir. Yinelenen işlerinizden biri aniden anormal bir şekilde performans gösterirse, başarısız olursa veya çok yavaşlarsa, şunları yapmak isteyebilirsiniz:

- Ne olduğunu görmek için yinelenen işin önceki çalıştırmalarının istatistik raporlarına bakın.
- Neyin değiştiğini anlamak için anormal işi normal bir iş ile karşılaştırın.

Visual Studio için Azure Veri Gölü Araçlarında **İlgili İş Görünümü,** her iki durumda da sorun giderme sürecini hızlandırmanıza yardımcı olur.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>Adım 1: Yinelenen işleri bulma ve İlişkili İş Görünümü'ni açma

Yinelenen bir iş sorununu gidermek için İlişkili İş Görünümü'ni kullanmak için önce Visual Studio'da yinelenen işi bulmanız ve ardından İlişkili İş Görünümü'nu açmanız gerekir.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Örnek 1: Yinelenen işin URL'si sizde

**Araçlar** > **Veri Gölü** > **İş Görünümü**aracılığıyla, Visual Studio'da İş Görünümü'nü açmak için iş URL'sini yapıştırabilirsiniz. İlişkili İş Görünümünü açmak için **İlişkili İşleri Görüntüle'yi** seçin.

![Data Lake Analytics Tools'ta İlgili İşler bağlantısını görüntüleyin](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Örnek 2: Yinelenen iş için ardışık alan var, ancak URL'ye sahip değil

Visual Studio'da, Azure Veri Gölü Analizi hesabınıza > Sunucu Gezgini aracılığıyla Pipeline Browser > **Pipelines'ı**açabilirsiniz. (Bu düğümü Server Explorer'da bulamazsanız, [en son eklentiyi indirin.)](https://aka.ms/adltoolsvs) 

![Boru Hatları düğümünü seçme](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

Pipeline Browser'da, Data Lake Analytics hesabının tüm ardışık hatları solda listelenir. Yinelenen tüm işleri bulmak için ardışık hatları genişletebilir ve sonra sorunları olan bir tane seçebilirsiniz. İlgili İş Görünümü sağda açılır.

![Bir ardışık kaynak seçme ve İlgili İş Görünümü'nü açma](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-a-statistics-report"></a>Adım 2: İstatistik raporunu çözümleme

İlgili İş Görünümü'nün en üstünde bir özet ve istatistik raporu gösterilir. Burada, sorunun potansiyel kök nedenini bulabilirsiniz. 

1.  Raporda, X ekseni iş teslim süresini gösterir. Anormal işi bulmak için kullan.
2.  İstatistikleri denetlemek ve sorun ve olası çözümler hakkında bilgi almak için aşağıdaki diyagramdaki işlemi kullanın.

![İstatistikleri denetlemek için işlem diyagramı](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-job-to-a-normal-job"></a>Adım 3: Anormal işi normal bir işle karşılaştırın

Gönderilen tüm yinelenen işleri Ilgili İş Görünümü'nün altındaki iş listesi aracılığıyla bulabilirsiniz. Daha fazla öngörü ve potansiyel çözüm bulmak için anormal işi sağ tıklatın. Anormal işi önceki normal iş ile karşılaştırmak için İş Diff görünümünü kullanın.

![İşleri karşılaştırmak için kısayol menüsü](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Bu iki iş arasındaki büyük farklılıklara dikkat edin. Bu farklılıklar muhtemelen performans sorunlarına neden oluyor. Daha fazla kontrol etmek için aşağıdaki diyagramdaki adımları kullanın:

![İşler arasındaki farkları denetlemek için işlem diyagramı](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Veri eğriltme sorunlarını çözme](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Başarısız U-SQL işleri için kullanıcı tanımlı C# kodu hata ayıklama](data-lake-analytics-debug-u-sql-jobs.md)
