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
ms.openlocfilehash: 6e7580abfd113a279e2223b9cc9665f6a0b86bc8
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95558384"
---
1. Python için Azure Machine Learning SDK 'sını yüklemek için [Azure MACHINE LEARNING SDK](/python/api/overview/azure/ml/install?view=azure-ml-py)  'daki yönergeleri kullanın

1. [Azure Machine Learning çalışma alanı](../articles/machine-learning/how-to-manage-workspace.md)oluşturun.

1. Bir  [yapılandırma dosyası](../articles/machine-learning/how-to-configure-environment.md#workspace) dosyası (**aml_config/config.js**) yazın.

1. [GitHub deposunu](https://aka.ms/aml-notebooks)kopyalayın.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```bash
    jupyter notebook
    ```