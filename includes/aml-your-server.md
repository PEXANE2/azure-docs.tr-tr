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
ms.date: 03/05/2020
ms.openlocfilehash: ff449626ce528cfe0218a95330a567303c547e5f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79486076"
---
1. Python için [Azure Machine Learning SDK'yı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) yüklemek için Azure Machine Learning SDK'daki yönergeleri kullanın

1. Azure [Machine Learning çalışma alanı](../articles/machine-learning/how-to-manage-workspace.md)oluşturun.

1. Yapılandırma [dosyası](../articles/machine-learning/how-to-configure-environment.md#workspace) **(aml_config/config.json)** yazın.

1. [GitHub deposunu](https://aka.ms/aml-notebooks)klonla.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```bash
    jupyter notebook
    ```