---
title: Uygulamaları hata ayıklamak için Apache Spark History Server'daki genişletilmiş özellikleri kullanın - Azure HDInsight
description: Spark uygulamalarını - Azure HDInsight - hata ayıklamak ve tanılamak için Apache Spark History Server'daki genişletilmiş özellikleri kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/25/2019
ms.openlocfilehash: 5cf1986711479f7330b0cd477744d9f4e2ac6459
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76548943"
---
# <a name="use-the-extended-features-of-the-apache-spark-history-server-to-debug-and-diagnose-spark-applications"></a>Spark uygulamalarını hata ayıklamak ve tanılamak için Apache Spark History Server'ın genişletilmiş özelliklerini kullanın

Bu makalede, tamamlanan veya çalıştırılan Spark uygulamalarını ayıklamak ve tanılamak için Apache Spark History Server'ın genişletilmiş özelliklerini nasıl kullanacağınızı gösterilmektedir. Uzantı, **Veri** sekmesi, **Grafik** sekmesi ve **Tanı** sekmesini içerir. **Veriler** sekmesinde, Spark işinin giriş ve çıktı verilerini denetleyebilirsiniz. **Grafik** sekmesinde, veri akışını denetleyebilir ve iş grafiğini yeniden oynatabilirsiniz. **Tanı** sekmesinde, **Veri Eğriliği,** **Zaman Eğriliği**ve Uygulayıcı **Kullanım Analizi** özelliklerine başvurabilirsiniz.

## <a name="get-access-to-the-spark-history-server"></a>Spark Geçmişi Sunucusu'na erişin

Spark History Server, Tamamlanan ve çalıştırılan Spark uygulamaları için web Kullanıcı Arabirimi'dir. Azure portalından veya URL'den açabilirsiniz.

### <a name="open-the-spark-history-server-web-ui-from-the-azure-portal"></a>Azure portalından Spark History Server web UI'sini açın

1. Azure [portalından](https://portal.azure.com/)Spark kümesini açın. Daha fazla bilgi için liste [ve kümeleri göster'e](../hdinsight-administer-use-portal-linux.md#showClusters)bakın.
2. **Küme panolarından,** **Spark geçmişi sunucusu'nü**seçin. İstendiğinde, Spark kümesi için yönetici kimlik bilgilerini girin.

    ![Azure portalından Spark History Server'ı başlatın.](./media/apache-azure-spark-history-server/azure-portal-dashboard-spark-history.png "Spark Tarih Sunucusu")

### <a name="open-the-spark-history-server-web-ui-by-url"></a>Spark History Server web Kullanıcı ını URL'ye göre açın

`https://CLUSTERNAME.azurehdinsight.net/sparkhistory` **CLUSTERNAME'nin** Spark kümenizin adı olduğu yere göz atarak Spark History Server'ı açın.

Spark History Server web UI bu görüntüye benzer görünebilir:

![Spark History Server sayfası.](./media/apache-azure-spark-history-server/hdinsight-spark-history-server.png)

## <a name="use-the-data-tab-in-the-spark-history-server"></a>Spark Geçmişi Sunucusu'ndaki Veri sekmesini kullanma

İş kimliğini seçin ve ardından veri görünümünü görmek için araç menüsündeki **Veriler'i** seçin.

+ Tek tek sekmeleri seçerek **Girişleri,** **Çıktıları**ve **Tablo İşlemlerini** gözden geçirin.

    ![Kıvılcım Uygulaması için Veri sekmeleri.](./media/apache-azure-spark-history-server/apache-spark-data-tabs.png)

+ **Kopyala** düğmesini seçerek tüm satırları kopyalayın.

    ![Verileri Kıvılcım uygulama sayfasında kopyalayın.](./media/apache-azure-spark-history-server/apache-spark-data-copy.png)

+ Tüm verileri bir . CSV dosyasını seçerek **csv** butonu.

    ![Verileri bir . Kıvılcım Uygulaması için Veriler sayfasından CSV dosyası.](./media/apache-azure-spark-history-server/apache-spark-data-save.png)

+ **Arama** alanına anahtar kelimeler girerek verileri arayın. Arama sonuçları hemen görüntülenir.

    ![Kıvılcım Uygulaması için Veriler sayfasındaki verileri arayın.](./media/apache-azure-spark-history-server/apache-spark-data-search.png)

+ Tabloyu sıralamak için sütun üstbilgisini seçin. Daha fazla ayrıntı göstermek için satırı genişletmek için artı işaretini seçin. Bir satırı daraltmak için eksi işaretini seçin.

    ![Kıvılcım Uygulaması için Veri tablosu.](./media/apache-azure-spark-history-server/apache-spark-data-table.png)

+ Sağdaki **Kısmi İndir** düğmesini seçerek tek bir dosya indirin. Seçili dosya yerel olarak karşıdan yüklenecek. Dosya artık yoksa, bu hata iletilerini göstermek için yeni bir sekme açılır.

    ![Kıvılcım Uygulaması için Veri yükleme satırı.](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ İndirme menüsünden genişleyen Tam Yolu **Kopyala** veya **Bağıl Yolu Kopyala** seçeneğini seçerek tam bir yolu veya göreli yolu kopyalayın. Azure Veri Gölü Depolama dosyaları için Azure Depolama Gezgini'ni başlatmak ve oturum açtıktan sonra klasörü bulmak için **Azure Depolama Gezgini'nde Aç'ı** seçin.

    ![Tam Yol'u kopyalayın ve Kıvılcım Uygulaması için Veri sayfasında Göreli Yol seçeneklerini kopyalayın.](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Tek bir sayfada görüntülenecek çok fazla satır varsa, gezinmek için tablonun altındaki sayfa numaralarını seçin.

    ![Kıvılcım Uygulaması için Veriler sayfasındaki sayfa numaraları.](./media/apache-azure-spark-history-server/apache-spark-data-page.png)

+ Daha fazla bilgi için, araç ipucunu göstermek **için Spark Uygulaması için Veri'nin** yanındaki soru işaretinin üzerine doğru gezin veya seçin.

    ![Spark Uygulaması için Veriler sayfasından daha fazla bilgi alın.](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+  Sorunlar hakkında geri bildirim göndermek için **bize geri bildirim sağlayın'ı**seçin.

    ![Kıvılcım Uygulaması için Veriler sayfasından geri bildirim sağlayın.](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="use-the-graph-tab-in-the-spark-history-server"></a>Spark Geçmişi Sunucusu'ndaki Grafik sekmesini kullanma

+ İş kimliğini seçin ve ardından iş grafiğini görmek için araç menüsünde **Grafik'i** seçin. Varsayılan olarak, grafik tüm işleri gösterir. **İş Kimliği** açılır menüsünü kullanarak sonuçları filtreleyin.

    ![Kıvılcım Uygulaması & İş Grafiği sayfasındaki İş Kimliği açılır menüsü.](./media/apache-azure-spark-history-server/apache-spark-graph-jobid.png)

+ **İlerleme** varsayılan olarak seçilir. **Ekran** açılır menüsünde **Oku** veya **Yazılı'yı** seçerek veri akışını kontrol edin.

    ![Spark Application & İş Grafiği sayfasındaki veri akışını kontrol edin.](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

+ Her görevin arka plan rengi bir ısı haritasına karşılık gelir.

   ![Kıvılcım Uygulaması & İş Grafiği sayfasındaki ısı haritası.](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)


    |Renk |Açıklama |
    |---|---|
    |Yeşil|İş başarıyla tamamlandı.|
    |Orange|Görev başarısız oldu, ancak bu işin nihai sonucunu etkilemez. Bu görevler, daha sonra başarılı olabilecek yinelenen veya yeniden deneme örneklerine sahiptir.|
    |Mavi|Görev çalışıyor.|
    |Beyaz|Görev çalıştırmak için bekliyor veya sahne atladı.|
    |Red|Görev başarısız oldu.|

     ![Spark Application & İş Grafiği sayfasında bir görev çalıştırıyorum.](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)

     Atlanan aşamalar beyaz olarak görüntülenir.
    ![Spark Application & İş Grafiği sayfasında atlanan bir görev.](./media/apache-azure-spark-history-server/sparkui-graph-color-skip.png)

    ![Spark Uygulaması & İş Grafiği sayfasında başarısız bir görev.](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)

     > [!NOTE]  
     > Tamamlanan işler için oynatma kullanılabilir. İşi geri oynatmak için **Oynatma** düğmesini seçin. Durdurma düğmesini seçerek istediğiniz zaman işi durdurun. Bir iş oynandığında, her görev durumunu renge göre görüntüler. Eksik işler için oynatma desteklenmez.

+ İş grafiğini yakınlaştırmak veya uzaklaştırmak için kaydırın veya kaydırın veya **ekrana sığması için sığdırmak için Yakınlaştır'ı** seçin.

    ![Spark Uygulaması & İş Grafiği sayfasına sığacak şekilde Yakınlaştır'ı seçin.](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Görevler başarısız olduğunda, araç ucunu görmek için grafik düğümünün üzerine gidin ve ardından yeni bir sayfada açmak için sahneyi seçin.

    ![Kıvılcım Uygulaması & İş Grafiği sayfasındaaraç ipucunu görüntüleyin.](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ Spark Application & İş Grafiği sayfasında, görevler bu koşulları karşılıyorsa aşamalar araç ipuçlarını ve küçük simgeleri görüntüler:
  + Veri eğriliği: Veri okuma boyutu > ortalama veri okuma boyutu bu aşamadaki tüm görevlerin okuma boyutu * 2 *ve* veri okuma boyutu > 10 MB.
  + Zaman çarpıklık: Yürütme süresi > bu aşamada * 2 *ve* yürütme süresi > 2 dakika içinde tüm görevlerin ortalama yürütme süresi.

    ![Kıvılcım Uygulaması & İş Grafiği sayfasındaki çarpık görev simgesi.](./media/apache-azure-spark-history-server/sparkui-graph-skew-icon.png)

+ İş grafiği düğümü her aşama hakkında aşağıdaki bilgileri görüntüler:
  + Kimlik
  + Ad veya açıklama
  + Toplam görev numarası
  + Veri okuma: giriş boyutu ve shuffle okuma boyutu toplamı
  + Veri yazma: çıktı boyutu ve shuffle yazma boyutunun toplamı
  + Yürütme süresi: ilk denemenin başlangıç saati ile son denemenin tamamlanma süresi arasındaki süre
  + Satır sayısı: giriş kayıtlarının toplamı, çıktı kayıtları, karışık okuma kayıtları ve karışık yazma kayıtları
  + İlerleme durumu

    > [!NOTE]  
    > Varsayılan olarak, iş grafiği düğümü her aşamanın son denemesinden (aşama yürütme süresi hariç) bilgileri görüntüler. Ancak oynatma sırasında, iş grafiği düğümü her deneme hakkında bilgi gösterir.

    > [!NOTE]  
    > Veri okuma ve veri yazma boyutları için 1MB = 1000 KB = 1000 * 1000 bayt kullanıyoruz.

+ **Bize geri bildirim sağlayın'ı**seçerek sorunlar hakkında geri bildirim gönderin.

    ![Kıvılcım Uygulaması & İş Grafiği sayfasındaki geri bildirim seçeneği.](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="use-the-diagnosis-tab-in-the-spark-history-server"></a>Spark History Server'da Tanı sekmesini kullanma

İş kimliğini seçin ve ardından iş tanı görünümünü görmek için araç menüsünde **Tanı'yı** seçin. **Tanı** sekmesi **Veri Eğriltme,** **Zaman Çarpık**ve Uygulayıcı **Kullanım Analizi**içerir.

+ Sırasıyla sekmeleri seçerek **Veri Eğriltme,** **Zaman Eğriltme**ve **Uygulayıcı Kullanım Çözümlemesi** gözden geçirin.

    ![Tanı sekmesindeki Veri Eğriltme sekmesi.](./media/apache-azure-spark-history-server/sparkui-diagnosis-tabs.png)

### <a name="data-skew"></a>Veri Eğriltme

Veri **Eğriltme** sekmesini seçin. Belirtilen parametrelere göre karşılık gelen çarpık görevler görüntülenir.

#### <a name="specify-parameters"></a>Parametreleri Belirt

**Parametreleri Belirt** bölümü, Veri Eğriliğini algılamak için kullanılan parametreleri görüntüler. Varsayılan kural şudur: Okunan görev verileri, okunan ortalama görev verilerinin üç katından büyüktür ve okunan görev verileri 10 MB'dan fazladır. Çarpık görevler için kendi kuralınızı tanımlamak istiyorsanız, parametrelerinizi seçebilirsiniz. **Çarpık Sahne ve** **Eğriltme Grafiği** bölümleri buna göre güncellenir.

#### <a name="skewed-stage"></a>Çarpık Sahne

**Çarpık Sahne** bölümü, belirtilen ölçütleri karşılayan çarpık görevleri olan aşamaları görüntüler. Bir aşamada birden fazla çarpık görev varsa, Çarpık **Aşama** bölümü yalnızca en çarpık görevi görüntüler (diğer bir deyişle, veri eğriliği için en büyük veri).

![Tanı sekmesindeki Veri Eğriliği sekmesinin daha büyük görünümü.](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section2.png)

##### <a name="skew-chart"></a>Çarpık Grafik

**Eğrilme Aşaması** tablosunda bir satır seçtiğinizde, **Eğrilme Grafiği** veri okuma ve yürütme süresine bağlı olarak daha fazla görev dağıtım ayrıntıları görüntüler. Eğriltilmiş görevler kırmızı ve normal görevler mavi ile işaretlenir. Performans değerlendirmesi için grafik en fazla 100 örnek görev görüntüler. Görev ayrıntıları sağ alt panelde görüntülenir.

![Kıvılcım UI'de Sahne 10 için Çarpık Grafik.](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section3.png)

### <a name="time-skew"></a>Zaman Çarpık

**Zaman Eğriltme** sekmesi, görev yürütme süresine bağlı olarak çarpık görevleri görüntüler.

#### <a name="specify-parameters"></a>Parametreleri Belirt

**Parametreleri Belirt** bölümü, zamanı algılamak için kullanılan parametreleri görüntüler. Varsayılan kural şudur: Görev yürütme süresi ortalama yürütme süresinin üç katından büyüktür ve görev yürütme süresi 30 saniyeden büyüktür. İhtiyaçlarınıza göre parametreleri değiştirebilirsiniz. **Çarpık Sahne ve** **Eğriltme Grafiği,** **tıpkı Data Skew** sekmesinde olduğu gibi ilgili aşamaları ve görevleri görüntüler.

**Zaman Eğriltme'yi**seçtiğinizde, filtre uygulanmış sonuç **Parametreleri Belirt** bölümünde ayarlanan parametrelere göre **Çarpık Aşama** bölümünde görüntülenir. **Çarpık Sahne Alanı** bölümünde bir öğe seçtiğinizde, ilgili grafik üçüncü bölümde hazırlanır ve görev ayrıntıları sağ alt panelde görüntülenir.

![Tanı sekmesiiçindeki Zaman eğrilme sekmesi.](./media/apache-azure-spark-history-server/sparkui-diagnosis-timeskew-section2.png)

### <a name="executor-usage-analysis-graphs"></a>Executor Kullanım Analizi grafikleri

**Executor Kullanım Grafiği,** işin gerçek yürütücü tahsisini ve çalışma durumunu görüntüler.  

**Executor Kullanım Analizi'ni**seçtiğinizde, uygulayıcı kullanımı yla ilgili dört farklı eğri hazırlanır: **Ayrılan Uygulayıcılar**, **Çalıştıran Uygulayıcılar**, **boşta Uygulayıcılar**ve **Max Executor Örnekleri**. Eklenen veya **çıkarılan** her **Uygulayıcı,** ayrılan uygulayıcıları artıracak veya azaltacaktır. Daha fazla karşılaştırma için **İşler** sekmesinde **Olay Zaman Çizelgesi'ni** denetleyebilirsiniz.

![Tanı sekmesindeki Executor Kullanım Analizi sekmesi.](./media/apache-azure-spark-history-server/sparkui-diagnosis-executors.png)

Tüm taslaklarda karşılık gelen içeriği seçmek veya seçmek için renk simgesini seçin.

 ![Executor Kullanım Analizi sekmesinde grafiği seçin.](./media/apache-azure-spark-history-server/sparkui-diagnosis-select-chart.png)

## <a name="faq"></a>SSS

### <a name="how-do-i-revert-to-the-community-version"></a>Topluluk sürümüne nasıl geri dönerim?

Topluluk sürümüne geri dönmek için aşağıdaki adımları yapın.

1. Ambari'de kümeyi açın.
1. **Spark2** > **Configs**gidin.
1. **Özel spark2-varsayılanları**seçin.
1. **Özellik Ekle'yi**seçin ... .
1. **spark.ui.enhancement.enabled=false**ekleyin ve sonra kaydedin.
1. Mülk şimdi **yanlış** ayarlar.
1. Yapılandırmayı kaydetmek için **Kaydet'i** seçin.

    ![Apache Ambari'deki bir özelliği kapatın.](./media/apache-azure-spark-history-server/apache-spark-turn-off.png)

1. Sol panelde **Spark2'yi** seçin. Ardından, **Özet** sekmesinde **Spark2 History Server'ı**seçin.

    ![Apache Ambari'deki özet görünüm.](./media/apache-azure-spark-history-server/apache-spark-restart1.png)

1. Spark History Server'ı yeniden başlatmak için, **Spark2 History Server'ın**sağındaki **Başlangıç** düğmesini ve ardından açılan menüden **Yeniden Başlat'ı** seçin.

    ![Apache Ambari'deki Kıvılcım Geçmişi Sunucusunu yeniden başlatın.](./media/apache-azure-spark-history-server/apache-spark-restart2.png)  

1. Spark History Server web UI'sini yenileyin. Topluluk versiyonuna geri dönecektir.

### <a name="how-do-i-upload-a-spark-history-server-event-to-report-it-as-an-issue"></a>Bir Spark History Server olayını sorun olarak bildirmek için nasıl yükleyebilirim?

Spark History Server'da bir hatayla karşılaştıysanız, olayı bildirmek için aşağıdaki adımları yapın.

1. Spark History Server web UI'de **İndir'i** seçerek etkinliği indirin.

    ![Olayı Spark History Server UI'da indirin.](./media/apache-azure-spark-history-server/sparkui-download-event.png)

2. Kıvılcım Uygulaması & **İş Grafiği** sayfasından bize geri **bildirimde** bulunun'u seçin.

    ![Kıvılcım Uygulaması & İş Grafiği sayfasından geri bildirim de bildirin](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

3. Başlığı ve hatanın açıklamasını sağlayın. Ardından,.zip dosyasını düzenleme alanına sürükleyin ve **yeni sorun gönder'i**seçin.

    ![Yeni bir sorun yükleyin ve gönderin.](./media/apache-azure-spark-history-server/apache-spark-file-issue.png)

### <a name="how-do-i-upgrade-a-jar-file-in-a-hotfix-scenario"></a>Düzeltme senaryosunda bir .jar dosyayı nasıl yükseltebilirim?

Bir düzeltmeyle yükseltmek istiyorsanız, yükseltecek `spark-enhancement.jar*`aşağıdaki komut dosyasını kullanın.

**upgrade_spark_enhancement.ş**:

   ```bash
    #!/usr/bin/env bash

    # Copyright (C) Microsoft Corporation. All rights reserved.

    # Arguments:
    # $1 Enhancement jar path

    if [ "$#" -ne 1 ]; then
        >&2 echo "Please provide the upgrade jar path."
        exit 1
    fi

    install_jar() {
        tmp_jar_path="/tmp/spark-enhancement-hotfix-$( date +%s )"

        if wget -O "$tmp_jar_path" "$2"; then
            for FILE in "$1"/spark-enhancement*.jar
            do
                back_up_path="$FILE.original.$( date +%s )"
                echo "Back up $FILE to $back_up_path"
                mv "$FILE" "$back_up_path"
                echo "Copy the hotfix jar file from $tmp_jar_path   to $FILE"
                cp "$tmp_jar_path" "$FILE"

                "Hotfix done."
                break
            done
        else    
            >&2 echo "Download jar file failed."
            exit 1
        fi
    }

    jars_folder="/usr/hdp/current/spark2-client/jars"
    jar_path=$1

    if ls ${jars_folder}/spark-enhancement*.jar 1>/dev/null 2>&1;   then
        install_jar "$jars_folder" "$jar_path"
    else
        >&2 echo "There is no target jar on this node. Exit with no action."
        exit 0
    fi
   ```

#### <a name="usage"></a>Kullanım

`upgrade_spark_enhancement.sh https://${jar_path}`

#### <a name="example"></a>Örnek

`upgrade_spark_enhancement.sh https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar`

#### <a name="use-the-bash-file-from-the-azure-portal"></a>Azure portalındaki bash dosyasını kullanma

1. Azure [portalını](https://ms.portal.azure.com)başlatın ve ardından kümenizi seçin.
2. Aşağıdaki parametrelerle bir [komut dosyası eylemini](../hdinsight-hadoop-customize-cluster-linux.md) tamamlayın.

    |Özellik |Değer |
    |---|---|
    |Komut dosyası türü|- Özel|
    |Adı|Yükseltme Kavanozu|
    |Bash script URI|`https://hdinsighttoolingstorage.blob.core.windows.net/shsscriptactions/upgrade_spark_enhancement.sh`|
    |Düğüm türü(ler)|Baş, İşçi|
    |Parametreler|`https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar`|

     ![Azure portalı komut dosyası eylemi gönderme](./media/apache-azure-spark-history-server/apache-spark-upload1.png)

## <a name="known-issues"></a>Bilinen sorunlar

+ Şu anda, Spark History Server yalnızca Spark 2.3 ve 2.4 için çalışmaktadır.

+ RDD kullanan giriş ve çıktı verileri **Veri** sekmesinde görüntülemeyecek.

## <a name="next-steps"></a>Sonraki adımlar

+ [HDInsight'ta bir Apache Spark kümesinin kaynaklarını yönetme](apache-spark-resource-manager.md)
+ [Apache Spark ayarlarını yapılandırma](apache-spark-settings.md)

## <a name="feedback"></a>Geri Bildirim

Herhangi bir geri bildiriminiz varsa veya bu aracı kullanırken[hdivstool@microsoft.com](mailto:hdivstool@microsoft.com)herhangi bir sorunla karşılaşırsanız, e-posta gönderin ( ).
