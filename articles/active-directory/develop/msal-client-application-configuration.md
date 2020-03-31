---
title: İstemci uygulama yapılandırması (MSAL) | Azure
titleSuffix: Microsoft identity platform
description: Microsoft Kimlik Doğrulama Kitaplığı'nı (MSAL) kullanarak ortak istemci ve gizli istemci uygulamaları için yapılandırma seçenekleri hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/27/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 0262d22ae00456ce06cb8efbf995f1a093b20043
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262899"
---
# <a name="application-configuration-options"></a>Uygulama yapılandırma seçenekleri

Kodunuzda, belirteçleri doğrulamak ve satın almak için yeni bir ortak veya gizli istemci uygulamasını (veya MSAL.js için kullanıcı aracısını) başlatırsınız. Microsoft Kimlik Doğrulama Kitaplığı'nda (MSAL) istemci uygulamasını başharfe aldığınızda bir dizi yapılandırma seçeneği ayarlayabilirsiniz. Bu seçenekler iki gruba ayrılır:

- Kayıt seçenekleri, dahil:
    - [Yetki](#authority) (uygulama için kimlik sağlayıcı [örneği](#cloud-instance) ve oturum açma [hedef kitlesi](#application-audience) ve muhtemelen kiracı kimliğinden oluşur).
    - [İstemci Kimliği](#client-id).
    - [URI'yi yeniden yönlendirin.](#redirect-uri)
    - [İstemci sırrı](#client-secret) (gizli istemci uygulamaları için).
- Günlük düzeyi, kişisel verilerin kontrolü ve kitaplığı kullanan bileşenin adı da dahil olmak üzere [günlüğe kaydetme seçenekleri.](#logging)

## <a name="authority"></a>Yetkili

Yetki, MSAL'ın jeton isteyebileceği bir dizini gösteren bir URL'dir. Ortak makamlar şunlardır:

- &gt; kiracının Azure\<\>Etkin Dizin (Azure AD) kiracısının veya bu Azure AD kiracısıyla ilişkili bir etki alanının kiracı kimliği olduğu &lt;https\://login.microsoftonline.com/ kiracısı., Yalnızca belirli bir kuruluşun kullanıcılarını oturum etmek için kullanılır.
- https\://login.microsoftonline.com/common/. İş ve okul hesapları veya kişisel Microsoft hesapları olan kullanıcıları oturum adamak için kullanılır.
- https\://login.microsoftonline.com/organizations/. İş ve okul hesapları olan kullanıcıları oturum alabilmek için kullanılır.
- https\://login.microsoftonline.com/consumers/. Yalnızca kişisel Microsoft hesapları (eskiden Windows Live ID hesapları olarak bilinir) olan kullanıcıları oturum açmada kullanılır.

Yetki ayarı, başvuru kayıt portalında beyan edilenlerle tutarlı olmalıdır.

Yetkili URL örnek ve hedef kitleden oluşur.

Yetki şu olabilir:
- Azure AD bulut yetkilisi.
- Azure AD B2C yetkilisi. [Bkz. B2C özellikleri.](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics)
- Aktif Dizin Federasyon Hizmetleri (AD FS) yetkilisi. [BKZ. AD FS desteği.](https://aka.ms/msal-net-adfs-support)

Azure AD bulut yetkililerinin iki bölümü vardır:
- Kimlik sağlayıcı *örneği*
- Uygulamaiçin oturum açma *hedef kitlesi*

Örnek ve hedef kitle kısıtlanabilir ve yetkili URL olarak sağlanabilir. MSAL 3'ten önceki MSAL.NET sürümlerinde. *x*, hedefalmak istediğiniz buluta ve oturum açma hedefkitlesine dayanarak yetkiyi kendiniz oluşturmanız gerekiyordu.  Bu diyagram, yetkili URL'nin nasıl oluşturuldurulur:

![Yetkili URL nasıl oluşturulur?](media/msal-client-application-configuration/authority.png)

## <a name="cloud-instance"></a>Bulut örneği

*Örnek,* uygulamanızın kullanıcıları Azure genel bulutundan mı yoksa ulusal bulutlardan mı imzaladığını belirtmek için kullanılır. Kodunuzda MSAL'ı kullanarak, bir numaralandırma kullanarak veya URL'yi `Instance` üye olarak [ulusal bulut örneğine](authentication-national-cloud.md#azure-ad-authentication-endpoints) geçirerek (bunu biliyorsanız) Azure bulut örneğini ayarlayabilirsiniz.

MSAL.NET her ikisi de `Instance` ve `AzureCloudInstance` belirtilirse açık bir özel durum atacaktır.

Bir örnek belirtmezseniz, uygulamanız Azure genel bulut örneğini (URL `https://login.onmicrosoftonline.com`örneği) hedefler.

## <a name="application-audience"></a>Uygulama hedef kitlesi

Oturum açma hedef kitlesi uygulamanızın iş gereksinimlerine bağlıdır:
- Bir iş hattı (LOB) geliştiricisiyseniz, büyük olasılıkla yalnızca kuruluşunuzda kullanılacak tek kiracılı bir uygulama üretebilirsiniz. Bu durumda, kuruluşu kiracı kimliğine (Azure AD örneğinizin kimliğine) veya Azure REKLAM örneğiyle ilişkili bir etki alanı adı yla belirtmeniz gerekir.
- ISV'yseniz, herhangi bir kuruluşta veya bazı kuruluşlarda (çok kiracılı uygulama) iş ve okul hesaplarıyla kullanıcılaroturum imzalamak isteyebilirsiniz. Ancak, kullanıcıların kişisel Microsoft hesaplarıyla oturum açmalarını da isteyebilirsiniz.

### <a name="how-to-specify-the-audience-in-your-codeconfiguration"></a>Kodunuzda/yapılandırmanızda hedef kitleyi belirtme

Kodunuzda MSAL'ı kullanarak, aşağıdaki değerlerden birini kullanarak hedef kitleyi belirtirsiniz:
- Azure AD yetkilisi hedef kitle numaralandırma
- Kiracı kimliği, hangi olabilir:
  - Tek kiracılı uygulamalar için bir GUID (Azure REKLAM örneğinizin kimliği)
  - Azure REKLAM örneğinizle ilişkili bir etki alanı adı (tek kiracılı uygulamalar için de)
- Azure AD yetkilisi hedef kitle numaralandırması yerine kiracı kimliği olarak bu yer sahiplerinden biri:
    - `organizations`çok kiracılı bir uygulama için
    - `consumers`kullanıcılara yalnızca kişisel hesaplarıyla oturum açma
    - `common`kullanıcıların iş ve okul hesapları veya kişisel Microsoft hesapları ile oturum

Hem Azure REKLAM yetkilisi kitlesini hem de kiracı kimliğini belirtirseniz, MSAL anlamlı bir özel durum oluşturur.

Bir hedef kitle belirtmezseniz, uygulamanız hedef kitle olarak Azure REKLAM'ı ve kişisel Microsoft hesaplarını hedefler. (Yani, belirtildiği gibi `common` olacaktır.)

### <a name="effective-audience"></a>Etkili hedef kitle

Uygulamanızın etkili hedef kitlesi, uygulamanızda belirlediğiniz hedef kitlenin ve uygulama kaydında belirtilen hedef kitlenin en az (bir kesişim varsa) olacaktır. Aslında, [Uygulama kayıtları](https://aka.ms/appregistrations) deneyimi, uygulamanın hedef kitlesini (desteklenen hesap türlerini) belirtmenize olanak tanır. Daha fazla bilgi için [Quickstart: Microsoft kimlik platformuna bir uygulama kaydedin.](quickstart-register-app.md)

Şu anda, bir uygulamanın yalnızca kişisel Microsoft hesapları olan kullanıcılarda oturum açmasını elde etmenin tek yolu, bu ayarların her ikisini de yapılandırmaktır:
- Uygulama kayıt hedef `Work and school accounts and personal accounts`kitlesini .
- Kodunuzda/yapılandırmanızdaki hedef kitleyi `AadAuthorityAudience.PersonalMicrosoftAccount` `TenantID` (veya ="tüketiciler") olarak ayarlayın.

## <a name="client-id"></a>İstemci Kimliği

İstemci kimliği, uygulama kaydedildiğinde Azure AD tarafından uygulamanız için atanan benzersiz uygulama (istemci) kimliğidir.

## <a name="redirect-uri"></a>Yeniden Yönlendirme URI'si

Uri yönlendirme, kimlik sağlayıcısının güvenlik belirteçlerini geri göndereceği URI'dir.

### <a name="redirect-uri-for-public-client-apps"></a>Genel istemci uygulamaları için URI'yi yeniden yönlendirme

MSAL kullanan bir genel istemci uygulama geliştiricisiyseniz:
- Masaüstü veya UWP `.WithDefaultRedirectUri()` uygulamalarında (MSAL.NET 4.1+) kullanmak istiyorsunuz. Bu yöntem, kamu istemcisi uygulamanın yeniden yönlendirme uri özelliğini, kamu istemcisi uygulamaları için önerilen varsayılan yeniden yönlendirme uri'ye ayarlar. 

  Platform  | Yeniden Yönlendirme URI'si  
  ---------  | --------------
  Masaüstü uygulaması (.NET FW) | `https://login.microsoftonline.com/common/oauth2/nativeclient` 
  UWP | değeri `WebAuthenticationBroker.GetCurrentApplicationCallbackUri()`. Bu webauthenticationBroker.GetCurrentApplicationCallbackUri() hangi kayıt gerekir sonucu değerini ayarlayarak tarayıcı ile SSO sağlar
  .NET Core | `https://localhost`. Bu, .NET Core'un şu anda gömülü web görünümü için bir kullanıcı arabirimi olmadığından, kullanıcının etkileşimli kimlik doğrulaması için sistem tarayıcısını kullanmasını sağlar.

- Brokeri desteklemeyen bir Xamarin Android ve iOS uygulaması oluşturuyorsanız yeniden yönlendirme URI eklemenize gerek yoktur (yeniden yönlendirme `msal{ClientId}://auth` URI otomatik olarak Xamarin Android ve iOS için ayarlanır

- [Uygulama kayıtlarında](https://aka.ms/appregistrations)yeniden yönlendirme URI yapılandırmanız gerekir:

   ![Uygulama kayıtlarında URI'yi yönlendirme](media/msal-client-application-configuration/redirect-uri.png)

Özelliği kullanarak URI'yi `RedirectUri` geçersiz kılabilirsiniz (örneğin, aracı kullanıyorsanız). Bu senaryo için yönlendirme URL'leri ile ilgili bazı örnekler aşağıda verilmiştir:

- `RedirectUriOnAndroid`= "msauth-5a434691-ccb2-4fd1-b97b-b64bcfbc03fc://com.microsoft.identity.client.sample";
- `RedirectUriOnIos`= $"msauth. {Bundle.ID}://auth";

Ek iOS ayrıntıları için bkz: [microsoft authenticator'ı ADAL.NET'dan MSAL.NET'ye kadar kullanan](msal-net-migration-ios-broker.md) ve [iOS'taki aracıyı kullanan](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS)iOS'u geçirin.
Ek Android ayrıntıları için, [Android'de Aracılı auth'a](brokered-auth.md)bakın.

### <a name="redirect-uri-for-confidential-client-apps"></a>Uri'yi gizli istemci uygulamaları için yönlendirme

Web uygulamaları için yeniden yönlendirme URI (veya yanıt URI), Azure AD'nin belirteci uygulamaya geri göndermek için kullanacağı URI'dir. Bu URI, gizli uygulama bunlardan biriyse web uygulamasının/Web API'sinin URL'si olabilir. Yeniden yönlendirme URI'nin uygulama kaydına kaydedilmesi gerekir. Bu kayıt, başlangıçta yerel olarak test ettiğiniz bir uygulamayı dağıttığınızda özellikle önemlidir. Daha sonra uygulama kayıt portalına dağıtılan uygulamanın yanıt URL'sini eklemeniz gerekir.

Daemon uygulamaları için yeniden yönlendirme URI belirtmeniz gerekmez.

## <a name="client-secret"></a>Gizli anahtar

Bu seçenek, gizli istemci uygulaması için istemci sırrını belirtir. Bu gizli (uygulama parolası), uygulama kayıt portalı tarafından sağlanır veya PowerShell AzureAD, PowerShell AzureRM veya Azure CLI ile uygulama kaydı sırasında Azure AD'ye sağlanır.

## <a name="logging"></a>Günlüğe Kaydetme

Diğer yapılandırma seçenekleri günlüğe kaydetmeyi ve sorun gidermeyi sağlar. Bunları nasıl kullanacağınız la ilgili ayrıntılar için [Günlük](msal-logging.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

MSAL.NET [kullanarak istemci uygulamalarını anında](msal-net-initializing-client-applications.md)kullanma hakkında bilgi edinin.
[MSAL.js kullanarak istemci uygulamaları anlık](msal-js-initializing-client-applications.md)hakkında bilgi edinin.
