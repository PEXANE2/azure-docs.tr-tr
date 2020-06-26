---
title: Azure SQL Edge 'de ONNX ile makine öğrenimi ve AI (Önizleme)
description: Azure SQL Edge 'de makine öğrenimi (Önizleme), Open sinir Network Exchange (ONNX) biçimindeki modelleri destekler. ONNX, çeşitli makine öğrenimi çerçeveleri ve araçları arasındaki modelleri değiş tokuş etmek için kullanabileceğiniz bir açık biçimdir.
keywords: SQL Edge dağıtma
services: sql-edge
ms.service: sql-edge
ms.subservice: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.date: 05/19/2020
ms.openlocfilehash: 2db6c728ac35c6fc2f1fee1a602725371e448104
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85368065"
---
# <a name="machine-learning-and-ai-with-onnx-in-sql-edge-preview"></a>SQL Edge 'de ONNX ile Machine Learning ve AI (Önizleme)

Azure SQL Edge 'de makine öğrenimi (Önizleme), [Open sinir Network Exchange (ONNX)](https://onnx.ai/) biçimindeki modelleri destekler. ONNX, çeşitli [makine öğrenimi çerçeveleri ve araçları](https://onnx.ai/supported-tools)arasındaki modelleri değiş tokuş etmek için kullanabileceğiniz bir açık biçimdir.

## <a name="overview"></a>Genel Bakış

Azure SQL Edge 'de makine öğrenimi modellerini çıkarması için öncelikle bir model almanız gerekir. Bu, önceden eğitilen bir model veya tercih ettiğiniz çerçeveye eğitilen özel bir model olabilir. Azure SQL Edge ONNX biçimini destekler ve modeli bu biçime dönüştürmeniz gerekir. Model doğruluğunun etkilenmemesi gerekir ve ONNX modeline sahip olduktan sonra modeli Azure SQL Edge 'de dağıtabilir ve [ön tahmin T-SQL işleviyle yerel Puanlama](/sql/advanced-analytics/sql-native-scoring/)kullanabilirsiniz.

## <a name="get-onnx-models"></a>ONNX modellerini al

ONNX biçiminde bir model elde etmek için:

- **Model oluşturma hizmetleri**: Azure Machine Learning ve [Azure özel görüntü işleme hizmeti](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) [Otomatik Machine Learning özelliği](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) gibi hizmetler, eğitilen modelin onnx biçiminde doğrudan verilmesini destekler.

- [**Mevcut modelleri dönüştürme ve/veya dışa aktarma**](https://github.com/onnx/tutorials#converting-to-onnx-format): çeşitli eğitim çerçeveleri (ör. [pytorch](https://pytorch.org/docs/stable/onnx.html), Chainer ve Caffe2), eğitilen modelinizi onnx biçiminin belirli bir sürümüne kaydetmenizi sağlayan onnx 'e yerel dışarı aktarma işlevselliğini destekler. Yerel dışarı aktarmayı desteklemeyen çerçeveler için, farklı makine öğrenme çerçevelerinden eğitilen modelleri ONNX biçimine dönüştürmenizi sağlayan, tek başına ONNX Dönüştürücüsü yüklenebilir paketleri vardır.

     **Desteklenen çerçeveler**
   * [PyTorch](http://pytorch.org/docs/master/onnx.html)
   * [TensorFlow](https://github.com/onnx/tensorflow-onnx)
   * [Keras](https://github.com/onnx/keras-onnx)
   * [Scikit-learn](https://github.com/onnx/sklearn-onnx)
   * [CoreML](https://github.com/onnx/onnxmltools)
    
    Desteklenen çerçeveler ve örneklerin tam listesi için bkz. [ONNX biçimine dönüştürme](https://github.com/onnx/tutorials#converting-to-onnx-format).

## <a name="limitations"></a>Sınırlamalar

Şu anda tüm ONNX modelleri Azure SQL Edge tarafından desteklenmez. Destek, **sayısal veri türleri**olan modellerle sınırlıdır:

- [int ve bigint](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql)
- [gerçek ve float](https://docs.microsoft.com/sql/t-sql/data-types/float-and-real-transact-sql).
  
Diğer sayısal türler, [dönüştürme ve dönüştürme](https://docs.microsoft.com/sql/t-sql/functions/cast-and-convert-transact-sql)kullanılarak desteklenen türlere dönüştürülebilir.

Model girişleri, modeldeki her girdinin tablodaki tek bir sütuna karşılık gelmesi için yapılandırılmalıdır. Örneğin, bir modeli eğitebilmeniz için bir Pandas dataframe kullanıyorsanız, her giriş modele ayrı bir sütun olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal üzerinden SQL Edge dağıtma](deploy-portal.md)
- [Azure SQL Edge 'de ONNX modeli dağıtma (Önizleme)](deploy-onnx.md)
