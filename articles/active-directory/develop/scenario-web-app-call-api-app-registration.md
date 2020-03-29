---
title: Web API'lerini çağıran bir web uygulaması kaydedin - Microsoft kimlik platformu | Azure
description: Web API'lerini çağıran bir web uygulamasını nasıl kaydedebilirsiniz öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 5a57fcef3569734964bf6e8a41faa49800798f9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759066"
---
# <a name="a-web-app-that-calls-web-apis-app-registration"></a>Web API'lerini çağıran bir web uygulaması: Uygulama kaydı

Web API'lerini çağıran bir web uygulaması, kullanıcıları imzalayan bir web uygulamasıyla aynı kayda sahiptir. Yani, [kullanıcılarda imzalayan bir web uygulamasındaki talimatları izleyin: Uygulama kaydı.](scenario-web-app-sign-user-app-registration.md)

Ancak, web uygulaması artık web API'leri de aradığı için, gizli bir istemci uygulaması haline gelir. Bu yüzden bazı ekstra kayıt gereklidir. Uygulama, istemci kimlik bilgilerini veya *sırları*Microsoft kimlik platformuyla paylaşmalıdır.

[!INCLUDE [Registration of client secrets](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API izinleri

Web uygulamaları, oturum açmış kullanıcı adına API'leri arar. Bunu yapmak için, *temsilci izni istemeleri*gerekir. Ayrıntılar için web [API'lerine erişmek için izin ekle'ye](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API'lerini çağıran bir web uygulaması: Kod yapılandırması](scenario-web-app-call-api-app-configuration.md)
