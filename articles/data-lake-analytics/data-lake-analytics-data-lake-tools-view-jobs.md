---
title: İş Görünümü& İş Tarayıcısı'nı kullanma - Azure Veri Gölü Analizi
description: Bu makalede, Azure Veri Gölü Analizi işleri için İş Tarayıcısı ve İş Görünümü nasıl kullanılacağı açıklanmaktadır.
ms.service: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
ms.assetid: bdf27b4d-6f58-4093-ab83-4fa3a99b5650
ms.topic: conceptual
ms.date: 08/02/2017
ms.openlocfilehash: 2d33a6ec5ff6b687913914e9433b85765aaa7aec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71309945"
---
# <a name="use-job-browser-and-job-view-for-azure-data-lake-analytics"></a>Azure Data Lake Analytics için İş Tarayıcısı’nı ve İş Görünümü’nü kullanma
Azure Veri Gölü Analizi hizmet arşivleri bir sorgu deposunda iş gönderdi. Bu makalede, geçmiş iş bilgilerini bulmak için Visual Studio için Azure Veri Gölü Araçlarında İş Tarayıcısı ve İş Görünümü'nü nasıl kullanacağınızı öğreneceksiniz. 

Varsayılan olarak, Data Lake Analytics hizmeti işleri 30 gün boyunca arşivler. Son kullanma süresi, özelleştirilmiş son kullanma ilkesini yapılandırarak Azure portalından yapılandırılabilir. Son kullanma tarihinden sonra iş bilgilerine erişemeyeceksiniz. 

## <a name="prerequisites"></a>Ön koşullar
[Visual Studio ön koşullar için Veri Gölü Araçları'na](data-lake-analytics-data-lake-tools-get-started.md#prerequisites)bakın.

## <a name="open-the-job-browser"></a>İş Tarayıcısını Aç
Visual Studio'da **Sunucu Gezgini>Azure>Data Lake Analytics>İşler** aracılığıyla İş Tarayıcısı'na erişin.  İş Tarayıcısını kullanarak, bir Data Lake Analytics hesabının sorgu deposuna erişebilirsiniz. İş Tarayıcısı, temel iş bilgilerini gösteren sorgu deposunu ve sağda ayrıntılı iş bilgilerini gösteren İş Görünümü'nu görüntüler.

## <a name="job-view"></a>İş Görünümü
İş Görünümü, bir işin ayrıntılı bilgilerini gösterir. Bir işi açmak için, İş Tarayıcısı'ndaki bir işi çift tıklatabilir veya İş Görünümü'nü tıklatarak Veri Gölü menüsünden açabilirsiniz. İş URL'si ile doldurulan bir iletişim kutusu görmeniz gerekir.

![Veri Gölü Araçları Görsel Stüdyo İş Tarayıcısı](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view.png)

İş Görünümü şunları içerir:

* İş Özeti
  
    Çalışan işlerle ilgili daha yeni bilgileri görmek için İş Görünümünü yenileyin.
  
  * İş Durumu (grafik):
    
      İş Durumu, iş aşamalarını özetler:
    
      ![Azure Veri Gölü Analizi iş aşamaları durumu](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-phases.png)
    
    * Hazırlama: Komut dosyanızı buluta yükleyin, derleme hizmetini kullanarak komut dosyasını derleyip optimize edin.
    * Sıraya alındı: İşler yeterli kaynak beklerken sıralanır veya işler hesap sınırlaması başına en fazla eşzamanlı işleri aşar. Öncelik ayarı sıralı işlerin sırasını belirler - sayı ne kadar düşükse, öncelik de o kadar yüksek olur.
    * Çalışma: İş aslında Data Lake Analytics hesabınızda çalışıyor.
    * Sonuçlandırma: İş tamamlıyor (örneğin, dosyayı sonuçlandırın.
      
      İş her aşamada başarısız olabilir. Örneğin, Hazırlama aşamasındaki derleme hataları, Sıraya Aşamasındaki zaman alakart hataları ve Çalışan aşamasındaki yürütme hataları vb.
  * Temel Bilgiler
    
      Temel iş bilgileri, İş Özeti panelinin alt kısmında gösterir.
    
      ![Azure Veri Gölü Analizi iş aşamaları durumu](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-info.png)
    
    * İş Sonucu: Başarılı veya başarısız oldu. İş her aşamada başarısız olabilir.
    * Toplam Süre: Gönderme süresi ile bitiş saati arasındaki duvar saati süresi (süresi).
    * Toplam İşlem Süresi: Her vertex yürütme süresinin toplamı, bunu işin yalnızca bir tepe noktası içinde yürütüldettiği zaman olarak düşünebilirsiniz. Vertex hakkında daha fazla bilgi edinmek için Toplam Vertices bakın.
    * Gönder/Başlangıç/Bitiş Saati: Data Lake Analytics hizmetinin iş gönderimi aldığı/işe başladığı/işi başarılı bir şekilde bitirtiği veya çalıştırdığı saat.
    * Derleme/Sıralı/Çalışan: Hazırlama/Sıraya/Çalıştırma aşamasında harcanan duvar saati süresi.
    * Hesap: İşi çalıştırmak için kullanılan Veri Gölü Analizi hesabı.
    * Yazar: İşi gönderen kullanıcı, gerçek bir kişinin hesabı veya sistem hesabı olabilir.
    * Öncelik: İşin önceliği. Sayı ne kadar düşükse, öncelik de o kadar yüksektir. Yalnızca kuyruktaki işlerin sırasını etkiler. Daha yüksek bir öncelik belirlemek, çalışan işleri engellemez.
    * Paralellik: İstenen maksimum eşzamanlı Azure Veri Gölü Analiz Birimi (ADLAUs), diğer adıyla tepe ler. Şu anda, bir tepe noktası iki sanal çekirdek ve altı GB RAM ile bir VM eşittir, ancak bu gelecekteki Data Lake Analytics güncellemeleri yükseltilebilir.
    * Bayt Sol: İş tamamlanana kadar işlenmesi gereken baytlar.
    * Bayt okuma/yazma: İşe başladığından beri okunan/yazılan baytlar.
    * Toplam tepe: İş iş birçok parçaya ayrılır, işin her parçası bir tepe noktası denir. Bu değer, işin kaç parçadan oluştuğunu açıklar. Bir tepe noktalarını temel bir işlem birimi olarak düşünebilirsiniz, diğer adıyla Azure Veri Gölü Analiz Birimi (ADLAU) ve vertices paralellik içinde çalıştırılabilir. 
    * Tamamlanan/Çalıştırma/Başarısız: Tamamlanan/çalışan/başarısız olan vertices sayısı. Vertices hem kullanıcı kodu ve sistem hataları nedeniyle başarısız olabilir, ancak sistem otomatik olarak birkaç kez vertices başarısız oldu. Tepe noktası yeniden denedikten sonra hala başarısız olursa, tüm iş başarısız olur.
* İş Grafiği
  
    U-SQL komut dosyası, giriş verilerini çıktı verilerine dönüştürme mantığını temsil eder. Komut dosyası derlenir ve Hazırlama aşamasında fiziksel bir yürütme planına optimize edilir. İş Grafiği fiziksel yürütme planını göstermektir.  Aşağıdaki diyagram işlemi göstermektedir:
  
    ![Azure Veri Gölü Analizi iş aşamaları durumu](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-logical-to-physical-plan.png)
  
    Bir iş birçok parçaya ayrılır. Her bir eserin adı Vertex. Tepeler Super Vertex (aka sahne) olarak gruplandırılır ve İş Grafiği olarak görselleştirilmiştir. İş grafiğindeki yeşil sahne levhaları aşamaları gösterir.
  
    Bir aşamadaki her tepe noktası, aynı verilerin farklı parçalarıyla aynı tür işler yapıyor. Örneğin, bir TB verisi içeren bir dosyanız varsa ve ondan yüzlerce tepe bilgisi okunuyorsa, her biri bir yığın okuyor. Bu vertices aynı aşamada gruplandırılır ve aynı giriş dosyasının farklı parçaları üzerinde aynı işi yapıyor.
  
  * <a name="state-information"></a>Aşama bilgileri
    
      Belirli bir aşamada, bazı sayılar pankartta gösterilir.
    
      ![Azure Veri Gölü Analizi iş grafiği aşaması](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage.png)
    
    * SV1 Özü: Bir sayı ve işlem yöntemi ile adlandırılan bir aşamanın adı.
    * 84 vertices: Bu aşamada vertices toplam sayısı. Şekil, bu aşamada kaç parçanın bölündüğügösterir.
    * 12.90 s/vertex: Bu aşama için ortalama vertex yürütme süresi. Bu rakam SUM (her vertex yürütme süresi) / (toplam Vertex sayısı) ile hesaplanır. Bu da demek oluyor ki, paralellik içinde yürütülen tüm tepeleri atayabilirseniz, tüm aşama 12.90'da tamamlanır. Ayrıca bu aşamada tüm iş seri yapılırsa, maliyet * AVG zaman #vertices olacağı anlamına gelir.
    * 850.895 satır yazılmış: Bu aşamada yazılan toplam satır sayısı.
    * R/W: Bu aşamada baytolarak okunan/yazılan veri miktarı.
    * Renkler: Renkler farklı tepe noktası durumunu belirtmek için sahnede kullanılır.
      
      * Yeşil, tepe noktasının başarılı olduğunu gösterir.
      * Turuncu tepe noktasının yeniden denendiğini gösterir. Yeniden denenen tepe noktası başarısız oldu ancak sistem tarafından otomatik ve başarılı bir şekilde yeniden denendi ve genel aşama başarıyla tamamlandı. Tepe noktası yeniden denenmiş ancak yine de başarısız olduysa, renk kırmızıya döner ve tüm iş başarısız olur.
      * Kırmızı, belirli bir tepe noktasının sistem tarafından birkaç kez yeniden denendiği ancak yine de başarısız olduğu anlamına gelen başarısız olduğunu gösterir. Bu senaryo, tüm işin başarısız olmasını neden olur.
      * Mavi belli bir tepe noktasının aktolduğu anlamına gelir.
      * Beyaz tepe noktasının Beklediğini gösterir. Bir ADLAU kullanılabilir olduğunda tepe noktası zamanlanmayı bekliyor olabilir veya giriş verileri hazır olmadığı için giriş bekliyor olabilir.
      
      Fare imlecinizi tek bir duruma göre gezdirerek sahne için daha fazla ayrıntı bulabilirsiniz:
      
      ![Azure Veri Gölü Analizi iş grafiği aşama ayrıntıları](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage-details.png)
  * Vertices: Vertices ayrıntıları açıklar, örneğin, toplam kaç vertices, kaç vertices tamamlanmış, onlar başarısız veya hala çalışan / bekliyor, vb.
  * Veriler çapraz/iç bölmede okunur: Dosyalar ve veriler dağıtılmış dosya sisteminde birden çok bölmede depolanır. Buradaki değer, aynı bölmede veya çapraz bölmede ne kadar veri okunduğunu açıklar.
  * Toplam işlem süresi: Sahnedeki her tepe noktası yürütme süresinin toplamı, sahnedeki tüm çalışmaların tek bir tepe noktası içinde yürütülmesi durumunda zaman olarak düşünebilirsiniz.
  * Yazılan/okunan veriler ve satırlar: Ne kadar veri veya satırın okunduğunu/yazıldığını veya okunması gerektiğini gösterir.
  * Vertex okuma hataları: Verileri okurken kaç tepe noktasının başarısız olduğunu açıklar.
  * Vertex yinelenen atar: Bir tepe noktası çok yavaş çalışırsa, sistem aynı çalışma parçasını çalıştırmak için birden çok vertices zamanlayabilir. Redüktif vertices bir kez vertices başarıyla tamamlandığında atılır. Vertex yinelenen aşamalarında yineleme olarak atılır vertices sayısını kaydeder.
  * Vertex iptalleri: Tepe noktası başarılı oldu, ancak daha sonra bazı nedenlerden dolayı yeniden çalıştırıldı. Örneğin, aşağı vertex ara giriş verilerini kaybederse, yukarı akış vertex'inin yeniden çalışmasını ister.
  * Vertex zamanlama yürütmeleri: Vertices zamanlanmış toplam süre.
  * Min/Average/Max Vertex veri okuma: Her tepe noktası nın minimum/ortalama/maksimum okuma verisi.
  * Süre: Bir sahnenin aldığı duvar saati saati, bu değeri görmek için profil yüklemeniz gerekir.
  * İş Kayıttan Yürütme
    
      Data Lake Analytics işleri yürütür ve vertices başlatıldığında, durdurulduğu, başarısız olduğu ve nasıl yeniden denendiği gibi işlerin vertices çalışan bilgileri arşivler. Tüm bilgiler sorgu deposunda otomatik olarak günlüğe kaydedilir ve İş Profilinde depolanır. İş Görünümü'ndeki "Load Profile" aracılığıyla İş Profilini indirebilir ve İş Profilini indirdikten sonra İş Oynatma'yı görüntüleyebilirsiniz.
    
      İş Oynatma kümede ne olduğunu bir özet görselleştirme. Bu iş yürütme ilerleme izlemek ve görsel olarak çok kısa bir süre içinde performans anomalileri ve darboğazları (daha az 30s genellikle) tespit yardımcı olur.
  * İş Isı Haritası Ekranı 
    
      İş Isı Haritası, İş Grafiği'ndeki Ekran açılır geçitten seçilebilir. 
    
      ![Azure Veri Gölü Analizi iş grafiği yığın harita ekranı](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-display.png)
    
      Bir işin G/Ç, zaman ve iş artışı ısı haritasını gösterir, bu haritada işin çoğu zaman nerede zaman geçirdiğini veya işinizin G/Ç sınır işi olup olmadığını ve benzeri bir iş olduğunu görebilirsiniz.
    
      ![Azure Veri Gölü Analizi iş grafiği yığın harita örneği](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-example.png)
    
    * İlerleme: İş yürütme süreci, bkz.
    * Okunan/yazılan veriler: Her aşamada okunan/yazılan toplam verinin ısı haritası.
    * Hesaplama süresi: SUM'un ısı haritası (her vertex yürütme süresi), sahnedeki tüm çalışmaların yalnızca 1 tepe noktası ile yürütülmesi durumunda bunun ne kadar süreceğini düşünebilirsiniz.
    * Düğüm başına ortalama yürütme süresi: SUM'un ısı haritası (her tepe noktası yürütme süresi) / (Vertex Sayısı). Bu da demek oluyor ki, paralellik içinde yürütülen tüm tepe leri atayabilirseniz, tüm aşama bu zaman diliminde tamamlanacak.
    * Giriş/Çıkış verimi: Her aşamanın giriş/çıkış veriminin ısı haritası, işinizin bu yolla G/Ç'ye bağlı bir iş olup olmadığını doğrulayabilirsiniz.
* Meta veri işlemleri
  
    U-SQL komut dosyanızda veritabanı oluşturma, tablo bırakma vb. bazı meta veri işlemleri gerçekleştirebilirsiniz. Bu işlemler derlemeden sonra Meta data işleminde gösterilir. İddiaları bulabilir, varlıklar oluşturabilir, varlıkları burada bırakabilirsiniz.
  
    ![Azure Veri Gölü Analizi İş Görünümü meta veri işlemleri](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-metadata-operations.png)
* Devlet Tarihi
  
    Durum Geçmişi de İş Özeti görselleştirilmiş, ancak burada daha fazla bilgi alabilirsiniz. İşin ne zaman hazırlandığı, sıraya alındığı, çalışmaya başladığı, bittiği gibi ayrıntılı bilgileri bulabilirsiniz. Ayrıca, işin kaç kez derlendiğini (CcsGirişimleri: 1), gerçekte kümeye gönderilen iş ne zaman (Ayrıntı: kümeye iş gönderme) vb.
  
    ![Azure Veri Gölü Analizi İş Görünümü durum geçmişi](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-state-history.png)
* Tanılama
  
    Araç, iş yürütmeyi otomatik olarak tanılar. İşinizde bazı hatalar veya performans sorunları olduğunda uyarılar alırsınız. Burada tam bilgi almak için Profil indirmeniz gerektiğini lütfen unutmayın. 
  
    ![Azure Veri Gölü Analizi İş Görünümü tanılama](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-diagnostics.png)
  
  * Uyarılar: Derleyici uyarısı ile burada bir uyarı gösterir. Uyarı göründükten sonra daha fazla ayrıntı yada "x issue(s)" bağlantısını tıklayabilirsiniz.
  * Vertex çok uzun vadede: Herhangi bir tepe zaman (5 saat diyelim) biterse, sorunlar burada bulunacaktır.
  * Kaynak kullanımı: İhtiyacızdan daha fazla veya yeterli Parallelism tahsis varsa, sorunları burada bulabilirsiniz. Ayrıca daha fazla ayrıntı görmek ve daha iyi bir kaynak ayırma bulmak için "varsa" senaryoları gerçekleştirmek için Kaynak kullanımını tıklatabilirsiniz (daha fazla ayrıntı için bu kılavuza bakın).
  * Bellek denetimi: Herhangi bir tepe noktası 5 GB'tan fazla bellek kullanıyorsa, sorunlar burada bulunur. Sistem sınırlaması daha fazla bellek kullanırsa iş yürütme sistem tarafından öldürülebilir.

## <a name="job-detail"></a>İş Detayı
İş Ayrıntısı, Komut Dosyası, Kaynaklar ve Vertex Yürütme Görünümü de dahil olmak üzere işin ayrıntılı bilgilerini gösterir.

![Azure Veri Gölü Analizi iş detayı](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-details.png)

* Betik
  
    İşin U-SQL komut dosyası sorgu deposunda depolanır. Orijinal U-SQL komut dosyasını görüntüleyebilir ve gerekirse yeniden gönderebilirsiniz.
* Kaynaklar
  
    Sorgu deposunda depolanan iş derleme çıktılarını Kaynaklar aracılığıyla bulabilirsiniz. Örneğin, İş Grafiğini, kaydettiğiniz montajları vb. göstermek için kullanılan "cebir.xml"i burada bulabilirsiniz.
* Tepe noktası yürütme görünümü
  
    Bu vertices yürütme ayrıntıları gösterir. İş Profili, toplam veri okuma/yazma, çalışma zamanı, durum gibi her vertex yürütme günlüğünü arşivler. Bu görünüm sayesinde, bir işin nasıl çalıştırılabildiği hakkında daha fazla bilgi edinebilirsiniz. Daha fazla bilgi için [bkz.](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)

## <a name="next-steps"></a>Sonraki Adımlar
* Tanılama bilgilerini günlüğe kaydetmek için bkz. [Azure Data Lake Analytics için tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md)
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* Vertex yürütme görünümünü kullanmak [için](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md) bkz.

