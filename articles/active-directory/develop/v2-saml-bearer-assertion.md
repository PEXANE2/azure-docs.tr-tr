---
title: Microsoft kimlik platformu & SAML taşıyıcı sıyrık iddiası akışı | Azure
description: SAML taşıyıcısı savşması açığını kullanarak kullanıcıyı kimlik bilgileri iStemden çıkmadan Microsoft Graph' tan verileri nasıl alınırıcını öğrenin.
services: active-directory
author: umeshbarapatre
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 1cd79b1f9e4cd3afadee250da0c184c0c5b8ac07
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886186"
---
# <a name="microsoft-identity-platform-and-oauth-20-saml-bearer-assertion-flow"></a>Microsoft kimlik platformu ve OAuth 2.0 SAML taşıyıcısı iddia akışı
OAuth 2.0 SAML taşıyıcı sıyrık akışı, bir istemcinin varolan bir güven ilişkisini kullanması gerektiğinde SAML iddiasını kullanarak Bir OAuth erişim belirteci istemenize olanak tanır. SAML iddiasıiçin uygulanan imza, yetkili uygulamanın kimlik doğrulamasını sağlar. SAML iddiası, bir kimlik sağlayıcısı tarafından verilen ve bir hizmet sağlayıcısı tarafından tüketilen bir XML güvenlik belirtecidir. Hizmet sağlayıcısı, iddianın konusunu güvenlikle ilgili amaçlarla tanımlamak için içeriğine güvenir.

SAML iddiası OAuth belirteci bitiş noktasına nakledilir.  Bitiş noktası, iddiayı işler ve uygulamanın önceden onaylanmasına dayalı bir erişim belirteci sağlar. İstemcinin yeni bir belirteci olması veya saklaması gerekmez, ne de istemci sırrının belirteç bitiş noktasına geçirilmesi gerekmez.

SAML Taşıyıcı Sataşması akışı, kullanıcıdan kimlik bilgileri istenmeden Microsoft Graph API'lerinden (yalnızca devredilen izinleri destekleyen) veri alırken yararlıdır. Bu senaryoda, arka plan işlemleri için tercih edilen istemci kimlik bilgileri hibesi çalışmaz.

SAML iddiasını almak için etkileşimli tarayıcı tabanlı oturum açma yapan ve ardından OAuth korumalı API'ye (Microsoft Graph gibi) erişim eklemek isteyen uygulamalar da, API için bir erişim belirteci almak için OAuth isteğinde bulunabilirsiniz. Tarayıcı, kullanıcının kimliğini doğrulamak için Azure AD'ye yönlendirildiğinde, tarayıcı oturumu SAML oturumundan alır ve kullanıcının kimlik bilgilerini girmesi gerekmez.

OAuth SAML Taşıyıcı Sıfatı akışı, Azure Active Directory'ye federe Active Directory Federation Services (ADFS) gibi kimlik sağlayıcılarıyla kimlik doğrulaması yapılan kullanıcılar için de desteklenir.  ADFS'den elde edilen SAML iddiası, kullanıcının kimliğini doğrulamak için Bir OAuth akışında kullanılabilir.

![OAuth akışı](./media/v2-saml-bearer-assertion/1.png)

## <a name="call-graph-using-saml-bearer-assertion"></a>SAML taşıyıcı sıfatını kullanarak Arama Grafiği
Şimdi saml iddiasını programlı olarak nasıl getirebileceğimizi anlayalım. Bu yaklaşım ADFS ile test edilir. Ancak, bu programlı SAML iddiasının dönüşünü destekleyen herhangi bir kimlik sağlayıcısı ile çalışır. Temel işlem şudur: BIR SAML iddiası almak, bir erişim belirteci almak ve Microsoft Graph erişin.

### <a name="prerequisites"></a>Ön koşullar

Yetkilendirme sunucusu/ortamı (Microsoft 365) ile kimlik sağlayıcısı veya SAML 2.0 taşıyıcı sıfatının (ADFS) vereni arasında bir güven ilişkisi kurun. ADFS'yi tek oturum açma ve kimlik sağlayıcısı olarak yapılandırmak için [bu makaleye](https://blogs.technet.microsoft.com/canitpro/2015/09/11/step-by-step-setting-up-ad-fs-and-enabling-single-sign-on-to-office-365/)başvurabilirsiniz.

Uygulamayı [portala](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)kaydedin:
1. [Portalın uygulama kayıt bıçağında](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) oturum açın (Grafik API için v2.0 uç noktalarını kullandığımızı ve dolayısıyla uygulamayı bu portala kaydetmemiz gerektiğini lütfen unutmayın. Aksi takdirde Azure etkin dizinindeki kayıtları kullanabilirdik). 
1. **Yeni kayıt**seçin.
1. Bir uygulama sayfası **kaydedin,** başvurunuzun kayıt bilgilerini girin: 
    1. **Ad** - Uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin.
    1. **Desteklenen hesap türleri** - Uygulamanızın desteklemesini istediğiniz hesapları seçin.
    1. **URI'yi yeniden yönlendir (isteğe bağlı)** - Oluşturmakta olduğunuz uygulama türünü, Web'i veya Kamu istemcisini (mobil & masaüstü) seçin ve ardından uygulamanız için yeniden yönlendirme URI'sini (veya yanıt URL'sini) girin.
    1. Bittiğinde **Kaydet**’i seçin.
1. Uygulama (istemci) kimliğini not edin.
1. Sol bölmede, **Sertifikalar & sırları**seçin. **İstemci sırları** bölümünde **Yeni istemci sırrını** tıklatın. Yeni istemci sırrını kopyalayın, bıçağı bıraktığınızda geri alamayacaksın.
1. Sol bölmede, **API izinlerini** seçin ve ardından **izin ekleyin.** Outlook Graph API'yi kullanmayı düşündüğümüzden **Microsoft Graph'ı**seçin, ardından **yetkinizin idesini**seçin ve **ardından Görevler.read'i** seçin. 

[Postacı](https://www.getpostman.com/)yükleyin , örnek isteklerini test etmek için gerekli bir araç.  Daha sonra, istekleri koda dönüştürebilirsiniz.

### <a name="get-the-saml-assertion-from-adfs"></a>ADFS'den SAML iddiasını alın
SAML iddiasını almak için SOAP zarfını kullanarak ADFS bitiş noktasına bir POST isteği oluşturun:

![SAML iddiasını alın](./media/v2-saml-bearer-assertion/2.png)

Üstbilgi değerleri:

![Üstbilgi değerleri](./media/v2-saml-bearer-assertion/3.png)

ADFS istek gövdesi:

![ADFS istek gövdesi](./media/v2-saml-bearer-assertion/4.png)

Bu istek başarıyla deftere nakledilen, ADFS'den bir SAML talebi almanız gerekir. Yalnızca **SAML:Assertion** etiket verileri gereklidir, daha fazla istekte kullanmak üzere base64 kodlamasına dönüştürün.

### <a name="get-the-oauth2-token-using-the-saml-assertion"></a>SAML iddiasını kullanarak OAuth2 belirteci alın 
Bu adımda, ADFS öne etme yanıtını kullanarak bir OAuth2 belirteci getirin.

1. Üstbilgi değerleri ile aşağıda gösterildiği gibi bir POST isteği oluşturun:

    ![POST isteği](./media/v2-saml-bearer-assertion/5.png)
1. İstek gövdesinde, **client_id**, **client_secret**ve **iddia** (base64 kodlanmış SAML iddiası önceki adımı elde) değiştirin:

    ![İstek gövdesi](./media/v2-saml-bearer-assertion/6.png)
1. Başarılı bir istek üzerine, Azure etkin dizininden bir erişim jetonu alırsınız.

### <a name="get-the-data-with-the-oauth-token"></a>Oauth belirteci ile verileri alın

Erişim jetonu aldıktan sonra Grafik API'lerini (bu örnekteki Outlook görevleri) arayın. 

1. Önceki adımda getirilen erişim belirteciyle bir GET isteği oluşturun:

    ![İstek AL](./media/v2-saml-bearer-assertion/7.png)

1. Başarılı bir istek üzerine, bir JSON yanıtı alacaksınız.

## <a name="next-steps"></a>Sonraki adımlar

Farklı kimlik [doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md)hakkında bilgi edinin.