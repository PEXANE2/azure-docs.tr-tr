---
title: Azure Akış Analizi sorgularını sorun giderme
description: Bu makalede, Azure Akış Analizi işlerinde sorgularınızı giderme teknikleri açıklanmaktadır.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: bf0740bbdd4754aeba43e64f1076a1bea33cffc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844437"
---
# <a name="troubleshoot-azure-stream-analytics-queries"></a>Azure Akış Analizi sorgularını sorun giderme

Bu makalede, Akış Analizi sorguları geliştirme ve bunları nasıl sorun giderme ile ilgili yaygın sorunlar açıklanmaktadır.

## <a name="query-is-not-producing-expected-output"></a>Sorgu beklenen çıktıyı üretmiyor
1.  Yerel olarak sınama yaparak hataları inceleyin:
    - Azure **portalında** Sorgu sekmesinde **Test'i**seçin. [Sorguyu sınamak](stream-analytics-test-query.md)için indirilen örnek verileri kullanın. Hataları inceleyin ve düzeltmeye çalışın.   
    - Visual Studio veya [Visual Studio Code](visual-studio-code-local-run-live-input.md)için Azure Akışı Analizi araçlarını kullanarak [sorgunuzu yerel olarak](stream-analytics-live-data-local-testing.md) da test edebilirsiniz. 

2.  Visual Studio için Azure Akış Analizi araçlarında [iş diyagramını kullanarak hata ayıklama sorguları adım adım](debug-locally-using-job-diagram.md) yerel olarak sorgular. İş diyagramı, birden çok sorgu adımı ve son olarak batmaya çıkış yoluyla veri giriş kaynaklarından (Event Hub, IoT Hub, vb.) nasıl aktığını göstermektir. Her sorgu adımı, WITH deyimi kullanılarak komut dosyasında tanımlanan geçici bir sonuç kümesine eşlenir. Sorunun kaynağını bulmak için ayarlanan her ara sonuç kümesinde her sorgu adımında verilerin yanı sıra ölçümleri de görüntüleyebilirsiniz.
    ![İş diyagramı önizleme sonucu](./media/debug-locally-using-job-diagram/preview-result.png)

3.  [**Timestamp By**](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics)kullanıyorsanız, olayların [iş başlangıç saatinden](stream-analytics-out-of-order-and-late-events.md)daha büyük zaman damgaları olduğunu doğrulayın.

4.  Yaygın tuzakları ortadan kaldırın, örneğin:
    - Sorgudaki [**WHERE**](https://docs.microsoft.com/stream-analytics-query/where-azure-stream-analytics) yan tümcesi tüm olayları filtreleyerek herhangi bir çıktının oluşturulmasını önler.
    - [**CAST**](https://docs.microsoft.com/stream-analytics-query/cast-azure-stream-analytics) işlevi başarısız olur ve bu da işin başarısız olmasıyla sonuçlaolur. Tür döküm hatalarını önlemek için bunun yerine [**TRY_CAST**](https://docs.microsoft.com/stream-analytics-query/try-cast-azure-stream-analytics) kullanın.
    - Pencere işlevlerini kullandığınızda, sorgudan bir çıktı görmek için tüm pencere süresinin gelmesini bekleyin.
    - Olaylar için zaman damgası iş başlangıç saatinden önce gelir ve bu nedenle olaylar bırakılır.

5.  Olay sıralama ilkelerinin beklendiği gibi yapılandırıldığından emin olun. **Ayarlar'a** gidin ve [**Olay Sırası'nı**](stream-analytics-out-of-order-and-late-events.md)seçin. Sorguyu sınamak için **Test** düğmesini kullandığınızda ilke *uygulanmaz.* Bu sonuç, tarayıcı içi testi ve üretimde işi çalıştırmak arasındaki bir farktır. 

6. Denetim ve tanılama günlüklerini kullanarak hata ayıklama:
    - Hataları tanımlamak ve hata ayıklamak için [Denetim Günlükleri'ni](../azure-resource-manager/resource-group-audit.md)ve filtreyi kullanın.
    - Hataları tanımlamak ve hata ayıklamak için [iş tanılama günlüklerini](stream-analytics-job-diagnostic-logs.md) kullanın.

## <a name="job-is-consuming-too-many-streaming-units"></a>İş çok fazla Akış Birimi tüketiyor
Azure Akış Analizi'nde paralelleştirmeden yararlandığınızdan emin olun. Giriş bölümlerini yapılandırarak ve analitik sorgu tanımını ayarlayarak Akış Analizi işlerinin [sorgu paralellemesi ile ölçeklendirmeyi](stream-analytics-parallelization.md) öğrenebilirsiniz.

## <a name="debug-queries-progressively"></a>Sorgularda aşamalı olarak hata ayıklama

Gerçek zamanlı veri işlemede, sorgunun ortasındaki verilerin nasıl göründüğünü bilmek yararlı olabilir. Bir Azure Akışı Analizi işinin girdileri veya adımları birden çok kez okunabildiği için, ek SELECT INTO deyimleri yazabilirsiniz. Bunu yapmak, ara verileri depolamaalanına verir ve tıpkı bir programı hata ayıklamada *izleme değişkenlerinin* yaptığı gibi verilerin doğruluğunu incelemenize olanak tanır.

Bir Azure Akış Analizi işinde ki aşağıdaki örnek sorguda bir akış girişi, iki başvuru veri girişi ve Azure Tablo Depolamasına çıktı vardır. Sorgu, ad ve kategori bilgilerini almak için olay merkezinden gelen verileri ve iki başvuru bloblarını birleştirir:

![Örnek Akış Analizi SELECT INTO sorgusu](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

İşin çalıştığını, ancak çıktıda hiçbir olay üretilmediğini unutmayın. Burada gösterilen **İzleme** döşemesinde, girişin veri ürettiğini görebilirsiniz, ancak **JOIN'in** hangi adımının tüm olayların düşmesine neden olduğunu bilmiyorsunuz.

![Akış Analizi İzleme karosu](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)

Bu durumda, ara JOIN sonuçlarını ve girişten okunan verileri "günlüğe kaydetmek" için birkaç ekstra SELECT INTO ifadeleri ekleyebilirsiniz.

Bu örnekte, iki yeni "geçici çıktı" ekledik. İstediğin gibi bir lavabo olabilirler. Burada Azure Depolama'yı örnek olarak kullanıyoruz:

![Akış Analizi sorgusuna ekstra SELECT INTO ifadeleri ekleme](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

Daha sonra sorguyu şu şekilde yeniden yazabilirsiniz:

![SELECT INTO Akış Analizi sorgusunun yeniden yazılması](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

Şimdi işe tekrar başlayın ve birkaç dakika çalışmasına izin verin. Ardından aşağıdaki tabloları oluşturmak için Visual Studio Cloud Explorer ile temp1 ve temp2'yi sorgulayın:

**temp1 tablosu**
![SELECT INTO temp1 tablo Akış Analizi sorgusu](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)

**temp2 tablo**
![SELECT INTO temp2 tablo Akış Analizi sorgusu](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)

Gördüğünüz gibi, temp1 ve temp2 her ikisi de veri var ve ad sütunu doğru temp2 doldurulur. Ancak, çıktıda hala veri olmadığından, bir sorun vardır:

![Veri Akışı Analizi sorgusu olmayan output1 tablosuna SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

Verileri örnekleyerek, sorunun ikinci JOIN ile olduğundan neredeyse emin olabilirsiniz. Kaynaktan başvuru verilerini indirebilir ve bir göz atabilirsiniz:

![REF TABLOSUNA SELECT Stream Analytics sorgusu](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

Gördüğünüz gibi, bu başvuru verilerindeki GUID biçimi, geçici 2'deki [gönderen] sütunun biçiminden farklıdır. Bu nedenle veriler beklendiği gibi output1'e ulaşmadı.

Veri biçimini düzeltebilir, başvuru blob'una yükleyebilir ve yeniden deneyebilirsiniz:

![SELECT INTO temp table Stream Analytics sorgusu](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

Bu kez, çıktıdaki veriler biçimlendirilir ve beklendiği gibi doldurulur.

![SELECT INTO final tablosu Stream Analytics sorgusu](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için [Azure Akışı Analizi forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
