---
title: Web API 'Lerini çağıran bir Web uygulamasını kaydetme-Microsoft Identity platform | Mavisi
description: Web API 'Lerini çağıran bir Web uygulamasını nasıl kaydedeceğinizi öğrenin
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
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76759066"
---
# <a name="a-web-app-that-calls-web-apis-app-registration"></a>Web API 'Leri çağıran bir Web uygulaması: uygulama kaydı

Web API 'Lerini çağıran bir Web uygulamasının, içindeki kullanıcıları imzaladığında bir Web uygulamasıyla aynı kaydı vardır. Bu nedenle, [kullanıcıları oturum açan bir Web uygulamasındaki yönergeleri izleyin: uygulama kaydı](scenario-web-app-sign-user-app-registration.md).

Ancak, Web uygulaması artık Web API 'Lerini de çağırdığı için gizli bir istemci uygulaması haline gelir. Bu nedenle bazı ek kayıt gereklidir. Uygulama, Microsoft Identity platformu ile istemci kimlik bilgilerini veya *gizli*dizileri paylaşmalıdır.

[!INCLUDE [Registration of client secrets](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API izinleri

Web Apps, oturum açmış kullanıcı adına API 'Leri çağırır. Bunu yapmak için, *temsilci izinleri*istemeleri gerekir. Ayrıntılar için bkz. [Web API 'lerine erişim Izinleri ekleme](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Leri çağıran bir Web uygulaması: kod yapılandırması](scenario-web-app-call-api-app-configuration.md)
