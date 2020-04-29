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
ms.openlocfilehash: 2504245a14eee5308e046148f8073cf34ffa8143
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79485943"
---
1. [Azure Machine Learning çalışma alanı oluşturun](../articles/machine-learning/how-to-manage-workspace.md).

1. [GitHub deposunu](https://aka.ms/aml-notebooks)kopyalayın.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Aşağıdaki yöntemlerden birini kullanarak kopyalanmış dizine bir çalışma alanı yapılandırma dosyası ekleyin:

    * [Azure Portal](https://ms.portal.azure.com), çalışma alanınızın **genel bakış** bölümünden **config. json dosyasını indir** ' i seçin. 

    ![Config. json dosyasını indir](./media/aml-dsvm-server/download-config.png)

    * Klonlanan dizindeki [Configuration. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) not defterindeki kodu kullanarak yeni bir çalışma alanı oluşturun.

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```bash
    jupyter notebook
    ```