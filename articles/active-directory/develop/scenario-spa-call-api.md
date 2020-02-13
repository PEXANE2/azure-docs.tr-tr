---
title: Web API 'sini çağıran tek sayfalı uygulama oluşturma-Microsoft Identity platform | Mavisi
description: Web API 'sini çağıran tek sayfalı bir uygulama oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: ryanwi
ms.custom: aaddev
ms.openlocfilehash: ccece6c840033913ec6d96b446dcb98c4befb32f
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77160007"
---
# <a name="single-page-application-call-a-web-api"></a>Tek sayfalı uygulama: Web API 'SI çağırma

Web API 'sini çağırmak için bir erişim belirteci almak veya yenilemek üzere `acquireTokenSilent` yöntemini çağırmanız önerilir. Belirteciniz olduktan sonra korumalı bir Web API 'SI çağırabilirsiniz.

## <a name="call-a-web-api"></a>Web API çağrısı

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Microsoft Graph API gibi herhangi bir Web API 'sini çağırmak için bir HTTP isteğindeki alınan erişim belirtecini bir taşıyıcı olarak kullanın. Örneğin:

```javascript
    var headers = new Headers();
    var bearer = "Bearer " + access_token;
    headers.append("Authorization", bearer);
    var options = {
         method: "GET",
         headers: headers
    };
    var graphEndpoint = "https://graph.microsoft.com/v1.0/me";

    fetch(graphEndpoint, options)
        .then(function (response) {
             //do something with response
        }
```

# <a name="angulartabangular"></a>[Angular](#tab/angular)

MSAL angular sarmalayıcısı, erişim belirteçlerini sessizce otomatik olarak almak ve bunları API 'lere HTTP isteklerine eklemek için HTTP yakalayıcısından yararlanır. Daha fazla bilgi için bkz. [API 'yi çağırmak için belirteç alma](scenario-spa-acquire-token.md).

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşı](scenario-spa-production.md)
