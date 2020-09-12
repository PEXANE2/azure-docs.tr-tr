---
title: Web API 'Lerini çağıran bir Web API 'sini kaydetme-Microsoft Identity platform | Mavisi
description: Aşağı akış Web API 'Lerini (uygulama kaydı) çağıran bir Web API 'SI oluşturmayı öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c0b009d6d53a117aa11d0629fb649b2dd55559af
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89438203"
---
# <a name="a-web-api-that-calls-web-apis-app-registration"></a>Web API 'Lerini çağıran bir Web API 'SI: uygulama kaydı

Aşağı akış Web API 'Lerini çağıran bir Web API 'si, korumalı bir Web API 'SI ile aynı kayda sahiptir. Bu nedenle, [korumalı Web API 'si: uygulama kaydı](scenario-protected-web-api-app-registration.md)'ndaki yönergeleri izlemeniz gerekir.

Web uygulaması artık Web API 'Lerini çağırdığı için gizli bir istemci uygulaması haline gelir. Ek kayıt bilgileri de gereklidir: uygulamanın, Microsoft Identity platformu ile gizli dizileri (istemci kimlik bilgileri) paylaşması gerekir.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API izinleri

Web Apps, taşıyıcı belirtecinin alındığı Kullanıcı adına API 'Leri çağırır. Web uygulamalarının temsilci izinleri istemesi gerekir. Daha fazla bilgi için bkz. [Web API 'nize erişmek için Izin ekleme](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-your-web-api).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Leri çağıran bir Web API 'SI: kod yapılandırması](scenario-web-api-call-api-app-configuration.md)
