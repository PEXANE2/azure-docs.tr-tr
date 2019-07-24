---
title: Visual Studio Code için Azure HDInsight
description: Sorgular ve betikler oluşturmak ve göndermek için Visual Studio Code için Spark & Hive araçları 'nı (Azure HDInsight) nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 31f6c34089c1825eca21283b01eae181c8112216
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68312186"
---
# <a name="use-spark--hive-tools-for-visual-studio-code"></a>Visual Studio Code için Spark & Hive araçlarını kullanma

Apache Hive Batch işleri, etkileşimli Hive sorguları ve Apache Spark için PySpark betikleri oluşturmak ve göndermek üzere Visual Studio Code için Spark & Hive araçlarını nasıl kullanacağınızı öğrenin. İlk olarak, Visual Studio Code Spark & Hive araçlarının nasıl yükleneceğini açıklayacağız ve sonra Hive ve Spark 'a nasıl iş göndereceğini adım adım inceleyeceğiz.  

Spark & Hive araçları, Windows, Linux ve macOS içeren Visual Studio Code tarafından desteklenen platformlar için yüklenebilir. Aşağıda, farklı platformlar için önkoşulları bulacaksınız.


## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlamak için aşağıdaki öğeler gereklidir:

- An HDInsight küme. Bir küme oluşturmak için bkz. [HDInsight kullanmaya başlama](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md). Ya da Livy uç noktasını destekleyen bir Spark/Hive kümesi.
- [Visual Studio Code](https://code.visualstudio.com/).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono yalnızca Linux ve macOS için gereklidir.
- [Visual Studio Code Için PySpark etkileşimli ortamını ayarlayın](set-up-pyspark-interactive-environment.md).
- **Hdexas**adlı yerel bir dizin.  Bu makalede **C:\hd\hdexbol**kullanılır.

## <a name="install-spark--hive-tools"></a>Spark & Hive araçları 'nı yükler

Önkoşulları tamamladıktan sonra, Visual Studio Code için Spark & Hive araçları 'nı yükleyebilirsiniz.  Spark & Hive araçları 'nı yüklemek için aşağıdaki adımları izleyin:

1. Visual Studio Code'u açın.

2. Menü çubuğundan **Görünüm** > **uzantıları**' na gidin.

3. Arama kutusuna **Spark & Hive**girin.

4. Arama sonuçlarından **Spark & Hive araçları** ' nı seçin ve ardından **Install**' ı seçin.  

   ![Visual Studio Code Python yüklemesi için Spark & Hive](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

5. Gerektiğinde **yeniden yükleyin** .


## <a name="open-work-folder"></a>Çalışma klasörünü aç

Bir iş klasörü açmak ve Visual Studio Code bir dosya oluşturmak için aşağıdaki adımları izleyin:

1. Menü çubuğundan **Dosya** > aç klasörü ' ne gidin **...** C:\hd\hdex, ardından **Klasör Seç** düğmesini seçin.   >  Klasör sol taraftaki **Gezgin** görünümünde görüntülenir.

2. **Gezgin** görünümünden, **hdexas**klasörünü ve ardından çalışma klasörünün yanındaki **yeni dosya** simgesini seçin.

   ![Yeni dosya](./media/hdinsight-for-vscode/new-file.png)

3. Yeni dosyayı `.hql` (Hive sorguları) `.py` ya da (Spark betiği) dosya uzantısıyla adlandırın.  Bu örnek **HelloWorld. HQL**kullanır.

## <a name="set-the-azure-environment"></a>Azure ortamını ayarlama

Ulusal bulut kullanıcısı için önce Azure ortamını ayarlama adımlarını izleyin ve ardından Azure 'u kullanın **: Azure 'da oturum açmak için oturum açınkomutu.**
   
1. **File\preferences\settings**' e tıklayın.
2. Azure **'da ara: Una**
3. Listeden Ulusal bulutu seçin.

   ![Varsayılan oturum açma girdisi yapılandırmasını ayarla](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="connect-to-azure-account"></a>Azure hesabına Bağlan

Visual Studio Code kümenize komut dosyaları gönderebilmeniz için önce Azure hesabınıza bağlanmanız veya bir kümeyi bağlamanız gerekir (ambarı Kullanıcı adı/parolası veya etki alanına katılmış hesap kullanarak).  Azure 'a bağlanmak için aşağıdaki adımları izleyin:

1. Menü çubuğundan **Görünüm** > **komut paleti...** ' a gidin ve Azure ' u **girin: Oturum açın**.

    ![Visual Studio Code oturum açma için Spark & Hive araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

2. Azure 'da oturum açmak için oturum açma yönergelerini izleyin. Bağlandıktan sonra, Azure hesabınızın adı Visual Studio Code penceresinin altındaki durum çubuğunda gösterilir.  
  

## <a name="link-a-cluster"></a>Bir kümeyi bağlama

### <a name="link-azure-hdinsight"></a>Bağlantısının Azure HDInsight

Bir etki alanı Kullanıcı adı (örneğin: user1@contoso.com) kullanarak bir [Apache ambarı](https://ambari.apache.org/) yönetilen Kullanıcı adı veya kurumsal güvenlik paketi güvenli Hadoop kümesi kullanarak normal bir kümeyi bağlayabilirsiniz.

1. Menü çubuğundan **Görünüm** > **komut paleti...** ' a gidin ve Spark/Hive **girin: Bir kümeyi**bağlayın.

   ![bağlama kümesi komutu](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Bağlı küme türü **Azure HDInsight**' ı seçin.

3. HDInsight kümesi URL 'sini girin.

4. Ambarı Kullanıcı adı girin, varsayılan **admin**' dir.

5. Ambarı parolasını girin.

6. Küme türünü seçin.

7. Kümenin görünen adını ayarlayın (Isteğe bağlı).

8. Doğrulama için **Çıkış** görünümünü gözden geçirin.

   > [!NOTE]  
   > Küme hem Azure abonelikte hem de bir kümeye bağlandığında, bağlantılı Kullanıcı adı ve parola kullanılır.  


### <a name="link-generic-livy-endpoint"></a>Bağlantısının Genel Livy uç noktası

1. Menü çubuğundan **Görünüm** > **komut paleti...** ' a gidin ve Spark/Hive **girin: Bir kümeyi**bağlayın.

2. Bağlı küme türü **genel Livy uç noktası**seçin.

3. Genel Livy uç noktasını girin, örneğin: http\://10.172.41.42:18080.

4. Yetkilendirme türü **temel** veya **yok**' u seçin.  **Temel**ise:  
    &emsp;a. Ambarı Kullanıcı adı girin, varsayılan **admin**' dir.  
    &emsp;kenarı. Ambarı parolasını girin.

5. Doğrulama için **Çıkış** görünümünü gözden geçirin.

## <a name="list-clusters"></a>Kümeleri Listele

1. Menü çubuğundan **Görünüm** > **komut paleti...** ' a gidin ve Spark/Hive **girin: Küme**listesi.

2. İstediğiniz aboneliği seçin.

3. **Çıktı** görünümünü gözden geçirin.  Görünüm, bağlantılı kümelerinizi ve tüm kümelerinizi Azure aboneliğiniz altında gösterecektir.

    ![Varsayılan küme yapılandırması ayarlama](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Varsayılan kümeyi ayarla

1. Daha önce kapatıldıysa, [daha önce](#open-work-folder) oluşturulan **hdexon** klasörünü yeniden açın.  

2. [Daha önce](#open-work-folder) oluşturulan **HelloWorld. HQL** dosyasını seçin ve betik düzenleyicisinde açılır.

3. Betik düzenleyicisine sağ tıklayın ve Spark/Hive öğesini **seçin: Varsayılan kümeyi**ayarlayın.  

4. Henüz yapmadıysanız Azure hesabınıza [bağlanın](#connect-to-azure-account) veya bir kümeyi bağlayın.

5. Geçerli betik dosyası için varsayılan küme olarak bir küme seçin. Araçlar yapılandırma dosyasını otomatik olarak güncelleştirir **. VSCode\settings.json**. 

   ![Varsayılan küme yapılandırmasını ayarla](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)


## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Etkileşimli Hive sorguları, Hive toplu iş betikleri gönder

Visual Studio Code için Spark & Hive araçları ile, kümelerinize etkileşimli Hive sorguları ve Hive toplu iş betikleri gönderebilirsiniz.

1. [Daha önce](#open-work-folder) kapatılmışsa **hdex,** oluşturulan klasörü yeniden açın.  

2. [Daha önce](#open-work-folder) oluşturulan **HelloWorld. HQL** dosyasını seçin ve betik düzenleyicisinde açılır.


3. Aşağıdaki kodu kopyalayıp Hive dosyanıza yapıştırın ve kaydedin.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```

4. Henüz yapmadıysanız Azure hesabınıza [bağlanın](#connect-to-azure-account) veya bir kümeyi bağlayın.

5. Betik düzenleyicisine sağ tıklayın, Hive ' ı **seçin: Sorguyu** göndermek için etkileşimli veya **Ctrl + alt + ı**kısayolunu kullanın.  Hive **seçin: Betiği** göndermek için batch veya **Ctrl + Alt + H**kısayolunu kullanın.  

6. Varsayılan bir küme belirtmediyse kümeyi seçin. Araçlar, bağlam menüsünü kullanarak betik dosyasının tamamı yerine bir kod bloğu göndermenize de olanak sağlar. Birkaç dakika sonra sorgu sonuçları yeni bir sekmede görüntülenir.

   ![Interactive Hive sonucu](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **Sonuçlar** paneli: Tüm sonucu yerel yola CSV, JSON veya Excel dosyası olarak kaydedebilir veya yalnızca birden çok satır seçebilirsiniz.

    - **İletiler** paneli: **Satır** numarası ' nı seçtiğinizde, çalışan betiğin ilk satırına atlar.

## <a name="submit-interactive-pyspark-queries"></a>Etkileşimli PySpark sorguları gönder

Aşağıdaki adımları izleyerek etkileşimli PySpark sorguları gönderebilirsiniz:

1. [Daha önce](#open-work-folder) kapatılmışsa **hdex,** oluşturulan klasörü yeniden açın.  

2. [Önceki](#open-work-folder) adımları izleyerek **HelloWorld.py** yeni bir dosya oluşturun.

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

4. Henüz yapmadıysanız Azure hesabınıza [bağlanın](#connect-to-azure-account) veya bir kümeyi bağlayın.

5. Tüm kodu seçin ve betik düzenleyicisine sağ tıklayıp Spark ' ı seçin **: Sorguyu göndermek için** pyspark etkileşimli veya **Ctrl + alt + ı**kısayolunu kullanın.

   ![pyspark etkileşimli bağlam menüsü](./media/hdinsight-for-vscode/pyspark-interactive-right-click.png)

6. Varsayılan bir küme belirtmediyse kümeyi seçin. Birkaç dakika sonra **Python etkileşimli** sonuçları yeni bir sekmede görüntülenir. Araçlar, bağlam menüsünü kullanarak betik dosyasının tamamı yerine bir kod bloğu göndermenize de olanak sağlar. 

   ![pyspark etkileşimli Python etkileşimli penceresi](./media/hdinsight-for-vscode/pyspark-interactive-python-interactive-window.png) 

7. **"%% Info"** yazın ve sonra iş bilgilerini görüntülemek için **SHIFT + enter** tuşlarına basın. Seçim

   ![iş bilgilerini görüntüle](./media/hdinsight-for-vscode/pyspark-interactive-view-job-information.png)

8. Araç **Spark SQL** sorgusunu da destekler.

   ![Pyspark etkileşimli görünüm sonucu](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png)

   Sorgu çalıştırırken, son durum çubuğunun sol tarafında gönderim durumu görünür. Durum **Pyspark Kernel (meşgul)** olduğunda başka sorgular göndermeyin.  

   > [!NOTE] 
   >
   > **Python uzantısı etkin** olmadığında (varsayılan ayar denetlenir), gönderilen pyspark etkileşim sonuçları eski pencereyi kullanır.
   >
   > ![pyspark etkileşimli Python uzantısı devre dışı](./media/hdinsight-for-vscode/pyspark-interactive-python-extension-disabled.png)


## <a name="submit-pyspark-batch-job"></a>PySpark Batch işini gönder

1. [Daha önce](#open-work-folder) kapatılmışsa **hdex,** oluşturulan klasörü yeniden açın.  

2. [Önceki](#open-work-folder) adımları izleyerek **BatchFile.py** yeni bir dosya oluşturun.

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

4. Henüz yapmadıysanız Azure hesabınıza [bağlanın](#connect-to-azure-account) veya bir kümeyi bağlayın.

5. Betik düzenleyicisine sağ tıkladıktan sonra Spark ' ı seçin **: Pyspark Batch**veya **Ctrl + Alt + H**kısayolunu kullanın. 

6. PySpark işinizin gönderileceği kümeyi seçin. 

   ![Python işi sonucunu gönder](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Bir Python işi gönderdikten sonra, gönderim günlükleri Visual Studio Code **Çıkış** penceresinde görüntülenir. **Spark Kullanıcı arabirimi URL 'si** ve **Yarn UI URL 'si** de gösterilir. İşi durumunu izlemek için URL 'YI bir Web tarayıcısında açabilirsiniz.

## <a name="apache-livy-configuration"></a>Apache Livy yapılandırması

[Apache Livy](https://livy.incubator.apache.org/) yapılandırması desteklenir, öğesinde ayarlanabilir **. Çalışma alanı klasöründe VSCode\settings.json** . Şu anda, Livy yapılandırma yalnızca Python betiğini destekliyor. Daha fazla ayrıntı için bkz. [Livy Benioku](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Livy yapılandırması tetiklenmesi**

Yöntem 1  
1. Menü çubuğundan **Dosya** > **tercihleri** > **ayarları**' na gidin.  
2. **Arama ayarları** metin kutusuna HDInsight iş sumisi girin **: Livy conf**.  
3. İlgili arama sonucu için **Settings. JSON Içinde Düzenle '** yi seçin.

Yöntem 2   
Bir dosya gönderirseniz,. vscode klasörünün iş klasörüne otomatik olarak eklendiğini görürsünüz. Livy yapılandırmasını **. vscode\settings.JSON**öğesine tıklayarak bulabilirsiniz.

+ Proje ayarları:

    ![Livy yapılandırması](./media/hdinsight-for-vscode/hdi-livyconfig.png)

>[!NOTE]
>Ayarlar **Drivermomory** ve **Executormomry**için, değeri, örneğin 1G veya 1024m gibi bir birimle ayarlayın. 

+ Desteklenen Livy yapılandırması:   

    **/Toplu iş SONRASı**   
    İstek Gövdesi

    | name | description | türü | 
    | :- | :- | :- | 
    | file | Yürütülecek uygulamayı içeren dosya | yol (gerekli) | 
    | proxyUser | İşi çalıştırırken taklit edilecek Kullanıcı | dize | 
    | Sınıf | Uygulama Java/Spark ana sınıfı | dize |
    | args | Uygulama için komut satırı bağımsız değişkenleri | dizelerin listesi | 
    | jars | Bu oturumda kullanılacak jar dosyaları dışındaki | Dize listesi | 
    | pyFiles | Bu oturumda kullanılacak Python dosyaları | Dize listesi |
    | files | Bu oturumda kullanılacak dosyalar | Dize listesi |
    | driverMemory | Sürücü işlemi için kullanılacak bellek miktarı | dize |
    | Driverçekirdekler | Sürücü işlemi için kullanılacak çekirdek sayısı | int |
    | Yürütişbelleği | Yürütücü işlemi başına kullanılacak bellek miktarı | dize |
    | Executorçekirdekler | Her yürütücü için kullanılacak çekirdek sayısı | int |
    | numExecutors | Bu oturum için başlatılacak yürüticilerinin sayısı | int |
    | archives | Bu oturumda kullanılacak Arşivler | Dize listesi |
    | queue | Gönderilen YARN kuyruğunun adı | dize |
    | name | Bu oturumun adı | dize |
    | conf | Spark yapılandırma özellikleri | Anahtar = Val eşlemesi |

    Yanıt gövdesi   
    Oluşturulan Batch nesnesi.

    | name | description | türü | 
    | :- | :- | :- | 
    | id | Oturum kimliği | int | 
    | appId | Bu oturumun uygulama kimliği |  Dize |
    | appInfo | Ayrıntılı uygulama bilgileri | Anahtar = Val eşlemesi |
    | log | Günlük satırları | dizelerin listesi |
    | state |   Toplu iş durumu | dize |

>[!NOTE]
>Atanmış Livy yapılandırması, komut dosyası gönderme sırasında çıkış bölmesinde görüntülenir.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Gezgin 'den Azure HDInsight ile tümleştirme

**Azure HDInsight** , Gezgin görünümüne eklendi. **Azure HDInsight**aracılığıyla doğrudan sizin kümenize göz atabilir ve bunları yönetebilirsiniz.

1. Henüz yapmadıysanız Azure hesabınıza [bağlanın](#connect-to-azure-account) veya bir kümeyi bağlayın.

2. Menü çubuğundan **Görünüm** > **Gezgini**' ne gidin.

3. Sol bölmeden **Azure HDInsight**' ı genişletin.  Kullanılabilir abonelikler ve kümeler (Spark, Hadoop ve HBase desteklenir) listelenir. 

   ![Azure HDInsight aboneliği](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

4. Hive meta veri veritabanını ve tablo şemasını görüntülemek için kümeyi genişletin.

   ![Azure HDInsight kümesi](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)


## <a name="preview-hive-table"></a>Hive tablosunun önizlemesini görüntüleyin
Doğrudan **Azure HDInsight** Explorer aracılığıyla kümenizdeki Hive tablosunun önizlemesini yapabilirsiniz.
1. Henüz yapmadıysanız Azure hesabınıza [bağlanın](#connect-to-azure-account) .

2. En soldaki sütundan **Azure** simgesine tıklayın.

3. Sol bölmeden **Azure HDInsight**' ı genişletin. Kullanılabilir abonelikler ve kümeler listelenecektir.

4. Hive meta veri veritabanını ve tablo şemasını görüntülemek için kümeyi genişletin.

5. Hive tablosuna sağ tıklayın, örneğin, hivesampletable. **Önizleme**' yi seçin. 

   ![Visual Studio Code Preview Hive tablosu için Spark & Hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-hive-table.png)

6. **Önizleme sonuçları** penceresi açılır.

   ![Visual Studio Code Preview sonuçları penceresi için Spark & Hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-results-window.png)
   
- **Sonuçlar** paneli

   Tüm sonucu yerel yola CSV, JSON veya Excel dosyası olarak kaydedebilir veya yalnızca birden çok satır seçebilirsiniz.

- **İletiler** bölmesi
   1. Tablodaki satır sayısı 100 satırdan fazlaysa ileti şu şekilde görünür: **Hive tablosu için ilk 100 satırı görüntülenir**.
   2. Tablodaki satır sayısı 100 satıra eşit veya bundan küçükse ileti şu şekilde görünür: **Hive tablosu için 60 satır görüntülenir**.
   3. Tabloda içerik yoksa ileti şu şekilde görünür: **Hive tablosu için 0 satırı görüntülenir**.

>[!NOTE]
>
>Linux 'ta tablo verilerini kopyalamayı etkinleştirmek için xclip ' i de yüklersiniz.
>
>![Linux 'ta Visual Studio Code için Spark & Hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-linux-install-xclip.png)
## <a name="additional-features"></a>Ek özellikler

Visual Studio Code için Spark & Hive aşağıdaki özellikleri destekler:

- **IntelliSense otomatik tamamlama**. Anahtar sözcük, Yöntemler, değişkenler vb. için açılan öneriler. Farklı simgeler farklı nesne türlerini temsil eder.

    ![Visual Studio Code IntelliSense nesne türleri için Spark & Hive araçları](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense hata işaretleyicisi**. Dil hizmeti, Hive betiğinin düzenlenme hatalarının altını çizer.     
- **Söz dizimi önemli**. Dil hizmeti değişkenleri, anahtar sözcükleri, veri türünü, işlevleri vb. ayırt etmek için farklı renkler kullanır. 

    ![Visual Studio Code sözdizimi için Spark & Hive araçları vurguları](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="reader-only-role"></a>Yalnızca okuyucu rolü

Yalnızca küme **okuyucusu**  **rolüne** sahip kullanıcılar artık HDInsight kümesine iş gönderemez ve Hive veritabanını görüntüleyemez. Rolünüzü [Azure Portal](https://ms.portal.azure.com/) [ **HDInsight** **kümesi** **işlecine** ](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-granular-access-cluster-configurations#add-the-hdinsight-cluster-operator-role-assignment-to-a-user) yükseltmek için küme yöneticisine başvurun. Ambarı kimlik bilgilerini biliyorsanız, aşağıdaki yönergeden sonra kümeyi el ile bağlayabilirsiniz.

### <a name="browse-hdinsight-cluster"></a>HDInsight kümesine gözatmaya  

HDInsight kümesini genişletmek için Azure HDInsight Gezgini ' ne tıkladığınızda, küme için yalnızca okuyucu rolü kullanıyorsanız, kümeyi bağlamanız istenir. Ambarı kimlik bilgileri aracılığıyla kümeye bağlamak için aşağıdaki adımları izleyin. 

### <a name="submit-job-to-hdinsight-cluster"></a>İşi HDInsight kümesine gönder

İş bir HDInsight kümesine gönderilirken, küme için yalnızca okuyucu rolüne sahipseniz kümeyi bağlamanız istenir. Ambarı kimlik bilgileri aracılığıyla kümeye bağlamak için aşağıdaki adımları izleyin. 

### <a name="link-to-cluster"></a>Kümeye bağla

1.  Ambarı Kullanıcı adını girin 
2.  Ambarı Kullanıcı parolasını girin.

   ![Visual Studio Code Kullanıcı adı için Spark & Hive araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-username.png)

   ![Visual Studio Code parola için Spark & Hive araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-password.png)

> [!NOTE]
>
>Spark/Hive kullanabilirsiniz: Bağlı kümeyi denetlemek için kümeyi listeleyin.
>
>![Visual Studio Code okuyucusu bağlantılı için Spark & Hive araçları](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="azure-data-lake-storage-gen2-adls-gen2"></a>Azure Data Lake Storage 2. (ADLS 2.)

### <a name="browse-an-adls-gen2-account"></a>ADLS 2. hesaba gözatıp

ADLS 2. bir hesabı genişletmek için Azure HDInsight Gezgini ' ne tıkladığınızda, Azure hesabınızın Gen2 depolamaya erişimi yoksa depolama **erişim anahtarını** girmeniz istenir. ADLS 2. hesap, erişim anahtarı başarıyla doğrulandıktan sonra otomatik olarak genişletilir. 

### <a name="submit-jobs-to-hdinsight-cluster-with-adls-gen2"></a>ADLS 2. ile iş HDInsight kümesine iş gönderme

ADLS 2. ile iş bir HDInsight kümesine gönderilirken, Azure hesabınızın Gen2 depolamaya yazma erişimi yoksa depolama **erişim anahtarını** girmeniz istenir.  Erişim anahtarı başarıyla doğrulandıktan sonra iş başarılı bir şekilde gönderilir. 

![Visual Studio Code AccessKey için Spark & Hive araçları](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-accesskey.png)   

> [!NOTE]
> 
>Depolama hesabı için erişim anahtarını Azure portal alabilirsiniz. Bilgi için bkz. [erişim anahtarlarını görüntüleme ve kopyalama](https://docs.microsoft.com/azure/storage/common/storage-account-manage#access-keys).

## <a name="unlink-cluster"></a>Kümenin bağlantısını kaldır

1. Menü çubuğundan **Görünüm** > **komut paleti...** ' a gidin ve Spark/Hive yazın **: Bir kümenin**bağlantısını kaldırın.  

2. Bağlantısını kaldırmak için kümeyi seçin.  

3. Doğrulama için **Çıkış** görünümünü gözden geçirin.  

## <a name="sign-out"></a>Oturumu kapat  

Menü çubuğundan **Görünüm** > **komut paleti...** ' a gidin ve Azure ' u girin **: Oturumu kapatın**.


## <a name="next-steps"></a>Sonraki adımlar
Visual Studio Code için Spark & Hive kullanma hakkında tanıtım videosu için bkz. [spark & Hive for Visual Studio Code](https://go.microsoft.com/fwlink/?linkid=858706)
