---
title: Docker Run komutunun kapsayıcı örneğini Çalıştır
titleSuffix: Azure Cognitive Services
description: Yaklaşım Analizi kapsayıcısı için Docker Run komutu
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: e67f65d252be0ea638d3b5fa241d9413e76f1a98
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80877059"
---
*Yaklaşım Analizi* kapsayıcısını çalıştırmak için aşağıdaki `docker run` komutu yürütün.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden bir *yaklaşım Analizi* kapsayıcısı çalıştırır
* Bir CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* TCP bağlantı noktası 5000 ' i kullanıma sunar ve kapsayıcı için bir sözde TTY ayırır
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.