---
author: burkeholland
ms.service: static-web-apps
ms.topic: include
ms.date: 05/08/2020
ms.author: buhollan
ms.openlocfilehash: 879d0c2e8c4cd66ce04c0298d00f7d6a1987acf5
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "83597236"
---
#### <a name="local-proxy"></a>Yerel ara sunucu

Tüm isteklerini ' `/api` de çalışan API uç noktasına yönlendiren canlı sunucu Visual Studio Code uzantısı için bir ara sunucu yapılandırabilirsiniz `http://127.0.0.1:7071/api` .

1. Dosyasında _. vscode/settings.js_ açın.

1. Canlı sunucu için bir proxy belirtmek üzere aşağıdaki ayarları ekleyin.

   ```json
   "liveServer.settings.proxy": {
      "enable": true,
      "baseUri": "/api",
      "proxyUri": "http://127.0.0.1:7071/api"
   }
   ```

   Bu yapılandırma, Kullanıcı ayarları dosyasının aksine, proje ayarları dosyasına en iyi şekilde kaydedilir.

   Proje ayarlarının kullanılması, proxy 'nin Visual Studio Code ' de açılan diğer tüm projelere uygulanmadığını sağlar.
