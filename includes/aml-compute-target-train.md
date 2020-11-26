---
title: include dosyası
description: include dosyası
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 09/17/2020
ms.openlocfilehash: 87b1f4ab7b7091970d7bb76ae1e00b06549fb0b4
ms.sourcegitcommit: 2e9643d74eb9e1357bc7c6b2bca14dbdd9faa436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96152610"
---
**İşlem hedefleri bir eğitim işinden sonrakine yeniden kullanılabilir.** Örneğin, çalışma alanınıza uzak bir VM iliştirdikten sonra, birden fazla iş için onu yeniden kullanabilirsiniz. Makine öğrenimi ardışık düzenleri için, her işlem hedefi için uygun işlem [hattı adımını](/python/api/azureml-pipeline-steps/azureml.pipeline.steps?preserve-view=true&view=azure-ml-py) kullanın.

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
