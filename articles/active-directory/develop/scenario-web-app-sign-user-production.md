---
title: Kullanıcılarda işaretleyen web uygulamasını üretime taşıyın - Microsoft kimlik platformu | Azure
description: Kullanıcılarda işaretleyen bir web uygulaması oluşturmayı öğrenin (üretime geçin)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 9c5fd444c55a20441325088912a07eb051219b84
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80881477"
---
# <a name="web-app-that-signs-in-users-move-to-production"></a>Kullanıcılarda işaretleyen web uygulaması: Üretime geç

Artık web API'lerini aramak için bir jetonu nasıl alacağınızı bildiğinize göre, nasıl üretime geçeceğinizi öğrenin.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Sonraki adımlar

### <a name="same-site"></a>Aynı site

Chrome tarayıcısının yeni sürümleriyle ilgili olası sorunları anladığınızdan emin olun

> [!div class="nextstepaction"]
> [Chrome tarayıcısında SameSite çerez değişiklikleri nasıl işleyebilir](howto-handle-samesite-cookie-changes-chrome-browser.md)

### <a name="scenario-for-calling-web-apis"></a>Web API'lerini arama senaryosu

Web uygulamanız kullanıcılarda oturum larını imzaladıktan sonra, oturum açmış kullanıcılar adına web API'lerini arayabilir. Web uygulamasından web API'lerini aramak aşağıdaki senaryonun nesnesidir:

> [!div class="nextstepaction"]
> [Web API'lerini çağıran web uygulaması](scenario-web-app-call-api-overview.md)

## <a name="deep-dive-aspnet-core-web-app-tutorial"></a>Derin dalış: ASP.NET Core web uygulaması öğretici

Bu ASP.NET Core öğreticisiyle kullanıcılarda oturum açmanın diğer yolları hakkında bilgi edinin: 

> [!div class="nextstepaction"]
> [Web uygulamalarınızın kullanıcılarda oturum açmasını ve geliştiriciler için Microsoft kimlik platformuyla API'leri aramasını sağlama](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) 

Bu aşamalı öğretici, bir web uygulaması için, hesaplarla oturum açma nın nasıl eklendirilebildiğini de içeren üretime hazır koda sahiptir:

- Kuruluşunuz
- Birden çok kuruluş
- İş veya okul hesapları veya kişisel Microsoft hesapları
- [Azure AD B2C](https://aka.ms/aadb2c)
- Ulusal bulutlar

## <a name="sample-code-java-web-app"></a>Örnek kod: Java web uygulaması

GitHub'daki bu örnekten Java web uygulaması hakkında daha fazla bilgi edinin: 

> [!div class="nextstepaction"]
> [Microsoft kimlik platformu ile kullanıcıları imzalayan ve Microsoft Graph çağıran bir Java Web uygulaması](https://github.com/Azure-Samples/ms-identity-java-webapp)
