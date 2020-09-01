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
ms.openlocfilehash: 54328c7ae4fe2112a718bd82b9f7330c9e95ec69
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89146751"
---
**İşlem hedefleri bir eğitim işinden sonrakine**yeniden kullanılabilir. Örneğin, çalışma alanınıza uzak bir sanal makine iliştirdikten sonra, birden fazla iş için onu yeniden kullanabilirsiniz.  Makine öğrenimi ardışık düzenleri için, her işlem hedefi için uygun işlem [hattı adımını](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py) kullanın.

|Eğitim &nbsp; hedefleri|[Otomatikleştirilmiş ML](../articles/machine-learning/concept-automated-ml.md) | [ML işlem hatları](../articles/machine-learning/concept-ml-pipelines.md) | [Azure Machine Learning Tasarımcısı](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Yerel bilgisayar](../articles/machine-learning/how-to-create-attach-compute-sdk.md#local)| evet | &nbsp; | &nbsp; |
|[Azure Machine Learning işlem kümesi](../articles/machine-learning/how-to-create-attach-compute-sdk.md#amlcompute)| Evet & <br/>Hiper parametre &nbsp; ayarlama | evet | evet |
|[Azure Machine Learning işlem örneği](../articles/machine-learning/how-to-create-attach-compute-sdk.md#instance) | Evet & <br/>Hiper parametre ayarlama | evet |  |
|[Uzak VM](../articles/machine-learning/how-to-create-attach-compute-sdk.md#vm) | Evet & <br/>Hiper parametre ayarlama | evet | &nbsp; |
|[Azure &nbsp; databricks](../articles/machine-learning/how-to-create-attach-compute-sdk.md#databricks)| Evet (yalnızca SDK yerel modu) | evet | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/how-to-create-attach-compute-sdk.md#adla) | &nbsp; | evet | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/how-to-create-attach-compute-sdk.md#hdinsight) | &nbsp; | evet | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-create-attach-compute-sdk.md#azbatch) | &nbsp; | evet | &nbsp; |
