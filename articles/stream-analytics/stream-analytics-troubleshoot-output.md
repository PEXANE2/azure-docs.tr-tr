---
title: Azure Stream Analytics çıkışları sorunlarını giderme
description: Bu makalede Azure Stream Analytics işlerinde çıkış bağlantılarınızın sorunlarını gidermeye yönelik teknikler açıklanmaktadır.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: d40157523a074547885a14a3d92379f8e8b6f351
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79254293"
---
# <a name="troubleshoot-azure-stream-analytics-outputs"></a>Azure Stream Analytics çıkışları sorunlarını giderme

Bu sayfada çıkış bağlantılarıyla ilgili yaygın sorunlar ve bunların nasıl giderileceği ve ele alınacağını açıklanmaktadır.

## <a name="output-not-produced-by-job"></a>İş tarafından üretilmeyen çıkış
1.  Her çıkış için **Bağlantıyı Sına** düğmesini kullanarak çıkışlara bağlantıyı doğrulayın.

2.  **İzleyici** sekmesinde [**izleme ölçümleri**](stream-analytics-monitoring.md) ' ne bakın. Değerler toplanmış olduğundan ölçümler birkaç dakika gecikiyor.
    - Giriş olayları 0 >, iş giriş verilerini okuyabilir. Giriş olayları > 0 değilse:
      - Veri kaynağının geçerli verilere sahip olup olmadığını görmek için [Service Bus Gezginini](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a)kullanarak denetleyin. Bu denetim, iş Olay Hub 'ını giriş olarak kullanıyorsa geçerlidir.
      - Veri serileştirme biçiminin ve veri kodlamasının beklenip beklenmediğini denetleyin.
      - İş bir olay hub 'ı kullanıyorsa, ileti gövdesinin *null*olup olmadığını kontrol edin.

    - Veri dönüştürme hataları 0 > ve clienmbing ise, aşağıdakiler doğru olabilir:
      - Çıkış olayı, hedef havuzun şemasına uymuyor.
      - Olay şeması, sorgudaki olayların tanımlı veya beklenen şemasıyla eşleşmeyebilir.
      - Olaydaki bazı alanların veri türleri beklentileri eşleşmeyebilir.

    - Çalışma zamanı hataları 0 >, işin verileri alabileceği ancak sorguyu işlerken hata üretebileceği anlamına gelir.
      - Hataları bulmak için [Denetim günlüklerine](../azure-resource-manager/management/view-activity-logs.md) gidin ve *başarısız* durumu filtreleyin.

    - Inputevents > 0 ve OutputEvents = 0 ise, aşağıdakilerden birinin doğru olduğu anlamına gelir:
      - Sorgu işleme sıfır çıkış olayıyla sonuçlandı.
      - Olaylar veya alanları hatalı biçimlendirilmiş olabilir ve sorgu işlemeden sonra sıfır çıkış elde edilir.
      - İş, bağlantı veya kimlik doğrulama nedenleriyle verileri çıkış havuzuna gönderemedi.

    - Daha önce bahsedilen tüm hata durumlarında, işlem günlüğü iletilerinde sorgu mantığının tüm olayları süzdüğü durumlar dışında ek ayrıntılar (ne olur dahil) açıklanmaktadır. Birden çok olayın işlenmesi hata oluşturursa, Stream Analytics aynı türdeki ilk üç hata iletisini Işlem günlüklerine 10 dakika içinde günlüğe kaydeder. Daha sonra, "hataların çok hızlı bir şekilde çalıştığını, bunların gizlenmekte olduğunu" belirten bir iletiyle aynı hata hatalarını bastırır.

## <a name="job-output-is-delayed"></a>İş çıkışı gecikti

### <a name="first-output-is-delayed"></a>İlk çıkış ertelendi
Stream Analytics işi başlatıldığında, giriş olayları okunur ama bazı durumlarda çıkışın oluşturulmasında bir gecikme olabilir.

Zamana bağlı sorgu öğeleri büyük saat değerleri için çıkış gecikmesi katkıda bulunabilir. Büyük zaman pencereleri doğru çıktı oluşturmak için iş akışında zaman penceresi doldurmak için en son zaman mümkün (en fazla yedi gün önce) verilerini okuyarak başlatılır. Bekleyen giriş olaylarını yakalama okuma işlemi tamamlanana kadar bu süre boyunca hiçbir çıktı üretilmiştir. Sistem, böylece iş yeniden başlatma akış işi, yükseltildiğinde bu sorun ortaya çıkabilir. Bu tür yükseltmeler, genellikle bir kez her birkaç ay oluşur.

Bu nedenle, Stream Analytics sorgunuz tasarlarken dikkatli olun. Büyük bir zaman penceresinde'nı kullanıyorsanız (birden fazla birkaç saat ayarlama için yedi gün) iş başlatıldığında veya yeniden başlatıldığında işin sorgu söz dizimi, zamana bağlı öğeleri için bunu bir gecikme için ilk çıktı yol açabilir.  

İlk çıkış gecikme bu tür için bir risk azaltma, sorgunun paralelleştirme teknikleri (veriyi bölümlendirme) kullanın veya daha fazla akış işi arayı kapatıncaya kadar işleme oranını artırmak için birim ekleyin sağlamaktır.  Daha fazla bilgi için bkz. [Stream Analytics işleri oluşturma konuları](stream-analytics-concepts-checkpoint-replay.md)

Bu etkenler oluşturulan ilk çıkışın dakikliğini etkiler:

1. Pencereli toplamlar (grup tarafından atlaması ve windows kayan atlayan,) kullanımı
   - Atlayan veya atlamalı pencere toplamlar için sonuçları penceresi süre sonunda oluşturulur.
   - Bir olay girer veya kayan pencere çıkar, kayan bir pencere için sonuçları üretilir.
   - Büyük pencere boyutu (> 1 saat) kullanmayı planlıyorsanız, böylece çıkışı daha sık bakın atlamalı veya kayan bir pencere seçmek idealdir.

2. Zamana bağlı birleşimler (DATEDIFF katılma) kullanımı
   - Eşleşen her iki tarafında eşleşen olaylar geldiğinde hemen sonra oluşturulur.
   - Bir eşleşme (sol dış birleştirme) eksik veri sonunda, DATEDIFF pencerenin sol tarafındaki her bir olay göre oluşturulur.

3. Zamana bağlı analitik işlevler (ISFİRST, LAST ve GECİKME süresi sınırı) kullanımı
   - Analitik İşlevler, her olay için bir çıktı oluşturulur, gecikme.

### <a name="output-falls-behind"></a>Çıkış geride
İşin normal işlem sırasında işin çıktısını (uzun ve daha uzun gecikme), geride kalıyor fark ederseniz bu faktörleri inceleyerek nedenlerini saptayabilirler:
- Aşağı Akış havuz olup kısıtlandı
- Yukarı Akış kaynağı olup olmadığını kısıtlandı
- Sorgu işleme mantığı yoğun işlem gücü kullanımlı olup olmadığı

Bu ayrıntıları görmek için, Azure portal akış işini seçin ve **iş diyagramını**seçin. Her bir giriş var olan bir bölüm biriktirme listesi olay ölçüm. Biriktirme listesi olay ölçümü artmaya devam ederse, sistem kaynaklarının sınırlı olduğu bir göstergesidir. Potansiyel olarak verilecek çıkış havuzu kısıtlama veya yüksek CPU olmasıdır. İş diyagramını kullanma hakkında daha fazla bilgi için, bkz. [iş diyagramını kullanarak veri odaklı hata ayıklama](stream-analytics-job-diagram-with-metrics.md).

## <a name="key-violation-warning-with-azure-sql-database-output"></a>Azure SQL veritabanı çıkışıyla anahtar ihlali uyarısı

Azure SQL veritabanı için bir Stream Analytics işi çıktı olarak yapılandırdığınızda, yığın kayıtları hedef tabloya ekler. Genel olarak, Azure Stream Analytics, çıkış havuzuna [en az bir kez teslim](https://docs.microsoft.com/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics) garantisi sağlarken, bir tane, SQL tablosu tanımlı benzersiz bir KıSıTLAMA olduğunda SQL çıktısına [tam bir kez gönderim elde]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) edebilir.

Azure Stream Analytics, benzersiz anahtar kısıtlamaları SQL tablosunda ayarlanır ve SQL tablosuna eklenen yinelenen kayıt sonra yinelenen kayıt kaldırır. Toplu işleri ve yinelemeli olarak tek bir yinelenen kayıt bulunana kadar toplu ekleme verileri ayırır. Akış işi önemli sayıda yinelenen satır, bu bölme olduğundan ve işlem eklemek daha az verimli ve zaman alıcı olan tek, yinelenenleri yok saymak vardır. Etkinlik günlüğü'nde son bir saat içinde birden çok anahtar ihlali uyarı iletisi görürseniz, SQL çıkışınızı tüm işin yavaşlattığını olasıdır.

Bu sorunu çözmek için, IGNORE_DUP_KEY seçeneğini etkinleştirerek anahtar ihlaline neden olan [dizini yapılandırmanız]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) gerekir. Bu seçeneğin etkinleştirilmesi, SQL toplu ekleme sırasında yoksayılacak yinelenen değerler sağlar ve SQL Azure, yalnızca bir uyarı iletisi yerine bir hata üretir. Azure Stream Analytics artık birincil anahtar ihlali hatalarını üretmez.

IGNORE_DUP_KEY dizin çeşitli türleri için yapılandırırken aşağıdaki gözlemlere unutmayın:

* Bir birincil anahtar veya ALTER INDEX kullanan benzersiz kısıtlama IGNORE_DUP_KEY ayarlayamazsınız, bırakın ve dizini yeniden oluşturmanız gerekir.  
* BİRİNCİL anahtar benzersiz kısıtlamasından farklıdır ve CREATE INDEX veya dizin tanımı kullanılarak oluşturulan benzersiz bir dizin için ALTER INDEX kullanarak IGNORE_DUP_KEY seçeneği ayarlayabilirsiniz.  
* Bu dizinlerin benzersizlik olamaz çünkü IGNORE_DUP_KEY sütun deposu dizinleri için geçerli değildir.  

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
