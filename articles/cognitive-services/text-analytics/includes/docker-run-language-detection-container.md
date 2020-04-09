---
title: Docker run komutunun kapsayıcı örneğini çalıştırma
titleSuffix: Azure Cognitive Services
description: Dil Algılama kapsayıcısı için Docker run komutu
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 1f6bfbf3a15f17c3cfd0487cdd0bbb535ffc31ae
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877041"
---
*Dil Algılama* kapsayıcısını çalıştırmak için `docker run` aşağıdaki komutu çalıştırın.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/language \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden bir *Dil Algılama* kapsayıcısı çalıştırın
* Bir CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* TCP bağlantı noktası 5000'i açığa çıkarır ve kapsayıcı için sözde TTY ayırır
* Çıktıktan sonra kapsayıcıotomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.