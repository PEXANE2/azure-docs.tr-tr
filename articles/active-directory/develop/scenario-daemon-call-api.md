---
title: Bir Daemon uygulamasının Web API 'sini çağırma-Microsoft Identity platform | Mavisi
description: Web API 'Lerini çağıran bir Daemon uygulaması derlemeyi öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 338b638d6b33bcbbb5cf377643a96c71b0d314bd
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76775187"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>Web API 'Lerini çağıran Daemon uygulaması-uygulamadan bir Web API 'SI çağırma

.NET Daemon uygulamaları, bir Web API 'SI çağırabilir. .NET Daemon uygulamaları, önceden onaylanmış birkaç Web API 'Sini de çağırabilir.

## <a name="calling-a-web-api-from-a-daemon-application"></a>Bir Daemon uygulamasından Web API 'SI çağırma

Bir API 'yi çağırmak için belirtecin nasıl kullanılacağı aşağıda verilmiştir:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# <a name="pythontabpython"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

# <a name="javatabjava"></a>[Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

---

## <a name="calling-several-apis"></a>Çeşitli API 'Ler çağırma

Daemon uygulamaları için, çağırdığınız Web API 'Lerinin önceden onaylanmış olması gerekir. Daemon uygulamalarıyla artımlı izin yoktur. (Kullanıcı etkileşimi yoktur.) Kiracı yöneticisinin, uygulama ve tüm API izinleri için önceden onay sağlaması gerekir. Çeşitli API 'Ler çağırmak isterseniz, her bir kaynak için `AcquireTokenForClient`her seferinde bir belirteç edinmeniz gerekir. MSAL, gereksiz hizmet çağrılarını önlemek için uygulama belirteci önbelleğini kullanır.

## <a name="next-steps"></a>Sonraki adımlar

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

> [!div class="nextstepaction"]
> [Daemon uygulaması-üretime taşı](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-production?tabs=dotnet)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Daemon uygulaması-üretime taşı](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-production?tabs=python)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Daemon uygulaması-üretime taşı](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-production?tabs=java)

---
