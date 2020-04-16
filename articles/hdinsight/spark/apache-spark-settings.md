---
title: Kıvılcım ayarlarını yapılandırma - Azure HDInsight
description: Azure HDInsight kümesi için Apache Spark ayarlarını görüntüleme ve yapılandırma
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/15/2020
ms.openlocfilehash: e13390067f8767e8c07b9c189264444e6d999a7a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81411293"
---
# <a name="configure-apache-spark-settings"></a>Apache Spark ayarlarını yapılandırma

HDInsight Spark kümesi, [Apache Spark](https://spark.apache.org/) kitaplığı yüklemesini içerir.  Her HDInsight kümesi, Spark da dahil olmak üzere tüm yüklü hizmetleri için varsayılan yapılandırma parametrelerini içerir.  HDInsight Apache Hadoop kümesini yönetmenin önemli bir yönü, Spark Jobs da dahil olmak üzere iş yükünü izlemektir. Spark işlerini en iyi çalıştırmak için kümenin mantıksal yapılandırmasını belirlerken fiziksel küme yapılandırmasını göz önünde bulundurun.

Varsayılan HDInsight Apache Spark kümesi aşağıdaki düğümleri içerir: üç [Apache ZooKeeper](https://zookeeper.apache.org/) düğümleri, iki baş düğümleri ve bir veya daha fazla işçi düğümleri:

![Spark HDInsight Mimarisi](./media/apache-spark-settings/spark-hdinsight-arch.png)

HDInsight kümenizdeki düğümler için VM ve VM boyutlarının sayısı Spark yapılandırmanızı etkileyebilir. Varsayılan olmayan HDInsight yapılandırma değerleri genellikle varsayılan olmayan Spark yapılandırma değerlerini gerektirir. Bir HDInsight Spark kümesi oluşturduğunuzda, bileşenlerin her biri için önerilen VM boyutları gösterilir. Şu anda Azure için [Bellek için optimize edilmiş Linux VM boyutları](../../virtual-machines/linux/sizes-memory.md) D12 v2 veya daha büyüktür.

## <a name="apache-spark-versions"></a>Apache Spark sürümleri

Kümeniz için en iyi Spark sürümünü kullanın.  HDInsight hizmeti, hem Spark hem de HDInsight'ın çeşitli sürümlerini içerir.  Spark'ın her sürümü varsayılan küme ayarları kümesini içerir.  

Yeni bir küme oluşturduğunuzda, aralarından seçim yapabileceğiniz birden çok Spark sürümü vardır. Tam listeyi görmek için, [HDInsight Bileşenleri ve Sürümleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning).

> [!NOTE]  
> HDInsight hizmetindeki Apache Spark'ın varsayılan sürümü önceden haber verilmeden değişebilir. Sürüm bağımlılığınız varsa, Microsoft .NET SDK, Azure PowerShell ve Azure Classic CLI kullanarak kümeler oluştururken bu sürümü belirtmenizi önerir.

Apache Spark üç sistem yapılandırma konumuvardır:

* Kıvılcım özellikleri çoğu uygulama parametrelerini denetler `SparkConf` ve bir nesne kullanılarak veya Java sistem özellikleri aracılığıyla ayarlanabilir.
* Ortam değişkenleri, her düğümdeki `conf/spark-env.sh` komut dosyası aracılığıyla IP adresi gibi makine başına ayarları ayarlamak için kullanılabilir.
* Günlüğe kaydetme yoluyla `log4j.properties`yapılandırılabilir.

Spark'ın belirli bir sürümünü seçtiğinizde, kümeniz varsayılan yapılandırma ayarlarını içerir.  Özel bir Spark yapılandırma dosyası kullanarak varsayılan Spark yapılandırma değerlerini değiştirebilirsiniz.  Aşağıda bir örnek gösterilmiştir.

```
spark.hadoop.io.compression.codecs org.apache.hadoop.io.compress.GzipCodec
spark.hadoop.mapreduce.input.fileinputformat.split.minsize 1099511627776
spark.hadoop.parquet.block.size 1099511627776
spark.sql.files.maxPartitionBytes 1099511627776
spark.sql.files.openCostInBytes 1099511627776
```

Yukarıda gösterilen örnek, beş Spark yapılandırma paramı için birkaç varsayılan değeri geçersiz kılar.  Bu değerler sıkıştırma codec, Apache Hadoop MapReduce bölünmüş minimum boyut ve parke bloğu boyutları vardır. Ayrıca, Spar SQL bölümü ve açık dosya boyutları varsayılan değerleri.  İlişkili veri ve işlerin (bu örnekte genomik veriler) belirli özelliklere sahip olması nedeniyle bu yapılandırma değişiklikleri seçilir. Bu özellikler, bu özel yapılandırma ayarlarını kullanarak daha iyi olacaktır.

---

## <a name="view-cluster-configuration-settings"></a>Küme yapılandırma ayarlarını görüntüleme

Kümede performans optimizasyonu yapmadan önce geçerli HDInsight küme yapılandırma ayarlarını doğrulayın. Spark küme bölmesindeki **Pano** bağlantısını tıklatarak Azure portalından HDInsight Panosu'nu başlatın. Küme yöneticisinin kullanıcı adı ve parolasıyla oturum açın.

Apache Ambari Web UI, anahtar küme kaynak kullanım ölçümlerinin bir panosuyla görüntülenir.  Ambari Panosu size Apache Spark yapılandırmasını ve diğer yüklü hizmetleri gösterir. Pano, Spark da dahil olmak üzere yüklü hizmetlere ait bilgileri görüntülediğiniz bir **Config Geçmişi** sekmesi içerir.

Apache Spark için yapılandırma değerlerini görmek için **Config History'yi**seçin ve ardından **Spark2'yi**seçin.  **Configs** sekmesini seçin ve `Spark` ardından `Spark2`servis listesindeki (veya sürüme bağlı olarak) bağlantısını seçin.  Kümeniz için yapılandırma değerlerinin listesini görürsünüz:

![Kıvılcım Yapılandırmaları](./media/apache-spark-settings/spark-configurations.png)

Tek tek Spark yapılandırma değerlerini görmek ve değiştirmek için başlıkta "kıvılcım" içeren herhangi bir bağlantı seçin.  Spark için yapılandırmalar, bu kategorilerde hem özel hem de gelişmiş yapılandırma değerlerini içerir:

* Özel Spark2-varsayılanlar
* Özel Spark2-metrics-özellikleri
* Gelişmiş Spark2 varsayılanları
* Gelişmiş Spark2-env
* Gelişmiş spark2-kovan-site-geçersiz kılma

Varsayılan olmayan bir yapılandırma değerleri kümesi oluşturursanız, güncelleştirme geçmişiniz görünür.  Bu yapılandırma geçmişi, hangi varsayılan olmayan yapılandırmanın en iyi performansa sahip olduğunu görmek için yararlı olabilir.

> [!NOTE]  
> Ortak Spark küme yapılandırma ayarlarını görmek, ancak değiştirmek için üst düzey **Spark İş Arabirimi** arabiriminde **Çevre** sekmesini seçin.

## <a name="configuring-spark-executors"></a>Kıvılcım uygulayıcıları yapılandırma

Aşağıdaki diyagram anahtar Kıvılcım nesnelerini gösterir: sürücü programı ve ilişkili Spark Bağlamı ve küme yöneticisi ve *onun n* alt düğümleri.  Her alt düğüm bir Yürütücü, önbellek ve *n* görev örnekleri içerir.

![Nesneleri kümele](./media/apache-spark-settings/hdi-spark-architecture.png)

Kıvılcım işleri, özellikle bellek olmak üzere alt kaynakları kullanır, bu nedenle çalışan düğüm Uygulayıcıları için Spark yapılandırma değerlerini ayarlamak yaygındır.

Genellikle uygulama gereksinimlerini geliştirmek için Spark yapılandırmaları ayarlamak için `spark.executor.instances` `spark.executor.cores`ayarlanmış `spark.executor.memory`üç önemli parametre , ve . Uygulayıcı, Bir Kıvılcım uygulaması için başlatılan bir işlemdir. Bir Uygulayıcı işçi düğümüüzerinde çalışır ve uygulama nın görevlerden sorumludur. İşçi düğümlerinin sayısı ve alt düğüm boyutu, uygulayıcıların sayısını ve uygulayıcı boyutlarını belirler. Bu değerler küme `spark-defaults.conf` baş düğümlerinde depolanır.  Ambari web Kullanıcı Aracı'nda Özel **kıvılcım varsayılanlarını** seçerek bu değerleri çalışan kümede düzenleme yapabilirsiniz.  Değişiklik yaptıktan sonra, UI tarafından etkilenen tüm hizmetleri **yeniden başlatmanız** istenir.

> [!NOTE]  
> Bu üç yapılandırma paramı küme düzeyinde (kümeüzerinde çalışan tüm uygulamalar için) ve her bir uygulama için de belirtilmiş olarak yapılandırılabilir.

Spark Uygulayıcıları tarafından kullanılan kaynaklar la ilgili bir diğer bilgi kaynağı da Spark Application UI'dir.  UI'de, Uygulayıcılar yapılandırmanın ve tüketilen kaynakların Özet ve Ayrıntı **görünümlerini** görüntüler.  Tüm küme için yürütme değerlerini mi yoksa belirli iş yürütme leri kümesini mi değiştireceğini belirleyin.

![Kıvılcım Uygulayıcıları](./media/apache-spark-settings/apache-spark-executors.png)

Veya HDInsight ve Spark küme yapılandırma ayarlarını programlı olarak doğrulamak için Ambari REST API'yi kullanabilirsiniz.  Daha fazla bilgi [GitHub üzerinde Apache Ambari API referans](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)mevcuttur.

Spark iş yükünüze bağlı olarak, varsayılan dışındaki bir Spark yapılandırmasının daha iyi Spark iş yürütmeleri sağladığına karar verebilirsiniz.  Varsayılan olmayan küme yapılandırmalarını doğrulamak için örnek iş yükleriyle kıyaslama testi yapın.  Ayarlamayı düşünebileceğiniz yaygın parametrelerden bazıları:

|Parametre |Açıklama|
|---|---|
|--num-uygulayıcılar|Uygulayıcıların sayısını ayarlar.|
|--executor-cores|Her uygulayıcı için çekirdek sayısını ayarlar. Diğer işlemler de kullanılabilir belleğin bir kısmını tükettiği için orta ölçekli yürütücüler kullanmanızı öneririz.|
|--yürütme-bellek|[Apache Hadoop İpn'deki](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html)her uygulayıcının bellek boyutunu (yığın boyutunu) denetler ve yürütme yükü için bazı bellek bırakmanız gerekir.|

Aşağıda, farklı yapılandırma değerlerine sahip iki alt düğüm örneği verilmiştir:

![İki düğüm yapılandırması](./media/apache-spark-settings/executor-configuration.png)

Aşağıdaki liste anahtar Spark yürütücü bellek parametrelerini gösterir.

|Parametre |Açıklama|
|---|---|
|kıvılcım.executor.memory|Bir uygulayıcı için kullanılabilen toplam bellek miktarını tanımlar.|
|kıvılcım.depolama.memoryFraction|(varsayılan ~60%) kalıcı RDD'leri depolamak için kullanılabilir bellek miktarını tanımlar.|
|spark.shuffle.memoryFraction|(varsayılan ~20%) karıştırma için ayrılmış bellek miktarını tanımlar.|
|spark.storage.unrollFraction ve spark.storage.safetyFraction|(toplam belleğin ~%30'u) - bu değerler Spark tarafından dahili olarak kullanılır ve değiştirilmemelidir.|

YARN, her Kıvılcım düğümündeki kapsayıcılar tarafından kullanılan maksimum bellek toplamını denetler. Aşağıdaki diyagram, İplik yapılandırma nesneleri ile Kıvılcım nesneleri arasındaki düğüm başına ilişkileri gösterir.

![İplik Kıvılcım Bellek Yönetimi](./media/apache-spark-settings/hdi-yarn-spark-memory.png)

## <a name="change-parameters-for-an-application-running-in-jupyter-notebook"></a>Jupyter dizüstü bilgisayarda çalışan bir uygulama için parametreleri değiştirme

HDInsight'taki kıvılcım kümeleri varsayılan olarak bir dizi bileşen içerir. Bu bileşenlerin her biri, gerektiğinde geçersiz kılınabilen varsayılan yapılandırma değerlerini içerir.

|Bileşen |Açıklama|
|---|---|
|Kıvılcım Çekirdek|Spark Core, Spark SQL, Spark akış API'leri, GraphX ve Apache Spark MLlib.|
|Anaconda|Bir python paket yöneticisi.|
|[Apaçi Livy](https://livy.incubator.apache.org/)|Apache Spark REST API, bir HDInsight Spark kümesine uzak işleri göndermek için kullanılır.|
|[Jupyter](https://jupyter.org/) ve [Apache Zeppelin](https://zeppelin.apache.org/) dizüstü bilgisayarlar|Spark kümenizle etkileşim kurmak için etkileşimli tarayıcı tabanlı ui.|
|ODBC sürücüsü|HDInsight'taki Spark kümelerini Microsoft Power BI ve Tableau gibi iş zekası (BI) araçlarına bağlar.|

Jupyter not defterinde çalışan uygulamalar `%%configure` için, not defterinin içinden yapılandırma değişiklikleri yapmak için komutu kullanın. Bu yapılandırma değişiklikleri, not defteri örneğinizden çalıştırılan Kıvılcım işlerine uygulanır. İlk kod hücrenizi çalıştırmadan önce, uygulamanın başında bu tür değişiklikler yapın. Değiştirilen yapılandırma oluşturulduğunda Livy oturumuna uygulanır.

> [!NOTE]  
> Uygulamanın sonraki bir aşamasında yapılandırmayı değiştirmek `-f` için (kuvvet) parametresini kullanın. Ancak, uygulamada tüm ilerleme kaybolur.

Aşağıdaki kod, Jupyter dizüstü bilgisayarında çalışan bir uygulamanın yapılandırmasını nasıl değiştireceğini gösterir.

```
%%configure
{"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}
```

## <a name="conclusion"></a>Sonuç

Spark işlerinizin öngörülebilir ve performanslı bir şekilde çalışmasını sağlamak için temel yapılandırma ayarlarını izleyin. Bu ayarlar, belirli iş yüklerinizle ilgili en iyi Spark küme yapılandırmasını belirlemeye yardımcı olur.  Ayrıca, uzun süren ve kaynak tüketen Spark iş yürütmelerinin yürütülmesini izlemeniz gerekir.  En yaygın zorluklar, yanlış boyutlandırılmış yürütücüler gibi yanlış yapılandırmalardan gelen bellek baskısı etrafında merkezlenir. Ayrıca, uzun süren işlemler ve kartezyen işlemleri ile sonuçlanan görevler.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Hadoop bileşenleri ve HDInsight ile kullanılabilir sürümleri?](../hdinsight-component-versioning.md)
* [HDInsight'ta bir Apache Spark kümesinin kaynaklarını yönetme](apache-spark-resource-manager.md)
* [Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight'ta küme oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
* [Apache Kıvılcım Yapılandırması](https://spark.apache.org/docs/latest/configuration.html)
* [Apache Hadoop İplik üzerinde Çalışan Apache Kıvılcım](https://spark.apache.org/docs/latest/running-on-yarn.html)
