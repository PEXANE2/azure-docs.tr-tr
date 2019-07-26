---
title: Güvenlik anahtarı (Önizleme) kullanmak için güvenlik bilgilerini ayarlama-Azure Active Directory | Microsoft Docs
description: Hızlı kimlik çevrimiçi (FIDO2) güvenlik anahtarı kullanarak kimliğinizi doğrulamak için güvenlik bilgilerinizi ayarlama.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: librown
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d2f06b054e433c0320019548c56539d102beaad
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68386505"
---
# <a name="set-up-security-info-to-use-a-security-key-preview"></a>Güvenlik anahtarı kullanmak için güvenlik bilgilerini ayarlama (Önizleme)

Güvenlik anahtarlarını, kuruluşunuzda bir passwordless oturum açma yöntemi olarak kullanabilirsiniz. Güvenlik anahtarı, iş veya okul hesabınızda oturum açmak için benzersiz bir PIN ile kullanılan fiziksel bir cihazdır. Güvenlik anahtarlarına fiziksel cihaz ve yalnızca bildiğiniz bir şey gerektiğinden, bu, Kullanıcı adı ve paroladan daha güçlü bir kimlik doğrulama yöntemi olarak kabul edilir.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-key.md)]

>[!Important]
>Bu içerik kullanıcılara yöneliktir. Yöneticiyseniz, Azure Active Directory (Azure AD) ortamınızı ayarlama ve yönetme hakkında daha fazla bilgi için [Azure Active Directory Belgelerine](https://docs.microsoft.com/azure/active-directory) bakabilirsiniz.

## <a name="what-is-a-security-key"></a>Güvenlik anahtarı nedir?

Şu anda, [hızlı kimlik çevrimiçi (FIDO)](https://fidoalliance.org/fido2/) (FIDO2) parolasız kimlik doğrulama yöntemi kullanarak birkaç tasarımı ve güvenlik anahtarı sağlayıcısını destekliyoruz. Bu yöntem, kuruluşunuzun bulut tabanlı kaynaklarına ve desteklenen tarayıcılara erişim sağlamak için iş veya okul hesabınızda bir kez oturum açmanıza olanak tanır.

Yöneticiniz veya kuruluşunuz size iş veya okul hesabınız için ihtiyaç duydukları bir güvenlik anahtarı sağlar. Kullanabileceğiniz farklı güvenlik anahtarı türleri vardır, örneğin cihazınıza taktığınız USB anahtarı veya bir NFC okuyucusuna dokunmanız gereken bir NFC anahtarı vardır. Kuruluşunuzun belgelerinden, ne tür olduğunu da içeren güvenlik anahtarınız hakkında daha fazla bilgi edinebilirsiniz.

> [!Note]
> Bir FIDO2 güvenlik anahtarı kullandıysanız, Microsoft Authenticator uygulaması veya Windows Hello gibi kullanabileceğiniz diğer passwordless kimlik doğrulama yöntemleri vardır. Microsoft Authenticator uygulaması hakkında daha fazla bilgi için, bkz. [Microsoft Authenticator uygulama nedir?](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-overview). Windows Hello hakkında daha fazla bilgi için bkz. [Windows Hello 'ya genel bakış](https://www.microsoft.com/windows/windows-hello).

## <a name="before-you-begin"></a>Başlamadan önce

Güvenlik anahtarınızı kaydetmeye başlamadan önce aşağıdakileri göz önünde bulabilirsiniz:

- Yöneticiniz bu özelliği kuruluşunuzda kullanılmak üzere açmış.

- En az Windows 10, sürüm 1903 çalıştıran bir cihazımız ve Microsoft Edge tarayıcısını kullanıyorsunuz.

- Yöneticinizden veya kuruluşunuzun fiziksel bir güvenlik anahtarını aldınız. Güvenlik anahtarınız hem FIDO2 hem de Microsoft uyumlu olmalıdır. Güvenlik anahtarınız ve uyumlu olup olmadığı hakkında sorularınız varsa kuruluşunuzun yardım masasına başvurun.

## <a name="register-your-security-key"></a>Güvenlik anahtarınızı kaydedin

Anahtar kullanarak iş veya okul hesabınızda oturum açabilmeniz için, güvenlik anahtarınızı Windows ve benzersiz bir PIN ile çalışacak şekilde hazırlamanız gerekir.

1. https://myprofile.microsoft.com ' De **Profilim** sayfasına gidin ve henüz yapmadıysanız oturum açın.

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

Güvenlik anahtarınızı artık kullanmak istemiyorsanız, anahtarı güvenlik bilgilerinizden silebilirsiniz. Bu, güvenlik anahtarının iş veya okul hesabınızla kullanılmasını durdururken, güvenlik anahtarı verilerinizi ve kimlik bilgilerini depolamaya devam eder. Güvenlik anahtarından veri ve kimlik bilgisi bilgilerini silmek için, bu makaledeki [Microsoft uyumlu güvenlik anahtarını sıfırlama](#reset-your-security-key) bölümündeki yönergeleri izlemeniz gerekir.

1. Kaldırmak için güvenlik anahtarından **Sil** bağlantısını seçin.

2. **Güvenlik anahtarını Sil** kutusundan **Tamam ' ı** seçin.

    Güvenlik anahtarınız silinir ve artık iş veya okul hesabınızda oturum açmak için kullanamazsınız.

>[!Important]
>Bu güvenlik anahtarını yanlışlıkla sildiyseniz, bu makalenin [güvenlik anahtarınızı kaydetme](#register-your-security-key) bölümündeki yönergeleri kullanarak yeniden kaydetmeniz gerekir.

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

2. Kimlik bilgilerinizi doğrulamak için, USB bağlantı noktasına güvenlik anahtarınızı ekleyin veya NFC okuyucunuz ' e dokunun. 5
3. **Güvenlik anahtarı sabitleme** alanından **Ekle** ' yi seçin, yeni güvenlik anahtarı PIN 'inizi yazıp onaylayın ve sonra **Tamam**' ı seçin.

    Güvenlik anahtarı, iş veya okul hesabınızla kullanılmak üzere yeni güvenlik anahtarı PIN 'ı ile güncelleştirilir. PIN 'inizi yeniden değiştirmeye karar verirseniz **Değiştir** düğmesini seçebilirsiniz. 6
4. **Yönet** ekranını kapatmak için **Kapat** ' ı seçin.

## <a name="additional-security-info-methods"></a>Ek güvenlik bilgileri yöntemleri

Kuruluşunuzun, kimliğinizi doğrulamak için size ne olduğuna bağlı olarak kimliğinizi nasıl öğrendiğini öğrenmek için ek seçenekleriniz vardır. Seçeneklere şunlar dahildir:

- **Authenticator uygulaması.** İki adımlı doğrulama veya parola sıfırlama için bir onay bildirimi ya da rastgele oluşturulmuş bir onay kodu almak üzere bir Authenticator uygulaması indirin ve kullanın. Microsoft Authenticator uygulamasını ayarlama ve kullanma hakkında adım adım yönergeler için bkz. [güvenlik bilgilerini bir Authenticator uygulaması kullanmak Için ayarlama](security-info-setup-auth-app.md).

- **Mobil cihaz metni.** Mobil cihaz numaranızı girin ve iki adımlı doğrulama veya parola sıfırlama için kullanacağınız kodu bir metin alın. Bir SMS mesajı (SMS) ile kimliğinizi doğrulamaya yönelik adım adım yönergeler için bkz. [güvenlik bilgilerini ayarlama hakkında bilgi iletisi (SMS)](security-info-setup-text-msg.md).

- **Mobil cihaz veya iş telefonu çağrısı.** Mobil cihaz numaranızı girin ve iki adımlı doğrulama veya parola sıfırlama için bir telefon araması alın. Telefon numarası ile kimliğinizi doğrulama hakkında adım adım yönergeler için bkz. [telefon aramalarını kullanmak için güvenlik bilgilerini ayarlama](security-info-setup-phone-number.md).

- **E-posta adresi.** Parola sıfırlama için bir e-posta almak üzere iş veya okul e-posta adresinizi girin. Bu seçenek, iki adımlı doğrulama için kullanılamaz. E-postanızı ayarlama hakkında adım adım yönergeler için bkz. [e-posta kullanmak için güvenlik bilgilerini ayarlama](security-info-setup-email.md).

- **Güvenlik soruları.** Yöneticiniz tarafından kuruluşunuz için oluşturulan bazı güvenlik sorularını yanıtlayın. Bu seçenek yalnızca parola sıfırlama için kullanılabilir ve iki adımlı doğrulama için kullanılamaz. Güvenlik sorularınızı ayarlama hakkında adım adım yönergeler için Güvenlik [sorularını kullanmak üzere güvenlik bilgilerini ayarlama](security-info-setup-questions.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Passwordless kimlik doğrulama yöntemleri hakkında daha fazla bilgi için [Microsoft 'un Azure AD 'de FIDO2 güvenlik anahtarlarının genel önizlemesine başlayan, passwordless oturum açma](https://www.onmsft.com/news/microsofts-azure-ad-begins-public-preview-of-fido2-security-keys-enabling-passwordless-logins) blogunu etkinleştiren veya [Microsoft Authenticator uygulamasının](https://docs.microsoft.com/azure/active-directory/user-help8user-help-auth-app-overview) [ne olduğunu okuduğunuzu okuyun. Windows Hello genel bakış](https://www.microsoft.com/windows/windows-hello) makaleleri.

- [Microsoft uyumlu güvenlik anahtarları](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/microsoft-compatible-security-key)hakkında daha ayrıntılı bilgi için.

- Parola [sıfırlama portalından](https://passwordreset.microsoftonline.com/) kayıp veya unuttuysanız parolanızı sıfırlayın veya [iş veya okul parolanızı sıfırlama](user-help-reset-password.md) makalesindeki adımları izleyin.

- [Microsoft hesabı makalesinde oturum açma](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) sorunları için sorun giderme ipuçları ve yardım alın.
