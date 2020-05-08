---
title: Doğrulama yönteminiz olarak bir güvenlik anahtarı ayarlama-Azure AD
description: Kimlik doğrulama yönteminiz olarak bir hızlı kimlik çevrimiçi (FIDO2) güvenlik anahtarı kullanmak üzere kimliğinizi doğrulamak için güvenlik bilgileriniz (Önizleme) sayfasını ayarlama.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: librown
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: curtand
ms.openlocfilehash: 016a09432499c5fd2be8fd488c7ef7a644c00bca
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82628342"
---
# <a name="set-up-a-security-key-as-your-verification-method"></a>Doğrulama yönteminiz olarak bir güvenlik anahtarı ayarlayın

Güvenlik anahtarlarını, kuruluşunuzda bir passwordless oturum açma yöntemi olarak kullanabilirsiniz. Güvenlik anahtarı, iş veya okul hesabınızda oturum açmak için benzersiz bir PIN ile kullanılan fiziksel bir cihazdır. Güvenlik anahtarlarına fiziksel cihaz ve yalnızca bildiğiniz bir şey gerektiğinden, bir Kullanıcı adı ve paroladan daha güçlü bir doğrulama yöntemi olarak kabul edilir.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-key.md)]

>[!Note]
> Güvenlik anahtarı seçeneğini görmüyorsanız, kuruluşunuz doğrulama için bu seçeneği kullanmanıza izin vermiyor olabilir. Bu durumda, daha fazla yardım almak için başka bir yöntem seçmeniz veya kuruluşunuzun yardım masasına başvurmanız gerekir.

## <a name="security-verification-versus-password-reset-authentication"></a>Güvenlik doğrulaması ve parola sıfırlama kimlik doğrulaması

Güvenlik bilgileri yöntemleri hem iki öğeli güvenlik doğrulaması hem de parola sıfırlama için kullanılır. Ancak, her ikisi için tüm yöntemler kullanılamaz.

| Yöntem | Kullanıldığı yerler |
| ------ | -------- |
| Doğrulayıcı uygulama | İki öğeli doğrulama ve parola sıfırlama kimlik doğrulaması. |
| SMS mesajları | İki öğeli doğrulama ve parola sıfırlama kimlik doğrulaması. |
| Telefon görüşmeleri | İki öğeli doğrulama ve parola sıfırlama kimlik doğrulaması. |
| Güvenlik anahtarı | İki öğeli doğrulama ve parola sıfırlama kimlik doğrulaması. |
| E-posta hesabı | Yalnızca parola sıfırlama kimlik doğrulaması. İki öğeli doğrulama için başka bir yöntem seçmeniz gerekir. |
| Güvenlik soruları | Yalnızca parola sıfırlama kimlik doğrulaması. İki öğeli doğrulama için başka bir yöntem seçmeniz gerekir. |

## <a name="what-is-a-security-key"></a>Güvenlik anahtarı nedir?

Şu anda [hızlı kimlik çevrimiçi (FIDO)](https://fidoalliance.org/fido2/) (FIDO2) parolasız kimlik doğrulama protokollerini kullanarak birkaç tasarımı ve güvenlik anahtarı sağlayıcısını destekliyoruz. Bu anahtarlar, desteklenen bir cihaz ve Web tarayıcısından kuruluşunuzun bulut tabanlı kaynaklarına erişmek için iş veya okul hesabınızda oturum açmanıza olanak tanır.

Yöneticiniz veya kuruluşunuz size iş veya okul hesabınız için ihtiyaç duydukları bir güvenlik anahtarı sağlar. Kullanabileceğiniz farklı güvenlik anahtarı türleri vardır, örneğin cihazınıza taktığınız USB anahtarı veya bir NFC okuyucusuna dokunmanız gereken bir NFC anahtarı vardır. Kuruluşunuzun belgelerinden, ne tür olduğunu da içeren güvenlik anahtarınız hakkında daha fazla bilgi edinebilirsiniz.

> [!Note]
> Bir FIDO2 güvenlik anahtarı kullanmanızın ardından Microsoft Authenticator uygulaması veya Windows Hello gibi kullanabileceğiniz diğer passwordless doğrulama yöntemleri vardır. Microsoft Authenticator uygulaması hakkında daha fazla bilgi için, bkz. [Microsoft Authenticator uygulama nedir?](user-help-auth-app-overview.md). Windows Hello hakkında daha fazla bilgi için bkz. [Windows Hello 'ya genel bakış](https://www.microsoft.com/windows/windows-hello).

## <a name="before-you-begin"></a>Başlamadan önce

Güvenlik anahtarınızı kaydedebilmek için aşağıdaki koşulların doğru olması gerekir:

- Yöneticiniz bu özelliği kuruluşunuzda kullanılmak üzere açmış.

- Windows 10 Mayıs 2019 güncelleştirme ve desteklenen bir tarayıcı kullanarak bir cihazımız.

- Yöneticiniz veya kuruluşunuz tarafından onaylanan bir fiziksel güvenlik anahtarınız vardır. Güvenlik anahtarınız hem FIDO2 hem de Microsoft uyumlu olmalıdır. Güvenlik anahtarınız ve uyumlu olup olmadığı hakkında sorularınız varsa kuruluşunuzun yardım masasına başvurun.

## <a name="register-your-security-key"></a>Güvenlik anahtarınızı kaydetme

Anahtar kullanarak iş veya okul hesabınızda oturum açabilmeniz için güvenlik anahtarınızı oluşturmanız ve bu anahtara benzersiz bir PIN sağlamanız gerekir. Hesabınıza kayıtlı en fazla 10 anahtarınız olabilir. 

1. ' De Profilim sayfasına gidin ve henüz yapmadıysanız oturum açın. **My Profile** https://myaccount.microsoft.com

2. **Güvenlik bilgileri**' ni seçin **, Yöntem Ekle**' yi seçin ve ardından **Yöntem Ekle** açılır listesinden **güvenlik anahtarı** ' nı seçin.

    ![Güvenlik anahtarı seçiliyken Yöntem kutusu Ekle](media/security-info/security-info-security-key-add-method.png)

3. **Ekle**' yi seçin ve ardından, **USB cihazı** veya **NFC cihazı**olan güvenlik anahtarı türünü seçin.

    ![Bir USB veya NFC güvenlik anahtarı türüne sahip olup olmadığını seçin](media/security-info/security-info-security-key-choose-type.png)

    > [!Note]
    > Sahip olduğunuz güvenlik anahtarı türünü bilmiyorsanız üreticinin belgelerine başvurun. Üreticiye ilişkin emin değilseniz, yardım almak için kuruluşunuzun yardım masasına başvurun.

4. Güvenlik anahtarınızı fiziksel olarak kullanılabilir yapın ve ardından **güvenlik anahtarı** kutusunda **İleri**' yi seçin.

    ![Güvenlik anahtarı başlangıç kayıt kutusu](media/security-info/security-info-security-key-start-setup.png)

    Yeni oturum açma yönteminizi ayarlamanıza yardımcı olacak yeni bir kutu belirir.

5. **Yeni oturum açma yönteminizi ayarlama** kutusunda **İleri**' yi seçin ve ardından:

    - Güvenlik anahtarınız bir USB cihazından, cihazınızın USB bağlantı noktasına güvenlik anahtarınızı ekleyin.

    - Güvenlik anahtarınız bir NFC cihazınızla, okuyucunuzun güvenlik anahtarınıza dokunun.

6. Benzersiz güvenlik anahtarı PIN 'inizi **Windows Güvenlik** kutusuna yazın ve ardından **Tamam**' ı seçin.

    **Yeni oturum açma yönteminiz kutusunu ayarlamaya** geri döneceksiniz.

7. **İleri**’yi seçin.

8. **Güvenlik bilgileri** sayfasına dönün, daha sonra yeni güvenlik anahtarınız için tanıyabileceğiniz bir ad yazın ve ardından **İleri**' yi seçin.

    ![Güvenlik bilgileri sayfası, güvenlik anahtarınızı adlandırma](media/security-info/security-info-security-key-name.png)

    Güvenlik anahtarınız kaydedilir ve iş veya okul hesabınızda oturum açmak için kullanmaya hazır hale gelir.

9. **Güvenlik anahtarı** kutusunu kapatmak için **bitti** ' yi seçin.

    **Güvenlik bilgileri** sayfası, güvenlik anahtarı bilgileriniz ile güncelleştirilir.

    ![Güvenlik bilgileri sayfası, tüm kayıtlı Yöntemler gösteriliyor](media/security-info/security-info-security-key-configured.png)

## <a name="delete-a-security-key-from-your-security-info"></a>Güvenlik bilgilerinizden bir güvenlik anahtarı silme

Artık bir güvenlik anahtarı kullanmak istemiyorsanız, güvenlik bilgilerinizden anahtarı silebilirsiniz. Bu, güvenlik anahtarının iş veya okul hesabınızla kullanılmasını durdururken, güvenlik anahtarı verilerinizi ve kimlik bilgilerini depolamaya devam eder. Güvenlik anahtarından veri ve kimlik bilgisi bilgilerini silmek için, bu makaledeki [Microsoft uyumlu güvenlik anahtarını sıfırlama](#reset-your-security-key) bölümündeki yönergeleri izlemeniz gerekir.

1. Kaldırmak için güvenlik anahtarından **Sil** bağlantısını seçin.

2. **Güvenlik anahtarını Sil** kutusundan **Tamam ' ı** seçin.

    Güvenlik anahtarınız silinir ve artık iş veya okul hesabınızda oturum açmak için kullanamazsınız.

>[!Important]
>Bir güvenlik anahtarını yanlışlıkla silerseniz, bu makalenin [güvenlik anahtarınızı kaydetme](#register-your-security-key) bölümündeki yönergeleri kullanarak yeniden kaydedebilirsiniz.

## <a name="manage-your-security-key-settings-from-windows-settings"></a>Güvenlik anahtarı ayarlarınızı Windows ayarlarından yönetme

Güvenlik anahtarınızı sıfırlama ve yeni bir güvenlik anahtarı PIN 'ı oluşturma da dahil olmak üzere, **Windows ayarları** uygulamasından güvenlik anahtarı ayarlarınızı yönetebilirsiniz.

### <a name="reset-your-security-key"></a>Güvenlik anahtarınızı sıfırlayın

Fiziksel güvenlik anahtarınıza depolanan tüm hesap bilgilerini silmek istiyorsanız, anahtarı fabrika varsayılan ayarlarına geri dönmeniz gerekir. Güvenlik anahtarınızı sıfırlamak anahtardan her şeyi siler, böylece baştan başlayabilirsiniz.

>[!IMPORTANT]
>Güvenlik anahtarınızı sıfırlama, anahtardan her şeyi, fabrika varsayılanlarına sıfırlayarak her şeyi siler.
>
> **Tüm veriler ve kimlik bilgileri temizlenir.**

#### <a name="to-reset-your-security-key"></a>Güvenlik anahtarınızı sıfırlamak için

1. Windows ayarları uygulamasını açın, **hesaplar**' ı seçin, **oturum açma seçenekleri**' ni seçin, **güvenlik anahtarı**' nı seçin ve ardından **Yönet**' i seçin.

2. Kimlik bilgilerinizi doğrulamak için, USB bağlantı noktasına güvenlik anahtarınızı ekleyin veya NFC okuyucunuz ' e dokunun.

3. Belirli güvenlik anahtarı üreticinize göre ekrandaki yönergeleri izleyin. Anahtar üreticiniz ekran yönergelerinde listelenmiyorsa daha fazla bilgi için üreticinin sitesine bakın.

4. **Yönet** ekranını kapatmak için **Kapat** ' ı seçin.

### <a name="create-a-new-security-key-pin"></a>Yeni bir güvenlik anahtarı PIN 'ı oluştur

Güvenlik anahtarınız için yeni bir güvenlik anahtarı PIN kodu oluşturabilirsiniz.

#### <a name="to-create-a-new-security-key-pin"></a>Yeni bir güvenlik anahtarı PIN 'ı oluşturmak için

1. Windows ayarları uygulamasını açın, **hesaplar**' ı seçin, **oturum açma seçenekleri**' ni seçin, **güvenlik anahtarı**' nı seçin ve ardından **Yönet**' i seçin.

2. Kimlik bilgilerinizi doğrulamak için, USB bağlantı noktasına güvenlik anahtarınızı ekleyin veya NFC okuyucunuz ' e dokunun.
3. **Güvenlik anahtarı sabitleme** alanından **Ekle** ' yi seçin, yeni güvenlik anahtarı PIN 'inizi yazıp onaylayın ve sonra **Tamam**' ı seçin.

     Güvenlik anahtarı, iş veya okul hesabınızla kullanılmak üzere yeni güvenlik anahtarı PIN 'ı ile güncelleştirilir. PIN 'inizi yeniden değiştirmeye karar verirseniz, **Değiştir** düğmesini seçebilirsiniz.
4. **Yönet** ekranını kapatmak için **Kapat** ' ı seçin.

## <a name="additional-security-info-methods"></a>Ek güvenlik bilgileri yöntemleri

Bir güvenlik anahtarını kaydetmek için, en az bir ek güvenlik doğrulama yöntemine kaydolmalısınız. Daha fazla bilgi için [genel bakış bölümüne](security-info-add-update-methods-overview.md) bakın. 

## <a name="next-steps"></a>Sonraki adımlar

- Passwordless doğrulama yöntemleri hakkında daha fazla bilgi için [Microsoft 'un Azure AD 'de FIDO2 güvenlik anahtarlarının genel önizlemesine başlayan, passwordless oturum açma](https://www.onmsft.com/news/microsofts-azure-ad-begins-public-preview-of-fido2-security-keys-enabling-passwordless-logins) blogunu etkinleştiren veya [Microsoft Authenticator App ne olduğunu](user-help-auth-app-overview.md) ve [Windows Hello 'ya genel bakış](https://www.microsoft.com/windows/windows-hello) makalelerini okuyun.

- [Microsoft uyumlu güvenlik anahtarları](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/microsoft-compatible-security-key)hakkında daha ayrıntılı bilgi için.

- Parola [sıfırlama portalından](https://passwordreset.microsoftonline.com/) kayıp veya unuttuysanız parolanızı sıfırlayın veya [iş veya okul parolanızı sıfırlama](active-directory-passwords-update-your-own-password.md) makalesindeki adımları izleyin.

- [Microsoft hesabı makalesinde oturum açma](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) sorunları için sorun giderme ipuçları ve yardım alın.
