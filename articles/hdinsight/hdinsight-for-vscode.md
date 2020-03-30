---
title: Visual Studio Code için Azure HDInsight
description: Sorgular ve komut dosyaları oluşturmak ve göndermek için Visual Studio Code için Spark & Hive Araçları'nı (Azure HDInsight) nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/11/2019
ms.openlocfilehash: 9a81868d678b4c0277e904e879c73185a378bf70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435676"
---
# <a name="use-spark--hive-tools-for-visual-studio-code"></a>Visual Studio Code için Kıvılcım & Kovan Araçlarını Kullanma

Apache Hive toplu iş, etkileşimli Hive sorguları ve Apache Spark için PySpark komut dosyaları oluşturmak ve göndermek için Görsel Stüdyo Kodu için Spark & Hive Tools'u nasıl kullanacağınızı öğrenin. Önce Visual Studio Code'a Spark & Hive Araçları'nı nasıl yükleyebileceğimizi açıklayacağız, sonra da Spark & Hive Tools'a nasıl iş gönderebileceğimizi inceeceğiz.  

Spark & Hive Araçları, Windows, Linux ve macOS'u içeren Visual Studio Code tarafından desteklenen platformlara yüklenebilir. Farklı platformlar için aşağıdaki ön koşullara dikkat edin.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için aşağıdaki öğeler gereklidir:

- Bir Azure HDInsight kümesi. Küme oluşturmak için [bkz.](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md) Veya Apache Livy bitiş noktasını destekleyen bir Kıvılcım ve Hive kümesi kullanın.
- [Görsel Stüdyo Kodu](https://code.visualstudio.com/).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono sadece Linux ve macOS için gereklidir.
- [Visual Studio Code için bir PySpark interaktif ortam](set-up-pyspark-interactive-environment.md).
- Yerel bir dizin. Bu makalede **C:\HD\HDexample**kullanır.

## <a name="install-spark--hive-tools"></a>Spark & Kovan Araçları Yükleyin

Ön koşulları yerine getirerek, Görsel Stüdyo Kodu için Spark & Hive Tools'u aşağıdaki adımları izleyerek yükleyebilirsiniz:

1. Visual Studio Code'u açın.

2. Menü çubuğundan**Uzantıları** **Görüntüle'ye** > gidin.

3. Arama kutusuna, **Spark & Hive**girin.

4. Arama sonuçlarından **Kovun araçları & Spark'ı** seçin ve ardından **Yükle'yi**seçin:

   ![Görsel Stüdyo Kodu Python yüklemek için Spark & Hive](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

5. Gerektiğinde **Yeniden Yükle'yi** seçin.

## <a name="open-a-work-folder"></a>Çalışma klasörü açma

Bir iş klasörünü açmak ve Visual Studio Code'da bir dosya oluşturmak için aşağıdaki adımları izleyin:

1. Menü çubuğundan **Dosya** > **Aç Klasörü'ne gidin...**  >  **C:\HD\HDexample**ve ardından **Klasör** seç düğmesini seçin. Klasör soldaki **Explorer** görünümünde görünür.

2. **Explorer** görünümünde, **HDexample** klasörünü seçin ve ardından iş klasörünyanındaki **Yeni Dosya** simgesini seçin:

   ![visual studio kodu yeni dosya simgesi](./media/hdinsight-for-vscode/visual-studio-code-new-file.png)

3. `.hql` (Hive sorguları) veya (Spark komut `.py` dosyası) dosya uzantısını kullanarak yeni dosyayı adlandırın. Bu **örnek, HelloWorld.hql**kullanır.

## <a name="set-the-azure-environment"></a>Azure ortamını ayarlama

Ulusal bir bulut kullanıcısı için önce Azure ortamını ayarlamak için aşağıdaki adımları izleyin ve ardından **Azure'da** oturum açtırmak için Azure: Oturum Aç komutunu kullanın:

1. **Dosya** > **Tercihleri** > **Ayarlarına**gidin.
2. Aşağıdaki dizeüzerinde arama: **Azure: Bulut**.
3. Listeden ulusal bulutu seçin:

   ![Varsayılan giriş girişi yapılandırması ayarlama](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="connect-to-an-azure-account"></a>Azure hesabına bağlanma

Visual Studio Code'tan kümelerinize komut dosyası göndermeden önce, Azure hesabınıza bağlanmanız veya bir kümeye bağlamanız gerekir (Apache Ambari kullanıcı adı ve parola kimlik bilgilerini kullanarak veya etki alanına bağlı bir hesap). Azure'a bağlanmak için aşağıdaki adımları izleyin:

1. Menü çubuğundan > **Komut Paletini Görüntüle'ye gidin...** ve **Azure girin: Oturum Aç**: **View**

    ![Görsel Stüdyo Kodu giriş için Spark & Hive Araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

2. Azure'da oturum açma yönergelerini izleyin. Bağlandıktan sonra Azure hesap adınız Visual Studio Code penceresinin altındaki durum çubuğunda gösterilir.  

## <a name="link-a-cluster"></a>Bir kümeye bağlantı

### <a name="link-azure-hdinsight"></a>Bağlantı: Azure HDInsight

[Apache Ambari](https://ambari.apache.org/)tarafından yönetilen bir kullanıcı adı kullanarak normal bir kümeye bağlayabilirsiniz veya bir etki alanı kullanıcı adı `user1@contoso.com`(örneğin: ).

1. Menü çubuğundan Komut > **Paletini Görüntüle'ye gidin...** ve **Spark / Hive girin: Bir Kümeye bağla.** **View**

   ![Komut Paletbağlantı küme komutu](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Bağlantılı küme türünü seçin **Azure HDInsight**.

3. HDInsight küme URL'sini girin.

4. Ambari kullanıcı adınızı girin; varsayılan **yöneticidir.**

5. Ambari şifrenizi girin.

6. Küme türünü seçin.

7. Kümenin görüntü adını ayarlayın (isteğe bağlı).

8. Doğrulama için **ÇıKTı** görünümünü gözden geçirin.

   > [!NOTE]  
   > Küme hem Azure aboneliğinde oturum açmış hem de bir kümeye bağlıysa, bağlantılı kullanıcı adı ve parola kullanılır.  

### <a name="link-generic-livy-endpoint"></a>Bağlantı: Genel Livy bitiş noktası

1. Menü çubuğundan Komut > **Paletini Görüntüle'ye gidin...** ve **Spark / Hive girin: Bir Kümeye bağla.** **View**

2. Bağlantılı küme türünü seçin **Genel Livy Bitiş Noktası.**

3. Genel Livy bitiş noktasını girin. Örneğin: http\://10.172.41.42:18080.

4. **Temel** veya **Yok**yetkilendirme türünü seçin.  **Temel'i**seçerseniz:  
    &emsp;A. Ambari kullanıcı adınızı girin; varsayılan **yöneticidir.**  
    &emsp;b. Ambari şifrenizi girin.

5. Doğrulama için **ÇıKTı** görünümünü gözden geçirin.

## <a name="list-clusters"></a>Liste kümeleri

1. Menü çubuğundan Komut**Paletini** **Görüntüle...** > ve **Spark / Hive: Liste Kümesi'ni**girin.

2. İstediğiniz aboneliği seçin.

3. **OUTPUT** görünümünü gözden geçirin. Bu görünüm, bağlı kümenizi (veya kümelerinizi) ve Azure aboneliğiniz altındaki tüm kümeleri gösterir:

    ![Varsayılan küme yapılandırması ayarlama](./media/hdinsight-for-vscode/list-cluster-result1.png)

## <a name="set-the-default-cluster"></a>Varsayılan kümeyi ayarlama

1. [Daha önce](#open-a-work-folder)tartışılan **HDexample** klasörünü , kapalıysa yeniden açın.  

2. [Daha önce](#open-a-work-folder)oluşturulan **HelloWorld.hql** dosyasını seçin. Senaryo düzenleyicisinde açılır.

3. Komut dosyası düzenleyicisini sağ tıklatın ve ardından **Spark / Hive: Varsayılan Küme'yi ayarlayın'ı**seçin.  

4. Azure hesabınıza [bağlanın](#connect-to-an-azure-account) veya henüz yapmadıysanız bir kümeye bağlayın.

5. Geçerli komut dosyası dosyası için varsayılan küme olarak bir küme seçin. Araçlar otomatik olarak **güncellenir. VSCode\settings.json** yapılandırma dosyası:

   ![Varsayılan küme yapılandırmayı ayarlama](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="submit-interactive-hive-queries-and-hive-batch-scripts"></a>Etkileşimli Hive sorguları ve Hive toplu komut dosyaları gönderme

Visual Studio Code için Spark & Hive Tools ile kümelerinize etkileşimli Kovan sorguları ve Hive toplu komut dosyaları gönderebilirsiniz.

1. [Daha önce](#open-a-work-folder)tartışılan **HDexample** klasörünü , kapalıysa yeniden açın.  

2. [Daha önce](#open-a-work-folder)oluşturulan **HelloWorld.hql** dosyasını seçin. Senaryo düzenleyicisinde açılır.

3. Aşağıdaki kodu Kovan dosyanıza kopyalayıp yapıştırın ve sonra kaydedin:

    ```hiveql
    SELECT * FROM hivesampletable;
    ```

4. Azure hesabınıza [bağlanın](#connect-to-an-azure-account) veya henüz yapmadıysanız bir kümeye bağlayın.

5. Komut dosyası düzenleyicisine sağ tıklayın ve sorguyu göndermek için **Hive: Interactive'i** seçin veya Ctrl+Alt+I klavye kısayolu'nu kullanın.  **Hive'** ı seçin: Komut dosyasını göndermek için toplu iş veya Ctrl+Alt+H klavye kısayolu kullanın.  

6. Varsayılan küme belirtmediyseniz, bir küme seçin. Araçlar ayrıca bağlam menüsünü kullanarak tüm komut dosyası dosyası yerine bir kod bloğu göndermenize de izin verebilirsiniz. Birkaç dakika sonra sorgu sonuçları yeni bir sekmede görünür:

   ![İnteraktif Apache Hive sorgu sonucu](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **SONUÇLAR** paneli: CSV, JSON veya Excel dosyası olarak tüm sonucu yerel bir yola kaydedebilir veya yalnızca birden çok satır seçebilirsiniz.

    - **MESAJLAR** paneli: Bir **Satır** numarası seçtiğinizde, çalışan komut dosyasının ilk satırına atlar.

## <a name="submit-interactive-pyspark-queries"></a>Etkileşimli PySpark sorguları gönderme

Etkileşimli PySpark sorguları göndermek için aşağıdaki adımları izleyin:

1. [Daha önce](#open-a-work-folder)tartışılan **HDexample** klasörünü , kapalıysa yeniden açın.  

2. [Önceki](#open-a-work-folder) adımları izleyerek yeni bir **HelloWorld.py** dosyası oluşturun.

3. Aşağıdaki kodu komut dosyası dosyasına kopyalayıp yapıştırın:

   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```

4. Azure hesabınıza [bağlanın](#connect-to-an-azure-account) veya henüz yapmadıysanız bir kümeye bağlayın.

5. Tüm kodu seçin, komut dosyası düzenleyicisine sağ tıklayın ve sorguyu göndermek için **Spark: PySpark Interactive'i** seçin. Veya Ctrl+Alt+I kısayolu kullanın.

   ![pyspark interaktif bağlam menüsü](./media/hdinsight-for-vscode/pyspark-interactive-right-click.png)

6. Varsayılan küme belirtmediyseniz, kümeyi seçin. Birkaç dakika sonra **Python Interactive** sonuçları yeni bir sekmede görünür. Araçlar ayrıca bağlam menüsünü kullanarak tüm komut dosyası dosyası yerine bir kod bloğu göndermenize de izin verebilirsiniz:

   ![pyspark interaktif python interaktif pencere](./media/hdinsight-for-vscode/pyspark-interactive-python-interactive-window.png)

7. **%%bilgi**girin ve ardından iş bilgilerini görüntülemek için Shift+Enter tuşuna basın (isteğe bağlı):

   ![pyspark interaktif görünüm iş bilgileri](./media/hdinsight-for-vscode/pyspark-interactive-view-job-information.png)

8. Araç ayrıca **Spark SQL** sorgusunu da destekler:

   ![Pyspark İnteraktif görünüm sonucu](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png)

   Sorguları çalıştırırken, gönderme durumu alt durum çubuğunun solunda görünür. Durum **PySpark Kernel (meşgul)** olduğunda diğer sorguları göndermeyin.  

   > [!NOTE]
   >
   > **Python Extension Enabled** ayarları temizlendiğinde (varsayılan olarak seçilir), gönderilen pyspark etkileşim sonuçları eski pencereyi kullanır:
   >
   > ![pyspark interaktif python uzantısı devre dışı](./media/hdinsight-for-vscode/pyspark-interactive-python-extension-disabled.png)

## <a name="submit-pyspark-batch-job"></a>PySpark toplu iş gönder

1. [Daha önce](#open-a-work-folder)tartıştığınız **HDexample** klasörünü , kapalıysa yeniden açın.  

2. [Önceki](#open-a-work-folder) adımları izleyerek yeni bir **BatchFile.py** dosyası oluşturun.

3. Aşağıdaki kodu komut dosyası dosyasına kopyalayıp yapıştırın:

    ```python
    from __future__ import print_function
    import sys
    from operator import add
    from pyspark.sql import SparkSession
    if __name__ == "__main__":
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
    
        lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' '))\
                    .map(lambda x: (x, 1))\
                    .reduceByKey(add)
        output = counts.collect()
        for (word, count) in output:
            print("%s: %i" % (word, count))
        spark.stop()
    ```

4. Azure hesabınıza [bağlanın](#connect-to-an-azure-account) veya henüz yapmadıysanız bir kümeye bağlayın.

5. Komut dosyası düzenleyicisini sağ tıklatın ve ardından **Kıvılcım: PySpark Toplu seçeneğini**belirleyin veya Ctrl+Alt+H klavye kısayolu kullanın.

6. PySpark işinizi göndermek için bir küme seçin:

   ![Python iş sonucu çıktısını gönder](./media/hdinsight-for-vscode/submit-pythonjob-result.png)

Python işi gönderdikten sonra, gönderme günlükleri Visual Studio Code'da **OUTPUT** penceresinde görünür. Kıvılcım UI URL'si ve İplik UI URL'si de gösterilir. İş durumunu izlemek için URL'yi bir web tarayıcısında açabilirsiniz.

## <a name="apache-livy-configuration"></a>Apache Livy yapılandırma

[Apache Livy](https://livy.incubator.apache.org/) yapılandırması desteklenir. Bunu ' da **yapılandırabilirsiniz. Çalışma alanı klasöründe VSCode\settings.json** dosyası. Şu anda, Livy yapılandırma yalnızca Python komut dosyalarını destekler. Daha fazla bilgi için [Livy README'a](https://github.com/cloudera/livy/blob/master/README.rst )bakın.

<a id="triggerlivyconf"></a>**Livy yapılandırması nasıl tetikler?**

Yöntem 1  
1. Menü çubuğundan **Dosya** > **Tercihleri** > **Ayarları'na**gidin.
2. Arama **ayarları kutusuna** **HDInsight İş Gönderimi: Livy Conf'ı**girin.  
3. İlgili arama sonucu için **settings.json'da Edit'i** seçin.

Yöntem 2 Dosya gönderin ve .vscode klasörünotomatik olarak iş klasörüne eklenmiştir. **.vscode\settings.json'u**seçerek Livy yapılandırmasını görebilirsiniz.

+ Proje ayarları:

    ![HDInsight Apache Livy yapılandırma](./media/hdinsight-for-vscode/hdi-apache-livy-config.png)

    >[!NOTE]
    >**DriverMemory** ve **executorMemory** ayarları için değeri ve birimi ayarlayın. Örneğin: 1g veya 1024m.

+ Desteklenen Livy yapılandırmaları:

    **POST /toplu iş** İstek gövdesi

    | ad | açıklama | type |
    | :- | :- | :- |
    |  dosyası | Yürütülecek uygulamayı içeren dosya | Yol (gerekli) |
    | proxyKullanıcı | İşi çalıştırırken kimliğe bürünecek kullanıcı | Dize |
    | Classname | Uygulama Java / Kıvılcım ana sınıf | Dize |
    | args | Uygulama için komut satırı bağımsız değişkenleri | Dizeleri listesi |
    | Kavanoz | Bu oturumda kullanılacak kavanozlar | Dizeleri listesi | 
    | pyFiles | Bu oturumda kullanılacak Python dosyaları | Dizeleri listesi |
    | files | Bu oturumda kullanılacak dosyalar | Dizeleri listesi |
    | sürücüBellek | Sürücü işlemi için kullanılacak bellek miktarı | Dize |
    | driverCores | Sürücü işlemi için kullanılacak çekirdek sayısı | int |
    | uygulayıcıBellek | Yürütme işlemi başına kullanılacak bellek miktarı | Dize |
    | executorCores | Her uygulayıcı için kullanılacak çekirdek sayısı | int |
    | numExecutors | Bu oturum için başlatılan uygulayıcıların sayısı | int |
    | Arşiv | Bu oturumda kullanılacak arşivler | Dizeleri listesi |
    | kuyruk | Gönderilecek İplik kuyruğunun adı| Dize |
    | ad | Bu oturumun adı | Dize |
    | conf | Yapılandırma özelliklerini kıvılcımla | Tuş haritası=val |

    Yanıt gövdesi Oluşturulan Toplu Iş nesnesi.

    | ad | açıklama | type |
    | :- | :- | :- |
    | id | Oturum kimliği | int |
    | appId | Bu oturumun başvuru kimliği | Dize |
    | Appinfo | Detaylı başvuru bilgileri | Tuş haritası=val |
    | Günlük | Günlük satırları | Dizeleri listesi |
    | durum |Toplu durum | Dize |

    > [!NOTE]
    > Atanan Livy config komut dosyası gönderdiğinde çıkış bölmesinde görüntülenir.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Explorer'dan Azure HDInsight ile tümleştirme

Kümelerinizde Kovan Tablosu'nu doğrudan **Azure HDInsight** gezgini aracılığıyla önizleyebilirsiniz:

1. Henüz yapmadıysanız Azure hesabınıza [bağlanın.](#connect-to-an-azure-account)

2. En sol sütundan **Azure** simgesini seçin.

3. Sol bölmeden **AZURE'u genişletin: HDINSIGHT**. Kullanılabilir abonelikler ve kümeler listelenir.

4. Hive meta veri veritabanını ve tablo şemasını görüntülemek için kümeyi genişletin.

5. Kovan tablosuna sağ tıklayın. Örneğin: **kovan örnekleme tablosu**. **Önizleme'yi**seçin.

   ![Görsel Stüdyo Kodu önizleme kovucu tablosu için Spark & Hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-hive-table.png)

6. **Önizleme Sonuçları** penceresi açılır:

   ![Görsel Stüdyo Kodu önizleme sonuçları penceresi için Spark & Hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-results-window.png)

- SONUÇLAR paneli

   CSV, JSON veya Excel dosyası olarak tüm sonucu yerel bir yola kaydedebilir veya yalnızca birden çok satır seçebilirsiniz.

- MESAJLAR paneli
   1. Tablodaki satır sayısı 100'den büyükolduğunda, aşağıdaki iletiyi görürsünüz: "Hive tablosu için ilk 100 satır görüntülenir."
   2. Tablodaki satır sayısı 100'den az veya eşit olduğunda, aşağıdaki gibi bir ileti görürsünüz: "Kovan tablosu için 60 satır görüntülenir."
   3. Tabloda içerik olmadığında aşağıdaki iletiyi görürsünüz: "Hive tablosu için 0 satır görüntülenir."

        >[!NOTE]
        >
        >Linux'ta, kopya tablo verilerini etkinleştirmek için xclip'i yükleyin.
        >
        >![Linux Visual Studio kodu için Spark & Hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-linux-install-xclip.png)

## <a name="additional-features"></a>Ek özellikler

Visual Studio Code için Spark & Hive da aşağıdaki özellikleri destekler:

- **IntelliSense otomatik tamamlama**. Anahtar kelimeler, yöntemler, değişkenler ve diğer programlama öğeleri için öneriler açılır. Farklı simgeler farklı nesne türlerini temsil eder:

    ![Görsel Stüdyo Kodu IntelliSense nesneler için Spark & Hive Araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)

- **IntelliSense hata işaretçisi**. Dil hizmeti, Hive komut dosyasındaki düzenleme hatalarının altını çizer.     
- **Sözdizimi vurguları.** Dil hizmeti değişkenleri, anahtar kelimeleri, veri türünü, işlevleri ve diğer programlama öğelerini ayırt etmek için farklı renkler kullanır:

    ![Görsel Stüdyo Kodu sözdizimi için Spark & Hive Araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="reader-only-role"></a>Yalnızca okuyucu rolü

Küme için yalnızca okuyucu rolüatanan kullanıcılar artık HDInsight kümesine iş gönderemez ve Hive veritabanını görüntüleyebilir. Rolünüzü [Azure portalındaki](https://ms.portal.azure.com/) [**HDInsight Cluster Operator'a**](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-granular-access-cluster-configurations#add-the-hdinsight-cluster-operator-role-assignment-to-a-user) yükseltmek için küme yöneticisine başvurun. Geçerli Ambari kimlik bilgilerine sahipseniz, aşağıdaki kılavuzu kullanarak kümeyi el ile bağlayabilirsiniz.

### <a name="browse-the-hdinsight-cluster"></a>HDInsight kümesine göz atın  

Bir HDInsight kümesini genişletmek için Azure HDInsight gezginini seçtiğinizde, küme için yalnızca okuyucu rolüne sahipseniz kümeyi bağlamanız istenir. Ambari kimlik bilgilerinizi kullanarak kümeye bağlanmak için aşağıdaki yöntemi kullanın.

### <a name="submit-the-job-to-the-hdinsight-cluster"></a>İşi HDInsight kümesine gönderme

Bir HDInsight kümesine iş gönderirken, kümeiçin yalnızca okuyucu rolündeyseniz, kümeyi bağlamanız istenir. Ambari kimlik bilgilerini kullanarak kümeye bağlanmak için aşağıdaki adımları kullanın.

### <a name="link-to-the-cluster"></a>Kümeye bağlantı

1. Geçerli bir Ambari kullanıcı adı girin.
2. Geçerli bir parola girin.

   ![Görsel Stüdyo Kodu Kullanıcı Adı için Kıvılcım & Hive Araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-username.png)

   ![Görsel Stüdyo Kodu Şifresi için Kıvılcım & Kovan Araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-password.png)

  > [!NOTE]
  >
  >Bağlantılı kümeyi denetlemek için kullanabilirsiniz: `Spark / Hive: List Cluster`
  >
  >![Görsel Stüdyo Kodu Okuyucu Bağlantılı için Spark & Hive Araçları](./media/hdinsight-for-vscode/list-cluster-result1.png)

## <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

### <a name="browse-a-data-lake-storage-gen2-account"></a>Veri Gölü Depolama Gen2 hesabına göz atın

Veri Gölü Depolama Gen2 hesabını genişletmek için Azure HDInsight gezginini seçtiğinizde, Azure hesabınızda Gen2 depolama alanına erişimi yoksa depolama erişim anahtarını girmeniz istenir. Erişim anahtarı doğrulandıktan sonra, Veri Gölü Depolama Gen2 hesabı otomatik olarak genişletilir.

### <a name="submit-jobs-to-an-hdinsight-cluster-with-data-lake-storage-gen2"></a>Data Lake Storage Gen2 ile bir HDInsight kümesine iş gönderme

Veri Gölü Depolama Gen2'yi kullanarak bir HDInsight kümesine iş gönderdiğinizde, Azure hesabınızda Gen2 depolamasına yazma erişimi yoksa depolama erişim anahtarını girmeniz istenir. Erişim anahtarı doğrulandıktan sonra, iş başarıyla gönderilir.

![Görsel Stüdyo Kodu AccessKey için Spark & Hive Araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-accesskey.png)

> [!NOTE]
>
> Azure portalından depolama hesabının erişim anahtarını alabilirsiniz. Daha fazla bilgi için [bkz.](../storage/common/storage-account-keys-manage.md)

## <a name="unlink-cluster"></a>Bağlantıyı boşaltma kümesi

1. Menü çubuğundan Komut**Paletini** **Görüntüle'ye** > gidin ve ardından **Spark / Hive: Bir Kümeyi Bağlantıyı Aç'** ı girin.  

2. Bağlantıyı kızorunda almak için bir küme seçin.  

3. Doğrulama için **OUTPUT** görünümüne bakın.  

## <a name="sign-out"></a>Oturumu kapat  

Menü çubuğundan**Komut Paletini** **Görüntüle'ye** > gidin ve ardından **Azure'u girin: Çıkış Yapın.**

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio Code için Spark & Hive kullanarak gösteren bir video [için, Görsel Stüdyo Kodu için Spark & Hive](https://go.microsoft.com/fwlink/?linkid=858706)bakın.
