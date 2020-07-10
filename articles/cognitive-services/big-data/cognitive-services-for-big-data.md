---
title: Büyük veriler için bilişsel hizmetler
description: Python, Java ve Scala kullanarak büyük veri kümelerinde Azure bilişsel hizmetler 'den nasıl yararlanacağınızı öğrenin. Büyük veriler için bilişsel hizmetler sayesinde, akıllı modellerinizi doğrudan Apache Spark &trade; ve SQL hesaplamaları içine ekleyebilirsiniz.
services: cognitive-services
author: mhamilton723
manager: nitinme
ms.service: cognitive-services
ms.topic: overview
ms.date: 07/09/2020
ms.author: marhamil
ms.openlocfilehash: 0001002636362a74915f9dae21a6beff52baea63
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86201781"
---
# <a name="azure-cognitive-services-for-big-data"></a>Büyük veriler için Azure bilişsel hizmetler

![Büyük veriler için Azure bilişsel hizmetler](media/cognitive-services-big-data-overview.svg)

Büyük veriler için Azure bilişsel hizmetler, kullanıcıların bilişsel hizmetler aracılığıyla [Apache Spark &trade; ](https://docs.microsoft.com/dotnet/spark/what-is-spark)kullanarak veri, terabaytlarca veri kanalı sağlar. Büyük veriler için bilişsel hizmetler sayesinde, herhangi bir veri deposu ile büyük ölçekli akıllı uygulamalar oluşturmak kolaydır.

Büyük veriler için bilişsel hizmetler sayesinde, akıllı modellerinizi doğrudan Apache Spark &trade; ve SQL hesaplamaları içine ekleyebilirsiniz. Bu araçlar, geliştiricilerin akıllı ve dağıtılmış uygulamalar oluşturmaya odaklanabilmeleri için düşük düzeyli ağ ayrıntılarından kurtulmasını sağlayabilir.

## <a name="features-and-benefits"></a>Özellikler ve avantajlar

Büyük veriler için bilişsel hizmetler, dünyanın herhangi bir bölgesindeki Hizmetleri ve [Kapsayıcılı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support)bilişsel hizmetler 'i kullanabilirler. Kapsayıcılar, ultra düşük gecikme süresine sahip düşük veya hiç bağlantı dağıtımını destekler. Kapsayıcılı bilişsel hizmetler yerel olarak, Spark kümenizin çalışan düğümlerinde veya Kubernetes gibi harici bir Orchestrator üzerinde çalıştırılabilir.

## <a name="supported-services"></a>Desteklenen hizmetler

API 'Ler ve SDK 'Lar aracılığıyla erişilen bilişsel [Hizmetler](https://docs.microsoft.com/azure/cognitive-services/), geliştiricilerin AI veya veri bilimi becerileri olmadan akıllı uygulamalar oluşturmasına yardımcı olur. Bilişsel hizmetler sayesinde uygulamalarınızın görmesini, duymasını, konuşmasını, anlamasını ve nedenini görmenizi sağlayabilirsiniz. Bilişsel hizmetler 'i kullanmak için uygulamanızın ağ üzerinden hizmete veri gönderilmesi gerekir. Hizmet alındıktan sonra, dönüş sırasında akıllı bir yanıt gönderir. Büyük veri iş yükleri için aşağıdaki hizmetler mevcuttur:

### <a name="vision"></a>Görsel

|Hizmet Adı|Hizmet Açıklaması|
|:-----------|:------------------|
|[Görüntü İşleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/ "Görüntü İşleme")| Görüntü İşleme Hizmeti, görüntüleri işlemeye ve bilgi döndürmeye yönelik gelişmiş algoritmalara erişmenizi sağlar. |
|[Yüz](https://docs.microsoft.com/azure/cognitive-services/face/ "Yüz")| Yüz tanıma hizmeti, yüz öznitelik algılamayı ve tanımayı etkinleştirerek gelişmiş yüz algoritmalarına erişim sağlar. |

### <a name="speech"></a>Konuşma

|Hizmet Adı|Hizmet Açıklaması|
|:-----------|:------------------|
|[Konuşma hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/ "Konuşma hizmeti")|Konuşma hizmeti, konuşma tanıma, konuşma birleştirme, konuşma çevirisi ve konuşmacı doğrulama ve tanımlama gibi özelliklere erişim sağlar.|

### <a name="decision"></a>Karar

|Hizmet Adı|Hizmet Açıklaması|
|:-----------|:------------------|
|[Anomali Algılayıcısı](https://docs.microsoft.com/azure/cognitive-services/anomaly-detector/ "Anomali Algılayıcısı") | Anomali algılayıcısı (Önizleme) hizmeti, zaman serisi verilerinizde normalleştirimler izlemenize ve bunları algılamanıza olanak tanır.|

### <a name="language"></a>Dil

|Hizmet Adı|Hizmet Açıklaması|
|:-----------|:------------------|
|[Metin Analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/ "Metin Analizi")| Metin Analizi hizmeti, yaklaşım analizi, anahtar ifade ayıklama ve dil algılama için ham metin üzerinde doğal dil işleme sağlar.|

### <a name="search"></a>Ara

|Hizmet Adı|Hizmet Açıklaması|
|:-----------|:------------------|
|[Bing Resim Arama](https://docs.microsoft.com/azure/cognitive-services/bing-image-search "Bing Resim Arama")|Bing Resim Arama hizmeti, kullanıcının sorgusuyla ilgili olduğunu belirlenen görüntülerin bir görüntüsünü döndürür.|

## <a name="supported-programming-languages-for-cognitive-services-for-big-data"></a>Büyük veriler için bilişsel hizmetler için desteklenen programlama dilleri

Büyük veriler için bilişsel hizmetler Apache Spark oluşturulmuştur. Apache Spark, Java, Scala, Python, R ve diğer birçok dili destekleyen bir dağıtılmış bilgi işlem kitaplığıdır. Bu diller şu anda destekleniyor.

### <a name="python"></a>Python

`mmlspark.cognitive` [Apache Spark IÇIN Microsoft ml](https://aka.ms/spark)ad alanında pyspark API sunuyoruz. Daha fazla bilgi için bkz. [Python geliştirici API 'si](https://mmlspark.blob.core.windows.net/docs/1.0.0-rc1/pyspark/mmlspark.cognitive.html). Kullanım örnekleri için bkz. [Python örnekleri](samples-python.md).

### <a name="scala-and-java"></a>Scala ve Java

`com.microsoft.ml.spark.cognitive` [Apache Spark IÇIN Microsoft ml](https://aka.ms/spark)ad alanına bir Scala ve Java tabanlı Spark API sunuyoruz. Daha fazla bilgi için bkz. [Scala geliştirici API 'si](https://mmlspark.blob.core.windows.net/docs/1.0.0-rc1/scala/index.html#package). Kullanım örnekleri için bkz. [Scala örnekleri](samples-scala.md).

## <a name="supported-platforms-and-connectors"></a>Desteklenen platformlar ve bağlayıcılar

Büyük veriler için bilişsel hizmetler Apache Spark gerektirir. Büyük veriler için bilişsel hizmetler 'i destekleyen çeşitli Apache Spark platformları vardır.

### <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks), Microsoft Azure bulut hizmetleri platformu için iyileştirilen Apache Spark tabanlı bir analiz platformudur. Tek tıklamayla kurulum, kolaylaştırılmış iş akışları ve veri bilimcileri, veri mühendisleri ve iş analistleri arasında işbirliğini destekleyen etkileşimli bir çalışma alanı sağlar.

### <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

[Azure SYNAPSE Analytics](https://docs.microsoft.com/azure/databricks/data/data-sources/azure/synapse-analytics) (eskı adıyla SQL veri ambarı), büyük ölçüde paralel işleme kullanan kurumsal veri ambarı olarak adlandırılmıştır. SYNAPSE Analytics ile, petabaytlarca veri genelinde karmaşık sorguları hızlı bir şekilde çalıştırabilirsiniz. Azure SYNAPSE Analytics, bir sezgisel Jupyter Notebook arabirimiyle Spark Işleri çalıştırmak için yönetilen Spark havuzları sağlar.

### <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

[Azure Kubernetes hizmeti (AKS)](https://docs.microsoft.com/azure/aks/) , Docker kapsayıcılarını ve dağıtılmış uygulamaları büyük ölçekli ölçekte düzenler. AKS, Azure 'da Kubernetes kullanımını kolaylaştıran, yönetilen bir Kubernetes sunumudur. Kubernetes bilişsel hizmet ölçeği, gecikme süresi ve ağ üzerinde ayrıntılı denetim sağlayabilir. Ancak, Apache Spark hakkında bilginiz varsa Azure Databricks veya SYNAPSE analizlerini kullanmanızı öneririz.

### <a name="data-connectors"></a>Veri bağlayıcıları

Spark kümeniz olduktan sonra, bir sonraki adım verilerinize bağlanıyor. Apache Spark, veritabanı bağlayıcılarının geniş bir koleksiyonuna sahiptir. Bu bağlayıcılar, uygulamaların nerede depolandıklarından bağımsız olarak büyük veri kümeleriyle çalışmasına izin verir. Desteklenen veritabanları ve bağlayıcılar hakkında daha fazla bilgi için, [Azure Databricks için desteklenen veri kaynaklarının listesine](https://docs.microsoft.com/azure/databricks/data/data-sources/)bakın.

## <a name="concepts"></a>Kavramlar

### <a name="spark"></a>Spark

[Apache Spark &trade; ](http://spark.apache.org/) , büyük ölçekli veri işlemeye yönelik Birleşik bir analiz altyapısıdır. Paralel işleme çerçevesi, büyük veri ve analitik uygulamaların performansını artırır. Spark, çekirdek uygulama kodunu değiştirmeden hem Batch hem de akış işleme sistemi olarak çalışabilir.

Spark 'un temeli veri çerçevesidir: Apache Spark çalışan düğümlerinde dağıtılan verilerin tablosal koleksiyonudur. Spark DataFrame, bir ilişkisel veritabanındaki bir tablo ya da R/Python içindeki bir veri çerçevesi gibi sınırsız ölçeğe benzer. DataFrames 'ler: yapılandırılmış veri dosyaları, Hive içindeki tablolar veya dış veritabanları gibi birçok kaynaktan oluşturulabilir. Verileriniz Spark veri çerçevesinden olduktan sonra şunları yapabilirsiniz:

   - Ekleme ve filtreleme tabloları gibi SQL stili hesaplamalar yapın.
   - MapReduce stili paralellik kullanarak büyük veri kümelerine işlevler uygulayın.
   - Apache Spark için Microsoft Machine Learning kullanarak dağıtılmış Machine Learning uygulayın.
   - Kullanıma hazırlamış akıllı hizmetlerle verilerinizi zenginleştirmek için büyük veriler için bilişsel hizmetler 'i kullanın.

### <a name="microsoft-machine-learning-for-apache-spark-mmlspark"></a>Apache Spark için Microsoft Machine Learning (MMLSpark)

[Apache Spark Için Microsoft Machine Learning](https://mmlspark.blob.core.windows.net/website/index.html#install) (mmlspark), Apache Spark üzerinde geliştirilen açık kaynaklı, dağıtılmış bir makine öğrenme KITAPLıĞıDıR (ml). Büyük veriler için bilişsel hizmetler bu pakete dahildir. Ayrıca, MMLSpark, LightGBM, Vowpal Wabbit, OpenCV, LIME ve daha fazlasını içeren Apache Spark için çeşitli diğer ML araçları içerir. MMLSpark ile, herhangi bir Spark veri kaynağından güçlü tahmine dayalı ve analitik modeller oluşturabilirsiniz.

### <a name="http-on-spark"></a>Spark üzerinde HTTP

Büyük veriler için bilişsel hizmetler, akıllı Web hizmetlerini büyük verilerle nasıl tümleştirebildiğimiz hakkında bir örnektir. Web Hizmetleri, dünya genelinde birçok uygulamayı ve çoğu hizmeti Köprü Metni Aktarım Protokolü (HTTP) üzerinden iletişim kurar. Büyük ölçeklerde *rastgele* Web hizmetleriyle çalışmak için Spark 'ta http sağlıyoruz. Spark üzerinde HTTP ile herhangi bir Web hizmeti aracılığıyla terabayt veri geçirebilirsiniz. Bu teknolojiyi kullanırken, büyük verilere yönelik hizmetleri Power bilişsel hizmetler için kullanırız.

## <a name="developer-samples"></a>Geliştirici örnekleri

- [Tarif: tahmine dayalı bakım](recipes/anomaly-detection.md)
- [Tarif: akıllı resim araştırması](recipes/art-explorer.md)

## <a name="blog-posts"></a>Blog yazıları

- [Bilişsel hizmetlerin nasıl çalıştığı hakkında daha fazla bilgi edinin Apache Spark&trade;](https://azure.microsoft.com/blog/dear-spark-developers-welcome-to-azure-cognitive-services/)
- [Leopards ve Spark hakkında derin öğrenme ve Görüntü İşleme kar kaydetme](http://www.datawizard.io/2017/06/27/saving-snow-leopards-with-deep-learning-and-computer-vision-on-spark/)
- [Microsoft Research podcast: MMLSpark, Mark Hafreton ile AI güçlendirin](https://blubrry.com/microsoftresearch/49485070/092-mmlspark-empowering-ai-for-good-with-mark-hamilton/)

## <a name="webinars-and-videos"></a>Web seminerleri ve videoları

- [Spark 'ta Azure bilişsel hizmetler: gömülü Akıllı hizmetlerle kümeler](https://databricks.com/session/the-azure-cognitive-services-on-spark-clusters-with-embedded-intelligent-services)
- [Spark Zirvesi Keynote: uygun ölçeklenebilir AI](https://databricks.com/session_eu19/scalable-ai-for-good)
- [Cosmos DB büyük veriler için bilişsel hizmetler](https://medius.studios.ms/Embed/Video-nc/B19-BRK3004?latestplayer=true&l=2571.208093)

## <a name="next-steps"></a>Sonraki adımlar

- [Büyük veriler için bilişsel hizmetler 'i kullanmaya başlama](getting-started.md)
- [Basit Python örnekleri](samples-python.md)
- [Basit Scala örnekleri](samples-scala.md)
