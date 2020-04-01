---
title: Azure Akış Analizi çıktılarını sorun giderme
description: Bu makalede, Azure Akış Analizi işlerinde çıktı bağlantılarınızı giderme teknikleri açıklanmaktadır.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/31/2020
ms.custom: seodec18
ms.openlocfilehash: 305632a0faa1eb7e217e86d36c5159e557df7aaf
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80409263"
---
# <a name="troubleshoot-azure-stream-analytics-outputs"></a>Azure Akış Analizi çıktılarını sorun giderme

Bu makalede, Azure Akış Analizi çıktı bağlantılarıyla ilgili sık karşılaşılan sorunlar, çıktı sorunlarının nasıl giderilen ve sorunların nasıl düzeltilene açıklanmaktadır. Birçok sorun giderme adımı, Akış Analizi işiniz için tanılama günlüklerinin etkinleştirilmesini gerektirir. Tanılama günlükleriniz etkinleştirildiyse, [tanılama günlüklerini kullanarak Azure Akış Analizi Sorun Giderme](stream-analytics-job-diagnostic-logs.md)bölümüne bakın.

## <a name="output-not-produced-by-job"></a>İş tarafından üretilmeyen çıktı

1.  Her çıktı için **Test Bağlantısı** düğmesini kullanarak çıktılara bağlantıyı doğrulayın.

2.  **Monitör** sekmesindeki [**İzleme Ölçümlerine**](stream-analytics-monitoring.md) bakın. Değerler toplandığı için, ölçümler birkaç dakika geciktirilir.
   * Giriş Olayları 0'dan büyükse, iş giriş verilerini okuyabilir. Giriş Olayları 0'dan büyük değilse, işin girişiyle ilgili bir sorun vardır. Giriş bağlantısı sorunlarını nasıl gidereceklerini öğrenmek için [sorun giderme giriş bağlantılarına](stream-analytics-troubleshoot-input.md) bakın.
   * Veri Dönüştürme Hataları 0'dan büyükse ve yükseliyorsa, veri dönüştürme hataları hakkında ayrıntılı bilgi için [Azure Akışı Analizi veri hatalarına](data-errors.md) bakın.
   * Runtime Hataları 0'dan büyükse, işiniz veri alabilir, ancak sorguyu işlerken hata lar oluşturur. Hataları bulmak için Denetim [Günlükleri'ne](../azure-resource-manager/management/view-activity-logs.md) gidin ve *Başarısız* durumuna filtre uygulayın.
   * InputEvents 0'dan büyükse ve OutputEvents 0'a eşitse, aşağıdakilerden biri doğrudur:
      * Sorgu işleme sıfır çıkış olayıyla sonuçlandı.
      * Olaylar veya alanlar yanlış biçimlendirilmiş olabilir ve sorgu işleminden sonra sıfır çıktı elde edilebilir.
      * İş, bağlantı veya kimlik doğrulama nedenleriyle verileri çıktı lavaboya itemedi.

   Daha önce bahsedilen tüm hata durumlarında, işlem günlüğü iletileri sorgu mantığının tüm olayları filtrelediği durumlar dışında ek ayrıntıları (neler olduğu dahil) açıklar. Birden çok olayın işlenmesi hata lar oluşturursa, hatalar her 10 dakikada bir toplanır.

## <a name="job-output-is-delayed"></a>İş çıkışı geciktirildi

### <a name="first-output-is-delayed"></a>İlk çıkış gecikti

Stream Analytics işi başlatıldığında, giriş olayları okunur ama bazı durumlarda çıkışın oluşturulmasında bir gecikme olabilir.

Zamansal sorgu öğelerindeki büyük zaman değerleri çıktı gecikmesine katkıda bulunabilir. Büyük zaman pencerelerinden doğru çıktı üretmek için, akış işi zaman penceresini doldurmak için mümkün olan en son zaman (en fazla yedi gün önce) verileri okuyarak başlar. Bu süre zarfında, bekleyen giriş olaylarının yetişme okuması tamamlanana kadar hiçbir çıktı üretilmez. Sistem akış işlerini yükselttiğinde bu sorun ortaya çıkabilir ve böylece işi yeniden başlatabilirsiniz. Bu tür yükseltmeler genellikle birkaç ayda bir gerçekleşir.

Bu nedenle, Akış Analizi sorgunuzu tasarlarken takdir inisiyatifini kullanın. İş sorgu sözdiziminde zamansal öğeler için büyük bir zaman penceresi (birkaç saatten fazla, yedi güne kadar) kullanırsanız, iş başlatıldığında veya yeniden başlatıldığında ilk çıktıda gecikmeye neden olabilir.  

Bu tür ilk çıktı gecikmesi için bir azaltma sorgu paralelleştirme teknikleri (veri bölümleme) kullanmak veya iş yakalayana kadar iş kadar iş geliştirmek için daha fazla Akış Birimleri eklemektir.  Daha fazla bilgi [için, Akış Analizi işleri oluştururken Dikkat Edilmesi Gerekenler](stream-analytics-concepts-checkpoint-replay.md)

Bu etkenler, oluşturulan ilk çıktının güncelliklerini etkiler:

1. Pencereli agregaların kullanımı (GROUP BY yuvarlanma, atlamalı ve sürgülü pencereler)
   - Yuvarlanan veya zıplayan pencere toplamları için, sonuçlar pencere zaman diliminin sonunda oluşturulur.
   - Kayan bir pencere için, bir olay kayan pencereye girdiğinde veya çıktığında sonuçlar oluşturulur.
   - Büyük pencere boyutu (> 1 saat) kullanmayı planlıyorsanız, çıktıyı daha sık görebilmeniz için atlamalı veya kayan pencereyi seçmek en iyisidir.

2. Zamansal birleştirmelerin kullanımı (DATEDIFF ile JOIN)
   - Eşleşen olayların her iki tarafı da varır varmaz maçlar oluşturulur.
   - Datediff penceresinin sonunda sol taraftaki her olayla ilgili olarak eşleşme (LEFT OUTER JOIN) bulunmayan veriler oluşturulur.

3. Zamansal analitik fonksiyonların kullanımı (LIMIT SÜRELİ ISFIRST, LAST ve LAG)
   - Analitik işlevler için, çıktı her olay için oluşturulur, gecikme yoktur.

### <a name="output-falls-behind"></a>Çıkış geride kalıyor

İşin normal çalışması sırasında, işin çıktısının geri kaldığını görürseniz (daha uzun ve daha uzun gecikme süresi), aşağıdaki faktörleri inceleyerek temel nedenleri belirleyebilirsiniz:
- Downstream lavabo nun daraltılmış olup olmadığı
- Yukarı akış kaynağının daraltılmış olup olmadığı
- Sorgudaki işlem mantığının bilgi işlem yoğun olup olmadığı

Bu ayrıntıları Görmek için Azure portalında akış işini seçin ve **İş diyagramını**seçin. Her giriş için, bölüm başına biriktirme listesi olay ölçümü vardır. Biriktirme listesi olay ölçümü artmaya devam ederse, sistem kaynaklarının kısıtlanmış olduğunun bir göstergesidir. Potansiyel olarak bu çıkış lavabo azaltma veya yüksek CPU kaynaklanmaktadır. İş diyagramını kullanma hakkında daha fazla bilgi için [iş diyagramını kullanarak Veri tabanlı hata ayıklama bölümüne](stream-analytics-job-diagram-with-metrics.md)bakın.

## <a name="key-violation-warning-with-azure-sql-database-output"></a>Azure SQL Veritabanı çıktısı ile anahtar ihlali uyarısı

Azure SQL veritabanını bir Akış Analizi işine çıktı olarak yapılandırırken, hedef tabloya toplu kayıt ekler. Genel olarak, Azure akış analitiği çıktı lavaboya [en az bir kez teslim](https://docs.microsoft.com/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics) garantisi, SQL tablosu benzersiz bir kısıtlama tanımlı olduğunda sql çıkışına tam olarak bir kez teslim [elde]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) edilebilir.

SQL tablosunda benzersiz anahtar kısıtlamaları ayarlandıktan ve SQL tablosuna yinelenen kayıtlar ekildikten sonra, Azure Akış Analizi yinelenen kaydı kaldırır. Verileri toplu gruplara böler ve tek bir yinelenen kayıt bulunana kadar toplu iş birliğini özyinelemeyle ekler. Akış işi önemli sayıda yinelenen satıra sahipse, bu bölme ve ekleme işlemi, daha az verimli ve zaman alan yinelenenleri tek tek yoksaymak zorundadır. Son bir saat içinde Etkinlik günlüğünde birden çok anahtar ihlali uyarı iletisi görürseniz, SQL çıkışınızın tüm işi yavaşlatıyor olması olasıdır.

Bu sorunu gidermek için, IGNORE_DUP_KEY seçeneğini etkinleştirerek anahtar ihlaline neden olan [dizini yapılandırmanız]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) gerekir. Bu seçeneği etkinleştirmek, yinelenen değerlerin toplu ekler sırasında SQL tarafından yoksayılmasına olanak tanır ve SQL Azure hata yerine bir uyarı iletisi üretir. Azure Akış Analizi artık birincil anahtar ihlali hataları üretmez.

Çeşitli dizin türleri için IGNORE_DUP_KEY yapılandırırken aşağıdaki gözlemlere dikkat edin:

* IGNORE_DUP_KEY bir birincil anahtara veya ALTER INDEX kullanan benzersiz bir kısıtlamaya ayarlayamazsınız, dizini bırakmanız ve yeniden oluşturmanız gerekir.  
* IGNORE_DUP_KEY seçeneğini, BIRINCIL ANAHTAR/BENZERSIZ kısıtlamasından farklı olan ve CREATE INDEX veya INDEX tanımı kullanılarak oluşturulan benzersiz bir dizin için ALTER INDEX'i kullanarak ayarlayabilirsiniz.  

* IGNORE_DUP_KEY sütun deposu dizinleri için geçerli değildir, çünkü bu tür dizinlerde benzersizliği zorlayamadığınız için.  

## <a name="column-names-are-lower-cased-by-azure-stream-analytics"></a>Sütun adları Azure Akış Analizi tarafından küçük harfle belirlenir
Azure Akış Analizi, özgün uyumluluk düzeyini (1.0) kullanırken sütun adlarını küçük harfle değiştirmek için kullanılır. Bu davranış daha sonraki uyumluluk düzeylerinde düzeltildi. Servis talebinin korunması için, müşterilere uyumluluk düzeyi 1.1 ve sonraki düzeye geçmelerini tavsiye ederiz. Azure Akış Analizi [işlerinin Uyumluluk düzeyi](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level)hakkında daha fazla bilgi bulabilirsiniz.

## <a name="get-help"></a>Yardım alın

Daha fazla yardım için [Azure Akışı Analizi forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
