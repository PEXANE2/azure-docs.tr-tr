---
title: Web API'lerini üretime çağıran bir daemon uygulamasını taşıma - Microsoft kimlik platformu | Azure
description: Web API'lerini üretime çağıran bir daemon uygulamasını nasıl taşıyın
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: db5f52c95daf4e93c140b4c93f39dad19971319d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262626"
---
# <a name="daemon-app-that-calls-web-apis---move-to-production"></a>Web API'leri çağıran Daemon uygulaması - üretime geçin

Artık bir hizmet çağrısı için bir jetonu nasıl elde edip kullanacağınızı bildiğinize göre, uygulamanızı üretime nasıl taşıyabileceğinizi öğrenin.

## <a name="deployment---multitenant-daemon-apps"></a>Dağıtım - çok kiracılı daemon uygulamaları

Birkaç kiracı da çalıştırabilen bir daemon uygulaması oluşturan bir ISV iseniz, kiracı yönetici emin olmanız gerekir:

- Hükümler uygulama için bir hizmet müdürü.
- Başvuruya onay verir.

Müşterilerinize bu işlemleri nasıl gerçekleştireceğiniz konusunda açıklama yapmanız gerekir. Daha fazla bilgi için [bkz.](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant)

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmenize yardımcı olacak birkaç bağlantı aşağıda veda edebilirsiniz:

# <a name="net"></a>[.NET](#tab/dotnet)

- Hızlı başlangıç: [Bir belirteç edinin ve uygulamanın kimliğini kullanarak bir konsol uygulamasından Microsoft Graph API'yi arayın.](./quickstart-v2-netcore-daemon.md)
- Başvuru belgeleri:
  - Anında [GizliMüşteri Uygulaması](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder).
  - [Arama AcquireTokenForClient](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenforclientparameterbuilder).
- Diğer örnekler/öğreticiler:
  - [microsoft-identity-platform-console-daemon,](https://github.com/Azure-Samples/microsoft-identity-platform-console-daemon) Microsoft Graph sorgulayan bir kiracının kullanıcılarını görüntüleyen basit bir .NET Core daemon konsol uygulamasına sahiptir.

    ![Örnek daemon uygulaması topolojisi](media/scenario-daemon-app/daemon-app-sample.svg)

    Aynı örnek, sertifikalarla ilgili bir varyasyonu da göstermektedir:

    ![Örnek daemon uygulaması topolojisi - sertifikalar](media/scenario-daemon-app/daemon-app-sample-with-certificate.svg)

  - [microsoft-identity-platform-aspnet-webapp-daemon,](https://github.com/Azure-Samples/microsoft-identity-platform-aspnet-webapp-daemon) kullanıcı adına değil, uygulamanın kimliğini kullanarak Microsoft Graph'tan gelen verileri senkronize eden bir ASP.NET MVC web uygulamasına sahiptir. Bu örnek aynı zamanda yönetici onay sürecini de göstermektedir.

    ![topology](media/scenario-daemon-app/damon-app-sample-web.svg)

# <a name="python"></a>[Python](#tab/python)

Hızlı başlat'ı deneyin [Bir belirteç edinin ve uygulamanın kimliğini kullanarak Python konsol uygulamasından Microsoft Graph API'yi arayın.](./quickstart-v2-python-daemon.md)

# <a name="java"></a>[Java](#tab/java)

MSAL Java şu anda genel önizlemededir. Daha fazla bilgi için [MSAL Java geliştirme örneklerine](https://github.com/AzureAD/microsoft-authentication-library-for-java/tree/dev/src/samples)bakın.

---
