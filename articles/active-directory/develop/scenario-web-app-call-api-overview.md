---
title: Web API 'Lerini çağıran bir Web uygulaması oluşturma-Microsoft Identity platform | Mavisi
description: Web API 'Lerini çağıran bir Web uygulaması oluşturmayı öğrenin (genel bakış)
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
ms.openlocfilehash: 3888c7f838d6009382f849bc7d3e34c49b3b70a4
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74962143"
---
# <a name="scenario-web-app-that-calls-web-apis"></a>Senaryo: Web API 'Lerini çağıran Web uygulaması

Microsoft Identity platformunda kullanıcılar oturumunu açan ve ardından oturum açmış kullanıcı adına Web API 'Lerini çağıran bir Web uygulaması oluşturmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

Bu senaryo, aşağıdaki senaryodan geçmiş olduğunu ortadan kaldırdık:

> [!div class="nextstepaction"]
> [Kullanıcıları oturum açan Web uygulaması](scenario-web-app-sign-user-overview.md)

## <a name="overview"></a>Genel Bakış

Web uygulamanıza kimlik doğrulaması eklersiniz, bu nedenle kullanıcılar oturum açabilir ve oturum açmış kullanıcı adına bir Web API 'SI çağırır.

![Web API'lerini çağıran web uygulaması](./media/scenario-webapp/web-app.svg)

Web API 'Lerini çağıran Web Apps:

- Gizli istemci uygulamaları.
- Azure AD ile gizli dizi (uygulama parolası veya sertifika) kaydettikleri budur. Bu gizli dizi, bir belirteç almak için Azure AD çağrısı sırasında geçirilir

## <a name="specifics"></a>Özelliklerini

> [!NOTE]
> Web uygulamasına oturum açma eklemek, Web uygulamasını koruma konusunda olduğu için MSAL kitaplıklarını kullanmaz. Kitaplıkları koruma, ara yazılım adlı kitaplıklar tarafından gerçekleştirilir. Bu, önceki senaryo [oturum açma kullanıcılarının bir Web uygulamasına yönelik](scenario-web-app-sign-user-overview.md) nesnesidir
>
> Web API 'Lerini bir Web uygulamasından çağırırken, bu Web API 'Leri için erişim belirteçleri almanız gerekir. Bu belirteçleri almak için MSAL kitaplıklarını kullanabilirsiniz.

Bu senaryoya yönelik geliştiricilerin uçtan uca deneyimi, bu nedenle aşağıdaki gibi belirli yönlere sahiptir:

- [Uygulama kaydı](scenario-web-app-call-api-app-registration.md)sırasında, bir veya birkaç tane sağlamanız gerekir (uygulamanızı birkaç konuma dağıtırsanız), yanıt URI 'lerinin, parolaların veya SERTIFIKALARıN Azure AD ile paylaşılması gerekir.
- Uygulama [yapılandırmasının](scenario-web-app-call-api-app-configuration.md) , uygulama kaydı SıRASıNDA Azure AD ile paylaşılan olarak istemci kimlik bilgilerini sağlaması gerekir

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-web-app-call-api-app-registration.md)
