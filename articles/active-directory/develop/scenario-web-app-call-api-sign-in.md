---
title: Oturum kapatma sırasında belirteç önbelleğinden hesapları kaldırma-Microsoft Identity platform | Mavisi
description: Oturum kapatma sırasında bir hesabı belirteç önbelleğinden kaldırma hakkında bilgi edinin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: e138b3513b42dda47b0a114d866d657e18e3e393
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82181656"
---
# <a name="a-web-app-that-calls-web-apis-remove-accounts-from-the-token-cache-on-global-sign-out"></a>Web API 'Lerini çağıran bir Web uygulaması: genel oturum açma sırasında belirteç önbelleğinden hesapları kaldırma

Web uygulamasında oturum açma ve oturum kapatma için [Kullanıcı oturumunu açan](scenario-web-app-sign-user-sign-in.md)Web uygulamasına oturum açma eklemeyi öğrendiniz.

Oturum kapatma, Web API 'lerini çağıran bir Web uygulaması için farklıdır. Kullanıcı uygulamanızdan veya herhangi bir uygulamadan oturumu kapattığında, bu kullanıcıyla ilişkili belirteçleri belirteç önbelleğinden kaldırmanız gerekir.

## <a name="intercept-the-callback-after-single-sign-out"></a>Çoklu oturum kapatıldıktan sonra geri çağırma işlemini kesme

Oturum açan hesapla ilişkili belirteç önbelleği girişini temizlemek için, uygulamanız After olayını ele geçirebilir `logout` . Web Apps, bir belirteç önbelleğindeki her bir kullanıcı için erişim belirteçlerini depolar. `logout`Geri aramadan sonra, Web uygulamanız kullanıcıyı önbellekten kaldırabilir.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Microsoft. Identity. Web, oturum kapatma işlemini sizin yerinize uygulamayı gerçekleştirir.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET örneği genel oturum kapatma üzerindeki önbellekten hesapları kaldırmaz.

# <a name="java"></a>[Java](#tab/java)

Java örneği, genel oturum kapatma üzerindeki önbellekten hesapları kaldırmaz.

# <a name="python"></a>[Python](#tab/python)

Python örneği genel oturum kapatma üzerindeki önbellekten hesapları kaldırmaz.

---

## <a name="next-steps"></a>Sonraki adımlar

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Web uygulaması için belirteç alma](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnetcore)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Web uygulaması için belirteç alma](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnet)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Web uygulaması için belirteç alma](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=java)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Web uygulaması için belirteç alma](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=python)

---
