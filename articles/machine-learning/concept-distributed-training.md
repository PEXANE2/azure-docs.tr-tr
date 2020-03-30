---
title: Dağıtılmış eğitim nedir?
titleSuffix: Azure Machine Learning
description: Dağıtılmış eğitim ve Azure Machine Learning'in bu eğitimi nasıl desteklediği hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
author: nibaccam
ms.author: nibaccam
ms.subservice: core
ms.topic: conceptual
ms.date: 03/27/2020
ms.openlocfilehash: a0d5bf795e4759a105b9a235770f37aa10bd6751
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385551"
---
# <a name="distributed-training-with-azure-machine-learning"></a>Azure Machine Learning ile dağıtılmış eğitim

Bu makalede, dağıtılmış eğitim ve Azure Machine Learning'in derin öğrenme modelleri için bu eğitimi nasıl desteklediği hakkında bilgi edinebilirsiniz. 

Dağıtılmış eğitimde, bir modeli eğitmek için iş yükü bölünür ve işçi düğümleri adı verilen birden çok mini işlemci arasında paylaşılır. Bu işçi düğümleri model eğitimini hızlandırmak için paralel olarak çalışır. Dağıtılmış eğitim geleneksel ML modelleri için kullanılabilir, ancak derin sinir ağları eğitimi için [derin öğrenme](concept-deep-learning-vs-machine-learning.md) gibi daha iyi bilgi işlem ve zaman yoğun görevler için uygundur.

## <a name="deep-learning-and-distributed-training"></a>Derin öğrenme ve dağıtılmış eğitim 

Dağıtılmış eğitimin iki ana türü vardır: [veri paralelliği](#data-parallelism) ve [model paralelciliği.](#model-parallelism) [Python'daki Azure Machine Learning SDK,](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) derin öğrenme modelleri üzerinde dağıtılmış eğitim için popüler çerçeveler, PyTorch ve TensorFlow ile tümleştirmeleri destekler. Her iki çerçeve de dağıtılmış eğitim için veri paralelliği kullanır ve bilgi işlem hızlarını optimize etmek için [horovod'dan](https://horovod.readthedocs.io/en/latest/summary_include.html) yararlanabilir. 

* [PyTorch ile dağıtılmış eğitim](how-to-train-pytorch.md#distributed-training)

* [TensorFlow ile dağıtılmış eğitim](how-to-train-tensorflow.md#distributed-training)

Dağıtılmış eğitim gerektirmeyen ML modelleri için, Python SDK'yı kullanarak modelleri eğitmenin farklı yolları için [Azure Machine Learning'e sahip tren modellerine](concept-train-machine-learning-model.md#python-sdk) bakın.

## <a name="data-parallelism"></a>Veri paralelliği

Veri paralelciliği, dağıtılan iki eğitim yaklaşımının uygulanması en kolay olandır ve çoğu kullanım örneğinde yeterlidir.

Bu yaklaşımda, veriler, işlem kümesindeki kullanılabilir düğüm lerin toplam sayısına eşit olan bölümlere ayrılır. Model bu alt düğümlerin her birinde kopyalanır ve her işçi verilerin kendi alt kümesi üzerinde çalışır. Her düğümün eğitilmekte olan modeli destekleme kapasitesine sahip olması gerektiğini, yani modelin her düğüme tamamen sığması gerektiğini unutmayın.

Her düğüm, eğitim örnekleri için öngörüleri ile etiketli çıktılar arasındaki hataları bağımsız olarak hesaplar. Buna karşılık, her düğüm kendi modelini hatalara göre güncelleştirir ve ilgili modellerini güncelleştirmek için tüm değişikliklerini diğer düğümlere iletmesi gerekir. Bu, işçi düğümlerinin tutarlı bir modeli eğittiklerinden emin olmak için toplu işlemin sonunda model parametrelerini veya degradeleri eşitlemeleri gerektiği anlamına gelir. 

## <a name="model-parallelism"></a>Model paralellik

Ağ paralelliği olarak da bilinen model paralellikte, model farklı düğümlerde aynı anda çalışabilen farklı parçalara ayrılır ve her biri aynı veriler üzerinde çalışır. Bu yöntemin ölçeklenebilirliği algoritmanın görev paralelleştirme derecesine bağlıdır ve uygulamak veri paralelciliğinden daha karmaşıktır. 

Model paralellikte, alt düğümlerin paylaşılan parametreleri genellikle her ileri veya geri yayılım adımı için bir kez eşitlemeleri gerekir. Ayrıca, her düğüm aynı eğitim verileri üzerinde modelin bir alt bölümünde çalıştığından, daha büyük modeller bir sorun değildir.

## <a name="next-steps"></a>Sonraki adımlar

* Python SDK ile eğitim ortamlarını nasıl [ayarlayınız](how-to-set-up-training-targets.md) öğrenin.
* Teknik bir örnek [için, başvuru mimarisi senaryosuna](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/training-deep-learning)bakın.
* [TensorFlow ile TRAIN ML modelleri.](how-to-train-tensorflow.md)
* [PyTorch ile TRAIN ML modelleri](how-to-train-pytorch.md). 