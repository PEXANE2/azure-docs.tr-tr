---
title: İş & Iş görünümünü kullanın-Azure Data Lake Analytics
description: Bu makalede Azure Data Lake Analytics işleri için Iş tarayıcısının ve Iş görünümünün nasıl kullanılacağı açıklanır.
ms.service: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
ms.assetid: bdf27b4d-6f58-4093-ab83-4fa3a99b5650
ms.topic: conceptual
ms.date: 08/02/2017
ms.openlocfilehash: 2d33a6ec5ff6b687913914e9433b85765aaa7aec
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "71309945"
---
# <a name="use-job-browser-and-job-view-for-azure-data-lake-analytics"></a>Azure Data Lake Analytics için İş Tarayıcısı’nı ve İş Görünümü’nü kullanma
Azure Data Lake Analytics hizmeti bir sorgu deposunda gönderilen işleri arşivler. Bu makalede, geçmiş iş bilgilerini bulmak için Visual Studio için Azure Data Lake Araçları ' de Iş tarayıcısını ve Iş görünümünü nasıl kullanacağınızı öğreneceksiniz. 

Varsayılan olarak, Data Lake Analytics hizmeti işleri 30 gün boyunca arşivler. Süre sonu dönemi, özelleştirilmiş süre sonu ilkesi yapılandırılarak Azure portal yapılandırılabilir. Süre dolduktan sonra iş bilgilerine erişemeyeceksiniz. 

## <a name="prerequisites"></a>Ön koşullar
Bkz. [Visual Studio için Data Lake araçları önkoşulları](data-lake-analytics-data-lake-tools-get-started.md#prerequisites).

## <a name="open-the-job-browser"></a>Iş tarayıcısını açın
Visual Studio 'da>iş **Data Lake Analytics>Azure>Sunucu Gezgini** aracılığıyla iş tarayıcısına erişin.  Iş tarayıcısını kullanarak bir Data Lake Analytics hesabının sorgu deposuna erişebilirsiniz. İş tarayıcısı, temel iş bilgilerini göstererek ve ayrıntılı iş bilgilerini gösteren doğru iş görünümünü gösteren sorgu deposunu sol tarafta görüntüler.

## <a name="job-view"></a>İş görünümü
İş görünümü bir işin ayrıntılı bilgilerini gösterir. Bir işi açmak için iş tarayıcısında bir işe çift tıklayabilir veya Iş görünümü ' ne tıklayarak Data Lake menüsünden açabilirsiniz. İş URL 'SI ile doldurulmuş bir iletişim kutusu görmeniz gerekir.

![Data Lake araçları Visual Studio Iş tarayıcısı](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view.png)

İş görünümü şunları içerir:

* İş Özeti
  
    Çalışan işlerin daha güncel bilgilerini görmek için Iş görünümünü yenileyin.
  
  * İş durumu (grafik):
    
      İş durumu iş aşamalarını özetler:
    
      ![Azure Data Lake Analytics iş aşamaları durumu](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-phases.png)
    
    * Hazırlama: komut dosyanızı buluta yükleyin, derleme hizmetini kullanarak betiği derleyip en iyi duruma getirme.
    * Kuyruğa Alınan: Işler, yeterli kaynak beklerken sıraya alınır veya işler hesap başına maksimum eşzamanlı iş sayısını aşırlar. Öncelik ayarı sıraya alınan işlerin sırasını belirler. sayı ne kadar düşükse öncelik o kadar yüksektir.
    * Çalışıyor: iş aslında Data Lake Analytics hesabınızda çalışır.
    * Sonlandırılıyor: iş Tamamlanıyor (örneğin, dosya sonlandırılıyor).
      
      İş her aşamada başarısız olabilir. Örneğin, hazırlama aşamasında derleme hataları, sıraya alınan aşamadaki zaman aşımı hataları ve çalıştırma aşamasında yürütme hataları vb.
  * Temel bilgiler
    
      Temel iş bilgileri, Iş Özeti panelinin alt bölümünde gösterilir.
    
      ![Azure Data Lake Analytics iş aşamaları durumu](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-info.png)
    
    * İş sonucu: başarılı veya başarısız oldu. İş her aşamada başarısız olabilir.
    * Toplam süre: gönderme saati ile bitiş saati arasındaki duvar saati zamanı (süre).
    * Toplam Işlem süresi: her köşe yürütme zamanının toplamı, bunu işin yalnızca bir köşede yürütüldüğü zaman olarak kabul edebilirsiniz. Köşe hakkında daha fazla bilgi edinmek için toplam köşelere bakın.
    * Gönderme/başlatma/bitiş saati: Data Lake Analytics hizmetinin iş gönderimini aldığı/işi çalıştırmaya başladığı saat veya işi başarıyla sonlandırıyorsa zaman.
    * Derleme/sıraya alınmış/çalışıyor: hazırlama/sıraya alma/çalıştırma aşamasında harcanan duvar saati zamanı.
    * Hesap: işi çalıştırmak için kullanılan Data Lake Analytics hesabı.
    * Yazar: işi gönderen Kullanıcı, gerçek bir kişinin hesabı veya sistem hesabı olabilir.
    * Öncelik: işin önceliği. Sayı ne kadar düşükse öncelik o kadar yüksektir. Yalnızca kuyruktaki işlerin sırasını etkiler. Daha yüksek bir öncelik ayarlamak işleri çalıştırmayı engellemez.
    * Paralellik: istenen en fazla eşzamanlı Azure Data Lake Analytics birimi sayısı (adlaus), diğer adıyla köşeleri. Şu anda, bir köşe iki sanal çekirdekli ve altı GB RAM 'e sahip bir VM 'ye eşittir, ancak gelecekte Data Lake Analytics güncelleştirmelerde yükseltilecektir.
    * Kalan bayt: iş tamamlanana kadar işlenmesi gereken baytlar.
    * Okunan/yazılan bayt sayısı: iş çalışmaya başladıktan sonra okunan/yazılan baytlar.
    * Toplam köşeler: iş çok sayıda iş parçasına bölündüğü her iş parçasına köşe denir. Bu değer, işin kaç iş parçasının oluştuğunu açıklar. Bir köşeyi temel işlem birimi, diğer adıyla Azure Data Lake Analytics birimi (adlau) olarak düşünebilirsiniz ve köşeler paralellik halinde çalıştırılabilir. 
    * Tamamlandı/çalışıyor/başarısız: tamamlanan/çalışan/başarısız köşelerin sayısı. Köşeler her iki Kullanıcı kodu ve sistem hatası nedeniyle başarısız olabilir, ancak sistem yeniden denemeler birkaç kez otomatik olarak başarısız olur. Yeniden denedikten sonra köşe hala başarısız olursa, tüm iş başarısız olur.
* İş grafiği
  
    U-SQL betiği, giriş verilerini çıkış verilerine dönüştürme mantığını temsil eder. Betik derlenir ve hazırlama aşamasında fiziksel bir yürütme planına iyileştirilir. İş grafiği, fiziksel yürütme planını gösteriyoruz.  Aşağıdaki diyagramda işlem gösterilmektedir:
  
    ![Azure Data Lake Analytics iş aşamaları durumu](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-logical-to-physical-plan.png)
  
    Bir iş, çok sayıda iş parçasına bölünmüştür. Her iş parçasına köşe denir. Köşeler süper köşe (aka aşaması) olarak gruplandırılır ve Iş grafiği olarak görselleştirilebilir. İş grafiğindeki yeşil aşama plagrafları, aşamaları gösterir.
  
    Bir aşamadaki her köşe, aynı verilerin farklı parçaları ile aynı türde çalışmalardır. Örneğin, bir TB veri içeren bir dosyanız varsa ve bunlardan oluşan yüzlerce köşe varsa, bunların her biri bir öbek okur. Bu köşeler aynı aşamada gruplandırılır ve aynı giriş dosyasının farklı parçaları üzerinde aynı çalışma yapılır.
  
  * <a name="state-information"></a>Aşama bilgileri
    
      Belirli bir aşamada, plat kartında bazı sayılar gösterilir.
    
      ![Azure Data Lake Analytics iş grafiği aşaması](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage.png)
    
    * SV1 Ayıkla: bir sayı ve işlem yöntemi tarafından adlandırılan bir aşamanın adı.
    * 84 köşeler: Bu aşamadaki köşelerin toplam sayısı. Şekil, bu aşamada kaç iş parçasının bölüneceğini gösterir.
    * 12,90 s/köşe: Bu aşamanın ortalama köşe yürütme süresi. Bu şekil SUM (her köşe yürütme süresi)/(Toplam köşe sayısı) tarafından hesaplanır. Paralellik olarak yürütülen tüm köşeleri atayabiliyorsanız, tüm aşamanın 12,90 s içinde tamamlandığı anlamına gelir. Ayrıca, bu aşamadaki tüm işler hizmet dışı olarak tamamlandığında, maliyetin #vertices * ort saat olacağını da gösterir.
    * 850.895 satır yazıldı: Bu aşamada yazılan toplam satır sayısı.
    * R/W: Bu aşamada bayt cinsinden okunan/yazılan veri miktarı.
    * Renkler: renkler farklı köşe durumunu göstermek için aşamada kullanılır.
      
      * Yeşil, köşe 'nin başarılı olduğunu gösterir.
      * Turuncu, köşeyi yeniden denendiğini gösterir. Yeniden denenen köşe başarısız oldu, ancak sistem tarafından otomatik olarak ve başarıyla yeniden denendi ve genel aşama başarıyla tamamlandı. Köşe yeniden denenip hala başarısız olduysa, renk kırmızıya dönüşür ve tüm iş başarısız oldu.
      * Red başarısız oldu, bu, belirli bir köşenin sistem tarafından birkaç kez yeniden denendiğini ancak hala başarısız olduğunu gösterir. Bu senaryo, tüm işin başarısız olmasına neden olur.
      * Mavi, belirli bir köşe 'nin çalıştığı anlamına gelir.
      * Beyaz, köşeyi beklediğini gösterir. Bir ADLAU kullanılabilir hale geldiğinde köşe zamanlanmayı bekliyor olabilir veya giriş verileri hazır olmadığından giriş bekleniyor olabilir.
      
      Fare imlecinizi tek bir duruma getirerek, aşama hakkında daha fazla ayrıntı bulabilirsiniz:
      
      ![Azure Data Lake Analytics iş grafiği aşama ayrıntıları](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage-details.png)
  * Köşeler: köşelerin ayrıntılarını açıklar; Örneğin, toplam cinsinden kaç köşe, tamamlanan köşe sayısı, ne başarısız veya hala çalışıyor/bekliyor, vb.
  * Veri okuma/şirket içi Pod: dosyalar ve veriler, dağıtılmış dosya sisteminde birden çok sayıda Pod 'de depolanır. Buradaki değer, aynı Pod veya Cross Pod 'da ne kadar veri okunduğunu açıklar.
  * Toplam işlem süresi: aşamadaki her köşe yürütme zamanının toplamı, bu süreyi, aşamadaki tüm çalışmalar yalnızca bir köşede yürütülürse yapılacak süre olarak kabul edebilirsiniz.
  * Yazılan/okunan veriler ve satırlar: ne kadar veri ya da satır okunup yazıldığını veya okunmayı belirtir.
  * Köşe okuma sorunları: verileri okurken kaç köşe başarısız olduğunu açıklar.
  * Köşe yinelemesi atımı: bir köşe çok yavaş çalışırsa, sistem aynı iş parçasını çalıştırmak için birden çok köşe zamanlayabilir. Köşelerden biri başarıyla tamamlandıktan sonra, Reductant köşeleri atılır. Köşe yinelemesi atma, aşamada çoğaltılan olarak atılan köşelerin sayısını kaydeder.
  * Köşe iptal edilecek: köşe başarılı oldu, ancak bazı nedenlerden dolayı daha sonra yeniden çalıştırın. Örneğin, aşağı akış köşesi ara giriş verilerini kaybederse, yukarı akış köşesinin yeniden çalıştırılması istenir.
  * Köşe zamanlaması yürütmeleri: köşelerin zamanlandığı toplam süre.
  * En az/ortalama/en fazla köşe verisi okuma: her köşenin veri okuma en düşük/ortalama/en yüksek değeri.
  * Süre: bir aşamanın zaman içindeki duvar saati zamanı, bu değeri görmek için profil yüklemeniz gerekir.
  * İş Kayıttan Yürütme
    
      Data Lake Analytics işleri çalıştırır ve köşelerin başlatıldığı, durdurulduğu, başarısız olduğu ve nasıl yeniden denendikleri gibi, vb. gibi işlerin çalışma bilgilerini arşivler. Tüm bilgiler otomatik olarak sorgu deposunda günlüğe kaydedilir ve Iş profilinde depolanır. İş profilini, iş görünümündeki "profil yükle" aracılığıyla indirebilir ve iş profilini indirdikten sonra Işi kayıttan yürütmeyi görüntüleyebilirsiniz.
    
      İş kayıttan yürütme, kümede ne olduğuna ilişkin bir Epitome görselleştirmedir. Bu, iş yürütme ilerlemesini izlemenize ve çok kısa bir süre içinde (genellikle 30 ' dan az) performans sorunlarını ve darboğazları tespit etmenize yardımcı olur.
  * İş ısı haritası ekranı 
    
      İş ısı haritası, Iş grafiğinde görüntü açılan menüsünden seçilebilir. 
    
      ![Azure Data Lake Analytics iş grafiği yığın eşlemesi görüntüsü](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-display.png)
    
      İş için g/ç, zaman ve üretilen iş ısı haritasını gösterir. Bu, işin en fazla ne kadar zaman harcadığını veya işinizin bir g/ç sınırı işi olup olmadığını bulabilir.
    
      ![Azure Data Lake Analytics iş grafiği yığın eşleme örneği](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-example.png)
    
    * İlerleme: iş yürütme ilerleme durumu, bkz. aşama bilgileri içindeki bilgiler.
    * Okunan/yazılan veriler: her aşamada okunan/yazılan toplam veri ısı haritası.
    * İşlem süresi: SUM (her köşe yürütme süresi) Için ısı haritası, aşamadaki tüm çalışmalar yalnızca 1 köşe ile yürütülürse ne kadar süreceğine göz önüne alabilirsiniz.
    * Düğüm başına ortalama yürütme süresi: TOPLAMıN (her köşe yürütme süresi)/(köşe numarası) ısı haritası. Bu, paralellik olarak yürütülen tüm köşeleri atayabilmeniz durumunda tüm aşamanın bu zaman çerçevesinde yapılacağını gösterir.
    * Giriş/çıkış işleme: her bir aşamanın giriş/çıkış işleme ısı haritası, işinizin bu aracılığıyla g/ç bağlantılı bir iş olup olmadığını doğrulayabilirsiniz.
* Meta veri Işlemleri
  
    U-SQL betiğinizdeki veritabanı oluşturma, tablo bırakma vb. gibi bazı meta veri işlemlerini gerçekleştirebilirsiniz. Bu işlemler, derlemeden sonra meta veri Işleminde gösterilir. Onaylamalar bulabilir, varlık oluşturabilir, varlıkları buradan bırakabilirsiniz.
  
    ![Azure Data Lake Analytics Iş görünümü meta verileri işlemleri](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-metadata-operations.png)
* Durum geçmişi
  
    Durum geçmişi Ayrıca Iş özetinde görselleştirilebilir, ancak burada daha fazla ayrıntı edinebilirsiniz. İşin hazırlandığı, kuyruğa alındığı, çalışmaya başladığı, ne zaman sona erdiği gibi ayrıntılı bilgileri bulabilirsiniz. Ayrıca, işin ne kadar zaman derlendiğini (CcsAttempts: 1), ne zaman kümeye (ayrıntı: bir işi kümeye dağıtma), vb.
  
    ![Azure Data Lake Analytics Iş görünümü durumu geçmişi](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-state-history.png)
* Tanılama
  
    Araç, iş yürütmeyi otomatik olarak tanılar. İşlerinizde bazı hatalar veya performans sorunları olduğunda uyarılar alacaksınız. Burada tam bilgi edinmek için profili indirmeniz gerektiğini lütfen unutmayın. 
  
    ![Azure Data Lake Analytics Iş görünümü tanılaması](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-diagnostics.png)
  
  * Uyarılar: bir uyarı, derleyici uyarısıyla burada görünür. Uyarı göründükten sonra daha fazla ayrıntı edinmek için "x sorun (ler)" bağlantısına tıklayabilirsiniz.
  * Köşe çok uzun çalışıyor: herhangi bir köşe zaman aşımına uğrar (5 saat), bu sorunlar burada bulunur.
  * Kaynak kullanımı: daha fazla veya ihtiyaktan yeterince paralellik ayırdıysanız, sorunlar burada bulunur. Ayrıca, daha fazla ayrıntı görmek için kaynak kullanımı ' na tıklayabilir ve daha iyi bir kaynak ayırmayı bulmak için durum senaryoları gerçekleştirebilirsiniz (daha fazla ayrıntı için bu kılavuza bakın).
  * Bellek denetimi: herhangi bir köşe 5 GB 'den fazla bellek kullanıyorsa, bu sorunlar burada bulunur. Sistem sınırlamasından daha fazla bellek kullanıyorsa, iş yürütmesi sistem tarafından sonlandırılabilir.

## <a name="job-detail"></a>İş ayrıntısı
İş ayrıntısı, iş betiği, kaynaklar ve köşe yürütme görünümü dahil olmak üzere işin ayrıntılı bilgilerini gösterir.

![Azure Data Lake Analytics iş ayrıntısı](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-details.png)

* Komut Dosyası
  
    İşin U-SQL betiği sorgu deposunda depolanır. Özgün U-SQL betiğini görüntüleyebilir ve gerekirse yeniden gönderebilirsiniz.
* Kaynaklar
  
    Sorgu deposunda depolanan iş derleme çıktılarını kaynaklar aracılığıyla bulabilirsiniz. Örneğin, Iş grafiğini, kaydettiğiniz derlemeleri vb. göstermek için kullanılan "algeköşeli. xml" öğesini bulabilirsiniz.
* Köşe yürütme görünümü
  
    Köşe yürütme ayrıntılarını gösterir. Iş profili, toplam veri okuma/yazma, çalışma zamanı, durum vb. gibi her köşe yürütme günlüğünü arşivler. Bu görünüm aracılığıyla, bir işin nasıl çalıştığı hakkında daha fazla bilgi edinebilirsiniz. Daha fazla bilgi için bkz. [Visual Studio için Data Lake araçları 'Nda köşe yürütme görünümünü kullanma](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).

## <a name="next-steps"></a>Sonraki Adımlar
* Tanılama bilgilerini günlüğe kaydetmek için bkz. [Azure Data Lake Analytics için tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md)
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* Köşe yürütme görünümünü kullanmak için bkz [. Visual Studio için Data Lake araçları 'Nda köşe yürütme görünümünü kullanma](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)

