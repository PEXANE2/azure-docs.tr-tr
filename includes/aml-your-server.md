---
title: içerme dosyası
description: içerme dosyası
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 09/26/2019
ms.openlocfilehash: 85d1c1cd294bfc02a2e0e327073bb6a80366548b
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71841112"
---
1. Python için Azure Machine Learning SDK 'sını yüklemek için [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) 'daki yönergeleri kullanın

1. [Azure Machine Learning hizmet çalışma alanı](../articles/machine-learning/service/how-to-manage-workspace.md)oluşturun.

1. Bir [yapılandırma dosyası](../articles/machine-learning/service/how-to-configure-environment.md#workspace) dosyası (**aml_config/config. JSON**) yazın.

1. [GitHub deposunu](https://aka.ms/aml-notebooks)kopyalayın.

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Kopyalanmış dizininizden Not defteri sunucusunu başlatın.

    ```shell
    jupyter notebook
    ```
