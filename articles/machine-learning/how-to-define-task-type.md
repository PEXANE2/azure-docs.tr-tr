---
title: Otomatik makine öğrenimi çalışması için makine öğrenimi görevi tanımlayın
titleSuffix: Azure Machine Learning
description: Otomatik makine öğrenimi çalışması için makine öğrenimi görevini nasıl tanımlayın
services: machine-learning
author: RachelKellam
ms.author: rakellam
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 11/04/2019
ms.openlocfilehash: fef3f0347d963b8a7d5fe2f1ca771293e9c3d2a9
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80475507"
---
# <a name="how-to-define-a-machine-learning-task"></a>Makine öğrenimi görevi nasıl tanımlanır? 
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, desteklenen makine öğrenimi görevlerini ve bunları otomatik leştirilmiş bir makine öğrenimi (otomatik ML) deneme çalışması için nasıl tanımlayabileceğinizi öğrenirsiniz.


## <a name="what-is-a-machine-learning-task"></a><a name="What is a machine learning task?"></a>Makine öğrenimi görevi nedir?

Makine öğrenimi görevi, tahmine dayalı bir model oluşturarak çözülen sorun türünü temsil eder. Otomatik ML sınıflandırma, regresyon ve zaman serisi tahmini de dahil olmak üzere üç farklı türde görevi destekler.

Görev türü| Açıklama| Örnek
----|----|----
Sınıflandırma | Bir veri kümesinde belirli bir satırın kategorisini tahmin etme görevi. | Kredi kartında dolandırıcılık tespiti. Hedef *sütun, Doğru* veya *Yanlış*kategorilerinde **Algılanan Sahtekarlık** olacaktır. Bu durumda, verilerdeki her satırı doğru veya yanlış olarak sınıflandırıyoruz.
Regresyon | Sürekli bir miktar çıktısını tahmin etme görevi. | Otomobil maliyeti özelliklerine göre, hedef sütun **fiyat**olacaktır.
Tahmin etme |Gelecekteki eğilimlerin yönünü belirlemede bilinçli tahminlerde bulunma görevi.| Önümüzdeki 48 saat boyunca enerji talebini tahmin edin. Hedef sütun **talep** olacak ve öngörülen değerler enerji talebindeki desenleri göstermek için kullanılacaktır.

Otomatik ML, otomasyon ve aparat işlemi sırasında aşağıdaki algoritmaları destekler. Bir kullanıcı olarak algoritmayı belirtmenize gerek yoktur.

Sınıflandırma | Regresyon | Zaman Serileri Tahmini
-- |-- |--
[Lojistik Regresyon](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Elastik Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Elastik Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Hafif GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Hafif GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Hafif GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Degrade Artırma](https://scikit-learn.org/stable/modules/ensemble.html#classification)|[Degrade Artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Degrade Artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Karar Ağacı](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Karar Ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)|[Karar Ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)
[K En Yakın Komşular](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K En Yakın Komşular](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K En Yakın Komşular](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[Lineer SVC](https://scikit-learn.org/stable/modules/svm.html#classification)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[C-Destek Vektör Sınıflandırması (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Stokhastik Gradyan İniş (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[Stokhastik Gradyan İniş (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Rastgele Orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Rastgele Orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Rastgele Orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Son derece randomize ağaçlar](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Son derece randomize ağaçlar](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Son derece randomize ağaçlar](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)|[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)| [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[DNN Sınıflandırıcı](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier)|[DNN Regresör](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor) | [DNN Regresör](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor)|
[DNN Lineer Sınıflandırıcı](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)|[Lineer Regresör](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)|[Lineer Regresör](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)
[Sade Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)||
[Stokhastik Gradyan İniş (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)||


### <a name="set-the-task-type"></a>Görev türünü ayarlama
SDK veya Azure Machine Learning stüdyosuyla yaptığınız otomatik ML denemeleri için görev türünü ayarlayabilirsiniz.

Deneme `task` türünü belirtmek `AutoMLConfig` için oluşturucudaki parametreyi kullanın.

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task="classification")
```

Görevi Azure Machine Learning stüdyosunda otomatik ML deneme çalıştırma oluşturmanızın bir parçası olarak ayarlayabilirsiniz. 

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

![Görev türü seçimi](./media/how-to-define-task-type/task-type.png)


## <a name="next-steps"></a>Sonraki adımlar

+ Azure Machine Learning'de [otomatik ml](concept-automated-ml.md) hakkında daha fazla bilgi edinin.
+ Azure Machine Learning'de [bir zaman serisi tahmin modelini otomatik olarak eğitme](how-to-auto-train-forecast.md) hakkında daha fazla bilgi edinin
+ Otomatik [Makine Öğrenimi Sınıflandırma](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb) sıyrıkını deneyin.
+ Otomatik [Makine Öğrenme Regresyon](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/regression-hardware-performance-explanation-and-featurization/auto-ml-regression-hardware-performance-explanation-and-featurization.ipynb) örnek not defterini deneyin.

