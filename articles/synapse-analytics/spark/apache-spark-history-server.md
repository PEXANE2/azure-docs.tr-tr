---
title: Uygulamaları hata ayıklamak için genişletilmiş Spark geçmişi sunucusunu kullanın - Azure Synapse'deki Apache Spark
description: Azure Synapse Analytics'teki Spark uygulamalarını hata ayıklamak ve tanılamak için genişletilmiş Spark geçmiş sunucusunu kullanın.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: euang
ms.openlocfilehash: 4f03033942517f4778192e0b12f84610df8fd469
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429219"
---
# <a name="use-extended-apache-spark-history-server-to-debug-and-diagnose-apache-spark-applications"></a>Apache Spark uygulamalarını hata ayıklamak ve tanılamak için genişletilmiş Apache Spark geçmişi sunucusukullanın

Bu makalede, tamamlanmış ve çalıştırılan Spark uygulamalarını hata ayıklamak ve tanılamak için genişletilmiş Apache Spark geçmiş sunucusunun nasıl kullanılacağı hakkında kılavuz verilmektedir.

Uzantı, bir veri sekmesi, grafik sekmesi ve **Data** tanı sekmesi içerir. **Grafik** sekmesi, iş grafiğinin veri akışını ve yeniden oynatmasını gösterir. **Tanı** sekmesi **size Veri Eğriltme,** **Zaman Eğriltme**ve **Uygulayıcı Kullanım Analizini**gösterir.

## <a name="access-the-apache-spark-history-server"></a>Apache Spark geçmiş sunucusuna erişin

Apache Spark geçmiş sunucusu tamamlanan ve çalıştırılan Spark uygulamaları için web kullanıcı arabirimidir. Azure Synapse Analytics'ten Apache Spark geçmişi sunucusu web arabirimini açabilirsiniz.

### <a name="open-the-spark-history-server-web-ui-from-apache-spark-applications-node"></a>Apache kıvılcım uygulama düğümünden Spark History Server web UI'sini açın

1. [Azure Synapse Analytics'i](https://web.azuresynapse.net/)açın.

2. **Monitör'ü**tıklatın, ardından **Apache Spark Applications'ı**seçin.

    ![Monitör'ü tıklatın ve ardından kıvılcım uygulamasını seçin.](./media/apache-spark-history-server/click-monitor-spark-application.png)

3. Bir uygulama seçin ve tıklayarak **Günlük sorgusu'yu** açın.

    ![Günlük sorgu pencereni açın.](./media/apache-spark-history-server/open-application-window.png)

4. **Spark geçmişi sunucusunu**seçin, ardından Spark History Server web UI'si gösterilecek.

    ![Kıvılcım geçmişi sunucusuaçın.](./media/apache-spark-history-server/open-spark-history-server.png)

### <a name="open-the-spark-history-server-web-ui-from-data-node"></a>Veri düğümünden Spark History Server web UI'sini açın

1. Azure Synapse Studio not defterinizden, iş yürütme çıktısı hücresinden veya not defteri belgesinin altındaki durum panelinden **Spark geçmiş sunucusunu** seçin. **Oturum ayrıntıları**’nı seçin.

   ![Spark geçmişi sunucusubaşlatın](./media/apache-spark-history-server/launch-history-server2.png "Spark geçmişi sunucusubaşlatın")

2. Slayt panelinden **Geçmiş Sunucu'yu spark'ı** seçin.

   ![Spark geçmişi sunucusubaşlatın](./media/apache-spark-history-server/launch-history-server.png "Spark geçmişi sunucusubaşlatın")

## <a name="explore-the-data-tab-in-spark-history-server"></a>Spark geçmişi sunucusunda Veri sekmesini keşfedin

Görüntülemek istediğiniz iş için İş Kimliğini seçin. Ardından, veri görünümünü almak için araç menüsündeki **Veriler'i** seçin. Bu bölümde, Veri sekmesinde çeşitli görevlerin nasıl yapılacağını gösterir.

* Sekmeleri ayrı ayrı seçerek **Girişler,** **Çıktılar**ve **Tablo İşlemleri'ni** denetleyin.

    ![Spark uygulama sekmeleri için veriler](./media/apache-spark-history-server/apache-spark-data-tabs.png)

* Kopyala'yı seçerek **Copy**tüm satırları kopyala.

    ![Kıvılcım uygulama kopyası için veriler](./media/apache-spark-history-server/apache-spark-data-copy.png)

* **CSV**seçerek tüm verileri CSV dosyası olarak kaydedin.

    ![Spark uygulama kaydedin verileri](./media/apache-spark-history-server/apache-spark-data-save.png)

* **Alan Arama'ya**anahtar kelimeler girerek arama yapın. Arama sonuçları hemen görüntülenir.

    ![Kıvılcım uygulama araması için veriler](./media/apache-spark-history-server/apache-spark-data-search.png)

* Tabloyu sıralamak için sütun üstbilgisini seçin, daha fazla ayrıntı göstermek için satırı genişletmek için artı işaretini seçin veya bir satırı daraltmak için eksi işaretini seçin.

    ![Kıvılcım uygulama tablosu için veriler](./media/apache-spark-history-server/apache-spark-data-table.png)

* Kısmi İndir'i seçerek tek bir dosyayı **indirin.** Seçili dosya yerele indirilir. Dosya artık yoksa, hata iletisiyle birlikte yeni bir sekme görüntülenir.

    ![Spark uygulama indirme satırı için veriler](./media/apache-spark-history-server/sparkui-data-download-row.png)

* Tam bir yolu veya göreli yolu kopyalamak için, açılan menüden genişleyen **Tam Yolu Kopyala** veya **Göreli Yolu Kopyala** seçeneklerini seçin. Azure Veri Gölü Depolama dosyaları için **Azure Depolama Gezgini'nde Açık,** Azure Depolama Gezgini'ni başlatırken, oturum açıldığında klasörü bulur.

    ![Kıvılcım uygulaması kopyalama yolu için veriler](./media/apache-spark-history-server/sparkui-data-copy-path.png)

* Bir sayfada görüntülenecek çok fazla satır olduğunda sayfalarda gezinmek için tablonun altındaki sayfa numaralarını seçin.

    ![Kıvılcım uygulama sayfası için veriler](./media/apache-spark-history-server/apache-spark-data-page.png)

* Araç ipucunu göstermek için **Data'nın** yanındaki soru işaretinin üzerine geçin veya daha fazla bilgi almak için soru işaretini seçin.

    ![Spark uygulaması için veriler daha fazla bilgi](./media/apache-spark-history-server/sparkui-data-more-info.png)

* **Bize geri bildirim sağlayın'ı**seçerek sorunlarla ilgili geri bildirim gönderin.

    ![Kıvılcım grafiği bize tekrar geri bildirim sağlar](./media/apache-spark-history-server/sparkui-graph-feedback.png)

## <a name="graph-tab-in-apache-spark-history-server"></a>Apache Spark geçmiş sunucusunda grafik sekmesi

Görüntülemek istediğiniz iş için İş Kimliğini seçin. Ardından, iş grafiği görünümünü almak için araç menüsündeki **Grafik'i** seçin.

### <a name="overview"></a>Genel Bakış

Oluşturulan iş grafiğinde işinize genel bir bakış görebilirsiniz. Varsayılan olarak, grafik tüm işleri gösterir. Bu görünümü İş **Kimliği'ne**göre filtreleyebilirsiniz.

![Kıvılcım uygulaması ve iş grafiği iş kimliği](./media/apache-spark-history-server/apache-spark-graph-jobid.png)

### <a name="display"></a>Ekran

Varsayılan olarak, **İlerleme** ekranı seçilir. **Görüntü** açılır listesinde **Oku** veya **Yazılı'yı** seçerek veri akışını kontrol edebilirsiniz.

![Kıvılcım uygulaması ve iş grafiği ekranı](./media/apache-spark-history-server/sparkui-graph-display.png)

Grafik düğümü, ısı haritası göstergesinde gösterilen renkleri görüntüler.

![Kıvılcım uygulaması ve iş grafiği ısı haritası](./media/apache-spark-history-server/sparkui-graph-heatmap.png)

### <a name="playback"></a>Kayıttan yürütme

İşi oynatmak için **Oynatma'yı**seçin. Durdurmak için istediğiniz zaman **Durdur'u** seçebilirsiniz. Görev renkleri, oynatırken farklı durumları gösterir:

|Renk|Anlamı|
|-|-|
|Yeşil|Başarılı: İş başarıyla tamamlandı.|
|Orange|Yeniden denenmiş: Başarısız olan ancak işin nihai sonucunu etkilemeyen görev örnekleri. Bu görevler, daha sonra başarılı olabilecek yinelenen veya yeniden deneme örneklerine sahipti.|
|Mavi|Çalışan: Görev çalışıyor.|
|Beyaz|Bekleme veya atlama: Görev çalıştırmak için bekliyor veya sahne atladı.|
|Red|Başarısız: Görev başarısız oldu.|

Aşağıdaki resimde Yeşil, Turuncu ve Mavi durum renkleri gösterilmektedir.

![Kıvılcım uygulaması ve iş grafiği renk örneği, çalışan](./media/apache-spark-history-server/sparkui-graph-color-running.png)

Aşağıdaki resimde Yeşil ve Beyaz durum renkleri gösterilmektedir.

![Kıvılcım uygulaması ve iş grafiği renk örneği, atlama](./media/apache-spark-history-server/sparkui-graph-color-skip.png)

Aşağıdaki resimde Kırmızı ve Yeşil durum renkleri gösterilmektedir.

![Kıvılcım uygulaması ve iş grafiği renk örneği, başarısız](./media/apache-spark-history-server/sparkui-graph-color-failed.png)

> [!NOTE]  
> Her iş için oynatma yada izin verilir. Tamamlanmamış işleriçin oynatma desteklenmez.

### <a name="zoom"></a>Zoom

İş grafiğini yakınlaştırmak ve uzaklaştırmak için fare kaydırmanızı kullanın veya ekrana sığdırmak **için sığdırmak için Yakınlaştır'ı** seçin.

![Sığacak şekilde uygulama ve iş grafiği yakınlaştırma kıvılcımı](./media/apache-spark-history-server/sparkui-graph-zoom2fit.png)

### <a name="tooltips"></a>Araç İpuçları

Başarısız görevler olduğunda araç ucunu görmek için grafik düğümüne geçin ve sahne sayfasını açmak için bir sahne seçin.

![Kıvılcım uygulaması ve iş grafiği araç ipucu](./media/apache-spark-history-server/sparkui-graph-tooltip.png)

İş grafiği sekmesinde, aşamaların bir araç ipucu ve aşağıdaki koşulları karşılayan görevleri varsa görüntülenen küçük bir simge vardır:

|Koşul|Açıklama|
|-|-|
|Veri eğriltme|veri okuma boyutu > ortalama veri okuma boyutu bu aşamadaki tüm görevlerin okuma boyutu * 2 ve veri okuma boyutu > 10 MB|
|Zaman çarpık|yürütme süresi > bu aşamadaki tüm görevlerin ortalama yürütme süresi * 2 ve yürütme süresi > 2 dakika|
   
![Kıvılcım uygulaması ve iş grafiği eğrilme simgesi](./media/apache-spark-history-server/sparkui-graph-skew-icon.png)

### <a name="graph-node-description"></a>Grafik düğümü açıklaması

İş grafiği düğümü her aşamanın aşağıdaki bilgilerini görüntüler:

  * Kimliği.
  * Ad veya açıklama.
  * Toplam görev numarası.
  * Veri okuma: giriş boyutu ve shuffle okuma boyutu toplamı.
  * Veri yazma: çıktı boyutu ve shuffle toplamı boyutu yazar.
  * Yürütme süresi: İlk denemenin başlangıç saati ile son denemenin tamamlanma süresi arasındaki süre.
  * Satır sayısı: giriş kayıtlarının, çıktı kayıtlarının, shuffle okuma kayıtlarının ve karışık yazma kayıtlarının toplamı.
  * Ilerleme.

    > [!NOTE]  
    > Varsayılan olarak, iş grafiği düğümü her aşamanın son denemesinden (aşama yürütme süresi hariç) bilgileri görüntüler. Ancak, oynatma sırasında grafik düğümü her girişimin bilgilerini gösterir.
    >  
    > Okuma yazma nın veri boyutu 1MB = 1000 KB = 1000 * 1000 Bayt'tır.

### <a name="provide-feedback"></a>Geri bildirimde bulunma

**Bize geri bildirim sağlayın'ı**seçerek sorunlarla ilgili geri bildirim gönderin.

![Kıvılcım uygulaması ve iş grafiği geri bildirimi](./media/apache-spark-history-server/sparkui-graph-feedback.png)

## <a name="explore-the-diagnosis-tab-in-apache-spark-history-server"></a>Apache Spark geçmişi sunucusunda Tanı sekmesini keşfedin

Tanı sekmesine erişmek için bir iş kimliği seçin. Ardından iş Tanı görünümü almak için araç menüsünde **Tanı'yı** seçin. Tanı sekmesi **Veri Eğriltme,** **Zaman Çarpık**ve **Uygulayıcı Kullanım Analizi**içerir.

Sekmeleri sırasıyla seçerek **Veri Eğriltme,** **Zaman Eğriltme**ve **Uygulayıcı Kullanım Çözümlemesi'ni** denetleyin.

![SparkUI tanı verileri yine sekme çarpık](./media/apache-spark-history-server/sparkui-diagnosis-tabs.png)

### <a name="data-skew"></a>Veri Eğriltme

**Data Skew** sekmesini seçtiğinizde, ilgili çarpık görevler belirtilen parametrelere göre görüntülenir.

* **Parametreleri Belirt** - İlk bölümde Veri Eğriliğini algılamak için kullanılan parametreler görüntülenir. Varsayılan kural şudur: Görev Verileri Okuma, okunan ortalama görev verilerinin üç katından büyüktür ve okunan görev verileri 10 MB'dan fazladır. Çarpık görevler için kendi kuralınızı tanımlamak istiyorsanız, parametrelerinizi seçebilirsiniz, **Çarpık Sahne ve** **Eğriltme Char** bölümleri buna göre yenilenir.

* **Çarpık Sahne** - İkinci bölümde, yukarıda belirtilen ölçütleri karşılayan çarpık görevler olan aşamalar görüntülenir. Bir aşamada birden fazla çarpık görev varsa, çarpık sahne tablosu yalnızca en çarpık görevi görüntüler (örneğin, veri eğriliği için en büyük veri).

    ![sparkui tanı verileri çarpık sekmesi](./media/apache-spark-history-server/sparkui-diagnosis-dataskew-section2.png)

* **Çarpık Grafik** – Çarpık sahne tablosundaki bir satır seçildiğinde, eğriltme grafiği veri okuma ve yürütme süresine bağlı olarak daha fazla görev dağıtım ayrıntıları görüntüler. Eğriltilmiş görevler kırmızı ve normal görevler mavi ile işaretlenir. Grafik en fazla 100 örnek görevi görüntüler ve görev ayrıntıları sağ alt panelde görüntülenir.

    ![evre 10 için sparkui çarpık grafik](./media/apache-spark-history-server/sparkui-diagnosis-dataskew-section3.png)

### <a name="time-skew"></a>Zaman Çarpık

**Zaman Eğriltme** sekmesi, görev yürütme süresine bağlı olarak çarpık görevleri görüntüler.

* **Parametreleri Belirt** - İlk bölümde Zaman Eğriliğini algılamak için kullanılan parametreler görüntülenir. Zamanı algılamak için varsayılan ölçütşudur: görev yürütme süresi ortalama yürütme süresinin üç katından büyüktür ve görev yürütme süresi 30 saniyeden büyüktür. İhtiyaçlarınıza göre parametreleri değiştirebilirsiniz. **Çarpık Sahne ve** **Eğriltme Grafiği,** yukarıdaki **Veri Eğriliği** sekmesi gibi ilgili aşamaları ve görevleri görüntüler.

* **Zaman Eğriltme'yi**seçin, ardından filtreuygulanan sonuç, bölüm deki parametrelere göre **Eğri Aşama** bölümünde görüntülenir **Parametreleri Belirtin.** **Çarpık Sahne** bölümünde bir öğe seçin, ardından ilgili grafik bölüm3'te hazırlanır ve görev ayrıntıları sağ alt panelde görüntülenir.

    ![sparkui tanı zaman çarpık bölümü](./media/apache-spark-history-server/sparkui-diagnosis-timeskew-section2.png)

### <a name="executor-usage-analysis"></a>Executor Kullanım Analizi

Executor Kullanım Grafiği, Kıvılcım iş yürütücüsün tahsisini ve çalışma durumunu görselleştirir.  

1. **Executor Kullanım Analizi'ni**seçin, daha sonra **Ayrılan Uygulayıcılar,** **Çalıştıran Uygulayıcılar,** Boşta Kalan **Uygulayıcılar**ve **Max Executor Örnekleri**dahil olmak üzere, uygulayıcı kullanımıyla ilgili dört tür eğri hazırlanır. Ayrılan uygulayıcılarla ilgili olarak, her "Uygulayıcı eklendi" veya "Executor kaldırıldı" olayı, ayrılan uygulayıcıları artırır veya azaltır. Daha fazla karşılaştırma için "İşler" sekmesinde "Olay Zaman Çizelgesi"ni işaretleyebilirsiniz.

   ![sparkui tanı uygulayıcıları sekmesi](./media/apache-spark-history-server/sparkui-diagnosis-executors.png)

2. Tüm taslaklarda karşılık gelen içeriği seçmek veya seçmek için renk simgesini seçin.

    ![sparkui select chart'ı tanılar](./media/apache-spark-history-server/sparkui-diagnosis-select-chart.png)

## <a name="known-issues"></a>Bilinen sorunlar

Esnek Dağıtılmış Veri Kümeleri (RDD' ler) kullanılarak giriş/çıktı verileri veri sekmesinde gösterilmez.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Synapse Analytics](../overview-what-is.md)
- [.NET Apache Spark belgeleri için](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

