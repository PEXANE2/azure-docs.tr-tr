---
title: Bir Web uygulamasından oturum açın - Microsoft kimlik platformu | Azure
description: Kullanıcılarda işaretleyen bir web uygulaması oluşturmayı öğrenin (genel bakış)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 65d254cec5735c54e19f5adfde57fb6aed776a2c
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80881494"
---
# <a name="scenario-web-app-that-signs-in-users"></a>Senaryo: Kullanıcılarda işaretleyen Web uygulaması

Kullanıcıları oturum etmek için Microsoft kimlik platformlarını kullanan bir web uygulaması oluşturmak için ihtiyacınız olan her şeyi öğrenin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Başlarken

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Kullanıcılarda işaretleyen ilk taşınabilir (ASP.NET Core) web uygulamanızı oluşturmak istiyorsanız, aşağıdaki hızlı başlangıcı izleyin:

> [!div class="nextstepaction"]
> [Quickstart: ASP.NET Core web uygulaması bu kullanıcılarda işaretler](quickstart-v2-aspnet-core-webapp.md)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Varolan bir ASP.NET web uygulamasına oturum açma nın nasıl eklendirilebildiğini anlamak istiyorsanız, aşağıdaki hızlı başlatmayı deneyin:

> [!div class="nextstepaction"]
> [Quickstart: ASP.NET web uygulaması bu kullanıcılarda imzalar](quickstart-v2-aspnet-webapp.md)

# <a name="java"></a>[Java](#tab/java)

Java geliştiricisiyseniz, aşağıdaki hızlı başlatmayı deneyin:

> [!div class="nextstepaction"]
> [Quickstart: Java web uygulamasına Microsoft ile oturum açma ekleme](quickstart-v2-java-webapp.md)

# <a name="python"></a>[Python](#tab/python)

Python ile gelişirseniz, aşağıdaki hızlı başlangıcı deneyin:

> [!div class="nextstepaction"]
> [Quickstart: Python web uygulamasına Microsoft ile oturum açma ekleme](quickstart-v2-python-webapp.md)

---

## <a name="overview"></a>Genel Bakış

Web uygulamanıza, kullanıcılararasında oturum açabilmesi için kimlik doğrulaması eklersiniz. Kimlik doğrulama eklemek, web uygulamanızın kullanıcılar için deneyimi özelleştirmek için sınırlı profil bilgilerine erişmesini sağlar. 

Web uygulamaları bir web tarayıcısında bir kullanıcının kimliğini doğrular. Bu senaryoda, web uygulaması kullanıcının tarayıcısını Azure Etkin Dizin 'de (Azure AD) oturum açmaları için yönlendirir. Azure AD, kullanıcının tarayıcısı aracılığıyla, güvenlik jetonundaki kullanıcı yla ilgili iddiaları içeren bir oturum açma yanıtı döndürür. Kullanıcılarda oturum açma, ara yazılım [kitaplıklarının](scenario-web-app-sign-user-app-configuration.md#libraries-for-protecting-web-apps)kullanımıyla basitleştirilen [Open ID Connect](./v2-protocols-oidc.md) standart protokolünden yararlanır.

![Kullanıcılarda Web uygulaması işaretleri](./media/scenario-webapp/scenario-webapp-signs-in-users.svg)

İkinci aşama olarak, uygulamanızın oturum açmış kullanıcı adına web API'lerini aramasını etkinleştirebilirsiniz. Bu sonraki aşama, [web API'lerini çağıran Web uygulamasında](scenario-web-app-call-api-overview.md)bulacağınız farklı bir senaryodur.

> [!NOTE]
> Bir web uygulamasına oturum açma eklemek, web uygulamasını korumak ve bir kullanıcı belirteciyle ilgilidir, bu **da ara yazılım** kitaplıkları tarafından yapılır. .NET durumunda, bu senaryo henüz microsoft kimlik doğrulama kitaplığı (MSAL), korumalı API'ler aramak için bir belirteç edinme hakkında gerektirmez. Web uygulamasının web API'lerini araması gerektiğinde, takip senaryosunda kimlik doğrulama kitaplıkları sunulacaktır.

## <a name="specifics"></a>Özellikleri

- Uygulama kaydı sırasında, bir veya birkaç (uygulamanızı çeşitli konumlara dağıtırsanız) Url'leri yanıtlamanız gerekir. Bazı durumlarda (ASP.NET ve ASP.NET Core), kimlik belirteci etkinleştirmeniz gerekir. Son olarak, uygulamanızın oturumdan çıkan kullanıcılara tepki göstermesi için bir çıkış URI'si ayarlamak isteyebilirsiniz.
- Uygulamanızın kodunda, web uygulaması temsilcilerinizin oturum açma yetkisini sağlamanız gerekir. Belirteç doğrulaması (özellikle, iş ortağı senaryolarında) özelleştirmek isteyebilirsiniz.
- Web uygulamaları tüm hesap türlerini destekler. Daha fazla bilgi için [desteklenen hesap türlerine](v2-supported-account-types.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=aspnetcore)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=aspnet)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=java)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=python)

---
