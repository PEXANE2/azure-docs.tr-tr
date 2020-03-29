---
title: Web API'larını çağıran bir web API'sı kaydolun - Microsoft kimlik platformu | Azure
description: Akış aşağı web API'lerini (uygulama kaydı) çağıran bir web API'sini nasıl oluştururduğa öğrenin.
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
ms.openlocfilehash: bafd71f34602535bb6193a8d8114a1182e4e8f40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76701799"
---
# <a name="a-web-api-that-calls-web-apis-app-registration"></a>Web API'lerini çağıran bir web API'sı: Uygulama kaydı

Alt akış web API'lerini çağıran bir web API'sı, korunan web API'si ile aynı kayda sahiptir. Bu nedenle, Korumalı web API yönergelerini izlemeniz [gerekir: Uygulama kaydı.](scenario-protected-web-api-app-registration.md)

Web uygulaması artık web API'lerini aradığı için, gizli bir istemci uygulaması haline gelir. Bu nedenle ek kayıt bilgileri gereklidir: uygulamanın sırları (istemci kimlik bilgileri) Microsoft kimlik platformuyla paylaşması gerekir.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API izinleri

Web uygulamaları, taşıyıcı belirteci alınan kullanıcılar adına API'leri arar. Web uygulamalarının temsilci izni istemesi gerekir. Daha fazla bilgi için [bkz.](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API'lerini çağıran bir web API'sı: Kod yapılandırması](scenario-web-api-call-api-app-configuration.md)
