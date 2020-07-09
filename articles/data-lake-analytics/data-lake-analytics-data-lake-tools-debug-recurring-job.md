---
title: Azure Data Lake Analytics yinelenen işleri hata ayıkla
description: Olağan dışı yinelenen bir işin hatalarını ayıklamak için Visual Studio için Azure Data Lake Araçları nasıl kullanacağınızı öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: how-to
ms.date: 05/20/2018
ms.openlocfilehash: b3fe23d2b4605289c89df1d5ef5033d35986e07c
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86117330"
---
# <a name="troubleshoot-an-abnormal-recurring-job"></a>Olağan dışı bir şekilde yinelenen iş sorunlarını giderme

Bu makalede yinelenen işlerle ilgili sorunları gidermek için [Visual Studio için Azure Data Lake araçları](https://aka.ms/adltoolsvs) nasıl kullanılacağı gösterilmektedir. [Azure Data Lake ve Azure HDInsight blogundan](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/)ardışık düzen ve yinelenen işler hakkında daha fazla bilgi edinin.

Yinelenen işler genellikle aynı sorgu mantığını ve benzer giriş verilerini paylaşır. Örneğin, her Pazartesi sabah saat 8 ' de çalışan bir yinelenen işiniz olduğunu düşünün. Son haftanın haftalık etkin kullanıcısını saymak için. Bu işlerin betikleri, sorgu mantığını içeren bir betik şablonunu paylaşır. Bu işlerin girişleri, geçen haftaki kullanım verişlerdir. Aynı sorgu mantığının ve benzer girişin paylaşılması genellikle bu işlerin performansının benzer ve kararlı olduğu anlamına gelir. Yinelenen işlerinizin biri aniden olağan dışı, başarısız olur veya çok az yavaşlar, şunları yapmak isteyebilirsiniz:

- Ne olduğunu görmek için yinelenen işin önceki çalıştırmaları için istatistik raporlarına bakın.
- Nelerin değiştirildiğini anlamak için olağan dışı işi normal bir ile karşılaştırın.

Visual Studio için Azure Data Lake Araçları **Ilgili Iş görünümü** her iki durumda da sorun giderme ilerlemesini hızlandırmanıza yardımcı olur.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>1. Adım: yinelenen işleri bulma ve Ilgili Iş görünümünü açma

Yinelenen bir iş sorununu gidermek üzere Ilgili Iş görünümünü kullanmak için önce Visual Studio 'da yinelenen işi bulmanız ve ardından Ilgili Iş görünümünü açmanız gerekir.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Durum 1: yinelenen işin URL 'SI var

**Araçlar**  >  **Data Lake**  >  **iş görünümü**aracılığıyla, Visual Studio 'da iş görünümü açmak için iş URL 'sini yapıştırabilirsiniz. İlgili Iş görünümünü açmak için **Ilgili Işleri görüntüle** ' yi seçin.

![Data Lake Analytics araçlarında Ilgili Işleri görüntüle bağlantısı](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Durum 2: yinelenen iş için işlem hattına sahipsiniz, ancak URL 'YI değil

Visual Studio 'da, işlem hattı tarayıcısı Azure Data Lake Analytics hesabınızı > işlem **hatları**> Sunucu Gezgini aracılığıyla açabilirsiniz. (Bu düğümü Sunucu Gezgini bulamazsanız, [en son eklentiyi indirin](https://aka.ms/adltoolsvs).) 

![Ardışık düzen düğümünü seçme](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

Ardışık düzen tarayıcısı ' nda, Data Lake Analytics hesabının tüm işlem hatları solda listelenmiştir. Tüm yinelenen işleri bulmak için işlem hatlarını genişletebilir ve ardından sorunları olan birini seçebilirsiniz. İlgili Iş görünümü doğru açılıyor.

![Işlem hattı seçme ve Ilgili Iş görünümünü açma](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-a-statistics-report"></a>2. Adım: istatistik raporunu analiz etme

Özet ve istatistik raporu, Ilgili Iş görünümünün en üstünde gösterilir. Burada, sorunun olası temel nedenini bulabilirsiniz. 

1.  Raporda, X ekseni iş gönderim süresini gösterir. Olağan dışı işi bulmak için bunu kullanın.
2.  İstatistikleri denetlemek ve sorun ve olası çözümler hakkında öngörüleri almak için aşağıdaki diyagramdaki işlemi kullanın.

![İstatistikleri denetlemek için işlem diyagramı](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-job-to-a-normal-job"></a>3. Adım: olağan dışı işi normal bir işle karşılaştırın

Tüm gönderilen yinelenen işleri, Ilgili Iş görünümünün alt kısmındaki iş listesinden bulabilirsiniz. Daha fazla öngörü ve potansiyel çözüm bulmak için olağan dışı işe sağ tıklayın. Olağan dışı işi önceki bir normal bir ile karşılaştırmak için Iş farkı görünümünü kullanın.

![İşleri karşılaştırmak için kısayol menüsü](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Bu iki iş arasındaki büyük farklılıklara dikkat edin. Bu farklılıklar, büyük olasılıkla performans sorunlarına neden oluyor. Daha fazla kontrol etmek için aşağıdaki diyagramdaki adımları kullanın:

![İşler arasındaki farkları denetlemek için işlem diyagramı](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Veri eğriltme sorunlarını çözme](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Başarısız U-SQL işleri için Kullanıcı tanımlı C# kodunda hata ayıklama](data-lake-analytics-debug-u-sql-jobs.md)
