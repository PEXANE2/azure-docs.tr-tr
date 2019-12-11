---
title: Taşıma cini uygulaması Web API 'Lerini üretime çağırma-Microsoft Identity platform | Mavisi
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 961928499008445207df3a34a51fc016723c294b
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74962602"
---
# <a name="daemon-app-that-calls-web-apis---move-to-production"></a>Web API 'Lerini çağıran Daemon uygulaması-üretime taşı

Artık bir hizmetten hizmete çağrı için bir belirteç edinmeyi ve kullanmayı öğrenmiş olduğunuza göre, uygulamanızı üretime taşımayı öğrenin.

## <a name="deployment---case-of-multi-tenant-daemon-apps"></a>Dağıtım-çok kiracılı Daemon uygulamalarının durumu

Birkaç kiracıda çalışabilen bir arka plan programı uygulaması oluşturuyorsanız, kiracı yöneticilerinin şu şekilde olduğundan emin olmanız gerekir:

- Uygulama için bir hizmet sorumlusu sağlar
- Uygulamaya onay verir

Müşterilerinize bu işlemleri gerçekleştirme hakkında açıklayabilmeniz gerekir. Daha fazla bilgi için, bkz. [Tüm kiracı için Izin isteme](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant).

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için birkaç bağlantı aşağıda verilmiştir:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

- Henüz yapmadıysanız, Hızlı Başlangıç ' ı [bir belirteç alma ve uygulama kimliğini kullanarak bir konsol uygulamasından MICROSOFT Graph API çağrısı](./quickstart-v2-netcore-daemon.md)yapın.
- İçin başvuru belgeleri:
  - [ConfidentialClientApplication](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder) örneğini oluşturma
  - [Acquiretokenforclient](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenforclientparameterbuilder) çağrılıyor
- Diğer örnekler/Öğreticiler:
  - [Microsoft-Identity-platform-Console-Daemon](https://github.com/Azure-Samples/microsoft-identity-platform-console-daemon) , Microsoft Graph sorgulayan bir kiracının kullanıcılarını görüntüleyen basit bir .NET Core Daemon konsol uygulamasına sahiptir.

    ![topology](media/scenario-daemon-app/daemon-app-sample.svg)

    Aynı örnek, sertifikalarla çeşitlemeyi de gösterir.

    ![topology](media/scenario-daemon-app/daemon-app-sample-with-certificate.svg)

  - [Microsoft-Identity-platform-ASPNET-WebApp-Daemon](https://github.com/Azure-Samples/microsoft-identity-platform-aspnet-webapp-daemon) , Microsoft Graph verileri bir kullanıcı adına değil uygulamanın kimliğini kullanarak eşitlediği BIR ASP.NET MVC web uygulamasıdır. Örnek, yönetici onay işlemini de gösterir.

    ![topology](media/scenario-daemon-app/damon-app-sample-web.svg)

# <a name="pythontabpython"></a>[Python](#tab/python)

Hızlı Başlangıç ['ı bir belirteç alma ve uygulama kimliğini kullanarak bir Python konsol uygulamasından MICROSOFT Graph API çağrısı](./quickstart-v2-python-daemon.md)yapın.

# <a name="javatabjava"></a>[Java](#tab/java)

MSAL Java Şu anda genel önizlemededir. Daha fazla bilgi için bkz. [msal Java dev Samples](https://github.com/AzureAD/microsoft-authentication-library-for-java/tree/dev/src/samples).

---
