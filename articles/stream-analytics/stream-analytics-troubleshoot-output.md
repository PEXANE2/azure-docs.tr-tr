---
title: Azure Stream Analytics çıkışları sorunlarını giderme
description: Bu makalede Azure Stream Analytics işlerinde çıkış bağlantılarınızın sorunlarını gidermeye yönelik teknikler açıklanmaktadır.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/31/2020
ms.custom: seodec18
ms.openlocfilehash: 5652df0cf142af2ff96590368892530abcb3d667
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82133225"
---
# <a name="troubleshoot-azure-stream-analytics-outputs"></a>Azure Stream Analytics çıkışları sorunlarını giderme

Bu makalede, Azure Stream Analytics çıkış bağlantılarıyla ilgili yaygın sorunlar, çıkış sorunlarını giderme ve sorunların nasıl giderileceği açıklanmaktadır. Birçok sorun giderme adımı, Stream Analytics işiniz için kaynak ve diğer tanılama günlüklerinin etkinleştirilmesini gerektirir. Kaynak günlüklerinizi etkinleştirmediyseniz, bkz. [Azure Stream Analytics sorun giderme kaynak günlüklerini kullanarak](stream-analytics-job-diagnostic-logs.md).

## <a name="output-not-produced-by-job"></a>İş tarafından üretilmeyen çıkış

1.  Her çıkış için **Bağlantıyı Sına** düğmesini kullanarak çıkışlara bağlantıyı doğrulayın.

2.  **İzleyici** sekmesinde [**izleme ölçümleri**](stream-analytics-monitoring.md) ' ne bakın. Değerler toplanmış olduğundan ölçümler birkaç dakika gecikiyor.
   * Giriş olayları 0 ' dan büyükse, iş giriş verilerini okuyabilir. Giriş olayları 0 ' dan büyük değilse, iş girişiyle ilgili bir sorun vardır. Giriş bağlantısı sorunlarını giderme hakkında bilgi edinmek için bkz. [giriş bağlantıları sorunlarını giderme](stream-analytics-troubleshoot-input.md) .
   * Veri dönüştürme hataları 0 ' dan büyükse ve clienmbing ise, veri dönüştürme hatalarıyla ilgili ayrıntılı bilgi için bkz. [Azure Stream Analytics Data Errors](data-errors.md) .
   * Çalışma zamanı hataları 0 ' dan büyükse, işiniz veri alabilir, ancak sorguyu işlerken hatalar oluşturuyor olabilir. Hataları bulmak için [Denetim günlüklerine](../azure-resource-manager/management/view-activity-logs.md) gidin ve *başarısız* durumu filtreleyin.
   * Inputevents 0 ' dan büyükse ve OutputEvents 0 ' dan büyükse, aşağıdakilerden biri doğrudur:
      * Sorgu işleme sıfır çıkış olayıyla sonuçlandı.
      * Olaylar veya alanlar hatalı olabilir ve sorgu işlemeden sonra sıfır çıkış ile sonuçlanır.
      * İş, bağlantı veya kimlik doğrulama nedenleriyle verileri çıkış havuzuna gönderemedi.

   Daha önce bahsedilen tüm hata durumlarında, işlem günlüğü iletilerinde sorgu mantığının tüm olayları süzdüğü durumlar dışında ek ayrıntılar (ne olur dahil) açıklanmaktadır. Birden çok olayın işlenmesi hata oluşturursa, hatalar her 10 dakikada bir toplanır.

## <a name="job-output-is-delayed"></a>İş çıkışı geciktirildi

### <a name="first-output-is-delayed"></a>İlk çıkış gecikiyor

Stream Analytics işi başlatıldığında, giriş olayları okunur ama bazı durumlarda çıkışın oluşturulmasında bir gecikme olabilir.

Zamana bağlı sorgu öğelerinde büyük saat değerleri, çıkış gecikmesine katkıda bulunabilir. Büyük zaman pencereleri üzerinde doğru çıkış üretmek için akış işi, zaman penceresini dolduracak olan en son zamandan (yedi güne kadar) verileri okuyarak başlar. Bu süre boyunca, bekleyen giriş olaylarının yakalama okuması tamamlanana kadar çıkış üretilmez. Bu sorun, sistem akış işlerini yükselttiğinde işi yeniden başlatarak yüzeysel olabilir. Bu tür yükseltmeler genellikle her iki ayda bir kez gerçekleşir.

Bu nedenle, Stream Analytics sorgunuzu tasarlarken dikkatli kullanın. İşin sorgu sözdiziminde zamana bağlı öğeler için büyük bir zaman penceresi (birkaç saatten fazla yedi güne kadar) kullanırsanız, iş başlatıldığında veya yeniden başlatıldığında ilk çıktıda gecikmeye neden olabilir.  

Bu tür ilk çıkış gecikmesi için bir hafifletme, sorgu paralelleştirme tekniklerini (verileri bölümleyerek) kullanmak veya iş bitene kadar üretilen işi geliştirmek için daha fazla akış birimi eklemektir.  Daha fazla bilgi için bkz. [Stream Analytics işleri oluşturma konuları](stream-analytics-concepts-checkpoint-replay.md)

Bu faktörler, oluşturulan ilk çıktının zaman çizelgesini etkiler:

1. Pencereli toplamaların kullanımı (Tingin, atlamalı ve kayan pencereler tarafından gruplama)
   - Atlayan veya atlamalı pencere toplamaları için sonuçlar pencere zaman çerçevesi sonunda oluşturulur.
   - Kayan bir pencere için, bir olay, kayan pencere girdiğinde ya da çıktığında oluşur.
   - Büyük pencere boyutunu kullanmayı planlıyorsunuz (> 1 saat), çıktıyı daha sık görebilmeniz için hoppıng veya kayan pencere ' ı seçmeniz en iyisidir.

2. Zamana bağlı birleşimler kullanımı (DATEDIFF ile BIRLEŞTIRME)
   - Eşleşmeler, eşleşen olayların her iki tarafında da geldiğinde oluşturulur.
   - Bir eşleşme (sol dış BIRLEŞIM) olmayan veriler, sol taraftaki her bir olaya göre DATEDıFF penceresinin sonunda oluşturulur.

3. Zamana bağlı analitik işlevlerin kullanımı (ıSFIRST, LAST ve LIMIT SÜRESI ile GECIKME)
   - Analiz işlevleri için, çıktı her olay için oluşturulur, hiçbir gecikme yoktur.

### <a name="output-falls-behind"></a>Çıkış geride

İşin normal işlemi sırasında, işin çıkışının gerisinde (daha uzun ve daha uzun gecikme süresi) düşmesini fark ederseniz, bu faktörleri inceleyerek kök nedenlerini de belirleyebilirsiniz:
- Aşağı akış havuzunun kısıtlanıp kısıtlanmayacağı
- Yukarı akış kaynağının kısıtlanıp kısıtlanmayacağı
- Sorgudaki işleme mantığının işlem yoğunluğu olup olmadığı

Bu ayrıntıları görmek için, Azure portal akış işini seçin ve **iş diyagramını**seçin. Her giriş için, bölüm başına biriktirme listesi olay ölçümü vardır. Biriktirme listesi olay ölçümü artmaya devam ederse, sistem kaynaklarının kısıtlandığı bir göstergedir. Bunun nedeni, çıkış havuzu azaltma veya yüksek CPU olabilir. İş diyagramını kullanma hakkında daha fazla bilgi için, bkz. [iş diyagramını kullanarak veri odaklı hata ayıklama](stream-analytics-job-diagram-with-metrics.md).

## <a name="key-violation-warning-with-azure-sql-database-output"></a>Azure SQL veritabanı çıkışıyla anahtar ihlali uyarısı

Azure SQL veritabanını bir Stream Analytics işine çıktı olarak yapılandırdığınızda, kayıt, hedef tabloya toplu ekler. Genel olarak, Azure Stream Analytics, çıkış havuzuna [en az bir kez teslim](https://docs.microsoft.com/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics) garantisi sağlarken, bir tane, SQL tablosu tanımlı benzersiz bir KıSıTLAMA olduğunda SQL çıktısına [tam bir kez gönderim elde]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) edebilir.

SQL tablosu 'nda benzersiz anahtar kısıtlamaları kurulduktan sonra ve SQL tablosuna eklenen yinelenen kayıtlar varsa, Azure Stream Analytics yinelenen kaydı kaldırır. Verileri toplu işlemlere böler ve tek bir yinelenen kayıt bulunana kadar toplu işleri tekrar ekleyerek. Akış işinde çok sayıda yinelenen satır varsa, bu bölünmüş ve ekleme işlemi, yinelenenleri bir tane daha az etkili ve zaman alıcı olacak şekilde yok sayması gerekir. Geçmiş bir saat içinde etkinlik günlüğünde birden çok anahtar ihlali uyarı iletisi görürseniz, büyük olasılıkla SQL çıktınızın tüm işi yavaşlatıyor olması olasıdır.

Bu sorunu çözmek için, IGNORE_DUP_KEY seçeneğini etkinleştirerek anahtar ihlaline neden olan [dizini yapılandırmanız]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) gerekir. Bu seçeneğin etkinleştirilmesi, toplu eklemeler sırasında SQL tarafından yinelenen değerlerin yok sayılmasına izin verir ve SQL Azure bir hata yerine yalnızca bir uyarı iletisi üretir. Azure Stream Analytics artık birincil anahtar ihlali hataları oluşturmaz.

Birkaç dizin türü için IGNORE_DUP_KEY yapılandırırken aşağıdaki gözlemleri aklınızda edin:

* Birincil anahtarda veya ALTER INDEX kullanan benzersiz bir kısıtlamada IGNORE_DUP_KEY ayarlayamazsınız, dizini bırakıp yeniden oluşturmanız gerekir.  
* BIRINCIL anahtar/UNIQUE kısıtlamasından farklı olan ve CREATE INDEX veya Index Definition kullanılarak oluşturulan benzersiz bir dizin için ALTER INDEX kullanarak IGNORE_DUP_KEY seçeneğini ayarlayabilirsiniz.  

* Bu tür dizinlerde benzersizliği zorlayamadığı için IGNORE_DUP_KEY sütun deposu dizinlerine uygulanmaz.  

## <a name="column-names-are-lower-cased-by-azure-stream-analytics"></a>Sütun adları Azure Stream Analytics tarafından düşük
Özgün uyumluluk düzeyini (1,0) kullanırken, Azure Stream Analytics sütun adlarını küçük harfe değiştirmek için kullanılır. Bu davranış sonraki uyumluluk düzeylerinde düzeltildi. Büyük/küçük harf durumunu korumak için müşterilerin 1,1 ve üzeri uyumluluk düzeyine taşınmasını tavsiye ederiz. [Azure Stream Analytics Işlerin uyumluluk düzeyi](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level)hakkında daha fazla bilgi edinebilirsiniz.

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için [Azure Stream Analytics Forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
