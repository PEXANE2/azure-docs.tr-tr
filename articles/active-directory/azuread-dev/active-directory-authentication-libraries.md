---
title: Azure Etkin Dizin Kimlik Doğrulama Kitaplıkları | Microsoft Dokümanlar
description: Azure AD Kimlik Doğrulama Kitaplığı (ADAL), istemci uygulama geliştiricilerin kullanıcıları bulut veya şirket içi Active Directory (AD) için kolayca doğrulaması ve ardından API çağrılarını güvence altına almak için erişim belirteçleri edinmelerine olanak tanır.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.topic: conceptual
ms.workload: identity
ms.date: 12/01/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 32375c14d95dc9e100cbf496c8550f0a195a11dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154619"
---
# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory Kimlik Doğrulama Kitaplıkları

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Azure Active Directory Authentication Library (ADAL) v1.0, uygulama geliştiricilerin kullanıcıları bulut veya şirket içi Active Directory (AD) kimlik doğrulaması yapmalarını ve API çağrılarını güvence altına almak için belirteçler edinmelerini sağlar. ADAL, şu gibi özelliklerle geliştiriciler için kimlik doğrulamayı kolaylaştırır:

- Erişim belirteçlerini depolayan ve belirteçleri yenileyen yapılandırılabilir belirteç önbelleği
- Erişim belirteci süresi dolduğunda ve yenileme belirteci kullanılabilir olduğunda otomatik belirteç yenileme
- Eşzamanlı yöntem çağrıları için destek

> [!NOTE]
> Azure AD v2.0 kitaplıklarını (MSAL) mi arıyorsunuz? [Kullanıma al, MSAL kitaplık kılavuzu.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries)
>
>

## <a name="microsoft-supported-client-libraries"></a>Microsoft destekli İstemci Kitaplıkları

| Platform | Kitaplık | İndirme | Kaynak Kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET İstemci, Windows Mağazası, UWP, Xamarin iOS ve Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Masaüstü uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Başvuru](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory?view=azure-dotnet) |
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Tek sayfalı uygulama](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Başvuru](http://cocoadocs.org/docsets/ADAL/2.5.1/)|
| Android |ADAL |[Maven](https://search.maven.org/search?q=g:com.microsoft.aad+AND+a:adal&core=gav) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](https://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | [Node.js web uygulaması](https://github.com/Azure-Samples/active-directory-node-webapp-openidconnect)|[Başvuru](https://docs.microsoft.com/javascript/api/overview/azure/activedirectory) |
| Java |ADAL4J |[Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3Aadal4j%20g%3Acom.microsoft.azure) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java web uygulaması](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) |[Başvuru](https://javadoc.io/doc/com.microsoft.azure/adal4j) |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[Python web uygulaması](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi) |[Başvuru](https://adal-python.readthedocs.io/) |

## <a name="microsoft-supported-server-libraries"></a>Microsoft destekli Sunucu Kitaplıkları

| Platform | Kitaplık | İndirme | Kaynak Kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET |AzureAD için OWIN|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.ActiveDirectory) |[MVC Uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OpenIDConnect için OWIN |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.OpenIdConnect) |[Web App](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| .NET |WS-Federation için OWIN |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.WsFederation) |[MVC Web Uygulaması](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |.NET 4.5 için Kimlik Protokolü Uzantıları |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |JWT Işleyicisi için .NET 4.5 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |

## <a name="scenarios"></a>Senaryolar

Uzak bir kaynağa erişen bir istemcide ADAL kullanmak için üç yaygın senaryo aşağıda verilmiştir:

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Aygıtta çalışan yerel istemci uygulamasının kullanıcılarını doğrulama

Bu senaryoda, bir geliştiricinin web API gibi uzak bir kaynağa erişmesi gereken bir mobil istemcisi veya masaüstü uygulaması vardır. Web API'sı anonim aramalara izin vermez ve kimliği doğrulanmış bir kullanıcı bağlamında çağrılmalıdır. Web API' si, belirli bir Azure AD kiracısı tarafından verilen erişim belirteçlerine güvenecek şekilde önceden yapılandırılmıştır. Azure AD, söz konusu kaynak için erişim belirteçleri vermek üzere önceden yapılandırılmıştır. Geliştirici, istemciden web API'sını çağırmak için Azure AD ile kimlik doğrulamayı kolaylaştırmak için ADAL'ı kullanır. ADAL'ı kullanmanın en güvenli yolu, kullanıcı kimlik bilgilerini toplamak için kullanıcı arabirimini işlemesidir (tarayıcı penceresi olarak işlenir).

ADAL, kullanıcının kimliğini doğrulamayı, Azure AD'den bir erişim belirteci edinmeyi ve belirteci yenilemeyi ve ardından erişim belirtecikullanarak web API'sını aramayı kolaylaştırır.

Azure AD kimlik doğrulaması kullanarak bu senaryoyu gösteren bir kod örneği [için, Web API'ye Yerel İstemci WPF Uygulaması'na](https://github.com/azureadsamples/nativeclient-dotnet)bakın.

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Web sunucusunda çalışan gizli bir istemci uygulamasının doğrulanması

Bu senaryoda, bir geliştiricinin web API gibi uzak bir kaynağa erişmesi gereken bir sunucuda çalışan bir uygulaması vardır. Web API'sı anonim aramalara izin vermez, bu nedenle yetkili bir servisten çağrılması gerekir. Web API' si, belirli bir Azure AD kiracısı tarafından verilen erişim belirteçlerine güvenecek şekilde önceden yapılandırılmıştır. Azure AD, bu kaynak için istemci kimlik bilgileri (istemci kimliği ve gizli) olan bir hizmete erişim belirteçleri vermek üzere önceden yapılandırılmıştır. ADAL, Azure AD'nin web API'yi aramak için kullanılabilecek bir erişim belirteci döndürmesiyle hizmetin kimlik doğrulamasını kolaylaştırır. ADAL ayrıca, erişim belirtecinin kullanım ömrünü önbelleğe alarak ve gerektiği gibi yenileyerek yönetir. Bu senaryoyu gösteren bir kod örneği [için, Daemon konsolu Application to Web API'ye](https://github.com/AzureADSamples/Daemon-DotNet)bakın.

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Bir kullanıcı adına sunucuda çalışan gizli bir istemci uygulamasının doğrulanması

Bu senaryoda, bir geliştiricinin web API gibi uzak bir kaynağa erişmesi gereken bir sunucuda çalışan bir web uygulaması vardır. Web API'sı anonim aramalara izin vermez, bu nedenle kimliği doğrulanmış bir kullanıcı adına yetkili bir hizmetten çağrılmalıdır. Web API' sı belirli bir Azure AD kiracısı tarafından verilen erişim belirteçlerine güvenecek şekilde önceden yapılandırılmıştır ve Azure AD, söz konusu kaynağın erişim belirteçlerini istemci kimlik bilgilerine sahip bir hizmete vermek üzere önceden yapılandırılmıştır. Kullanıcının web uygulamasında kimliği doğrulandıktan sonra, uygulama Azure AD'den kullanıcı için bir yetkilendirme kodu alabilir. Web uygulaması daha sonra, Azure AD'den uygulamayla ilişkili yetkilendirme kodunu ve istemci kimlik bilgilerini kullanarak bir kullanıcı adına bir erişim jetonu almak ve belirteci yenilemek için ADAL'ı kullanabilir. Web uygulaması erişim belirteci ne zaman sahip, belirteç sona erene kadar web API çağırabilirsiniz. Belirteç süresi dolduğunda, web uygulaması daha önce alınan yenileme belirteci kullanarak yeni bir erişim belirteci almak için ADAL'ı kullanabilir. Bu senaryoyu gösteren bir kod örneği [için, Yerel istemciden Web API'sine ve Web API'sine](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof)bakın.

## <a name="see-also"></a>Ayrıca Bkz.

- [Azure Active Directory geliştirici kılavuzu](v1-overview.md)
- [Azure Etkin dizini için kimlik doğrulama senaryoları](v1-authentication-scenarios.md)
- [Azure Active Directory kod örnekleri](sample-v1-code.md)
