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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 784de823e94aace6f91222c19c1ff8130c3f995f
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74962942"
---
# <a name="web-app-that-calls-web-apis---app-registration"></a>Web API 'Lerini çağıran Web uygulaması-uygulama kaydı

Web API 'Lerini çağıran bir Web uygulaması, bir Web uygulaması ile oturum açan kullanıcılar ile aynı kayda sahiptir. Bu nedenle, [kullanıcıların oturum açtığı Web](scenario-web-app-sign-user-app-registration.md) uygulamasındaki yönergeleri uygulamanız gerekir

Ancak, Web uygulaması artık Web API 'Lerini çağırırsa, bu, gizli bir istemci uygulaması haline gelir. Bu nedenle çok sayıda ek kayıt gereklidir: Microsoft Identity platformu ile gizli dizileri (istemci kimlik bilgileri) paylaşmalıdır.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API izinleri

Web uygulamaları, oturum açmış kullanıcı adına API 'Leri çağırır. Temsilci izinleri istemeleri gerekir. Ayrıntılar için bkz. [Web API 'lerine erişim Izinleri ekleme](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulamanın kod yapılandırması](scenario-web-app-call-api-app-configuration.md)
