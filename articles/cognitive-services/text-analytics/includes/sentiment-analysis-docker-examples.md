---
title: Yaklaşım Analizi kapsayıcı Docker örnekleri
titleSuffix: Azure Cognitive Services
description: Yaklaşım Analizi kapsayıcı Docker örnekleri
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/21/2019
ms.author: dapine
ms.openlocfilehash: 369b0e30791b38dfa2048ac9f840123daa04ecba
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051115"
---
## <a name="sentiment-analysis-container-docker-examples"></a>Yaklaşım Analizi kapsayıcı Docker örnekleri

Aşağıdaki Docker örnekleri Yaklaşım Analizi kapsayıcısı içindir.

### <a name="basic-example"></a>Temel örnek 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Günlüğe kaydetme örneği 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
  ```
