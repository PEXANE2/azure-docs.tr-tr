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
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 316ab055a077b251e88421ab26997f8556a6e31f
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74482481"
---
# <a name="scenario-web-app-that-signs-in-users"></a>Senaryo: kullanıcılarda oturum açan Web uygulaması

Kullanıcıların oturum açması için Microsoft Identity platformunu kullanan bir Web uygulaması oluşturmak için ihtiyacınız olan tüm kullanıcıları öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Başlarken

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Çekirdeği](#tab/aspnetcore)

Kullanıcılar tarafından oturum açan ilk taşınabilir (ASP.NET Core) Web uygulamanızı oluşturmak istiyorsanız bu hızlı başlangıcı izleyin:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: kullanıcılarda oturum açan Web uygulamasını ASP.NET Core](quickstart-v2-aspnet-core-webapp.md)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Mevcut bir ASP.NET Web uygulamasına oturum açma ekleme hakkında bilgi edinmek istiyorsanız, aşağıdaki hızlı başlangıcı deneyin:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: kullanıcılar oturum açan ASP.NET Web uygulaması](quickstart-v2-aspnet-webapp.md)

# <a name="javatabjava"></a>[Java](#tab/java)

Java geliştiricisiyseniz aşağıdaki hızlı başlangıcı deneyin:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Microsoft 'a Java Web uygulamasına oturum açma ekleme](quickstart-v2-java-webapp.md)

# <a name="pythontabpython"></a>[Python](#tab/python)

Python ile geliştirirseniz, aşağıdaki hızlı başlangıcı deneyin:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Microsoft 'a Python web uygulamasına oturum açma ekleme](quickstart-v2-python-webapp.md)

---

## <a name="overview"></a>Genel Bakış

Web uygulamanıza, kullanıcıların oturum açmasını sağlamak için kimlik doğrulaması eklersiniz. Kimlik doğrulaması eklemek, Web uygulamanızın, Kullanıcı deneyimini özelleştirmek için sınırlı profil bilgilerine erişmesini sağlar. 

Web Apps bir kullanıcının kimliğini bir Web tarayıcısında doğrular. Bu senaryoda, Web uygulaması, kullanıcının tarayıcısını Azure Active Directory (Azure AD) içinde oturum açmak üzere yönlendirir. Azure AD, kullanıcının tarayıcısı aracılığıyla bir güvenlik belirtecinde Kullanıcı hakkında talepler içeren bir oturum açma yanıtı döndürür. Kullanıcıların oturum açması, ara yazılım [kitaplıkları](scenario-web-app-sign-user-app-configuration.md#libraries-for-protecting-web-apps)kullanılarak Basitleştirilen [Açık kimlik Connect](./v2-protocols-oidc.md) standart protokolünden yararlanır.

![Kullanıcılarda Web uygulaması işaretleri](./media/scenario-webapp/scenario-webapp-signs-in-users.svg)

İkinci bir aşamada, uygulamanızı oturum açmış kullanıcı adına Web API 'Leri çağırabilmesini sağlayabilirsiniz. Bu sonraki aşama, Web [API 'lerini çağıran Web](scenario-web-app-call-api-overview.md)uygulamasında bulacağınız farklı bir senaryodur.

> [!NOTE]
> Web uygulamasına oturum açma ekleme, Web uygulamasını koruma ve **Ara yazılım** kitaplıklarının yaptığı bir kullanıcı belirtecini doğrulama ile ilgilidir. .NET söz konusu olduğunda, bu senaryo, korumalı API 'Leri çağırmak için bir belirteç almak üzere Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) henüz gerektirmez. Web uygulamasının Web API 'Lerini çağırması gerektiğinde, izleme senaryosunda kimlik doğrulama kitaplıkları tanıtılacaktır.

## <a name="specifics"></a>Özelliklerini

- Uygulama kaydı sırasında, bir veya birkaç (uygulamanızı birkaç konuma dağıtırsanız) yanıt URI 'Lerini sağlamanız gerekir. Bazı durumlarda (ASP.NET ve ASP.NET Core), KIMLIK belirtecini etkinleştirmeniz gerekir. Son olarak, uygulamanızın kullanıcı oturumunu açmasını sağlayacak bir oturum açma URI 'SI ayarlamak isteyeceksiniz.
- Uygulamanızın kodunda, Web uygulamanızın temsilci olarak çalıştırılacağı yetkiyi sağlamanız gerekir. Belirteç doğrulamayı (özellikle de iş ortağı senaryolarında) özelleştirmek isteyebilirsiniz.
- Web uygulamaları tüm hesap türlerini destekler. Daha fazla bilgi için bkz. [Desteklenen hesap türleri](v2-supported-account-types.md).

## <a name="next-steps"></a>Sonraki adımlar

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Çekirdeği](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=aspnetcore)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=aspnet)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=java)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=python)

---
