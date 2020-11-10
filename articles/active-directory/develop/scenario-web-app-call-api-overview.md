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
ms.date: 07/14/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: b4123b0424e52c6a2919de4b60bcc6158062b0f1
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94442642"
---
# <a name="scenario-a-web-app-that-calls-web-apis"></a>Senaryo: Web API 'Lerini çağıran bir Web uygulaması

Microsoft Identity platformunda kullanıcıları imzalayan ve ardından oturum açmış kullanıcı adına Web API 'Lerini çağıran bir Web uygulaması oluşturmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

Bu senaryo, zaten bir senaryoyu tamamlamış olduğunu varsayar [: kullanıcıları oturum açan Web uygulaması](scenario-web-app-sign-user-overview.md).

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

- [Uygulama kaydı](scenario-web-app-call-api-app-registration.md)SıRASıNDA Azure AD ile paylaşılacak bir yanıt URI 'si, gizli anahtar veya sertifika sağlamanız gerekir. Uygulamanızı çeşitli konumlara dağıtırsanız, her konum için bir yanıt URI 'SI sağlarsınız.
- [Uygulama yapılandırması](scenario-web-app-call-api-app-configuration.md) , uygulama kaydı SıRASıNDA Azure AD ile paylaşılan istemci kimlik bilgilerini sağlamalıdır.

## <a name="recommended-reading"></a>Önerilen okuma

[!INCLUDE [recommended-topics](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu senaryonun [uygulama kaydı](scenario-web-app-call-api-app-registration.md)olan sonraki makaleye geçin.