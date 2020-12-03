---
title: Azure Active Directory B2C kod örnekleri | Microsoft Docs
description: Azure Active Directory B2C mobil, masaüstü, web ve tek sayfalı uygulamaları için kod örnekleri.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/02/2020
ms.custom: mvc
ms.topic: sample
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 8642f4284770df54704a4e7066054d3bb4640486
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96545884"
---
# <a name="azure-active-directory-b2c-code-samples"></a>Azure Active Directory B2C kod örnekleri

Aşağıdaki tablolarda iOS, Android, .NET ve Node.js de dahil olmak üzere uygulamalar için örneklerin bağlantıları verilmektedir.

## <a name="mobile-and-desktop-apps"></a>Mobil uygulamalar ve masaüstü uygulamaları

| Örnek | Açıklama |
|--------| ----------- |
| [ios-swift-native-msal](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal) | Swift’te, Azure AD B2C kullanıcılarının kimliğini doğrulayan ve OAuth 2.0 kullanarak bir API’yi çağıran iOS örneği |
| [android-native-msal](https://github.com/Azure-Samples/ms-identity-android-java#b2cmodefragment-class) | Azure Active Directory B2C ile kullanıcıların kimliklerinin nasıl doğrulanacağını ve sonuç belirteçleriyle bir Web API’sine nasıl erişileceğini gösteren basit bir Android uygulaması. |
| [ios-native-appauth](https://github.com/Azure-Samples/active-directory-b2c-ios-native-appauth) | Objective C'de, Azure AD B2C kimlik hizmetimiz için Microsoft Identity kullanıcılarının kimliğini doğrulayan bir iOS uygulaması oluşturmak amacıyla üçüncü taraf bir kitaplığı nasıl kullanabileceğinizi gösteren bir örnek. |
| [android-native-appauth](https://github.com/Azure-Samples/active-directory-b2c-android-native-appauth) | B2C kimlik hizmetimiz için Microsoft Identity kullanıcılarının kimliğini doğrulayan ve OAuth 2.0 erişim belirteçlerini kullanarak web API’si çağıran bir Android uygulaması oluşturmak amacıyla üçüncü- taraf kitaplığı nasıl kullanabileceğinizi gösteren bir örnek. |
| [dotnet-desktop](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) | Windows Masaüstü .NET (WPF) uygulamasının Azure AD B2C’yi kullanarak bir kullanıcının oturumunu nasıl açabildiğini, MSAL.NET kullanarak bir erişim belirtecini nasıl alabildiğini ve bir API’yi nasıl çağırabildiğini gösteren bir örnek. |
| [xamarin-native](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) | Azure Active Directory B2C aracılığıyla kullanıcıların kimlik doğrulaması için MSAL’nin nasıl kullanılacağını ve sonuç belirteçleriyle bir Web API’sine nasıl erişilebileceğini gösteren basit bir Xamarin Forms uygulaması. |

## <a name="web-apps-and-apis"></a>Web uygulamaları ve API'leri

| Örnek | Açıklama |
|--------| ----------- |
| [dotnet-webapp-and-webapi](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) | .NET Web API'si çağıran bir .NET web uygulaması için, her ikisi de Azure AD B2C kullanılarak güvende tutulan birleştirilmiş bir örnek. |
| [dotnetcore-WebApp-openıdconnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-5-B2C) | Azure AD B2C kullanıcıları oturum açmak için OpenID Connect kullanan ASP.NET Core bir Web uygulaması. |
| [dotnetcore-WebApp-msal-API](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/4-WebApp-your-API/4-2-B2C) | Azure AD B2C’yi kullanarak bir kullanıcının oturumunu açabilen, MSAL.NET kullanarak bir erişim belirteci alabilen ve bir API’yi çağırabilen ASP.NET Core web uygulaması. |
| [openidconnect-nodejs](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS) | OpenID Connect’i kullanarak Express ile Web uygulaması ayarlamanın hızlı ve kolay bir yolunu sağlayan Node.js uygulaması. |
| [javascript-nodejs-webapi](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) | Azure AD B2C için, Web API’nizin nasıl korunacağını ve passport.js kullanılarak B2C erişim belirteçlerinin nasıl kabul edileceğini gösteren küçük bir node.js Web API’si. |
| [MS-Identity-Python-WebApp](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/master/README_B2C.md) | Microsoft Identity platform B2C 'yi bir Python web uygulamasıyla tümleştirmeyi gösterir.  |

## <a name="single-page-apps"></a>Tek sayfa uygulamalar

| Örnek | Açıklama |
|--------| ----------- |
| [MS-Identity-B2C-JavaScript-Spa](https://github.com/Azure-Samples/ms-identity-b2c-javascript-spa) | Bir Web API’sini çağıran tek sayfalı uygulama (SPA). Azure AD B2C, MSAL.js kullanılarak kimlik doğrulaması gerçekleştirilir. Bu örnek, PKI CE ile yetkilendirme kodu akışını kullanır. |
| [javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) | Bir Web API’sini çağıran tek sayfalı uygulama (SPA). Azure AD B2C, MSAL.js kullanılarak kimlik doğrulaması gerçekleştirilir. Bu örnekler örtük akışı kullanır.|

## <a name="saml-test-application"></a>SAML test uygulaması

| Örnek | Açıklama |
|--------| ----------- |
| [SAML-SP-sınayıcı](https://github.com/azure-ad-b2c/saml-sp-tester/tree/master/source-code) | SAML kimlik sağlayıcısı olarak davranacak şekilde Azure AD B2C test edilecek SAML test uygulaması. |

## <a name="api-connectors"></a>API bağlayıcıları

Aşağıdaki tablolar, [API bağlayıcıları](api-connectors-overview.md)kullanarak Kullanıcı akışlarınızdaki Web API 'lerini kullanmak için kod örneklerine bağlantılar sağlar.

### <a name="azure-function-quickstarts"></a>Azure Işlevi hızlı başlangıç

| Örnek                                                                                                                          | Açıklama                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [.NET Core](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-api-connector-azure-function-validate) | Bu .NET Core Azure Işlevi örneği, oturum açma işlemlerinin belirli e-posta etki alanlarına nasıl sınırlandırılacağını ve Kullanıcı tarafından belirtilen bilgilerin nasıl doğrulandığını gösterir. |
| [Node.js](https://github.com/Azure-Samples/active-directory-nodejs-external-identities-api-connector-azure-function-validate)   | Bu Node.js Azure Işlevi örneği, oturum açma işlemlerinin belirli e-posta etki alanlarına nasıl sınırlandırılacağını ve Kullanıcı tarafından belirtilen bilgilerin nasıl doğrulandığını gösterir.  |
| [Python](https://github.com/Azure-Samples/active-directory-python-external-identities-api-connector-azure-function-validate)    | Bu Python Azure Işlevi örneği, oturum açma işlemlerinin belirli e-posta etki alanlarına nasıl sınırlandırılacağını ve Kullanıcı tarafından belirtilen bilgilerin nasıl doğrulandığını gösterir.    |


### <a name="automated-fraud-protection-services--captcha"></a>Otomatik sahtekarlık koruma hizmetleri & CAPTCHA
| Örnek                                                                                                            | Açıklama                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| [Arkoo Labs sahtekarlık ve uygunsuz kullanım koruması](https://github.com/Azure-Samples/active-directory-b2c-node-sign-up-user-flow-arkose) | Bu örnekte, Arkoo Labs sahtekarlık ve uygunsuz kullanım koruma hizmeti kullanılarak Kullanıcı oturum açma işlemlerinin nasıl korunacağı gösterilmektedir. |
| [reCAPTCHA](https://github.com/Azure-Samples/active-directory-b2c-node-sign-up-user-flow-captcha) | Bu örnek, otomatik kötüye kullanımı engellemek için reCAPTCHA Challenge kullanarak Kullanıcı oturum açma bilgilerinizi nasıl koruyabileceğinizi gösterir. |


### <a name="identity-verification"></a>Kimlik doğrulama

| Örnek                                                                                                            | Açıklama                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| [IDology](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-idology-identity-verification) | Bu örnek, IDology ile tümleştirme için bir API Bağlayıcısı kullanarak, kaydolma akışlarınızın bir parçası olarak bir kullanıcı kimliğini nasıl doğrulayacağınızı gösterir. |
| [Experian](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-experian-identity-verification) | Bu örnek, Experian ile tümleştirme için bir API Bağlayıcısı kullanarak, kaydolma akışlarınızın bir parçası olarak bir kullanıcı kimliğini nasıl doğrulayacağınızı gösterir. |


### <a name="other"></a>Diğer

| Örnek                                                                                                            | Açıklama                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| [Davet kodu](https://github.com/Azure-Samples/active-directory-b2c-node-sign-up-user-flow-invitation-code) | Bu örnekte, davet kodları kullanılarak nasıl kayıt yapılacağını belirli kitlelere kısıtlama yapılacağı gösterilmektedir.|
| [API Bağlayıcısı topluluk örnekleri](https://github.com/azure-ad-b2c/api-connector-samples) | Bu depoda, API bağlayıcıları tarafından etkinleştirilen senaryoların topluluk tarafından korunan örnekleri bulunur.|
