---
title: Visual Studio Code için Azure HDInsight
description: Sorgular ve betikler oluşturmak ve göndermek için Visual Studio Code için Spark & Hive araçları 'nı (Azure HDInsight) nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 51f0e223abd7103663fddd8c06dcdf0be549c671
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70930773"
---
# <a name="use-spark--hive-tools-for-visual-studio-code"></a>Visual Studio Code için Spark & Hive araçlarını kullanma

Apache Hive Batch işleri, etkileşimli Hive sorguları ve Apache Spark için PySpark betikleri oluşturmak ve göndermek üzere Visual Studio Code için Spark & Hive araçlarını nasıl kullanacağınızı öğrenin. İlk olarak, Spark & Hive araçlarının Visual Studio Code nasıl yükleneceğini açıklayacağız ve sonra Spark & Hive araçlarına nasıl iş göndereceğini adım adım inceleyeceğiz.  

Spark & Hive araçları, Windows, Linux ve macOS içeren Visual Studio Code tarafından desteklenen platformlar için yüklenebilir. Farklı platformlar için aşağıdaki önkoşullara göz önünde edin.


## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlamak için aşağıdaki öğeler gereklidir:

- Bir Azure HDInsight kümesi. Bir küme oluşturmak için bkz. [HDInsight kullanmaya başlama](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md). Veya Apache Livy uç noktasını destekleyen bir Spark ve Hive kümesi kullanın.
- [Visual Studio Code](https://code.visualstudio.com/).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono yalnızca Linux ve macOS için gereklidir.
- [Visual Studio Code Için PySpark etkileşimli ortamı](set-up-pyspark-interactive-environment.md).
- **Hdexas**adlı yerel bir dizin.  Bu makalede **C:\hd\hdexbol**kullanılır.

## <a name="install-spark--hive-tools"></a>Spark & Hive araçları 'nı yükler

Önkoşulları karşıladıktan sonra, aşağıdaki adımları izleyerek Visual Studio Code için Spark & Hive araçları 'nı yükleyebilirsiniz:

1. Visual Studio Code'u açın.

2. Menü çubuğunda,**uzantıları** **görüntüle** > ' ye gidin.

3. Arama kutusuna **Spark & Hive**girin.

4. Arama sonuçlarından **Spark & Hive araçları** ' nı seçin ve ardından **Install**' ı seçin:

   ![Visual Studio Code Python yüklemesi için Spark & Hive](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

5. Gerektiğinde **yeniden yükle** ' yi seçin.


## <a name="open-a-work-folder"></a>Bir iş klasörü açın

Bir iş klasörü açmak ve Visual Studio Code bir dosya oluşturmak için aşağıdaki adımları izleyin:

1. Menü çubuğundan **Dosya** > **Aç klasörü** > **c:\hd\hdexor**' a gidin ve ardından **Klasör Seç** düğmesini seçin. Klasör sol taraftaki **Gezgin** görünümünde görüntülenir.

2. **Gezgin** görünümü ' nde, **hdexas** klasörünü seçin ve sonra Iş klasörünün yanındaki **yeni dosya** simgesini seçin:

   ![Yeni dosya](./media/hdinsight-for-vscode/visual-studio-code-new-file.png)

3. `.hql` (Hive sorguları) `.py` ya da (Spark betiği) dosya uzantısını kullanarak yeni dosyayı adlandırın. Bu örnek **HelloWorld. HQL**kullanır.

## <a name="set-the-azure-environment"></a>Azure ortamını ayarlama

Ulusal bir bulut kullanıcısı için, önce Azure ortamını ayarlamak için aşağıdaki adımları izleyin ve ardından **Azure 'u kullanın: Azure 'da oturum açmak için oturum açkomutu:**
   
1. **File\preferences\settings**öğesini seçin.
2. Aşağıdaki dizeyi arayın: **Mavisi Una**
3. Listeden Ulusal bulutu seçin:

   ![Varsayılan oturum açma girdisi yapılandırmasını ayarla](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="connect-to-an-azure-account"></a>Azure hesabına bağlanma

Visual Studio Code kümenize komut dosyaları gönderebilmeniz için önce Azure hesabınıza bağlanmanız veya bir kümeyi bağlamanız gerekir (Apache ambarı Kullanıcı adı ve parola kimlik bilgilerini veya etki alanına katılmış bir hesabı kullanarak). Azure 'a bağlanmak için şu adımları izleyin:

1. Menü çubuğundan **Görünüm** > **komut paleti**' ne gidin ve Azure ' u girin **: Oturum aç**:

    ![Visual Studio Code oturum açma için Spark & Hive araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

2. Azure 'da oturum açmak için oturum açma yönergelerini izleyin. Bağlandıktan sonra, Azure hesabınızın adı Visual Studio Code penceresinin altındaki durum çubuğunda gösterilir.  
  

## <a name="link-a-cluster"></a>Bir kümeyi bağlama

### <a name="link-azure-hdinsight"></a>Bağlantısının Azure HDInsight

[Apache ambarı](https://ambari.apache.org/)ile yönetilen bir Kullanıcı adı kullanarak normal kümeyi bağlayabilir veya bir etki alanı Kullanıcı adı (örneğin: `user1@contoso.com`) kullanarak bir kurumsal güvenlik paketi güvenli Hadoop kümesini bağlayabilirsiniz.

1. Menü çubuğundan **Görünüm** > **komut paleti**' ne gidin ve Spark/Hive girin **: Bir kümeyi**bağlayın.

   ![bağlama kümesi komutu](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Bağlı küme türü **Azure HDInsight**' ı seçin.

3. HDInsight kümesi URL 'sini girin.

4. Ambarı Kullanıcı adınızı girin; **yönetici**varsayılandır.

5. Ambarı parolanızı girin.

6. Küme türünü seçin.

7. Kümenin görünen adını ayarlayın (isteğe bağlı).

8. Doğrulama için **Çıkış** görünümünü gözden geçirin.

   > [!NOTE]  
   > Küme hem Azure abonelikte hem de bir kümeye bağlandığında, bağlantılı Kullanıcı adı ve parola kullanılır.  


### <a name="link-generic-livy-endpoint"></a>Bağlantısının Genel Livy uç noktası

1. Menü çubuğundan **Görünüm** > **komut paleti**' ne gidin ve Spark/Hive girin **: Bir kümeyi**bağlayın.

2. Bağlı küme türü **genel Livy uç noktası**seçin.

3. Genel Livy uç noktasını girin. Örneğin: http\://10.172.41.42:18080.

4. Yetkilendirme türü **temel** veya **yok**' u seçin.  **Temel**' i seçerseniz:  
    &emsp;a. Ambarı Kullanıcı adınızı girin; **yönetici**varsayılandır.  
    &emsp;kenarı. Ambarı parolanızı girin.

5. Doğrulama için **Çıkış** görünümünü gözden geçirin.

## <a name="list-clusters"></a>Kümeleri Listele

1. Menü çubuğundan **Görünüm** > **komut paleti**' ne gidin ve Spark/Hive girin **: Küme**listesi.

2. İstediğiniz aboneliği seçin.

3. **Çıktı** görünümünü gözden geçirin. Bu görünüm, bağlantılı kümenizi (veya kümelerinizi) ve Azure aboneliğinizdeki tüm kümeleri gösterir:

    ![Varsayılan küme yapılandırması ayarlama](./media/hdinsight-for-vscode/list-cluster-result1.png)

## <a name="set-the-default-cluster"></a>Varsayılan kümeyi ayarlama

1. Daha önce, kapatıldıysa, [daha önce](#open-a-work-folder)ele alınan **hdexon** klasörünü yeniden açın.  

2. [Daha önce](#open-a-work-folder)oluşturulmuş **HelloWorld. HQL** dosyasını seçin. Betik düzenleyicisinde açılır.

3. Betik düzenleyicisine sağ tıklayın ve ardından Spark/Hive öğesini **seçin: Varsayılan kümeyi**ayarlayın.  

4. Azure hesabınıza [bağlanın](#connect-to-an-azure-account) veya henüz yapmadıysanız bir kümeyi bağlayın.

5. Geçerli betik dosyası için varsayılan küme olarak bir küme seçin. Araçlar öğesini otomatik olarak güncelleştirir **. VSCode\settings.json** yapılandırma dosyası:

   ![Varsayılan küme yapılandırmasını ayarla](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)


## <a name="submit-interactive-hive-queries-and-hive-batch-scripts"></a>Etkileşimli Hive sorguları ve Hive toplu iş betikleri gönder

Visual Studio Code için Spark & Hive araçları ile, kümelerinize etkileşimli Hive sorguları ve Hive toplu iş betikleri gönderebilirsiniz.

1. Daha önce, kapatıldıysa, [daha önce](#open-a-work-folder)ele alınan **hdexon** klasörünü yeniden açın.  

2. [Daha önce](#open-a-work-folder)oluşturulmuş **HelloWorld. HQL** dosyasını seçin. Betik düzenleyicisinde açılır.


3. Aşağıdaki kodu kopyalayıp Hive dosyanıza yapıştırın ve kaydedin:

    ```hiveql
    SELECT * FROM hivesampletable;
    ```

4. Azure hesabınıza [bağlanın](#connect-to-an-azure-account) veya henüz yapmadıysanız bir kümeyi bağlayın.

5. Betik düzenleyicisine sağ tıklayıp Hive ' yi seçin **: Sorguyu** göndermek için etkileşimli veya Ctrl + alt + ı klavye kısayolunu kullanın.  Hive **seçin: Betiği** göndermek için batch veya Ctrl + Alt + H klavye kısayolunu kullanın.  

6. Varsayılan bir küme belirtmediyseniz, bir küme seçin. Araçlar ayrıca bağlam menüsünü kullanarak tüm betik dosyası yerine bir kod bloğu göndermenize olanak tanır. Birkaç dakika sonra sorgu sonuçları yeni bir sekmede görünür:

   ![Interactive Hive sonucu](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **Sonuçlar** paneli: Tüm sonucu bir CSV, JSON veya Excel dosyası olarak yerel bir yola kaydedebilir veya yalnızca birden çok satır seçebilirsiniz.

    - **İletiler** paneli: Bir **satır** numarası seçtiğinizde, çalışan betiğin ilk satırına atlar.

## <a name="submit-interactive-pyspark-queries"></a>Etkileşimli PySpark sorguları gönder

Etkileşimli PySpark sorgularını göndermek için şu adımları izleyin:

1. Daha önce, kapatıldıysa, [daha önce](#open-a-work-folder)ele alınan **hdexon** klasörünü yeniden açın.  

2. [Önceki](#open-a-work-folder) adımları izleyerek yeni bir **HelloWorld.py** dosyası oluşturun.

3. Aşağıdaki kodu kopyalayıp betik dosyasına yapıştırın:
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

4. Azure hesabınıza [bağlanın](#connect-to-an-azure-account) veya henüz yapmadıysanız bir kümeyi bağlayın.

5. Tüm kodu seçin, betik düzenleyicisine sağ tıklayın ve Spark ' ı seçin **: Sorguyu göndermek için** pyspark etkileşimli. Ya da Ctrl + alt + ı kısayolunu kullanın.

   ![pyspark etkileşimli bağlam menüsü](./media/hdinsight-for-vscode/pyspark-interactive-right-click.png)

6. Varsayılan bir küme belirtmediyse kümeyi seçin. Birkaç dakika sonra **Python etkileşimli** sonuçları yeni bir sekmede görüntülenir. Araçlar ayrıca bağlam menüsünü kullanarak tüm betik dosyası yerine bir kod bloğu göndermenize olanak tanır:

   ![pyspark etkileşimli Python etkileşimli penceresi](./media/hdinsight-for-vscode/pyspark-interactive-python-interactive-window.png) 

7. **%% Info**girin ve sonra iş bilgilerini görüntülemek için SHIFT + enter tuşlarına basın (isteğe bağlı):

   ![iş bilgilerini görüntüle](./media/hdinsight-for-vscode/pyspark-interactive-view-job-information.png)

8. Araç **Spark SQL** sorgusunu da destekler:

   ![Pyspark etkileşimli görünüm sonucu](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png)

   Sorgu çalıştırırken, gönderim durumu alt durum çubuğunun solunda görünür. Durum **Pyspark Kernel (meşgul)** olduğunda başka sorgular göndermeyin.  

   > [!NOTE] 
   >
   > Ayarlarda **Python uzantısı etkinleştirildiğinde** (varsayılan olarak seçilidir), gönderilen pyspark etkileşim sonuçları eski pencereyi kullanır:
   >
   > ![pyspark etkileşimli Python uzantısı devre dışı](./media/hdinsight-for-vscode/pyspark-interactive-python-extension-disabled.png)


## <a name="submit-pyspark-batch-job"></a>PySpark Batch işini gönder

1. Daha önce, kapatılmışsa, [daha önce](#open-a-work-folder)tartışılan **hdex,** klasörünü yeniden açın.  

2. [Önceki](#open-a-work-folder) adımları izleyerek yeni bir **BatchFile.py** dosyası oluşturun.

3. Aşağıdaki kodu kopyalayıp betik dosyasına yapıştırın:

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

4. Azure hesabınıza [bağlanın](#connect-to-an-azure-account) veya henüz yapmadıysanız bir kümeyi bağlayın.

5. Betik düzenleyicisine sağ tıkladıktan sonra Spark ' ı seçin **: Pyspark Batch**veya Ctrl + Alt + H klavye kısayolunu kullanın. 

6. PySpark işinizi göndermek için bir küme seçin:

   ![Python işi sonucunu gönder](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Bir Python işi gönderdikten sonra, gönderim günlükleri Visual Studio Code **Çıkış** penceresinde görüntülenir. Spark Kullanıcı arabirimi URL 'SI ve Yarn UI URL 'SI de gösterilir. İşi durumunu izlemek için URL 'YI bir Web tarayıcısında açabilirsiniz.

## <a name="apache-livy-configuration"></a>Apache Livy yapılandırması

[Apache Livy](https://livy.incubator.apache.org/) yapılandırması desteklenir. İçinde yapılandırabilirsiniz **. Çalışma alanı klasöründeki VSCode\settings.json** dosyası. Şu anda, Livy yapılandırma yalnızca Python betiğini destekliyor. Daha fazla ayrıntı için bkz. [Livy Benioku](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Livy yapılandırması tetiklenmesi**

Yöntem 1  
1. Menü çubuğundan **Dosya** > **tercihleri** > **ayarları**' na gidin.  
2. **Arama ayarları** kutusuna HDInsight iş gönderimi girin **: Livy conf**.  
3. İlgili arama sonucu için **Settings. JSON Içinde Düzenle '** yi seçin.

Yöntem 2   
Bir dosya gönderir ve. vscode klasörünün otomatik olarak iş klasörüne eklendiğinden emin olun. **. Vscode\settings.JSON**öğesini seçerek Livy yapılandırmasını görebilirsiniz.

+ Proje ayarları:

    ![Livy yapılandırması](./media/hdinsight-for-vscode/hdi-apache-livy-config.png)

    >[!NOTE]
    >**Drivermemory** ve **executormemory** ayarları için değeri ve birimi ayarlayın. Örneğin: 1G veya 1024m.

+ Desteklenen Livy yapılandırması:   

    **/Toplu iş SONRASı**   
    İstek gövdesi

    | name | description | type | 
    | :- | :- | :- | 
    | file | Yürütülecek uygulamayı içeren dosya | yol (gerekli) |
    | proxyUser | İşi çalıştırırken taklit edilecek Kullanıcı | Dize |
    | Sınıf | Uygulama Java/Spark ana sınıfı | Dize |
    | args | Uygulama için komut satırı bağımsız değişkenleri | dizelerin listesi |
    | jars | Bu oturumda kullanılacak jar dosyaları dışındaki | dizelerin listesi | 
    | pyFiles | Bu oturumda kullanılacak Python dosyaları | dizelerin listesi |
    | dosyaları | Bu oturumda kullanılacak dosyalar | dizelerin listesi |
    | driverMemory | Sürücü işlemi için kullanılacak bellek miktarı | Dize |
    | Driverçekirdekler | Sürücü işlemi için kullanılacak çekirdek sayısı | Int |
    | Yürütişbelleği | Yürütücü işlemi başına kullanılacak bellek miktarı | Dize |
    | Executorçekirdekler | Her yürütücü için kullanılacak çekirdek sayısı | Int |
    | numExecutors | Bu oturum için başlatılacak yürüticilerinin sayısı | Int |
    | archives | Bu oturumda kullanılacak Arşivler | dizelerin listesi |
    | queue | Gönderilecek YARN kuyruğunun adı| Dize |
    | name | Bu oturumun adı | Dize |
    | conf | Spark yapılandırma özellikleri | Anahtar = Val eşlemesi |

    Yanıt gövdesi   
    Oluşturulan Batch nesnesi.

    | name | description | type |
    | :- | :- | :- | 
    | id | Oturum kimliği | Int | 
    | appId | Bu oturumun uygulama kimliği | Dize |
    | appInfo | Ayrıntılı uygulama bilgileri | Anahtar = Val eşlemesi |
    | log | Günlük satırları | dizelerin listesi |
    | state |Toplu iş durumu | Dize |

    >[!NOTE]
    >Atanan Livy yapılandırması, betiği gönderdiğinizde çıkış bölmesinde görüntülenir.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Gezgin 'den Azure HDInsight ile tümleştirme

**Azure HDInsight** , Gezgin görünümüne eklendi. Kümelerinizi doğrudan **Azure HDInsight**aracılığıyla göz atabilir ve yönetebilirsiniz.

1. Azure hesabınıza [bağlanın](#connect-to-an-azure-account) veya henüz yapmadıysanız bir kümeyi bağlayın.

2. Menü çubuğundan **Görünüm** > **Gezgini**' ne gidin.

3. Sol bölmeden **Azure HDInsight**' ı genişletin.  Kullanılabilir abonelikler ve kümeler (Spark, Hadoop ve HBase desteklenir) listelenmiştir:

   ![Azure HDInsight aboneliği](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

4. Hive meta veri veritabanını ve tablo şemasını görüntülemek için kümeyi genişletin:

   ![Azure HDInsight kümesi](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)


## <a name="preview-hive-table"></a>Hive tablosunun önizlemesini görüntüleyin
Doğrudan **Azure HDInsight** Gezgini aracılığıyla kümelerinizde Hive tablosunun önizlemesini yapabilirsiniz:
1. Henüz yapmadıysanız Azure hesabınıza [bağlanın](#connect-to-an-azure-account) .

2. En soldaki sütundan **Azure** simgesini seçin.

3. Sol bölmeden **Azure HDInsight**' ı genişletin. Kullanılabilir abonelikler ve kümeler listelenir.

4. Hive meta veri veritabanını ve tablo şemasını görüntülemek için kümeyi genişletin.

5. Hive tablosuna sağ tıklayın. Örneğin: **hivesampletable**. **Önizleme**' yi seçin. 

   ![Visual Studio Code Preview Hive tablosu için Spark & Hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-hive-table.png)

6. **Önizleme sonuçları** penceresi açılır:

   ![Visual Studio Code Önizleme sonuçları penceresi için Spark & Hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-results-window.png)
   
- SONUÇLAR paneli

   Tüm sonucu bir CSV, JSON veya Excel dosyası olarak yerel bir yola kaydedebilir veya yalnızca birden fazla satır seçebilirsiniz.

- ILETILER bölmesi
   1. Tablodaki satır sayısı 100 ' den büyükse aşağıdaki iletiyi görürsünüz: "Hive tablosu için ilk 100 satır görüntülenir."
   2. Tablodaki satır sayısı 100 ' den az veya bu değere eşitse, aşağıdakine benzer bir ileti görürsünüz: "60 satır Hive tablosu için gösteriliyor."
   3. Tabloda içerik yoksa, aşağıdaki iletiyi görürsünüz: "Hive tablosu için 0 satır görüntülenir."

        >[!NOTE]
        >
        >Linux 'ta, kopyalama tablosu verilerini etkinleştirmek için xclip ' i de yüklersiniz.
        >
        >![Linux 'ta Visual Studio Code için Spark & Hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-linux-install-xclip.png)
    ## <a name="additional-features"></a>Ek özellikler

Visual Studio Code için Spark & Hive aşağıdaki özellikleri de destekler:

- **IntelliSense otomatik tamamlama**. Anahtar sözcükler, Yöntemler, değişkenler ve diğer programlama öğeleri için açılan öneriler. Farklı simgeler farklı nesne türlerini temsil eder:

    ![Visual Studio Code IntelliSense nesne türleri için Spark & Hive araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense hata işaretleyicisi**. Dil hizmeti, Hive betiğinin düzenlenme hatalarının altını çizer.     
- **Söz dizimi önemli**. Dil hizmeti değişkenleri, anahtar sözcükleri, veri türünü, işlevleri ve diğer programlama öğelerini ayırt etmek için farklı renkler kullanır:

    ![Visual Studio Code sözdizimi için Spark & Hive araçları vurguları](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="reader-only-role"></a>Yalnızca okuyucu rolü

Küme için yalnızca okuyucu rolü atanan kullanıcılar artık HDInsight kümesine iş gönderemez ve Hive veritabanını görüntüleyebilirler. Rolünüzü [Azure Portal](https://ms.portal.azure.com/) [**HDInsight kümesi işlecine**](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-granular-access-cluster-configurations#add-the-hdinsight-cluster-operator-role-assignment-to-a-user) yükseltmek için küme yöneticisine başvurun. Geçerli bir ambarı kimlik bilgileriniz varsa, aşağıdaki kılavuzu kullanarak kümeyi el ile bağlayabilirsiniz.

### <a name="browse-the-hdinsight-cluster"></a>HDInsight kümesine gözatın  

Bir HDInsight kümesini genişletmek için Azure HDInsight Gezginini seçtiğinizde, küme için yalnızca okuyucu rolüne sahipseniz kümeyi bağlamanız istenir. Ambarı kimlik bilgilerinizi kullanarak kümeye bağlamak için aşağıdaki yöntemi kullanın. 

### <a name="submit-the-job-to-the-hdinsight-cluster"></a>İşi HDInsight kümesine gönder

İş bir HDInsight kümesine gönderilirken, küme için yalnızca okuyucu rolünde olduğunuzda kümeyi bağlamanız istenir. Ambarı kimlik bilgilerini kullanarak kümeye bağlamak için aşağıdaki adımları kullanın.

### <a name="link-to-the-cluster"></a>Kümeye bağlantı

1.  Geçerli bir ambarı Kullanıcı adı girin.
2.  Geçerli bir parola girin.

   ![Visual Studio Code Kullanıcı adı için Spark & Hive araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-username.png)

   ![Visual Studio Code parola için Spark & Hive araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-password.png)

  > [!NOTE]
  >
  >Bağlantılı kümeyi denetlemek `Spark / Hive: List Cluster` için ' i kullanabilirsiniz:
  >
  >![Visual Studio Code okuyucusu bağlantılı için Spark & Hive araçları](./media/hdinsight-for-vscode/list-cluster-result1.png)

## <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. Nesil

### <a name="browse-a-data-lake-storage-gen2-account"></a>Data Lake Storage 2. hesaba gözatıp

Data Lake Storage 2. bir hesabı genişletmek için Azure HDInsight Gezginini seçtiğinizde, Azure hesabınızın Gen2 depolama alanına erişimi yoksa depolama erişim anahtarını girmeniz istenir. Erişim anahtarı doğrulandıktan sonra, Data Lake Storage 2. hesabı otomatik genişletilir.

### <a name="submit-jobs-to-an-hdinsight-cluster-with-data-lake-storage-gen2"></a>Data Lake Storage 2. olan bir HDInsight kümesine iş gönderme

Data Lake Storage 2. kullanarak bir HDInsight kümesine iş gönderdiğinizde, Azure hesabınızın Gen2 Storage 'a yazma erişimi yoksa depolama erişim anahtarını girmeniz istenir. Erişim anahtarı doğrulandıktan sonra, iş başarılı bir şekilde gönderilir.

![Visual Studio Code AccessKey için Spark & Hive araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-accesskey.png)   

> [!NOTE]
> 
>Depolama hesabı için erişim anahtarını Azure portal alabilirsiniz. Daha fazla bilgi için bkz. [erişim anahtarlarını görüntüleme ve kopyalama](https://docs.microsoft.com/azure/storage/common/storage-account-manage#access-keys).

## <a name="unlink-cluster"></a>Kümenin bağlantısını kaldır

1. Menü çubuğundan **Görünüm** > **komut paleti**' ne gidin ve Spark/Hive girin **: Bir kümenin**bağlantısını kaldırın.  

2. Bağlantısını kaldırmak için bir küme seçin.  

3. Doğrulama için **Çıkış** görünümüne bakın.  

## <a name="sign-out"></a>Oturumu kapat  

Menü çubuğundan **Görünüm** > **komut paleti**' ne gidin ve Azure ' u girin **: Oturumu kapatın**.


## <a name="next-steps"></a>Sonraki adımlar
Visual Studio Code için Spark & Hive kullanmayı gösteren bir video için bkz. [spark & Hive for Visual Studio Code](https://go.microsoft.com/fwlink/?linkid=858706).
