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
ms.openlocfilehash: ac5656a8efe1dee932bfe753f8bdc06b787e20af
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75529805"
---
1. [Azure Machine Learning çalışma alanı oluşturun](../articles/machine-learning/how-to-manage-workspace.md).

1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Aşağıdaki yöntemlerden birini kullanarak kopyalanmış dizine bir çalışma alanı yapılandırma dosyası ekleyin:

    * [Azure Portal](https://ms.portal.azure.com), çalışma alanınızın **genel bakış** bölümünden **config. json dosyasını indir** ' i seçin. 

    ![Config. json dosyasını indir](./media/aml-dsvm-server/download-config.png)

    * Klonlanan dizindeki [Configuration. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) not defterindeki kodu kullanarak yeni bir çalışma alanı oluşturun.

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```shell
    jupyter notebook
    ```