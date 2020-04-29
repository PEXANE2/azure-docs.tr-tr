---
title: Anahtar İfade Ayıklama kapsayıcı Docker örnekleri
titleSuffix: Azure Cognitive Services
description: Anahtar İfade Ayıklama kapsayıcı Docker örnekleri
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 1c079df3171d89da4ef40e5008aaeb08b6504e66
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80878705"
---
### <a name="key-phrase-extraction-container-docker-examples"></a>Anahtar İfade Ayıklama kapsayıcı Docker örnekleri

Aşağıdaki Docker örnekleri Anahtar İfade Ayıklama kapsayıcısı içindir.

#### <a name="basic-example"></a>Temel örnek 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/keyphrase \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} 
  ```

#### <a name="logging-example"></a>Günlüğe kaydetme örneği 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/keyphrase \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
  ```
