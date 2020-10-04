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
ms.openlocfilehash: 5d312e5ccc0eeed3e17ffa855c496779c5769c5c
ms.sourcegitcommit: 19dce034650c654b656f44aab44de0c7a8bd7efe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2020
ms.locfileid: "91707753"
---
**İşlem hedefleri bir eğitim işinden sonrakine**yeniden kullanılabilir. Örneğin, çalışma alanınıza uzak bir sanal makine iliştirdikten sonra, birden fazla iş için onu yeniden kullanabilirsiniz.  Makine öğrenimi ardışık düzenleri için, her işlem hedefi için uygun işlem [hattı adımını](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py&preserve-view=true) kullanın.

Çoğu iş için eğitim işlem hedefi için aşağıdaki kaynaklardan herhangi birini kullanabilirsiniz.  Ancak, otomatik makine öğrenimi, makine öğrenimi ardışık düzenleri veya tasarımcı için tüm kaynaklar kullanılamaz:

|Eğitim &nbsp; hedefleri|[Otomatikleştirilmiş ML](../articles/machine-learning/concept-automated-ml.md) | [ML işlem hatları](../articles/machine-learning/concept-ml-pipelines.md) | [Azure Machine Learning tasarımcısı](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Yerel bilgisayar](../articles/machine-learning/how-to-attach-compute-targets.md#local)| evet | &nbsp; | &nbsp; |
|[Azure Machine Learning işlem kümesi](../articles/machine-learning/how-to-create-attach-compute-cluster.md)| evet | evet | evet |
|[Azure Machine Learning işlem örneği](../articles/machine-learning/how-to-create-manage-compute-instance.md) | Evet (SDK aracılığıyla)  | evet |  |
|[Uzak VM](../articles/machine-learning/how-to-attach-compute-targets.md#vm) | evet  | evet | &nbsp; |
|[Azure &nbsp; databricks](../articles/machine-learning/how-to-attach-compute-targets.md#databricks)| Evet (yalnızca SDK yerel modu) | evet | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/how-to-attach-compute-targets.md#adla) | &nbsp; | evet | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/how-to-attach-compute-targets.md#hdinsight) | &nbsp; | evet | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-attach-compute-targets.md#azbatch) | &nbsp; | evet | &nbsp; |
