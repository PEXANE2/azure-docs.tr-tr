---
title: HDInsight spark'ta PySpark, Scala - Analytics Team Data Science Process'i
description: Bir Azure HDInsight Spark PySpark ve Scala kullanımını adım adım sunan ekip veri bilimi sürecinin örnekleri.
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
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75864154"
---
# <a name="hdinsight-spark-data-science-walkthroughs-using-pyspark-and-scala-on-azure"></a>PySpark ve Scala Azure'da kullanarak HDInsight Spark veri bilimi kılavuzları

Bu izlenecek yollar PySpark ve Scala, Tahmine dayalı analiz yapmak için bir Azure Spark kümesinde kullanın. Bunlar, Team Data Science Process içinde verilen adımları izleyin. Team Data Science Process genel bakış için bkz: [Data Science Process](overview.md). HDInsight üzerinde Spark genel bakış için bkz. [HDInsight üzerinde Spark giriş](../../hdinsight/spark/apache-spark-overview.md).

Team Data Science Process yürütme ek veri bilimi kılavuzları tarafından gruplanır **platform** kullandıkları. Bkz: [Team Data Science Process yürütme izlenecek yollar](walkthroughs.md) Bu örneklerde bir döküm için.

## <a name="predict-taxi-tips-using-pyspark-on-azure-spark"></a>PySpark Azure Spark kullanarak taksi bahşişlerini tahmin etme

New York TAXI verilerini kullanarak, [Azure HDInsight 'Ta Spark kullanma](spark-overview.md) Kılavuzu, bir ipucunun ödenip ödenmediğini ve beklenen tutarların aralığını tahmin eder. Bu örnek, genel olarak kullanılabilir NYC TAXI yolculuğuna ve tarifeli havayolu veri kümesinden verileri depolamak, araştırmak ve özellik mühendislerinden veri eklemek için bir [Azure HDInsight Spark kümesi](https://azure.microsoft.com/services/hdinsight/) kullanan bir senaryoda Team Data Science işlemini kullanır. Bu genel bakış konusu, HDInsight Spark kümesi ve Jupyıter PySpark not defterlerini kullanır. Bu not defterlerini nasıl oluşturarak verilerinizi araştırmanıza ve nasıl oluşturulup tüketim modelleri görüntüleyin. Gelişmiş Veri keşfi ve modelleme Not Defteri, çapraz doğrulama, hyper-Süpürme saldırısı yapılabilir, parametre eklemek ve değerlendirme modeli gösterilmektedir.

### <a name="data-exploration-and-modeling-with-spark"></a>Spark ile veri keşfi ve modelleme 
Veri kümesini araştırmak ve oluşturma, Puanlama ve makine öğrenimi modellerini aracılığıyla değerlendirmek [Spark MLlib araç seti ile veriler için ikili sınıflandırma ve regresyon modellerini oluşturma](spark-data-exploration-modeling.md) konu.

### <a name="model-consumption"></a>Model tüketim
Bu konu başlığında oluşturduğunuz sınıflandırma ve regresyon modellerini puanlamanıza öğrenmek için bkz. [puanı ve Spark'a yerleşik machine learning modellerini değerlendirme](spark-model-consumption.md).

### <a name="cross-validation-and-hyperparameter-sweeping"></a>Çapraz doğrulama ve hiper parametre Süpürme
Bkz: [Gelişmiş Veri keşfi ve modelleme Spark ile](spark-advanced-data-exploration-modeling.md) modelleri nasıl olabileceğini üzerinde çapraz doğrulama ve hiper parametreli Süpürme kullanarak eğitim.


## <a name="predict-taxi-tips-using-scala-on-azure-spark"></a>Azure üzerinde Spark Scala kullanarak taksi bahşişlerini tahmin etme

[Azure 'Da Spark Ile Scala 'Yı kullanma](scala-walkthrough.md) Kılavuzu, bir ipucunun ödenip ödenmediğini tahmin eder ve ücretlendirildiği tutarların aralığını tahmin eder. Bu, Scala ile Spark makine öğrenimi kitaplığı (MLlib) ve bir Azure HDInsight Spark kümesi üzerinde SparkML paketleri görevleri öğrenme denetimli makine kullanmak nasıl gösterir. Oluşturan görevlerinde size yol gösterir [Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/): veri alımı ve keşfi, görselleştirme, özellik Mühendisliği, modelleme ve model tüketim. Mantıksal ve doğrusal regresyon, rastgele ormanları ve gradyan artırmalı ağaçları modellerinin içerir.


## <a name="next-steps"></a>Sonraki adımlar

Ekip veri bilimi Işlemine genel bakış için bkz. [Team Data Science işlemine genel bakış](overview.md).

Team Data Science Işlem yaşam döngüsü hakkında bir tartışma için bkz. [Team Data Science işlem yaşam döngüsü](lifecycle.md). Bu yaşam döngüsü, başlangıçtan sonuna kadar olan adımları özetler ve bu da projeler genellikle yürütüldüğünde izler. 

