---
title: Federe tek oturum açma galerisi uygulamasına oturum açma sorunları | Microsoft Dokümanlar
description: Azure AD ile SAML tabanlı federe tek oturum açma için yapılandırdığınız bir uygulamada oturum açarken belirli hatalar için kılavuz
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/18/2019
ms.author: mimart
ms.reviewer: luleon, asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 874d273e26a728afc0a1dc1a16852016797067ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77367890"
---
# <a name="problems-signing-in-to-a-gallery-application-configured-for-federated-single-sign-on"></a>Federe tek oturum açma için yapılandırılan bir galeri uygulamasında oturum açma sorunları

Aşağıdaki oturum açma sorunlarını gidermek için, daha iyi tanı almak ve çözüm adımlarını otomatikleştirmek için aşağıdaki öneriyi izlemenizi öneririz:

- Azure portalındaki test deneyimini kullanırken daha iyi tanı ve çözümler sağlamak için Azure Etkin Dizini'nin (Azure AD) yardımcı olması için [Uygulamalarım Güvenli Tarayıcı Uzantısı'nı](access-panel-extension-problem-installing.md) yükleyin.
- Azure portalındaki uygulama yapılandırma sayfasındaki test deneyimini kullanarak hatayı yeniden üretin. [Hata Ayıklama SAML tabanlı tek oturum açma uygulamaları](../azuread-dev/howto-v1-debug-saml-sso-issues.md) hakkında daha fazla bilgi edinin


## <a name="application-not-found-in-directory"></a>Uygulama dizinde bulunamadı

*Hata AADSTS70001: Identifier 'https:\//contoso.com' ile uygulama dizinde bulunamadı.*

**Olası nedeni**

`Issuer` SAML isteğinde uygulamadan Azure AD'ye gönderilen öznitelik, Azure AD'deki uygulama için yapılandırılan Tanımlayıcı değeriyle eşleşmez.

**Çözünürlük**

SAML `Issuer` isteğindeki özniteliğin Azure AD'de yapılandırılan Tanımlayıcı değeriyle eşleştiğinden emin olun. Azure portalındaki [test deneyimini](../azuread-dev/howto-v1-debug-saml-sso-issues.md) My Apps Secure Browser Extension ile kullanıyorsanız, bu adımları el ile izlemeniz gerekmez.

1.  Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin**olarak oturum açın.

1.  Ana sol gezinme menüsünün üst **kısmındaki tüm hizmetleri** seçerek Azure Etkin **Dizin Uzantısı'nı** açın.

1.  Filtre arama kutusuna **"Azure Etkin Dizin"** yazın ve **Azure Etkin Dizin** öğesini seçin.

1.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** seçin.

1.  **Tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı seçin.

    Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** üst kısmındaki **Filtre** denetimini kullanın ve **Tüm Uygulamalar**için **Göster** seçeneğini ayarlayın.

1.  Tek oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

1.  Uygulama yüklendikten sonra **Temel SAML yapılandırması**'nı açın. Tanımlayıcı metin kutusundaki değerin hatada görüntülenen tanımlayıcı değeriyle eşleştiğini doğrulayın.



## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>Yanıt adresi, uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor

*Hata AADSTS50011: 'https:\//contoso.com' yanıt adresi, uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor*

**Olası nedeni**

SAML isteğindeki değer, `AssertionConsumerServiceURL` Azure AD'de yapılandırılan YanıtURL değeri veya desenle eşleşmez. SAML isteğindeki `AssertionConsumerServiceURL` değer, hatada gördüğünüz URL'dir.

**Çözünürlük**

SAML `AssertionConsumerServiceURL` isteğindeki değerin Azure AD'de yapılandırılan Yanıt URL değeriyle eşleştiğinden emin olun. Azure portalındaki [test deneyimini](../azuread-dev/howto-v1-debug-saml-sso-issues.md) My Apps Secure Browser Extension ile kullanıyorsanız, bu adımları el ile izlemeniz gerekmez.

1.  Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin**olarak oturum açın.

1.  Ana sol gezinme menüsünün üst **kısmındaki tüm hizmetleri** seçerek Azure Etkin **Dizin Uzantısı'nı** açın.

1.  Filtre arama kutusuna **"Azure Etkin Dizin"** yazın ve **Azure Etkin Dizin** öğesini seçin.

1.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** seçin.

1.  **Tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı seçin.

    Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** üst kısmındaki **Filtre** denetimini kullanın ve **Tüm Uygulamalar**için **Göster** seçeneğini ayarlayın.

1.  Tek oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

1.  Uygulama yüklendikten sonra **Temel SAML yapılandırması**'nı açın. SAML isteğindeki `AssertionConsumerServiceURL` değeri eşleştirmek için YanıtLA URL metin kutusundaki değeri doğrulayın veya güncelleştirin.    
    
Azure AD'de Yanıtla URL değerini güncelledikten ve saml isteğinde uygulama tarafından gönderilen değerle eşleştikten sonra uygulamada oturum açabilmelisiniz.

## <a name="user-not-assigned-a-role"></a>Kullanıcıya bir rol atanmamış

*Hata AADSTS50105: Oturum açmış\@kullanıcı 'brian contoso.com' uygulama için bir rol atanmamış.*

**Olası nedeni**

Azure AD'de kullanıcıya uygulama için erişim verilmedi.​

**Çözünürlük**

Bir veya daha fazla kullanıcıyı doğrudan bir uygulamaya atamak için aşağıdaki adımları izleyin. Azure portalındaki [test deneyimini](../azuread-dev/howto-v1-debug-saml-sso-issues.md) My Apps Secure Browser Extension ile kullanıyorsanız, bu adımları el ile izlemeniz gerekmez.

1.  Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator**olarak oturum açın.

1.  Ana sol gezinme menüsünün üst **kısmındaki tüm hizmetleri** seçerek Azure Etkin **Dizin Uzantısı'nı** açın.

1.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

1.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** seçin.

1.  **Tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı seçin.

    Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** üst kısmındaki **Filtre** denetimini kullanın ve **Tüm Uygulamalar**için **Göster** seçeneğini ayarlayın.

1.  Uygulamalar listesinden, kullanıcıatamak istediğiniz uygulamayı seçin.

1.  Uygulama yüklendikten sonra, uygulamanın sol daki gezinme menüsünden **Kullanıcılar ve Gruplar'ı** seçin.

1.  **Atama** **Ekle** bölmesini açmak için Kullanıcılar **ve Gruplar** listesinin üst kısmındaki Ekle düğmesini tıklatın.

1.  **Atama Ekle** bölmesinden Kullanıcılar **ve gruplar** seçiciyi seçin.

1. **Ad veya e-posta adresiyle ara** arama kutusuna, eklemek istediğiniz kullanıcının tam adını veya e-posta adresini yazın.

1. Bir **onay kutusunu**ortaya çıkarmak için listedeki **kullanıcının** üzerine taşırın. Kullanıcıyı **Seçili** listeye eklemek için kullanıcının profil fotoğrafının veya logosunun yanındaki onay kutusunu tıklatın.

1. **İsteğe bağlı:** **Birden fazla kullanıcı eklemek**istiyorsanız, **Ada veya e-posta adresi** arama kutusuna başka bir tam ad veya e-posta adresi yazın ve kullanıcıyı **Seçili** listeye eklemek için onay kutusunu tıklatın.

1. Kullanıcıları seçmeyi bitirdiğinizde, bunları uygulamaya atanacak kullanıcı ve gruplar listesine eklemek için **Seç** düğmesini tıklatın.

1. **İsteğe bağlı:** Seçtiğiniz kullanıcılara atamak için bir rol seçmek için **Atama Ekle** bölmesinde Rol seçiyi **seçin'** i tıklatın.

1. Uygulamayı seçili kullanıcılara atamak için **Atla** düğmesini tıklatın.

Kısa bir süre sonra, seçtiğiniz kullanıcılar çözüm açıklaması bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatabilecektir.

## <a name="not-a-valid-saml-request"></a>Geçerli bir SAML isteği değil

*Hata AADSTS75005: İstek geçerli bir Saml2 iletişim kuralı iletisi değildir.*

**Olası nedeni**

Azure AD, uygulama tarafından çoklu oturum açma için gönderilen SAML isteğini desteklemiyor. Yaygın sorunlardan bazıları:

-   SAML isteğinde gerekli alanlar eksik
-   SAML isteği kodlama yöntemi

**Çözünürlük**

1. SAML isteğini yakalayın. SAML isteğini nasıl yakalayacağınıöğrenmek [için Azure AD'deki uygulamalarda SAML tabanlı tek oturum](../azuread-dev/howto-v1-debug-saml-sso-issues.md) açma hatasını nasıl yaptığı nızı öğrenin.

1. Uygulama satıcısıyla iletişim kurun ve onlarla aşağıdaki bilgileri paylaşın:

   -   SAML isteği

   -   [Azure AD Tek Oturum AÇMA SAML protokol gereksinimleri](../develop/single-sign-on-saml-protocol.md)

Uygulama satıcısı, tek oturum açma için Azure AD SAML uygulamasını desteklediğini doğrulamalıdır.

## <a name="misconfigured-application"></a>Yanlış yapılandırılmış uygulama

*Hata AADSTS650056: Yanlış yapılandırılmış uygulama. Bunun nedeni aşağıdakilerden biri olabilir: İstemci, istemcinin başvuru kaydında istenen izinlerde herhangi bir izin listelemedi. Veya, yönetici kiracı izin vermedi. Veya, yapılandırılan istemci uygulama tanımlayıcısı ile eşleştiğinden emin olmak için istekteki uygulama tanımlayıcısını denetleyin. Kiracı adına yapılandırmayı veya onayı düzeltmek için lütfen yöneticinize başvurun.*

**Olası nedeni**

`Issuer` SAML isteğinde uygulamadan Azure AD'ye gönderilen öznitelik, Azure AD'deki uygulama için yapılandırılan Tanımlayıcı değeriyle eşleşmez.

**Çözünürlük**

SAML `Issuer` isteğindeki özniteliğin Azure AD'de yapılandırılan Tanımlayıcı değeriyle eşleştiğinden emin olun. Azure portalındaki [test deneyimini](../azuread-dev/howto-v1-debug-saml-sso-issues.md) My Apps Secure Browser Extension ile kullanıyorsanız, aşağıdaki adımları el ile izlemeniz gerekmez:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin**olarak oturum açın.

1.  Ana sol gezinme menüsünün üst **kısmındaki tüm hizmetleri** seçerek Azure Etkin **Dizin Uzantısı'nı** açın.

1.  Filtre arama kutusuna **"Azure Etkin Dizin"** yazın ve **Azure Etkin Dizin** öğesini seçin.

1.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** seçin.

1.  **Tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı seçin.

    Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** üst kısmındaki **Filtre** denetimini kullanın ve **Tüm Uygulamalar**için **Göster** seçeneğini ayarlayın.

1.  Tek oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

1.  Uygulama yüklendikten sonra **Temel SAML yapılandırması**'nı açın. Tanımlayıcı metin kutusundaki değerin hatada görüntülenen tanımlayıcı değeriyle eşleştiğini doğrulayın.


## <a name="certificate-or-key-not-configured"></a>Sertifika veya anahtar yapılandırılmamada

*Hata AADSTS50003: İmza anahtarı yapılandırıldı.*

**Olası nedeni**

Uygulama nesnesi bozuk ve Azure AD uygulama için yapılandırılan sertifikayı tanımıyor.

**Çözünürlük**

Yeni bir sertifikayı silmek ve oluşturmak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin**olarak oturum açın.

1. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

1. Filtre arama kutusuna **"Azure Etkin Dizin"** yazın ve **Azure Etkin Dizin** öğesini seçin.

1. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** seçin.

1. **Tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı seçin.

    Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** üst kısmındaki **Filtre** denetimini kullanın ve **Tüm Uygulamalar**için **Göster** seçeneğini ayarlayın.

1. Tek oturum açma yapılandırmak istediğiniz uygulamayı seçin

1. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

1. **SAML imzalama Sertifikası** bölümü altında yeni sertifika **oluştur'u** seçin.

1. Son kullanma tarihini seçin ve ardından **Kaydet'i**tıklatın.

1. Etkin sertifikayı geçersiz kılmak için **yeni sertifikayı etkin hale getir'i** işaretleyin. Ardından bölmenin üstündeki **Kaydet**’e tıklayın ve geçiş sertifikasını etkinleştirmeyi kabul edin.

1. **SAML İmzaSertifikası** bölümünün altında, **Kullanılmayan** sertifikayı kaldırmak için **kaldır'ı** tıklatın.

## <a name="saml-request-not-present-in-the-request"></a>SAML İstek istekte bulunmaz

*Hata AADSTS750054: SAMLRequest veya SAMLResponse SAML Yönlendirme bağlama için HTTP isteği sorgu dize parametreleri olarak mevcut olmalıdır.*

**Olası nedeni**

Azure AD, HTTP isteğindeki URL parametreleri içinde SAML isteğini tanımlayamadı. Bu durum, SAML isteğini Azure AD'ye gönderirken UYGULAMA HTTP yeniden yönlendirme bağlamayı kullanmıyorsa gerçekleşebilir.

**Çözünürlük**

Uygulamanın, HTTP yönlendirme bağlamasını kullanarak konum üstbilgisine kodlanmış SAML isteğini göndermesi gerekir. Bunun nasıl gerçekleştirileceği hakkında daha fazla bilgi için [SAML protokolü belirtimi belgesinde](https://docs.oasis-open.org/security/saml/v2.0/saml-bindings-2.0-os.pdf) HTTP Yeniden Yönlendirme Bağlaması bölümünü okuyun.

## <a name="azure-ad-is-sending-the-token-to-an-incorrect-endpoint"></a>Azure AD belirteci yanlış bir bitiş noktasına gönderiyor

**Olası nedeni**

Tek oturum açma sırasında, oturum açma isteği açık bir yanıt URL'si (İddia Tüketici Hizmeti URL'si) içermiyorsa, Azure AD bu uygulama için yapılandırılan rely URL'lerinden herhangi birini seçer. Uygulama açık bir yanıt URL'si yapılandırıldı bile, kullanıcı https://127.0.0.1:444yeniden yönlendirilmiş olabilir. 

Uygulama galeriden olmayan bir uygulama olarak eklendiğinde, Azure Active Directory bu yanıt URL'sini bir varsayılan değer olarak oluşturuldu. Bu davranış değiştirildi ve Azure Active Directory artık varsayılan olarak bu URL'yi eklemez. 

**Çözünürlük**

Uygulama için yapılandırılan kullanılmayan yanıt URL'lerini silin.

1.  Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin**olarak oturum açın.

2.  Ana sol gezinme menüsünün üst **kısmındaki tüm hizmetleri** seçerek Azure Etkin **Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizin"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** seçin.

5.  **Tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı seçin.

    Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** üst kısmındaki **Filtre** denetimini kullanın ve **Tüm Uygulamalar**için **Göster** seçeneğini ayarlayın.

6.  Tek oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra **Temel SAML yapılandırması**'nı açın. Yanıt **URL'sinde (İddia Tüketici Hizmeti URL'si)** sistem tarafından oluşturulan kullanılmayan veya varsayılan Yanıt URL'lerini silin. Örneğin, `https://127.0.0.1:444/applications/default.aspx`.

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Bir uygulamaya gönderilen SAML taleplerini özelleştirirken sorun

Uygulamanıza gönderilen SAML öznitelik taleplerini nasıl özelleştiriştireceklerini öğrenmek için [Azure Etkin Dizini'nde Talepler eşleme](../develop/active-directory-claims-mapping.md)sine bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD'deki uygulamalarda SAML tabanlı tek oturum açma hataayıklama](../azuread-dev/howto-v1-debug-saml-sso-issues.md)
