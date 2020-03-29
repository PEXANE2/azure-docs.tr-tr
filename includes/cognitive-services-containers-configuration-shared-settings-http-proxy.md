---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 84cd8ed79281b005407b5a857398b5669635c072
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68320525"
---
Giden isteklerde bulunmak için bir HTTP proxy yapılandırmanız gerekiyorsa, şu iki bağımsız değişkeni kullanın:

| Adı | Veri türü | Açıklama |
|--|--|--|
|HTTP_PROXY|string|Kullanılacak proxy, örneğin,`http://proxy:8888`<br>`<proxy-url>`|
|HTTP_PROXY_CREDS|string|Proxy'ye karşı kimlik doğrulaması için gereken kimlik bilgileri, örneğin, kullanıcı adı:parola.|
|`<proxy-user>`|string|Proxy için kullanıcı.|
|`<proxy-password>`|string|Proxy `<proxy-user>` için ilişkili parola.|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=<proxy-url> \
HTTP_PROXY_CREDS=<proxy-user>:<proxy-password> \
```
