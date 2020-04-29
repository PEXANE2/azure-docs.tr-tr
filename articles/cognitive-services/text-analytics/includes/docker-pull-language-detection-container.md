---
title: Dil Algılama kapsayıcısı için Docker Pull
titleSuffix: Azure Cognitive Services
description: Dil Algılama kapsayıcısı için Docker Pull komutu
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 3444d6267c09056697abf8339cc688a606b6dc17
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80877100"
---
#### <a name="docker-pull-for-the-language-detection-container"></a>Dil Algılama kapsayıcısı için Docker Pull

Microsoft Container Registry [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) bir kapsayıcı görüntüsünü indirmek için komutunu kullanın.

Metin Analizi kapsayıcıları için kullanılabilir etiketlerin tam açıklaması için, Docker Hub 'ındaki [dil algılama](https://go.microsoft.com/fwlink/?linkid=2018759) kapsayıcısına bakın.

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```
