---
title: Kullanıcılara oturum açan Web uygulaması (genel bakış)-Microsoft Identity platform
description: Kullanıcılara oturum açan bir Web uygulaması oluşturma hakkında bilgi edinin (genel bakış)
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
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95aeeacfd85dd79453bff4e365e5b050039f77b9
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852459"
---
# <a name="scenario-web-app-that-signs-in-users"></a>Senaryo: Kullanıcıların oturum açtığı web uygulaması

Kullanıcıları Microsoft Identity platformu ile oturum açan bir Web uygulaması oluşturmak için ihtiyacınız olan her şey hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Başlarken

Kullanıcıların oturum açmasını sağlayan ilk taşınabilir (ASP.NET Core) Web uygulamalarınızı oluşturmak istiyorsanız bu hızlı başlangıcı izleyin:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Kullanıcıları oturum açan ASP.NET Core Web uygulaması](quickstart-v2-aspnet-core-webapp.md)

ASP.NET ile kalmak isterseniz, aşağıdaki öğreticiyi deneyin:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Kullanıcıları oturum açan ASP.NET Web uygulaması](quickstart-v2-aspnet-webapp.md)

## <a name="overview"></a>Genel Bakış

Web uygulamanıza kimlik doğrulaması ekleyerek kullanıcıların oturum açmasını sağlayabilirsiniz. Kimlik doğrulaması eklemek, Web uygulamanızın sınırlı profil bilgilerine erişmesini sağlar ve örneğin kullanıcılarına sunduğunuz deneyimi özelleştirmek için. Web Apps bir kullanıcının kimliğini bir Web tarayıcısında doğrular. Bu senaryoda Web uygulaması, kullanıcının tarayıcısını Azure AD 'de oturum açmak üzere yönlendirir. Azure AD, kullanıcının tarayıcısı aracılığıyla bir güvenlik belirtecinde Kullanıcı hakkında talepler içeren bir oturum açma yanıtı döndürür. Oturum açma kullanıcıları, ara yazılım [kitaplıklarının](scenario-web-app-sign-user-app-configuration.md#libraries-used-to-protect-web-apps)kullanımı Ile BASITLEŞTIRILMIŞ [Açık kimlik Connect](./v2-protocols-oidc.md) standart protokolünü kullanır.

![Web uygulaması oturum açan kullanıcılar](./media/scenario-webapp/scenario-webapp-signs-in-users.svg)

İkinci bir aşamada, uygulamanızı oturum açmış kullanıcı adına Web API 'Leri çağırmak için de etkinleştirebilirsiniz. Bu sonraki aşama, Web [uygulaması çağrılarında Web API 'lerinde](scenario-web-app-call-api-overview.md) bulacağınız farklı bir senaryodur.

> [!NOTE]
> Web uygulamasına oturum açma ekleme, Web uygulamasını koruma ve **Ara yazılım** kitaplıklarının yaptığı bir kullanıcı belirtecini doğrulama ile ilgilidir. Bu senaryo, korumalı API 'Leri çağırmak için bir belirteç almak üzere, henüz Microsoft kimlik doğrulama kitaplıklarını (MSAL) gerektirmez. Kimlik doğrulama kitaplıkları yalnızca Web uygulamasının Web API 'Lerini çağırması gerektiğinde izleme senaryosunda tanıtılacaktır.

## <a name="specifics"></a>Özelliklerini

- Uygulama kaydı sırasında, bir veya birkaç tane (uygulamanızı birkaç konuma dağıtırsanız) yanıt URI 'Lerini sağlamanız gerekir. Bazı durumlarda (ASP.NET/ASP.NET Core) ıdtoken 'ı etkinleştirmeniz gerekir. Son olarak, uygulamanızın kullanıcı oturumu açmasını yeniden yapabilmesi için bir oturum açma URI 'SI ayarlamak isteyeceksiniz.
- Uygulamanızın kodunda, Web uygulamasının temsilci olarak oturum açma yetkisini sağlamanız gerekir. Belirteç doğrulamasını özelleştirmek isteyebilirsiniz (özellikle ISV senaryolarında).
- Web uygulamaları tüm hesap türlerini destekler. Daha fazla bilgi için bkz. [Desteklenen hesap türleri](v2-supported-account-types.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-web-app-sign-user-app-registration.md)
