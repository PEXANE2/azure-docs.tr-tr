---
title: Makine öğrenimi modellerini Apache Spark ile eğitme
description: Makine öğrenimi modellerini eğitmek için Azure SYNAPSE Analytics 'te Apache Spark kullanma
author: midesa
ms.author: midesa
ms.reviewer: euang
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: machine-learning
ms.date: 09/13/2020
ms.openlocfilehash: 8250f7154cfb2739d065980284da67a200680d89
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95920141"
---
# <a name="train-machine-learning-models"></a>Makine öğrenimi modellerini eğitme
Azure SYNAPSE Analytics 'teki Apache Spark, büyük miktarlarda yapılandırılmış, yapılandırılmamış ve hızlı hareketli verilerden değerli Öngörüler elde etme olanağı sunan büyük verilerle makine öğrenmesini sağlar. Azure SYNAPSE Analytics 'te Azure Spark kullanarak makine öğrenimi modellerini eğitmek için çeşitli seçenekler vardır: Apache Spark MLlib, Azure Machine Learning ve diğer diğer açık kaynak kitaplıkları. 

## <a name="apache-sparkml-and-mllib"></a>Apache mini ml ve MLlib
Azure SYNAPSE Analytics 'te Apache Spark, Microsoft 'un buluttaki Apache Spark uygulamalarından biridir. Büyük veri analizlerini artırmak üzere bellek içi işlemeyi destekleyen Birleşik, açık kaynaklı ve paralel veri işleme çerçevesini sağlar. Spark işleme altyapısı hız, kullanım kolaylığı ve gelişmiş analiz için oluşturulmuştur. Spark 'ın bellek içi dağıtılmış hesaplama özellikleri, makine öğrenimi ve grafik hesaplamaları 'nda kullanılan yinelemeli algoritmalar için iyi bir seçenek yapar. 

Algoritmik modelleme yeteneklerini bu dağıtılmış ortama getiren iki ölçeklenebilir makine öğrenimi kitaplığı vardır: MLlib ve mini-ml. MLlib, RDD 'lerin üstünde oluşturulmuş özgün API 'yi içerir. Parlak ml, ML işlem hatları oluşturmak için veri çerçevelerinin üzerine inşa edilen daha yüksek düzeyde bir API sağlayan daha yeni bir pakettir. Parlak ml, MLlib özelliklerinin tümünü henüz desteklemez, ancak MLlib 'i Spark 'ın Standart makine öğrenimi kitaplığı olarak değiştiriyor.

> [!NOTE]
> 
> Bu [öğreticiyi](../spark/apache-spark-azure-machine-learning-tutorial.md)izleyerek mini bir ml modeli oluşturma hakkında daha fazla bilgi edinebilirsiniz.

## <a name="popular-libraries"></a>Popüler kitaplıklar
Azure SYNAPSE Analytics 'teki tüm Apache Spark havuzları, önceden yüklenmiş ve popüler makine öğrenimi kitaplıkları kümesiyle birlikte gelir. Bu kitaplıklar, programlarınıza veya projelerinize dahil etmek isteyebileceğiniz yeniden kullanılabilir kodu sağlar. Varsayılan olarak eklenen ilgili makine öğrenimi kitaplıklarının bazıları şunlardır:
- [Scikit-](https://scikit-learn.org/stable/index.html) klasik ml algoritmaları için en popüler tek düğümlü makine öğrenimi kitaplıklarından biridir. Scikit-denetimli ve denetlenmediğini öğrenme algoritmalarının çoğunu destekler ve veri araştırma ve veri analizi için de kullanılabilir.
  
- [Xgboost](https://xgboost.readthedocs.io/en/latest/) , eğitim kararı ağaçları ve rastgele ormanlar için iyileştirilmiş algoritmalar içeren popüler bir makine öğrenme kitaplığıdır. 
  
- [Pytorch](https://pytorch.org/)  &  [TensorFlow](https://www.tensorflow.org/) , güçlü Python derin öğrenme kitaplıklarıdır. Azure SYNAPSE Analytics 'teki bir Apache Spark havuzunda, havuzunuzdaki yürüticilerinin sayısını sıfıra ayarlayarak tek makineli modeller oluşturmak için bu kitaplıkları kullanabilirsiniz. Bu yapılandırmada Apache Spark çalışmayabilse de, tek makineli modeller oluşturmanın basit ve ekonomik bir yoludur.

Yayımlanan [Azure SYNAPSE Analytics çalışma zamanını](../spark/apache-spark-version-support.md)görüntüleyerek kullanılabilir kitaplıklar ve ilgili sürümler hakkında daha fazla bilgi edinebilirsiniz.

## <a name="mmlspark"></a>MMLSpark
Apache Spark için Microsoft Machine Learning kitaplığı, [Mmlspark](https://github.com/Azure/mmlspark). Bu kitaplık, veri bilimcilerinin Spark üzerinde daha üretken olmasını, deneme oranını artırması ve büyük veri kümelerinde derin öğrenme dahil olmak üzere son teknoloji makine öğrenimi tekniklerinden yararlanmasını sağlamak için tasarlanmıştır. 

MMLSpark, dizin oluşturma dizeleri, makine öğrenimi algoritmaları tarafından beklenen bir düzene zorlama verileri ve özellik vektörlerini montaj gibi ölçeklenebilir ML modelleri derlerken, modern ml 'nin alt düzey API 'Lerinin üzerine bir katman sağlar. MMLSpark kitaplığı, PySpark 'da model oluşturmaya yönelik bu ve diğer yaygın görevleri basitleştirir.

## <a name="automated-ml-in-azure-machine-learning"></a>Azure Machine Learning otomatik ML 
Azure Machine Learning, makine öğrenimi modellerini eğmenize, dağıtmanıza, otomatikleştirmenize, yönetmenize ve izlemenize olanak tanıyan bulut tabanlı bir ortamdır. Azure Machine Learning içindeki otomatik ML (otomatik ml), eğitim verilerini ve yapılandırma ayarlarını kabul eder ve en iyi modele ulaşmak için farklı özellik normalleştirme/standartlaştırma yöntemlerinin, modellerinin ve hiper parametre ayarlarının birleşimleri üzerinden otomatik olarak yinelenir. 

Azure SYNAPSE Analytics 'te oto ml kullanırken, kimlik doğrulama & modeli eğitimi basitleştirecek farklı hizmetler arasında derin tümleştirmeden yararlanabilirsiniz. 

> [!NOTE]
> 
> Bu [öğreticiyi](./spark/../apache-spark-azure-machine-learning-tutorial.md)izleyerek Azure Machine Learning bir oto ml denemesi oluşturma hakkında daha fazla bilgi edinebilirsiniz.

## <a name="azure-cognitive-services"></a>Azure Bilişsel Hizmetler
[Azure](https://docs.microsoft.com/azure/cognitive-services/what-are-cognitive-services) bilişsel hizmetler, nesnelerin veya yüzlerin tanınması için metin analizi veya görüntüleri analiz etme gibi genel sorunları çözümlemek için makine öğrenimi özellikleri sağlar. Bu hizmetleri kullanmak için makine öğrenmesi veya veri bilimi alanında bilgi sahibi olmanıza gerek yoktur. Bilişsel hizmet, bir makine öğrenimi çözümünde bileşenlerin bir kısmını veya tümünü sağlar: veri, algoritma ve eğitilen model. Bu hizmetler, makine öğrenimi veya veri bilimi konusunda deneyim sahibi olmadan verileriniz hakkında genel bilgi gerektirmek üzere tasarlanmıştır. Bu önceden eğitilen hizmetlerden Azure SYNAPSE Analytics içinde otomatik olarak yararlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure SYNAPSE Analytics 'teki Apache Spark havuzlarında makine öğrenimi modellerini eğitmek için çeşitli seçeneklere genel bakış sunulmaktadır. Aşağıdaki öğreticiyi izleyerek model eğitimi hakkında daha fazla bilgi edinebilirsiniz:

- Azure Machine Learning ve Azure SYNAPSE Analytics kullanarak otomatikleştirilmiş ML denemeleri çalıştırma: [Otomatikml öğreticisi](./spark/../apache-spark-autoscale.md) 
- Parlak ml denemeleri çalıştırma: [Apache mini ml öğreticisi](../spark/apache-spark-azure-machine-learning-tutorial.md)
- Varsayılan kitaplıkları görüntüleme: [Azure SYNAPSE Analytics çalışma zamanı](../spark/apache-spark-version-support.md)