---
title: Azure SQL veritabanı Edge önizlemesinde ONNX ile Machine Learning ve AI | Microsoft Docs
description: Azure SQL veritabanı Edge önizlemesinde makine öğrenimi, Open sinir Network Exchange (ONNX) biçimindeki modelleri destekler. ONNX, çeşitli makine öğrenimi çerçeveleri ve araçları arasındaki modelleri değiş tokuş etmek için kullanabileceğiniz bir açık biçimdir.
keywords: SQL veritabanı ucunu dağıtma
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: ronychatterjee
ms.author: achatter
ms.reviewer: davidph
ms.date: 11/04/2019
ms.openlocfilehash: c4c87f7f6f8735c9a50c61b0e083c77b915e0d98
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73514023"
---
# <a name="machine-learning-and-ai-with-onnx-in-sql-database-edge-preview"></a>SQL veritabanı Edge önizlemesinde ONNX ile Machine Learning ve AI

Azure SQL veritabanı Edge önizlemesinde makine öğrenimi, [Open sinir Network Exchange (ONNX)](https://onnx.ai/) biçimindeki modelleri destekler. ONNX, çeşitli [makine öğrenimi çerçeveleri ve araçları](https://onnx.ai/supported-tools)arasındaki modelleri değiş tokuş etmek için kullanabileceğiniz bir açık biçimdir.

## <a name="supported-tool-kits"></a>Desteklenen araç setleri

ONNXMLTools, farklı makine öğrenme aracı setlerinden bir ONNX modeline modelleri dönüştürmenizi sağlar. Şu anda, sayısal veri türleri ve tek sütunlu girişler için aşağıdaki araç setleri desteklenir:

* [scikit-learn](https://github.com/onnx/sklearn-onnx)
* [Tensorflow](https://github.com/onnx/tensorflow-onnx)
* [Keras](https://github.com/onnx/keras-onnx)
* [CoreML](https://github.com/onnx/onnxmltools)
* [Spark ML (deneysel)](https://github.com/onnx/onnxmltools/tree/master/onnxmltools/convert/sparkml)
* [LightGBM](https://github.com/onnx/onnxmltools)
* [libsvm](https://github.com/onnx/onnxmltools)
* [XGBoost](https://github.com/onnx/onnxmltools)

## <a name="get-onnx-models"></a>ONNX modellerini al

ONNX biçiminde bir model elde etmeniz için çeşitli yollar vardır:

- [Onnx model Zoo](https://github.com/onnx/models): farklı görev türleri için önceden eğitilen bırkaç onnx modeli içerir. Windows Machine Learning tarafından desteklenen sürümleri indirebilir ve kullanabilirsiniz.

- [Machine Learning eğitim çerçevelerinden yerel dışarı aktarma](https://onnx.ai/supported-tools): çeşitli eğitim çerçeveleri, eğitilen MODELINIZI onnx biçiminin belirli bir sürümüne kaydetmenizi sağlayan onnx 'e yerel dışarı aktarma işlevselliğini destekler. Örneğin, Chainer, Caffee2 ve PyTorch. Ayrıca, [Azure Machine Learning hizmeti](https://azure.microsoft.com/services/machine-learning-service/) ve [Azure özel görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) gibi hizmetler yerel onnx dışarı aktarmayı da sağlar.

  - Özel Görüntü İşleme kullanarak bulutta ONNX modelini eğitme ve dışa aktarma hakkında bilgi edinmek için bkz. [öğretici: WINDOWS ml ile özel görüntü işleme BIR onnx modeli kullanma (Önizleme)](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/custom-vision-onnx-windows-ml).

- [Mevcut modelleri WinMLTools kullanarak Dönüştür](https://docs.microsoft.com/windows/ai/windows-ml/convert-model-winmltools): Bu Python paketi, modellerin çeşitli eğitim çerçevesi biçimlerinden onnx 'e dönüştürülmesini sağlar. Uygulamanızın hangi Windows yapılarına hedeflediğini bağlı olarak, modelinizi hangi ONNX sürümüne dönüştürmek istediğinizi belirtebilirsiniz. Python ile ilgili bilgi sahibi değilseniz, modellerinizi dönüştürmek için [Windows Machine Learning kullanıcı arabirimi tabanlı Pano 'yu](https://github.com/Microsoft/Windows-Machine-Learning/tree/master/Tools/WinMLDashboard) kullanabilirsiniz.

> [!IMPORTANT]
> Tüm ONNX sürümleri Azure SQL veritabanı Edge tarafından desteklenmez. Yalnızca ONNX modeliyle sayısal veri türleri tahmin edilebilir.

Bir ONNX modeline sahip olduktan sonra modeli Azure SQL veritabanı Edge 'de dağıtabilirsiniz. Ardından, [ön tahmin T-SQL işleviyle yerel Puanlama](/sql/advanced-analytics/sql-native-scoring/)kullanabilirsiniz.

## <a name="limitations"></a>Sınırlamalar

Şu anda bu destek, **sayısal veri türlerine**sahip modellerle sınırlıdır:

- [int ve bigint](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql5)
- [gerçek ve float](https://docs.microsoft.com/sql/t-sql/data-types/float-and-real-transact-sql).
  
Diğer sayısal türler, dönüştürme ve dönüştürme [dönüştürme ve dönüştürme](https://docs.microsoft.com/sql/t-sql/functions/cast-and-convert-transact-sql)kullanılarak desteklenen türlere dönüştürülebilir.

Model girişleri, modeldeki her girdinin tablodaki tek bir sütuna karşılık gelmesi için yapılandırılmalıdır. Örneğin, bir modeli eğitebilmeniz için bir Pandas dataframe kullanıyorsanız, her giriş modele ayrı bir sütun olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal aracılığıyla SQL veritabanı ucunu dağıtma](deploy-portal.md)
- [Azure SQL veritabanı Edge önizlemesine bir ONNX modeli dağıtma](deploy-onnx.md)