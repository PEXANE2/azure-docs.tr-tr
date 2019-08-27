---
title: Dil Algılama kapsayıcısı için Docker Pull
titleSuffix: Azure Cognitive Services
description: Dil Algılama kapsayıcısı için Docker Pull komutu
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/20/2019
ms.author: dapine
ms.openlocfilehash: da6f04f1042d1a02178f345c5fe67387ae0a7d0f
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051359"
---
## <a name="pull-the-language-detection-container"></a>Dil Algılama kapsayıcısını çekme

Metin Analizi için kapsayıcı görüntüleri Microsoft Container Registry kullanılabilir.

| Kapsayıcı | Container Registry/depo/görüntü adı |
|-----------|------------|
| Dil Algılama | `mcr.microsoft.com/azure-cognitive-services/language` |

Microsoft Container Registry bir kapsayıcı görüntüsünü indirmek için [komutunukullanın.`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)

Metin Analizi kapsayıcıları için kullanılabilir etiketlerin tam açıklaması için, Docker Hub 'ındaki [dil algılama](https://go.microsoft.com/fwlink/?linkid=2018759) kapsayıcısına bakın.


### <a name="docker-pull-for-the-language-detection-container"></a>Dil Algılama kapsayıcısı için Docker Pull

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```
