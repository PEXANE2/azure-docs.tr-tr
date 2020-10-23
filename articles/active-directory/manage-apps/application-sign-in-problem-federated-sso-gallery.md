---
title: SAML tabanlı çoklu oturum açma yapılandırılmış uygulamalarda oturum açma sorunları
description: Azure Active Directory ile SAML tabanlı federe çoklu oturum açma için yapılandırdığınız bir uygulamada oturum açarken oluşan belirli hatalara yönelik kılavuz
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: troubleshooting
ms.date: 02/18/2019
ms.author: kenwith
ms.reviewer: luleon, asteen
ms.custom: contperfq2
ms.openlocfilehash: ec39a6d106973808e26b7c06dce8b3054af490ff
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92427381"
---
# <a name="problems-signing-in-to-saml-based-single-sign-on-configured-apps"></a>SAML tabanlı çoklu oturum açma yapılandırılmış uygulamalarda oturum açma sorunları
Aşağıdaki oturum açma sorunlarını gidermek için, çözümü daha iyi tanılayıp otomatik hale getirmek için aşağıdaki işlemleri yapmanızı öneririz:

- Azure portal Test deneyimini kullanırken daha iyi tanılama ve çözümler sağlamak üzere Azure Active Directory (Azure AD) yardımcı olmak için [uygulamalarım güvenli tarayıcı uzantısını](access-panel-extension-problem-installing.md) yüklemek.
- Azure portal, uygulama yapılandırma sayfasında Test deneyimini kullanarak hatayı yeniden oluşturun. [SAML tabanlı çoklu oturum açma uygulamalarında hata ayıklama](../azuread-dev/howto-v1-debug-saml-sso-issues.md) hakkında daha fazla bilgi edinin

Uygulamalarım güvenli tarayıcı uzantısı ile Azure portal [Test deneyimini](../azuread-dev/howto-v1-debug-saml-sso-issues.md) KULLANıYORSANıZ, SAML tabanlı çoklu oturum açma yapılandırması sayfasını açmak için aşağıdaki adımları el ile izlemeniz gerekmez.

SAML tabanlı çoklu oturum açma yapılandırma sayfasını açmak için:
1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **coadmin**olarak oturum açın.
1.  Ana sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i seçerek **Azure Active Directory uzantısını** açın.
1.  Filtre arama kutusuna **"Azure Active Directory"** yazın ve **Azure Active Directory** öğesini seçin.
1.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' ı seçin.
1.  Tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' ı seçin.
    Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar**olarak ayarlayın.
1.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.
1. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğini belirleyin.
1. SAML tabanlı SSO 'yu seçin.

## <a name="application-not-found-in-directory"></a>Uygulama dizinde bulunamadı
`Error AADSTS70001: Application with Identifier 'https:\//contoso.com' was not found in the directory.`

**Olası nedeni**

`Issuer`UYGULAMANıN SAML Isteğindeki Azure AD 'ye gönderdiği öznitelik, Azure AD 'de uygulama için yapılandırılmış tanımlayıcı değeriyle eşleşmiyor.

**Çözünürlük**

`Issuer`SAML isteğindeki özniteliğinin Azure AD 'de yapılandırılan tanımlayıcı değeri ile eşleştiğinden emin olun.

SAML tabanlı SSO yapılandırma sayfasında, **temel SAML yapılandırması** bölümünde, tanımlayıcı metin kutusundaki değerin hatada görüntülenen tanımlayıcı değer için değerle eşleştiğini doğrulayın.

## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>Yanıt adresi, uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor
`Error AADSTS50011: The reply address 'https:\//contoso.com' does not match the reply addresses configured for the application.`

**Olası nedeni**

`AssertionConsumerServiceURL`SAML isteğindeki değer, Azure AD 'de yapılandırılan yanıt URL 'si değeri veya düzeniyle eşleşmiyor. `AssertionConsumerServiceURL`SAML isteğindeki değeri, hatada gördüğünüz URL 'dir.

**Çözünürlük**

`AssertionConsumerServiceURL`SAML isteğindeki değerin Azure AD 'de yapılandırılan yanıt URL 'si değeri ile eşleştiğinden emin olun. 

Yanıt URL 'SI metin kutusundaki değeri SAML isteğindeki değerle eşleşecek şekilde doğrulayın veya güncelleştirin `AssertionConsumerServiceURL` .   
    
Azure AD 'de yanıt URL 'SI değerini güncelleştirdikten ve SAML isteğindeki uygulama tarafından gönderilen değeri eşleştirdikten sonra, uygulamada oturum açabiliyor olmanız gerekir.

## <a name="user-not-assigned-a-role"></a>Kullanıcıya bir rol atanmamış
`Error AADSTS50105: The signed in user 'brian\@contoso.com' is not assigned to a role for the application.`

**Olası nedeni**

Azure AD'de kullanıcıya uygulama için erişim verilmedi.​

**Çözünürlük**

Bir uygulamaya doğrudan bir veya daha fazla kullanıcı atamak için bkz. [hızlı başlangıç: bir uygulamaya Kullanıcı atama](add-application-portal-assign-users.md).

## <a name="not-a-valid-saml-request"></a>Geçerli bir SAML isteği değil
`Error AADSTS75005: The request is not a valid Saml2 protocol message.`

**Olası nedeni**

Azure AD, uygulama tarafından çoklu oturum açma için gönderilen SAML isteğini desteklemiyor. Yaygın sorunlardan bazıları:
- SAML isteğinde gerekli alanlar eksik
- SAML isteği kodlama yöntemi

**Çözünürlük**

1. SAML isteğini yakalayın. SAML isteğini nasıl yakalayacağınızı öğrenmek için [Azure AD 'de uygulamalar IÇIN SAML tabanlı çoklu oturum açma hatalarını ayıklama](../azuread-dev/howto-v1-debug-saml-sso-issues.md) öğreticisini izleyin.
1. Uygulama satıcısıyla iletişim kurun ve onlarla aşağıdaki bilgileri paylaşın:
    - SAML isteği
    - [Azure AD çoklu oturum açma SAML protokolü gereksinimleri](../develop/single-sign-on-saml-protocol.md)

Uygulama satıcısı, çoklu oturum açma için Azure AD SAML uygulamasını destekledikleri doğrulaması gerekir.

## <a name="misconfigured-application"></a>Yanlış yapılandırılmış uygulama
`Error AADSTS650056: Misconfigured application. This could be due to one of the following: The client has not listed any permissions in the requested permissions in the client's application registration. Or, The admin has not consented in the tenant. Or, Check the application identifier in the request to ensure it matches the configured client application identifier. Please contact your admin to fix the configuration or consent on behalf of the tenant.`

**Olası nedeni**

`Issuer`UYGULAMANıN SAML Isteğindeki Azure AD 'ye gönderdiği öznitelik, Azure AD 'de uygulama için yapılandırılan tanımlayıcı değeriyle eşleşmiyor.

**Çözünürlük**

`Issuer`SAML isteğindeki özniteliğinin Azure AD 'de yapılandırılan tanımlayıcı değeri ile eşleştiğinden emin olun. 

Tanımlayıcı metin kutusundaki değerin hatada görüntülenen tanımlayıcı değeri için olan değerle eşleştiğini doğrulayın.

## <a name="certificate-or-key-not-configured"></a>Sertifika veya anahtar yapılandırılmadı
`Error AADSTS50003: No signing key configured.`

**Olası nedeni**

Uygulama nesnesi bozuk ve Azure AD uygulama için yapılandırılan sertifikayı tanımıyor.

**Çözünürlük**

Yeni bir sertifika silmek ve oluşturmak için aşağıdaki adımları izleyin:
1. SAML tabanlı SSO yapılandırma ekranında, **SAML Imzalama sertifikası** bölümünde **Yeni sertifika oluştur** ' u seçin.
1. Sona erme tarihi ' ni seçin ve **Kaydet**' e tıklayın.
1. Etkin sertifikayı geçersiz kılmak için **Yeni sertifika etkin hale getirin** ' i işaretleyin. Ardından bölmenin üstündeki **Kaydet**’e tıklayın ve geçiş sertifikasını etkinleştirmeyi kabul edin.
1. **Kullanılmayan** sertifikayı kaldırmak Için **SAML imzalama sertifikası** bölümünde **Kaldır** ' ı tıklatın.

## <a name="saml-request-not-present-in-the-request"></a>İstekte SAML Isteği yok
`Error AADSTS750054: SAMLRequest or SAMLResponse must be present as query string parameters in HTTP request for SAML Redirect binding.`

**Olası nedeni**

Azure AD, HTTP isteğindeki URL parametreleri içindeki SAML isteğini tanımlayamadı. Bu durum, uygulamanın Azure AD 'ye SAML isteği gönderilirken HTTP yeniden yönlendirme bağlamasını kullanmadığı durumlarda meydana gelebilir.

**Çözünürlük**

Uygulamanın, HTTP yeniden yönlendirme bağlamasını kullanarak konum başlığına kodlanmış SAML isteğini gönderebilmesi gerekir. Bunun nasıl gerçekleştirileceği hakkında daha fazla bilgi için [SAML protokolü belirtimi belgesinde](https://docs.oasis-open.org/security/saml/v2.0/saml-bindings-2.0-os.pdf) HTTP Yeniden Yönlendirme Bağlaması bölümünü okuyun.

## <a name="azure-ad-is-sending-the-token-to-an-incorrect-endpoint"></a>Azure AD, belirteci yanlış bir uç noktaya gönderiyor
**Olası nedeni**

Çoklu oturum açma sırasında, oturum açma isteği açık bir yanıt URL 'SI içermiyorsa (onaylama tüketici hizmeti URL 'SI), Azure AD bu uygulama için yapılandırılmış yanıt URL 'Lerinden birini seçer. Uygulamanın açık bir yanıt URL 'SI yapılandırılmış olsa bile, Kullanıcı yeniden yönlendirilebilir https://127.0.0.1:444 . 

Uygulama galeriden olmayan bir uygulama olarak eklendiğinde, Azure Active Directory bu yanıt URL'sini bir varsayılan değer olarak oluşturuldu. Bu davranış değiştirildi ve Azure Active Directory artık varsayılan olarak bu URL'yi eklemez. 

**Çözünürlük**

Uygulama için yapılandırılmış kullanılmayan yanıt URL 'Lerini silin.

SAML tabanlı SSO yapılandırma sayfasında, **yanıt URL 'si (onaylama tüketici hizmeti URL 'si)** bölümünde, sistem tarafından oluşturulan kullanılmayan veya varsayılan yanıt URL 'lerini silin. Örneğin, `https://127.0.0.1:444/applications/default.aspx`.

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Bir uygulamaya gönderilen SAML taleplerini özelleştirirken sorun oluştu
Uygulamanıza gönderilen SAML öznitelik taleplerini özelleştirmeyi öğrenmek için [Azure Active Directory 'de talep eşlemesi](../develop/active-directory-claims-mapping.md)' ne bakın.

## <a name="errors-related-to-misconfigured-apps"></a>Yanlış yapılandırılmış uygulamalarla ilgili hatalar
Portaldaki yapılandırmaların, uygulamanızda sahip olduğunuz ile eşleştiğini doğrulayın. Özellikle, Istemci/uygulama KIMLIĞINI, yanıt URL 'Lerini, Istemci gizli dizilerini/anahtarlarını ve uygulama KIMLIĞI URI 'sini karşılaştırın.

Yalnızca yapılandırdığınız kaynakları istediğinizden emin olmak için, kodda erişim isteğinde bulunan kaynağı **gerekli kaynaklar** sekmesinde yapılandırılan izinlerle karşılaştırın.

## <a name="next-steps"></a>Sonraki adımlar
- [Uygulama yönetiminde hızlı başlangıç serisi](add-application-portal-assign-users.md)
- [Azure AD 'de uygulamalar için SAML tabanlı çoklu oturum açma hatalarını ayıklama](../azuread-dev/howto-v1-debug-saml-sso-issues.md)
- [Azure AD çoklu oturum açma SAML protokolü gereksinimleri](../develop/active-directory-single-sign-on-protocol-reference.md)
