---
title: Duyarlılık Analizi konteyner için Docker çekin
titleSuffix: Azure Cognitive Services
description: Sentiment Analysis konteyneri için Docker çekme komutu
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/12/2019
ms.author: dapine
ms.openlocfilehash: df00d469052fa30c3f2aaa5afe1881ef74587f9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70966802"
---
#### <a name="docker-pull-for-the-sentiment-analysis-container"></a>Duyarlılık Analizi konteyner için Docker çekin

Microsoft [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) Kapsayıcı Kayıt Defteri'nden bir kapsayıcı resmi indirmek için komutu kullanın.

Text Analytics kapsayıcıları için kullanılabilir etiketlerin tam açıklaması için Docker Hub'daki [Sentiment Analysis](https://go.microsoft.com/fwlink/?linkid=2018654) konteynerine bakın.

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```
