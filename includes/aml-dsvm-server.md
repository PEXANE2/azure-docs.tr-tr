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
ms.openlocfilehash: e289cf7aea6e0ea46ff049f3ea8bf9e1517e8aaf
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78673639"
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