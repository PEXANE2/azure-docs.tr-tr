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
ms.date: 12/04/2019
ms.openlocfilehash: 9a04bca11c6fca480ea1c9fd3d4a51956c1830de
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88933128"
---
**İşlem hedefleri bir eğitim işinden sonrakine**yeniden kullanılabilir. Örneğin, çalışma alanınıza uzak bir sanal makine iliştirdikten sonra, birden fazla iş için onu yeniden kullanabilirsiniz.  Makine öğrenimi ardışık düzenleri için, her işlem hedefi için uygun işlem [hattı adımını](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py) kullanın.

|Eğitim &nbsp; hedefleri|[Otomatikleştirilmiş ML](../articles/machine-learning/concept-automated-ml.md) | [ML işlem hatları](../articles/machine-learning/concept-ml-pipelines.md) | [Azure Machine Learning Tasarımcısı](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Yerel bilgisayar](../articles/machine-learning/how-to-set-up-training-targets.md#local)| evet | &nbsp; | &nbsp; |
|[Azure Machine Learning işlem kümesi](../articles/machine-learning/how-to-set-up-training-targets.md#amlcompute)| Evet & <br/>Hiper parametre &nbsp; ayarlama | evet | evet |
|[Azure Machine Learning işlem örneği](../articles/machine-learning/how-to-set-up-training-targets.md#instance) | Evet & <br/>Hiper parametre ayarlama | evet |  |
|[Uzak VM](../articles/machine-learning/how-to-set-up-training-targets.md#vm) | Evet & <br/>Hiper parametre ayarlama | evet | &nbsp; |
|[Azure &nbsp; databricks](../articles/machine-learning/how-to-set-up-training-targets.md#databricks)| Evet (yalnızca SDK yerel modu) | evet | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/how-to-set-up-training-targets.md#adla) | &nbsp; | evet | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/how-to-set-up-training-targets.md#hdinsight) | &nbsp; | evet | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-set-up-training-targets.md#azbatch) | &nbsp; | evet | &nbsp; |
