---
title: dosya dahil etme
description: dosya dahil etme
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 09/17/2020
ms.openlocfilehash: cdf2c3d1840f64b267f1aac1f8877d5f0bf76082
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91841975"
---
**İşlem hedefleri bir eğitim işinden sonrakine yeniden kullanılabilir.** Örneğin, çalışma alanınıza uzak bir VM iliştirdikten sonra, birden fazla iş için onu yeniden kullanabilirsiniz. Makine öğrenimi ardışık düzenleri için, her işlem hedefi için uygun işlem [hattı adımını](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py&preserve-view=true) kullanın.

Çoğu iş için eğitim işlem hedefi için aşağıdaki kaynaklardan herhangi birini kullanabilirsiniz. Her kaynak otomatik makine öğrenimi, makine öğrenimi ardışık düzenleri veya tasarımcı için kullanılamaz.

|Eğitim &nbsp; hedefleri|[Otomatik makine öğrenimi](../articles/machine-learning/concept-automated-ml.md) | [Makine öğrenmesi işlem hatları](../articles/machine-learning/concept-ml-pipelines.md) | [Azure Machine Learning tasarımcısı](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Yerel bilgisayar](../articles/machine-learning/how-to-attach-compute-targets.md#local)| Evet | &nbsp; | &nbsp; |
|[Azure Machine Learning işlem kümesi](../articles/machine-learning/how-to-create-attach-compute-cluster.md)| Evet | Evet | Evet |
|[Azure Machine Learning işlem örneği](../articles/machine-learning/how-to-create-manage-compute-instance.md) | Evet (SDK aracılığıyla)  | Evet |  |
|[Uzak VM](../articles/machine-learning/how-to-attach-compute-targets.md#vm) | Evet  | Evet | &nbsp; |
|[Azure &nbsp; databricks](../articles/machine-learning/how-to-attach-compute-targets.md#databricks)| Evet (yalnızca SDK yerel modu) | Evet | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/how-to-attach-compute-targets.md#adla) | &nbsp; | Evet | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/how-to-attach-compute-targets.md#hdinsight) | &nbsp; | Evet | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-attach-compute-targets.md#azbatch) | &nbsp; | Evet | &nbsp; |
