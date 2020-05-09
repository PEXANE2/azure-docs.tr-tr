---
title: Azure Active Directory B2C 'de SSO oturumu | Microsoft Docs
description: Azure Active Directory B2C 'de oturum davranışını yapılandırın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ea8c40faad4ee709ae98f868e36fd42e46501bea
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82927046"
---
# <a name="azure-ad-b2c-session"></a>Azure AD B2C oturumu

Çoklu oturum açma (SSO), kullanıcılar Azure Active Directory B2C uygulamalarda (Azure AD B2C) oturum açtığında güvenlik ve kolaylık sağlar. Bu makalede, Azure AD B2C kullanılan çoklu oturum açma yöntemleri açıklanmakta ve ilkenizi yapılandırırken en uygun SSO yöntemini seçmenize yardımcı olur.

Çoklu oturum açma ile kullanıcılar tek bir hesapla oturum açıp birden çok uygulamaya erişim sağlar. Uygulama, platform veya etki alanı adından bağımsız olarak bir Web, mobil veya tek sayfalı uygulama olabilir.

Kullanıcı başlangıçta bir uygulamaya oturum açtığında, Azure AD B2C tanımlama bilgisi tabanlı bir oturumu devam ettirir. Sonraki kimlik doğrulama istekleri üzerine Azure AD B2C, tanımlama bilgisi tabanlı oturumu okuyup doğrular ve kullanıcıdan tekrar oturum açmasını istemeden bir erişim belirteci yayınlar. Tanımlama bilgisi tabanlı oturumun süresi dolarsa veya geçersiz olursa kullanıcıdan tekrar oturum açması istenir.  

## <a name="sso-session-types"></a>SSO oturum türleri

Azure AD B2C tümleştirme üç tür SSO oturumu içerir:

- **Azure AD B2C** -oturum Azure AD B2C tarafından yönetiliyor
- **Federal kimlik sağlayıcısı** -oturum, kimlik sağlayıcısı tarafından yönetilen, örneğin Facebook, Salesforce veya Microsoft hesabı
- Web, mobil veya tek sayfalı uygulama tarafından yönetilen **uygulama** -oturum

![SSO oturumu](media/session-overview/sso-session-types.png)

### <a name="azure-ad-b2c-sso"></a>Azure AD B2C SSO 'SU 

Bir kullanıcı yerel veya sosyal hesapla kimliğini başarıyla doğruladığında, Azure AD B2C kullanıcının tarayıcısına tanımlama bilgisi tabanlı bir oturum depolar. Tanımlama bilgisi, gibi Azure AD B2C kiracı etki alanı adı altında depolanır `https://contoso.b2clogin.com`.

Bir kullanıcı başlangıçta bir Federasyon hesabıyla oturum açarsa ve oturum zaman penceresi (yaşam süresi veya TTL) sırasında aynı uygulamada veya farklı bir uygulamada oturum açarsa, federasyon kimliği sağlayıcısından yeni bir erişim belirteci almaya çalışır Azure AD B2C. Federal Kimlik sağlayıcısı oturumunun geçerliliği dolmuşsa veya geçersizse, federal kimlik sağlayıcısı kullanıcıdan kimlik bilgilerini ister. Oturum hala etkin ise (veya kullanıcı federe hesap yerine yerel bir hesapla oturum açmışsa), Azure AD B2C kullanıcıyı yetkilendirir ve diğer istemleri ortadan kaldırır.

Oturum TTL 'sini ve Azure AD B2C ilkeleri ve uygulamalar arasında oturumu nasıl paylaştığını de içeren oturum davranışını yapılandırabilirsiniz.

### <a name="federated-identity-provider-sso"></a>Federal Kimlik sağlayıcısı SSO 'SU

Bir sosyal veya kurumsal kimlik sağlayıcısı kendi oturumunu yönetir. Tanımlama bilgisi, kimlik sağlayıcısının etki alanı adı altında depolanır `https://login.salesforce.com`. Azure AD B2C, federal kimlik sağlayıcısı oturumunu denetlemez. Bunun yerine, oturum davranışı federal kimlik sağlayıcısı tarafından belirlenir. 

Şu senaryoyu göz önünde bulundurun:

1. Kullanıcı, akışını denetlemek için Facebook 'ta oturum açar.
2. Daha sonra Kullanıcı, uygulamanızı açar ve oturum açma işlemini başlatır. Uygulama, oturum açma işlemini tamamlaması için kullanıcıyı Azure AD B2C yönlendirir.
3. Azure AD B2C kaydolma veya oturum açma sayfasında Kullanıcı, Facebook hesabıyla oturum açmayı seçer. Kullanıcı Facebook 'a yönlendirilir. Facebook 'ta etkin bir oturum varsa, kullanıcıdan kimlik bilgilerini sağlaması istenmez ve Facebook belirtecine sahip Azure AD B2C hemen yeniden yönlendirilir.

### <a name="application-sso"></a>Uygulama SSO 'SU

Web, mobil veya tek sayfalı bir uygulama, OAuth erişimi, KIMLIK belirteçleri veya SAML belirteçleri tarafından korunabilir. Bir Kullanıcı uygulamadaki korumalı bir kaynağa erişmeyi denediğinde uygulama tarafında etkin bir oturum olup olmadığını denetler. Uygulama oturumu yoksa veya oturumun süresi dolmuşsa, uygulama oturum açma sayfasına Azure AD B2C kullanıcıyı alır.

Uygulama oturumu, uygulama etki alanı adı altında depolanan tanımlama bilgisi tabanlı bir oturum olabilir, örneğin `https://contoso.com`. Mobil uygulamalar, oturumu farklı bir şekilde saklayabilir, ancak benzer bir yaklaşım kullanabilir.

## <a name="azure-ad-b2c-session-configuration"></a>Azure AD B2C oturum yapılandırması

### <a name="session-scope"></a>Oturum kapsamı

Azure AD B2C oturumu aşağıdaki kapsamlar ile yapılandırılabilir:

- **Kiracı** -Bu ayar varsayılan ayardır. Bu ayarın kullanılması, B2C kiracınızdaki birden fazla uygulamanın ve Kullanıcı akışının aynı kullanıcı oturumunu paylaşmasına izin verir. Örneğin, bir Kullanıcı bir uygulamada oturum açtıktan sonra, Kullanıcı da erişimi daha da sorunsuz bir şekilde oturum açabilir.
- **Uygulama** -Bu ayar, başka uygulamalardan bağımsız olarak bir uygulama için Kullanıcı oturumunu korumanıza olanak sağlar. Örneğin, kullanıcının contoso Market 'te zaten oturum açmış olup olmamasına bakılmaksızın kullanıcının contoso Ilaç 'da oturum açmasını istiyorsanız bu ayarı kullanabilirsiniz.
- **İlke** -Bu ayar, bir kullanıcı oturumunun, kendisini kullanan uygulamalardan bağımsız olarak bir Kullanıcı akışı için bakımını yapmanıza olanak sağlar. Örneğin, Kullanıcı zaten oturum açmışsa ve bir Multi-Factor Authentication (MFA) adımını tamamlamışsa, Kullanıcı akışına bağlı olan oturumun süresi dolana kadar, kullanıcıya birden çok uygulamanın daha yüksek güvenlik bölümlerine erişim verilebilir.
- **Devre dışı** -Bu ayar, kullanıcıyı ilkenin her yürütülmesinden sonra Kullanıcı akışının tamamı boyunca çalışmaya zorlar.

### <a name="session-life-time"></a>Oturum yaşam süresi

**Oturum yaşam süresi** , kimlik doğrulamasının başarılı olmasından sonra Azure AD B2C oturum tanımlama bilgisinin kullanıcının tarayıcısına depolanacağı zaman miktarıdır. Oturum yaşam süresini 15 ile 720 dakika arasında bir değere ayarlayabilirsiniz.

### <a name="keep-me-signed-in"></a>Oturumum açık kalsın

Oturumumu [açık tut](custom-policy-keep-me-signed-in.md) özelliği, oturum ömrünü kalıcı bir tanımlama bilgisinin kullanımıyla uzatır. Kullanıcı tarayıcıyı kapatıp yeniden açtıktan sonra oturum etkin kalır. Oturum yalnızca Kullanıcı oturumunu kapattığında iptal edilir. Oturumumu Açık tut özelliği yalnızca yerel hesaplarla oturum açmak için geçerlidir.

Oturumumu Açık tut özelliği, oturum kullanım zamanına göre önceliklidir. Oturumumu Açık bırak özelliği etkinse ve Kullanıcı onu seçerse, bu özellik oturumun ne zaman sona ereceğini belirler. 

### <a name="session-expiry-type"></a>Oturum süre sonu türü

**Oturum süre sonu türü** , bir oturumun oturum yaşam süresi ayarı veya Oturumumu Açık tut ayarıyla nasıl uzatıldığını gösterir.

- Sıralı **-kullanıcının** tanımlama bilgisi tabanlı kimlik doğrulaması (varsayılan) her gerçekleştirdiğinde oturumun genişletildiğini gösterir.
- **Mutlak** -kullanıcının belirtilen süre sonunda yeniden kimlik doğrulamasına zorlandığını gösterir.

## <a name="sign-out"></a>Oturumu kapatma

Kullanıcının uygulamadan oturum açmasını istediğinizde, uygulamanın tanımlama bilgilerini temizlemek veya oturumu Kullanıcı ile sonlandırmak yeterli değildir. Oturumu kapatmak için kullanıcıyı Azure AD B2C yeniden yönlendirmeniz gerekir. Aksi takdirde, Kullanıcı, kimlik bilgilerini tekrar girmeden uygulamalarınızda yeniden kimlik doğrulaması yapabilir.

Bir oturum kapatma isteği sonrasında Azure AD B2C:

1. Azure AD B2C tanımlama bilgisine dayalı oturumu geçersiz kılar.
1. Federal Kimlik sağlayıcılarından oturum açmaya çalışır:
   - OpenID Connect-kimlik sağlayıcısı iyi bilinen yapılandırma uç noktası bir `end_session_endpoint` konum belirtiyorsa.
   - SAML-kimlik sağlayıcısı meta verileri `SingleLogoutService` konumu içeriyorsa.
1. İsteğe bağlı olarak, diğer uygulamalardan oturum kapatır. Daha fazla bilgi için bkz. [Çoklu oturum kapatma](#single-sign-out) bölümü.

Oturum açma, kullanıcının çoklu oturum açma durumunu Azure AD B2C temizler, ancak kullanıcıyı sosyal kimlik sağlayıcısı oturumunun dışında imzalayamayabilir. Kullanıcı sonraki oturum açma sırasında aynı kimlik sağlayıcısını seçerse, kimlik bilgilerini girmeden yeniden kimlik doğrulaması sağlayabilir. Kullanıcı uygulamanın oturumunu kapatmak isterse, Facebook hesabında oturumu kapatmak istedikleri anlamına gelmez. Ancak, yerel hesaplar kullanılıyorsa, kullanıcının oturumu doğru şekilde sona erer.

### <a name="single-sign-out"></a>Çoklu oturum kapatma 


> [!NOTE]
> Bu özellik [özel ilkelerle](custom-policy-overview.md)sınırlıdır.

Kullanıcıyı Azure AD B2C oturum kapatma uç noktasına yönlendirirsiniz (hem OAuth2 hem de SAML protokolleri için), Azure AD B2C kullanıcının oturumunu tarayıcıdan temizler. Ancak Kullanıcı, kimlik doğrulaması için Azure AD B2C kullanan diğer uygulamalarda oturum açmış olabilir. Bu uygulamaların kullanıcıyı aynı anda imzalamasını sağlamak için, Azure AD B2C kullanıcının oturum açmış olduğu tüm uygulamalara kayıtlı `LogoutUrl` BIR http get isteği gönderir.


Uygulamalar, kullanıcıyı tanıtan ve `200` yanıt döndüren tüm oturumları temizleyerek bu isteğe yanıt vermelidir. Uygulamanızda çoklu oturum açmayı desteklemek istiyorsanız, uygulamanızın kodunda bir `LogoutUrl` uygulamanız gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcı akışındaki oturum davranışını yapılandırmayı](session-behavior.md)öğrenin.
- [Özel ilkelerde oturum davranışını yapılandırmayı](session-behavior-custom-policy.md)öğrenin.