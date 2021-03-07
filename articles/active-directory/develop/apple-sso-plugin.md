---
title: Apple cihazlar için Microsoft Enterprise SSO eklentisi
titleSuffix: Microsoft identity platform | Azure
description: İOS, ıpados ve macOS cihazları için Microsoft 'un Azure Active Directory SSO eklentisi hakkında bilgi edinin.
services: active-directory
author: brandwe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/15/2020
ms.author: brandwe
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: 96fbf23128896f23beee70a6b3d32b4b87a454ea
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102427105"
---
# <a name="microsoft-enterprise-sso-plug-in-for-apple-devices-preview"></a>Apple cihazları için Microsoft Enterprise SSO eklentisi (Önizleme)

>[!IMPORTANT]
> Bu özellik [!INCLUDE [PREVIEW BOILERPLATE](../../../includes/active-directory-develop-preview.md)]

*Apple cihazları Için Microsoft ENTERPRISE SSO eklentisi* , Apple 'ın [Kurumsal Çoklu oturum açma](https://developer.apple.com/documentation/authenticationservices) özelliğini destekleyen tüm uygulamalarda MacOS, iOS ve ıpados 'LARDA Azure Active Directory (Azure AD) hesapları için çoklu oturum açma (SSO) sağlar. Bu, işletmenizin bağlı olabileceği, ancak henüz en son kimlik kitaplıklarını veya protokolleri desteklemeyen eski uygulamaları içerir. Microsoft, Apple ve Microsoft 'un sağlayabilecekleri en iyi korumayı sağlarken uygulamanızın kullanılabilirliğini artırmak üzere bu eklentiyi geliştirmek üzere Apple ile yakın bir şekilde çalışmıştır.

Enterprise SSO eklentisi şu anda aşağıdaki uygulamaların yerleşik bir özelliği olarak kullanılabilir:

* [Microsoft Authenticator](../user-help/user-help-auth-app-overview.md) -IOS, ıpados
* Microsoft Intune [Şirket portalı](/mem/intune/apps/apps-company-portal-macos) -MacOS

## <a name="features"></a>Özellikler

Apple cihazları için Microsoft Enterprise SSO eklentisi aşağıdaki avantajları sunar:

- Apple 'ın Kurumsal tek Sign-On özelliğini destekleyen tüm uygulamalarda Azure AD hesapları için SSO sağlar.
- , Herhangi bir mobil cihaz yönetimi (MDM) çözümü tarafından etkinleştirilebilir.
- SSO 'yu henüz Microsoft Identity platform kitaplıklarını kullanmayan uygulamalara genişletir.
- SSO 'yu OAuth2, OpenID Connect ve SAML kullanan uygulamalarla genişletir.

## <a name="requirements"></a>Gereksinimler

Apple cihazları için Microsoft Enterprise SSO eklentisini kullanmak için:

- Cihaz, **yüklü** Apple cihazları Için MICROSOFT Enterprise SSO eklentisini içeren bir uygulamayı **desteklemelidir** ve içermelidir:
  - iOS 13.0 +: [Microsoft Authenticator uygulaması](../user-help/user-help-auth-app-overview.md)
  - Idos 13.0 +: [Microsoft Authenticator uygulaması](../user-help/user-help-auth-app-overview.md)
  - macOS 10.15 +: [Intune şirket portalı uygulaması](/mem/intune/user-help/enroll-your-device-in-intune-macos-cp)
- Cihazın MDM ile **kayıtlı** olması gerekir (örneğin, Microsoft Intune ile).
- Cihazda Enterprise SSO eklentisini etkinleştirmek için yapılandırma **cihaza itilmiş** olmalıdır. Bu güvenlik kısıtlaması Apple tarafından gerektirilir.

### <a name="ios-requirements"></a>iOS gereksinimleri:
- cihazda iOS 13,0 veya üzeri yüklü olmalıdır.
- Apple cihazları için Microsoft Enterprise SSO eklentisini sağlayan bir Microsoft uygulamasının cihazda yüklü olması gerekir. Genel önizleme için bu uygulamalar [Microsoft Authenticator uygulamasıdır](/intune/user-help/user-help-auth-app-overview.md).


### <a name="macos-requirements"></a>macOS gereksinimleri:
- macOS 10,15 veya üzeri cihazda yüklü olmalıdır. 
- Apple cihazları için Microsoft Enterprise SSO eklentisini sağlayan bir Microsoft uygulamasının cihazda yüklü olması gerekir. Genel önizleme için bu uygulamalar [Intune şirket portalı uygulamasını](/intune/user-help/enroll-your-device-in-intune-macos-cp.md)içerir.

## <a name="enable-the-sso-plug-in-with-mobile-device-management-mdm"></a>Mobil cihaz yönetimi (MDM) ile SSO eklentisini etkinleştirme

### <a name="microsoft-intune-configuration"></a>Microsoft Intune yapılandırması

MDM hizmetiniz olarak Microsoft Intune kullanıyorsanız, Microsoft Enterprise SSO eklentisini etkinleştirmek için yerleşik yapılandırma profili ayarlarını kullanabilirsiniz.

İlk olarak, bir yapılandırma profilinin [Çoklu oturum açma uygulama uzantısı](/mem/intune/configuration/device-features-configure#single-sign-on-app-extension) ayarlarını yapılandırın ve [profili bir kullanıcı veya cihaz grubuna atayın](/mem/intune/configuration/device-profile-assign) (henüz atanmamışsa).

SSO eklentisini etkinleştiren profil ayarları, her cihaz Intune ile ilk kez iade ettiğinde grubun cihazlarına otomatik olarak uygulanır.

### <a name="manual-configuration-for-other-mdm-services"></a>Diğer MDM Hizmetleri için el ile yapılandırma

Mobil cihaz yönetimi için Microsoft Intune kullanmıyorsanız, Apple cihazları için Microsoft Enterprise SSO eklentisini yapılandırmak için aşağıdaki parametreleri kullanın.

#### <a name="ios-settings"></a>iOS ayarları:

- **UZANTı kimliği**: `com.microsoft.azureauthenticator.ssoextension`
- **Takım Kimliği**: (Bu alan iOS için gerekli değildir)

#### <a name="macos-settings"></a>macOS ayarları:

- **UZANTı kimliği**: `com.microsoft.CompanyPortalMac.ssoextension`
- **Takım Kimliği**: `UBF8T346G9`

#### <a name="common-settings"></a>Ortak ayarlar:

- **Tür**: Redirect
  - `https://login.microsoftonline.com`
  - `https://login.microsoft.com`
  - `https://sts.windows.net`
  - `https://login.partner.microsoftonline.cn`
  - `https://login.chinacloudapi.cn`
  - `https://login.microsoftonline.de`
  - `https://login.microsoftonline.us`
  - `https://login.usgovcloudapi.net`
  - `https://login-us.microsoftonline.com`
  
### <a name="additional-configuration-options"></a>Ek yapılandırma seçenekleri
Ek yapılandırma seçenekleri, SSO işlevselliğini ek uygulamalara genişletmek için eklenebilir.

#### <a name="enable-sso-for-apps-that-dont-use-a-microsoft-identity-platform-library"></a>Microsoft Identity platform kitaplığı kullanmayan uygulamalar için SSO 'yu etkinleştirme

SSO eklentisi, Microsoft kimlik doğrulama kitaplığı (MSAL) gibi bir Microsoft SDK kullanılarak geliştirmeseler bile, tüm uygulamaların çoklu oturum açma 'ya katılmasını sağlar.

SSO eklentisi, iOS ve ıpados üzerinde Microsoft Authenticator uygulamayı indirmiş olan cihazlar tarafından otomatik olarak yüklenir ve macOS üzerinde Intune Şirket Portalı uygulama ve cihazlarını kuruluşunuzla kaydettiniz. Kuruluşunuz muhtemelen çok faktörlü kimlik doğrulaması, parola-daha az kimlik doğrulama ve koşullu erişim gibi senaryolar için kimlik doğrulayıcı uygulamasını günümüzde kullanıyor. Microsoft 'un Intune 'un Microsoft Uç Nokta Yöneticisi içinde yapılandırılması kolay olsa da, herhangi bir MDM sağlayıcısı kullanılarak uygulamalarınız için etkinleştirilebilir. Bu uygulamaları SSO eklentisini kullanacak şekilde yapılandırmak için bir izin verilenler listesi kullanılır.

>[!IMPORTANT]
> Yalnızca yerel Apple ağ teknolojilerini veya Web görünümlerini kullanan uygulamalar desteklenir. Bir uygulama kendi ağ katmanı uygulamasını kullanıyorsa, Microsoft Enterprise SSO eklentisi desteklenmez.  

Microsoft Identity platform kitaplığı kullanmayan uygulamalar için Microsoft Enterprise SSO eklentisini yapılandırmak için aşağıdaki parametreleri kullanın:

Belirli uygulamaların bir listesini sağlamak istiyorsanız:

- **Anahtar**: `AppAllowList`
- **Şunu yazın**: `String`
- **Değer**: SSO 'ya katılmasına izin verilen uygulamalar için uygulama paket kimliklerinin virgülle ayrılmış listesi
- **Örnek**: `com.contoso.workapp, com.contoso.travelapp`

Ön eklerin bir listesini sağlamak istiyorsanız:
- **Anahtar**: `AppPrefixAllowList`
- **Şunu yazın**: `String`
- **Değer**: SSO 'ya katılmasına izin verilen uygulamalar için uygulama paket kimliği öneklerinin virgülle ayrılmış listesi. Bunun, belirli bir önekle başlayan tüm uygulamaların SSO 'ya katılmasını olanaklı olduğunu unutmayın.
- **Örnek**: `com.contoso., com.fabrikam.`

MDM yöneticisinin SSO 'ya katılması için izin verilen [uygulamalar](./application-consent-experience.md) , son kullanıcı için sessizce bir belirteç alabilir. Bu nedenle, yalnızca izin verilenler listesine güvenilen uygulamalar eklemek önemlidir. 

>[!NOTE]
> Bu listeye MSAL veya ASWebAuthenticationSession kullanan uygulamalar eklemeniz gerekmez. Bu uygulamalar varsayılan olarak etkinleştirilmiştir. 

##### <a name="how-to-discover-app-bundle-identifiers-on-ios-devices"></a>İOS cihazlarında uygulama paketi tanımlayıcılarını bulma

Apple, App Store 'dan paket kimliklerini bulmayı kolay bir yol sağlamaz. SSO için kullanmak istediğiniz uygulamaların paket kimliklerini bulmanın en kolay yolu, satıcınız veya uygulama geliştiricinizi sormaktan biridir. Bu seçenek kullanılamıyorsa, paket kimliklerini saptamak için MDM yapılandırmanızı kullanabilirsiniz. 

MDM yapılandırmanızda aşağıdaki bayrağı geçici olarak etkinleştirin:

- **Anahtar**: `admin_debug_mode_enabled`
- **Şunu yazın**: `Integer`
- **Değer**: 1 veya 0

Bu bayrak, paket KIMLIĞINI bildirmek istediğiniz cihazdaki iOS uygulamalarında oturum açın. Sonra Microsoft Authenticator App-> yardım-> günlükleri Gönder-> görüntüleme günlükleri ' ni açın. 

Günlük dosyasında aşağıdaki satırı arayın:

`[ADMIN MODE] SSO extension has captured following app bundle identifiers:`

Bu, SSO uzantısına görünür tüm uygulama paketi tanımlayıcılarını yakalemelidir. Daha sonra bu tanımlayıcıları, bu uygulamalar için SSO 'yu yapılandırmak üzere kullanabilirsiniz. 

#### <a name="allow-user-to-sign-in-from-unknown-applications-and-the-safari-browser"></a>Kullanıcının bilinmeyen uygulamalardan ve Safari tarayıcısından oturum açmasını sağlar.

Varsayılan olarak, Microsoft Enterprise SSO eklentisi yetkili uygulamalar için, yalnızca bir Kullanıcı ADAL veya MSAL gibi bir Microsoft Identity platform kitaplığı kullanan bir uygulamadan oturum açmışsa, SSO sağlar. Microsoft Enterprise SSO eklentisi, yeni bir belirteç alımı sırasında Microsoft Identity platform kitaplığı kullanan başka bir uygulama tarafından çağrıldığında paylaşılan bir kimlik bilgisi de alabilir.

`browser_sso_interaction_enabled`Bayrak etkinleştirme, ilk önyükleme yapmak ve paylaşılan bir kimlik bilgisi almak Için Microsoft Identity platform kitaplığı kullanmayan uygulamayı etkinleştirir. Ayrıca Safari tarayıcısının ilk önyükleme yapmasına ve paylaşılan bir kimlik bilgisi almasına olanak tanır. Microsoft Enterprise SSO eklentisinin, henüz paylaşılan bir kimlik bilgisi yoksa, Safari tarayıcısı, ASWebAuthenticationSession, SafariViewController veya başka bir izin verilen yerel uygulama içindeki bir Azure AD URL 'sinden her oturum açma işlemi yapıldığında bir tane almaya çalışır.  

- **Anahtar**: `browser_sso_interaction_enabled`
- **Şunu yazın**: `Integer`
- **Değer**: 1 veya 0

MacOS için bu ayar, tüm uygulamalar üzerinde daha tutarlı bir deneyim sağlamak için gereklidir. İOS ve ıpados için bu ayar gerekli değildir çünkü çoğu uygulama oturum açma için Microsoft Authenticator uygulamasını kullanır. Ancak, iOS veya ıpados üzerinde Microsoft Authenticator kullanmayan bazı uygulamalarınız varsa, bu bayrak, ayarı etkinleştirmenizi öneririz. Varsayılan olarak devre dışıdır.

#### <a name="disable-asking-for-mfa-on-initial-bootstrapping"></a>İlk önyükleme sırasında MFA sorulmayı devre dışı bırak

Varsayılan olarak, Microsoft Enterprise SSO eklentisi, kullanıcının başlatıldığı geçerli uygulama için gerekli olmasa bile ilk önyüklemeyi gerçekleştirirken ve paylaşılan bir kimlik bilgisi alırken kullanıcıyı her zaman Multi-Factor Authentication (MFA) için ister. Bu nedenle, MFA daha sonra gerekli hale gelirse, paylaşılan kimlik bilgileri kullanıcıya sormadan tüm ek uygulamalarda kolayca kullanılabilir. Bu, kullanıcının cihazda sorulmasının gerektiği süreyi azaltır ve genellikle iyi bir karardır.

Etkinleştirme `browser_sso_disable_mfa` bunu devre dışı bırakır ve yalnızca bir uygulama veya kaynak IÇIN MFA gerektiğinde kullanıcıya sorar. 

- **Anahtar**: `browser_sso_disable_mfa`
- **Şunu yazın**: `Integer`
- **Değer**: 1 veya 0

Kullanıcının cihazda istenmesi gereken süreyi azalttığından, bu bayrağın devre dışı kalmasını öneririz. Kuruluşunuz genellikle MFA kullanıyorsa, bayrağı etkinleştirmek isteyebilirsiniz, ancak bunun yerine MFA 'yı daha sık kullanmanızı öneririz. Bu nedenle, varsayılan olarak devre dışıdır.

#### <a name="disable-oauth2-application-prompts"></a>OAuth2 uygulama istemlerini devre dışı bırak

Microsoft Enterprise SSO eklentisi, izin verilen uygulamalardan gelen ağ isteklerine paylaşılan kimlik bilgilerini ekleyerek SSO sağlar. Ancak bazı OAuth2 uygulamalar, protokol katmanında Son Kullanıcı komut istemlerini yanlış bir şekilde uygulayabilir. Bu durumda, paylaşılan kimlik bilgilerinin bu uygulamalar için yoksayıldığını görürsünüz ve Microsoft Enterprise SSO eklentisi diğer uygulamalar için çalışsa bile, kullanıcıdan oturum açması istenir.  

`disable_explicit_app_prompt`Bayrak etkinleştirme, hem yerel hem de Web uygulamalarının, protokol katmanında Son Kullanıcı istemi 'ni zorunlu hale getirme ve SSO atlama olanağını kısıtlar.

- **Anahtar**: `disable_explicit_app_prompt`
- **Şunu yazın**: `Integer`
- **Değer**: 1 veya 0

Tüm uygulamalar üzerinde daha tutarlı bir deneyim sağlamak için bu bayrağın etkinleştirilmesini öneririz. Varsayılan olarak devre dışıdır. 

#### <a name="enable-sso-through-cookies-for-specific-application"></a>Belirli bir uygulama için tanımlama bilgileri aracılığıyla SSO 'yu etkinleştirme

Az sayıda uygulama SSO uzantısıyla uyumsuz olabilir. Özellikle, Gelişmiş ağ ayarlarına sahip uygulamalar, SSO için etkinleştirildiklerinde beklenmeyen sorunlarla karşılaşabilir (örneğin, ağ isteğinin iptal edildiğini veya kesintiye uğradığını belirten bir hata görebilirsiniz). 

Bölümünde açıklanan yöntemi kullanarak oturum açmada sorun yaşıyorsanız `Enable SSO for apps that don't use MSAL` , bu uygulamalar için alternatif yapılandırmayı deneyebilirsiniz. 

Bu belirli uygulamalar için Microsoft Enterprise SSO eklentisini yapılandırmak için aşağıdaki parametreleri kullanın:

- **Anahtar**: `AppCookieSSOAllowList`
- **Şunu yazın**: `String`
- **Değer**: SSO 'ya katılmasına izin verilen uygulamalar için uygulama paket kimliği öneklerinin virgülle ayrılmış listesi. Bunun, belirli bir önekle başlayan tüm uygulamaların SSO 'ya katılmasını olanaklı olduğunu unutmayın.
- **Örnek**: `com.contoso.myapp1, com.fabrikam.myapp2`

Bu mekanizmayı kullanan SSO için etkinleştirilen uygulamaların hem hem de içine eklenmesi gerektiğini unutmayın `AppCookieSSOAllowList` `AppPrefixAllowList` .

Bu seçeneği yalnızca beklenmedik oturum açma hatalarıyla karşılaşan uygulamalar için denemeyi öneririz. 

#### <a name="use-intune-for-simplified-configuration"></a>Basitleştirilmiş yapılandırma için Intune 'U kullanma

Daha önce belirtildiği gibi, eklentiyi etkinleştirmek ve daha eski uygulamalarınızı SSO almak için izin verilenler listesine eklemek dahil olmak üzere Microsoft Enterprise SSO eklentisinin yapılandırılmasını kolaylaştırmak için MDM hizmeti olarak Microsoft Intune kullanabilirsiniz. Daha fazla bilgi için bkz. [Intune yapılandırma belgeleri](/intune/configuration/ios-device-features-settings).

## <a name="using-the-sso-plug-in-in-your-application"></a>Uygulamanızdaki SSO eklentisini kullanma

[Apple cihazları Için Microsoft kimlik doğrulama kitaplığı (msal)](https://github.com/AzureAD/microsoft-authentication-library-for-objc) sürüm 1.1.0 ve üzeri, Apple cihazları Için MICROSOFT Enterprise SSO eklentisini destekler. Microsoft Enterprise SSO eklentisi desteği eklemek için önerilen yoldur ve Microsoft Identity platformunun tüm yeteneklerini almanızı sağlar.

Frontline çalışan senaryoları için bir uygulama oluşturuyorsanız, özelliğin ek kurulumu için [iOS cihazları Için paylaşılan cihaz modu](msal-ios-shared-devices.md) ' na bakın.

## <a name="how-the-sso-plug-in-works"></a>SSO eklentisinin nasıl çalıştığı

Microsoft Enterprise SSO eklentisi [Apple 'ın Kurumsal tek Sign-On çerçevesini](https://developer.apple.com/documentation/authenticationservices/asauthorizationsinglesignonprovider?language=objc)kullanır. Çerçeveye eklenen kimlik sağlayıcıları, etki alanları için ağ trafiğini yakalayabilir ve bu isteklerin işlenme biçimini artırabilir veya değiştirebilir. Örneğin, SSO eklentisi Son Kullanıcı kimlik bilgilerini güvenli bir şekilde toplamak, MFA istemek veya uygulamaya sessizce belirteç sağlamak için ek kullanıcı arabirimi gösterebilir.

Yerel uygulamalar aynı zamanda özel işlemler uygulayabilir ve doğrudan SSO eklentisine konuşabilir.
[Apple 'dan bu 2019 WWDC videosunda](https://developer.apple.com/videos/play/tech-talks/301/) çoklu oturum açma çerçevesi hakkında bilgi edinebilirsiniz

### <a name="applications-that-use-a-microsoft-identity-platform-library"></a>Microsoft Identity platform kitaplığı kullanan uygulamalar

[Apple cihazları Için Microsoft kimlik doğrulama kitaplığı (msal)](https://github.com/AzureAD/microsoft-authentication-library-for-objc) sürüm 1.1.0 ve üzeri, Apple cihazları Için MICROSOFT Enterprise SSO eklentisini iş ve okul hesaplarında yerel olarak destekler. 

[Önerilen tüm adımları](./quickstart-v2-ios.md) izlediyseniz ve varsayılan [yeniden yönlendirme URI biçimini](./redirect-uris-ios.md)kullandıysanız, gerekli özel bir yapılandırma yoktur. SSO eklentisinin mevcut olduğu bir cihazda çalışırken, MSAL otomatik olarak tüm etkileşimli ve sessiz Belirteç istekleri için, hesap numaralandırması ve hesap kaldırma işlemleri için bu işlemi otomatik olarak çağırır. MSAL, özel işlemlere dayanan yerel SSO eklenti protokolünü gerçekleştirdiğinden, bu kurulum son kullanıcıya en iyi yerel deneyimi sağlar. 

SSO eklentisi MDM tarafından etkinleştirilmemişse, ancak cihazda Microsoft Authenticator uygulama mevcutsa, MSAL uygulamasını herhangi bir etkileşimli belirteç isteği için Microsoft Authenticator kullanacaktır. SSO eklentisi Microsoft Authenticator App ile SSO paylaşır.

### <a name="applications-that-dont-use-a-microsoft-identity-platform-library"></a>Microsoft Identity platform kitaplığı kullanmayan uygulamalar

MSAL gibi bir Microsoft Identity platform kitaplığı kullanmayan uygulamalar, yönetici tarafından izin verilenler listesine açıkça eklendiğinde SSO alabilir. 

Aşağıdaki koşullar karşılanmadığı sürece bu uygulamalarda kod değişikliği yapmanız gerekmez:

- Uygulama, ağ isteklerini yürütmek için Apple çerçeveleri kullanıyor (örneğin, [Wkwebview](https://developer.apple.com/documentation/webkit/wkwebview), [nsurlsession](https://developer.apple.com/documentation/foundation/nsurlsession)) 
- Uygulama, Azure AD ile iletişim kurmak için standart protokolleri kullanıyor (örneğin, OAuth2, SAML, WS-Federation)
- Uygulama yerel kullanıcı arabiriminde düz metin Kullanıcı adı ve parola toplamıyor

Bu durumda, uygulama bir ağ isteği oluşturduğunda ve Kullanıcı oturumu açmak üzere bir Web tarayıcısı açtığında SSO sağlanır. Bir Kullanıcı Azure AD oturum açma URL 'sine yeniden yönlendirildiğinde, SSO eklentisi URL 'YI doğrular ve bu URL için kullanılabilir SSO kimlik bilgisi olup olmadığını denetler. Bir tane varsa, SSO eklentisi SSO kimlik bilgisini Azure AD 'ye geçirir, bu da uygulamanın, kimlik bilgilerini girmesini istemeden ağ isteğini tamamlamasını yetkilenmektedir. Ayrıca, cihaz Azure AD ile biliniyorsa, SSO eklentisi cihaz sertifikasını cihaz tabanlı koşullu erişim denetimini karşılayacak şekilde de geçilecektir. 

MSAL olmayan uygulamalarda SSO 'yu desteklemek için, SSO eklentisi, [birincil yenileme belirteci nedir?](../devices/concept-primary-refresh-token.md#browser-sso-using-prt)bölümünde açıklanan Windows tarayıcı eklentisine benzer bir protokol uygular. 

MSAL tabanlı uygulamalarla karşılaştırıldığında, SSO eklentisi, uygulamaların sağladığı mevcut tarayıcı oturum açma deneyimiyle tümleştirerek MSAL olmayan uygulamalar için daha şeffaf bir şekilde davranır. Son Kullanıcı tanıdık deneyimini görebilir ve uygulamaların her birinde ek oturum açma işlemleri gerçekleştirmeme avantajına sahiptir. Örneğin, yerel hesap seçiciyi göstermek yerine, SSO eklentisi Web tabanlı hesap seçici deneyimine SSO oturumları ekler. 

## <a name="next-steps"></a>Sonraki adımlar

İOS 'ta paylaşılan cihaz modu hakkında daha fazla bilgi için bkz. [iOS cihazları Için paylaşılan cihaz modu](msal-ios-shared-devices.md).
