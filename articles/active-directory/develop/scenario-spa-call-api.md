---
title: Web API çağıran tek sayfalık uygulama oluşturma - Microsoft kimlik platformu | Azure
description: Web API çağıran tek sayfalık bir uygulama oluşturmayı öğrenin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77160007"
---
# <a name="single-page-application-call-a-web-api"></a>Tek sayfalı uygulama: Web API'sını arayın

Web API'yi `acquireTokenSilent` aramadan önce bir erişim jetonunu elde etmek veya yenilemek için yöntemi aramanızı öneririz. Bir belirteç aldıktan sonra, korumalı web API'sini arayabilirsiniz.

## <a name="call-a-web-api"></a>Web API çağrısı

# <a name="javascript"></a>[Javascript](#tab/javascript)

Microsoft Graph API gibi web API'sini aramak için bir HTTP isteğinde edinilmiş erişim belirtecisini taşıyıcı olarak kullanın. Örnek:

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

MSAL Açısal sarıcı, erişim belirteçlerini otomatik olarak sessizce elde etmek ve API'lere HTTP isteklerine eklemek için HTTP önleme örüntüsünden yararlanır. Daha fazla bilgi için [bkz.](scenario-spa-acquire-token.md)

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşıma](scenario-spa-production.md)
