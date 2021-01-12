---
title: Apache Spark Machine Learning
description: Bu makalede, Azure SYNAPSE Analytics üzerinde Apache Spark aracılığıyla sunulan Machine Learning ve veri bilimi özelliklerine kavramsal bir genel bakış sunulmaktadır.
services: synapse-analytics
author: midesa
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: machine-learning
ms.date: 11/13/2020
ms.author: midesa
ms.reviewer: jrasnick
ms.openlocfilehash: 0485f697b9360b0f2dfe94fdf07629978b5127c1
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98116934"
---
# <a name="machine-learning-with-apache-spark"></a>Apache Spark ile makine öğrenimi

Azure SYNAPSE Analytics 'teki Apache Spark, büyük miktarlarda yapılandırılmış, yapılandırılmamış ve hızlı hareketli verilerden değerli Öngörüler elde etme olanağı sunan büyük verilerle makine öğrenmesini sağlar. 

Bu bölüm, araştırmacı veri analizi, özellik Mühendisliği, model eğitimi, model Puanlama ve dağıtım dahil olmak üzere makine öğrenimi iş akışları için genel bakış ve öğreticiler içerir.  

## <a name="synapse-runtime"></a>SYNAPSE çalışma zamanı 
SYNAPSE çalışma zamanı, veri bilimi ve makine öğrenimi için iyileştirilmiş, seçkin bir ortamdır. SYNAPSE çalışma zamanı, varsayılan olarak Azure Machine Learning SDK 'da bir dizi popüler açık kaynaklı kitaplık ve derleme sunar. SYNAPSE çalışma zamanı, PyTorch, Scikit-öğren, XGBoost ve daha fazlası dahil olmak üzere birçok Harici kitaplık da içerir.

Yayımlanan [Azure SYNAPSE Analytics çalışma zamanını](../spark/apache-spark-version-support.md)görüntüleyerek kullanılabilir kitaplıklar ve ilgili sürümler hakkında daha fazla bilgi edinin.

## <a name="exploratory-data-analysis"></a>Araştırmacı veri analizi
Azure SYNAPSE Analytics 'te Apache Spark kullanırken, verilerinizi görselleştirmenize yardımcı olan SYNAPSE Not defteri grafik seçenekleri, Seaborn ve Matplotlib gibi popüler açık kaynaklı kitaplıklara erişim ve SYNAPSE SQL ve Power BI tümleştirmesi gibi çeşitli yerleşik seçenekler vardır.

[Azure SYNAPSE not defterlerini kullanarak verileri görselleştirmeyle](../spark/apache-spark-data-visualization.md)ilgili makaleyi görüntüleyerek veri görselleştirme ve veri analizi seçenekleri hakkında daha fazla bilgi edinin.

## <a name="feature-engineering"></a>Özellik mühendisliği
Varsayılan olarak, SYNAPSE çalışma zamanı, özellik Mühendisliği için yaygın olarak kullanılan bir kitaplıklar kümesi içerir. Büyük veri kümeleri için Spark SQL, MLlib ve Koalas özelliklerini özellik Mühendisliği için kullanabilirsiniz. Daha küçük veri kümeleri için, sayısal tuş y, Pandas ve Scikit gibi üçüncü taraf kitaplıkları, bu senaryolar için yararlı yöntemler de sağlar.

## <a name="train-models"></a>Modelleri eğitme
Azure SYNAPSE Analytics 'te Azure Spark kullanarak makine öğrenimi modellerini eğitmek için çeşitli seçenekler vardır: Apache Spark MLlib, Azure Machine Learning ve diğer diğer açık kaynak kitaplıkları. 

[Azure SYNAPSE Analytics 'te modelleri eğitme](../spark/apache-spark-machine-learning-training.md)hakkında makaleyi görüntüleyerek makine öğrenimi özellikleri hakkında daha fazla bilgi edinin.

### <a name="sparkml-and-mllib"></a>Mini ml ve MLlib
Spark 'ın bellek içi dağıtılmış hesaplama özellikleri, makine öğrenimi ve grafik hesaplamaları 'nda kullanılan yinelemeli algoritmalar için iyi bir seçenek yapar. ```spark.ml``` , kullanıcıların makine öğrenimi işlem hatlarını oluşturmasına ve ayarlamaya yardımcı olan Tekdüzen bir üst düzey API kümesi sağlar. Hakkında daha fazla bilgi edinmek için ```spark.ml``` [Apache Spark ml programlama kılavuzunu](https://spark.apache.org/docs/1.2.2/ml-guide.html)ziyaret edebilirsiniz.

### <a name="azure-machine-learning-automated-ml"></a>Azure Machine Learning otomatikleştirilen ML
[Azure Machine Learning OTOMATIKLEŞTIRILEN ml](../../machine-learning/concept-automated-ml.md) (otomatik makine öğrenimi), makine öğrenimi modellerini geliştirme sürecini otomatikleştirmenize yardımcı olur. Veri bilimcilerinin, analistlerin ve geliştiricilerin, model kalitesini sürdürüp yüksek ölçekli, verimlilik ve üretkenlik özelliklerine sahip ML modelleri oluşturmalarına olanak tanır. Otomatikleştirilmiş ML SDK Azure Machine Learning çalıştırmak için bileşenler doğrudan SYNAPSE çalışma zamanına yerleştirilmiştir.

### <a name="open-source-libraries"></a>Açık kaynak kitaplıkları
Azure SYNAPSE Analytics 'teki tüm Apache Spark havuzları, önceden yüklenmiş ve popüler makine öğrenimi kitaplıkları kümesiyle birlikte gelir.  Varsayılan olarak eklenen ilgili makine öğrenimi kitaplıklarının bazıları şunlardır:

- [Scikit-](https://scikit-learn.org/stable/index.html) klasik ml algoritmaları için en popüler tek düğümlü makine öğrenimi kitaplıklarından biridir. Scikit-denetimli ve denetlenmediğini öğrenme algoritmalarının çoğunu destekler ve veri araştırma ve veri analizi için de kullanılabilir.
  
- [Xgboost](https://xgboost.readthedocs.io/en/latest/) , eğitim kararı ağaçları ve rastgele ormanlar için iyileştirilmiş algoritmalar içeren popüler bir makine öğrenme kitaplığıdır. 
  
- [Pytorch](https://pytorch.org/)  &  [TensorFlow](https://www.tensorflow.org/) , güçlü Python derin öğrenme kitaplıklarıdır. Azure SYNAPSE Analytics 'teki bir Apache Spark havuzunda, havuzunuzdaki yürüticilerinin sayısını sıfıra ayarlayarak tek makineli modeller oluşturmak için bu kitaplıkları kullanabilirsiniz. Bu yapılandırmada Apache Spark çalışmayabilse de, tek makineli modeller oluşturmanın basit ve ekonomik bir yoludur.

## <a name="track-model-development"></a>Model geliştirmeyi izleme
[Mlflow](https://www.mlflow.org/) , Machine Learning denemeleri 'in yaşam döngüsünü yönetmeye yönelik açık kaynaklı bir kitaplıktır. MLFlow Izlemesi, öğreticinizi günlüğe kaydeden ve izleyen bir MLflow bileşenidir. Azure SYNAPSE Analytics ve Azure Machine Learning aracılığıyla MLFlow Izlemeyi nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için, [mlflow kullanma](../../machine-learning/how-to-use-mlflow.md)hakkında bu öğreticiyi ziyaret edin.

## <a name="model-scoring"></a>Model Puanlama
Model Puanlama veya ınvanı, tahmine dayalı hale getirmek için bir modelin kullanıldığı aşamadır. Mini veri çerçevesinde doğrudan bir Spark XML veya MLLib ile model Puanlama için yerel Spark yöntemlerinden yararlanabilirsiniz. Diğer açık kaynak kitaplıkları ve model türleri için, büyük veri kümelerinde çıkarımı ölçeklendirmek üzere bir Spark UDF de oluşturabilirsiniz. Daha küçük veri kümeleri için, kitaplık tarafından sunulan yerel model çıkarım yöntemlerini de kullanabilirsiniz.

## <a name="register-and-serve-models"></a>Modelleri kaydetme ve sunma
Bir modeli kaydetmek, çalışma alanınızdaki modeller hakkında meta verileri depolamanıza, sürümüne ve izlemenize olanak sağlar. Modelinize eğitim tamamladıktan sonra modelinizi [Azure Machine Learning modeli kayıt defterine](../../machine-learning/concept-model-management-and-deployment.md#register-package-and-deploy-models-from-anywhere)kaydedebilirsiniz. Kaydolduktan sonra ONNX modelleri adanmış SQL havuzlarında depolanan [verileri](../machine-learning/tutorial-sql-pool-model-scoring-wizard.md) zenginleştirmek için de kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
Azure SYNAPSE Analytics 'te Machine Learning 'i kullanmaya başlamak için aşağıdaki öğreticilere göz atın:
- [Azure SYNAPSE Not defterleri ile verileri analiz etme](../spark/apache-spark-data-visualization-tutorial.md)

- [Otomatik ML ile makine öğrenimi modelini eğitme](../spark/apache-spark-azure-machine-learning-tutorial.md)

- [Machine Learning modelini Apache Spark MLlib ile eğitme](../spark/apache-spark-machine-learning-mllib-notebook.md)
