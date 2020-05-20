---
title: Otomatik makine öğrenimi çalıştırması için makine öğrenimi görevi tanımlama
titleSuffix: Azure Machine Learning
description: Otomatik makine öğrenimi çalıştırması için makine öğrenimi görevi tanımlama hakkında bilgi edinin
services: machine-learning
author: RachelKellam
ms.author: rakellam
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 11/04/2019
ms.openlocfilehash: cb2fbcda7f30db24b876a66df22071f14df74814
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83653258"
---
# <a name="how-to-define-a-machine-learning-task"></a>Makine öğrenimi görevi tanımlama 
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede desteklenen makine öğrenimi görevlerini ve bunları otomatik makine öğrenimi (otomatik ML) deneme çalıştırması için nasıl tanımlayacağınızı öğrenirsiniz.


## <a name="what-is-a-machine-learning-task"></a><a name="What is a machine learning task?"></a>Machine Learning görevi nedir?

Bir makine öğrenimi görevi, bir tahmine dayalı model oluşturarak çözülemekte olan sorunun türünü temsil eder. Otomatik ML, sınıflandırma, regresyon ve zaman serisi tahmini dahil olmak üzere üç farklı görev türünü destekler.

Görev türü| Açıklama| Örnek
----|----|----
Sınıflandırma | Bir veri kümesindeki belirli bir satırın kategorisini tahmin etmek için görev. | Kredi kartında sahtekarlık algılama. Hedef sütun, *doğru* veya *yanlış*kategorileriyle **sahtekarlık tespit etti** . Bu durumda, verilerdeki her satırı doğru ya da yanlış olarak sınıflandırıyoruz.
Regresyon | Sürekli miktar çıkışını tahmin etmek için görev. | Kendi özelliklerine göre otomobil maliyeti, hedef sütun **Fiyat**olur.
Tahmin etme |Gelecekteki eğilimlerin yönünü belirlemede bilinçli tahminler yapma görevi.| Enerji taleplerini önümüzdeki 48 saat boyunca tahmin edin. Hedef sütun **talep** olur ve tahmini değerler, enerji talebinde desenleri göstermek için kullanılır.

Otomatikleştirilen ML Otomasyon ve ayarlama işlemi sırasında aşağıdaki algoritmaları destekler. Bir kullanıcı olarak, algoritmayı belirtmeniz gerekmez.

Sınıflandırma | Regresyon | Zaman Serileri Tahmini
-- |-- |--
[Lojistik Regresyon](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Elastik Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Elastik Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Hafif GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Hafif GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Hafif GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#classification)|[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Gradyan artırma](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)|[Karar ağacı](https://scikit-learn.org/stable/modules/tree.html#regression)
[K en yakın komşuları](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K en yakın komşuları](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K en yakın komşuları](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[Doğrusal SVC](https://scikit-learn.org/stable/modules/svm.html#classification)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[C-destek vektör sınıflandırması (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Stochastic gradyan (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[Stochastic gradyan (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Rastgele Orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Rastgele Orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Rastgele Orman](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Son derece rastgele ağaçlar](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Son derece rastgele ağaçlar](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Son derece rastgele ağaçlar](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)|[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)| [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[DNN sınıflandırıcı](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier)|[DNN gerileme](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor) | [DNN gerileme](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor)|
[DNN doğrusal sınıflandırıcı](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)|[Doğrusal gerileme](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)|[Doğrusal gerileme](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)
[Sade Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)||
[Stochastic gradyan (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)||


### <a name="set-the-task-type"></a>Görev türünü ayarlama
Otomatik ML denemeleri için görev türünü SDK veya Azure Machine Learning Studio ile ayarlayabilirsiniz.

`task` `AutoMLConfig` Deneme türünü belirtmek için oluşturucuda parametresini kullanın.

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task="classification")
```

Görevi, Azure Machine Learning Studio 'da otomatik ML deneme çalıştırması oluşturma ' nın bir parçası olarak ayarlayabilirsiniz. 

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

![Görev türü seçimi](./media/how-to-define-task-type/task-type.png)


## <a name="next-steps"></a>Sonraki adımlar

+ Azure Machine Learning 'de [Otomatikleştirilmiş ml](concept-automated-ml.md) hakkında daha fazla bilgi edinin.
+ Azure Machine Learning [bir zaman serisi tahmin modeli için otomatik eğitim](how-to-auto-train-forecast.md) hakkında daha fazla bilgi edinin
+ [Otomatik Machine Learning sınıflandırma](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb) öğreticisini deneyin.
+ [Otomatik Machine Learning gerileme](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/regression-explanation-featurization/auto-ml-regression-explanation-featurization.ipynb) örnek Not defterini deneyin.

