---
title: Makine öğrenimine genel bakış - Azure HDInsight
description: Azure HDInsight'ta kümeler için büyük veri makinesi öğrenme seçeneklerine genel bakış.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/06/2019
ms.openlocfilehash: 6c7f50e4ba3803b5cb929b75f5ad1127f7f063d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75931676"
---
# <a name="machine-learning-on-hdinsight"></a>HDInsight'ta makine öğrenimi

HDInsight, büyük miktarlarda (petabaytlar, hatta eksabaytlar) yapılandırılmış, yapılandırılmamış ve hızlı hareket eden verilerden değerli bilgiler elde etme olanağı sağlayarak büyük verilerle makine öğrenimini sağlar. HDInsight'ta çeşitli makine öğrenimi seçenekleri vardır: SparkML ve Apache Spark MLlib, R, Apache Hive ve Microsoft Cognitive Toolkit.

## <a name="sparkml-and-mllib"></a>SparkML ve MLlib

[HDInsight Spark,](spark/apache-spark-overview.md) büyük veri analitiğini artırmak için bellek içi işlemeyi destekleyen birleşik, açık kaynak kodlu, paralel veri işleme çerçevesi [olan Apache Spark'ın](https://spark.apache.org/)Azure tarafından barındırılan bir teklifidir. Spark işleme motoru hız, kullanım kolaylığı ve gelişmiş analizler için üretilmiştir. Spark'ın bellek içi dağıtılmış hesaplama yetenekleri, makine öğrenimi ve grafik hesaplamalarında kullanılan yinelemeli algoritmalar için iyi bir seçim dir. Bu dağıtılmış ortama algoritmik modelleme yetenekleri getiren iki ölçeklenebilir makine öğrenme kitaplığı vardır: MLlib ve SparkML. MLlib RDD'lerin üzerine inşa edilmiş orijinal API içerir. SparkML, ML boru hatları oluşturmak için DataFrames'in üzerine inşa edilmiş daha üst düzey bir API sağlayan yeni bir pakettir. SparkML henüz MLlib'in tüm özelliklerini desteklemez, ancak Spark'ın standart makine öğrenimi kütüphanesi olarak MLlib'in yerini almaktadır.

Apache Spark için Microsoft Machine Learning kitaplığı [MMLSpark'tır.](https://github.com/Azure/mmlspark) Bu kütüphane, veri bilimcileri Spark'ta daha üretken hale getirmek, deney oranını artırmak ve çok büyük veri kümelerinde derin öğrenme de dahil olmak üzere en ileri makine öğrenimi tekniklerinden yararlanmak için tasarlanmıştır. MMLSpark, dizeleri dizine ekleyerek, verileri makine öğrenme algoritmaları tarafından beklenen bir düzene zorlama ve özellik vektörlerini birleştirme gibi ölçeklenebilir ML modellerini yaparken SparkML'in düşük düzeyli API'lerinin üstünde bir katman sağlar. MMLSpark kitaplığı, PySpark'ta modeller oluşturmak için bu ve diğer yaygın görevleri basitleştirir.

## <a name="r"></a>R

[R](https://www.r-project.org/) şu anda dünyanın en popüler istatistiksel programlama dilidir. 2,5 milyondan fazla kullanıcıdan oluşan ve büyüyen bir topluluğu olan açık kaynak kodlu bir veri görselleştirme aracıdır. Gelişen kullanıcı tabanı ve 8.000'den fazla katkıda bulunan paketleri ile R, makine öğrenimine ihtiyaç duyan birçok şirket için olası bir seçimdir. Büyük veri kümeleri ve modelleri ile kullanıma hazır ML Hizmetleri ile bir HDInsight kümesi oluşturabilirsiniz. Bu özellik, veri bilimcileri ve istatistikçiler, küme kurulumu ve bakımı nın yükü olmadan, hdinsight aracılığıyla isteğe bağlı ölçeklendirilebilen tanıdık bir R arabirimi sağlar.

![R sunucusu ile tahmin için eğitim](./media/hdinsight-machine-learning-overview/training-for-prediction.png)

Bir kümenin kenar düğümü kümeye bağlanmak ve R komut dosyalarınızı çalıştırmak için uygun bir yer sağlar.  ScaleR'ın Hadoop Harita Azaltma veya Kıvılcım bilgi işlem bağlamlarını kullanarak kümenin düğümlerinde R komut dosyalarını da çalıştırabilirsiniz.

Spark ile HDInsight'taki ML Hizmetleri ile, Bir Spark bilgi işlem bağlamını kullanarak bir kümenin düğümleri arasında eğitim paralelleştirebilirsiniz. R komut dosyalarını, kullanılabilir tüm çekirdekleri gerektiği gibi paralel olarak kullanarak doğrudan kenar düğümünde çalıştırabilirsiniz. Alternatif olarak, kümedeki tüm düğümlere dağıtılan işleme başlamak için kodunuzu kenar düğümünden çalıştırabilirsiniz. Spark ile HDInsight'taki ML Hizmetleri, istenirse açık kaynak R paketlerinden paralelleme işlevleri de sağlar.

## <a name="azure-machine-learning-and-apache-hive"></a>Azure Makine Öğrenimi ve Apache Hive

Azure Machine Learning, tahmine dayalı analitiği modellemek için araçlar ve tahmine dayalı modellerinizi kullanıma hazır web hizmetleri olarak dağıtmak için kullanabileceğiniz tam olarak yönetilen bir hizmet sağlar. Azure Machine Learning, tahmine dayalı modeller oluşturmak, test etmek, operasyonel hale getirmek ve yönetmek için bulutta kullanabileceğiniz tam bir tahmine dayalı analitik çözümdür. Büyük bir algoritma kitaplığından seçim yapın, modeller oluşturmak için web tabanlı bir stüdyo kullanın ve modelinizi web hizmeti olarak kolayca dağıtın.

![Microsoft Azure makine öğrenimine genel bakış](./media/hdinsight-machine-learning-overview/azure-machine-learning.png)

[Hive sorgularını](../machine-learning/team-data-science-process/create-features-hive.md)kullanarak HDInsight Hadoop kümesindeki veriler için özellikler oluşturun. *Özellik mühendisliği,* öğrenme sürecini kolaylaştıran ham verilerden özellikler oluşturarak öğrenme algoritmalarının tahmin gücünü artırmaya çalışır. Azure Machine Learning Studio'dan (klasik) HiveQL sorgularını çalıştırabilir ve [İçe İşlem Veri modülasyonlarını](../machine-learning/studio/import-data.md)kullanarak Hive'da işlenen ve blob depolamada depolanan verilere erişebilirsiniz.

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti

[Derin öğrenme,](https://www.microsoft.com/en-us/research/group/dltc/) insan beyninin biyolojik süreçlerinden esinlenerek sinir ağları kullanan bir makine öğrenimi dalıdır. Birçok araştırmacı yapay zeka geliştirmek için umut verici bir yaklaşım olarak derin öğrenme bakın. Derin öğrenme örnekleri konuşulan dil çevirmenleri, görüntü tanıma sistemleri ve makine muhakemesidir.

Microsoft, derin öğrenme de kendi çalışmalarını ilerletmeye yardımcı olmak için ücretsiz, kullanımı kolay, açık kaynak [Microsoft Cognitive Toolkit'i](https://www.microsoft.com/en-us/cognitive-toolkit/)geliştirdi. Bu araç seti, çok çeşitli Microsoft ürünleri, dünya çapında ki şirketler tarafından, ölçekte derin öğrenmeyi dağıtma ihtiyacı olan şirketler ve en son algoritma lar ve tekniklerle ilgilenen öğrenciler tarafından kullanılmaktadır.

## <a name="see-also"></a>Ayrıca bkz.

### <a name="scenarios"></a>Senaryolar

* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Apache Mahout ile film önerileri oluşturun](hadoop/apache-hadoop-mahout-linux-mac.md)
* [Apache Hive ve Azure Makine Öğrenimi](../machine-learning/team-data-science-process/create-features-hive.md)
* [Apache Hive ve Azure Machine Learning uçuca](../machine-learning/team-data-science-process/hive-walkthrough.md)
* [HDInsight'ta Apache Spark ile makine öğrenimi](../machine-learning/team-data-science-process/spark-overview.md)

### <a name="deep-learning-resources"></a>Derin öğrenme kaynakları

* [Azure HDInsight Spark kümesi ile Microsoft Cognitive Toolkit derin öğrenme modelini kullanın](spark/apache-spark-microsoft-cognitive-toolkit.md)
* [Derin Öğrenme ve Veri Bilimi Sanal Makine (DSVM) üzerinde Yapay Bilgi çerçeveleri](../machine-learning/data-science-virtual-machine/dsvm-tools-deep-learning-frameworks.md)
