---
title: Web API'leri çağıran bir web uygulaması oluşturma - Microsoft kimlik platformu | Azure
description: Web API'lerini çağıran bir web uygulaması oluşturmayı öğrenin (genel bakış)
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
ms.openlocfilehash: 14f513306b3f0bc0c06a4143e5174c3ecddaef62
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80617157"
---
# <a name="scenario-a-web-app-that-calls-web-apis"></a>Senaryo: Web API'lerini çağıran bir web uygulaması

Kullanıcıları Microsoft kimlik platformuna kaydeden ve oturum açmış kullanıcı adına web API'lerini çağıran bir web uygulaması oluşturmayı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

Bu senaryo, aşağıdaki senaryoyu zaten geçtiğinizvarsa varsayıyor:

> [!div class="nextstepaction"]
> [Kullanıcıların oturum açtığı web uygulaması](scenario-web-app-sign-user-overview.md)

## <a name="overview"></a>Genel Bakış

Oturum açabilmek ve oturum açan kullanıcı adına bir web API'sını arayabilmek için web uygulamanıza kimlik doğrulaması eklersiniz.

![Web API'lerini çağıran web uygulaması](./media/scenario-webapp/web-app.svg)

Web API'lerini arayan web uygulamaları gizli istemci uygulamalarıdır.
Bu nedenle Azure Active Directory (Azure AD) ile gizli (uygulama parolası veya sertifika) kaydederler. Bu sır, bir belirteç almak için Azure AD'ye yapılan çağrı sırasında iletilir.

## <a name="specifics"></a>Özellikleri

> [!NOTE]
> Bir web uygulamasına oturum açma eklemek, web uygulamasının kendisini korumakla ilgilidir. Bu koruma, Microsoft Kimlik Doğrulama Kitaplığı (MSAL) değil, *ara yazılım* kitaplıkları kullanılarak elde edilir. Önceki senaryo, [Web uygulaması bu konuda kullanıcılarda işaretler,](scenario-web-app-sign-user-overview.md)bu konuyu kapsamaktadır.
>
> Bu senaryo, bir web uygulamasından web API'leri nasıl çağrılmasını kapsar. Bu web API'leri için erişim belirteçleri almalısınız. Bu belirteçleri elde etmek için MSAL kitaplıklarını kullanırsınız.

Bu senaryo için geliştirme şu özel görevleri içerir:

- [Uygulama kaydı](scenario-web-app-call-api-app-registration.md)sırasında, Azure AD ile paylaşılmak üzere bir yanıt URI, gizli veya sertifika sağlamanız gerekir. Uygulamanızı çeşitli konumlara dağıtırsanız, bu bilgileri her konum için sağlarsınız.
- [Uygulama yapılandırması,](scenario-web-app-call-api-app-configuration.md) uygulama kaydı sırasında Azure AD ile paylaşılan istemci kimlik bilgilerini sağlamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API'lerini çağıran bir web uygulaması: Uygulama kaydı](scenario-web-app-call-api-app-registration.md)
