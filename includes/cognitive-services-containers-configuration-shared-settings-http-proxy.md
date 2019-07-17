---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 664cea26f910fa5b3354e2879a33de50eb13a7f3
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68286149"
---
Giden istekler oluşturmak için bir HTTP proxy 'si yapılandırmanız gerekiyorsa şu iki bağımsız değişkeni kullanın:

| Ad | Veri türü | Açıklama |
|--|--|--|
|HTTP_PROXY|dize|Kullanılacak proxy, örneğin,`http://proxy:8888`<br>`<proxy-url>`|
|HTTP_PROXY_CREDS|dize|Proxy için kimlik doğrulaması yapmak için gereken tüm kimlik bilgileri, örneğin, Kullanıcı adı: parola.|
|`<proxy-user>`|dize|Proxy için Kullanıcı.|
|`proxy-password`|dize|Ara sunucu için ile `<proxy-user>` ilişkili parola.|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=<proxy-url> \
HTTP_PROXY_CREDS=<proxy-user>:<proxy-password> \
```
