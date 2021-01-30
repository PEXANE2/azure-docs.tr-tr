---
title: Web API 'Lerini üretime çağıran bir Daemon uygulamasını taşıma | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'Lerini üretime çağıran bir Daemon uygulamasını nasıl taşıyacağınızı öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 04ee4ccc3b0553d6532869d6403000fdedd93b5f
ms.sourcegitcommit: 1a98b3f91663484920a747d75500f6d70a6cb2ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99062720"
---
# <a name="daemon-app-that-calls-web-apis---move-to-production"></a>Web API 'Lerini çağıran Daemon uygulaması-üretime taşı

Artık bir hizmetten hizmete çağrı için bir belirteç edinmeyi ve kullanmayı öğrenmiş olduğunuza göre, uygulamanızı üretime taşımayı öğrenin.

## <a name="deployment---multitenant-daemon-apps"></a>Dağıtım-çok kiracılı Daemon uygulamaları

Birkaç kiracıda çalışabilen bir arka plan programı uygulaması oluşturuyorsanız, kiracı yöneticisinin şu şekilde olduğundan emin olmanız gerekir:

- Uygulama için bir hizmet sorumlusu sağlar.
- Uygulamaya onay verir.

Müşterilerinize bu işlemleri gerçekleştirme hakkında açıklayabilmeniz gerekir. Daha fazla bilgi için, bkz. [Tüm kiracı için Izin isteme](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant).

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="code-samples"></a>Kod örnekleri

# <a name="net"></a>[.NET](#tab/dotnet)

- İçin başvuru belgeleri:
  - [ConfidentialClientApplication](/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder)örneği oluşturuluyor.
  - [Acquiretokenforclient](/dotnet/api/microsoft.identity.client.acquiretokenforclientparameterbuilder)çağrılıyor.
- Diğer örnekler/Öğreticiler:
  - [Microsoft-Identity-platform-Console-Daemon](https://github.com/Azure-Samples/microsoft-identity-platform-console-daemon) , bir kiracı tarafından Microsoft Graph sorgulayan kullanıcıları görüntüleyen küçük bir .NET Core Daemon konsol uygulamasına sahiptir.

    ![Örnek Daemon uygulama topolojisi](media/scenario-daemon-app/daemon-app-sample.svg)

    Aynı örnek, sertifikalarla bir çeşitlemesi de gösterir:

    ![Örnek Daemon uygulama topolojisi-sertifikalar](media/scenario-daemon-app/daemon-app-sample-with-certificate.svg)

  - [Microsoft-Identity-platform-ASPNET-WebApp-Daemon](https://github.com/Azure-Samples/microsoft-identity-platform-aspnet-webapp-daemon) , bir kullanıcı adına değil uygulamanın kimliğini kullanarak Microsoft Graph verileri eşitlenen BIR ASP.NET MVC web uygulamasıdır. Bu örnek, yönetici onay işlemini de gösterir.

    ![topology](media/scenario-daemon-app/damon-app-sample-web.svg)

---

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmenize yardımcı olacak birkaç bağlantı aşağıda verilmiştir:

# <a name="python"></a>[Python](#tab/python)

Hızlı Başlangıç ['ı bir belirteç alma ve uygulama kimliğini kullanarak bir Python konsol uygulamasından MICROSOFT Graph API çağrısı](./quickstart-v2-python-daemon.md)yapın.

# <a name="java"></a>[Java](#tab/java)

Hızlı Başlangıç ['ı bir belirteç alma ve uygulama kimliğini kullanarak bir Java konsol uygulamasından MICROSOFT Graph API çağrısı](./quickstart-v2-java-daemon.md)yapın.

---
