---
title: Web API 'sini çağıran tek sayfalı uygulama oluşturma-Microsoft Identity platform | Mavisi
description: Web API 'sini çağıran tek sayfalı bir uygulama oluşturmayı öğrenin
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/06/2019
ms.author: ryanwi
ms.custom: aaddev
ms.openlocfilehash: 3b71f66dfcbd33cdecc1a6fea46871ba0cda687d
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94442897"
---
# <a name="single-page-application-call-a-web-api"></a>Tek sayfalı uygulama: Web API 'SI çağırma

`acquireTokenSilent`Web API 'sini çağırmak için bir erişim belirteci almak veya yenilemek üzere yöntemini çağırmanız önerilir. Belirteciniz olduktan sonra korumalı bir Web API 'SI çağırabilirsiniz.

## <a name="call-a-web-api"></a>Web API çağrısı

# <a name="javascript"></a>[JavaScript](#tab/javascript)

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

# <a name="angular"></a>[Angular](#tab/angular)

MSAL angular sarmalayıcısı, erişim belirteçlerini sessizce otomatik olarak almak ve bunları API 'lere HTTP isteklerine eklemek için HTTP yakalayıcısından yararlanır. Daha fazla bilgi için bkz. [API 'yi çağırmak için belirteç alma](scenario-spa-acquire-token.md).

---

## <a name="next-steps"></a>Sonraki adımlar

Bu senaryodaki sonraki makaleye geçin, [üretime geçin](scenario-spa-production.md).