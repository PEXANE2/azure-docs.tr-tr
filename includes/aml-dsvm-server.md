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
ms.openlocfilehash: 2504245a14eee5308e046148f8073cf34ffa8143
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "79485943"
---
1. [Azure Machine Learning çalışma alanı oluşturun](../articles/machine-learning/how-to-manage-workspace.md).

1. [GitHub deposunu](https://aka.ms/aml-notebooks)kopyalayın.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Aşağıdaki yöntemlerden birini kullanarak kopyalanmış dizine bir çalışma alanı yapılandırma dosyası ekleyin:

    * [Azure Portal](https://ms.portal.azure.com), çalışma alanınızın **genel bakış** bölümünden **config.jsindir** ' i seçin. 

    ![config.jsindir](./media/aml-dsvm-server/download-config.png)

    * Klonlanan dizindeki [Configuration. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) not defterindeki kodu kullanarak yeni bir çalışma alanı oluşturun.

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```bash
    jupyter notebook
    ```