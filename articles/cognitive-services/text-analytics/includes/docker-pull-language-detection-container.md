---
title: Dil Algılama konteyneri için Docker çekme
titleSuffix: Azure Cognitive Services
description: Dil Algılama konteyneri için Docker çekme komutu
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/12/2019
ms.author: dapine
ms.openlocfilehash: 23669a5440799ff071b3a30e3e41ab5d49f212df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70966735"
---
#### <a name="docker-pull-for-the-language-detection-container"></a>Dil Algılama konteyneri için Docker çekme

Microsoft [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) Kapsayıcı Kayıt Defteri'nden bir kapsayıcı resmi indirmek için komutu kullanın.

Text Analytics kapsayıcıları için kullanılabilir etiketlerin tam açıklaması için Docker Hub'daki [Dil Algılama](https://go.microsoft.com/fwlink/?linkid=2018759) konteynerine bakın.

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```
