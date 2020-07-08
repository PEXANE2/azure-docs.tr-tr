---
title: Web API 'Lerini çağıran bir Web uygulaması oluşturma-Microsoft Identity platform | Mavisi
description: Web API 'Lerini çağıran bir Web uygulaması oluşturmayı öğrenin (genel bakış)
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
ms.openlocfilehash: 5af9e34baf6115e801fbfe35e6e3895e48b360e7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80881732"
---
# <a name="scenario-a-web-app-that-calls-web-apis"></a>Senaryo: Web API 'Lerini çağıran bir Web uygulaması

Microsoft Identity platformunda kullanıcıları imzalayan ve ardından oturum açmış kullanıcı adına Web API 'Lerini çağıran bir Web uygulaması oluşturmayı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

Bu senaryo, aşağıdaki senaryodan zaten geçmiş olduğunu varsayar:

> [!div class="nextstepaction"]
> [Kullanıcıların oturum açtığı web uygulaması](scenario-web-app-sign-user-overview.md)

## <a name="overview"></a>Genel Bakış

' Deki kullanıcıları imzalamak ve oturum açmış kullanıcı adına bir Web API 'SI çağırmak için Web uygulamanıza kimlik doğrulaması eklersiniz.

![Web API’lerini çağıran web uygulaması](./media/scenario-webapp/web-app.svg)

Web API 'Lerini çağıran Web uygulamaları gizli istemci uygulamalardır.
Bu nedenle, Azure Active Directory (Azure AD) ile bir gizli dizi (uygulama parolası veya sertifika) kaydettikleri budur. Bu gizli dizi, bir belirteç almak için Azure AD çağrısı sırasında geçirilir.

## <a name="specifics"></a>Özelliklerini

> [!NOTE]
> Web uygulamasına oturum açma ekleme, Web uygulamasının kendi kendine korunmasını sağlar. Bu koruma, Microsoft kimlik doğrulama kitaplığı (MSAL) değil, *Ara yazılım* kitaplıkları kullanılarak elde edilir. Yukarıdaki senaryo, [kullanıcılar tarafından oturum açan Web uygulaması](scenario-web-app-sign-user-overview.md), bu konu ele alınmıştır.
>
> Bu senaryo, bir Web uygulamasından Web API 'Lerinin nasıl çağrılacağını ele alır. Bu Web API 'Leri için erişim belirteçleri almalısınız. Bu belirteçleri almak için MSAL kitaplıklarını kullanırsınız.

Bu senaryoya yönelik geliştirme, bu özel görevleri içerir:

- [Uygulama kaydı](scenario-web-app-call-api-app-registration.md)SıRASıNDA Azure AD ile paylaşılacak bir yanıt URI 'si, gizli anahtar veya sertifika sağlamanız gerekir. Uygulamanızı çeşitli konumlara dağıtırsanız, bu bilgileri her konum için sağlarsınız.
- [Uygulama yapılandırması](scenario-web-app-call-api-app-configuration.md) , uygulama kaydı SıRASıNDA Azure AD ile paylaşılan istemci kimlik bilgilerini sağlamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Leri çağıran bir Web uygulaması: uygulama kaydı](scenario-web-app-call-api-app-registration.md)
