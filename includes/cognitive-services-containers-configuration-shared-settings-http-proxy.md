---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 84cd8ed79281b005407b5a857398b5669635c072
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68320525"
---
Giden istekler oluşturmak için bir HTTP proxy 'si yapılandırmanız gerekiyorsa şu iki bağımsız değişkeni kullanın:

| Adı | Veri türü | Açıklama |
|--|--|--|
|HTTP_PROXY|string|Kullanılacak proxy, örneğin,`http://proxy:8888`<br>`<proxy-url>`|
|HTTP_PROXY_CREDS|string|Proxy için kimlik doğrulaması yapmak için gereken tüm kimlik bilgileri, örneğin, Kullanıcı adı: parola.|
|`<proxy-user>`|string|Proxy için Kullanıcı.|
|`<proxy-password>`|string|Ara sunucu için ile `<proxy-user>` ilişkili parola.|
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
