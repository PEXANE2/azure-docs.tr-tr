---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: e3732823be1c8391f2bec9018a094200c7c93a5e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91711637"
---
| Özellik | Açıklama |
|:--- |:---|
|**kimlik/tür** | İsteği yapmak için kullanılan kimlik doğrulaması türü. Örneğin: `OAuth` ,, `SAS Key` `Account Key` , veya `Anonymous` |
|**Identity/tokenHash**|Bu alan yalnızca iç kullanım için ayrılmıştır. |
|**Yetkilendirme/eylem** | İsteğe atanan eylem. |
|**Authorization/Roleatamakimliği** | Rol atama KIMLIĞI. Örneğin: `4e2521b7-13be-4363-aeda-111111111111`.|
|**Yetkilendirme/Roledefinitionıd** | Rol tanımı KIMLIĞI. Örneğin: `ba92f5b4-2d11-453d-a403-111111111111"`.|
|**sorumlular/ID** | Güvenlik sorumlusunun KIMLIĞI. Örneğin: `a4711f3a-254f-4cfb-8a2d-111111111111`.|
|**sorumlular/tür** | Güvenlik sorumlusu türü. Örneğin: `ServicePrincipal`. |
|**istek sahibi/AppID** | İstek sahibi olarak kullanılan Open Authorization (OAuth) uygulama KIMLIĞI. <br> Örneğin: `d3f7d5fe-e64a-4e4e-871d-333333333333`.|
|**istek sahibi/izleyici** | İsteğin OAuth hedef kitlesi. Örneğin: `https://storage.azure.com`. |
|**istek sahibi/ObjectID** | İstek sahibinin OAuth nesne KIMLIĞI. Kerberos kimlik doğrulaması durumunda Kerberos kimliği doğrulanmış kullanıcının nesne tanımlayıcısını temsil eder. Örneğin: `0e0bf547-55e5-465c-91b7-2873712b249c`. |
|**istek sahibi/Tenantıd** | Kimliğin OAuth kiracı KIMLIĞI. Örneğin: `72f988bf-86f1-41af-91ab-222222222222`.|
|**istek sahibi/Tokenıssuer** | OAuth belirteci veren. Örneğin: `https://sts.windows.net/72f988bf-86f1-41af-91ab-222222222222/`.|
|**istek sahibi/UPN** | İstek sahibinin kullanıcı asıl adı (UPN). Örneğin: `someone@contoso.com`. |
|**istek sahibi/Kullanıcı adı** | Bu alan yalnızca iç kullanım için ayrılmıştır.|