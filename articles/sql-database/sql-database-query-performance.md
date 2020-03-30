---
title: Sorgu Performansı İçgörüleri
description: Sorgu performans izleme, Azure SQL veritabanında tek ve havuzlu veritabanları için en çok CPU tüketen ve uzun süren sorguları tanımlar.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 03/10/2020
ms.openlocfilehash: f5998fde6659715de4fcb533cb0f41a8939b1c48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214055"
---
# <a name="query-performance-insight-for-azure-sql-database"></a>Azure SQL Veritabanı için Sorgu Performans Öngörüsü

Query Performance Insight, tek ve havuzlu veritabanları için akıllı sorgu çözümlemesi sağlar. İş yüklerinizdeki en iyi kaynak tüketen ve uzun süredir devam eden sorguları belirlemeye yardımcı olur. Bu, genel iş yükü performansını artırmak ve ödeme yaptığınız kaynağı verimli bir şekilde kullanmak için sorguları bulmanıza yardımcı olur. Sorgu Performans Öngörüsü, veritabanı performansını giderme konusunda daha az zaman harcamanıza yardımcı olur:

* Veritabanları kaynak (DTU) tüketimi hakkında daha derin bilgiler
* CPU, süre ve yürütme sayısına göre en iyi veritabanı sorguları ayrıntıları (performans iyileştirmeleri için potansiyel ayar adayları)
* Sorgunun ayrıntılarını detaya inme, sorgu metnini ve kaynak kullanım geçmişini görüntüleme becerisi
* [Veritabanı danışmanlarından](sql-database-advisor.md) performans önerileri gösteren ek açıklamalar

![Sorgu Performansı İçgörüleri](./media/sql-database-query-performance/opening-title.png)

## <a name="prerequisites"></a>Ön koşullar

Sorgu Performans Öngörüsü, [Query Store'un](https://msdn.microsoft.com/library/dn817826.aspx) veritabanınızda etkin olduğunu gerektirir. Varsayılan olarak tüm Azure SQL veritabanları için otomatik olarak etkinleştirilir. Sorgu Mağazası çalışmıyorsa, Azure portalı bunu etkinleştirmenizi ister.

> [!NOTE]
> Portalda "Sorgu Deposu bu veritabanında düzgün şekilde yapılandırılmamışsa" iletisi görünürse, [bkz.](#optimize-the-query-store-configuration)

## <a name="permissions"></a>İzinler

Sorgu Performans Öngörüsyün'ünkullanımı için aşağıdaki [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) izinlerine ihtiyacınız vardır:

* **Okuyucu**, **Sahibi**, **Katılımcı**, **SQL DB Katılımcısı**veya **SQL Server Katılımcısı** izinleri en iyi kaynak tüketen sorguları ve grafikleri görüntülemek için gereklidir.
* **Sorgu**metnini görüntülemek için Sahibi , **Katılımcısı**, **SQL DB Katılımcısı**veya **SQL Server Katılımcısı** izinleri gereklidir.

## <a name="use-query-performance-insight"></a>Sorgu Performansı İçgörüleri’ni kullanma

Sorgu Performans Öngörüsi kullanımı kolaydır:

1. Azure [portalını](https://portal.azure.com/) açın ve incelemek istediğiniz bir veritabanı bulun.
2. Sol taraftaki menüden **Akıllı Performans** > **Sorgusu Performans Öngörüsünün açını.**
  
   ![Menüde Performans Öngörüsünün Sorgula](./media/sql-database-query-performance/tile.png)

3. İlk sekmede, en çok kaynak tüketen sorgular listesini gözden geçirin.
4. Ayrıntılarını görüntülemek için tek bir sorgu seçin.
5. **Akıllı Performans** > **Performansı önerilerini** açın ve performans önerileri olup olmadığını kontrol edin. Yerleşik performans önerileri hakkında daha fazla bilgi için [SQL Database Advisor'a](sql-database-advisor.md)bakın.
6. Gözlenen aralığı değiştirmek için kaydırıcıları veya yakınlaştırma simgelerini kullanın.

   ![Performans panosu](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> SQL Veritabanı'nın Sorgu Performans Öngörüsü'ndeki bilgileri işlemesi için Sorgu Deposu'nun birkaç saatlik verileri yakalaması gerekir. Veritabanında etkinlik yoksa veya Sorgu Deposu belirli bir süre boyunca etkin değilse, Query Performance Insight bu zaman aralığını gösterdiğinde grafikler boş olur. Çalışmıyorsa Sorgu Mağazası'nı istediğiniz zaman etkinleştirebilirsiniz. Daha fazla bilgi için [Sorgu Mağazası ile en iyi uygulamalara](https://docs.microsoft.com/sql/relational-databases/performance/best-practice-with-the-query-store)bakın.
>

Veritabanı performans önerileri için Sorgu Performans Öngörüsü gezinti bıçağındaki [Öneriler'i](sql-database-advisor.md) seçin.

![Öneriler sekmesi](./media/sql-database-query-performance/ia.png)

## <a name="review-top-cpu-consuming-queries"></a>En iyi CPU tüketen sorguları gözden geçirin

Varsayılan olarak, Sorgu Performans Öngörüsü ilk açtığınızda ilk kez cpu tüketen ilk beş sorguyu gösterir.

1. Onay kutularını kullanarak bunları içerecek veya grafikten çıkarmak için tek tek sorguları seçin veya temizleyin.

   Üst satırveritabanı için genel DTU yüzdesini gösterir. Çubuklar, seçili aralıkta tüketilen seçili sorguların CPU yüzdesini gösterir. Örneğin, **Geçmiş hafta** seçilirse, her çubuk tek bir günü temsil eder.

   ![En çok gelen sorgular](./media/sql-database-query-performance/top-queries.png)

   > [!IMPORTANT]
   > Gösterilen DTU satırı, bir saatlik dönemlerde maksimum tüketim değerine toplanır. Yalnızca sorgu yürütme istatistikleriyle üst düzey bir karşılaştırma içindir. Bazı durumlarda, DTU kullanımı yürütülen sorgulara göre çok yüksek görünebilir, ancak bu durum böyle olmayabilir.
   >
   > Örneğin, bir sorgu yalnızca birkaç dakika için DTU'yu %100'e çıkardıysa, Sorgu Performans Öngörüsündeki DTU satırı tüketim saatinin tamamını %100 (maksimum toplam değerin sonucu) olarak gösterir.
   >
   > Daha hassas bir karşılaştırma için (bir dakikaya kadar), özel bir DTU kullanım grafiği oluşturmayı düşünün:
   >
   > 1. Azure portalında **Azure SQL Veritabanı** > **İzleme'yi**seçin.
   > 2. **Ölçümler**’i seçin.
   > 3. **+Ekle grafiği'ni**seçin.
   > 4. Grafikteki DTU yüzdesini seçin.
   > 5. Ayrıca, sol üst menüde **Son 24 saat'i** seçin ve bir dakikaya değiştirin.
   >
   > Sorgu yürütme grafiğiyle karşılaştırmak için daha ince ayrıntı düzeyine sahip özel DTU grafiğini kullanın.

   Alt ızgara, görünür sorgular için toplu bilgileri gösterir:

   * Veritabanındaki sorgu için benzersiz bir tanımlayıcı olan Sorgu Kimliği.
   * Toplama işlevine bağlı olarak gözlemlenebilir bir aralık sırasında sorgu başına CPU.
   * Toplama işlevine de bağlı olan sorgu başına süre.
   * Belirli bir sorgu için toplam yürütme sayısı.

2. Verileriniz eskihaline **gelirse, Yenile** düğmesini seçin.

3. Gözlem aralığını değiştirmek ve tüketim artışlarını araştırmak için kaydırıcıları ve yakınlaştırma düğmelerini kullanın:

   ![Aralığı değiştirmek için kaydırıcılar ve yakınlaştırma düğmeleri](./media/sql-database-query-performance/zoom.png)

4. İsteğe bağlı olarak, görünümü özelleştirmek için **Özel** sekmesini seçebilirsiniz:

   * Metrik (CPU, süre, yürütme sayısı).
   * Zaman aralığı (son 24 saat, geçen hafta veya geçen ay).
   * Sorgu sayısı.
   * Toplama işlevi.
  
   ![Özel sekme](./media/sql-database-query-performance/custom-tab.png)
  
5. Özelleştirilmiş görünümü görmek için **git >** düğmesini seçin.

   > [!IMPORTANT]
   > Sorgu Performans Öngörüsü, seçiminize bağlı olarak en çok tüketen 5-20 sorguyu görüntülemekle sınırlıdır. Veritabanınız gösterilen en iyi sorguların ötesinde çok daha fazla sorgu çalıştırabilir ve bu sorgular grafiğe dahil edilmez.
   >
   > Gösterilen üst sorguların ötesinde, sık sık çalışan ve DTU'nun çoğunluğunu kullanan çok sayıda küçük sorgunun bulunduğu bir veritabanı iş yükü türü olabilir. Bu sorgular performans grafiğinde görünmez.
   >
   > Örneğin, bir sorgu, gözlenen dönemdeki toplam tüketimi diğer en çok tüketen sorgulardan daha az olmasına rağmen, bir süre için önemli miktarda DTU tüketmiş olabilir. Böyle bir durumda, bu sorgunun kaynak kullanımı grafikte görünmez.
   >
   > Sorgu Performansı Öngörüsü sınırlamalarının ötesinde en iyi sorgu yürütmelerini anlamanız gerekiyorsa, gelişmiş veritabanı performans izleme ve sorun giderme için [Azure SQL Analytics'i](../azure-monitor/insights/azure-sql.md) kullanmayı düşünün.
   >

## <a name="view-individual-query-details"></a>Tek tek sorgu ayrıntılarını görüntüleme

Sorgu ayrıntılarını görüntülemek için:

1. En iyi sorgular listesindeki herhangi bir sorguyu seçin.

    ![En iyi sorgular listesi](./media/sql-database-query-performance/details.png)

   Ayrıntılı bir görünüm açılır. CPU tüketimini, süresini ve yürütme sayısını zaman içinde gösterir.

2. Ayrıntılar için grafik özelliklerini seçin.

   * Üst grafik, genel veritabanı DTU yüzdesini içeren bir çizgi gösterir. Çubuklar, seçili sorgunun tükettiği CPU yüzdesidir.
   * İkinci grafik, seçili sorgunun toplam süresini gösterir.
   * Altgrafik, seçili sorguya göre toplam yürütme sayısını gösterir.

   ![Sorgu ayrıntıları](./media/sql-database-query-performance/query-details.png)

3. İsteğe bağlı olarak, kaydırıcıları kullanın, yakınlaştırma düğmelerini kullanın veya sorgu verilerinin nasıl görüntüleneceğini özelleştirmek veya farklı bir zaman aralığı seçmek için **Ayarlar'ı** seçin.

   > [!IMPORTANT]
   > Sorgu Performans Öngörüsü herhangi bir DDL sorgusu yakalamaz. Bazı durumlarda, tüm geçici sorguları yakalayamayabilir.
   >

## <a name="review-top-queries-per-duration"></a>Süre başına en iyi sorguları gözden geçirme

Sorgu Performans Öngörüsü'ndeki iki ölçüm, olası darboğazları bulmanıza yardımcı olabilir: süre ve yürütme sayısı.

Uzun süren sorgular, kaynakları daha uzun süre kilitleme, diğer kullanıcıları engelleme ve ölçeklenebilirliği sınırlama en büyük potansiyele sahiptir. Aynı zamanda optimizasyon için en iyi adaylar.

Uzun süren sorguları tanımlamak için:

1. Seçili veritabanı için Sorgu Performans Öngörüsündeki **Özel** sekmesini açın.
2. Ölçümleri **süreye**değiştirin.
3. Sorgu sayısını ve gözlem aralığını seçin.
4. Toplama işlevini seçin:

   * **Sum,** tüm gözlem aralığı için tüm sorgu yürütme süresini toplar.
   * **Max,** tüm gözlem aralığı için yürütme süresinin en fazla olduğu sorguları bulur.
   * **Avg,** tüm sorgu yürütmelerinin ortalama yürütme süresini bulur ve bu ortalamalar için en üsttekileri gösterir.

   ![Sorgu süresi](./media/sql-database-query-performance/top-duration.png)

5. Özelleştirilmiş görünümü görmek için **git >** düğmesini seçin.

   > [!IMPORTANT]
   > Sorgu görünümünü ayarlamak DTU satırını güncelleştirmez. DTU satırı her zaman aralık için maksimum tüketim değerini gösterir.
   >
   > Veritabanı DTU tüketimini daha ayrıntılı (bir dakikaya kadar) anlamak için Azure portalında özel bir grafik oluşturmayı düşünün:
   >
   > 1. Azure SQL Veritabanı > **İzleme'yi**seçin. **Azure SQL Database**
   > 2. **Ölçümler**’i seçin.
   > 3. **+Ekle grafiği'ni**seçin.
   > 4. Grafikteki DTU yüzdesini seçin.
   > 5. Ayrıca, sol üst menüde **Son 24 saat'i** seçin ve bir dakikaya değiştirin.
   >
   > Sorgu performans grafiğiyle karşılaştırmak için özel DTU grafiğini kullanmanızı öneririz.
   >

## <a name="review-top-queries-per-execution-count"></a>Yürütme sayısı başına en iyi sorguları gözden geçirme

Veritabanını kullanan bir kullanıcı uygulaması, yüksek sayıda yürütme veritabanının kendisini etkilemese ve kaynak kullanımı düşük olsa da yavaşlayabilir.

Bazı durumlarda, yüksek yürütme sayısı daha fazla ağ tur gezileri yol açabilir. Gidiş-dönüşler performansı etkiler. Ağ gecikmesine ve downstream sunucu gecikmesine tabidirler.

Örneğin, birçok veri tabanlı web sitesi her kullanıcı isteği için veritabanına yoğun olarak erişir. Bağlantı birleştirme yardımcı olsa da, veritabanı sunucusunda artan ağ trafiği ve işlem yükü performansı yavaşlatabilir. Genel olarak, en az gidiş-dönüş geziler tutun.

Sık sık yürütülen ("geveze") sorguları tanımlamak için:

1. Seçili veritabanı için Sorgu Performans Öngörüsündeki **Özel** sekmesini açın.
2. Ölçümleri yürütme **sayısıolarak değiştirin.**
3. Sorgu sayısını ve gözlem aralığını seçin.
4. Özelleştirilmiş görünümü görmek için **git >** düğmesini seçin.

   ![Sorgu yürütme sayısı](./media/sql-database-query-performance/top-execution.png)

## <a name="understand-performance-tuning-annotations"></a>Performans atonlarını anlama ek açıklamalarını anlama

Sorgu Performans Öngörüsü'nde iş yükünüzü keşfederken, grafiğin üstünde dikey çizgi bulunan simgeler görebilirsiniz.

Bu simgeler ek açıklamalardır. [SQL Database Advisor'dan](sql-database-advisor.md)performans önerileri gösterirler. Bir ek açıklama üzerinde gezinerek, performans önerileri hakkında özet bilgiler alabilirsiniz.

   ![Sorgu ek açıklama](./media/sql-database-query-performance/annotation.png)

Daha fazla anlamak veya danışmanın önerisini uygulamak istiyorsanız, önerilen eylemin ayrıntılarını açmak için simgeyi seçin. Bu etkin bir öneriyse, portaldan hemen uygulayabilirsiniz.

   ![Ek açıklama ayrıntılarını sorgula](./media/sql-database-query-performance/annotation-details.png)

Bazı durumlarda, yakınlaştırma düzeyi nedeniyle, birbirine yakın ek açıklamaları tek bir ek açıklama içine daraltılmış olabilir. Sorgu Performans Öngörüsü bunu grup ek açıklama simgesi olarak temsil eder. Grup ek açıklama simgesini seçmek, ek açıklamaları listeleyen yeni bir bıçak açar.

Sorguları ve performans tuning eylemlerini ilişkilendirmek, iş yükünüzü daha iyi anlamanıza yardımcı olabilir.

## <a name="optimize-the-query-store-configuration"></a>Sorgu Deposu yapılandırmasını optimize edin

Sorgu Performans Öngörüsi'ni kullanırken aşağıdaki Sorgu Deposu hata iletilerini görebilirsiniz:

* "Query Store bu veritabanında düzgün bir şekilde yapılandırılmamıştır. Daha fazla bilgi için buraya tıklayın."
* "Query Store bu veritabanında düzgün bir şekilde yapılandırılmamıştır. Ayarları değiştirmek için buraya tıklayın."

Bu iletiler genellikle Sorgu Deposu yeni veri toplayadığında görünür.

İlk durum, Query Store salt okunur durumda olduğunda ve parametreler en iyi şekilde ayarlandığında gerçekleşir. Bunu, veri deposunun boyutunu artırarak veya Sorgu Deposu'nu temizleyerek düzeltebilirsiniz. (Sorgu Deposu'nun temizlenirseniz, önceden toplanan tüm telemetriler kaybolur.)

   ![Mağaza ayrıntılarını sorgula](./media/sql-database-query-performance/qds-off.png)

Sorgu Deposu etkinleştirilmediğinde veya parametreler en iyi şekilde ayarlanmadığında ikinci servis talebi gerçekleşir. [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) veya Azure portalından sağlanan aşağıdaki komutları çalıştırarak bekletme ve yakalama ilkesini değiştirebilir ve Sorgu Mağazası'nı etkinleştirebilirsiniz.

### <a name="recommended-retention-and-capture-policy"></a>Önerilen bekletme ve yakalama ilkesi

İki tür bekletme ilkesi vardır:

* **Boyut tabanlı**: Bu ilke **AUTO**olarak ayarlanırsa, en büyük boyuta yaklaşıldığında verileri otomatik olarak temizler.
* **Zaman tabanlı**: Varsayılan olarak, bu ilke 30 gün olarak ayarlanır. Sorgu Deposu'nun alanı biterse, sorgu bilgilerini 30 günden daha eski bir şekilde siler.

Yakalama ilkesini şu şekilde ayarlayabilirsiniz:

* **Tüm**: Query Store tüm sorguları yakalar.
* **Otomatik**: Sorgu Mağazası, önemsiz derleme ve yürütme süresi olan seyrek sorguları ve sorguları yok sayar. Yürütme sayısı, derleme süresi ve çalışma süresi eşikleri dahili olarak belirlenir. Bu varsayılan seçenektir.
* **Yok**: Sorgu Deposu yeni sorguları yakalamayı durdurur, ancak zaten yakalanan sorgular için çalışma zamanı istatistikleri hala toplanır.

[SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) veya Azure portalından aşağıdaki komutları uygulayarak tüm ilkeleri **AUTO** ve temizleme ilkesine ayarlamanızı öneririz. (Veritabanı `YourDB` adı ile değiştirin.)

```sql
    ALTER DATABASE [YourDB]
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);
```

[SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) veya Azure portalı aracılığıyla bir veritabanına bağlanarak ve aşağıdaki sorguyu çalıştırarak Sorgu Mağazası boyutunu artırın. (Veritabanı `YourDB` adı ile değiştirin.)

```SQL
    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);
```

Bu ayarların uygulanması, sonunda Sorgu Mağazası'nın yeni sorgular için telemetri toplaması sağlar. Sorgu Mağazası'nın hemen çalışır durumda olması gerekiyorsa, aşağıdaki sorguyu SSMS veya Azure portalı üzerinden çalıştırarak Sorgu Mağazası'nı temizlemeyi seçebilirsiniz. (Veritabanı `YourDB` adı ile değiştirin.)

> [!NOTE]
> Aşağıdaki sorguyu çalıştırmak, Sorgu Deposu'nda önceden toplanan tüm izlenen telemetrileri siler.

```SQL
    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;
```

## <a name="next-steps"></a>Sonraki adımlar

Tek ve havuzlu veritabanları, esnek havuzlar, yönetilen örnekler ve örnek veritabanlarından oluşan büyük bir filonun gelişmiş performans izlemeiçin [Azure SQL Analytics'i](../azure-monitor/insights/azure-sql.md) kullanmayı düşünün.
