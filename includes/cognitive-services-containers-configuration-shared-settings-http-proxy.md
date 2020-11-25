---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: ce4cc68826b39b5707549afc799d2d214e8876c6
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96001219"
---
Giden istekler oluşturmak için bir HTTP proxy 'si yapılandırmanız gerekiyorsa şu iki bağımsız değişkeni kullanın:

| Name | Veri türü | Açıklama |
|--|--|--|
|HTTPS_PROXY|string|Kullanılacak proxy, örneğin, `https://proxy:8888`<br>`<proxy-url>`|
|HTTPS_PROXY_CREDS|string|Proxy için kimlik doğrulaması yapmak için gereken tüm kimlik bilgileri, örneğin, Kullanıcı adı: parola.|
|`<proxy-user>`|string|Proxy için Kullanıcı.|
|`<proxy-password>`|string|`<proxy-user>`Ara sunucu için ile ilişkili parola.|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
HTTPS_PROXY=<proxy-url> \
HTTPS_PROXY_CREDS=<proxy-user>:<proxy-password> \
```
