---
title: Docker Run komutunun kapsayıcı örneğini Çalıştır
titleSuffix: Azure Cognitive Services
description: Dil Algılama kapsayıcısı için Docker Run komutu
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/29/2020
ms.author: aahi
ms.openlocfilehash: f1f36cc730ceca9a961769775d17de6be8d26948
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90905995"
---
*Dil algılama* kapsayıcısını çalıştırmak için aşağıdaki `docker run` komutu yürütün. Aşağıdaki yer tutucuları kendi değerlerinizle değiştirin:

| Yer tutucu | Değer | Biçim veya örnek |
|-------------|-------|---|
| **{API_KEY}** | Metin Analizi kaynağınız için anahtar. Bunu, kaynağın **anahtar ve uç nokta** sayfasında, Azure Portal bulabilirsiniz. |`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`|
| **{ENDPOINT_URI}** | Metin Analizi API'si erişmek için uç nokta. Bunu, kaynağın **anahtar ve uç nokta** sayfasında, Azure Portal bulabilirsiniz. | `https://<your-custom-subdomain>.cognitiveservices.azure.com` |


```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/textanalytics/language \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden bir *dil algılama* kapsayıcısı çalıştırır
* Bir CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* TCP bağlantı noktası 5000 ' i kullanıma sunar ve kapsayıcı için bir sözde TTY ayırır
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.