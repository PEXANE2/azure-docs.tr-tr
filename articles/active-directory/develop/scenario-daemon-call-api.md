---
title: Daemon uygulamasından web API'sını arayın - Microsoft kimlik platformu | Azure
description: Web API'lerini çağıran bir daemon uygulaması oluşturmayı öğrenin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76775187"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>Web API'ları çağıran Daemon uygulaması - uygulamadan bir web API'sini arayın

.NET daemon uygulamaları web API'sını arayabilir. .NET daemon uygulamaları da birkaç önceden onaylanmış web API'si arayabilirsiniz.

## <a name="calling-a-web-api-from-a-daemon-application"></a>Daemon uygulamasından web API'si çağırma

API çağırmak için belirteci şu şekilde kullanabilirsiniz:

# <a name="net"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# <a name="python"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

# <a name="java"></a>[Java](#tab/java)

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

## <a name="calling-several-apis"></a>Birkaç API'yi arama

Daemon uygulamaları için, aradığınız web API'leri önceden onaylanması gerekir. Daemon uygulamalarında artımlı onay yoktur. (Kullanıcı etkileşimi yoktur.) Kiracı yöneticinin uygulama ve tüm API izinleri için önceden onay vermesi gerekir. Birkaç API çağırmak istiyorsanız, her kaynak için bir belirteç edinmeniz gerekir, her arama. `AcquireTokenForClient` MSAL, gereksiz servis çağrılarını önlemek için uygulama belirteci önbelleğini kullanır.

## <a name="next-steps"></a>Sonraki adımlar

# <a name="net"></a>[.NET](#tab/dotnet)

> [!div class="nextstepaction"]
> [Daemon uygulaması - üretime geçin](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-production?tabs=dotnet)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Daemon uygulaması - üretime geçin](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-production?tabs=python)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Daemon uygulaması - üretime geçin](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-production?tabs=java)

---
