---
title: Derin dalış - gelişmiş analitik - Azure HDInsight
description: Azure HDInsight'ta büyük verileri işlemek için gelişmiş analizlerin algoritmaları nasıl kullandığını öğrenin.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/01/2020
ms.openlocfilehash: 6fc6ea9ca0ce3bf82f027b964db94df50f07f2bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646532"
---
# <a name="deep-dive---advanced-analytics"></a>Derin dalış - gelişmiş analitik

## <a name="what-is-advanced-analytics-for-hdinsight"></a>HDInsight için gelişmiş analitik nedir?

HDInsight, büyük miktarda yapılandırılmış, yapılandırılmamış ve hızlı hareket eden verilerden değerli bilgiler elde etme olanağı sağlar. Gelişmiş analitik, size anlamlı öngörüler sağlamak için yüksek ölçeklenebilir mimarilerin, istatistiksel ve makine öğrenimi modellerinin ve akıllı panoların kullanılmasıdır. Makine öğrenimi veya *tahmine dayalı analitik,* öngörülerde bulunmak ve kararlarınıza rehberlik etmek için verilerinizdeki ilişkileri tanımlayan ve öğrenen algoritmalar kullanır.

## <a name="advanced-analytics-process"></a>Gelişmiş analitik süreci

![Gelişmiş analitik süreç akışı](./media/apache-hadoop-deep-dive-advanced-analytics/hdinsight-analytic-process.png)

İş sorununu belirledikten ve verilerinizi toplamaya ve işlemeye başladıktan sonra, tahmin etmek istediğiniz soruyu temsil eden bir model oluşturmanız gerekir. Modeliniz, işletme gereksinimlerinize en uygun tahmin türünü yapmak için bir veya daha fazla makine öğrenimi algoritması kullanır.  Verilerinizin çoğu modelinizi eğitmek için, geri kalanı ise test etmek veya değerlendirmek için kullanılmalıdır.

Modelinizi oluşturduktan, yükledikten, test ettikten ve değerlendirdikten sonra, bir sonraki adım, modelinizi sorularınızın yanıtlarını sağlamaya başlaması için dağıtmaktır. Son adım, modelinizin performansını izlemek ve gerektiği gibi ayarlamaktır.

## <a name="common-types-of-algorithms"></a>Yaygın algoritma türleri

Gelişmiş analitik çözümleri, bir dizi makine öğrenimi algoritması sağlar. Burada algoritmalar ve ilişkili ortak iş kullanım örnekleri kategorilerinin bir özetidir.

![Makine Öğrenimi kategori özetleri](./media/apache-hadoop-deep-dive-advanced-analytics/machine-learning-use-cases.png)

En uygun algoritma(lar) seçiminin yanı sıra, eğitim için veri sağlamanız gerekip gerekmediğini de göz önünde bulundurmanız gerekir. Makine öğrenimi algoritmaları aşağıdaki gibi sınıflandırılır:

* Denetlenen - algoritma nın sonuç verebilmesi için önce etiketli bir veri kümesi üzerinde eğitilmesi gerekir
* Yarı denetimli - algoritma, eğitimin ilk aşamasında mevcut olmayan bir eğitmen tarafından interaktif sorgu yoluyla ekstra hedeflerle artırılabilir
* Denetimsiz - algoritma eğitim verileri gerektirmez
* Pekiştirme - algoritma belirli bir bağlamda ideal davranışı belirlemek için yazılım aracıları kullanır (genellikle robotik kullanılır)

| Algoritma Kategorisi| Kullanım | Öğrenme Türü | Algoritmalar |
| --- | --- | --- | -- |
| Sınıflandırma | Kişileri veya şeyleri gruplar halinde sınıflandırma | Denetimli | Karar ağaçları, Lojistik regresyon, sinir ağları |
| Kümeleme | Bir dizi örneği homojen gruplara bölme | Denetimsiz | K-kümeleme anlamına gelir |
| Desen algılama | Verilerdeki sık çağrışımları belirleme | Denetimsiz | İlişkilendirme kuralları |
| Regresyon | Sayısal sonuçları tahmin etme | Denetimli | Doğrusal regresyon, sinir ağları |
| Takviye | Robotlar için en uygun davranışı belirleme | Takviye | Monte Carlo Simülasyonları, DeepMind |

## <a name="machine-learning-on-hdinsight"></a>HDInsight'ta makine öğrenimi

HDInsight gelişmiş bir analitik iş akışı için çeşitli makine öğrenimi seçeneklerine sahiptir:

* Makine Öğrenimi ve Apache Kıvılcım
* R ve ML Hizmetleri
* Azure Makine Öğrenimi ve Apache Hive
* Apaçi Kıvılcım ve Derin öğrenme

### <a name="machine-learning-and-apache-spark"></a>Makine Öğrenimi ve Apache Kıvılcım

[HDInsight Spark,](../spark/apache-spark-overview.md) Büyük Veri analitiğini artırmak için bellek içi işlemeyi kullanan birleşik, açık kaynak kodlu paralel bir veri işleme çerçevesi olan [Apache Spark'ın](https://spark.apache.org/)Azure tarafından barındırılan bir teklifidir. Spark işleme motoru hız, kullanım kolaylığı ve gelişmiş analizler için üretilmiştir. Spark'ın bellek içi dağıtılmış hesaplama yetenekleri, makine öğrenimi ve grafik hesaplamalarında kullanılan yinelemeli algoritmalar için iyi bir seçim dir.

Bu dağıtılmış ortama algoritmik modelleme yetenekleri getiren üç ölçeklenebilir makine öğrenme kitaplıkları vardır:

* [**MLlib**](https://spark.apache.org/docs/latest/ml-guide.html) - MLlib, Kıvılcım RDD'lerinin üzerine inşa edilmiş orijinal API içerir.
* [**SparkML**](https://spark.apache.org/docs/1.2.2/ml-guide.html) - SparkML, ML boru hatları oluşturmak için Spark DataFrames'in üzerine inşa edilmiş daha üst düzey bir API sağlayan daha yeni bir pakettir.
* [**MMLSpark**](https://github.com/Azure/mmlspark) - Apache Spark (MMLSpark) için Microsoft Machine Learning kitaplığı, veri bilimcileri Spark'ta daha üretken hale getirmek, deneme oranını artırmak ve çok büyük veri kümelerinde derin öğrenme de dahil olmak üzere en ileri teknoloji makine öğrenimi tekniklerinden yararlanmak için tasarlanmıştır. MMLSpark kitaplığı, PySpark'ta modeller oluşturmak için ortak modelleme görevlerini basitleştirir.

### <a name="r-and-ml-services"></a>R ve ML Hizmetleri

HDInsight'ın bir parçası olarak, büyük veri kümeleri ve modelleri ile kullanılmaya hazır [ML Hizmetleri](../r-server/r-server-overview.md) ile bir HDInsight kümesi oluşturabilirsiniz. Bu yeni yetenek, veri bilimcileri ve istatistikçilere, küme kurulumu ve bakımının yükü olmadan HDInsight aracılığıyla isteğe bağlı ölçeklendirebilen tanıdık bir R arabirimi sağlar.

### <a name="azure-machine-learning-and-apache-hive"></a>Azure Makine Öğrenimi ve Apache Hive

[Azure Machine Learning Studio (klasik),](https://studio.azureml.net/) tahmine dayalı analitiği modellemek için araçlar ve tahmine dayalı modellerinizi kullanıma hazır web hizmetleri olarak dağıtmak için kullanabileceğiniz tam olarak yönetilen bir hizmet sağlar. Azure Machine Learning, tahmine dayalı modelleri hızlı bir şekilde oluşturmak, test etmek, operasyonel hale getirmek ve yönetmek için bulutta tam tahmine dayalı analitik çözümleri oluşturmak için araçlar sağlar. Büyük bir algoritma kitaplığından seçim yapın, modeller oluşturmak için web tabanlı bir stüdyo kullanın ve modelinizi web hizmeti olarak kolayca dağıtın.

### <a name="apache-spark-and-deep-learning"></a>Apaçi Kıvılcım ve Derin öğrenme

[Derin öğrenme,](https://www.microsoft.com/research/group/dltc/) insan beyninin biyolojik süreçlerinden esinlenerek *derin sinir ağları* (DNNs) kullanan bir makine öğrenimi dalıdır. Birçok araştırmacı derin öğrenmeyi yapay zeka için umut verici bir yaklaşım olarak görüyor. Derin öğrenmenin bazı örnekleri konuşulan dil çevirmenleri, görüntü tanıma sistemleri ve makine muhakemesidir. Derin öğrenme de kendi çalışmalarını ilerletmek için, Microsoft ücretsiz, kullanımı kolay, açık kaynak [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/)geliştirdi. Araç seti, çok çeşitli Microsoft ürünleri, dünya çapında derin öğrenmeyi ölçekte dağıtma ihtiyacı olan şirketler ve en son algoritma ve tekniklerle ilgilenen öğrenciler tarafından yaygın olarak kullanılmaktadır.

## <a name="scenario---score-images-to-identify-patterns-in-urban-development"></a>Senaryo - Kentsel Gelişimde Örüntüleri Belirlemek İçin Resim Puanlama

HDInsight'ı kullanarak gelişmiş bir analitik makine öğrenme boru hattı örneğini gözden geçirelim.

Bu senaryoda, DN'lerin derin öğrenme çerçevesinde, Microsoft'un Bilişsel Araç Seti 'nde (CNTK) nasıl üretildiğini, HDInsight Spark kümesinde PySpark kullanarak Azure Blob Depolama hesabında depolanan büyük görüntü koleksiyonlarını puanlamak için nasıl operasyonel hale getirildiğini göreceksiniz. Bu yaklaşım, ortak bir DNN kullanım örneğine, havadan görüntü sınıflandırmasına uygulanır ve kentsel gelişimdeki son desenleri tanımlamak için kullanılabilir.  Önceden eğitilmiş bir görüntü sınıflandırma modeli kullanacaksınız. Model [CIFAR-10 veri setinde](https://www.cs.toronto.edu/~kriz/cifar.html) önceden eğitilmiş ve 10.000 bekletilen görüntülere uygulanmıştır.

Bu gelişmiş analitik senaryosunda üç temel görev vardır:

1. Apache Spark 2.1.0 dağılımına sahip bir Azure HDInsight Hadoop kümesi oluşturun.
2. Azure HDInsight Spark kümesinin tüm düğümlerine Microsoft Cognitive Toolkit'i yüklemek için özel bir komut dosyası çalıştırın.
3. Spark Python API 'yi (PySpark) kullanarak Azure Blob Depolama Hesabındaki dosyalara eğitimli bir Microsoft Cognitive Tooltoolkit derin öğrenme modelini uygulamak için HDInsight Spark kümenize önceden oluşturulmuş bir Jupyter dizüstü bilgisayar yükleyin.

Bu örnek, Alex Krizhevsky, Vinod Nair ve Geoffrey Hinton tarafından derlenen ve dağıtılan CIFAR-10 görüntü kümesini kullanır. CIFAR-10 veri seti, birbirini dışlayan 10 sınıfa ait 60.000 32×32 renkli görüntü içerir:

![Makine Öğrenimi örnek resimleri](./media/apache-hadoop-deep-dive-advanced-analytics/machine-learning-images.png)

Veri seti hakkında daha fazla bilgi için Alex Krizhevsky'nin [Tiny Images'dan Birden Fazla Özellik Katmanı Öğrenme](https://www.cs.toronto.edu/~kriz/learning-features-2009-TR.pdf)bölümüne bakın.

Veri kümesi 50.000 görüntüden oluşan bir eğitim setine ve 10.000 görüntüden oluşan bir test kümesine ayrılmıştır. İlk set, Microsoft Cognitive Toolkit'i kullanarak yirmi katmanlı kıvrımlı kalıntı ağ (ResNet) modelini, Bilişsel Toolkit GitHub deposundan [bu öğreticiyi](https://github.com/Microsoft/CNTK/tree/master/Examples/Image/Classification/ResNet) izleyerek eğitmek için kullanıldı. Geri kalan 10.000 görüntü modelin doğruluğunu test etmek için kullanıldı. Burada dağıtılmış bilgi işlem devreye girer: ön işleme ve görüntüleri puanlama görevi son derece paralelleştirilebilir. Elimizde kayıtlı eğitimli model ile, biz kullanılır:

* PySpark görüntüleri dağıtmak ve kümenin işçi düğümlerine model eğitimli.
* Python, HDInsight Spark kümesinin her düğümündeki görüntüleri önceden işlemek için.
* Modeli yüklemek ve her düğümüzerinde önceden işlenmiş görüntüleri puan bilişsel Araç seti.
* PySpark komut dosyasını çalıştırmak, sonuçları toplamak ve model performansını görselleştirmek için [Matplotlib'i](https://matplotlib.org/) kullanmak için Jupyter Notebooks.

10.000 görüntünün ön işleme/puanlamasının tamamı, 4 alt düğümlü bir kümede bir dakikadan az sürer. Model doğru ~ 9.100 (%91) etiketleri tahmin Görüntü. Bir karışıklık matrisi en yaygın sınıflandırma hatalarını gösterir. Örneğin, matris, köpekleri kedi olarak yanlış etiketlemenin diğer etiket çiftlerine göre daha sık oluştuğunu gösterir.

![Machine Learning sonuç grafiği](./media/apache-hadoop-deep-dive-advanced-analytics/machine-learning-results.png)

### <a name="try-it-out"></a>Deneyin!

Bu çözümü uçtan uca uygulamak için [bu öğreticiyi](../spark/apache-spark-microsoft-cognitive-toolkit.md) izleyin: bir HDInsight Spark kümesi kurun, Bilişsel Araç Seti'ni kurun ve 10.000 CIFAR görüntüsü alan Jupyter Notebook'u çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Apache Hive ve Azure Makine Öğrenimi

* [Apache Hive ve Azure Machine Learning uçuca](../../machine-learning/team-data-science-process/hive-walkthrough.md)
* [1-TB veri kümesinde Azure HDInsight Hadoop Kümesi kullanma](../../machine-learning/team-data-science-process/hive-criteo-walkthrough.md)

Apaçi Kıvılcım ve MLLib

* [HDInsight'ta Apache Spark ile makine öğrenimi](../../machine-learning/team-data-science-process/spark-overview.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Apache Spark'ı kullanın](../spark/apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Apache Spark'ı kullanın](../spark/apache-spark-machine-learning-mllib-ipython.md)

Derin Öğrenme, Bilişsel Araç Seti ve diğerleri

* [Veri Bilimi Azure Sanal Makine](../../machine-learning/data-science-virtual-machine/overview.md)
* [Azure HDInsight'ta H2O.ai tanıtımı](https://azure.microsoft.com/blog/introducing-h2o-ai-with-on-azure-hdinsight-to-bring-the-most-robust-ai-platform-for-enterprises/)
