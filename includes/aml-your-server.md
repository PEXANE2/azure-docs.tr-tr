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
ms.openlocfilehash: 4bce52ba3320506b85949493407dded1d52415a3
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78673636"
---
1. Python için Azure Machine Learning SDK 'sını yüklemek için [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) 'daki yönergeleri kullanın

1. [Azure Machine Learning çalışma alanı](../articles/machine-learning/how-to-manage-workspace.md)oluşturun.

1. Bir [yapılandırma dosyası](../articles/machine-learning/how-to-configure-environment.md#workspace) dosyası (**aml_config/config.exe**) yazın.

1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```shell
    jupyter notebook
    ```
