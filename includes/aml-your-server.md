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
ms.date: 01/25/2019
ms.openlocfilehash: 6c93d1243db1b3c4277a54cf71e10f6bbc648d26
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68845998"
---
- Python için Azure Machine Learning SDK 'Sı yüklendi. Şunları yapmak için [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) 'daki yönergeleri kullanın:


1. Şunları yapmak için [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) 'daki yönergeleri kullanın:
    * Miniconda ortamı oluşturma [Azure Machine Learning hizmeti çalışma alanları oluşturma ve yönetme]
    * Python için Azure Machine Learning SDK 'sını yükler

1. [Azure Machine Learning hizmet çalışma alanı](../articles/machine-learning/service/how-to-manage-workspace.md)oluşturun.

1. Bir [yapılandırma dosyası](../articles/machine-learning/service/how-to-configure-environment.md#workspace) dosyası (**aml_config/config. JSON**) yazın.

1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```shell
    jupyter notebook
    ```