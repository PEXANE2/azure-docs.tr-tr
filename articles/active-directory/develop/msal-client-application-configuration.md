---
title: İstemci uygulama yapılandırması (Microsoft kimlik doğrulama kitaplığı) | Mavisi
description: Microsoft kimlik doğrulama kitaplığı 'nda (MSAL) ortak istemci ve gizli istemci uygulamaları için yapılandırma seçenekleri hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 767f7362a6c46d864ba17f23f6506bf6cdb71414
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68304723"
---
# <a name="application-configuration-options"></a>Uygulama yapılandırma seçenekleri

Kodunuzda, belirteçlerin kimliğini doğrulamak ve almak için yeni bir genel veya gizli istemci uygulaması (veya MSAL. js için Kullanıcı Aracısı) başlatabilirsiniz. İstemci uygulamasını Microsoft kimlik doğrulama kitaplığı 'nda (MSAL) başlatırken bir dizi yapılandırma seçeneği belirleyebilirsiniz. Bu seçenekler iki gruba ayrılır:

- Kayıt seçenekleri, şunlar dahildir:
    - [Yetkili](#authority) (kimlik sağlayıcı [örneğinden](#cloud-instance) ve uygulama için oturum açma kitme [](#application-audience) ve muhtemelen Kiracı kimliği) oluşur.
    - [ISTEMCI kimliği](#client-id).
    - [Yeniden yönlendirme URI 'si](#redirect-uri).
    - [İstemci parolası](#client-secret) (gizli istemci uygulamaları için).
- Günlük düzeyi, kişisel verilerin denetimi ve kitaplığı kullanarak bileşen adı dahil olmak üzere [günlüğe kaydetme seçenekleri](#logging).

## <a name="authority"></a>İniz
Yetkili, MSAL tarafından belirteçleri isteyebileceğini bir dizini gösteren bir URL 'dir. Ortak yetkililer şunlardır:

- https\://Login.microsoftonline.com/\<kiracısı\>/, &lt;kiracı&gt; , Azure Active Directory (Azure AD) kiracısı veya bu Azure AD kiracısıyla ilişkili bir etki alanı olan kiracı kimliğidir. Yalnızca belirli bir kuruluşun kullanıcılarına oturum açmak için kullanılır.
- https\://Login.microsoftonline.com/Common/. Kullanıcıları iş ve okul hesaplarıyla veya kişisel Microsoft hesaplarıyla oturum açmak için kullanılır.
- https\://Login.microsoftonline.com/Organizations/. Kullanıcılara iş ve okul hesaplarıyla oturum açmak için kullanılır.
- https\://Login.microsoftonline.com/consumers/. Kullanıcılara yalnızca kişisel Microsoft hesaplarıyla (eski adıyla Windows Live ID hesapları olarak biliniyordu) oturum açmak için kullanılır.

Yetkili ayarının, uygulama kayıt portalında bildirildiği verilerle tutarlı olması gerekir.

Yetkili URL 'SI örnekten ve hedef kitlenden oluşur.

Yetkili şu olabilir:
- Bir Azure AD bulut yetkilisi.
- Azure AD B2C yetkilisi. [B2C özelliklerine](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics)bakın.
- Active Directory Federasyon Hizmetleri (AD FS) (AD FS) yetkilisi. Bkz. [AD FS desteği](https://aka.ms/msal-net-adfs-support).

Azure AD bulut yetkilileri iki bölümden oluşur:
- Kimlik sağlayıcısı *örneği*
- Uygulama için oturum açma *hedef kitlesi*

Örnek ve hedef kitle, yetkili URL 'SI olarak birleştirilebilir ve belirtilebilir. MSAL 3 ' ten önceki MSAL.NET sürümlerinde. *x*, hedeflemek istediğiniz buluta ve oturum açma kitlesini temel alarak yetkili bir şekilde oluşturmanız gerekiyordu.  Bu diyagram, yetkili URL 'sinin nasıl oluşturulduğunu gösterir:

![Yetkili URL 'SI nasıl oluşturulur?](media/msal-client-application-configuration/authority.png)

## <a name="cloud-instance"></a>Bulut örneği
*Örnek* , uygulamanızın Azure genel bulutundaki veya ulusal bulutlardan Kullanıcı imzalanmasını belirtmek için kullanılır. Kodunuzda msal kullanarak, bir sabit listesi kullanarak veya URL 'yi (bildiğiniz takdirde) [Ulusal bulut örneğine](authentication-national-cloud.md#azure-ad-authentication-endpoints) `Instance` geçirerek Azure bulut örneğini ayarlayabilirsiniz.

Her ikisi de `Instance` `AzureCloudInstance` belirtilirse, msal.net açık bir özel durum oluşturur.

Bir örnek belirtmezseniz, uygulamanız Azure genel bulut örneğini (URL `https://login.onmicrosoftonline.com`'nin örneği) hedefleyecek.

## <a name="application-audience"></a>Uygulama hedef kitlesi

Oturum açma hedef kitlesi, uygulamanız için iş ihtiyaçlarına bağlıdır:
- İş kolu (LOB) geliştiricisiyseniz, muhtemelen yalnızca kuruluşunuzda kullanılacak tek kiracılı bir uygulama oluşturacaksınız. Bu durumda, kuruluş KIMLIĞI (Azure AD örneğinizin KIMLIĞI) veya Azure AD örneğiyle ilişkili bir etki alanı adı ile organizasyonu belirtmeniz gerekir.
- ISV iseniz, kullanıcıların herhangi bir kuruluşta veya bazı kuruluşlarda (çok kiracılı uygulama) iş ve okul hesaplarıyla oturum açmak isteyebilirsiniz. Ancak kullanıcıların kişisel Microsoft hesaplarıyla oturum açmasını da isteyebilirsiniz.

### <a name="how-to-specify-the-audience-in-your-codeconfiguration"></a>Kodunuzda/yapılandırmanızda hedef kitleyi belirtme
Kodunuzda MSAL kullanarak, aşağıdaki değerlerden birini kullanarak hedef kitleyi belirtirsiniz:
- Azure AD yetkilisi hedef kitlesi numaralandırması
- Kiracı KIMLIĞI:
  - Tek kiracılı uygulamalar için GUID (Azure AD örneğinizin KIMLIĞI)
  - Azure AD örneğiniz ile ilişkili bir etki alanı adı (tek kiracılı uygulamalar için de)
- Azure AD yetkilisi hedef kitlesi numaralandırması yerine bir kiracı KIMLIĞI olarak bu yer tutuculardan biri:
    - `organizations`çok kiracılı bir uygulama için
    - `consumers`kullanıcıları yalnızca kendi kişisel hesaplarıyla oturum açmak için
    - `common`kullanıcılara iş ve okul hesaplarıyla veya kendi kişisel Microsoft hesaplarıyla oturum açmak için

Hem Azure AD yetkilisi kitleyi hem de kiracı KIMLIĞINI belirtirseniz MSAL anlamlı bir özel durum oluşturur.

Hedef kitle belirtmezseniz, uygulamanız Azure AD ve kişisel Microsoft hesaplarını bir hedef kitle olarak hedefleyecek. (Yani, belirtildiği gibi `common` davranır.)

### <a name="effective-audience"></a>Etkin hedef kitle
Uygulamanız için geçerli hedef kitle, uygulamanızda belirlediğiniz hedef kitlesinin ve uygulama kaydında belirtilen hedef kitlesinin en az (bir kesişmesi varsa) olacaktır. Aslında [uygulama kayıtları](https://aka.ms/appregistrations) deneyimi, uygulama için hedef kitleyi (desteklenen hesap türleri) belirtmenize olanak tanır. Daha fazla bilgi için bkz [. hızlı başlangıç: Bir uygulamayı Microsoft Identity platformu](quickstart-register-app.md)ile kaydedin.

Şu anda, yalnızca kişisel Microsoft hesaplarıyla kullanıcıların oturum açması için bir uygulama almanın tek yolu bu ayarların her ikisini de yapılandırmaktır:
- Uygulama kaydı kitlesini olarak `Work and school accounts and personal accounts`ayarlayın.
- Kodunuzda/yapılandırmanızda bulunan hedef kitleyi (veya `AadAuthorityAudience.PersonalMicrosoftAccount` `TenantID` = "tüketiciler") olarak ayarlayın.

## <a name="client-id"></a>İstemci Kimliği
İstemci KIMLIĞI, uygulama kaydedildiğinde Azure AD tarafından uygulamanıza atanan benzersiz uygulama (istemci) KIMLIĞIDIR.

## <a name="redirect-uri"></a>Yeniden yönlendirme URI 'SI
Yeniden yönlendirme URI 'si, kimlik sağlayıcısı 'nın güvenlik belirteçlerini uygulamasına geri göndereceği URI 'dir.

### <a name="redirect-uri-for-public-client-apps"></a>Ortak istemci uygulamaları için yeniden yönlendirme URI 'SI
MSAL kullanan ortak bir istemci uygulama geliştiricisiyseniz:
- Masaüstü veya UWP uygulamalarında kullanmak `.WithDefaultRedirectUri()` istersiniz (msal.NET 4.1 +). Bu yöntem, ortak istemci uygulamasının yeniden yönlendirme URI özelliğini genel istemci uygulamaları için önerilen varsayılan yeniden yönlendirme URI 'si olarak ayarlar. 

  Platform  | Yeniden yönlendirme URI 'SI  
  ---------  | --------------
  Masaüstü uygulaması (.NET ILT) | `https://login.microsoftonline.com/common/oauth2/nativeclient` 
  UWP | `WebAuthenticationBroker.GetCurrentApplicationCallbackUri()`değeri. Bu, kaydolmanız gereken WebAuthenticationBroker. GetCurrentApplicationCallbackUri () sonucuna değer ayarlayarak, SSO 'yu tarayıcıda sağlar
  .NET Core | `https://localhost`. Bu, .NET Core 'un katıştırılmış Web görünümü için bir kullanıcı arabirimine sahip olmadığından kullanıcının etkileşimli kimlik doğrulaması için sistem tarayıcısını kullanmasına olanak sağlar.

- Aracıyı desteklemeyen bir Xamarin Android ve iOS uygulaması oluşturuyorsanız, yeniden yönlendirme URI 'si eklemeniz gerekmez (yeniden yönlendirme URI 'si Xamarin Android ve iOS `msal{ClientId}://auth` için otomatik olarak ayarlanır)

- [Uygulama kayıtları](https://aka.ms/appregistrations)YENIDEN yönlendirme URI 'sini yapılandırmanız gerekir:

   ![Uygulama kayıtları yeniden yönlendirme URI 'SI](media/msal-client-application-configuration/redirect-uri.png)

`RedirectUri` Özelliğini kullanarak yeniden yönlendirme URI 'sini geçersiz kılabilirsiniz (örneğin, aracılar kullanıyorsanız). Bu senaryo için yeniden yönlendirme URI 'Leri örnekleri aşağıda verilmiştir:

- `RedirectUriOnAndroid` = "msauth-5a434691-ccb2-4fd1-b97b-b64bcfbc03fc://com.microsoft.identity.client.sample";
- `RedirectUriOnIos`= $ "msauth. {Demeti. ID}://kimlik doğrulaması ";

Ayrıntılar için bkz. [Android ve iOS belgeleri](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS).

### <a name="redirect-uri-for-confidential-client-apps"></a>Gizli istemci uygulamaları için yeniden yönlendirme URI 'SI
Web Apps için, yeniden yönlendirme URI 'SI (veya Yanıt URI 'SI), Azure AD 'nin belirteci uygulamaya geri göndermek için kullanacağı URI 'dir. Gizli uygulama bunlardan biri ise, bu URI Web uygulaması/Web API 'sinin URL 'si olabilir. Yeniden yönlendirme URI 'sinin uygulama kaydında kayıtlı olması gerekir. Başlangıçta yerel olarak test ettiğiniz bir uygulamayı dağıtırken bu kayıt özellikle önemlidir. Ardından, uygulama kayıt portalı 'nda dağıtılan uygulamanın yanıt URL 'sini eklemeniz gerekir.

Daemon uygulamaları için bir yeniden yönlendirme URI 'SI belirtmeniz gerekmez.

## <a name="client-secret"></a>Gizli anahtar
Bu seçenek, gizli istemci uygulaması için istemci parolasını belirtir. Bu gizli dizi (uygulama parolası) uygulama kayıt portalı tarafından sağlanır veya PowerShell AzureAD, PowerShell Azurerd veya Azure CLı ile uygulama kaydı sırasında Azure AD 'ye sağlanır.

## <a name="logging"></a>Günlüğe Kaydetme
Diğer yapılandırma seçenekleri günlüğe kaydetme ve sorun gidermeyi etkinleştirir. Nasıl kullanılacağına ilişkin ayrıntılar için [günlüğe kaydetme](msal-logging.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Msal.NET kullanarak istemci uygulamaları örneğini](msal-net-initializing-client-applications.md)oluşturma hakkında bilgi edinin.

[Msal. js kullanarak istemci uygulamaları örneğini](msal-js-initializing-client-applications.md)oluşturma hakkında bilgi edinin.
