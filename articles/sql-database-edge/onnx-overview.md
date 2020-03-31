---
title: Azure SQL Veritabanı Kenarı Önizlemesinde ONNX ile makine öğrenimi ve Yapay AI | Microsoft Dokümanlar
description: Azure SQL Veritabanı Kenarı Önizleme'de makine öğrenimi, Açık Nöral Ağ Değişimi (ONNX) biçimindeki modelleri destekler. ONNX, çeşitli makine öğrenimi çerçeveleri ve araçları arasında modelleri değiştirmek için kullanabileceğiniz açık bir biçimdir.
keywords: sql veritabanı kenarını dağıtmak
services: sql-database-edge
ms.service: sql-database-edge
ms.subservice: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.date: 03/26/2020
ms.openlocfilehash: 7813a08b6b18e517b81e8c4bfac660d198eba7f7
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366266"
---
# <a name="machine-learning-and-ai-with-onnx-in-sql-database-edge-preview"></a>SQL Database Edge Preview'da ONNX ile makine öğrenimi ve Yapay AI

Azure SQL Veritabanı Kenarı Önizleme'de makine [öğrenimi, Açık Nöral Ağ Değişimi (ONNX)](https://onnx.ai/) biçimindeki modelleri destekler. ONNX çeşitli [makine öğrenme çerçeveleri ve araçları](https://onnx.ai/supported-tools)arasında modelleri değiştirmek için kullanabileceğiniz açık bir formattır.

## <a name="overview"></a>Genel Bakış

Azure SQL Veritabanı Kenarı'ndaki makine öğrenimi modellerini çıkarmak için öncelikle bir model almanız gerekir. Bu önceden eğitilmiş bir model veya seçtiğiniz çerçeve ile eğitilmiş özel bir model olabilir. Azure SQL Veritabanı Kenarı ONNX biçimini destekler ve modeli bu biçime dönüştürmeniz gerekir. Model doğruluğu üzerinde hiçbir etkisi olmamalıdır ve ONNX modeline sahip olduktan sonra, modeli Azure SQL Veritabanı Kenarı'nda dağıtabilir ve [PREDICT T-SQL işlevi ile yerel puanlamayı](/sql/advanced-analytics/sql-native-scoring/)kullanabilirsiniz.

## <a name="get-onnx-models"></a>ONNX modellerini alın

ONNX formatında bir model elde etmek için:

- **Model Oluşturma Hizmetleri**: Azure Machine Learning ve Azure Custom Vision [Service'deki otomatik Machine Learning özelliği](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) gibi hizmetler, eğitilmiş modeli DOĞRUDAN ONNX formatında dışa aktarır. [Azure Custom Vision Service](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier)

- [**Mevcut modelleri dönüştürün ve/veya dışa aktarın**](https://github.com/onnx/tutorials#converting-to-onnx-format): Çeşitli eğitim çerçeveleri (örneğin [PyTorch,](https://pytorch.org/docs/stable/onnx.html)Chainer ve Caffe2) yerel ihracat işlevselliğini ONNX'e destekler ve bu da eğitimli modelinizi ONNX biçiminin belirli bir sürümüne kaydetmenizi sağlar. Yerel dışa aktarmayı desteklemeyen çerçeveler için, farklı makine öğrenimi çerçevelerinden eğitilmiş modelleri ONNX biçimine dönüştürmenize olanak tanıyan bağımsız ONNX Dönüştürülebilir paketler vardır.

     **Desteklenen çerçeveler**
   * [PyTorch](http://pytorch.org/docs/master/onnx.html)
   * [Tensör akışı](https://github.com/onnx/tensorflow-onnx)
   * [Keras](https://github.com/onnx/keras-onnx)
   * [Scikit-öğren](https://github.com/onnx/sklearn-onnx)
   * [CoreML](https://github.com/onnx/onnxmltools)
    
    Desteklenen çerçeveler ve örneklerin tam listesi [için](https://github.com/onnx/tutorials#converting-to-onnx-format)bkz.

## <a name="limitations"></a>Sınırlamalar

Şu anda, tüm ONNX modelleri Azure SQL Database Edge tarafından desteklenmez. Destek **sayısal veri tipleri**olan modeller ile sınırlıdır:

- [int ve bigint](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql)
- [gerçek ve float](https://docs.microsoft.com/sql/t-sql/data-types/float-and-real-transact-sql).
  
Diğer sayısal türler [CAST ve CONVERT](https://docs.microsoft.com/sql/t-sql/functions/cast-and-convert-transact-sql)kullanılarak desteklenen türlere dönüştürülebilir.

Model girişleri, modele yapılan her girişin bir tablodaki tek bir sütuna karşılık gelecek şekilde yapılandırılmalıdır. Örneğin, bir modeli eğitmek için bir pandalar veri çerçevesi kullanıyorsanız, her giriş modeliçin ayrı bir sütun olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalı üzerinden SQL Veritabanı Kenarı dağıtma](deploy-portal.md)
- [Azure SQL Veritabanı Kenarı Önizlemesi'nde bir ONNX modeli dağıtma](deploy-onnx.md)
