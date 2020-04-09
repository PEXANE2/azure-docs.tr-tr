---
title: Docker run komutunun kapsayıcı örneğini çalıştırma
titleSuffix: Azure Cognitive Services
description: Sentiment Analysis konteyneri için Docker run komutu
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: e67f65d252be0ea638d3b5fa241d9413e76f1a98
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877059"
---
*Duyarlılık Analizi* kapsayıcısını çalıştırmak için `docker run` aşağıdaki komutu uygulayın.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Konteyner görüntüsünden bir *Sentiment Analysis* kapsayıcısı çalıştırın
* Bir CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* TCP bağlantı noktası 5000'i açığa çıkarır ve kapsayıcı için sözde TTY ayırır
* Çıktıktan sonra kapsayıcıotomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.