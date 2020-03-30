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
ms.date: 12/04/2019
ms.openlocfilehash: 6106d4e0801500b0429e634651f3de342646b754
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77156739"
---
**Hesaplama hedefleri bir eğitim işinden diğerine yeniden kullanılabilir.** Örneğin, çalışma alanınıza uzak bir VM iliştirdikten sonra, birden çok iş için yeniden kullanabilirsiniz.  Makine öğrenimi boru hatları için, her bilgi işlem hedefi için uygun [boru hattı adımını](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py) kullanın.

|Eğitim &nbsp;hedefleri|[Otomatikleştirilmiş ML](../articles/machine-learning/concept-automated-ml.md) | [ML işlem hatları](../articles/machine-learning/concept-ml-pipelines.md) | [Azure Machine Learning tasarımcısı](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Yerel bilgisayar](../articles/machine-learning/how-to-set-up-training-targets.md#local)| evet | &nbsp; | &nbsp; |
|[Azure Machine Learning bilgi işlem kümesi](../articles/machine-learning/how-to-set-up-training-targets.md#amlcompute)| Evet & <br/>hiperparametre&nbsp;atomu | evet | evet |
|[Uzaktan VM](../articles/machine-learning/how-to-set-up-training-targets.md#vm) | Evet & <br/>hiperparametre atomu | evet | &nbsp; |
|[Azure&nbsp;Veri Tuğlaları](../articles/machine-learning/how-to-create-your-first-pipeline.md#databricks)| Evet (Yalnızca SDK yerel modu) | evet | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/how-to-create-your-first-pipeline.md#adla) | &nbsp; | evet | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/how-to-set-up-training-targets.md#hdinsight) | &nbsp; | evet | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-set-up-training-targets.md#azbatch) | &nbsp; | evet | &nbsp; |
