---
title: Duyarlılık Analizi konteyner için Docker çekin
titleSuffix: Azure Cognitive Services
description: Sentiment Analysis konteyneri için Docker çekme komutu
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: e502b886c8031174180d40f1c5b7e373a974ee8e
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877060"
---
#### <a name="docker-pull-for-the-sentiment-analysis-container"></a>Duyarlılık Analizi konteyner için Docker çekin

Microsoft [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) Kapsayıcı Kayıt Defteri'nden bir kapsayıcı resmi indirmek için komutu kullanın.

Text Analytics kapsayıcıları için kullanılabilir etiketlerin tam açıklaması için Docker Hub'daki [Sentiment Analysis](https://go.microsoft.com/fwlink/?linkid=2018654) konteynerine bakın.

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```
