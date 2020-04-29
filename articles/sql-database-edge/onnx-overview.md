---
title: Azure SQL veritabanı Edge önizlemesinde ONNX ile Machine Learning ve AI | Microsoft Docs
description: Azure SQL veritabanı Edge önizlemesinde makine öğrenimi, Open sinir Network Exchange (ONNX) biçimindeki modelleri destekler. ONNX, çeşitli makine öğrenimi çerçeveleri ve araçları arasındaki modelleri değiş tokuş etmek için kullanabileceğiniz bir açık biçimdir.
keywords: SQL veritabanı ucunu dağıtma
services: sql-database-edge
ms.service: sql-database-edge
ms.subservice: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.date: 03/26/2020
ms.openlocfilehash: 7813a08b6b18e517b81e8c4bfac660d198eba7f7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80366266"
---
# <a name="machine-learning-and-ai-with-onnx-in-sql-database-edge-preview"></a>SQL veritabanı Edge önizlemesinde ONNX ile Machine Learning ve AI

Azure SQL veritabanı Edge önizlemesinde makine öğrenimi, [Open sinir Network Exchange (ONNX)](https://onnx.ai/) biçimindeki modelleri destekler. ONNX, çeşitli [makine öğrenimi çerçeveleri ve araçları](https://onnx.ai/supported-tools)arasındaki modelleri değiş tokuş etmek için kullanabileceğiniz bir açık biçimdir.

## <a name="overview"></a>Genel Bakış

Azure SQL veritabanı Edge 'de makine öğrenimi modellerini çıkarsmak için öncelikle bir model almanız gerekir. Bu, önceden eğitilen bir model veya tercih ettiğiniz çerçeveye eğitilen özel bir model olabilir. Azure SQL veritabanı Edge ONNX biçimini destekler ve modeli bu biçime dönüştürmeniz gerekir. Model doğruluğunun etkilenmemesi gerekir ve ONNX modeline sahip olduğunuzda modeli Azure SQL veritabanı Edge 'de dağıtabilir ve [ön tahmin T-SQL işleviyle yerel Puanlama](/sql/advanced-analytics/sql-native-scoring/)kullanabilirsiniz.

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

Şu anda tüm ONNX modelleri Azure SQL veritabanı Edge tarafından desteklenmez. Destek, **sayısal veri türleri**olan modellerle sınırlıdır:

- [int ve bigint](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql)
- [gerçek ve float](https://docs.microsoft.com/sql/t-sql/data-types/float-and-real-transact-sql).
  
Diğer sayısal türler, [dönüştürme ve dönüştürme](https://docs.microsoft.com/sql/t-sql/functions/cast-and-convert-transact-sql)kullanılarak desteklenen türlere dönüştürülebilir.

Model girişleri, modeldeki her girdinin tablodaki tek bir sütuna karşılık gelmesi için yapılandırılmalıdır. Örneğin, bir modeli eğitebilmeniz için bir Pandas dataframe kullanıyorsanız, her giriş modele ayrı bir sütun olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal aracılığıyla SQL veritabanı ucunu dağıtma](deploy-portal.md)
- [Azure SQL veritabanı Edge önizlemesine bir ONNX modeli dağıtma](deploy-onnx.md)
