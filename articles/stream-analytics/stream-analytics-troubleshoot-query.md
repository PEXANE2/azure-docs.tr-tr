---
title: Azure Stream Analytics sorgularının sorunlarını giderme
description: Bu makalede Azure Stream Analytics işlerinde sorgularınızda sorun gidermeye yönelik teknikler açıklanmaktadır.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 22e542715afa8c87ffb742bec6c22f758cd16587
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354275"
---
# <a name="troubleshoot-azure-stream-analytics-queries"></a>Azure Stream Analytics sorgularının sorunlarını giderme

Bu makalede, Stream Analytics sorguları geliştirmeyle ilgili yaygın sorunlar ve bunların nasıl giderileceği açıklanmaktadır.

## <a name="query-is-not-producing-expected-output"></a>Sorgu beklenen çıktıyı üretmiyor 
1.  Yerel olarak test ederek hataları inceleyin:
    - **Sorgu** sekmesinde **Test**' i seçin. [Sorguyu test](stream-analytics-test-query.md)etmek için indirilen örnek verileri kullanın. Tüm hataları inceleyin ve bunları düzeltmeye çalışın.   
    - Ayrıca, Visual Studio için Stream Analytics araçları 'nı kullanarak [sorgunuzu doğrudan canlı girişte test](stream-analytics-live-data-local-testing.md) edebilirsiniz.

2.  [**Zaman damgası**](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics)kullanıyorsanız, olayların zaman damgalarının [iş başlangıç zamanından](stream-analytics-out-of-order-and-late-events.md)daha büyük olduğunu doğrulayın.

3.  Genel sınırları ortadan kaldırın, örneğin:
    - Sorgudaki [**WHERE**](https://docs.microsoft.com/stream-analytics-query/where-azure-stream-analytics) yan tümcesi tüm olayları filtreleyerek tüm çıktının oluşturulmasını önler.
    - [**Atama**](https://docs.microsoft.com/stream-analytics-query/cast-azure-stream-analytics) işlevi başarısız olur ve işin başarısız olmasına neden olur. Tür dönüştürme hatalarından kaçınmak için, bunun yerine [**TRY_CAST**](https://docs.microsoft.com/stream-analytics-query/try-cast-azure-stream-analytics) kullanın.
    - Pencere işlevleri kullandığınızda, tüm pencere süresinin sorgudaki bir çıktıyı görmesini bekleyin.
    - Olaylar için zaman damgası iş başlangıç zamanından önce gelir ve bu nedenle olaylar bırakılıyor.

4.  Olay sıralama ilkelerinin beklenen şekilde yapılandırıldığından emin olun. **Ayarlar** ' a gidin ve [**olay sıralaması**](stream-analytics-out-of-order-and-late-events.md)' nı seçin. Sorguyu test etmek için **Test** düğmesini kullandığınızda *ilke uygulanmaz.* Bu sonuç, test sırasında çalıştırılan iş ve üretimde iş arasında bir farklılık olduğunu fark ediyor. 

5. Denetim ve tanılama günlüklerini kullanarak hata ayıklayın:
    - [Denetim günlüklerini](../azure-resource-manager/resource-group-audit.md)kullanın ve hataları tanımlamak ve hatalarını ayıklamak için filtre uygulayın.
    - Hataları tanımlamak ve hatalarını ayıklamak için [iş tanılama günlüklerini](stream-analytics-job-diagnostic-logs.md) kullanın.

## <a name="job-is-consuming-too-many-streaming-units"></a>İş çok fazla akış birimi kullanıyor
Azure Stream Analytics ' de paralelleştirme avantajını kullandığınızdan emin olun. Giriş bölümlerini yapılandırarak ve analiz sorgu tanımını ayarlayarak Stream Analytics işlerin [sorgu paralel hale getirme ile ölçeklendirmeyi](stream-analytics-parallelization.md) öğrenebilirsiniz.

## <a name="debug-queries-progressively"></a>Sorguları aşamalı olarak hata ayıklama

Gerçek zamanlı veri işlemede, verilerin ortasında ne gibi göründüğünü bilmek faydalı olabilir. Azure Stream Analytics bir işin girişleri veya adımları birden çok kez okunabileceğinden, ek SELECT INTO deyimlerini yazabilirsiniz. Bunun yapılması, ara verileri depolama alanına çıkarır ve verilerin doğruluğunu, bir programda hata ayıkladığınızda olduğu gibi, *izleme değişkenlerini* incelemenizi sağlar.

Azure Stream Analytics işinde aşağıdaki örnek sorgu, bir akış girişi, iki başvuru veri girişi ve Azure Tablo depolama 'ya bir çıktı içerir. Sorgu, ad ve kategori bilgilerini almak için Olay Hub 'ından ve iki başvuru Bloblarından verileri birleştirir:

![Örnek Stream Analytics sorgu Seç](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

İşin çalıştığını, ancak çıktıda hiçbir olayın üretilmediğini unutmayın. Burada gösterilen **izleme** kutucuğunda, girişin veri üretmekte olduğunu, ancak **birleşimin** hangi adımının tüm olayların bırakılmasına neden olduğunu bilemezsiniz.

![Stream Analytics Izleme kutucuğu](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
Bu durumda, ara JOIN sonuçlarını ve girişten okunan verileri "günlüğe kaydetmek" için birkaç ekstra SELECT INTO ifadesi ekleyebilirsiniz.

Bu örnekte, iki yeni "geçici çıkış" ekledik. Dilediğiniz havuz olabilir. Burada Azure Storage 'ı örnek olarak kullanırız:

![Stream Analytics sorguya ek SELECT INTO deyimleri ekleme](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

Daha sonra sorguyu şunun gibi yeniden yazabilirsiniz:

![Yeniden yazan Stream Analytics sorgu Seç](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

Şimdi işi yeniden başlatın ve birkaç dakika boyunca çalışmasına izin verin. Daha sonra aşağıdaki tabloları oluşturmak için Temp1 ve Temp2 'i Visual Studio Cloud Explorer ile sorgulayın:

**Temp1 tablo**
![temp1 tablo Stream ANALYTICS sorgu seçin](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)

**Temp2 tablo**
![temp2 tablo Stream ANALYTICS sorgu seçin](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)

Gördüğünüz gibi, Temp1 ve Temp2 her ikisinde de veri bulunur ve ad sütunu Temp2 içinde doğru doldurulur. Ancak hala çıktıda hiç veri bulunmadığından bir sorun oluştu:

![Veri Stream Analytics sorgu olmadan output1 tablo seçme](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

Verileri örnekleyerek, sorunun ikinci BIRLEŞIMDE geldiğinden neredeyse emin olabilirsiniz. Blob 'dan başvuru verilerini indirebilir ve bir göz atabilirsiniz:

![Sorgu Stream Analytics ref tablosuna Seç](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

Görebileceğiniz gibi, bu başvuru verilerinde GUID biçimi Temp2 içindeki [from] sütununun biçiminden farklıdır. Bu nedenle, veriler beklenen şekilde output1 'e ulaşamamaktadır.

Veri biçimini giderebilir, başvuru blobuna yükleyebilir ve yeniden deneyebilirsiniz:

![Sorgu Stream Analytics geçici tablo SEÇIN](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

Bu kez, çıkışdaki veriler biçimlendirilir ve beklendiği gibi doldurulur.

![Son tablo Stream Analytics sorguda SEÇIN](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
