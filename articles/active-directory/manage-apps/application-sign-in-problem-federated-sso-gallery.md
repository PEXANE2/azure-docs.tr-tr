---
title: Federasyon çoklu oturum açma Galeri uygulamasında oturum açma sorunları | Microsoft Docs
description: Azure AD ile SAML tabanlı federe çoklu oturum açma için yapılandırdığınız bir uygulamada oturum açarken oluşan belirli hatalara yönelik yönergeler
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 02/18/2019
ms.author: kenwith
ms.reviewer: luleon, asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c2dc73038151297952dc208031b4a3b6dbcf146
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84759326"
---
# <a name="problems-signing-in-to-a-gallery-application-configured-for-federated-single-sign-on"></a>Federasyon çoklu oturum açma için yapılandırılmış galeri uygulamasında oturum açma soruları

Aşağıdaki oturum açma sorunlarını gidermek için aşağıdaki öneriyi izleyerek daha iyi tanılama ve çözüm adımlarını otomatikleştirmeniz önerilir:

- Azure portal Test deneyimini kullanırken daha iyi tanılama ve çözümler sağlamak üzere Azure Active Directory (Azure AD) yardımcı olmak için [uygulamalarım güvenli tarayıcı uzantısını](access-panel-extension-problem-installing.md) yüklemek.
- Azure portal, uygulama yapılandırma sayfasında Test deneyimini kullanarak hatayı yeniden oluşturun. [SAML tabanlı çoklu oturum açma uygulamalarında hata ayıklama](../azuread-dev/howto-v1-debug-saml-sso-issues.md) hakkında daha fazla bilgi edinin


## <a name="application-not-found-in-directory"></a>Uygulama dizinde bulunamadı

*Hata AADSTS70001: ' https: \/ /contoso.com ' tanımlayıcısına sahip uygulama dizinde bulunamadı*.

**Olası nedeni**

`Issuer`UYGULAMANıN SAML Isteğindeki Azure AD 'ye gönderdiği öznitelik, Azure AD 'de uygulama için yapılandırılmış tanımlayıcı değeriyle eşleşmiyor.

**Çözünürlüğüne**

`Issuer`SAML isteğindeki özniteliğinin Azure AD 'de yapılandırılan tanımlayıcı değeri ile eşleştiğinden emin olun. Uygulamalarım güvenli tarayıcı uzantısı ile Azure portal [Test deneyimini](../azuread-dev/howto-v1-debug-saml-sso-issues.md) kullanıyorsanız, bu adımları el ile izlemeniz gerekmez.

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici**olarak oturum açın.

1.  Ana sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i seçerek **Azure Active Directory uzantısını** açın.

1.  Filtre arama kutusuna **"Azure Active Directory"** yazın ve **Azure Active Directory** öğesini seçin.

1.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' ı seçin.

1.  Tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' ı seçin.

    Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar**olarak ayarlayın.

1.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

1.  Uygulama yüklendikten sonra **Temel SAML yapılandırması**'nı açın. Tanımlayıcı metin kutusundaki değerin hatada görüntülenen tanımlayıcı değeri için olan değerle eşleştiğini doğrulayın.



## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>Yanıt adresi, uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor

*Hata AADSTS50011: yanıt adresi ' https: \/ /contoso.com ', uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor*

**Olası nedeni**

`AssertionConsumerServiceURL`SAML isteğindeki değer, Azure AD 'de yapılandırılan yanıt URL 'si değeri veya düzeniyle eşleşmiyor. `AssertionConsumerServiceURL`SAML isteğindeki değeri, hatada gördüğünüz URL 'dir.

**Çözünürlüğüne**

`AssertionConsumerServiceURL`SAML isteğindeki değerin Azure AD 'de yapılandırılan yanıt URL 'si değeri ile eşleştiğinden emin olun. Uygulamalarım güvenli tarayıcı uzantısı ile Azure portal [Test deneyimini](../azuread-dev/howto-v1-debug-saml-sso-issues.md) kullanıyorsanız, bu adımları el ile izlemeniz gerekmez.

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici**olarak oturum açın.

1.  Ana sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i seçerek **Azure Active Directory uzantısını** açın.

1.  Filtre arama kutusuna **"Azure Active Directory"** yazın ve **Azure Active Directory** öğesini seçin.

1.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' ı seçin.

1.  Tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' ı seçin.

    Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar**olarak ayarlayın.

1.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

1.  Uygulama yüklendikten sonra **Temel SAML yapılandırması**'nı açın. Yanıt URL 'SI metin kutusundaki değeri SAML isteğindeki değerle eşleşecek şekilde doğrulayın veya güncelleştirin `AssertionConsumerServiceURL` .    
    
Azure AD 'de yanıt URL 'SI değerini güncelleştirdikten ve SAML isteğindeki uygulama tarafından gönderilen değeri eşleştirdikten sonra, uygulamada oturum açabiliyor olmanız gerekir.

## <a name="user-not-assigned-a-role"></a>Kullanıcıya bir rol atanmamış

*Hata AADSTS50105: oturum açan ' brian \@ contoso.com ' kullanıcısı uygulama için bir role atanmamış*.

**Olası nedeni**

Azure AD'de kullanıcıya uygulama için erişim verilmedi.​

**Çözünürlüğüne**

Bir uygulamaya doğrudan bir veya daha fazla kullanıcı atamak için aşağıdaki adımları izleyin. Uygulamalarım güvenli tarayıcı uzantısı ile Azure portal [Test deneyimini](../azuread-dev/howto-v1-debug-saml-sso-issues.md) kullanıyorsanız, bu adımları el ile izlemeniz gerekmez.

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici**olarak oturum açın.

1.  Ana sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i seçerek **Azure Active Directory uzantısını** açın.

1.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

1.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' ı seçin.

1.  Tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' ı seçin.

    Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar**olarak ayarlayın.

1.  Uygulama listesinden, kullanıcı atamak istediğiniz birini seçin.

1.  Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Kullanıcılar ve gruplar** ' ı seçin.

1.  **Atama Ekle** bölmesini açmak için **Kullanıcılar ve gruplar** listesinin üstündeki **Ekle** düğmesine tıklayın.

1.  **Atama Ekle** bölmesinden **Kullanıcılar ve gruplar** seçicisini seçin.

1. **Ad veya e-posta adresiyle ara** arama kutusuna, eklemek istediğiniz kullanıcının tam adını veya e-posta adresini yazın.

1. Bir **onay kutusunu**açığa çıkarmak için listedeki **kullanıcının** üzerine gelin. Kullanıcıyı **Seçili** listeye eklemek için kullanıcının profil fotoğrafı veya logosu yanındaki onay kutusuna tıklayın.

1. **Isteğe bağlı:** Birden **fazla kullanıcı eklemek**istiyorsanız **ada veya e-posta adresine göre ara** kutusuna başka bir tam ad veya e-posta adresi yazın ve kullanıcıyı **Seçili** listeye eklemek için onay kutusuna tıklayın.

1. Kullanıcıları seçmeyi tamamladığınızda, uygulamayı atanacak kullanıcılar ve gruplar listesine eklemek için **Seç** düğmesine tıklayın.

1. **Isteğe bağlı:** Seçtiğiniz kullanıcılara atanacak bir rol seçmek için **atama Ekle** bölmesinde **rol seçicisini Seç** ' e tıklayın.

1. Uygulamayı seçili kullanıcılara atamak için **ata** düğmesine tıklayın.

Kısa bir süre sonra, seçtiğiniz kullanıcılar çözüm açıklaması bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatabilecektir.

## <a name="not-a-valid-saml-request"></a>Geçerli bir SAML isteği değil

*Hata AADSTS75005: istek geçerli bir SAML2 protokol iletisi değil.*

**Olası nedeni**

Azure AD, uygulama tarafından çoklu oturum açma için gönderilen SAML isteğini desteklemiyor. Yaygın sorunlardan bazıları:

-   SAML isteğinde gerekli alanlar eksik
-   SAML isteği kodlama yöntemi

**Çözünürlüğüne**

1. SAML isteğini yakalayın. SAML isteğini nasıl yakalayacağınızı öğrenmek için [Azure AD 'de uygulamalar IÇIN SAML tabanlı çoklu oturum açma hatalarını ayıklama](../azuread-dev/howto-v1-debug-saml-sso-issues.md) öğreticisini izleyin.

1. Uygulama satıcısıyla iletişim kurun ve onlarla aşağıdaki bilgileri paylaşın:

   -   SAML isteği

   -   [Azure AD çoklu oturum açma SAML protokolü gereksinimleri](../develop/single-sign-on-saml-protocol.md)

Uygulama satıcısı, çoklu oturum açma için Azure AD SAML uygulamasını destekledikleri doğrulaması gerekir.

## <a name="misconfigured-application"></a>Yanlış yapılandırılmış uygulama

*Hata AADSTS650056: yanlış yapılandırılmış uygulama. Bunun nedeni aşağıdakilerden biri olabilir: istemci, istemcinin uygulama kaydında istenen izinlerde herhangi bir izin listelenmemiştir. Ya da yönetici kiracıya onaylamadı. Ya da, yapılandırılan istemci uygulama tanımlayıcısıyla eşleştiğinden emin olmak için istekteki uygulama tanımlayıcısını kontrol edin. Kiracı adına yapılandırmayı veya onayı onarmak için lütfen yöneticinize başvurun.*

**Olası nedeni**

`Issuer`UYGULAMANıN SAML Isteğindeki Azure AD 'ye gönderdiği öznitelik, Azure AD 'de uygulama için yapılandırılan tanımlayıcı değeriyle eşleşmiyor.

**Çözünürlüğüne**

`Issuer`SAML isteğindeki özniteliğinin Azure AD 'de yapılandırılan tanımlayıcı değeri ile eşleştiğinden emin olun. Uygulamalarım güvenli tarayıcı uzantısı ile Azure portal [Test deneyimini](../azuread-dev/howto-v1-debug-saml-sso-issues.md) kullanıyorsanız, aşağıdaki adımları el ile izlemeniz gerekmez:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici**olarak oturum açın.

1.  Ana sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i seçerek **Azure Active Directory uzantısını** açın.

1.  Filtre arama kutusuna **"Azure Active Directory"** yazın ve **Azure Active Directory** öğesini seçin.

1.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' ı seçin.

1.  Tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' ı seçin.

    Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar**olarak ayarlayın.

1.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

1.  Uygulama yüklendikten sonra **Temel SAML yapılandırması**'nı açın. Tanımlayıcı metin kutusundaki değerin hatada görüntülenen tanımlayıcı değeri için olan değerle eşleştiğini doğrulayın.


## <a name="certificate-or-key-not-configured"></a>Sertifika veya anahtar yapılandırılmadı

*Hata AADSTS50003: imzalama anahtarı yapılandırılmadı.*

**Olası nedeni**

Uygulama nesnesi bozuk ve Azure AD uygulama için yapılandırılan sertifikayı tanımıyor.

**Çözünürlüğüne**

Yeni bir sertifika silmek ve oluşturmak için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici**olarak oturum açın.

1. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

1. Filtre arama kutusuna **"Azure Active Directory"** yazın ve **Azure Active Directory** öğesini seçin.

1. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' ı seçin.

1. Tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' ı seçin.

    Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar**olarak ayarlayın.

1. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin

1. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

1. **SAML Imzalama sertifikası** bölümünde **Yeni sertifika oluştur** ' u seçin.

1. Sona erme tarihi ' ni seçin ve **Kaydet**' e tıklayın.

1. Etkin sertifikayı geçersiz kılmak için **Yeni sertifika etkin hale getirin** ' i işaretleyin. Ardından bölmenin üstündeki **Kaydet**’e tıklayın ve geçiş sertifikasını etkinleştirmeyi kabul edin.

1. **Kullanılmayan** sertifikayı kaldırmak Için **SAML imzalama sertifikası** bölümünde **Kaldır** ' ı tıklatın.

## <a name="saml-request-not-present-in-the-request"></a>İstekte SAML Isteği yok

*Hata AADSTS750054: SAML yeniden yönlendirme bağlaması için HTTP isteğinde sorgu dizesi parametreleri olarak SAMLRequest veya SAMLResponse var olmalıdır.*

**Olası nedeni**

Azure AD, HTTP isteğindeki URL parametreleri içindeki SAML isteğini tanımlayamadı. Bu durum, uygulamanın Azure AD 'ye SAML isteği gönderilirken HTTP yeniden yönlendirme bağlamasını kullanmadığı durumlarda meydana gelebilir.

**Çözünürlüğüne**

Uygulamanın, HTTP yeniden yönlendirme bağlamasını kullanarak konum başlığına kodlanmış SAML isteğini gönderebilmesi gerekir. Bunun nasıl gerçekleştirileceği hakkında daha fazla bilgi için [SAML protokolü belirtimi belgesinde](https://docs.oasis-open.org/security/saml/v2.0/saml-bindings-2.0-os.pdf) HTTP Yeniden Yönlendirme Bağlaması bölümünü okuyun.

## <a name="azure-ad-is-sending-the-token-to-an-incorrect-endpoint"></a>Azure AD, belirteci yanlış bir uç noktaya gönderiyor

**Olası nedeni**

Çoklu oturum açma sırasında, oturum açma isteği açık bir yanıt URL 'SI içermiyorsa (onaylama tüketici hizmeti URL 'SI), Azure AD bu uygulama için yapılandırılmış tüm özel URL 'Lerden birini seçer. Uygulamanın açık bir yanıt URL 'SI yapılandırılmış olsa bile, Kullanıcı yeniden yönlendirilebilir https://127.0.0.1:444 . 

Uygulama galeriden olmayan bir uygulama olarak eklendiğinde, Azure Active Directory bu yanıt URL'sini bir varsayılan değer olarak oluşturuldu. Bu davranış değiştirildi ve Azure Active Directory artık varsayılan olarak bu URL'yi eklemez. 

**Çözünürlüğüne**

Uygulama için yapılandırılmış kullanılmayan yanıt URL 'Lerini silin.

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici**olarak oturum açın.

2.  Ana sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i seçerek **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory"** yazın ve **Azure Active Directory** öğesini seçin.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' ı seçin.

5.  Tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' ı seçin.

    Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar**olarak ayarlayın.

6.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra **Temel SAML yapılandırması**'nı açın. **Yanıt URL 'si (onaylama tüketici hizmeti URL 'si)** içinde, sistem tarafından oluşturulan kullanılmayan veya varsayılan yanıt URL 'lerini silin. Örneğin, `https://127.0.0.1:444/applications/default.aspx`.

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Bir uygulamaya gönderilen SAML taleplerini özelleştirirken sorun oluştu

Uygulamanıza gönderilen SAML öznitelik taleplerini özelleştirmeyi öğrenmek için [Azure Active Directory 'de talep eşlemesi](../develop/active-directory-claims-mapping.md)' ne bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD 'de uygulamalar için SAML tabanlı çoklu oturum açma hatalarını ayıklama](../azuread-dev/howto-v1-debug-saml-sso-issues.md)
