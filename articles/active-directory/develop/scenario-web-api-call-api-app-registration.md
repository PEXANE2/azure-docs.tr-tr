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
ms.openlocfilehash: 048f7d41bd9d106121859e6b1fc013258067af9c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80885132"
---
# <a name="a-web-api-that-calls-web-apis-app-registration"></a>Web API 'Lerini çağıran bir Web API 'SI: uygulama kaydı

Aşağı akış Web API 'Lerini çağıran bir Web API 'si, korumalı bir Web API 'SI ile aynı kayda sahiptir. Bu nedenle, [korumalı Web API 'si: uygulama kaydı](scenario-protected-web-api-app-registration.md)'ndaki yönergeleri izlemeniz gerekir.

Web uygulaması artık Web API 'Lerini çağırdığı için gizli bir istemci uygulaması haline gelir. Ek kayıt bilgileri de gereklidir: uygulamanın, Microsoft Identity platformu ile gizli dizileri (istemci kimlik bilgileri) paylaşması gerekir.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API izinleri

Web Apps, taşıyıcı belirtecinin alındığı Kullanıcı adına API 'Leri çağırır. Web uygulamalarının temsilci izinleri istemesi gerekir. Daha fazla bilgi için bkz. [Web API 'lerine erişim Izinleri ekleme](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Leri çağıran bir Web API 'SI: kod yapılandırması](scenario-web-api-call-api-app-configuration.md)
