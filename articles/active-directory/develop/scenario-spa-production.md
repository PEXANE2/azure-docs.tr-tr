---
title: Tek sayfalı uygulamayı üretime taşıyın
titleSuffix: Microsoft identity platform
description: Tek sayfalı bir uygulama oluşturmayı öğrenin (üretime geçin)
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 0c1e9e6da02478f5e4703676b74fc8247a4f619b
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98753520"
---
# <a name="single-page-application-move-to-production"></a>Tek sayfalı uygulama: üretime taşı

Artık, Web API 'Lerini çağırmak için bir belirteç edinmeyi öğrenmiş olduğunuza göre, üretime nasıl geçeceğinizi öğrenin.

## <a name="improve-your-app"></a>Uygulamanızı geliştirme

Uygulama üretim ortamınızı hazırlamak için [günlük kaydını etkinleştirin](msal-logging.md) .

## <a name="test-your-integration"></a>Tümleştirmenizi test etme

[Microsoft Identity platform tümleştirme denetim listesini](identity-platform-integration-checklist.md)izleyerek tümleştirmenizi test edin.

## <a name="deploy-your-app"></a>Uygulamanızı dağıtın

SPA ve Web API projelerinizi, sırasıyla Azure Storage ve Azure Uygulama Hizmetleri ile dağıtmayı öğrenmek için bir [dağıtım örneğine](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-aspnet-webapi-multitenant/tree/master/Chapter3) göz atın. 

## <a name="next-steps"></a>Sonraki adımlar

- Kullanıcı oturumu açma ve **MSAL.js**: [JavaScript Spa öğreticisini](./tutorial-v2-javascript-spa.md)kullanarak **Microsoft Graph API** 'sini çağırmak için bir erişim belirteci alma kodunu açıklayan hızlı başlangıç örneğinin derinlemesine bakış.

- [Bir ASP.net arka ucu ile](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-aspnetcore-webapi) **MSAL.js**: Spa kullanarak kendı arka uç web API 'niz (ASP.NET Core) için belirteçlerin nasıl alınacağını gösteren örnek.

- **Passport-Azure-AD**: [Node.js Web API (Azure AD](https://github.com/Azure-Samples/active-directory-javascript-nodejs-webapi-v2)) kullanarak arka uç web API 'niz (Node.js) için erişim belirteçlerinin nasıl doğrulandığını gösteren örnek.

- **Azure Active Directory B2C** (Azure AD B2C): [Spa Azure AD B2C](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)ile kaydedilmiş bir uygulamadaki kullanıcılara oturum açmak için **MSAL.js** nasıl kullanılacağını gösteren örnek.

- **Azure Active Directory B2C** (Azure AD B2C) ile kaydedilen uygulamalar için erişim belirteçlerini doğrulamak üzere **Passport-Azure-AD** ' i nasıl kullanacağınızı gösteren örnek: [Node.js Web API 'si (Azure AD B2C)](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi).
