---
title: Tek sayfalı uygulama (Web API 'SI çağırma)-Microsoft Identity platform
description: Tek sayfalı uygulama oluşturmayı öğrenin (Web API 'SI çağırma)
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4170a6642d35802581b5d1ff28eb802a6eb3482b
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74766166"
---
# <a name="single-page-application-call-a-web-api"></a>Tek sayfalı uygulama: Web API 'SI çağırma

Web API 'sini çağırmak için bir erişim belirteci almak veya yenilemek üzere `acquireTokenSilent` yöntemini çağırmanız önerilir. Belirteciniz olduktan sonra korumalı bir Web API 'SI çağırabilirsiniz.

## <a name="call-a-web-api"></a>Web API çağrısı

### <a name="javascript"></a>JavaScript

Microsoft Graph API gibi herhangi bir Web API 'sini çağırmak için bir HTTP isteğindeki alınan erişim belirtecini bir taşıyıcı olarak kullanın. Örnek:

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

### <a name="angular"></a>Angular

MSAL angular sarmalayıcısı, erişim belirteçlerini sessizce otomatik olarak almak ve bunları API 'lere HTTP isteklerine eklemek için HTTP yakalayıcısından yararlanır. Daha fazla bilgi için bkz. [API 'yi çağırmak için belirteç alma](scenario-spa-acquire-token.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşı](scenario-spa-production.md)
