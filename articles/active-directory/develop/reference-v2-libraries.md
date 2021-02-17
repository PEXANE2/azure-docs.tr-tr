---
title: Microsoft Identity platform kimlik doğrulama kitaplıkları | Mavisi
description: Microsoft Identity platform ile uyumlu istemci kitaplıkları ve ara yazılım listesi. Uygulamalarınıza Kullanıcı oturum açma (kimlik doğrulama) ve korumalı Web API erişimi (yetkilendirme) için destek eklemek üzere bu kitaplıkları kullanın.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: reference
ms.workload: identity
ms.date: 01/29/2021
ms.author: marsma
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: 228a15e9e9e27cbcfd71d4762db2f4ab9f6dfffe
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100560146"
---
# <a name="microsoft-identity-platform-authentication-libraries"></a>Microsoft Identity platform kimlik doğrulama kitaplıkları

Aşağıdaki tablolarda çeşitli uygulama türleri için Microsoft kimlik doğrulama kitaplığı desteği gösterilmektedir. Bunlar kitaplık kaynak koduna bağlantılar, örneğin, uygulamanızın projesi için paketin alınacağı ve kitaplığın Kullanıcı oturum açma (kimlik doğrulama), korumalı Web API 'Lerine (yetkilendirme) veya her ikisine de destekleyip desteklemediğini içerir.

Microsoft Identity platformu, OpenID Foundation tarafından [sertifikalı bir OpenID sağlayıcısı](https://openid.net/certification/)olarak onaylandı. Microsoft kimlik doğrulama kitaplığı (MSAL) veya Microsoft tarafından desteklenen başka bir kitaplık dışında bir kitaplık kullanmayı tercih ederseniz, [sertifikalı bir OpenID Connect uygulamasıyla](https://openid.net/developers/certified/)bir tane seçin.

[OAuth 2,0 veya OpenID Connect 1,0](active-directory-v2-protocols.md)' nin protokol düzeyi uygulamanızı tek başına kodlayabilirsiniz, her bir standart belirtimindeki güvenlik konularına yakın dikkat edin ve [Microsoft SDL][Microsoft-SDL]gibi bir yazılım GELIŞTIRME yaşam döngüsü (SDL) metodolojisini izleyin.

## <a name="single-page-application-spa"></a>Tek sayfalı uygulama (SPA)

Tek sayfalı bir uygulama tamamen tarayıcı yüzeyine çalışır ve sayfa verilerini (HTML, CSS ve JavaScript) dinamik olarak veya uygulama yükleme zamanında getirir. Arka uç veri kaynaklarıyla etkileşim kurmak için Web API 'Lerini çağırabilir.

SPA 'nın kodu tamamen tarayıcıda çalıştığından, gizli dizileri güvenli bir şekilde depolayamadığı *ortak bir istemci* olarak kabul edilir.

| Dil/çerçeve | Proje açık<br/>GitHub                                                                                                    | Paket                                                                      | Almanızı<br/>başlama                             | Oturum açma kullanıcıları                                         | Web API 'Lerine erişin                                                 | Genel olarak kullanılabilir (GA) *veya*<br/>Genel Önizleme<sup>1</sup> |
|----------------------|--------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|:-----------------------------------------------:|:-----------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| Angular              | [MSAL angular 2,0](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)         | [@azure/msal-angular](https://www.npmjs.com/package/@azure/msal-angular)     | —                                               | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | Genel Önizleme                                               |
| Angular              | [MSAL angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/msal-angular-v1/lib/msal-angular) | [@azure/msal-angular](https://www.npmjs.com/package/@azure/msal-angular)     | [Öğretici](tutorial-v2-angular.md)              | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
| AngularJS            | [MSAL AngularJS](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angularjs)         | [@azure/msal-angularjs](https://www.npmjs.com/package/@azure/msal-angularjs) | —                                               | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | Genel Önizleme                                               |
| JavaScript           | [MSAL.js 2,0](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser)              | [@azure/msal-browser](https://www.npmjs.com/package/@azure/msal-browser)     | [Öğretici](tutorial-v2-javascript-auth-code.md) | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
| JavaScript           | [MSAL.js 1,0](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-core)              | [@azure/msal-core](https://www.npmjs.com/package/@azure/msal-core)     | [Öğretici](tutorial-v2-javascript-spa.md) | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
| React                | [MSAL tepki verme](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-react)                 | [@azure/msal-react](https://www.npmjs.com/package/@azure/msal-react)         | —                                               | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | Genel Önizleme                                               |
<!--
| Vue | [Vue MSAL]( https://github.com/mvertopoulos/vue-msal) | [vue-msal]( https://www.npmjs.com/package/vue-msal) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Microsoft Azure önizlemeleri için ek kullanım koşulları,][preview-tos] kitaplıklar için *genel önizlemede* geçerlidir.

## <a name="web-application"></a>Web uygulaması

Web uygulaması, bir kullanıcının Web tarayıcısına işlenmesi için HTML, CSS ve JavaScript üreten ve gönderen bir sunucuda kodu çalıştırır. Kullanıcının kimliği, kullanıcının tarayıcısı (ön uç) ve Web sunucusu (arka uç) arasında bir oturum olarak tutulur.

Bir Web uygulamasının kodu Web sunucusunda çalıştığı için gizli dizileri güvenli bir şekilde depolayabilen *gizli bir istemci* olarak kabul edilir.

| Dil/çerçeve | Proje açık<br/>GitHub                                                                                     | Paket                                                                                                    | Almanızı<br/>başlama                               | Oturum açma kullanıcıları                                            | Web API 'Lerine erişin                                                    | Genel olarak kullanılabilir (GA) *veya*<br/>Genel Önizleme<sup>1</sup> |
|----------------------|-----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|:-------------------------------------------------:|:--------------------------------------------------------:|:------------------------------------------------------------------:|:------------------------------------------------------------:|
| .NET                 | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)                        | [Microsoft. Identity. Client](https://www.nuget.org/packages/Microsoft.Identity.Client)                      | —                                                 | ![Kitaplık, Kullanıcı oturumu açma için KIMLIK belirteçleri isteyemezsiniz.][n] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y]    | GA                                                           |
| ASP.NET Core         | [ASP.NET güvenliği](/aspnet/core/security/)                                                                | [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) | —                                                 | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y]    | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyemedi.][n] | GA                                                           |
| ASP.NET Core         | [Microsoft. Identity. Web](https://github.com/AzureAD/microsoft-identity-web)                               | [Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web)                            | —                                                 | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y]    | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y]    | GA                                                           |
| Java                 | [MSAL4J](https://github.com/AzureAD/microsoft-authentication-library-for-java)                            | [msal4j](https://search.maven.org/artifact/com.microsoft.azure/msal4j)                                     | [Hızlı Başlangıç](quickstart-v2-java-webapp.md)        | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y]    | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y]    | GA                                                           |
| Node.js              | [MSAL düğümü](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) | [msal-Node](https://www.npmjs.com/package/@azure/msal-node)                                                | [Hızlı Başlangıç](quickstart-v2-nodejs-webapp-msal.md) | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y]    | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y]    | GA                                               |
| Node.js              | [Azure AD Passport](https://github.com/AzureAD/passport-azure-ad)                                         | [Passport-Azure-AD](https://www.npmjs.com/package/passport-azure-ad)                                       | [Hızlı Başlangıç](quickstart-v2-nodejs-webapp.md)      | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y]    | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
| Python               | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)                     | [msal](https://pypi.org/project/msal)                                                                      | [Hızlı Başlangıç](quickstart-v2-python-webapp.md)      | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y]    | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y]    | GA                                                           |
<!--
| Java | [ScribeJava](https://github.com/scribejava/scribejava) | [ScribeJava 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | ![X indicating no.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
| Java | [Gluu oxAuth](https://github.com/GluuFederation/oxAuth) | [oxAuth 3.0.2](https://github.com/GluuFederation/oxAuth/releases/tag/3.0.2) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
| Node.js | [openid-client](https://github.com/panva/node-openid-client/) | [openid-client 2.4.5](https://github.com/panva/node-openid-client/releases/tag/v2.4.5) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
| PHP | [PHP League oauth2-client](https://github.com/thephpleague/oauth2-client) | [oauth2-client 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | ![X indicating no.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
| Ruby | [OmniAuth](https://github.com/omniauth/omniauth) | [omniauth 1.3.1](https://github.com/omniauth/omniauth/releases/tag/v1.3.1)<br/>[omniauth-oauth2 1.4.0](https://github.com/intridea/omniauth-oauth2) | ![X indicating no.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Microsoft Azure önizlemeleri için ek kullanım koşulları,][preview-tos] kitaplıklar için *genel önizlemede* geçerlidir.

## <a name="desktop-application"></a>Masaüstü uygulaması

Masaüstü uygulaması, genellikle bir kullanıcı arabirimini sunan ve kullanıcının masaüstünde çalışması amaçlanan ikili (derlenmiş) koddur.

Bir masaüstü uygulaması kullanıcının masaüstünde çalıştığından, gizli dizileri güvenli bir şekilde depolayamadığı *ortak bir istemci* olarak kabul edilir.

| Dil/çerçeve | Proje açık<br/>GitHub                                                                                     | Paket                                                                               | Almanızı<br/>başlama                        | Oturum açma kullanıcıları                                         | Web API 'Lerine erişin                                                 | Genel olarak kullanılabilir (GA) *veya*<br/>Genel Önizleme<sup>1</sup> |
|----------------------|-----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|:------------------------------------------:|:-----------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| Tron             | [MSAL düğümü](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) | [@azure/msal-node](https://www.npmjs.com/package/@azure/msal-node)                    | [Öğretici](tutorial-v2-nodejs-desktop.md)   | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                               |
| Java                 | [MSAL4J](https://github.com/AzureAD/microsoft-authentication-library-for-java)                            | [msal4j](https://mvnrepository.com/artifact/com.microsoft.azure/msal4j)               | —                                          | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
| macOS (Swift/obj-C)  | [iOS ve macOS için MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-objc)            | [MSAL](https://cocoapods.org/pods/MSAL)                                               | [Öğretici](tutorial-v2-ios.md)             | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
| UWP                  | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)                        | [Microsoft. Identity. Client](https://www.nuget.org/packages/Microsoft.Identity.Client) | [Öğretici](tutorial-v2-windows-uwp.md)     | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
| WPF                  | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)                        | [Microsoft. Identity. Client](https://www.nuget.org/packages/Microsoft.Identity.Client) | [Öğretici](tutorial-v2-windows-desktop.md) | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
<!--
| Java | Scribe | [Scribe Java](https://mvnrepository.com/artifact/org.scribe/scribe) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
| React Native | [React Native App Auth](https://github.com/FormidableLabs/react-native-app-auth/blob/main/docs/config-examples/azure-active-directory.md) | [react-native-app-auth](https://www.npmjs.com/package/react-native-app-auth) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Microsoft Azure önizlemeleri için ek kullanım koşulları,][preview-tos] kitaplıklar için *genel önizlemede* geçerlidir.

## <a name="mobile-application"></a>Mobil uygulama

Bir mobil uygulama, genellikle bir kullanıcı arabirimini sunan ve kullanıcının mobil cihazında çalıştırılması amaçlanan ikili (derlenmiş) koddur.

Bir mobil uygulama kullanıcının mobil cihazında çalıştığından, gizli dizileri güvenli bir şekilde depolayamadığı *ortak bir istemci* olarak kabul edilir.

| Platform          | Proje açık<br/>GitHub                                                                          | Paket                                                                               | Almanızı<br/>başlama                    | Oturum açma kullanıcıları                                         | Web API 'Lerine erişin                                                 | Genel olarak kullanılabilir (GA) *veya*<br/>Genel Önizleme<sup>1</sup> |
|-------------------|------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|:--------------------------------------:|:-----------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| Android (Java)    | [MSAL Android](https://github.com/AzureAD/microsoft-authentication-library-for-android)        | [MSAL](https://mvnrepository.com/artifact/com.microsoft.identity.client/msal)         | [Hızlı Başlangıç](quickstart-v2-android.md) | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
| Android (Kotlin)  | [MSAL Android](https://github.com/AzureAD/microsoft-authentication-library-for-android)        | [MSAL](https://mvnrepository.com/artifact/com.microsoft.identity.client/msal)         | —                                      | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
| iOS (Swift/obj-C) | [iOS ve macOS için MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [MSAL](https://cocoapods.org/pods/MSAL)                                               | [Öğretici](tutorial-v2-ios.md)         | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
| Xamarin (.NET)    | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)             | [Microsoft. Identity. Client](https://www.nuget.org/packages/Microsoft.Identity.Client) | —                                      | ![Kitaplık, Kullanıcı oturum açma için KIMLIK belirteçleri isteyebilir.][y] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
<!--
| React Native |[React Native App Auth](https://github.com/FormidableLabs/react-native-app-auth/blob/main/docs/config-examples/azure-active-directory.md) | [react-native-app-auth](https://www.npmjs.com/package/react-native-app-auth) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Microsoft Azure önizlemeleri için ek kullanım koşulları,][preview-tos] kitaplıklar için *genel önizlemede* geçerlidir.

## <a name="service--daemon"></a>Hizmet/Daemon

Hizmetler ve Daemon 'ları, genellikle sunucudan sunucuya ve diğer katılımsız (bazen *gözetimsiz* olarak adlandırılır) iletişim için kullanılır. Klavyede kimlik bilgileri veya kaynak erişimine onay girmeye yönelik bir Kullanıcı bulunmadığından, bu uygulamalar, bir Web API 'sinin kaynaklarına yetkili erişim isteğinde bulunduğunda Kullanıcı değil, kendileri olarak kimlik doğrular.

Sunucuda çalışan bir hizmet veya Daemon, gizli dizilerini güvenli bir şekilde depolayabilen bir *Gizli istemci* olarak kabul edilir.

| Dil/çerçeve | Proje açık<br/>GitHub                                                                 | Paket                                                                                | Almanızı<br/>başlama                           | Oturum açma kullanıcıları                                            | Web API 'Lerine erişin                                                 | Genel olarak kullanılabilir (GA) *veya*<br/>Genel Önizleme<sup>1</sup> |
|----------------------|---------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|:---------------------------------------------:|:--------------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| .NET                 | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)    | [Microsoft. Identity. Client](https://www.nuget.org/packages/Microsoft.Identity.Client/) | [Hızlı Başlangıç](quickstart-v2-netcore-daemon.md) | ![Kitaplık, Kullanıcı oturumu açma için KIMLIK belirteçleri isteyemezsiniz.][n] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
| Java                 | [MSAL4J](https://github.com/AzureAD/microsoft-authentication-library-for-java)        | [msal4j](https://javadoc.io/doc/com.microsoft.azure/msal4j/latest/index.html)          | —                                             | ![Kitaplık, Kullanıcı oturumu açma için KIMLIK belirteçleri isteyemezsiniz.][n] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA                                                           |
| Düğüm               | [MSAL düğümü](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) | [msal-Node](https://www.npmjs.com/package/@azure/msal-node)  | [Hızlı Başlangıç](quickstart-v2-nodejs-console.md)  | ![Kitaplık, Kullanıcı oturumu açma için KIMLIK belirteçleri isteyemezsiniz.][n] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA  |
| Python               | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python) | [msal-Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)  | —  | ![Kitaplık, Kullanıcı oturumu açma için KIMLIK belirteçleri isteyemezsiniz.][n] | ![Kitaplık, korumalı Web API 'Leri için erişim belirteçleri isteyebilir.][y] | GA |
<!--
|PHP| [The PHP League oauth2-client](https://oauth2-client.thephpleague.com/usage/) | [League\OAuth2](https://oauth2-client.thephpleague.com/) | ![Green check mark.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Microsoft Azure önizlemeleri için ek kullanım koşulları,][preview-tos] kitaplıklar için *genel önizlemede* geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft kimlik doğrulama Kitaplığı hakkında daha fazla bilgi için bkz. [Microsoft kimlik doğrulama kitaplığı 'Na genel bakış (msal)](msal-overview.md).

<!--Image references-->
[y]: ./media/common/yes.png
[n]: ./media/common/no.png

<!--Reference-style links -->
[AAD-App-Model-V2-Overview]: v2-overview.md
[Microsoft-SDL]: https://www.microsoft.com/securityengineering/sdl/
[preview-tos]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
