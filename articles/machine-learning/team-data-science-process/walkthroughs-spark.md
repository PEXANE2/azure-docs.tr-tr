---
title: PySpark, Scala ile HDInsight Spark'ta Analytics - Ekip Veri Bilimi Süreci
description: Bir Azure HDInsight Kıvılcımı'nda PySpark ve Scala'nın kullanımından geçen Ekip Veri Bilimi Süreci'ne örnekler.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 91aac279a264d64ace5988d147c4caf8c52e9656
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75864154"
---
# <a name="hdinsight-spark-data-science-walkthroughs-using-pyspark-and-scala-on-azure"></a>HDInsight Spark veri bilimi walkthroughs Azure PySpark ve Scala kullanarak

Bu gözden geçirme ler, tahmine dayalı analizler yapmak için Bir Azure Kıvılcım kümesinde PySpark ve Scala'yı kullanır. Ekip Veri Bilimi Sürecinde belirtilen adımları izlerler. Ekip Veri Bilimi Sürecine genel bir bakış için [bkz.](overview.md) HDInsight'ta Kıvılcım'a genel bir bakış için, [HDInsight'ta Kıvılcıma Giriş](../../hdinsight/spark/apache-spark-overview.md)bölümüne bakın.

Ekip Veri Bilimi İşlemi'ni yürüten ek veri bilimi gözden geçirmeleri, kullandıkları **platforma** göre gruplandırılır. Bu örneklerin bir öğeleştirme için [Takım Veri Bilimi İşlemi'ni yürüten Walkthroughs'a](walkthroughs.md) bakın.

## <a name="predict-taxi-tips-using-pyspark-on-azure-spark"></a>Azure Kıvılcımı'nda PySpark'ı kullanarak taksi ipuçlarını tahmin etme

[Azure HDInsight'taki Spark'ı kullanma,](spark-overview.md) New York taksi verilerini kullanarak bir bahşişin ödenip ödenmediğini ve beklenen tutarların aralığını tahmin eder. Bu örnek, kamuya açık NYC taksi yolculuğu ve ücret veri kümesindeki mühendis verilerini depolamak, keşfetmek ve özellik sağlamak için [bir Azure HDInsight Spark kümesini](https://azure.microsoft.com/services/hdinsight/) kullanarak Bir senaryoda Ekip Veri Bilimi İşlemi'ni kullanır. Bu genel bakış konusu bir HDInsight Spark kümesi ve Jupyter PySpark dizüstü bilgisayarları kullanır. Bu not defterleri, verilerinizi nasıl keşfedersiniz ve ardından modelleri nasıl oluşturup tükettiğinizi gösterir. Gelişmiş veri arama ve modelleme not defteri çapraz doğrulama, hiper-parametre süpürme ve model değerlendirme nasıl dahil gösterir.

### <a name="data-exploration-and-modeling-with-spark"></a>Spark ile Veri Arama ve Modelleme 
Veri kümesini keşfedin ve [Spark MLlib araç seti konusuyla veri için ikili sınıflandırma ve regresyon modelleri oluştur](spark-data-exploration-modeling.md) üzerinden çalışarak makine öğrenimi modellerini oluşturun, puanlandırın ve değerlendirin.

### <a name="model-consumption"></a>Model tüketimi
Bu konuda oluşturulan sınıflandırma ve regresyon modellerini nasıl puanlandırıp değerlendireceklerini öğrenmek için [Puan'a bakın ve Kıvılcım yapımı makine öğrenimi modellerini değerlendirin.](spark-model-consumption.md)

### <a name="cross-validation-and-hyperparameter-sweeping"></a>Çapraz doğrulama ve hiperparametre süpürme
Spark ile modellerin çapraz doğrulama ve hiper parametre süpürme kullanılarak nasıl eğitilebileceğine ilişkin Gelişmiş veri arama ve [modellemeye](spark-advanced-data-exploration-modeling.md) bakın.


## <a name="predict-taxi-tips-using-scala-on-azure-spark"></a>Azure Kıvılcımı'nda Scala'yı kullanarak taksi ipuçlarını tahmin etme

[Azure'da Spark ile Scala Kullanımı,](scala-walkthrough.md) bir bahşişin ödenip ödenmediğini ve ödenmesi beklenen tutarların aralığını tahmin eder. Azure HDInsight Spark kümesindeki Spark makine öğrenimi kitaplığı (MLlib) ve SparkML paketleriyle denetlenen makine öğrenimi görevleri için Scala'nın nasıl kullanılacağını gösterir. [Veri Bilimi Süreci'ni](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)oluşturan görevlerde size yol açar: veri alma ve araştırma, görselleştirme, özellik mühendisliği, modelleme ve model tüketimi. Üretilen modeller arasında lojistik ve doğrusal regresyon, rastgele ormanlar ve degrade artırılmış ağaçlar yer almaktadır.


## <a name="next-steps"></a>Sonraki adımlar

Ekip Veri Bilimi Sürecine genel bakış [için, Takım Veri Bilimi Sürecine genel bakış](overview.md)bölümüne bakın.

Ekip Veri Bilimi Süreci yaşam döngüsünü tartışmak [için, Bkz. Takım Veri Bilimi Süreci yaşam döngüsü.](lifecycle.md) Bu yaşam döngüsü, projelerin genellikle yürütüldüğünde takip ettiği adımları, başından sonuna kadar özetler. 

