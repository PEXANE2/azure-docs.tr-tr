---
title: Anahtar İfade Ayıklama kapsayıcısı için Docker Pull
titleSuffix: Azure Cognitive Services
description: Anahtar İfade Ayıklama kapsayıcısı için Docker Pull komutu
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/20/2019
ms.author: dapine
ms.openlocfilehash: e71db5455a3d6726cd6567d568a13fcdcf9cb520
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051395"
---
## <a name="pull-the-key-phrase-extraction-container"></a>Anahtar İfade Ayıklama kapsayıcısını çekme

Metin Analizi için kapsayıcı görüntüleri Microsoft Container Registry kullanılabilir.

| Kapsayıcı | Container Registry/depo/görüntü adı |
|-----------|------------|
| Anahtar İfade Ayıklama | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |

Microsoft Container Registry bir kapsayıcı görüntüsünü indirmek için [komutunukullanın.`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)

Metin Analizi kapsayıcıları için kullanılabilir etiketlerin tam açıklaması için, Docker Hub 'ındaki [anahtar ifade ayıklama](https://go.microsoft.com/fwlink/?linkid=2018757) kapsayıcısına bakın.

### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>Anahtar İfade Ayıklama kapsayıcısı için Docker Pull

```
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```
