---
title: Büyük veriler için bilişsel hizmetler 'i kullanmaya başlama
description: Azure Databricks 'de bilişsel hizmetler ile MMLSpark işlem hattınızı ayarlayın ve bir örnek çalıştırın.
services: cognitive-services
author: mhamilton723
manager: nitinme
ms.service: cognitive-services
ms.topic: quickstart
ms.date: 07/06/2020
ms.author: marhamil
ms.openlocfilehash: 7f4849c75b36b1663416ad1a97e3264fabcdaa67
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86201740"
---
# <a name="getting-started"></a>Başlarken

Ortamınızı ayarlamak, verileriniz için bir işlem hattı oluşturmanın ilk adımıdır. Ortamınız hazırlandıktan sonra, örnek çalıştırmak hızlı ve kolaydır.

Bu makalede, başlamanıza başlamak için şu adımları gerçekleştiririz:

1. [Bilişsel Hizmetler kaynağı oluşturma](#create-a-cognitive-services-resource)
1. [Apache Spark kümesi oluşturma](#create-an-apache-spark-cluster)
1. [Örnek deneyin](#try-a-sample)

## <a name="create-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağı oluşturma

Büyük veri bilişsel hizmetlerini kullanmak için, ilk olarak iş akışımız için bir bilişsel hizmet oluşturmanız gerekir. İki ana bilişsel hizmetler türü vardır: Azure 'da barındırılan bulut hizmetleri ve kullanıcılar tarafından yönetilen Kapsayıcılı hizmetler. Daha basit bulut tabanlı bilişsel hizmetler ile başlamasını öneririz.

### <a name="cloud-services"></a>Bulut hizmetleri

Bulut tabanlı bilişsel hizmetler, Azure 'da barındırılan akıllı algoritmalardır. Bu hizmetler eğitim olmadan kullanıma yöneliktir, yalnızca bir internet bağlantısına ihtiyacınız vardır. Azure portal veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli?tabs=windows)ile bilişsel [bir hizmet oluşturabilirsiniz](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows) .

### <a name="containerized-services-optional"></a>Kapsayıcılı hizmetler (isteğe bağlı)

Uygulamanız veya iş yükünüz çok büyük veri kümeleri kullanıyorsa, özel ağ gerektirir veya buluta başvuramıyorum, Cloud Services ile iletişim kurma imkansızçıkabilir. Bu durumda Kapsayıcılı bilişsel hizmetler şu avantajlara sahiptir:

* **Düşük bağlantı**: Kapsayıcılı bilişsel Hizmetleri, hem bulutta hem de kapalı bir bilgi işlem ortamında dağıtabilirsiniz. Uygulamanız buluta başvuramıyorum, uygulamanıza Kapsayıcılı bilişsel hizmetler dağıtımı yapmayı göz önünde bulundurun.

* **Düşük gecikme süresi**: Kapsayıcılı hizmetler buluta/buluttan gidiş dönüş iletişimi gerektirmediğinden, yanıtlar çok daha düşük gecikme süreleriyle döndürülür.

* **Gizlilik ve veri güvenliği**: Kapsayıcılı Hizmetleri özel ağlara dağıtarak gizli verilerin ağdan ayrılmaması için.

* **Yüksek ölçeklenebilirlik**: Kapsayıcılı hizmetlerin "oran limitleri" yoktur ve Kullanıcı tarafından yönetilen bilgisayarlarda çalışır. Bu nedenle, çok daha büyük iş yüklerini işlemek için bilişsel hizmetleri son olmadan ölçeklendirebilirsiniz.

Kapsayıcılı bir bilişsel hizmet oluşturmak için [Bu kılavuzu](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support?tabs=luis) izleyin.

## <a name="create-an-apache-spark-cluster"></a>Apache Spark kümesi oluşturma

[Apache Spark &trade; ](http://spark.apache.org/) , büyük veri verileri işleme için tasarlanan dağıtılmış bir bilgi işlem çerçevesidir. Kullanıcılar, Azure 'da Azure Databricks, Azure SYNAPSE Analytics, HDInsight ve Azure Kubernetes hizmetleri gibi hizmetlerle Apache Spark çalışabilir. Büyük veri bilişsel hizmetlerini kullanmak için önce bir küme oluşturmanız gerekir. Zaten bir Spark kümeniz varsa, bir örnek deneyebilirsiniz.

### <a name="azure-databricks"></a>Azure Databricks

Azure Databricks, tek tıklamayla kurulum, kolaylaştırılmış iş akışları ve etkileşimli bir çalışma alanı olan Apache Spark tabanlı bir analiz platformudur. Genellikle veri bilimcileri, mühendisler ve iş analistleri arasında işbirliği yapmak için kullanılır. Azure Databricks büyük veri bilişsel hizmetlerini kullanmak için şu adımları izleyin:

1. [Azure Databricks çalışma alanı oluşturma](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal#create-an-azure-databricks-workspace)
1. [Databricks’te Spark kümesi oluşturma](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal#create-a-spark-cluster-in-databricks)
1. Büyük veri bilişsel hizmetler 'i yükler
    * Databricks çalışma alanınızda yeni bir kitaplık oluşturma  
       <img src="media/create-library.png" alt="Create library" width="50%"/>
    * Aşağıdaki Maven koordinatları koordinatlarını girin: `com.microsoft.ml.spark:mmlspark_2.11:1.0.0-rc1` Depo:`https://mmlspark.azureedge.net/maven`  
      <img src="media/library-coordinates.png" alt="Library Coordinates" width="50%"/>
    * Kitaplığı bir kümeye yükler  
      <img src="media/install-library.png" alt="Install Library on Cluster" width="50%"/>

### <a name="synapse-analytics-optional"></a>SYNAPSE Analytics (isteğe bağlı)

İsteğe bağlı olarak, SYNAPSE Analytics 'i kullanarak bir Spark kümesi oluşturabilirsiniz. Azure SYNAPSE Analytics, kurumsal veri depolama ve büyük veri analizlerini birlikte sunar. Bu sayede, daha az sayıda isteğe bağlı veya sağlanan kaynakları kullanarak koşullarınızda verileri sorgulama özgürlüğü elde edersiniz. SYNAPSE Analytics 'i kullanmaya başlamak için şu adımları izleyin:

1. [Bir Synapse çalışma alanı (Önizleme) oluşturun](https://docs.microsoft.com/azure/synapse-analytics/quickstart-create-workspace).
1. [Azure Portal kullanarak yeni bir Apache Spark Havuzu (Önizleme) oluşturun](https://docs.microsoft.com/azure/synapse-analytics/quickstart-create-apache-spark-pool-portal).

SYNAPSE Analytics 'te bilişsel hizmetler için büyük veri varsayılan olarak yüklüdür.

### <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Kapsayıcılı bilişsel hizmetler kullanıyorsanız, kapsayıcılarla birlikte Spark dağıtmaya yönelik popüler bir seçenek de Azure Kubernetes hizmetidir.

Azure Kubernetes hizmetini kullanmaya başlamak için şu adımları izleyin:

1. [Azure portalını kullanarak Azure Kubernetes Service (AKS) kümesi dağıtma](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal)
1. [Apache Spark 2.4.0 Held grafiğini yükler](https://hub.helm.sh/charts/microsoft/spark)
1. [Held kullanarak bilişsel hizmet kapsayıcısı yükler](https://docs.microsoft.com/azure/cognitive-services/computer-vision/deploy-computer-vision-on-premises)

## <a name="try-a-sample"></a>Örnek deneyin

Spark kümenizi ve ortamınızı ayarladıktan sonra kısa bir örnek çalıştırabiliriz. Bu bölümde, Azure Databricks 'de bilişsel hizmetler için büyük verilerin nasıl kullanılacağı gösterilmektedir.

İlk olarak, Azure Databricks bir not defteri oluşturuyoruz. Diğer Spark kümesi sağlayıcıları için, not defterlerini veya Spark gönderimini kullanın.

1. **Azure Databricks** menüsünden **Yeni Not defteri** ' ni seçerek yeni bir databricks Not defteri oluşturun.

    <img src="media/new-notebook.png" alt="Create a new notebook" width="50%"/>

1. **Not Defteri Oluştur** iletişim kutusuna bir ad girin, dil olarak **Python** ' ı seçin ve daha önce oluşturduğunuz Spark kümesini seçin.

    <img src="media/databricks-notebook-details.jpg" alt="New notebook details" width="50%"/>

    **Oluştur**’u seçin.

1. Bu kod parçacığını yeni Not defterinize yapıştırın.

```python
from mmlspark.cognitive import *
from pyspark.sql.functions import col

# Add your subscription key from Text Analytics (or a general Cognitive Service key)
service_key = "ADD-SUBSCRIPTION-KEY-HERE"

df = spark.createDataFrame([
  ("I am so happy today, its sunny!", "en-US"),
  ("I am frustrated by this rush hour traffic", "en-US"),
  ("The cognitive services on spark aint bad", "en-US"),
], ["text", "language"])

sentiment = (TextSentiment()
    .setTextCol("text")
    .setLocation("eastus")
    .setSubscriptionKey(service_key)
    .setOutputCol("sentiment")
    .setErrorCol("error")
    .setLanguageCol("language"))

results = sentiment.transform(df)

# Show the results in a table
display(results.select("text", col("sentiment")[0].getItem("score").alias("sentiment")))

```

1. Azure portal Metin Analizi panonuzun **anahtarlar ve uç nokta** menüsünden abonelik anahtarınızı alın.
1. Databricks Not defteri kodunuzda abonelik anahtarı yer tutucusunu abonelik anahtarınızla değiştirin.
1. Örneği çalıştırmak için Not defteri hücrelerinizin sağ üst köşesinde bulunan Play veya Triangle simgesini seçin. İsteğe bağlı olarak, tüm hücreleri çalıştırmak için not defterinizin en üstünde **Tümünü Çalıştır** ' ı seçin. Yanıtlar, bir tablodaki hücrenin altında görüntülenir.

### <a name="expected-results"></a>Beklenen sonuçlar

| metin                                      |   yaklaşım |
|:------------------------------------------|------------:|
| Bugün bugün kutlu olsun!           |   0,978959  |
| Bu aceleniz saat trafiği tarafından hayal dışındayım |   0,0237956 |
| Spark aint hatalı üzerinde bilişsel hizmetler  |   0,888896  |

## <a name="next-steps"></a>Sonraki adımlar

- [Kısa Python örnekleri](samples-python.md)
- [Kısa Scala örnekleri](samples-scala.md)
- [Tarif: tahmine dayalı bakım](recipes/anomaly-detection.md)
- [Tarif: akıllı resim araştırması](recipes/art-explorer.md)
