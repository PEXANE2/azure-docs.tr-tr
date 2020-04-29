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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79486076"
---
1. Python için Azure Machine Learning SDK 'sını yüklemek için [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) 'daki yönergeleri kullanın

1. [Azure Machine Learning çalışma alanı](../articles/machine-learning/how-to-manage-workspace.md)oluşturun.

1. Bir [yapılandırma dosyası](../articles/machine-learning/how-to-configure-environment.md#workspace) dosyası (**aml_config/config.exe**) yazın.

1. [GitHub deposunu](https://aka.ms/aml-notebooks)kopyalayın.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```bash
    jupyter notebook
    ```