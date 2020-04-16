---
title: Öğretici - IntelliJ için Azure Araç Seti (Spark uygulaması)
description: Öğretici - Scala'da yazılmış Spark uygulamalarını geliştirmek ve bunları bir Apache Spark havuzuna göndermek için IntelliJ için Azure Araç Kiti'ni kullanın (önizleme).
services: synapse-analytics
author: v-jiche
ms.author: v-jiche
ms.reviewer: jrasnick, carlrab
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 04/15/2020
ms.openlocfilehash: 310dfec2bbd1bf6cb69f6293becead30487d6cbd
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422658"
---
# <a name="tutorial-use-azure-toolkit-for-intellij-to-create-apache-spark-applications-for-spark-pools-preview"></a>Öğretici: Spark havuzları için Apache Spark uygulamaları oluşturmak için IntelliJ için Azure Araç Kiti'ni kullanın (önizleme)

Bu öğretici, [Scala'da](https://www.scala-lang.org/)yazılan Apache Spark uygulamalarını geliştirmek ve bunları doğrudan IntelliJ tümleşik geliştirme ortamından (IDE) doğrudan bir Spark havuzuna (önizleme) göndermek için IntelliJ eklentisi için Azure Araç Kiti'nin nasıl kullanılacağını gösterir. Eklentiyi birkaç şekilde kullanabilirsiniz:

- Spark havuzunda Bir Scala Spark uygulaması geliştirin ve gönderin.
- Kıvılcım havuzları kaynaklarına erişin.
- Yerel olarak bir Scala Spark uygulaması geliştirin ve çalıştırın.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
>
> - IntelliJ eklentisi için Azure Araç Kiti'ni kullanma
> - Apache Spark uygulamalarını geliştirin
> - Spark havuzlarına başvuru gönderme

## <a name="prerequisites"></a>Ön koşullar

- [IntelliJ IDEA Topluluk Sürümü](https://www.jetbrains.com/idea/download/download-thanks.html?platform=windows&code=IIC).
- Azure araç seti eklentisi 3.27.0-2019.2 – [IntelliJ Plugin deposundan](/java/azure/intellij/azure-toolkit-for-intellij-installation?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) yükle
- [JDK (Sürüm 1.8)](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
- Scala Plugin - [IntelliJ Plugin deposundan yükleyin.](/hdinsight/spark/apache-spark-intellij-tool-plugin#install-scala-plugin-for-intellij-idea.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- Bu ön koşul yalnızca Windows kullanıcıları içindir.

  Bir Windows bilgisayarda yerel Spark Scala uygulamasını çalıştırırken, [SPARK-2356'da](https://issues.apache.org/jira/browse/SPARK-2356)açıklandığı gibi bir özel durum elde edebilirsiniz. WinUtils.exe Windows'da eksik olduğundan özel durum oluşur.
  Bu hatayı gidermek için, [yürütülebilir WinUtils'i](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) **C:\WinUtils\bin**gibi bir konuma indirin. Sonra, ortam değişkenini **HADOOP_HOME**ekleyin ve değişkenin değerini **C:\WinUtils'e**ayarlayın.

## <a name="create-a-spark-scala-application-for-a-spark-pool"></a>Kıvılcım havuzu için Kıvılcım Scala uygulaması oluşturma

1. IntelliJ IDEA'yı başlatın ve **Yeni Proje** penceresini açmak için Yeni **Proje Oluştur'u** seçin.
2. Sol bölmeden **Apache Spark/HDInsight'ı** seçin.
3. Ana pencereden **Örneklerle Kıvılcım Projesi'ni(Scala)** seçin.
4. Yapı **aracı** açılır listesinden aşağıdaki türlerden birini seçin:

   - Scala proje oluşturma sihirbazı desteği için **Maven.**
   - Bağımlılıkları yönetmek ve Scala projesi için bina için **SBT.**

    ![IntelliJ IDEA Yeni Proje iletişim kutusu](./media/intellij-tool-synapse/create-synapse-application01.png)

5. **İleri**’yi seçin.
6. Yeni **Proje** penceresinde aşağıdaki bilgileri sağlayın:

    | Özellik | Açıklama |
    | ----- | ----- |
    |Proje adı| Bir ad girin. Bu öğreticide `myApp` kullanılır.|
    |Proje&nbsp;konumu| Projenizi kaydetmek için aranan konumu girin.|
    |Proje SDK| IDEA'yı ilk kullanımınızda boş olabilir. **Yeni...'yi** seçin ve JDK'nıza gidin.|
    |Kıvılcım Sürümü|Oluşturma sihirbazı Spark SDK ve Scala SDK için uygun sürümü entegre eder. Synapse sadece **Spark 2.4.0**destekler.|

    ![Apache Kıvılcım SDK'nın seçilmesi](./media/intellij-tool-synapse/create-synapse-application02.png)

7. **Bitiş'i**seçin. Projenin kullanıma sunulması birkaç dakika sürebilir.
8. Kıvılcım projesi sizin için otomatik olarak bir yapı oluşturur. Yapıyı görüntülemek için aşağıdaki işletimi yapın:

   a. Menü çubuğundan **Dosya** > **Proje Yapısı'na gidin...**.

   b. Proje **Yapısı** penceresinden **Yapılar'ı**seçin.

   c. Yapıyı görüntüledikten sonra **İptal'i** seçin.

    ![İletişim kutusundaki artifakı bilgileri](./media/intellij-tool-synapse/default-artifact-dialog.png)

9. **myApp** > **src** > **ana** > **scala**> **örnek**>  **LogQuery gelen** **LogQuery**bulun . Bu öğretici çalıştırmak için **LogQuery** kullanır.

   ![Project'ten Scala sınıfı oluşturma komutları](./media/intellij-tool-synapse/select-run-file.png)

## <a name="connect-to-your-spark-pools"></a>Kıvılcım havuzlarınıza bağlanın

Spark havuzlarınıza bağlanmak için Azure aboneliğinde oturum açın.

### <a name="sign-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

1. Menü çubuğundan, Windows > **Azure Gezgini****Aracını** **Görüntüle'ye** > gidin.

   ![IntelliJ IDEA Azure Explorer'ı gösteriyor](./media/intellij-tool-synapse/show-azure-explorer1.png)

2. Azure Gezgini'nden **Azure** düğümüne sağ tıklayın ve ardından **Oturum Aç'ı**seçin.

   ![IntelliJ IDEA explorer sağ tıklayın azure](./media/intellij-tool-synapse/explorer-rightclick-azure.png)

3. Azure **Oturum Aç** iletişim kutusunda **Aygıt Girişi'ni**seçin ve ardından **Oturum Aç'ı**seçin.

    ![IntelliJ IDEA azure oturum açma](./media/intellij-tool-synapse/intellij-view-explorer2.png)

4. Azure **Aygıt Giriş** iletişim kutusunda, **Kopyala&Aç'ı**tıklatın.

   ![IntelliJ IDEA azure cihaz girişi](./media/intellij-tool-synapse/intellij-view-explorer5.png)

5. Tarayıcı arabiriminde, kodu yapıştırın ve sonra **İleri'yi**tıklatın.

   ![Microsoft HDI için kod iletişim kutusu girer](./media/intellij-tool-synapse/intellij-view-explorer6.png)

6. Azure kimlik bilgilerinizi girin ve ardından tarayıcıyı kapatın.

   ![Microsoft HDI için e-posta iletişim kutusu girer](./media/intellij-tool-synapse/intellij-view-explorer7.png)

7. Oturum açtıktan sonra, **Abonelikleri Seç** iletişim kutusu kimlik bilgileriyle ilişkili tüm Azure aboneliklerini listeler. Aboneliğinizi seçin ve ardından **Seç'i**tıklatın.

    ![Abonelik Seç iletişim kutusu](./media/intellij-tool-synapse/Select-Subscriptions.png)

8. **Azure Gezgini'nden,** aboneliklerinizde bulunan Çalışma Alanlarını görüntülemek için **Synapse'deki Apache Spark'ı** genişletin.

    ![IntelliJ IDEA Azure Explorer ana görünümü](./media/intellij-tool-synapse/azure-explorer-workspace.png)

9. Kıvılcım havuzlarını görüntülemek için çalışma alanını daha da genişletebilirsiniz.

    ![Azure Explorer depolama hesapları](./media/intellij-tool-synapse/azure-explorer-pool.png)

## <a name="remote-run-a-spark-scala-application-on-a-spark-pool"></a>Bir Kıvılcım havuzunda Uzaktan Çalıştırma Bir Kıvılcım Scala uygulaması

Bir Scala uygulaması oluşturduktan sonra, uzaktan çalıştırabilirsiniz.

1. Simgeyi tıklatarak **Çalıştır/Hata Ayıklama Yapılandırmaları** penceresini açın.

    ![HDInsight komutuna Kıvılcım Başvurusu Gönder](./media/intellij-tool-synapse/open-configuration-window.png)

2. **Çalıştır/Hata Ayıklama Yapılandırmaları** iletişim penceresinde, **+** **Synapse'de Apache Spark'ı**tıklatın.

    ![HDInsight komutuna Kıvılcım Başvurusu Gönder](./media/intellij-tool-synapse/create-synapse-configuration02.png)

3. **Çalıştır/Hata Ayıklama Yapılandırmaları** penceresinde aşağıdaki değerleri sağlayın ve ardından **Tamam'ı**seçin:

    |Özellik |Değer |
    |----|----|
    |Kıvılcım havuzları|Başvurunuzu çalıştırmak istediğiniz Kıvılcım havuzlarını seçin.|
    |Göndermek için bir Artifakı Seçin|Varsayılan ayarı bırakın.|
    |Ana sınıf adı|Varsayılan değer, seçili dosyadaki ana sınıftır. Elipsleri seçerek ve başka bir **...** sınıf seçerek sınıfı değiştirebilirsiniz.|
    |İş yapılandırmaları|Varsayılan anahtarı ve değerleri değiştirebilirsiniz. Daha fazla bilgi için, [Apache Livy REST API](https://livy.incubator.apache.org./docs/latest/rest-api.html)bakın.|
    |Komut satırı bağımsız değişkenleri|Gerekirse ana sınıf için alana ayrılmış bağımsız değişkenler girebilirsiniz.|
    |Başvurulan Kavanozlar ve Başvurulan Dosyalar|Başvurulan Kavanozlar ve varsa dosyaların yollarını girebilirsiniz. Ayrıca, şu anda yalnızca ADLS Gen 2 kümesini destekleyen Azure sanal dosya sistemindeki dosyalara da göz atabilirsiniz. Daha fazla bilgi için: [Apache Spark Yapılandırma](https://spark.apache.org/docs/latest/configuration.html#runtime-environment) ve [nasıl küme kaynakları yüklemek için](../../storage/blobs/storage-quickstart-blobs-storage-explorer.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).|
    |İş Yükleme Depolama|Ek seçenekleri ortaya çıkarmak için genişletin.|
    |Depolama Türü|Açılan listeden **yüklemek için Azure Blob'u kullan'ı** seçin.|
    |Depolama Hesabı|Depolama hesabınızı girin.|
    |Depolama Anahtarı|Depolama anahtarınızı girin.|
    |Depolama Konteyneri|**Depolama Hesabı** ve **Depolama Anahtarı** girildikten sonra açılan listeden depolama kabınızı seçin.|

    ![Kıvılcım Gönderme iletişim kutusu](./media/intellij-tool-synapse/create-synapse-configuration03.png)

4. Projenizi seçili Spark havuzuna göndermek için **SparkJobRun** simgesini tıklatın. **Küme sekmesinde Uzak Spark İşi,** iş yürütme ilerlemesini en altta görüntüler. Kırmızı butonu tıklatarak uygulamayı durdurabilirsiniz.

    ![Apache Kıvılcım Gönderme penceresi](./media/intellij-tool-synapse/remotely-run-synapse.png)

    ![Kıvılcım Gönderme iletişim kutusu](./media/intellij-tool-synapse/remotely-run-result.png)

## <a name="local-rundebug-apache-spark-applications"></a>Yerel Çalıştırma/Hata Ayıklama Apache Spark uygulamaları

Apache Spark işiniz için yerel çalıştırmanızı ve yerel hata ayıklamanızı ayarlamak için aşağıdaki yönergeleri takip edebilirsiniz.

### <a name="scenario-1-do-local-run"></a>Senaryo 1: Yerel çalıştırma yı

1. **Çalıştır/Hata Ayıklama Yapılandırmaları** iletişim kutusunu açın,**+** artı işaretini seçin ( ). Ardından **Synapse seçeneğinde Apache Spark'ı** seçin. **Ad**, **Ana sınıf adı** için bilgileri kaydedin.

    ![Intellij Çalıştır hata ayıklama yapılandırmaları yerel çalıştırma](./media/intellij-tool-synapse/local-run-synapse.png)

    - Çevre değişkenleri ve WinUtils.exe Konum yalnızca windows kullanıcıları içindir.
    - Ortam değişkenleri: Sistem ortamı değişkeni, daha önce ayarladıysanız ve el ile eklemenize gerek yoksa otomatik olarak algılanabilir.
    - [WinUtils.exe Konum](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe): Sağdaki klasör simgesine tıklayarak WinUtils konumunu belirtebilirsiniz.

2. Ardından yerel oynat düğmesini tıklatın.

    ![Intellij Çalıştır hata ayıklama yapılandırmaları yerel çalıştırma](./media/intellij-tool-synapse/local-run-synapse01.png)

3. Yerel çalıştırma tamamlandıktan sonra, komut dosyası çıktı içeriyorsa, çıktı dosyasını **veri** > **__varsayılanından__** denetleyebilirsiniz.

    ![Intellij Projesi yerel çalışma sonucu](./media/intellij-tool-synapse/spark-local-run-result.png)

### <a name="scenario-2-do-local-debugging"></a>Senaryo 2: Yerel hata ayıklama yapın

1. **LogQuery** komut dosyasını açın, kesme noktaları ayarlayın.
2. Yerel hata ayıklama yapmak için **Yerel hata ayıklama** simgesini tıklatın.

    ![Intellij Projesi yerel çalışma sonucu](./media/intellij-tool-synapse/local-debug-synapse.png)

## <a name="access-and-manage-synapse-workspace"></a>Synapse Çalışma Alanına erişin ve yönetin

IntelliJ için Azure Araç Seti'nde Azure Explorer'da farklı işlemler gerçekleştirebilirsiniz. Menü çubuğundan, Windows > **Azure Gezgini****Aracını** **Görüntüle'ye** > gidin.

### <a name="launch-workspace"></a>Çalışma alanını başlatma

1. Azure Gezgini'nden **Synapse'deki Apache Spark'a**gidin ve genişletin.

    ![IntelliJ IDEA Azure Explorer ana görünümü](./media/intellij-tool-synapse/azure-explorer-workspace.png)

2. Bir çalışma alanına sağ tıklayın, ardından **Başlat çalışma alanını**seçin, web sitesi açılacaktır.

    ![Kıvılcım İş Görünümü Uygulama detayları](./media/intellij-tool-synapse/launch-workspace-synapse.png)

    ![Kıvılcım İş Görünümü Uygulama detayları](./media/intellij-tool-synapse/launch-workspace-result.png)

## <a name="spark-console"></a>Kıvılcım konsolu

Spark Local Console(Scala) veya Spark Livy Interactive Session Console(Scala) çalıştırabilirsiniz.

### <a name="spark-local-console-scala"></a>Yerel konsolu kıvılcımla (Scala)

WINUTILS'i memnun ettiğinizden emin olun. EXE ön koşul.

1. Menü çubuğundan, Düzenleme Yapılandırmalarını **Çalıştır'a** > **gidin...**.
2. Sol **bölmede, Çalıştır/Hata Ayıklama Yapılandırmaları** penceresinden >  **Synapse'deki Apache Spark'a****gidin [Synapse'de Kıvılcım] myApp.'ye**gidin.
3. Ana pencereden Yerel **Çalıştır** sekmesini seçin.
4. Aşağıdaki değerleri sağlayın ve sonra **Tamam'ı**seçin:

    |Özellik |Değer |
    |----|----|
    |Ortam değişkenleri|HADOOP_HOME değerinin doğru olduğundan emin olun.|
    |WINUTILS.exe konumu|Yolun doğru olduğundan emin olun.|

    ![Yerel Konsol Seti Yapılandırması](./media/intellij-tool-synapse/local-console-synapse01.png)

5. Proje itibaren, **myApp** > **src** > **ana** > **scala** > **myApp**gidin.
6. Menü çubuğundan **Tools** > **Spark konsoluna** > gidin**Yerel Konsol çalıştırın.**
7. Ardından, bağımlılıkları otomatik olarak düzeltmek isteyip istemediğinizi sormak için iki iletişim kutusu görüntülenebilir. Eğer öyleyse, **Otomatik Düzeltme'yi**seçin.

    ![IntelliJ IDEA Spark Otomatik Düzeltme iletişim kutusu1](./media/intellij-tool-synapse/intellij-console-autofix1.png)

    ![IntelliJ IDEA Spark Otomatik Düzeltme iletişim kutusu2](./media/intellij-tool-synapse/intellij-console-autofix2.png)

8. Konsol aşağıdaki resme benzer görünmelidir. Konsol penceresi türünde `sc.appName`ve ardından ctrl+Enter tuşuna basın. Sonuç gösterilir. Kırmızı düğmeye tıklayarak yerel konsolu durdurabilirsiniz.

    ![IntelliJ IDEA yerel konsol sonucu](./media/intellij-tool-synapse/local-console-result.png)

### <a name="spark-livy-interactive-session-console-scala"></a>Spark Livy interaktif oturum konsolu (Scala)

Sadece IntelliJ 2018.2 ve 2018.3'te desteklenir.

1. Menü çubuğundan, Düzenleme Yapılandırmalarını **Çalıştır'a** > **gidin...**.

2. Sol **bölmede, Çalıştır/Hata Ayıklama Yapılandırmaları** penceresinden, sinaps > **[Sinapstaki Kıvılcım] myApp'taki** **Apache Spark'a**gidin.

3. Ana pencereden Küme **sekmesinde Uzaktan Çalıştır sekmesini** seçin.

4. Aşağıdaki değerleri sağlayın ve sonra **Tamam'ı**seçin:

    |Özellik |Değer |
    |----|----|
    |Kıvılcım havuzları|Başvurunuzu çalıştırmak istediğiniz Kıvılcım havuzlarını seçin.|
    ||

    ![İnteraktif Konsol Seti Yapılandırması](./media/intellij-tool-synapse/interactive-console-configuration.png)

5. Proje itibaren, **myApp** > **src** > **ana** > **scala** > **myApp**gidin.

6. Menü çubuğundan **Tools** > **Spark konsoluna** > gidin**Çalıştır Spark Livy Etkileşimli Oturum Konsolu(Scala)**.
7. Konsol aşağıdaki resme benzer görünmelidir. Konsol penceresi türünde `sc.appName`ve ardından ctrl+Enter tuşuna basın. Sonuç gösterilir. Kırmızı düğmeye tıklayarak yerel konsolu durdurabilirsiniz.

    ![IntelliJ IDEA İnteraktif Konsol Sonucu](./media/intellij-tool-synapse/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>Seçimi Spark konsoluna gönder

Yerel konsola veya Livy Interactive Session Console'a (Scala) bazı kodlar göndererek komut dosyası sonucunu öngörmeniz uygundur. Scala dosyasında bazı kodları vurgulayabilirsiniz, ardından **Spark konsoluna Seçim Gönder'i**sağ tıklatın. Seçili kod konsola gönderilir ve tamamlanır. Sonuç konsoldaki koddan sonra görüntülenir. Konsol varsa hataları denetler.

   ![Seçim'i Spark konsoluna gönder](./media/intellij-tool-synapse/send-selection-to-console.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Synapse Analytics](../overview-what-is.md)
- [Azure Synapse Analytics çalışma alanı için yeni bir Apache Spark havuzu oluşturun](../../synapse-analytics/quickstart-create-apache-spark-pool.md)
