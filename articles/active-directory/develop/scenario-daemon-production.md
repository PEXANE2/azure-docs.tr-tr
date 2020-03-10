---
title: Web API 'Lerini üretime çağıran bir Daemon uygulamasını üretime taşıma-Microsoft Identity platform | Mavisi
description: Web API 'Lerini üretime çağıran bir Daemon uygulamasını nasıl taşıyacağınızı öğrenin
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
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78377333"
---
# <a name="daemon-app-that-calls-web-apis---move-to-production"></a>Web API 'Lerini çağıran Daemon uygulaması-üretime taşı

Artık bir hizmetten hizmete çağrı için bir belirteç edinmeyi ve kullanmayı öğrenmiş olduğunuza göre, uygulamanızı üretime taşımayı öğrenin.

## <a name="deployment---multitenant-daemon-apps"></a>Dağıtım-çok kiracılı Daemon uygulamaları

Birkaç kiracıda çalışabilen bir arka plan programı uygulaması oluşturuyorsanız, kiracı yöneticisinin şu şekilde olduğundan emin olmanız gerekir:

- Uygulama için bir hizmet sorumlusu sağlar.
- Uygulamaya onay verir.

Müşterilerinize bu işlemleri gerçekleştirme hakkında açıklayabilmeniz gerekir. Daha fazla bilgi için, bkz. [Tüm kiracı için Izin isteme](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant).

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmenize yardımcı olacak birkaç bağlantı aşağıda verilmiştir:

# <a name="net"></a>[.NET](#tab/dotnet)

- Hızlı başlangıç: [bir belirteç alın ve uygulama kimliğini kullanarak bir konsol uygulamasından Microsoft Graph API 'si çağırın](./quickstart-v2-netcore-daemon.md).
- İçin başvuru belgeleri:
  - [ConfidentialClientApplication](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder)örneği oluşturuluyor.
  - [Acquiretokenforclient](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenforclientparameterbuilder)çağrılıyor.
- Diğer örnekler/Öğreticiler:
  - [Microsoft-Identity-platform-Console-Daemon](https://github.com/Azure-Samples/microsoft-identity-platform-console-daemon) , bir kiracı Microsoft Graph sorgulayan kullanıcıları görüntüleyen basit bir .NET Core Daemon konsol uygulamasıdır.

    ![Örnek Daemon uygulama topolojisi](media/scenario-daemon-app/daemon-app-sample.svg)

    Aynı örnek, sertifikalarla bir çeşitlemesi de gösterir:

    ![Örnek Daemon uygulama topolojisi-sertifikalar](media/scenario-daemon-app/daemon-app-sample-with-certificate.svg)

  - [Microsoft-Identity-platform-ASPNET-WebApp-Daemon](https://github.com/Azure-Samples/microsoft-identity-platform-aspnet-webapp-daemon) , bir kullanıcı adına değil uygulamanın kimliğini kullanarak Microsoft Graph verileri eşitlenen BIR ASP.NET MVC web uygulamasıdır. Bu örnek, yönetici onay işlemini de gösterir.

    ![topology](media/scenario-daemon-app/damon-app-sample-web.svg)

# <a name="python"></a>[Python](#tab/python)

Hızlı Başlangıç ['ı bir belirteç alma ve uygulama kimliğini kullanarak bir Python konsol uygulamasından MICROSOFT Graph API çağrısı](./quickstart-v2-python-daemon.md)yapın.

# <a name="java"></a>[Java](#tab/java)

MSAL Java Şu anda genel önizlemededir. Daha fazla bilgi için bkz. [msal Java dev Samples](https://github.com/AzureAD/microsoft-authentication-library-for-java/tree/dev/src/samples).

---
