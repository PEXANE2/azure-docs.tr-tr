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
ms.date: 08/19/2020
ms.openlocfilehash: bbd32f7e40e3039a69a95b4458de86ff35f26867
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91643947"
---
**İşlem hedefleri bir eğitim işinden sonrakine**yeniden kullanılabilir. Örneğin, çalışma alanınıza uzak bir sanal makine iliştirdikten sonra, birden fazla iş için onu yeniden kullanabilirsiniz.  Makine öğrenimi ardışık düzenleri için, her işlem hedefi için uygun işlem [hattı adımını](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py) kullanın.

|Eğitim &nbsp; hedefleri|[Otomatikleştirilmiş ML](../articles/machine-learning/concept-automated-ml.md) | [ML işlem hatları](../articles/machine-learning/concept-ml-pipelines.md) | [Azure Machine Learning tasarımcısı](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Yerel bilgisayar](../articles/machine-learning/how-to-create-attach-compute-sdk.md#local)| evet | &nbsp; | &nbsp; |
|[Azure Machine Learning işlem kümesi](../articles/machine-learning/how-to-create-attach-compute-sdk.md#amlcompute)| Evet & <br/>Hiper parametre &nbsp; ayarlama | evet | evet |
|[Azure Machine Learning işlem örneği](../articles/machine-learning/how-to-create-attach-compute-sdk.md#instance) | Evet (SDK aracılığıyla) | evet |  |
|[Uzak VM](../articles/machine-learning/how-to-create-attach-compute-sdk.md#vm) | Evet & <br/>Hiper parametre ayarlama | evet | &nbsp; |
|[Azure &nbsp; databricks](../articles/machine-learning/how-to-create-attach-compute-sdk.md#databricks)| Evet (yalnızca SDK yerel modu) | evet | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/how-to-create-attach-compute-sdk.md#adla) | &nbsp; | evet | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/how-to-create-attach-compute-sdk.md#hdinsight) | &nbsp; | evet | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-create-attach-compute-sdk.md#azbatch) | &nbsp; | evet | &nbsp; |
