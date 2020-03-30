---
title: Kıvılcım ayarlarını yapılandırma - Azure HDInsight
description: Azure HDInsight kümesi için Apache Spark ayarlarını görüntüleme ve yapılandırma
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/17/2019
ms.openlocfilehash: 48f19e5da8c7703cc597518246c2f62ebce3ae17
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272025"
---
# <a name="configure-apache-spark-settings"></a>Apache Spark ayarlarını yapılandırma

HDInsight Spark kümesi, [Apache Spark](https://spark.apache.org/) kitaplığı yüklemesini içerir.  Her HDInsight kümesi, Spark da dahil olmak üzere tüm yüklü hizmetleri için varsayılan yapılandırma parametrelerini içerir.  HDInsight Apache Hadoop kümesini yönetmenin önemli bir yönü, işlerin öngörülebilir bir şekilde çalıştığından emin olmak için Spark Jobs da dahil olmak üzere iş yükünü izlemektir. Spark işlerini en iyi çalıştırmak için, kümenin mantıksal yapılandırmasını en iyi duruma getirmenin nasıl yapılacağını belirlerken fiziksel küme yapılandırmasını göz önünde bulundurun.

Varsayılan HDInsight Apache Spark kümesi aşağıdaki düğümleri içerir: üç [Apache ZooKeeper](https://zookeeper.apache.org/) düğümleri, iki baş düğümleri ve bir veya daha fazla işçi düğümleri:

![Spark HDInsight Mimarisi](./media/apache-spark-settings/spark-hdinsight-arch.png)

HDInsight kümenizdeki düğümler için VM sayısı ve VM boyutları da Spark yapılandırmanızı etkileyebilir. Varsayılan olmayan HDInsight yapılandırma değerleri genellikle varsayılan olmayan Spark yapılandırma değerlerini gerektirir. Bir HDInsight Spark kümesi oluşturduğunuzda, bileşenlerin her biri için önerilen VM boyutları gösterilir. Şu anda Azure için [Bellek için optimize edilmiş Linux VM boyutları](../../virtual-machines/linux/sizes-memory.md) D12 v2 veya daha büyüktür.

## <a name="apache-spark-versions"></a>Apache Spark sürümleri

Kümeniz için en iyi Spark sürümünü kullanın.  HDInsight hizmeti, hem Spark hem de HDInsight'ın çeşitli sürümlerini içerir.  Spark'ın her sürümü varsayılan küme ayarları kümesini içerir.  

Yeni bir küme oluşturduğunuzda, aralarından seçim yapabileceğiniz birden çok Spark sürümü vardır. Tam listeyi görmek [için, HDInsight Bileşenleri ve Sürümleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning)


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

Yukarıda gösterilen örnek, beş Spark yapılandırma paramı için birkaç varsayılan değeri geçersiz kılar.  Bu sıkıştırma codec, Apache Hadoop MapReduce bölünmüş minimum boyut ve parke bloğu boyutları ve aynı zamanda Spar SQL bölümü ve açık dosya boyutları varsayılan değerlerivardır.  İlişkili veriler ve işler (bu örnekte genomik veriler) bu özel yapılandırma ayarlarını kullanarak daha iyi performans gösteren belirli özelliklere sahip olduğundan, bu yapılandırma değişiklikleri seçilir.

---

## <a name="view-cluster-configuration-settings"></a>Küme yapılandırma ayarlarını görüntüleme

Kümede performans optimizasyonu gerçekleştirmeden önce geçerli HDInsight küme yapılandırma ayarlarını doğrulayın. Spark küme bölmesindeki **Pano** bağlantısını tıklatarak Azure portalından HDInsight Panosu'nu başlatın. Küme yöneticisinin kullanıcı adı ve parolasıyla oturum açın.

Apache Ambari Web UI, anahtar küme kaynak kullanım ölçümlerinin pano görünümüyle görüntülenir.  Ambari Panosu size Apache Spark yapılandırmasını ve yüklediğiniz diğer hizmetleri gösterir. Pano, Spark da dahil olmak üzere tüm yüklü hizmetlerin yapılandırma bilgilerini görüntüleyebileceğiniz bir **Config History** sekmesi içerir.

Apache Spark için yapılandırma değerlerini görmek için **Config History'yi**seçin ve ardından **Spark2'yi**seçin.  **Configs** sekmesini seçin ve `Spark` ardından `Spark2`servis listesindeki (veya sürüme bağlı olarak) bağlantısını seçin.  Kümeniz için yapılandırma değerlerinin listesini görürsünüz:

![Kıvılcım Yapılandırmaları](./media/apache-spark-settings/spark-configurations.png)

Tek tek Spark yapılandırma değerlerini görmek ve değiştirmek için bağlantı başlığında "kıvılcım" sözcüğünün olduğu herhangi bir bağlantı seçin.  Spark için yapılandırmalar, bu kategorilerde hem özel hem de gelişmiş yapılandırma değerlerini içerir:

* Özel Spark2-varsayılanlar
* Özel Spark2-metrics-özellikleri
* Gelişmiş Spark2 varsayılanları
* Gelişmiş Spark2-env
* Gelişmiş spark2-kovan-site-geçersiz kılma

Varsayılan olmayan bir yapılandırma değerleri kümesi oluşturursanız, yapılandırma güncelleştirmelerinizin geçmişini de görebilirsiniz.  Bu yapılandırma geçmişi, hangi varsayılan olmayan yapılandırmanın en iyi performansa sahip olduğunu görmek için yararlı olabilir.

> [!NOTE]  
> Ortak Spark küme yapılandırma ayarlarını görmek, ancak değiştirmek için üst düzey **Spark İş Arabirimi** arabiriminde **Çevre** sekmesini seçin.

## <a name="configuring-spark-executors"></a>Kıvılcım uygulayıcıları yapılandırma

Aşağıdaki diyagram anahtar Kıvılcım nesnelerini gösterir: sürücü programı ve ilişkili Spark Bağlamı ve küme yöneticisi ve *onun n* alt düğümleri.  Her alt düğüm bir Yürütücü, önbellek ve *n* görev örnekleri içerir.

![Nesneleri kümele](./media/apache-spark-settings/hdi-spark-architecture.png)

Kıvılcım işleri, özellikle bellek olmak üzere alt kaynakları kullanır, bu nedenle çalışan düğüm Uygulayıcıları için Spark yapılandırma değerlerini ayarlamak yaygındır.

Genellikle uygulama gereksinimlerini geliştirmek için Spark yapılandırmaları ayarlamak için `spark.executor.instances` `spark.executor.cores`ayarlanmış `spark.executor.memory`üç önemli parametre , ve . Uygulayıcı, Bir Kıvılcım uygulaması için başlatılan bir işlemdir. Bir Uygulayıcı işçi düğümüüzerinde çalışır ve uygulama nın görevlerden sorumludur. Her küme için, varsayılan uygulayıcı sayısı ve uygulayıcı boyutları, alt düğüm sayısı ve alt düğüm boyutuna göre hesaplanır. Bunlar küme kafa `spark-defaults.conf` düğümlerinde depolanır.  Ambari web Kullanıcı Aracı'ndaki **Özel kıvılcım varsayılanları** bağlantısını seçerek bu değerleri çalışan kümede düzenleme yapabilirsiniz.  Değişiklik yaptıktan sonra, UI tarafından etkilenen tüm hizmetleri **yeniden başlatmanız** istenir.

> [!NOTE]  
> Bu üç yapılandırma paramı küme düzeyinde (kümeüzerinde çalışan tüm uygulamalar için) ve her bir uygulama için de belirtilmiş olarak yapılandırılabilir.

Spark Yürütücüleri tarafından kullanılan kaynaklar hakkında bir diğer bilgi kaynağı Spark Uygulaması Kullanıcı Arabirimidir.  Spark UI'de, **uygulayıcılar** tarafından tüketilen yapılandırma ve kaynakların Özet ve Ayrıntı görünümlerini görüntülemek için Uygulayıcılar sekmesini seçin.  Bu görünümler varsayılan değerleri kümenin tamamında Spark yürütücüleri için mi yoksa belirli bir iş yürütmeleri kümesi için mi değiştireceğinizi saptamanıza yardımcı olabilir.

![Kıvılcım Uygulayıcıları](./media/apache-spark-settings/apache-spark-executors.png)

Alternatif olarak, HDInsight ve Spark küme yapılandırma ayarlarını programlı olarak doğrulamak için Ambari REST API'yi kullanabilirsiniz.  Daha fazla bilgi [GitHub üzerinde Apache Ambari API referans](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)mevcuttur.

Spark iş yükünüze bağlı olarak, varsayılan dışındaki bir Spark yapılandırmasının daha iyi Spark iş yürütmeleri sağladığına karar verebilirsiniz.  Varsayılan dışındaki küme yapılandırmalarının doğrulanması için örnek iş yükleriyle karşılaştırmalı testler yapmalısınız.  Ayarlamayı düşünebileceğiniz yaygın parametrelerden bazıları:

* `--num-executors`uygulayıcıların sayısını ayarlar.
* `--executor-cores`her uygulayıcı için çekirdek sayısını ayarlar. Diğer işlemler de kullanılabilir belleğin bir kısmını tükettiği için orta ölçekli yürütücüler kullanmanızı öneririz.
* `--executor-memory`[Apache Hadoop İpn](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html)her uygulayıcısının bellek boyutunu (yığın boyutu) denetler ve yürütme yükü için bazı bellek bırakmak gerekir.

Aşağıda, farklı yapılandırma değerlerine sahip iki alt düğüm örneği verilmiştir:

![İki düğüm yapılandırması](./media/apache-spark-settings/executor-configuration.png)

Aşağıdaki liste anahtar Spark yürütücü bellek parametrelerini gösterir.

* `spark.executor.memory`bir uygulayıcı için kullanılabilir toplam bellek miktarını tanımlar.
* `spark.storage.memoryFraction`(varsayılan ~60%) kalıcı RDD'leri depolamak için kullanılabilir bellek miktarını tanımlar.
* `spark.shuffle.memoryFraction`(varsayılan ~20%) karıştırma için ayrılmış bellek miktarını tanımlar.
* `spark.storage.unrollFraction`ve `spark.storage.safetyFraction` (toplam belleğin ~%30'u) - bu değerler Spark tarafından dahili olarak kullanılır ve değiştirilmemelidir.

YARN, her Kıvılcım düğümündeki kapsayıcılar tarafından kullanılan maksimum bellek toplamını denetler. Aşağıdaki diyagram, İplik yapılandırma nesneleri ile Kıvılcım nesneleri arasındaki düğüm başına ilişkileri gösterir.

![İplik Kıvılcım Bellek Yönetimi](./media/apache-spark-settings/hdi-yarn-spark-memory.png)

## <a name="change-parameters-for-an-application-running-in-jupyter-notebook"></a>Jupyter dizüstü bilgisayarda çalışan bir uygulama için parametreleri değiştirme

HDInsight'taki kıvılcım kümeleri varsayılan olarak bir dizi bileşen içerir. Bu bileşenlerin her biri, gerektiğinde geçersiz kılınabilen varsayılan yapılandırma değerlerini içerir.

* Spark Core - Spark Core, Spark SQL, Spark streaming API'leri, GraphX ve Apache Spark MLlib.
* Anaconda - bir python paket yöneticisi.
* [Apache Livy - Apache](https://livy.incubator.apache.org/) Spark REST API, bir HDInsight Spark kümesine uzak işleri göndermek için kullanılır.
* [Jupyter](https://jupyter.org/) ve [Apache Zeppelin](https://zeppelin.apache.org/) dizüstü bilgisayarlar - Spark kümenizle etkileşim kurmak için etkileşimli tarayıcı tabanlı ui.
* ODBC sürücüsü - HDInsight'taki Kıvılcım kümelerini Microsoft Power BI ve Tableau gibi iş zekası (BI) araçlarına bağlar.

Jupyter not defterinde çalışan uygulamalar `%%configure` için, not defterinin içinden yapılandırma değişiklikleri yapmak için komutu kullanın. Bu yapılandırma değişiklikleri, not defteri örneğinizden çalıştırılan Kıvılcım işlerine uygulanır. İlk kod hücrenizi çalıştırmadan önce, uygulamanın başında bu tür değişiklikler yapmalısınız. Değiştirilen yapılandırma oluşturulduğunda Livy oturumuna uygulanır.

> [!NOTE]  
> Uygulamanın sonraki bir aşamasında yapılandırmayı değiştirmek `-f` için (kuvvet) parametresini kullanın. Ancak, uygulamada tüm ilerleme kaybolur.

Aşağıdaki kod, Jupyter dizüstü bilgisayarında çalışan bir uygulamanın yapılandırmasını nasıl değiştireceğini gösterir.

```
%%configure
{"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}
```

## <a name="conclusion"></a>Sonuç

Spark işlerinizin öngörülebilir ve performanslı bir şekilde çalışmasını sağlamak için izlemeniz ve ayarlamanız gereken birkaç temel yapılandırma ayarı vardır. Bu ayarlar, belirli iş yüklerinizle ilgili en iyi Spark küme yapılandırmasını belirlemeye yardımcı olur.  Ayrıca, uzun süredir çalışan ve/veya kaynak tüketen Spark iş yürütmelerinin yürütülmesini izlemeniz gerekir.  Yanlış yapılandırmalar (özellikle yanlış boyutlandırılmış yürütücüler), uzun süren işlemler ve Kartezyen işlemlerle sonuçlanan görevler nedeniyle bellek baskısı etrafında en yaygın zorluklar merkezi.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Hadoop bileşenleri ve HDInsight ile kullanılabilir sürümleri?](../hdinsight-component-versioning.md)
* [HDInsight'ta bir Apache Spark kümesinin kaynaklarını yönetme](apache-spark-resource-manager.md)
* [Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight'ta küme oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
* [Apache Kıvılcım Yapılandırması](https://spark.apache.org/docs/latest/configuration.html)
* [Apache Hadoop İplik üzerinde Çalışan Apache Kıvılcım](https://spark.apache.org/docs/latest/running-on-yarn.html)
