---
title: Azure HDInsight'ta Spark kümelerinde Jupyter dizüstü bilgisayar için çekirdekler
description: Azure HDInsight'ta Spark kümeleri ile kullanılabilen Jupyter dizüstü bilgisayar için PySpark, PySpark3 ve Spark çekirdekleri hakkında bilgi edinin.
keywords: kıvılcım üzerinde jupyter dizüstü bilgisayar, jupyter kıvılcım
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 03/20/2020
ms.openlocfilehash: a04b8fee31ffa5280bc8ad0fca35495bb87e0e8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064477"
---
# <a name="kernels-for-jupyter-notebook-on-apache-spark-clusters-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Spark kümelerinde Jupyter dizüstü bilgisayar için çekirdekler

HDInsight Spark kümeleri, uygulamalarınızı test etmek için [Apache Spark'taki](./apache-spark-overview.md) Jupyter dizüstü bilgisayarıyla kullanabileceğiniz çekirdekler sağlar. Çekirdek, kodunuzu çalıştıran ve yorumlayan bir programdır. Üç çekirdek leri şunlardır:

- **PySpark** - Python2 ile yazılmış uygulamalar için.
- **PySpark3** - Python3 ile yazılmış uygulamalar için.
- **Spark** - Scala yazılmış uygulamalar için.

Bu makalede, bu çekirdekleri nasıl kullanacağınızı ve bunları kullanmanın faydalarını öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

HDInsight'ta bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-on-spark-hdinsight"></a>Spark HDInsight'ta Bir Jupyter dizüstü bilgisayar oluşturma

1. Azure [portalından](https://portal.azure.com/)Kıvılcım kümenizi seçin.  Bkz. Yönergeler için [liste ve gösteri kümeleri.](../hdinsight-administer-use-portal-linux.md#showClusters) **Genel Bakış** görünümü açılır.

2. Genel **Bakış** görünümünden, **Küme panoları** **kutusunda, Jupyter not defterini**seçin. İstenirse, küme için yönetici kimlik bilgilerini girin.

    ![Apache Spark üzerinde Jupyter dizüstü bilgisayar](./media/apache-spark-jupyter-notebook-kernels/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Kıvılcım üzerinde Jupyter dizüstü bilgisayar")
  
   > [!NOTE]  
   > Ayrıca tarayıcınızda aşağıdaki URL'yi açarak Spark kümesindeki Jupyter dizüstü bilgisayarına da ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

3. **Yeni**'yi seçin ve ardından not defteri oluşturmak için **Pyspark**, **PySpark3**veya **Spark'ı** seçin. Scala uygulamaları için Spark çekirdeğini, Python2 uygulamaları için PySpark çekirdeğini ve Python3 uygulamaları için PySpark3 çekirdeğini kullanın.

    ![Kıvılcım jupyter dizüstü için Çekirdekler](./media/apache-spark-jupyter-notebook-kernels/kernel-jupyter-notebook-on-spark.png "Kıvılcım jupyter dizüstü için Çekirdekler")

4. Seçtiğiniz çekirdekle birlikte bir not defteri açılır.

## <a name="benefits-of-using-the-kernels"></a>Çekirdekleri kullanmanın faydaları

Burada Spark HDInsight kümeleri Jupyter dizüstü bilgisayar ile yeni çekirdekleri kullanarak birkaç faydaları vardır.

- **Önceden ayarlanmış bağlamlar.** **PySpark**, **PySpark3**veya **Spark** çekirdekleri ile, uygulamalarınızla çalışmaya başlamadan önce Spark veya Hive bağlamlarını açıkça ayarlamanız gerekmez. Bunlar varsayılan olarak kullanılabilir. Bu bağlamlar şunlardır:

  - **sc** - Kıvılcım bağlamı için
  - **sqlContext** - Kovan bağlamı için

    Bu nedenle, bağlamları ayarlamak için aşağıdaki gibi ifadeler çalıştırmak zorunda **değilsiniz:**

         sc = SparkContext('yarn-client')
         sqlContext = HiveContext(sc)

    Bunun yerine, doğrudan uygulamanızda önceden ayarlanmış bağlamları kullanabilirsiniz.

- **Hücre büyüleri.** PySpark çekirdeği, (örneğin,) `%%` `%%MAGIC` `<args>`çağırabileceğiniz özel komutlar olan bazı önceden tanımlanmış "büyüler" sağlar. Sihirli komut, bir kod hücresindeki ilk sözcük olmalı ve birden çok içerik satırına izin vermelidir. Sihirli kelime hücredeki ilk kelime olmalı. Büyüden önce bir şey eklemek, hatta yorumlar bile, bir hataya neden olur.     Büyüler hakkında daha fazla bilgi için [buraya](https://ipython.readthedocs.org/en/stable/interactive/magics.html)bakın.

    Aşağıdaki tabloçekirdekleri aracılığıyla mevcut farklı büyüleri listeler.

   | Sihirli | Örnek | Açıklama |
   | --- | --- | --- |
   | Yardım |`%%help` |Örnek ve açıklama ile tüm mevcut büyübir tablo oluşturur |
   | bilgiler |`%%info` |Geçerli Livy bitiş noktası için çıktıoturumu bilgileri |
   | Yapılandırmak |`%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} |Oturum oluşturmak için parametreleri yapılandırır. Bir oturum zaten oluşturulduysa kuvvet bayrağı (-f) zorunludur, bu da oturumun bırakıldığını ve yeniden oluşturulmasını sağlar. Geçerli parametrelerin bir listesi için [Livy's POST /sessions İstek Gövdesi'ne](https://github.com/cloudera/livy#request-body) bakın. Parametreler json dizesi olarak geçirilmeli ve örnek sütunda gösterildiği gibi büyüden sonra bir sonraki satırda olmalıdır. |
   | sql |`%%sql -o <variable name>`<br> `SHOW TABLES` |sqlContext'a karşı bir Hive sorgusu yürütür. `-o` Parametre geçirilirse, sorgunun sonucu [Pandalar](https://pandas.pydata.org/) veri çerçevesi olarak %yerel Python bağlamında devam eder. |
   | yerel |`%%local`<br>`a=1` |Sonraki satırlarda tüm kod yerel olarak yürütülür. Kod, kullandığınız çekirdekten bağımsız olarak bile geçerli Python2 kodu olmalıdır. Bu nedenle, not defterini oluştururken **PySpark3** veya **Spark** çekirdeklerini `%%local` seçmiş olsanız bile, bir hücrede sihri kullanıyorsanız, o hücrenin yalnızca geçerli Python2 kodu na sahip olması gerekir. |
   | günlükler |`%%logs` |Geçerli Livy oturumuiçin günlükleri çıktıları. |
   | delete |`%%delete -f -s <session number>` |Geçerli Livy bitiş noktasının belirli bir oturumunu siler. Çekirdeğin kendisi için başlatılan oturumu silemezsiniz. |
   | temizle |`%%cleanup -f` |Bu not defterinin oturumu da dahil olmak üzere geçerli Livy bitiş noktasının tüm oturumlarını siler. Kuvvet bayrağı -f zorunludur. |

   > [!NOTE]  
   > PySpark çekirdeği tarafından eklenen büyülere ek olarak, [dahili IPython büyülerini](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics)de `%%sh`kullanabilirsiniz. Küme başlığı `%%sh` üzerinde komut dosyaları ve kod bloğu çalıştırmak için sihirli kullanabilirsiniz.

- **Otomatik görselleştirme**. Pyspark çekirdeği Hive ve SQL sorgularının çıktısını otomatik olarak görselleştirir. Tablo, Pasta, Çizgi, Alan, Çubuk gibi birkaç farklı görseltür arasında seçim yapabilirsiniz.

## <a name="parameters-supported-with-the-sql-magic"></a>%sql büyüsü ile desteklenen parametreler

Büyü, `%%sql` sorguları çalıştırdığınızda aldığınız çıktı türünü denetlemek için kullanabileceğiniz farklı parametreleri destekler. Aşağıdaki tabloçıktıyı listeler.

| Parametre | Örnek | Açıklama |
| --- | --- | --- |
| -o |`-o <VARIABLE NAME>` |Sorgunun sonucunu % yerel Python bağlamında [Pandalar](https://pandas.pydata.org/) veri çerçevesi olarak sürdürmek için bu parametreyi kullanın. Veri çerçevesi değişkeninin adı belirttiğiniz değişken adıdır. |
| -q |`-q` |Hücrenin görselöğelerini kapatmak için bunu kullanın. Bir hücrenin içeriğini otomatik görselleştirmek istemiyorsanız ve yalnızca bir veri çerçevesi olarak yakalamak `-q -o <VARIABLE>`istiyorsanız, bunu kullanın. Sonuçları yakalamadan görselleştirmeleri kapatmak istiyorsanız (örneğin, `CREATE TABLE` bir deyim gibi bir SQL sorgusu `-q` çalıştırmak için), `-o` bağımsız değişken belirtmeden kullanın. |
| -m |`-m <METHOD>` |**YÖNTEM'in** **aldığı** veya **örneklendiği** yer (varsayılan **kabul**dür). Yöntem **alıyorsa,** çekirdek, MAXROWS tarafından belirtilen sonuç veri kümesinin üstünden öğeleri alır (bu tabloda daha sonra açıklanmıştır). Yöntem **örnek**ise, çekirdek bu tabloda sonraki açıklanan parametreye `-r` göre veri kümesinin öğelerini rasgele örnekler. |
| -r |`-r <FRACTION>` |Burada **KESIR** 0.0 ile 1.0 arasında kayan nokta sayısıdır. SQL sorgusu için örnek yöntem `sample`ise, çekirdek rasgele sizin için belirlenen sonuç öğelerinin belirtilen kısmını örnekler. Örneğin, bağımsız değişkenlerle `-m sample -r 0.01`bir SQL sorgusu çalıştırırsanız, sonuç satırlarının %1'i rasgele örneklenir. |
| -n |`-n <MAXROWS>` |**MAXROWS** bir gerçek değeridir. Çekirdek, çıkış satırlarının sayısını **MAXROWS**ile sınırlar. **MAXROWS** **-1**gibi negatif bir sayıise, sonuç kümesindeki satır sayısı sınırlı değildir. |

**Örnek:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2
    SELECT * FROM hivesampletable

Yukarıdaki ifade aşağıdakileri yapar:

- **Kovan örnekleme tablosundan**tüm kayıtları seçer.
- -q kullandığımız için otomatik görselleştirmeyi kapatır.
- Çünkü biz `-m sample -r 0.1 -n 500` rasgele kovan örnekleme tablosunda satırların% 10 örnekleri ve 500 satır olarak belirlenen sonuç boyutunu sınırlar kullanın.
- Son olarak, `-o query2` kullandığımız için de **query2**adlı bir veri çerçevesi içine çıktı kaydeder.

## <a name="considerations-while-using-the-new-kernels"></a>Yeni çekirdekleri kullanırken dikkat edilmesi gerekenler

Hangi çekirdeği kullanırsanız kullanın, not defterlerinin çalışmasını bırakmak küme kaynaklarını tüketir.  Bu çekirdeklerle, bağlamlar önceden ayarlandığı için, not defterlerinden çıkmak bağlamı öldürmez ve bu nedenle küme kaynakları kullanılmaya devam eder. İyi bir uygulama, not defterini kullanmayı bitirdiğinizde not defterinin **Dosya** menüsünden **Kapat ve Durdur** seçeneğini kullanmaktır, bu da bağlamı öldürür ve not defterinden çıkar.

## <a name="where-are-the-notebooks-stored"></a>Defterler nerede saklanıyor?

Kümeniz varsayılan depolama hesabı olarak Azure Depolama'yı kullanıyorsa, Jupyter dizüstü bilgisayarlar **/HdiNotebooks** klasörü altında depolama hesabına kaydedilir.  Jupyter içinden oluşturduğunuz not defterlerine, metin dosyalarına ve klasörlere depolama hesabından erişilebilir.  Örneğin, bir **klasör myfolder** ve bir not defteri **myfolder/mynotebook.ipynb**oluşturmak için Jupyter kullanırsanız, depolama hesabı `/HdiNotebooks/myfolder/mynotebook.ipynb` içinde bu not defterierişebilirsiniz.  Bunun tersi de doğrudur, yani bir not defterini doğrudan `/HdiNotebooks/mynotebook1.ipynb`depolama hesabınıza yüklerseniz, not defteri Jupyter'dan da görülebilir.  Küme silindikten sonra bile not defterleri depolama hesabında kalır.

> [!NOTE]  
> Varsayılan depolama alanı olarak Azure Veri Gölü Depolama'ya sahip HDInsight kümeleri not defterlerini ilişkili depolama alanında depolamaz.

Dizüstü bilgisayarların depolama hesabına kaydedilebiş şekli [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html)ile uyumludur. Bu nedenle, kümeye SSH girerseniz, aşağıdaki parçacıkta gösterildiği gibi dosya yönetimi komutlarını kullanabilirsiniz:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                   # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it's visible from Jupyter

Kümenin varsayılan depolama hesabı olarak Azure Depolama'yı mı yoksa Azure Veri Gölü Depolaması'nı `/var/lib/jupyter`mı kullandığına bakılmaksızın, not defterleri de 'deki küme başlığına kaydedilir.

## <a name="supported-browser"></a>Desteklenen tarayıcı

Spark HDInsight kümelerinde yer alan jupyter dizüstü bilgisayarlar yalnızca Google Chrome'da desteklenir.

## <a name="feedback"></a>Geri Bildirim

Yeni çekirdekler gelişen aşamasında ve zaman içinde olgun olacaktır. Bu aynı zamanda API'ler bu çekirdekler olgunlaştıkça değişebilir anlamına da gelebilir. Bu yeni çekirdekleri kullanırken sahip olduğunuz herhangi bir geri bildirim için minnettarız. Bu çekirdeklerin son sürümü şekillendirmede yararlıdır. Yorumlarınızı/geri bildirimlerinizi bu makalenin altındaki **Geri Bildirim** bölümüne bırakabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

- [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar

- [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi yapın](apache-spark-use-bi-tools.md)
- [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
- [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-machine-learning-mllib-ipython.md)
- [HDInsight'ta Apache Spark kullanarak web sitesi günlük analizi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma

- [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
- [Apache Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

- [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
- [Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ IDEA için HDInsight Araçları Eklentisini kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
- [HDInsight'ta Apache Spark kümesine sahip Apache Zeppelin dizüstü bilgisayarları kullanma](apache-spark-zeppelin-notebook.md)
- [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
- [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme

- [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
- [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
