---
title: Dil Algılama konteyneri için Docker çekme
titleSuffix: Azure Cognitive Services
description: Dil Algılama konteyneri için Docker çekme komutu
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 3444d6267c09056697abf8339cc688a606b6dc17
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877100"
---
#### <a name="docker-pull-for-the-language-detection-container"></a>Dil Algılama konteyneri için Docker çekme

Microsoft [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) Kapsayıcı Kayıt Defteri'nden bir kapsayıcı resmi indirmek için komutu kullanın.

Text Analytics kapsayıcıları için kullanılabilir etiketlerin tam açıklaması için Docker Hub'daki [Dil Algılama](https://go.microsoft.com/fwlink/?linkid=2018759) konteynerine bakın.

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```
