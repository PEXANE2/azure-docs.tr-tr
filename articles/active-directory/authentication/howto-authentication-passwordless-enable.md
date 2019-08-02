---
title: Azure Active Directory parolasız oturum açma (Önizleme) yapılandırma
description: FIDO2 güvenlik anahtarlarını veya Microsoft Authenticator uygulamasını (Önizleme) kullanarak Azure AD 'de passwordless oturum açmayı etkinleştirme
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: cce5de98fe4914e18ee0154d4ac5f1af008d32a6
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68561298"
---
# <a name="enable-passwordless-sign-in-for-azure-ad-preview"></a>Azure AD için passwordless oturum açma özelliğini etkinleştirme (Önizleme)

## <a name="requirements"></a>Gereksinimler

* Azure Multi-Factor Authentication
* SSPR için etkinleştirilen kullanıcılar ile Birleşik kayıt önizlemesi
* FIDO2 güvenlik anahtarı önizlemesi uyumlu FIDO2 güvenlik anahtarları gerektirir
* WebAuthN, Windows 10 sürüm 1809 veya üzeri sürümlerde Microsoft Edge gerektirir
* FIDO2 tabanlı Windows oturum açma, Azure AD 'ye katılmış Windows 10 sürüm 1809 veya üstünü gerektirir

## <a name="prepare-devices-for-preview"></a>Cihazları önizleme için hazırlama

İle pillendirilecektir, Windows 10 sürüm 1809 veya üstünü çalıştırıyor olmalıdır. En iyi deneyim Windows 10 sürüm 1903 veya daha yüksektir.

## <a name="enable-security-keys-for-windows-sign-in"></a>Windows oturum açma için güvenlik anahtarlarını etkinleştir

Kuruluşlar, Windows oturum açma için güvenlik anahtarlarının kullanımını etkinleştirmek üzere aşağıdaki yöntemlerden birini veya daha fazlasını kullanmayı seçebilir.

### <a name="enable-credential-provider-via-intune"></a>Intune aracılığıyla kimlik bilgisi sağlayıcısını etkinleştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Cihaz kaydı** **Windows kaydı** > **iş için Windows Hello** **özelliklerini**Microsoft Intune gidin. >  >  > 
1. **Ayarlar** ' ın altında **, oturum açma Için güvenlik anahtarlarını kullan '** ın altında **etkinleştirilmesi**.

Oturum açma için güvenlik anahtarlarının yapılandırılması, Iş için Windows Hello 'Yu yapılandırmaya bağlı değildir.

#### <a name="enable-targeted-intune-deployment"></a>Hedeflenen Intune dağıtımını etkinleştir

Kimlik bilgisi sağlayıcısını etkinleştirmek üzere belirli cihaz gruplarını hedeflemek için, Intune aracılığıyla aşağıdaki özel ayarları kullanın. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Microsoft Intune**cihazyapılandırma > **profilleri**profil**Oluştur**' a gidin. >  > 
1. Yeni profili aşağıdaki ayarlarla yapılandırın
   1. Adı: Windows oturum açma için güvenlik anahtarları
   1. Açıklama: Windows oturum açma sırasında FIDO güvenlik anahtarlarının kullanılmasını sağlar
   1. Platform: Windows 10 ve üzeri
   1. Platform türü: Özel
   1. Özel OMA-URI ayarları:
      1. Adı: Windows oturum açma için FIDO güvenlik anahtarlarını aç
      1. OMA-URI:./Device/Vendor/MSFT/passportforwork/SecurityKey/usesecuritykeyforsignın
      1. Veri türü: Tamsayı
      1. Değer: 1. 
1. Bu ilke belirli kullanıcılara, cihazlara veya gruplara atanabilir. [Microsoft Intune, Kullanıcı ve cihaz profilleri atama](https://docs.microsoft.com/intune/device-profile-assign)makalesinde daha fazla bilgi bulunabilir.

![Intune özel cihaz yapılandırma ilkesi oluşturma](./media/howto-authentication-passwordless-enable/intune-custom-profile.png)

### <a name="enable-credential-provider-via-provisioning-package"></a>Sağlama paketi aracılığıyla kimlik bilgisi sağlayıcısını etkinleştir

Intune tarafından yönetilmeyen cihazlar için, işlevselliği etkinleştirmek üzere bir sağlama paketi yükleyebilirsiniz. Windows yapılandırma Tasarımcısı uygulaması [Microsoft Store](https://www.microsoft.com/store/apps/9nblggh4tx22)yüklenebilir.

1. Windows yapılandırma Tasarımcısı 'nı başlatın.
1. **Dosya** > **Yeni proje**' yi seçin.
1. Projenize bir ad verin ve projenizin oluşturulduğu yolu bir yere göz atın.
1. **İleri**’yi seçin.
1. **Seçili proje iş akışı** olarak **sağlama paketini** seçili bırakın ve **İleri ' yi**seçin.
1. Görüntülenecek **ve yapılandırılacak ayarları seçin** altında **tüm Windows masaüstü sürümlerini** seçin ve **İleri ' yi**seçin.
1. **Son**’u seçin.
1. Yeni oluşturduğunuz projenizde, **çalışma zamanı ayarları** > **windowshelloforbusiness** > **SecurityKeys** > **usesecuritykeyforsign'** e gidin.
1. **Usesecuritykeyforsignın** öğesini **etkin**olarak ayarlayın.
1. **Sağlama paketini** **dışarı aktar** > 'ı seçin
1. **Sağlama paketini açıkla** altında **derleme** penceresinde varsayılan değerleri bırakın ve **İleri ' yi**seçin.
1. **Sağlama paketinin güvenlik ayrıntılarını Seç** altında **derleme** penceresinde varsayılan değerleri bırakın ve **İleri ' yi**seçin.
1. **Sağlama paketinin kaydedileceği yeri seçin** ' ın altında **derleme** pencereleri ' ne göz atın veya yolu değiştirin ve **İleri ' yi**seçin.
1. **Sağlama paketini oluşturma** sayfasında **Oluştur** ' u seçin.
1. Oluşturulan iki dosyayı (ppkg ve Cat), daha sonra makinelere uygulayabileceğiniz bir konuma kaydedin.
1. Oluşturduğunuz sağlama paketini uygulamak için [bir sağlama paketi uygulama](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-apply-package)makalesindeki yönergeleri izleyin.

## <a name="obtain-fido2-security-keys"></a>FIDO2 güvenlik anahtarlarını alma

Desteklenen anahtarlar ve üreticiler hakkında daha fazla [bilgi için,](concept-authentication-passwordless.md) FIDO2 güvenlik anahtarları bölümüne bakın.

> [!NOTE]
> Satınalma ve NFC tabanlı güvenlik anahtarları kullanmayı planlıyorsanız, desteklenen bir NFC okuyucusuna ihtiyacınız olacaktır.

## <a name="enable-passwordless-authentication-methods"></a>Passwordless kimlik doğrulama yöntemlerini etkinleştir

### <a name="enable-the-combined-registration-experience"></a>Birleşik kayıt deneyimini etkinleştir

FIDO2 güvenlik anahtarlarına yönelik kayıt özellikleri, Birleşik kayıt önizlemesine dayanır. Birleşik kayıt önizlemesini etkinleştirmek için aşağıdaki adımları izleyin.

1. [Azure portalda](https://portal.azure.com) oturum açma
1. **Azure Active Directory** > **Kullanıcı ayarlarına** gidin
   1. **Kullanıcı özelliği Önizleme ayarlarını yönet** ' e tıklayın.
   1. **Kullanıcılar altında, güvenlik bilgilerini kaydetmek ve yönetmek için Önizleme özelliklerini kullanabilir-geliştirildi**.
      1. **Seçili** ' ı seçin ve önizlemeye katılacak kullanıcı grubunu seçin.
      1. Ya da dizininizdeki herkes için etkinleştirmek üzere **Tümü** seçeneğini belirleyin.
1. **Kaydet**'e tıklayın.

### <a name="enable-new-passwordless-authentication-methods"></a>Yeni passwordless kimlik doğrulama yöntemlerini etkinleştir

1. [Azure portalda](https://portal.azure.com) oturum açma
1. **Azure Active Directory** > **güvenlik** **kimlik doğrulama yöntemleri kimlik doğrulama** **yöntemi ilkesini inceleyin (Önizleme)**  >  > 
1. Her **yöntemin**altında, aşağıdaki seçenekleri belirleyin
   1. **Etkinleştir** -Evet veya Hayır
   1. **Hedef** -tüm kullanıcılar veya kullanıcıları seçin
1. Her yöntemi **Kaydet**

> [!WARNING]
> FIDO2 "anahtar kısıtlama ilkeleri" henüz çalışmıyor. Bu işlevsellik genel kullanıma sunulmadan önce kullanılabilir olacaktır, lütfen bu ilkeleri varsayılan olarak değiştirmeyin.

> [!NOTE]
> Passwordless yöntemlerinin her ikisini de kabul etmeniz gerekmez (yalnızca bir passwordless yönteminin önizlemesini yapmak istiyorsanız, yalnızca bu yöntemi etkinleştirebilirsiniz). Her iki yöntem de kendi avantajlarına sahip olduğundan her iki yöntemi de denemenizi öneririz.

## <a name="user-registration-and-management-of-fido2-security-keys"></a>FIDO2 güvenlik anahtarlarının Kullanıcı kaydı ve yönetimi

1. Buraya gidin[https://myprofile.microsoft.com](https://myprofile.microsoft.com)
1. Henüz yoksa oturum aç
1. **Güvenlik bilgileri** ' ne tıklayın
   1. Kullanıcı zaten en az bir Azure Multi-Factor Authentication yöntemi kayıtlıysa, bir FIDO2 güvenlik anahtarını hemen kaydedebilirler.
   1. Kayıtlı en az bir Azure Multi-Factor Authentication yöntemi yoksa, bir tane eklemesi gerekir.
1. **Yöntem Ekle** ' ye tıklayıp **güvenlik anahtarı** ' nı seçerek bir FIDO2 güvenlik anahtarı ekleyin
1. **USB cihazı** veya **NFC cihazını** seçin
1. Anahtarınızı hazırlayın ve **İleri ' yi** seçin
1. Bir kutu görünür ve güvenlik anahtarınız için bir PIN oluşturmanız/girmeniz istenir ve ardından anahtarınız için Biyometri ya da Touch için gerekli hareketi gerçekleştirirsiniz.
1. Birleşik kayıt deneyimine döndürülür ve belirtebilmeniz için anlamlı bir ad girmeniz istenir. bu sayede, birden çok           **İleri**'ye tıklayın.
1. İşlemi gerçekleştirmek için **bitti** ' ye tıklayın

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Güvenlik anahtarını biyometrik, PIN veya sıfırlama güvenlik anahtarını yönetme

* Windows 10 sürüm 1809
   * Güvenlik anahtarı satıcısından yardımcı yazılım gereklidir
* Windows 10 sürüm 1903 veya üzeri
   * Kullanıcılar, cihaz > **hesapları** > **güvenlik anahtarı** 'nda **Windows ayarlarını** açabilir
   * Kullanıcılar PIN kodlarını değiştirebilir, Biyometri güncelleştirebilir veya güvenlik anahtarını sıfırlayabilir

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasının Kullanıcı kaydı ve yönetimi

Microsoft Authenticator uygulamasını telefonla oturum açma için yapılandırmak için, [Microsoft Authenticator uygulamasını kullanarak hesaplarınızda oturum açma](../user-help/user-help-auth-app-sign-in.md)makalesindeki yönergeleri izleyin.

## <a name="sign-in-with-passwordless-credentials"></a>Passwordless kimlik bilgileriyle oturum açın

### <a name="sign-in-at-the-lock-screen"></a>Kilit ekranında oturum açın

Aşağıdaki örnekte bala Sandhu kullanıcısı, FIDO2 güvenlik anahtarını zaten sağladı. Bala, Windows 10 kilit ekranından güvenlik anahtarı kimlik bilgileri sağlayıcısını seçebilir ve Windows 'da oturum açmak için güvenlik anahtarını ekleyebilir.

![Windows 10 kilit ekranında güvenlik anahtarı oturum açma](./media/howto-authentication-passwordless-enable/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="sign-in-on-the-web"></a>Web 'de oturum açın

Aşağıdaki örnekte, bir Kullanıcı FIDO2 güvenlik anahtarını zaten sağladı. Kullanıcı, Windows 10 sürüm 1809 veya üzeri sürümlerde Microsoft Edge tarayıcısının içindeki FIDO2 güvenlik anahtarıyla Web 'de oturum açmayı seçebilir.

![Microsoft Edge 'de güvenlik anahtarı oturum açma](./media/howto-authentication-passwordless-enable/fido2-windows-10-1903-edge-sign-in.png)

Microsoft Authenticator uygulamasını kullanarak oturum açma hakkında daha fazla bilgi için, [Microsoft Authenticator uygulamasını kullanarak hesaplarınızda oturum açın](../user-help/user-help-auth-app-sign-in.md)makalesini inceleyin.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="fido2-security-keys"></a>FIDO2 güvenlik anahtarları

#### <a name="security-key-provisioning"></a>Güvenlik anahtarı sağlama

Güvenlik anahtarlarının yönetici tarafından sağlanması ve devre dışı sağlanması genel önizlemede bulunmamaktadır.

#### <a name="hybrid-azure-ad-join"></a>Hibrit Azure AD'ye katılma

FIDO2 güvenlik anahtarları veya passwordless Microsoft Authenticator oturum açma gibi yönetilen kimlik bilgilerini kullanan WIA SSO 'ya bağlı kullanıcılar SSO avantajlarından faydalanmak için Windows 10 ' da karma birleşime gerek duyar. Ancak, güvenlik anahtarları yalnızca Azure Active Directory katılmış makineler için çalışır. Yalnızca saf Azure Active Directory katılmış makinelerde Windows kilit ekranı için FIDO2 güvenlik anahtarlarını denemenizi öneririz. Bu sınırlama Web için geçerlidir.

#### <a name="upn-changes"></a>UPN değişiklikleri

Karma ASıFATı ve ASıFAA cihazlarında UPN değişikliğine izin veren bir özelliği desteklemeye çalışıyoruz. Bir kullanıcının UPN 'si değişirse FIDO2 güvenlik anahtarlarını artık bu hesaba göre değiştiremezsiniz. Yani tek yaklaşım, cihazı sıfırlamadır ve kullanıcının yeniden kaydolması yeterlidir.

### <a name="authenticator-app"></a>Authenticator uygulaması

#### <a name="ad-fs-integration"></a>AD FS tümleştirme

Kullanıcı Microsoft Authenticator parolasız kimlik bilgisini etkinleştirmişse, bu kullanıcı için kimlik doğrulaması her zaman onay için bir bildirim gönderir. Bu mantık, karma Kiracıdaki kullanıcıların, Kullanıcı yerine "parolanızı kullan" seçeneğine tıklamadan oturum açma doğrulaması için ADFS 'ye yönlendirilmesini engeller. Bu işlem, şirket içi koşullu erişim ilkelerinin yanı sıra geçişli kimlik doğrulama akışlarını da atlar. Bu işlemin özel durumu bir login_hint belirtilmişse, bir kullanıcı AD FS olarak yeniden iletilir ve passwordless kimlik bilgisini kullanma seçeneğini atlar.

#### <a name="azure-mfa-server"></a>Azure MFA sunucusu

Bir kuruluşun şirket içi Azure MFA sunucusu aracılığıyla MFA için etkinleştirilen son kullanıcılar, yine de tek bir parolasız telefon oturum açma kimlik bilgisi oluşturabilir ve kullanabilir. Kullanıcı, kimlik bilgileriyle Microsoft Authenticator birden çok yüklemeyi (5 +) yükseltmeyi denerse, bu değişiklik hataya neden olabilir.  

#### <a name="device-registration"></a>Cihaz kaydı

Bu yeni, güçlü kimlik bilgisini oluşturmak için önkoşullardan biri, bulunduğu cihazın Azure AD kiracısında tek bir kullanıcıya kayıtlı olması olabilir. Cihaz Kayıt kısıtlamaları nedeniyle, bir cihaz yalnızca tek bir kiracıda kaydedilebilir. Bu sınır, Microsoft Authenticator uygulamasındaki yalnızca bir iş veya okul hesabının, telefonla oturum açma için etkinleştiribileceği anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

[Cihaz kaydı hakkında bilgi edinin](../devices/overview.md)

[Azure Multi-Factor Authentication hakkında bilgi edinin](../authentication/howto-mfa-getstarted.md)
