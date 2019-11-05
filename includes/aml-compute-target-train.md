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
ms.date: 05/30/2019
ms.openlocfilehash: aa1120db5451583153928bd2cbc383a46781a267
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489560"
---
**İşlem hedefleri bir eğitim işinden sonrakine**yeniden kullanılabilir. Örneğin, çalışma alanınıza uzak bir sanal makine iliştirdikten sonra, birden fazla iş için onu yeniden kullanabilirsiniz.

|Eğitim &nbsp;hedefleri| GPU desteği |[Otomatik ML](../articles/machine-learning/service/concept-automated-ml.md) | [ML işlem hatları](../articles/machine-learning/service/concept-ml-pipelines.md) | [Azure Machine Learning Tasarımcısı](../articles/machine-learning/service/concept-designer.md)
|----|:----:|:----:|:----:|:----:|
|[Yerel bilgisayar](../articles/machine-learning/service/how-to-set-up-training-targets.md#local)| Belki | evet | &nbsp; | &nbsp; |
|[Azure Machine Learning işlem örneği](../articles/machine-learning/service/concept-compute-instance.md)| evet | | evet |  |
|[Azure Machine Learning işlem kümesi](../articles/machine-learning/service/how-to-set-up-training-targets.md#amlcompute)| evet | Evet & <br/>Hiper parametre&nbsp;ayarlama | evet | evet |
|[Uzak VM](../articles/machine-learning/service/how-to-set-up-training-targets.md#vm) |evet | Evet & <br/>Hiper parametre ayarlama | evet | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#databricks)| &nbsp; | evet | evet | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#adla)| &nbsp; | &nbsp; | evet | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/service/how-to-set-up-training-targets.md#hdinsight)| &nbsp; | &nbsp; | evet | &nbsp; |
|[Azure Batch](../articles/machine-learning/service/how-to-set-up-training-targets.md#azbatch)| &nbsp; | &nbsp; | evet | &nbsp; |
