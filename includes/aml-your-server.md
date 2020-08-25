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
ms.date: 03/05/2020
ms.openlocfilehash: ff449626ce528cfe0218a95330a567303c547e5f
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "79486076"
---
1. Python için Azure Machine Learning SDK 'sını yüklemek için [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)  'daki yönergeleri kullanın

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