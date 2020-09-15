---
title: Apple cihazlar için Microsoft Enterprise SSO eklentisi
titleSuffix: Microsoft identity platform | Azure
description: İOS ve macOS cihazları için Microsoft 'un Azure Active Directory SSO eklentisi hakkında bilgi edinin.
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
ms.openlocfilehash: e43ce318ca9e9b14ad059dd296799667653e0f95
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90561355"
---
# <a name="microsoft-enterprise-sso-plug-in-for-apple-devices-preview"></a>Apple cihazları için Microsoft Enterprise SSO eklentisi (Önizleme)

> [!NOTE]
> Bu özellik genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

*Apple cihazları Için Microsoft ENTERPRISE SSO eklentisi* , Apple 'ın [Kurumsal Çoklu oturum açma](https://developer.apple.com/documentation/authenticationservices) özelliğini destekleyen tüm uygulamalarda Azure Active Directory (Azure AD) hesapları için çoklu oturum açma (SSO) sağlar. Microsoft, Apple ve Microsoft 'un sağlayabilecekleri en iyi korumayı sağlarken uygulamanızın kullanılabilirliğini artırmak üzere bu eklentiyi geliştirmek üzere Apple ile yakın bir şekilde çalışmıştır.

Bu genel önizleme sürümünde, Enterprise SSO eklentisi yalnızca iOS cihazlarında kullanılabilir ve belirli Microsoft uygulamalarında dağıtılır.

## <a name="features"></a>Özellikler

Apple cihazları için Microsoft Enterprise SSO eklentisi aşağıdaki avantajları sunar:

- Apple 'ın Kurumsal Çoklu oturum açma özelliğini destekleyen tüm uygulamalarda Azure AD hesapları için SSO sağlar.
- Microsoft Authenticator otomatik olarak teslim edilebilir ve herhangi bir mobil cihaz yönetimi (MDM) çözümü tarafından etkinleştirilebilir.

## <a name="requirements"></a>Gereksinimler

Apple cihazları için Microsoft Enterprise SSO eklentisini kullanmak için:

- cihazda iOS 13,0 veya üzeri yüklü olmalıdır.
- Apple cihazları için Microsoft Enterprise SSO eklentisini sağlayan bir Microsoft uygulamasının cihazda yüklü olması gerekir. Genel önizleme için bu uygulamalar [Microsoft Authenticator uygulamasını](../user-help/user-help-auth-app-overview.md)içerir.
- Cihazın MDM ile kayıtlı olması gerekir (örneğin, Microsoft Intune ile).
- Cihazdaki Apple cihazları için Microsoft Enterprise SSO eklentisini etkinleştirmek üzere cihaza bir yapılandırma itilmiş olmalıdır. Bu güvenlik kısıtlaması Apple tarafından gerektirilir.

## <a name="enable-the-sso-plug-in-with-mobile-device-management-mdm"></a>Mobil cihaz yönetimi (MDM) ile SSO eklentisini etkinleştirme

Apple cihazları için Microsoft Enterprise SSO eklentisini etkinleştirmek üzere, cihazlarınızın MDM hizmeti aracılığıyla bir sinyal gönderilmesi gerekir. Microsoft, [Microsoft Authenticator](..//user-help/user-help-auth-app-overview.md)UYGULAMASıNDAKI Kurumsal SSO eklentisini de Içerdiğinden, MICROSOFT Enterprise SSO eklentisini etkinleştirmek üzere uygulamayı YAPıLANDıRMAK için MDM 'nizi kullanın.

Apple cihazları için Microsoft Enterprise SSO eklentisini yapılandırmak için aşağıdaki parametreleri kullanın:

- **Tür**: Redirect
- **UZANTı kimliği**: `com.microsoft.azureauthenticator.ssoextension`
- **Takım Kimliği**: (Bu alan iOS için gerekli değildir)
- **URL 'ler**:
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

#### <a name="enable-sso-for-apps-that-dont-use-msal"></a>MSAL kullanmayan uygulamalar için SSO 'yu etkinleştirme

SSO eklentisi, Microsoft kimlik doğrulama kitaplığı (MSAL) gibi bir Microsoft SDK kullanılarak geliştirmeseler bile, tüm uygulamaların çoklu oturum açma 'ya katılmasını sağlar.

SSO eklentisi, Microsoft Authenticator uygulamasını indirmiş ve cihazlarını kuruluşunuzla kaydolan cihazlar tarafından otomatik olarak yüklenir. Kuruluşunuz muhtemelen çok faktörlü kimlik doğrulaması, parola-daha az kimlik doğrulama ve koşullu erişim gibi senaryolar için kimlik doğrulayıcı uygulamasını günümüzde kullanıyor. Microsoft 'un Intune 'un Microsoft Uç Nokta Yöneticisi içinde yapılandırılması kolay olsa da, herhangi bir MDM sağlayıcısı kullanılarak uygulamalarınız için etkinleştirilebilir. Bu uygulamaları, kimlik doğrulayıcı uygulaması tarafından yüklenen SSO eklentisini kullanacak şekilde yapılandırmak için bir izin verilenler listesi kullanılır.

Yalnızca yerel Apple ağ teknolojilerini veya Web görünümlerini kullanan uygulamalar desteklenir. Bir uygulama kendi ağ katmanı uygulamasını kullanıyorsa, Microsoft Enterprise SSO eklentisi desteklenmez.  

MSAL kullanmayan uygulamalar için Microsoft Enterprise SSO eklentisini yapılandırmak için aşağıdaki parametreleri kullanın:

- **Anahtar**: `AppAllowList`
- **Şunu yazın**: `String`
- **Değer**: SSO 'ya katılmasına izin verilen uygulamalar için uygulama paket kimliklerinin virgülle ayrılmış listesi
- **Örnek**: `com.contoso.workapp, com.contoso.travelapp`

MDM yöneticisinin SSO 'ya katılması için izin verilen [uygulamalar](./application-consent-experience.md) , son kullanıcı için sessizce bir belirteç alabilir. Bu nedenle, yalnızca izin verilenler listesine güvenilen uygulamalar eklemek önemlidir. 

Bu listeye MSAL veya ASWebAuthenticationSession kullanan uygulamalar eklemeniz gerekmez. Bu uygulamalar varsayılan olarak etkinleştirilmiştir. 

#### <a name="allow-creating-sso-session-from-any-application"></a>Herhangi bir uygulamadan SSO oturumu oluşturulmasına izin ver

Varsayılan olarak, Microsoft Enterprise SSO eklentisi, yalnızca SSO eklentisinin zaten paylaşılan bir kimlik bilgisi olduğunda yetkilendirilmiş uygulamalar için SSO sağlar. Microsoft Enterprise SSO eklentisi, belirteç alma sırasında başka bir ADAL veya MSAL tabanlı uygulama tarafından çağrıldığında paylaşılan bir kimlik bilgisi alabilir. Microsoft uygulamalarının çoğu Microsoft Authenticator veya SSO eklentisini kullanır. Bu, varsayılan olarak yerel uygulama akışları dışında SSO 'nun en iyi çaba olduğunu gösterir.  

`browser_sso_interaction_enabled`Bayrak etkinleştirme, msal olmayan uygulamalara ve Safari tarayıcısına ilk önyüklemeyi ve paylaşılan bir kimlik bilgisi almasını sağlar. Microsoft Enterprise SSO eklentisinin paylaşılan bir kimlik bilgisi yoksa, Safari tarayıcısı, ASWebAuthenticationSession, SafariViewController veya diğer bir beyaz listelenmiş yerel uygulama içindeki bir Azure AD URL 'sinden her oturum açma işlemi yapıldığında bir tane almaya çalışır.  

- **Anahtar**: `browser_sso_interaction_enabled`
- **Şunu yazın**: `Integer`
- **Değer**: 1 veya 0

Tüm uygulamalar üzerinde daha tutarlı bir deneyim sağlamak için bu bayrağın etkinleştirilmesini öneririz. Varsayılan olarak devre dışıdır. 

#### <a name="disable-oauth2-application-prompts"></a>OAuth2 uygulama istemlerini devre dışı bırak

Microsoft Enterprise SSO eklentisi, izin verilen uygulamalardan gelen ağ isteklerine paylaşılan kimlik bilgilerini ekleyerek SSO sağlar. Bazı OAuth2 uygulamaları, protokol katmanında Son Kullanıcı istemi 'ni zorlamaya yönelik olabilir. Paylaşılan kimlik bilgileri bu uygulamalar için yok sayılacak.  

`disable_explicit_app_prompt`Bayrak etkinleştirme, hem yerel hem de Web uygulamalarının, protokol katmanında Son Kullanıcı istemi 'ni zorunlu hale getirme ve SSO atlama olanağını kısıtlar.

- **Anahtar**: `disable_explicit_app_prompt`
- **Şunu yazın**: `Integer`
- **Değer**: 1 veya 0

Tüm uygulamalar üzerinde daha tutarlı bir deneyim sağlamak için bu bayrağın etkinleştirilmesini öneririz. Varsayılan olarak devre dışıdır. 

#### <a name="use-intune-for-simplified-configuration"></a>Basitleştirilmiş yapılandırma için Intune 'U kullanma

Microsoft Enterprise SSO eklentisinin yapılandırılmasını kolaylaştırmak için MDM hizmetiniz olarak Microsoft Intune kullanabilirsiniz. Daha fazla bilgi için bkz. [Intune yapılandırma belgeleri](/intune/configuration/ios-device-features-settings).

## <a name="using-the-sso-plug-in-in-your-application"></a>Uygulamanızdaki SSO eklentisini kullanma

[Apple cihazları Için Microsoft kimlik doğrulama kitaplığı (msal)](https://github.com/AzureAD/microsoft-authentication-library-for-objc) sürüm 1.1.0 ve üzeri, Apple cihazları Için MICROSOFT Enterprise SSO eklentisini destekler.

Frontline çalışan senaryoları için bir uygulama oluşturuyorsanız, özelliğin ek kurulumu için [iOS cihazları Için paylaşılan cihaz modu](msal-ios-shared-devices.md) ' na bakın.

## <a name="how-the-sso-plug-in-works"></a>SSO eklentisinin nasıl çalıştığı

Microsoft Enterprise SSO eklentisi [Apple 'ın Kurumsal Çoklu oturum açma çerçevesini](https://developer.apple.com/documentation/authenticationservices/asauthorizationsinglesignonprovider?language=objc)kullanır. Çerçeveye eklenen kimlik sağlayıcıları, etki alanları için ağ trafiğini yakalayabilir ve bu isteklerin işlenme biçimini artırabilir veya değiştirebilir. Örneğin, SSO eklentisi Son Kullanıcı kimlik bilgilerini güvenli bir şekilde toplamak, MFA istemek veya uygulamaya sessizce belirteç sağlamak için ek kullanıcı arabirimi gösterebilir.

Yerel uygulamalar aynı zamanda özel işlemler uygulayabilir ve doğrudan SSO eklentisine konuşabilir.
[Apple 'dan bu 2019 WWDC videosunda](https://developer.apple.com/videos/play/tech-talks/301/) çoklu oturum açma çerçevesi hakkında bilgi edinebilirsiniz

### <a name="applications-that-use-msal"></a>MSAL kullanan uygulamalar

[Apple cihazları Için Microsoft kimlik doğrulama kitaplığı (msal)](https://github.com/AzureAD/microsoft-authentication-library-for-objc) sürüm 1.1.0 ve üzeri, Apple cihazları Için MICROSOFT Enterprise SSO eklentisini iş ve okul hesaplarında yerel olarak destekler. 

[Önerilen tüm adımları](./quickstart-v2-ios.md) izlediyseniz ve varsayılan [yeniden yönlendirme URI biçimini](./redirect-uris-ios.md)kullandıysanız, gerekli özel bir yapılandırma yoktur. SSO eklentisinin mevcut olduğu bir cihazda çalışırken, MSAL otomatik olarak tüm etkileşimli ve sessiz Belirteç istekleri için, hesap numaralandırması ve hesap kaldırma işlemleri için bu işlemi otomatik olarak çağırır. MSAL, özel işlemlere dayanan yerel SSO eklenti protokolünü gerçekleştirdiğinden, bu kurulum son kullanıcıya en iyi yerel deneyimi sağlar. 

SSO eklentisi MDM tarafından etkinleştirilmemişse, ancak cihazda Microsoft Authenticator uygulama mevcutsa, MSAL uygulamasını herhangi bir etkileşimli belirteç isteği için Microsoft Authenticator kullanacaktır. SSO eklentisi Microsoft Authenticator App ile SSO paylaşır.

### <a name="applications-that-dont-use-msal"></a>MSAL kullanmayan uygulamalar

MSAL kullanmayan uygulamalar, yönetici tarafından izin verilenler listesine açıkça eklerse SSO alabilir. 

Aşağıdaki koşullar karşılanmadığı sürece bu uygulamalarda kod değişikliği yapmanız gerekmez:

- Uygulama, ağ isteklerini yürütmek için Apple çerçeveleri kullanıyor (örneğin, [Wkwebview](https://developer.apple.com/documentation/webkit/wkwebview), [nsurlsession](https://developer.apple.com/documentation/foundation/nsurlsession)) 
- Uygulama, Azure AD ile iletişim kurmak için standart protokolleri kullanıyor (örneğin, OAuth2, SAML, WS-Federation)
- Uygulama yerel kullanıcı arabiriminde düz metin Kullanıcı adı ve parola toplamıyor

Bu durumda, uygulama bir ağ isteği oluşturduğunda ve Kullanıcı oturumu açmak üzere bir Web tarayıcısı açtığında SSO sağlanır. Bir Kullanıcı Azure AD oturum açma URL 'sine yeniden yönlendirildiğinde, SSO eklentisi URL 'YI doğrular ve bu URL için kullanılabilir SSO kimlik bilgisi olup olmadığını denetler. Bir tane varsa, SSO eklentisi SSO kimlik bilgisini Azure AD 'ye geçirir, bu da uygulamanın, kimlik bilgilerini girmesini istemeden ağ isteğini tamamlamasını yetkilenmektedir. Ayrıca, cihaz Azure AD ile biliniyorsa, SSO eklentisi cihaz sertifikasını cihaz tabanlı koşullu erişim denetimini karşılayacak şekilde de geçilecektir. 

MSAL olmayan uygulamalarda SSO 'yu desteklemek için, SSO eklentisi, [birincil yenileme belirteci nedir?](../devices/concept-primary-refresh-token.md#browser-sso-using-prt)bölümünde açıklanan Windows tarayıcı eklentisine benzer bir protokol uygular. 

MSAL tabanlı uygulamalarla karşılaştırıldığında, SSO eklentisi, uygulamaların sağladığı mevcut tarayıcı oturum açma deneyimiyle tümleştirerek MSAL olmayan uygulamalar için daha şeffaf bir şekilde davranır. Son Kullanıcı tanıdık deneyimini görebilir ve uygulamaların her birinde ek oturum açma işlemleri gerçekleştirmeme avantajına sahiptir. Örneğin, yerel hesap seçiciyi göstermek yerine, SSO eklentisi Web tabanlı hesap seçici deneyimine SSO oturumları ekler. 

## <a name="next-steps"></a>Sonraki adımlar

İOS 'ta paylaşılan cihaz modu hakkında daha fazla bilgi için bkz. [iOS cihazları Için paylaşılan cihaz modu](msal-ios-shared-devices.md).
