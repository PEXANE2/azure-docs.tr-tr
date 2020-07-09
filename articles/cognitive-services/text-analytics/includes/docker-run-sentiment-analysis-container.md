---
title: Docker Run komutunun kapsayıcı örneğini Çalıştır
titleSuffix: Azure Cognitive Services
description: Yaklaşım Analizi kapsayıcısı için Docker Run komutu
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/29/2020
ms.author: aahi
ms.openlocfilehash: 459ab7a254994929174e4f81ad25f0729f43fa0b
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108979"
---
*Yaklaşım Analizi v3* kapsayıcısını çalıştırmak için aşağıdaki `docker run` komutu yürütün. Aşağıdaki yer tutucuları kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
| **{API_KEY}** | Metin Analizi kaynağınız için anahtar. Bunu, kaynağın **anahtar ve uç nokta** sayfasında, Azure Portal bulabilirsiniz. |`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`|
| **{ENDPOINT_URI}** | Metin Analizi API'si erişmek için uç nokta. Bunu, kaynağın **anahtar ve uç nokta** sayfasında, Azure Portal bulabilirsiniz. | `https://<your-custom-subdomain>.cognitiveservices.azure.com` |

```bash
docker run --rm -it -p 5000:5000 --memory 8g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden bir *yaklaşım Analizi* kapsayıcısı çalıştırır
* Bir CPU çekirdeği ve 8 gigabayt (GB) bellek ayırır
* TCP bağlantı noktası 5000 ' i kullanıma sunar ve kapsayıcı için bir sözde TTY ayırır
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.