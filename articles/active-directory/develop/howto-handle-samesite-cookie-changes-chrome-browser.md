---
title: Chrome tarayıcısında SameSite çerez değişiklikleri nasıl ele | Azure
titleSuffix: Microsoft identity platform
description: Chrome tarayıcısında SameSite çerez değişiklikleri nasıl ele öğrenin.
services: active-directory
documentationcenter: ''
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: jmprieur
ms.reviewer: kkrishna
ms.custom: aaddev
ms.openlocfilehash: 056b787bbbcde6ba7f9510043deabdcf85ac7467
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050533"
---
# <a name="handle-samesite-cookie-changes-in-chrome-browser"></a>Chrome tarayıcısında SameSite tanımlama bilgisi değişikliklerini işleme

## <a name="what-is-samesite"></a>SameSite nedir?

`SameSite`web uygulamalarında Site Arası İstek Sahteciliği (CSRF) saldırılarını önlemek için HTTP tanımlama bilgilerinde ayarlanabilen bir özelliktir:

- Lax `SameSite` olarak **Lax**ayarlandığında, çerez aynı site içinde istekler ve diğer sitelerden GET istekleri gönderilir. Get isteklerinde çapraz etki alanı olan gönderilmez.
- **Strict** değeri, çerezin yalnızca aynı site içinde istekler halinde gönderilmesini sağlar.

Varsayılan olarak, `SameSite` değer tarayıcılarda ayarlanmaz ve bu nedenle isteklerde gönderilen tanımlama bilgilerinde herhangi bir kısıtlama yoktur. Bir uygulama, gereksinimlerine göre **Lax** veya **Strict** ayarlayarak CSRF korumasını kabul etmek gerekir.

## <a name="samesite-changes-and-impact-on-authentication"></a>SameSite değişiklikleri ve kimlik doğrulama üzerindeki etkisi

[SameSite'deki standartlara](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) yapılan son güncellemeler, hiçbir `SameSite` değer Lax olarak ayarlandığında varsayılan davranışı yaparak uygulamaları korumayı önerir. Bu azaltma, çerezlerin diğer sitelerden GET dışında HTTP isteklerinde kısıtlanacağı anlamına gelir. Ayrıca, gönderilen tanımlama bilgileriyle ilgili kısıtlamaları kaldırmak için **Hiçbiri** değeri getirilir. Bu güncellemeler yakında Chrome tarayıcısının yaklaşan bir sürümünde yayımlanacaktır.

Web uygulamaları "form_post" yanıt modunu kullanarak Microsoft Identity platformunda kimlik doğruladığında, giriş sunucusu belirteçleri veya auth kodunu göndermek için bir HTTP POST kullanarak uygulamaya yanıt verir. Bu istek bir etki alanı arası `login.microsoftonline.com` istek olduğundan (örneğin, `https://contoso.com/auth`etki alanınızdan), uygulamanız tarafından ayarlanan tanımlama bilgileri artık Chrome'daki yeni kurallara girer. Siteler arası senaryolarda kullanılması gereken tanımlama bilgileri, giriş isteğinde de gönderilen *durum* ve *nonce* değerlerini tutan tanımlama bilgileridir. Oturumu tutmak için Azure AD tarafından bırakılan başka tanımlama bilgileri de vardır.

Web uygulamalarınızı güncellemezseniz, bu yeni davranış kimlik doğrulama hatalarına neden olur.

## <a name="mitigation-and-samples"></a>Azaltma ve numuneler

Kimlik doğrulama hatalarını aşmak için, Microsoft kimlik platformuyla kimlik `SameSite` doğrulaması `None` yapan web uygulamaları, özelliği Chrome tarayıcısında çalışırken etki alanı arası senaryolarda kullanılan tanımlama bilgileriiçin ayarlayabilir.
Diğer tarayıcılar (tam bir liste için [buraya](https://www.chromium.org/updates/same-site/incompatible-clients) `SameSite` bakın) önceki davranışını `SameSite=None` izler ve ayarlanmışsa çerezleri içermez.
Bu nedenle, birden çok tarayıcıda kimlik doğrulamayı desteklemek için `SameSite` web `None` uygulamalarının değeri yalnızca Chrome'a ayarlaması ve değeri diğer tarayıcılarda boş bırakması gerekir.

Bu yaklaşım aşağıdaki kod örneklerimizde gösterilmiştir.

# <a name="net"></a>[.NET](#tab/dotnet)

Aşağıdaki tabloda, ASP.NET ve ASP.NET Core örneklerimizde Aynı Site değişiklikleri etrafında çalışan çekme istekleri ni ASP.NET.

| Örnek | Çekme isteği |
| ------ | ------------ |
|  [ASP.NET Core Web App artımlı öğretici](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)  |  [Aynı site çerez düzeltme #261](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/pull/261)  |
|  [ASP.NET MVC Web Uygulaması örneği](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect)  |  [Aynı site çerez düzeltme #35](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/pull/35)  |
|  [active-directory-dotnet-admin-sınırlı kapsamlar-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2)  |  [Aynı site çerez düzeltme #28](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2/pull/28)  |

ASP.NET ve ASP.NET Core'da SameSite çerezlerinin nasıl işleyeceğiniz hakkında ayrıntılı bilgi için ayrıca bkz:

- [ASP.NET Core'da SameSite çerezleri ile çalışın.](https://docs.microsoft.com/aspnet/core/security/samesite)
- [SameSite sayısında ASP.NET blog](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)

# <a name="python"></a>[Python](#tab/python)

| Örnek |
| ------ |
|  [ms-kimlik-python-webapp](https://github.com/Azure-Samples/ms-identity-python-webapp)  |

# <a name="java"></a>[Java](#tab/java)

| Örnek | Çekme isteği |
| ------ | ------------ |
|  [ms-kimlik-java-webapp](https://github.com/Azure-Samples/ms-identity-java-webapp)  | [Aynı site çerez düzeltme #24](https://github.com/Azure-Samples/ms-identity-java-webapp/pull/24)
|  [ms-kimlik-java-webapi](https://github.com/Azure-Samples/ms-identity-java-webapi)  | [Aynı site çerez düzeltme #4](https://github.com/Azure-Samples/ms-identity-java-webapi/pull/4)

---

## <a name="next-steps"></a>Sonraki adımlar

SameSite ve Web uygulaması senaryosu hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Google Chrome'un Aynı Sitedeki SSS'si](https://www.chromium.org/updates/same-site/faq)

> [!div class="nextstepaction"]
> [Krom SameSite sayfası](https://www.chromium.org/updates/same-site)

> [!div class="nextstepaction"]
> [Senaryo: Kullanıcılarda işaretleyen Web uygulaması](scenario-web-app-sign-user-overview.md)