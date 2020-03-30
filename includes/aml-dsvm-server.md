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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79485943"
---
1. [Azure Machine Learning çalışma alanı oluşturun.](../articles/machine-learning/how-to-manage-workspace.md)

1. [GitHub deposunu](https://aka.ms/aml-notebooks)klonla.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Bu yöntemlerden birini kullanarak klonlanmış dizine bir çalışma alanı yapılandırma dosyası ekleyin:

    * Azure [portalında,](https://ms.portal.azure.com)çalışma alanınızın **Genel Bakış** bölümünden **config.json indir'i** seçin. 

    ![Config.json karşıdan yükleme](./media/aml-dsvm-server/download-config.png)

    * Klonlanmış dizininizdeki [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) not defterinde kodu kullanarak yeni bir çalışma alanı oluşturun.

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```bash
    jupyter notebook
    ```