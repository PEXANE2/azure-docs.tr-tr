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
ms.openlocfilehash: 09a3cc5a623be2ee5a9d50204f0902ca9f400a76
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68857514"
---
1. [Azure Machine Learning hizmet çalışma alanı oluşturun](../articles/machine-learning/service/how-to-manage-workspace.md).

1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Aşağıdaki yöntemlerden birini kullanarak kopyalanmış dizine bir çalışma alanı yapılandırma dosyası ekleyin:

    * [Azure Portal](https://ms.portal.azure.com), çalışma alanınızın **genel bakış** bölümünden **config. json dosyasını indir** ' i seçin. 

    ![Config.json dosyasını indir](./media/aml-dsvm-server/download-config.png)

    * Klonlanan dizindeki [Configuration. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) not defterindeki kodu kullanarak yeni bir çalışma alanı oluşturun.

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```shell
    jupyter notebook
    ```