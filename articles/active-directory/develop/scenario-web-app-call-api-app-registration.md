---
title: Web API 'Lerini çağıran bir Web uygulamasını kaydetme-Microsoft Identity platform | Mavisi
description: Web API 'Lerini çağıran bir Web uygulamasını nasıl kaydedeceğinizi öğrenin
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
ms.openlocfilehash: f94a3da96243e30faa90277ce86efec037f54672
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89436484"
---
# <a name="a-web-app-that-calls-web-apis-app-registration"></a>Web API 'Leri çağıran bir Web uygulaması: uygulama kaydı

Web API 'Lerini çağıran bir Web uygulamasının, içindeki kullanıcıları imzaladığında bir Web uygulamasıyla aynı kaydı vardır. Bu nedenle, [kullanıcıları oturum açan bir Web uygulamasındaki yönergeleri izleyin: uygulama kaydı](scenario-web-app-sign-user-app-registration.md).

Ancak, Web uygulaması artık Web API 'Lerini de çağırdığı için gizli bir istemci uygulaması haline gelir. Bu nedenle bazı ek kayıt gereklidir. Uygulama, Microsoft Identity platformu ile istemci kimlik bilgilerini veya *gizli*dizileri paylaşmalıdır.

[!INCLUDE [Registration of client secrets](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API izinleri

Web Apps, oturum açmış kullanıcı adına API 'Leri çağırır. Bunu yapmak için, *temsilci izinleri*istemeleri gerekir. Ayrıntılar için bkz. [Web API 'nize erişmek için Izin ekleme](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-your-web-api).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Leri çağıran bir Web uygulaması: kod yapılandırması](scenario-web-app-call-api-app-configuration.md)
