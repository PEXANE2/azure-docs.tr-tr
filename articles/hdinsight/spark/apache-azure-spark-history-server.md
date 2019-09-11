---
title: Spark uygulamalarında hata ayıklamak için genişletilmiş Spark geçmiş sunucusu-Azure HDInsight
description: Spark uygulamalarında hata ayıklamak ve tanılamak için genişletilmiş Spark geçmiş sunucusunu kullanın-Azure HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 09/04/2019
ms.openlocfilehash: 9ba03f6b1e658e08c3d07d7ccb5e2a99e96fe69c
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70736463"
---
# <a name="use-extended-apache-spark-history-server-to-debug-and-diagnose-apache-spark-applications"></a>Apache Spark uygulamalarda hata ayıklamak ve tanılamak için genişletilmiş Apache Spark geçmiş sunucusunu kullanma

Bu makalede, hata ayıklama ve tanılama uygulamalarının çalıştırılması için genişletilmiş Apache Spark geçmiş sunucusunun nasıl kullanılacağına ilişkin yönergeler sunulmaktadır. Uzantı, veri sekmesini ve grafik sekmesini ve tanılama sekmesini içerir. **Veriler** sekmesinde, kullanıcılar Spark işinin giriş ve çıkış verilerini denetleyebilir. **Grafik** sekmesinde, kullanıcılar veri akışını denetleyebilir ve iş grafiğini yeniden oynatılabilir. **Tanılama** sekmesinde, Kullanıcı **veri eğme**, **zaman eğriltme** ve **yürütücü kullanım analizine**başvurabilir.

## <a name="get-access-to-apache-spark-history-server"></a>Apache Spark geçmiş sunucusuna erişim sağlayın

Apache Spark geçmiş sunucusu, tamamlanan ve Spark uygulamalarının çalıştırıldığı Web Kullanıcı arabirimi. 

### <a name="open-the-apache-spark-history-server-web-ui-from-azure-portal"></a>Azure portal Apache Spark geçmiş sunucusu Web Kullanıcı arabirimini açın

1. [Azure Portal](https://portal.azure.com/)Spark kümesini açın. Daha fazla bilgi için bkz. [kümeleri listeleme ve gösterme](../hdinsight-administer-use-portal-linux.md#showClusters).
2. **Hızlı bağlantılardan** **küme panosu**' na ve ardından **Spark geçmiş sunucusu**' na tıklayın. İstendiğinde Spark kümesi için yönetici kimlik bilgilerini girin. 

    ![Spark geçmiş sunucusu](./media/apache-azure-spark-history-server/launch-history-server.png "Spark geçmiş sunucusu")

### <a name="open-the-spark-history-server-web-ui-by-url"></a>URL 'ye göre Spark geçmiş sunucusu Web Kullanıcı arabirimini açın
Aşağıdaki URL 'ye giderek Spark geçmiş sunucusunu açın, müşteriyi Spark kümesi adıyla `<ClusterName>` değiştirin.

   ```
   https://<ClusterName>.azurehdinsight.net/sparkhistory
   ```

Spark geçmiş sunucusu Web Kullanıcı arabirimi şöyle görünür:

![HDInsight Spark geçmiş sunucusu](./media/apache-azure-spark-history-server/hdinsight-spark-history-server.png)


## <a name="data-tab-in-spark-history-server"></a>Spark geçmiş sunucusunda veri sekmesi
Veri görünümünü almak için iş KIMLIĞI ' ni seçin ve araç menüsündeki **veriler** ' e tıklayın.

+ Sekmeleri ayrı olarak seçerek **girişleri**, **çıkışları**ve **tablo işlemlerini** kontrol edin.

    ![Veri sekmeleri](./media/apache-azure-spark-history-server/sparkui-data-tabs.png)

+ Düğme **Kopyala**' ya tıklayarak tüm satırları kopyalayın.

    ![Veri kopyalama](./media/apache-azure-spark-history-server/sparkui-data-copy.png)

+ Düğme **CSV**' ye tıklayarak tüm verileri CSV dosyası olarak kaydedin.

    ![Veri kaydetme](./media/apache-azure-spark-history-server/sparkui-data-save.png)

+ Arama sonucu, alan **aramasına**anahtar sözcükler girerek hemen görüntülenecektir.

    ![Veri arama](./media/apache-azure-spark-history-server/sparkui-data-search.png)

+ Tabloyu sıralamak için sütun başlığına tıklayın, daha fazla ayrıntı göstermek üzere bir satırı genişletmek için artı işaretine tıklayın veya bir satırı daraltmak için eksi işaretine tıklayın.

    ![Veri tablosu](./media/apache-azure-spark-history-server/sparkui-data-table.png)

+ Tek bir dosyayı indirin ve sağ tarafta bulunan düğme **kısmi indirmesi** ' ne tıklayarak seçili dosya yerel olarak indirilir, ancak dosya artık yoksa hata iletilerini göstermek için yeni bir sekme açar.

    ![Veri indirme satırı](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ Tam yolu **Kopyala**, indirme menüsünden genişleyen **göreli yolu Kopyala** ' yı seçerek tam yolu veya göreli yolu kopyalayın. Azure Data Lake depolama dosyaları için **Azure Depolama Gezgini ' de açın** Azure Depolama Gezgini başlatılır ve oturum açtığınızda klasörü bulun.

    ![Veri kopyalama yolu](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Tek sayfada görüntülenecek çok fazla satır olduğunda sayfalarda gezinmek için tablonun altındaki sayıya tıklayın. 

    ![Veri sayfası](./media/apache-azure-spark-history-server/sparkui-data-page.png)

+ Araç ipucunu göstermek için verilerin yanındaki soru işaretine gelin veya daha fazla bilgi edinmek için soru işaretine tıklayın.

    ![Veri daha fazla bilgi](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+ **Bize geri bildirim sağlayın**' a tıklayarak sorunlar ile geri bildirim gönderin.

    ![grafik geri bildirimi](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="graph-tab-in-apache-spark-history-server"></a>Apache Spark geçmiş sunucuda grafik sekmesi
İş KIMLIĞI ' ni seçin ve ardından araç menüsündeki **Graph** ' a tıklayarak iş grafiği görünümünü alın.

+ Oluşturulan iş grafiğine göre işinizin genel görünümünü denetleyin. 

+ Varsayılan olarak, tüm işleri gösterir ve **Iş kimliğine**göre filtrelenebilir.

    ![Graph iş KIMLIĞI](./media/apache-azure-spark-history-server/sparkui-graph-jobid.png)

+ Varsayılan olarak, **ilerleme** seçilidir. Kullanıcı, **ekran**açılan listesinde **Oku/yazılır** ' i seçerek veri akışını denetleyebilir.

    ![grafik görüntüsü](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

    Grafik düğümü, heatmap 'i gösteren renkli renkte görüntülenir.

    ![Graf ısı haritasını](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)

+ **Kayıttan yürütme** düğmesine tıklayıp Durdur düğmesine tıklayarak her zaman durarak işi oynatın. Kayıttan yürütme sırasında farklı durumu göstermek için görev Color olarak görüntülenir:

  + Başarılı için yeşil: İş başarıyla tamamlandı.
  + Yeniden denenmek üzere turuncu: Başarısız olan ancak işin nihai sonucunu etkilemeyen görev örnekleri. Bu görevler daha sonra başarılı olabilecek örnekleri tekrarlamıştır veya yeniden dener.
  + Çalıştırmak için mavi: Görev çalışıyor.
  + Bekleyen veya Atlanan beyaz: Görev çalışmayı bekliyor veya aşama atlandı.
  + For Failed için Red: Görev başarısız oldu.

    ![grafik rengi örneği, çalışıyor](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)
 
    Atlanan aşama beyaz olarak görüntülenir.
    ![grafik rengi örneği, atla](./media/apache-azure-spark-history-server/sparkui-graph-color-skip.png)

    ![grafik rengi örneği, başarısız](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)
 
    > [!NOTE]  
    > Her iş için kayıttan yürütmeye izin verilir. Tamamlanmamış iş için kayıttan yürütme desteklenmez.


+ Fare, iş grafiğini yakınlaştırmak/uzaklaştırmak için kayar veya ekrana sığacak hale getirmek için **Yakınlaştır** ' a tıklayın.
 
    ![grafiğe sığacak kadar yaklaştır](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Başarısız görevler olduğunda araç ipucunu görmek için Graph düğümünde üzerine gelin ve aşama sayfasını açmak için aşama ' a tıklayın.

    ![Grafik araç ipucu](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ İş grafiği sekmesinde, aşağıdaki koşullara uyan görevler varsa, aşamalar araç ipucu ve küçük simge olarak görüntülenir:
  + Veri eğriltme: veri okuma boyutu > Bu aşama içindeki tüm görevlerin ortalama veri okuma boyutu * 2 ve veri okuma boyutu > 10 MB.
  + Zaman eğriltme: yürütme süresi > Bu aşama içindeki tüm görevlerin ortalama yürütme süresi * 2 ve yürütme süresi > 2 dakika.

    ![grafik eğriltme simgesi](./media/apache-azure-spark-history-server/sparkui-graph-skew-icon.png)

+ İş grafiği düğümü her aşama için aşağıdaki bilgileri görüntüler:
  + NUMARASINI.
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

+ **Bize geri bildirim sağlayın**' a tıklayarak sorunlar ile geri bildirim gönderin.

    ![grafik geri bildirimi](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="diagnosis-tab-in-apache-spark-history-server"></a>Apache Spark geçmiş sunucusunda Tanılama sekmesi
İş KIMLIĞI ' ni seçin ve ardından araç menüsünde **Tanılama** ' ya tıklayarak iş tanılama görünümünü alın. Tanılama sekmesi **veri eğriltme**, **zaman eğriltme**ve **yürütücü kullanım analizini**içerir.
    
+ Sırasıyla sekmeleri seçerek **veri eğriltme**, **zaman eğriltme**ve **yürütücü kullanım analizlerini** denetleyin.

    ![Tanılama sekmeleri](./media/apache-azure-spark-history-server/sparkui-diagnosis-tabs.png)

### <a name="data-skew"></a>Veri eğriltme
**Veri eğriltme** sekmesi ' ne tıklayın, ilgili asimetrik görevler belirtilen parametrelere göre görüntülenir. 

+ **Parametreleri belirtin** -ilk bölüm, veri eğriliğini algılamak için kullanılan parametreleri görüntüler. Yerleşik kural: Okunan görev verileri, ortalama görev verisinin 3 katından büyük ve okunan görev verileri 10 ' dan fazla. Çarpıtılmış görevler için kendi kuralınızı tanımlamak istiyorsanız, parametrelerinizi, **asimetrik aşamayı**ve **çarpıklık** bölümünü seçebilirsiniz.

+ **Çarpıtılmış aşama** -ikinci bölüm, yukarıda belirtilen ölçütlere uyan bir görev gösteren aşamaları görüntüler. Bir aşamada birden çok asimetrik görev varsa, asimetrik aşama tablosu yalnızca en çok eğilmiş görevi görüntüler (ör. veri eğriliği için en büyük veriler).

    ![Veri eğriltme section2](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section2.png)

+ **Grafiği eğriltme** : eğme aşaması tablosundaki bir satır seçildiğinde, eğriltme grafiği, verileri okuma ve yürütme zamanına göre daha fazla görev dağıtım ayrıntısı görüntüler. Asimetrik görevler kırmızı olarak işaretlenir ve normal görevler mavi olarak işaretlenir. Performans değerlendirmesi için grafik yalnızca en fazla 100 örnek görevi görüntüler. Görev ayrıntıları sağ alt panelde görüntülenir.

    ![Veri eğriltme section3](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section3.png)

### <a name="time-skew"></a>Zaman eğriltme
**Zaman eğriltme** sekmesi, görev yürütme zamanına göre asimetrik görevleri görüntüler. 

+ **Parametreleri belirtin** -ilk bölüm, zaman eğriliğini algılamak için kullanılan parametreleri görüntüler. Zaman eğriliğini algılamaya yönelik varsayılan ölçüt: görev yürütme süresi, ortalama yürütme süresinin 3 katından büyük ve görev yürütme süresi 30 saniyeden fazla. Parametreleri gereksinimlerinize göre değiştirebilirsiniz. **Asimetrik aşama** ve **eğme grafiği** , yukarıdaki **veri eğriltme** sekmesinde olduğu gibi ilgili aşamaları ve görev bilgilerini görüntüler.

+ **Zaman eğriliği**' ne tıklayın ve ardından filtrelenmiş sonuç, bölüm **belirtme**bölümünde ayarlanan parametrelere göre **asimetrik aşama** bölümünde görüntülenir. **Asimetrik aşama** bölümünde bir öğeye tıklayın, ardından karşılık gelen grafik section3 içinde drafted ve görev ayrıntıları sağ alt panelde görüntülenir.

    ![Zaman eğriltme section2](./media/apache-azure-spark-history-server/sparkui-diagnosis-timeskew-section2.png)

### <a name="executor-usage-analysis"></a>Yürütücü Kullanım Analizi
Yürütücü kullanım grafiği, Spark işi gerçek yürütücü ayırmayı ve çalışma durumunu görselleştirir.  

+ **Yürütücü Kullanım Analizi**' ne tıklayın, ardından yürütücü kullanımı ile ilgili dört tür eğrileri drafted, **yürütme**,**boştaki Yürüticileri**ve **en fazla yürütücü örnekleri** **de dahil olmak**üzere, Ayrılmış yürüticilere ilgili olarak, her "yürütücü eklenmiş" veya "yürütücü kaldırıldı" olayı ayrılan yürütmeleri arttırır veya azaltacaktır, daha fazla karşılaştırma için "Işler" sekmesinde "olay zaman çizelgesini" kontrol edebilirsiniz.

    ![Yürüticileri sekmesi](./media/apache-azure-spark-history-server/sparkui-diagnosis-executors.png)

+ Tüm taslaklarına karşılık gelen içeriği seçmek veya seçimini kaldırmak için renk simgesine tıklayın.

    ![Grafik Seç](./media/apache-azure-spark-history-server/sparkui-diagnosis-select-chart.png)


## <a name="faq"></a>SSS

### <a name="1-revert-to-community-version"></a>1. Topluluk sürümüne dön

Topluluk sürümüne dönmek için aşağıdaki adımları uygulayın:

1. Kümeyi ambarı 'nda açın. Sol panelde **Spark2** ' a tıklayın.
2. Yapılandırma **' ya** tıklayın.
3. Grup **özel spark2-varsayılanlar**' ı genişletin.
4. **Özellik Ekle**' ye tıklayın, **spark. UI. geliştirmesi Ekle. Enabled = False**, Kaydet.
5. Özelliği şimdi **yanlış** olarak ayarlanır.
6. Yapılandırmayı kaydetmek için **Kaydet** ' e tıklayın.

    ![Özellik kapatılır](./media/apache-azure-spark-history-server/sparkui-turn-off.png)

7. Sol bölmede **Spark2** ' ye tıklayın, **Özet** sekmesinde, **Spark2 geçmiş sunucusu**' na tıklayın.

    ![Sunucu1 'yi yeniden Başlat](./media/apache-azure-spark-history-server/sparkui-restart-1.png) 

8. **Spark2 geçmiş sunucusunu** **Yeniden Başlat** ' a tıklayarak geçmiş sunucusunu yeniden başlatın.

    ![Sunucu2 'yi yeniden Başlat](./media/apache-azure-spark-history-server/sparkui-restart-2.png)  

9. Spark geçmiş sunucusu Web Kullanıcı arabirimini yenileyerek, bu işlem topluluk sürümüne geri döndürülecek.

### <a name="2-upload-history-server-event"></a>2. Geçmiş sunucusu olayını karşıya yükle

Geçmiş sunucu hatasıyla karşılaşırsanız, olayı sağlamak için aşağıdaki adımları izleyin:
1. Geçmiş sunucusu Web Kullanıcı arabiriminde **İndir** ' e tıklayarak olayı indirin.

    ![indirme olayı](./media/apache-azure-spark-history-server/sparkui-download-event.png)

2. Veri/grafik sekmesinden **bize geri bildirim sağla** ' ya tıklayın.

    ![grafik geri bildirimi](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

3. Hatanın başlığını ve açıklamasını girip, ZIP dosyasını düzenleme alanına sürükleyin ve sonra **yeni sorun gönder**' e tıklayın.

    ![dosya sorunu](./media/apache-azure-spark-history-server/sparkui-file-issue.png)


### <a name="3-upgrade-jar-file-for-hotfix-scenario"></a>3. Düzeltme senaryosu için jar dosyasını yükselt

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
2. **Betik eylemleri**' ne ve ardından **Yeni Gönder**' e tıklayın. **Betik eylemini gönder** formunu tamamlayıp **Oluştur** düğmesine tıklayın.
    
    + **Betik türü**: **özel**' i seçin.
    + **Ad**: bir betik adı belirtin.
    + **Bash betiği URI 'si**: bash dosyasını özel kümeye yükleyin ve URL 'yi buraya kopyalayın. Alternatif olarak, sağlanmış URI 'yi kullanın.
    
   ```upgrade_spark_enhancement
    https://hdinsighttoolingstorage.blob.core.windows.net/shsscriptactions/upgrade_spark_enhancement.sh
   ```

   + **Baş** ve **çalışanı**denetleyin.
   + **Parametreler**: parametreleri ayarla Bash kullanımını izleyin.

     ![karşıya yükleme günlüğü veya düzeltmeyi yükseltme](./media/apache-azure-spark-history-server/sparkui-upload2.png)


## <a name="known-issues"></a>Bilinen sorunlar

1.  Şu anda yalnızca Spark 2,3 ve 2,4 kümesi için geçerlidir.

2.  RDD kullanan giriş/çıkış verileri, veri sekmesinde gösterilmez.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Apache Spark kümesi için kaynakları yönetme](apache-spark-resource-manager.md)
* [Apache Spark ayarlarını yapılandırma](apache-spark-settings.md)


## <a name="contact-us"></a>Bizimle iletişim kurun

Herhangi bir geri bildiriminiz varsa veya bu aracı kullanırken başka sorunlarla karşılaşırsanız ([hdivstool@microsoft.com](mailto:hdivstool@microsoft.com)) adresine bir e-posta gönderin.
