---
title: Yaklaşım Analizi kapsayıcısı için Docker Pull
titleSuffix: Azure Cognitive Services
description: Yaklaşım Analizi kapsayıcısı için Docker Pull komutu
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/20/2019
ms.author: dapine
ms.openlocfilehash: 0954ef88f6917159156fbb73103b7b7af3283c00
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051319"
---
## <a name="pull-the-sentiment-analysis-container"></a>Yaklaşım Analizi kapsayıcısını çekme

Metin Analizi için kapsayıcı görüntüleri Microsoft Container Registry kullanılabilir.

| Kapsayıcı | Container Registry/depo/görüntü adı |
|-----------|------------|
| Duygu Analizi | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Microsoft Container Registry bir kapsayıcı görüntüsünü indirmek için [komutunukullanın.`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)

Metin Analizi kapsayıcıları için kullanılabilir etiketlerin tam açıklaması için, Docker Hub 'ındaki [yaklaşım Analizi](https://go.microsoft.com/fwlink/?linkid=2018654) kapsayıcısına bakın.

### <a name="docker-pull-for-the-sentiment-analysis-container"></a>Yaklaşım Analizi kapsayıcısı için Docker Pull

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```
