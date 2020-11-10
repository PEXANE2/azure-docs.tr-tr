---
title: Tek sayfalı uygulamayı üretime taşıma-Microsoft Identity platform | Mavisi
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
ms.openlocfilehash: 0a51442870fb72e2b3cd93d9f03736d2c679ed06
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94442829"
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

- Kullanıcı oturumu açma ve **MSAL.js** : [JavaScript Spa öğreticisini](./tutorial-v2-javascript-spa.md)kullanarak **Microsoft Graph API** 'sini çağırmak için bir erişim belirteci alma kodunu açıklayan hızlı başlangıç örneğinin derinlemesine bakış.

- [Bir ASP.net arka ucu ile](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-aspnetcore-webapi) **MSAL.js** : Spa kullanarak kendı arka uç web API 'niz (ASP.NET Core) için belirteçlerin nasıl alınacağını gösteren örnek.

- **Passport-Azure-AD** : [Node.js Web API (Azure AD](https://github.com/Azure-Samples/active-directory-javascript-nodejs-webapi-v2)) kullanarak arka uç web API 'niz (Node.js) için erişim belirteçlerinin nasıl doğrulandığını gösteren örnek.

- **Azure Active Directory B2C** (Azure AD B2C): [Spa Azure AD B2C](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)ile kaydedilmiş bir uygulamadaki kullanıcılara oturum açmak için **MSAL.js** nasıl kullanılacağını gösteren örnek.

- **Azure Active Directory B2C** (Azure AD B2C) ile kaydedilen uygulamalar için erişim belirteçlerini doğrulamak üzere **Passport-Azure-AD** ' i nasıl kullanacağınızı gösteren örnek: [Node.js Web API 'si (Azure AD B2C)](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi).
