---
title: Akış Analizi tümleştirmesini kullanarak veri akışı (önizleme)
description: Verileri bir Azure SQL veritabanına aktarmak için Azure Akış Analizi'ni kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: ajetasin
ms.author: ajetasi
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: d233d3c98cc495e4b9e84142781f5eb9faa7eec8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73820835"
---
# <a name="stream-data-by-using-azure-sql-database-stream-analytics-integration-preview"></a>Azure SQL Veritabanı Akışı Analizi tümleştirmesini kullanarak veri akışı (önizleme)

Kullanıcılar artık [Azure Akış Analitiği'ni](../stream-analytics/stream-analytics-introduction.md)kullanarak azure portalındaki bir SQL veritabanından gerçek zamanlı akış verilerini doğrudan bir tabloya alabilir, işleyebilir, görüntüleyebilir ve analiz edebilir. Bu deneyim, bağlı araç, uzaktan izleme, sahtekarlık algılama ve daha birçok senaryo gibi çok çeşitli senaryolar sağlar. Azure portalında bir etkinlik kaynağı (Olay Hub/IoT Hub) seçebilir, gelen gerçek zamanlı olayları görüntüleyebilir ve olayları depolamak için bir tablo seçebilirsiniz. Ayrıca, gelen olayları dönüştürmek ve bunları seçili tabloda depolamak için portala Akış Analizi Sorgu Dili sorguları da yazabilirsiniz. Bu yeni giriş noktası, Stream Analytics'te zaten var olan oluşturma ve yapılandırma deneyimlerine ek olarak gelir. Bu deneyim veritabanınızın bağlamından başlar ve bir Akış Analizi işi hızla ayarlamanızı ve Azure SQL Veritabanı ve Akış Analizi deneyimleri arasında sorunsuz bir şekilde gezinmenizi sağlar.

![Akış Analizi akışı](media/sql-database-stream-analytics/stream-analytics-flow.png)

## <a name="key-benefits"></a>Önemli avantajlar

- Minimum bağlam geçişi: Portaldaki bir SQL Veritabanından başlayabilir ve başka bir hizmete geçmeden gerçek zamanlı verileri tabloya sindirebilirsiniz. 
- Azaltılmış adım sayısı: Veritabanınızın ve tablonuzun bağlamı, Bir Akış Analizi işini önceden yapılandırmak için kullanılır.
- Önizleme verileriyle ek kullanım kolaylığı: Etkinlik kaynağından gelen verileri (Olay Hub'ı/IoT Hub) seçili tablo bağlamında önizleme 


## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için aşağıdaki kaynaklar gereklidir:

- Azure aboneliği. Azure aboneliğiniz yoksa, ücretsiz [bir hesap oluşturun.](https://azure.microsoft.com/free/) 
- Bir SQL veritabanı. Ayrıntılar için Azure [SQL Veritabanı'nda tek bir veritabanı oluşturma'ya](sql-database-single-database-get-started.md)bakın.
- Bilgisayarınızın Azure SQL sunucusuna bağlanmasına izin veren bir güvenlik duvarı kuralı. Ayrıntılar için bkz. [sunucu düzeyinde güvenlik duvarı kuralı oluştur.](sql-database-server-level-firewall-rule.md)


## <a name="configure-stream-analytics-integration"></a>Akış analizi entegrasyonunu yapılandırın

1. Azure Portal’da oturum açın. 
2. Akış verilerinizi yutmak istediğiniz SQL veritabanınıza gidin. **Akış analizini (önizleme)** seçin. 

    ![Stream Analytics](media/sql-database-stream-analytics/stream-analytics.png)

3. Akış verilerinizi bu SQL veritabanına almaya başlamak **için, Oluştur'u** seçin ve akış işinize bir ad verin ve sonra **Sonraki: Giriş'i**seçin. 

    ![Stream Analytics işi oluşturun](media/sql-database-stream-analytics/create-job.png)

4. Etkinlik kaynağı bilgilerinizi girin ve sonra **Sonraki: Çıktı'yı**seçin.

   - **Giriş türü**: Event Hub/IoT Hub
   - **Giriş takma adı**: Olay kaynağınızı tanımlamak için bir ad girin 
   - **Abonelik**: SQL Veritabanı aboneliği ile aynı 
   - **Olay Hub ad alanı**: Ad alanı için ad 
   - **Olay Hub adı**: Seçili ad alanı içindeki olay merkezinin adı 
   - **Olay Hub ilke adı** (Varsayılan yeni oluşturmak için): Bir ilke adı verin 
   - **Olay Hub tüketici grubu** (Varsayılan yeni oluşturmak için): Bir tüketici grubu adı verin  
     - Buradan oluşturduğunuz her yeni Azure Akışı Analizi işi için bir tüketici grubu ve bir ilke oluşturmanızı öneririz. Tüketici grupları yalnızca beş eşzamanlı okuyucuya izin verir, bu nedenle her iş için özel bir tüketici grubu sağlamak, bu sınırı aşmaktan doğabilecek hataları önler. Özel bir ilke, anahtarınızı döndürmenize veya diğer kaynakları etkilemeden izinleri iptal etmenizi sağlar.

    ![Stream Analytics işi oluşturun](media/sql-database-stream-analytics/create-job-output.png)

5. Akış verilerinizi hangi tabloya yutmak istediğinizi seçin. Bittikten sonra **Oluştur'u**seçin.
   - **Kullanıcı Adı**, **Şifre**: SQL sunucu kimlik doğrulaması için kimlik bilgilerinizi girin. **Doğrula**'yı seçin.
   - **Tablo**: **Yeni Oluştur'u** seçin veya **varolan kullan.** Bu akışta **Oluştur'u**seçelim. Bu, akış Analytics işine başladığınızda yeni bir tablo oluşturur.

    ![Stream Analytics işi oluşturun](media/sql-database-stream-analytics/create.png)

6. Bir sorgu sayfası aşağıdaki ayrıntıları ile açılır:

   - Verileri sindireceğiniz **Giriş** (giriş olayları kaynağı)  
   - Dönüştürülmüş verileri depolayacak **Çıktınız** (çıktı tablosu) 
   - SELECT deyimi ile örnek [SAQL sorgusu.](../stream-analytics/stream-analytics-stream-analytics-query-patterns.md) 
   - **Giriş önizlemesi**: Giriş olayları kaynağından gelen en son verilerin anlık görüntüsünü gösterir.  
     - Verilerinizdeki serileştirme türü otomatik olarak algılanır (JSON/CSV). El ile JSON/CSV/AVRO olarak da değiştirebilirsiniz. 
     - Gelen verileri Tablo biçiminde veya Raw biçiminde önizleyebilirsiniz. 
     - Gösterilen verileriniz güncel değilse, en son olayları görmek için **Yenile'yi** seçin. 
     - Sorgunuzu belirli bir zaman aralığına karşı test etmek için **zaman aralığını seçin.** 
     - Örnek bir JSON/CSV dosyası yükleyerek sorgunuzu test etmek için **Upload örnek girişi'ni** seçin. Bir SAQL sorgusunun sınanması hakkında daha fazla bilgi için [bkz.](../stream-analytics/stream-analytics-test-query.md) 

    ![test sorgusu](media/sql-database-stream-analytics/test-query.png)


   - **Test sonuçları**: **Test sorgusunu** seçin ve akış sorgunuzun sonuçlarını görebilirsiniz 

    ![test sonuçları](media/sql-database-stream-analytics/test-results.png)

   - **Test sonuçları şeması**: Test ten sonra akış sorgunuzun sonuçlarının şemasını gösterir. Test sonuçlarının şema nın çıktı şemanızla eşleştiğinden emin olun. 

    ![test sonuçları şema](media/sql-database-stream-analytics/test-results-schema.png)


   - **Çıktı şeması**: Adım 5'te (yeni veya varolan) seçtiğiniz tablonun şemasını içerir.
     - Yeni oluştur: Bu seçeneği adım 5'te seçtiyseniz, akış işine başlayana kadar şema'yı henüz görmezsiniz. Yeni bir tablo oluştururken, uygun tablo dizini seçin. Tablo dizini hakkında daha fazla bilgi için, [Açıklanan Kümelenmiş ve Kümesiz Dizinler'e](/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described/)bakın.
    - Varolan kullanın: Bu seçeneği adım 5'te seçtiyseniz, seçili tablonun şemasını görürsünüz. 
 
7. Sorguyu & yazmayı bitirdikten sonra **sorguyu kaydet'i**seçin. Dönüştürülmüş verileri SQL tablosuna almaya başlamak için **Başlat Akışı Analizi işini** seçin. Aşağıdaki alanları sonlandırmak sonra, işe **başlayın.** 
   - **Çıktı başlangıç saati**: Bu, işin ilk çıktısının saatini tanımlar.  
     - Şimdi: İş şimdi başlayacak ve yeni gelen verileri işleyecek.
     - Özel: İş şimdi başlayacak, ancak verileri belirli bir zaman noktasından (geçmişte veya gelecekte olabilir) işleyecektir. Daha fazla bilgi için Azure [Akış Analizi işini nasıl başlatılabilirsiniz.](../stream-analytics/start-job.md)
   - **Akış birimleri**: Azure Akış Analizi, verileri hizmete işlemek için gereken akış birimi sayısına göre fiyatlandırılır. Daha fazla bilgi için Azure [Akış Analizi fiyatlandırması](https://azure.microsoft.com/pricing/details/stream-analytics/)bölümüne bakın. 
   - **Çıktı veri hata işleme:**  
     - Yeniden deneme: Bir hata oluştuğunda, Azure Akış Analizi yazma başarılı olana kadar olayı süresiz olarak yeniden çalışır. Yeniden denemeler için zaman arası yoktur. Sonunda, sonraki tüm olayların yeniden denemeye gelen olay tarafından işlenmesi engellenir. Bu seçenek varsayılan çıktı hata işleme ilkesidir. 
     - Bırakma: Azure Akış Analizi, veri dönüştürme hatasıyla sonuçlanan tüm çıktı olaylarını düşürür. Bırakılan olaylar daha sonra yeniden işleme için kurtarılamaz. Tüm geçici hatalar (örneğin, ağ hataları) çıktı hata işleme ilkesi yapılandırması ne olursa olsun yeniden denener. 
   - **SQL Veritabanı çıktı ayarları**: Tabloya birden çok yazarla tam paralel topoloji sağlamak için önceki sorgu adımınızın bölümleme düzenini devralma seçeneğidir. Daha fazla bilgi için Azure [Akış Analizi çıktısı için Azure SQL Veritabanı'na](../stream-analytics/stream-analytics-sql-output-perf.md)bakın.
   - **Maksimum toplu iş sayısı**: Her toplu ekleme işlemiyle gönderilen kayıt sayısında önerilen üst sınırdır.  
    Çıktı hata işleme hakkında daha fazla bilgi için [Azure Akış Analizi'nde Çıktı hata ilkeleri](../stream-analytics/stream-analytics-output-error-policy.md)bölümüne bakın.  

    ![işe başlama](media/sql-database-stream-analytics/start-job.png)

8. İşe başladıktan sonra, Listede Çalışan işi görürsünüz ve aşağıdaki işlemleri yapabilirsiniz: 
   - **İşi başlat/durdur :** İş çalışıyorsa, işi durdurabilirsiniz. İş durdurulursa, işe başlayabilirsin. 
   - **İşi edit**: Sorguyu dayapabilirsiniz. İş ex'te daha fazla değişiklik yapmak istiyorsanız, daha fazla giriş/çıkış ekleyin ve akışı Stream Analytics'te açın. İş çalışırken edit seçeneği devre dışı bırakılır. 
   - **Önizleme çıktı tablosu**: SQL sorgu düzenleyicisinde tabloyu önizleyebilirsiniz. 
   - **Stream Analytics'te Aç**: İşin izleme, hata ayıklama ayrıntılarını görüntülemek için Stream Analytics hizmetinde işi açın. 


    ![akış analizi işleri](media/sql-database-stream-analytics/jobs.png)






## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stream Analytics belgeleri](https://docs.microsoft.com/azure/stream-analytics/)
- [Azure Stream Analytics çözüm desenleri](../stream-analytics/stream-analytics-solution-patterns.md)
