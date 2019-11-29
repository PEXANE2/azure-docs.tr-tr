---
title: Uygulamalarda hata ayıklamak için genişletilmiş Spark geçmiş sunucusu-Azure HDInsight
description: Spark uygulamalarında hata ayıklamak ve tanılamak için genişletilmiş Spark geçmiş sunucusunu kullanın-Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/25/2019
ms.openlocfilehash: 7e9ab0e41086a4c9478f95c5a56754640feeab4e
ms.sourcegitcommit: c31dbf646682c0f9d731f8df8cfd43d36a041f85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74561824"
---
# <a name="use-extended-apache-spark-history-server-to-debug-and-diagnose-apache-spark-applications"></a>Apache Spark uygulamalarda hata ayıklamak ve tanılamak için genişletilmiş Apache Spark geçmiş sunucusunu kullanma

Bu makalede, hata ayıklama ve tanılama uygulamalarının çalıştırılması için genişletilmiş Apache Spark geçmiş sunucusunun nasıl kullanılacağına ilişkin yönergeler sunulmaktadır. Uzantı, veri sekmesini ve grafik sekmesini ve tanılama sekmesini içerir. **Veriler** sekmesinde, kullanıcılar Spark işinin giriş ve çıkış verilerini denetleyebilir. **Grafik** sekmesinde, kullanıcılar veri akışını denetleyebilir ve iş grafiğini yeniden oynatılabilir. **Tanılama** sekmesinde, Kullanıcı **veri eğme**, **zaman eğriltme**ve **yürütücü kullanım analizine**başvurabilir.

## <a name="get-access-to-apache-spark-history-server"></a>Apache Spark geçmiş sunucusuna erişim sağlayın

Apache Spark geçmiş sunucusu, tamamlanan ve Spark uygulamalarının çalıştırıldığı Web Kullanıcı arabirimi.

### <a name="open-the-apache-spark-history-server-web-ui-from-azure-portal"></a>Azure portal Apache Spark geçmiş sunucusu Web Kullanıcı arabirimini açın

1. [Azure Portal](https://portal.azure.com/)Spark kümesini açın. Daha fazla bilgi için bkz. [kümeleri listeleme ve gösterme](../hdinsight-administer-use-portal-linux.md#showClusters).
2. **Küme panolarından** **Spark geçmiş sunucusu**' nu seçin. İstendiğinde Spark kümesi için yönetici kimlik bilgilerini girin.

    ![Portal Spark geçmiş sunucusunu Başlat](./media/apache-azure-spark-history-server/azure-portal-dashboard-spark-history.png "Spark geçmiş sunucusu")

### <a name="open-the-spark-history-server-web-ui-by-url"></a>URL 'ye göre Spark geçmiş sunucusu Web Kullanıcı arabirimini açın

CLUSTERNAME 'ın Spark Kümenizin adı olduğu `https://CLUSTERNAME.azurehdinsight.net/sparkhistory` göz atarak Spark geçmiş sunucusunu açın.

Spark geçmiş sunucusu Web Kullanıcı arabirimi şuna benzer olabilir:

![HDInsight Spark geçmiş sunucusu](./media/apache-azure-spark-history-server/hdinsight-spark-history-server.png)

## <a name="data-tab-in-spark-history-server"></a>Spark geçmiş sunucusunda veri sekmesi

Veri görünümünü almak için iş KIMLIĞI ' ni seçin ve araç menüsündeki **veriler** ' i seçin.

+ Sekmeleri ayrı olarak seçerek **girişleri**, **çıkışları**ve **tablo işlemlerini** gözden geçirin.

    ![Spark uygulama sekmeleri için veriler](./media/apache-azure-spark-history-server/apache-spark-data-tabs.png)

+ Düğme **Kopyala**' yı seçerek tüm satırları kopyalayın.

    ![Spark uygulaması kopyalama verileri](./media/apache-azure-spark-history-server/apache-spark-data-copy.png)

+ Düğme **CSV**'yi seçerek tüm verileri CSV dosyası olarak kaydedin.

    ![Spark uygulaması kaydetme verileri](./media/apache-azure-spark-history-server/apache-spark-data-save.png)

+ Arama sonucu, alan **aramasına**anahtar sözcükler girerek hemen görüntülenecektir.

    ![Spark uygulaması arama verileri](./media/apache-azure-spark-history-server/apache-spark-data-search.png)

+ Tabloyu sıralamak için sütun üst bilgisini seçin, daha fazla ayrıntı göstermek için bir satırı genişletmek üzere artı işaretini seçin ya da bir satırı daraltmak için eksi işaretini seçin.

    ![Spark uygulama tablosu verileri](./media/apache-azure-spark-history-server/apache-spark-data-table.png)

+ Tek bir dosyayı indirin sağ tarafta bulunan düğme **kısmi indirmeyi** seçip seçili dosya artık mevcut değilse, hata iletilerini göstermek için yeni bir sekme açar.

    ![Spark uygulaması indirme satırı verileri](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ Tam yolu **Kopyala**, indirme menüsünden genişleyen **göreli yolu Kopyala** ' yı seçerek tam yolu veya göreli yolu kopyalayın. Azure Data Lake depolama dosyaları için **Azure Depolama Gezgini ' de açın** Azure Depolama Gezgini başlatılır ve oturum açarken klasörü bulun.

    ![Spark uygulaması kopyalama yolu verileri](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Tek sayfada görüntülenecek çok fazla satır olduğunda sayfalarda gezinmek için tablonun altındaki sayıyı seçin.

    ![Spark uygulama sayfası verileri](./media/apache-azure-spark-history-server/apache-spark-data-page.png)

+ Araç ipucunu göstermek için verilerin yanındaki soru işaretine gelin veya daha fazla bilgi almak için soru işaretini seçin.

    ![Spark uygulaması için veriler daha fazla bilgi](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+ **Bize geri bildirim sağlayın**' a tıklayarak sorunlar ile geri bildirim gönderin.

    ![Spark Graph bize geri bildirim sağlar](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="graph-tab-in-apache-spark-history-server"></a>Apache Spark geçmiş sunucuda grafik sekmesi

İş KIMLIĞI ' ni seçin ve ardından araç menüsündeki **Graph** ' a tıklayarak iş grafiği görünümünü alın.

+ Oluşturulan iş grafiğine göre işinize genel bakışı gözden geçirin.

+ Varsayılan olarak, tüm işleri gösterir ve **Iş kimliğine**göre filtrelenebilir.

    ![Spark uygulaması ve iş grafiği iş KIMLIĞI](./media/apache-azure-spark-history-server/apache-spark-graph-jobid.png)

+ Varsayılan olarak, **ilerleme** seçilidir. Kullanıcı, **ekran**açılan listesinde **Oku/yazılır** ' i seçerek veri akışını denetleyebilir.

    ![Spark uygulaması ve iş grafiği görüntüsü](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

    Grafik düğümü, heatmap 'i gösteren renkli renkte görüntülenir.

    ![Spark uygulaması ve iş grafiği ısı haritasını](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)

+ **Kayıttan yürütme** düğmesini seçip Durdur düğmesini seçerek işi yeniden yürütün. Kayıttan yürütme sırasında farklı durumu göstermek için görev Color olarak görüntülenir:

    |Renk |Açıklama |
    |---|---|
    |Yeşil|İş başarıyla tamamlandı.|
    |Orange|Başarısız olan, ancak işin nihai sonucunu etkilemeyen görev örnekleri. Bu görevler daha sonra başarılı olabilecek örnekleri tekrarlamıştır veya yeniden dener.|
    |Mavi|Görev çalışıyor.|
    |Beyaz|Görev çalışmayı bekliyor veya aşama atlandı.|
    |Kırmızı|Görev başarısız oldu.|

    ![Spark uygulaması ve iş grafiği renk örneği, çalışıyor](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)

    Atlanan aşama beyaz olarak görüntülenir.
    Spark uygulaması ve iş grafiği renk örneği ![](./media/apache-azure-spark-history-server/sparkui-graph-color-skip.png) atla

    ![Spark uygulaması ve iş grafiği renk örneği, başarısız oldu](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)

    > [!NOTE]  
    > Her iş için kayıttan yürütmeye izin verilir. Tamamlanmamış iş için kayıttan yürütme desteklenmez.

+ Fare, iş grafiğini yakınlaştırmak/uzaklaştırmak için kayar veya ekrana sığacak hale getirmek için **Yakınlaştır** ' a tıklayın.

    ![Spark uygulaması ve iş grafiği sığacak kadar yaklaştır](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Başarısız görevler olduğunda araç ipucunu görmek için Graph düğümünde üzerine gelin ve aşama sayfasını açmak için aşama ' a tıklayın.

    ![Spark uygulaması ve iş grafiği araç ipucu](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ İş grafiği sekmesinde, aşağıdaki koşullara uyan görevler varsa, aşamalar araç ipucu ve küçük simge olarak görüntülenir:
  + Veri eğriltme: veri okuma boyutu > Bu aşama içindeki tüm görevlerin ortalama veri okuma boyutu * 2 ve veri okuma boyutu > 10 MB.
  + Zaman eğriltme: yürütme süresi > Bu aşama içindeki tüm görevlerin ortalama yürütme süresi * 2 ve yürütme süresi > 2 dakika.

    ![Spark uygulaması ve iş grafiği eğriltme simgesi](./media/apache-azure-spark-history-server/sparkui-graph-skew-icon.png)

+ İş grafiği düğümü her aşama için aşağıdaki bilgileri görüntüler:
  + Numarasını.
  + Ad veya açıklama.
  + Toplam görev numarası.
  + Okunan veriler: giriş boyutu ve karışık okuma boyutu toplamı.
  + Veri yazma: çıkış boyutunun ve karışık yazma boyutunun toplamı.
  + Yürütme Süresi: son denemede ilk deneme ve tamamlanma zamanının başlangıç saati arasındaki süre.
  + Satır sayısı: giriş kayıtlarının toplamı, çıkış kayıtları, okuma kayıtlarını karıştırma ve yazma kayıtlarını karıştırma.
  + Lemesine.

    > [!NOTE]  
    > Varsayılan olarak, iş grafiği düğümü her bir aşamanın son denemesinden (aşama yürütme süresi hariç) bilgileri görüntüler, ancak oynatma grafiği düğümü sırasında her girişimin bilgileri gösterilir.

    > [!NOTE]  
    > Okuma ve yazma veri boyutu için 1 MB/1000 KB = 1000 * 1000 bayt kullanılır.

+ Sorunları **bize geri bildirim**gönderin ' i seçerek geri bildirimde bulunun.

    ![Spark uygulaması ve iş grafiği geri bildirimi](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="diagnosis-tab-in-apache-spark-history-server"></a>Apache Spark geçmiş sunucusunda Tanılama sekmesi

İş KIMLIĞI ' ni seçin ve ardından araç menüsünde **Tanılama** ' yı seçerek iş tanılama görünümünü alın. Tanılama sekmesi **veri eğriltme**, **zaman eğriltme**ve **yürütücü kullanım analizini**içerir.

+ Sırasıyla sekmeleri seçerek **veri eğriltme**, **zaman eğriltme**ve **yürütücü kullanım analizini** gözden geçirin.

    ![Mini UI tanılama veri eğriltme sekmesi yeniden](./media/apache-azure-spark-history-server/sparkui-diagnosis-tabs.png)

### <a name="data-skew"></a>Veri eğriltme

**Veri eğriltme** sekmesini seçin, ilgili asimetrik görevler belirtilen parametrelere göre görüntülenir.

+ **Parametreleri belirtin** -ilk bölüm, veri eğriliğini algılamak için kullanılan parametreleri görüntüler. Yerleşik kural: veri okuma, ortalama görev verilerinin üç katından daha büyük ve okunan görev verisi 10 MB 'tan fazla. Çarpıtılmış görevler için kendi kuralınızı tanımlamak istiyorsanız, parametrelerinizi, **asimetrik aşamayı**ve **çarpıklık** bölümünü seçebilirsiniz.

+ **Çarpıkık aşama** -ikinci bölüm, yukarıda belirtilen ölçütlere uyan, eğimli görevlere sahip aşamaları görüntüler. Bir aşamada birden çok asimetrik görev varsa, asimetrik aşama tablosu yalnızca en çok eğilmiş görevi görüntüler (ör. veri eğriliği için en büyük veriler).

    ![Mini UI tanılama veri eğriltme sekmesi](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section2.png)

+ **Grafiği eğriltme** : eğme aşaması tablosundaki bir satır seçildiğinde, eğriltme grafiği, verileri okuma ve yürütme zamanına göre daha fazla görev dağıtım ayrıntısı görüntüler. Asimetrik görevler kırmızı olarak işaretlenir ve normal görevler mavi olarak işaretlenir. Performans değerlendirmesi için grafik yalnızca en fazla 100 örnek görevi görüntüler. Görev ayrıntıları sağ alt panelde görüntülenir.

    ![10. aşama için mini UI eğriltme grafiği](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section3.png)

### <a name="time-skew"></a>Zaman eğriltme

**Zaman eğriltme** sekmesi, görev yürütme zamanına göre asimetrik görevleri görüntüler.

+ **Parametreleri belirtin** -ilk bölüm, zaman eğriliğini algılamak için kullanılan parametreleri görüntüler. Zaman eğriliğini algılamaya yönelik varsayılan ölçüt: görev yürütme süresi, ortalama yürütme süresinin üç katından büyük ve görev yürütme süresi 30 saniyeden fazla. Parametreleri gereksinimlerinize göre değiştirebilirsiniz. **Asimetrik aşama** ve **eğme grafiği** , yukarıdaki **veri eğriltme** sekmesinde olduğu gibi ilgili aşamaları ve görev bilgilerini görüntüler.

+ **Zaman eğriliğini**seçin, sonra filtrelenmiş sonuç, bölüm **belirtme**bölümünde ayarlanan parametrelere göre **asimetrik aşama** bölümünde görüntülenir. **Asimetrik aşama** bölümünde bir öğe seçin, ardından karşılık gelen grafik section3 içinde drafted ve görev ayrıntıları sağ alt panelde görüntülenir.

    ![Mini UI tanılama zaman eğriltme bölümü](./media/apache-azure-spark-history-server/sparkui-diagnosis-timeskew-section2.png)

### <a name="executor-usage-analysis"></a>Yürütücü Kullanım Analizi

Yürütücü kullanım grafiği, Spark işi gerçek yürütücü ayırmayı ve çalışma durumunu görselleştirir.  

+ **Yürütücü Kullanım Analizi**' ni seçin, sonra da Yürütücü kullanımı hakkında dört tür eğrileri, **ayrılmış Yürüticileri**, **yürüticileri**, **Boşta**çalışma ve **en fazla yürütücü örneklerini**içeren drafted. Ayrılmış yürüticilere ilgili olarak, her "yürütücü eklenmiş" veya "yürütücü kaldırıldı" olayı ayrılan yürütmeleri arttırır veya azaltacaktır, daha fazla karşılaştırma için "Işler" sekmesinde "olay zaman çizelgesini" kontrol edebilirsiniz.

    ![Mini UI tanılama yürüticileri sekmesi](./media/apache-azure-spark-history-server/sparkui-diagnosis-executors.png)

+ Tüm taslaklarına karşılık gelen içeriği seçmek veya seçimini kaldırmak için renk simgesini seçin.

    ![Mini UI tanısı grafik seç](./media/apache-azure-spark-history-server/sparkui-diagnosis-select-chart.png)

## <a name="faq"></a>SSS

### <a name="1-revert-to-community-version"></a>1. topluluk sürümüne dönün

Topluluk sürümüne dönmek için aşağıdaki adımları uygulayın:

1. Kümeyi ambarı 'nda açın.
1. **Spark2** > **configs** > **özel Spark2-varsayılanlar**sayfasına gidin.
1. **Özellik Ekle...** seçeneğini belirleyin, **spark. UI. geliştirmesi ekleyin. Enabled = False**, Kaydet.
1. Özelliği şimdi **yanlış** olarak ayarlanır.
1. Yapılandırmayı kaydetmek için **Kaydet** ' i seçin.

    ![Apache ambarı özelliği kapanır](./media/apache-azure-spark-history-server/apache-spark-turn-off.png)

1. Sol bölmede **Spark2** öğesini seçin, **Özet** sekmesinde, **Spark2 geçmiş sunucusu**' nu seçin.

    ![Apache ambarı Spark2 Özet görünümü](./media/apache-azure-spark-history-server/apache-spark-restart1.png)

1. **Spark2 geçmiş sunucusunun** **yeniden başlatılmasını** seçerek geçmiş sunucusunu yeniden başlatın.

    ![Apache ambarı Spark2 geçmişi yeniden başlatması](./media/apache-azure-spark-history-server/apache-spark-restart2.png)  
1. Spark geçmiş sunucusu Web Kullanıcı arabirimini yenileyerek, bu işlem topluluk sürümüne geri döndürülecek.

### <a name="2-upload-history-server-event"></a>2. geçmiş sunucusu olayını karşıya yükle

Geçmiş sunucu hatasıyla karşılaşırsanız, olayı sağlamak için aşağıdaki adımları izleyin:

1. Geçmiş sunucusu Web Kullanıcı arabiriminde **İndir** ' i seçerek olayı indirin.

    ![Spark2 geçmiş sunucusu indirme](./media/apache-azure-spark-history-server/sparkui-download-event.png)

2. Veri/grafik sekmesinden **bize geri bildirim sağla** ' yı seçin.

    ![Spark Graph bize geri bildirim sağlar](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

3. Hatanın başlığını ve açıklamasını girip, ZIP dosyasını düzenleme alanına sürükleyin ve sonra **yeni sorun gönder**' e tıklayın.

    ![Apache Spark dosyası sorun örneği](./media/apache-azure-spark-history-server/apache-spark-file-issue.png)

### <a name="3-upgrade-jar-file-for-hotfix-scenario"></a>3. bir düzeltme senaryosu için jar dosyasını yükseltin

Düzeltme ile yükseltmek istiyorsanız, aşağıdaki betiği kullanın, Spark-Enhancement. jar * öğesini yükseltecek.

**upgrade_spark_enhancement. sh**:

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

**Kullanım**:

`upgrade_spark_enhancement.sh https://${jar_path}`

**Örnek**:

`upgrade_spark_enhancement.sh https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar`

**Azure portal bash dosyasını kullanmak için**

1. [Azure Portal](https://ms.portal.azure.com)başlatın ve kümenizi seçin.
2. Aşağıdaki parametrelerle bir [betik eylemi](../hdinsight-hadoop-customize-cluster-linux.md) doldurun:

    |Özellik |Değer |
    |---|---|
    |Betik türü|-Özel|
    |Adı|UpgradeJar|
    |Bash betiği URI 'SI|`https://hdinsighttoolingstorage.blob.core.windows.net/shsscriptactions/upgrade_spark_enhancement.sh`|
    |Düğüm türleri|Baş, çalışan|
    |Parametreler|`https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar`|

     ![Betik eylemini Azure portal gönder](./media/apache-azure-spark-history-server/apache-spark-upload1.png)

## <a name="known-issues"></a>Bilinen sorunlar

+ Şu anda yalnızca Spark 2,3 ve 2,4 kümesi için geçerlidir.

+ RDD kullanan giriş/çıkış verileri, veri sekmesinde gösterilmez.

## <a name="next-steps"></a>Sonraki adımlar

+ [HDInsight üzerinde Apache Spark kümesi için kaynakları yönetme](apache-spark-resource-manager.md)
+ [Apache Spark ayarlarını yapılandırma](apache-spark-settings.md)

## <a name="contact-us"></a>Bize ulaşın

Herhangi bir geri bildiriminiz varsa veya bu aracı kullanırken herhangi bir sorunla karşılaşırsanız ([hdivstool@microsoft.com](mailto:hdivstool@microsoft.com)) adresine bir e-posta gönderin.
