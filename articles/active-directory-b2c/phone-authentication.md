---
title: Telefon kaydı ve özel ilkelerle oturum açma
titleSuffix: Azure AD B2C
description: Bir kerelik parolalar (OTP) metin iletilerinde, uygulama kullanıcılarınızın telefonlarınıza Azure Active Directory B2C ' de özel ilkelerle gönderin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/01/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 4a429314d4a992ea93f4c068203371cda769a4ff
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90029171"
---
# <a name="set-up-phone-sign-up-and-sign-in-with-custom-policies-in-azure-ad-b2c"></a>Azure AD B2C özel ilkelerle telefon kayıt ve oturum açma ayarlama

Azure Active Directory B2C (Azure AD B2C) telefon kaydı ve oturum açma, kullanıcılarınızın telefonunuza bir kısa mesajdan gönderilen bir kerelik parola (OTP) kullanarak uygulamalarınıza kaydolmalarını ve oturum açmasını sağlar. Bir kerelik parolalar, kullanıcılarınızın parolalarının tehlikeye düşmesi veya bu uygulamaların güvenliğinin aşılmasına neden olması riskini en aza indirmenize yardımcı olabilir.

Müşterilerinizin, telefonunuza bir kerelik parola kullanarak uygulamalarınıza kaydolup oturum açmasını sağlamak üzere özel ilkeleri kullanmak için bu makaledeki adımları izleyin.

## <a name="pricing"></a>Fiyatlandırma

Bir kerelik parolalar SMS metin iletileri kullanılarak kullanıcılarınıza gönderilir ve gönderilen her ileti için ücretlendirilmeyebilirsiniz. Fiyatlandırma bilgileri için [Azure Active Directory B2C fiyatlandırmasının](https://azure.microsoft.com/pricing/details/active-directory-b2c/) **ayrı ücretler** bölümüne bakın.

## <a name="user-experience-for-phone-sign-up-and-sign-in"></a>Telefon kayıt ve oturum açma için Kullanıcı deneyimi

Telefon kayıt ve oturum açma ile Kullanıcı, birincil tanımlayıcıları olarak bir telefon numarası kullanarak uygulamaya kaydolabilir. Kaydolma ve oturum açma sırasında son kullanıcının deneyimi aşağıda açıklanmaktadır.

> [!NOTE]
> Kaydolma ve oturum açma deneyiminize aşağıdaki örnek metinlere benzer onay bilgileri dahil etmeyi kesinlikle öneririz. Bu örnek metin yalnızca bilgilendirme amaçlıdır. Lütfen [CUR Web sitesinde](https://www.ctia.org/programs) kısa kod izleme el kitabı bakın ve son metin ve özellik yapılandırmanıza, kendi uyumluluk ihtiyaçlarınızı karşılayacak yönergeler için kendi yasal veya uyumluluk uzmanlarıyla temasa geçin:
>
> *Telefon numaranızı girerek, eklemek üzere oturum açmanıza yardımcı olmak üzere kısa mesaj tarafından gönderilen bir kerelik geçiş kodu almayı kabul etmiş olursunuz * &lt; : uygulamanızın adı &gt; *. Standart ileti ve veri ücretleri uygulanabilir.*
>
> *&lt;Ekle: Gizlilik Bildiriinize bir bağlantı&gt;*<br/>*&lt;Ekle: hizmet koşullarınıza bir bağlantı&gt;*

Kendi onay bilgilerinizi eklemek için aşağıdaki örneği özelleştirin ve görüntüleme denetimiyle birlikte kendi kendini onaylanan sayfa tarafından kullanılan ContentDefinition için LocalizedResources öğesine ekleyin (telefon kayıt & oturum açma paketinin Phone-Email-Base.xml dosyası):

```xml
<LocalizedResources Id="phoneSignUp.en">        
    <LocalizedStrings>
    <LocalizedString ElementType="DisplayControl" ElementId="phoneControl" StringId="disclaimer_msg_intro">By providing your phone number, you consent to receiving a one-time passcode sent by text message to help you sign into {insert your application name}. Standard messsage and data rates may apply.</LocalizedString>          
    <LocalizedString ElementType="DisplayControl" ElementId="phoneControl" StringId="disclaimer_link_1_text">Privacy Statement</LocalizedString>                
    <LocalizedString ElementType="DisplayControl" ElementId="phoneControl" StringId="disclaimer_link_1_url">{insert your privacy statement URL}</LocalizedString>          
    <LocalizedString ElementType="DisplayControl" ElementId="phoneControl" StringId="disclaimer_link_2_text">Terms and Conditions</LocalizedString>             
    <LocalizedString ElementType="DisplayControl" ElementId="phoneControl" StringId="disclaimer_link_2_url">{insert your terms and conditions URL}</LocalizedString>          
    <LocalizedString ElementType="UxElement" StringId="initial_intro">Please verify your country code and phone number</LocalizedString>        
    </LocalizedStrings>      
</LocalizedResources>
   ```

### <a name="phone-sign-up-experience"></a>Telefon kayıt deneyimi

Kullanıcının uygulamanız için zaten bir hesabı yoksa, **Şimdi kaydolun** bağlantısını seçerek bir tane oluşturabilir. Kullanıcının **ülkesini**seçtiği, telefon numarasını girdiği ve **kodu gönder**seçtiği bir kaydolma sayfası görüntülenir.

![Kullanıcı telefon kaydı başlatır](media/phone-authentication/phone-signup-start.png)

Kullanıcının telefon numarasına bir kerelik doğrulama kodu gönderilir. Kullanıcı **doğrulama kodunu** kaydolma sayfasına girer ve **kodu doğrula**' yı seçer. (Kullanıcı kodu alamıyorsa, **Yeni kod gönder**' i seçebiliyor.)

![Kullanıcı telefon kaydı sırasında kodu doğrular](media/phone-authentication/phone-signup-verify-code.png)

 Kullanıcı kaydolma sayfasında istenen diğer bilgileri (örneğin, **görünen ad**, **verilen ad**ve **Soyadı** ) girer (ülke ve telefon numarası doldurulmuş olarak kalır). Kullanıcı farklı bir telefon numarası kullanmak isterse, kayıt işlemini yeniden başlatmak için **numarayı Değiştir** ' i seçebilirler. İşiniz bittiğinde, Kullanıcı **devam**' ı seçer.

![Kullanıcı ek bilgi sağlar](media/phone-authentication/phone-signup-additional-info.png)

Ardından, kullanıcıdan bir kurtarma e-postası sağlaması istenir. Kullanıcı e-posta adresini girip, **doğrulama kodu gönder**' i seçer. Bir kod, kullanıcının e-posta gelen kutusuna gönderilir ve bu kimlik **doğrulama kodu** kutusuna girebilirler. Ardından Kullanıcı **doğrulama kodunu**seçer. 

Kod doğrulandıktan sonra Kullanıcı, hesabını oluşturmak için **Oluştur** ' u seçer. Ya da Kullanıcı farklı bir e-posta adresi kullanmak isterse, **e-postayı değiştir**' i seçebilirler.

![Kullanıcı Hesap oluşturuyor](media/phone-authentication/email-verification.png)

### <a name="phone-sign-in-experience"></a>Telefonla oturum açma deneyimi

Kullanıcı, telefon numarası tanımlayıcı olarak bir hesap hesabı içeriyorsa, Kullanıcı telefon numarasını girer ve **devam**' ı seçer. **Devam**' i seçerek ülke ve telefon numarasını onaylarlar ve telefonlarına bir kerelik doğrulama kodu gönderilir. Kullanıcı doğrulama kodunu girer ve oturum açmak için **devam** ' i seçer.

![Telefonla oturum açma kullanıcı deneyimi](media/phone-authentication/phone-signin-screens.png)

## <a name="deleting-a-user-account"></a>Kullanıcı hesabını silme

Belirli durumlarda, Azure AD B2C dizininizden bir kullanıcıyı ve ilişkili verileri silmeniz gerekebilir. Azure portal aracılığıyla bir kullanıcı hesabının nasıl silineceği hakkında ayrıntılar için, [Bu yönergelere](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-azure#step-5-delete)bakın. 

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]



## <a name="prerequisites"></a>Ön koşullar

OTP 'yi ayarlamadan önce aşağıdaki kaynaklara sahip olmanız gerekir.

* [Azure AD B2C kiracı](tutorial-create-tenant.md)
* Kiracınızda [kayıtlı Web uygulaması](tutorial-register-applications.md)
* Kiracınıza yüklenen [özel ilkeler](custom-policy-get-started.md)

## <a name="get-the-phone-sign-up--sign-in-starter-pack"></a>Telefon kaydolma & oturum açma başlangıç paketini alın

Telefon kayıt ve oturum açma özel ilke dosyalarını güncelleştirerek Azure AD B2C kiracınızla çalışacak şekilde çalışmaya başlayın.

Aşağıdaki adımlarda [önkoşulları](#prerequisites) tamamladığınız ve [özel ilke Başlatıcı paketi][starter-pack] deposunu zaten yerel makinenize Klonladığınız varsayılır.

1. Başlangıç paketi deposunun yerel kopyanızda [Telefon kayıt ve oturum açma özel ilke dosyalarını][starter-pack-phone] bulun veya doğrudan indirin. XML ilke dosyaları şu dizinde bulunur:

    `active-directory-b2c-custom-policy-starterpack/scenarios/`**`phone-number-passwordless`**

1. Her dosyada, dizeyi `yourtenant` Azure AD B2C kiracınızın adıyla değiştirin. Örneğin, B2C kiracınızın adı *contosob2c*ise, tüm örnekleri `yourtenant.onmicrosoft.com` olur `contosob2c.onmicrosoft.com` .

1. [Azure Active Directory B2C özel ilkeleri kullanmaya başlama](custom-policy-get-started.md)konusunun [özel Ilkeye uygulama kimlikleri ekleme](custom-policy-get-started.md#add-application-ids-to-the-custom-policy) bölümündeki adımları uygulayın. Bu durumda, `/phone-number-passwordless/` **`Phone_Email_Base.xml`** *IdentityExperienceFramework* ve *ProxyIdentityExperienceFramework*önkoşullarını tamamlarken kaydettiğiniz Iki uygulamanın **uygulama (istemci) kimlikleriyle** güncelleştirin.

## <a name="upload-the-policy-files"></a>İlke dosyalarını karşıya yükle

1. [Azure Portal](https://portal.azure.com) oturum açın ve Azure AD B2C kiracınıza gidin.
1. **İlkeler**altında **kimlik deneyimi çerçevesi**' ni seçin.
1. **Özel Ilkeyi karşıya yükle**' yi seçin.
1. İlke dosyalarını aşağıdaki sırada karşıya yükleyin:
    1. *Phone_Email_Base.xml*
    1. *SignUpOrSignInWithPhone.xml*
    1. *SignUpOrSignInWithPhoneOrEmail.xml*
    1. *ProfileEditPhoneOnly.xml*
    1. *ProfileEditPhoneEmail.xml*
    1. *ChangePhoneNumber.xml*
    1. *PasswordResetEmail.xml*

Her dosyayı karşıya yüklerken Azure, ön eki ekler `B2C_1A_` .

## <a name="test-the-custom-policy"></a>Özel ilkeyi test etme

1. **Özel ilkeler**altında **B2C_1A_SignUpOrSignInWithPhone**' yi seçin.
1. **Uygulama Seç**altında önkoşulları tamamlarken kaydettiğiniz *WebApp1* uygulamasını seçin.
1. **Yanıt URL 'Si Seç**için öğesini seçin `https://jwt.ms` .
1. **Şimdi Çalıştır** ' ı seçin ve bir e-posta adresi veya telefon numarası kullanarak kaydolun.
1. **Şimdi Çalıştır** ' ı bir kez daha seçin ve doğru yapılandırmaya sahip olduğunu onaylamak için aynı hesapla oturum açın.

## <a name="get-user-account-by-phone-number"></a>Telefon numarasına göre Kullanıcı hesabını al

Telefon numarası ile kaydolan ancak bir kurtarma e-posta adresi sağlamayan bir Kullanıcı, oturum açma adı olarak telefon numarası ile Azure AD B2C dizinine kaydedilir. Kullanıcı daha sonra telefon numaralarını değiştirmeyi istiyorsa, yardım masasına veya destek ekibinizin öncelikle hesabını bulması ve sonra telefon numaralarını güncelleştirmesi gerekir.

[Microsoft Graph](manage-user-accounts-graph-api.md)kullanarak, bir kullanıcıyı telefon numarası (oturum açma adı) ile bulabilirsiniz:

```http
GET https://graph.microsoft.com/v1.0/users?$filter=identities/any(c:c/issuerAssignedId eq '+{phone number}' and c/issuer eq '{tenant name}.onmicrosoft.com')
```

Örneğin:

```http
GET https://graph.microsoft.com/v1.0/users?$filter=identities/any(c:c/issuerAssignedId eq '+450334567890' and c/issuer eq 'contosob2c.onmicrosoft.com')
```

## <a name="next-steps"></a>Sonraki adımlar

GitHub 'da telefon kayıt ve oturum açma özel İlkesi başlangıç paketini (ve diğer başlangıç paketleri) bulabilirsiniz: [Azure-Samples/Active-Directory-B2C-Custom-Policy-starterpack/senaryolar/Phone-Number-passwordless][starter-pack-phone] Başlatıcı paketi ilke dosyaları Multi-Factor Authentication teknik profillerini ve telefon numarası talep dönüşümlerini kullanır:
* [Azure Multi-Factor Authentication teknik profili tanımlama](multi-factor-auth-technical-profile.md)
* [Telefon numarası talep dönüşümlerini tanımlayın](phone-number-claims-transformations.md)

<!-- LINKS - External -->
[starter-pack]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
[starter-pack-phone]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/phone-number-passwordless
