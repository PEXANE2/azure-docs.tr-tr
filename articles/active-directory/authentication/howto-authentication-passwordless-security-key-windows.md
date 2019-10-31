---
title: Azure AD (Önizleme) için passwordless güvenlik anahtarı oturum açma özelliğini etkinleştirme-Azure Active Directory
description: FIDO2 güvenlik anahtarlarını (Önizleme) kullanarak Azure AD 'de passwordless güvenlik anahtarı oturum açma özelliğini etkinleştirme
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: b5758b1fbb9d311219e3dc4dd483691f6c9d80c1
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73172175"
---
# <a name="enable-passwordless-security-key-sign-in-to-windows-10-devices-preview"></a>Windows 10 cihazlarında passwordless güvenlik anahtarı oturum açma özelliğini etkinleştir (Önizleme)

Bu belge, Windows 10 cihazlarıyla, FIDO2 güvenlik anahtarı tabanlı passwordless kimlik doğrulamasını etkinleştirmeye odaklanır. Bu makalenin sonunda, bir FIDO2 güvenlik anahtarı kullanarak hem Web tabanlı uygulamalarda hem de Azure AD 'ye katılmış Windows 10 cihazlarında Azure AD hesabınızla oturum açabilirsiniz.

|     |
| --- |
| FIDO2 güvenlik anahtarları Azure Active Directory genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="requirements"></a>Gereksinimler

| Cihaz türü | Azure AD 'ye katılmış | Karma Azure AD 'ye katılmış |
| --- | --- | --- |
| [Azure Multi-Factor Authentication](howto-mfa-getstarted.md) | X | X |
| [Birleşik güvenlik bilgileri kayıt önizlemesi](concept-registration-mfa-sspr-combined.md) | X | X |
| Uyumlu [FIDO2 güvenlik anahtarları](concept-authentication-passwordless.md#fido2-security-keys) | X | X |
| WebAuthN, Windows 10 sürüm 1809 veya üstünü gerektirir | X | X |
| [Azure AD 'ye katılmış cihazlar](../devices/concept-azure-ad-join.md) için Windows 10 sürüm 1809 veya üzeri gerekir | X |   |
| [Karma Azure AD 'ye katılmış cihazlar](../devices/concept-azure-ad-join-hybrid.md) için Windows 10 ınsider Build 18945 veya üzeri gerekir |   | X |
| Tam düzeltme eki uygulanan Windows Server 2016/2019 etki alanı denetleyicileri. |   | X |
| [Azure AD Connect](../hybrid/how-to-connect-install-roadmap.md#install-azure-ad-connect) en son sürümüne yükseltin |   | X |
| [Microsoft Intune](https://docs.microsoft.com/intune/fundamentals/what-is-intune) (isteğe bağlı) | X | X |
| Sağlama paketi (Isteğe bağlı) | X | X |
| Grup ilkesi (Isteğe bağlı) |   | X |

### <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

- Windows Server Active Directory Domain Services (AD DS) etki alanına katılmış (Şirket içi cihazlar) dağıtımı **desteklenmez**.
- RDP, VDı ve Citrix senaryoları güvenlik anahtarı kullanılarak **desteklenmez** .
- Güvenlik anahtarı kullanılarak S/MIME **desteklenmez** .
- "Farklı çalıştır" güvenlik anahtarı kullanılarak **desteklenmez** .
- Güvenlik anahtarı kullanılarak bir sunucuda oturum açma **desteklenmez**.
- Çevrimiçi ortamda cihazınızda oturum açmak için güvenlik anahtarınızı kullanmadıysanız, çevrimdışı oturum açmak veya oturumu açmak için kullanamazsınız.

## <a name="prepare-devices-for-preview"></a>Cihazları önizleme için hazırlama

İle pillendirilebilecek Azure AD 'ye katılmış cihazların Windows 10 sürüm 1809 veya üstünü çalıştırması gerekir. En iyi deneyim Windows 10 sürüm 1903 veya daha yüksektir.

İle pillendirilebilecek karma Azure AD 'ye katılmış cihazların Windows 10 Insider Build 18945 veya daha yeni bir sürümü çalıştırması gerekir.

## <a name="enable-security-keys-for-windows-sign-in"></a>Windows oturum açma için güvenlik anahtarlarını etkinleştir

Kuruluşlar, kuruluşunuzun gereksinimlerine bağlı olarak Windows oturum açma için güvenlik anahtarlarının kullanımını etkinleştirmek üzere aşağıdaki yöntemlerden birini veya daha fazlasını kullanmayı seçebilir.

- [Intune ile etkinleştirme](#enable-with-intune)
   - [Hedeflenen Intune dağıtımı](#targeted-intune-deployment)
- [Sağlama paketiyle etkinleştir](#enable-with-a-provisioning-package)
- [Grup ilkesi ile etkinleştir (yalnızca karma Azure AD 'ye katılmış cihazlar)](#enable-with-group-policy)

> [!IMPORTANT]
> **Karma Azure AD 'ye katılmış cihazlara** sahip kuruluşlar, WINDOWS 10 FIDO2 güvenlik anahtarı kimlik doğrulamasının çalışması [için önce ŞIRKET içi kaynaklarda FIDO2 kimlik doğrulamasını etkinleştirme](howto-authentication-passwordless-security-key-on-premises.md) makalesindeki adımları **da** tamamlamalıdır.
>
> **Azure AD 'ye katılmış cihazlara** sahip kuruluşlar, cihazların FIDO2 güvenlik anahtarlarıyla şirket içi kaynaklarda kimlik doğrulaması yapabilmesi için bunu yapması gerekir.

### <a name="enable-with-intune"></a>Intune ile etkinleştirme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. **Microsoft Intune** > **cihaz kaydına** gidin > **Windows kaydı** > **iş için Windows Hello** > **özellikleri**.
1. **Ayarlar** ' ın altında **, oturum açma Için güvenlik anahtarlarını kullan '** ın altında **etkinleştirilmesi**.

Oturum açma için güvenlik anahtarlarının yapılandırılması, Iş için Windows Hello 'Yu yapılandırmaya bağlı değildir.

#### <a name="targeted-intune-deployment"></a>Hedeflenen Intune dağıtımı

Kimlik bilgisi sağlayıcısını etkinleştirmek üzere belirli cihaz gruplarını hedeflemek için, Intune aracılığıyla aşağıdaki özel ayarları kullanın.

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. **Profil oluşturmak** >  > **cihaz yapılandırması** > **profillerine** gidin **Microsoft Intune** .
1. Yeni profili aşağıdaki ayarlarla yapılandırın
   1. Ad: Windows oturum açma için güvenlik anahtarları
   1. Açıklama: Windows oturum açma sırasında FIDO güvenlik anahtarlarının kullanılmasını sağlar
   1. Platform: Windows 10 ve üzeri
   1. Profil türü: özel
   1. Özel OMA-URI ayarları:
      1. Ad: Windows oturum açma için FIDO güvenlik anahtarlarını açın
      1. OMA-URI:./Device/Vendor/MSFT/passportforwork/SecurityKey/usesecuritykeyforsignın
      1. Veri türü: tamsayı
      1. Değer: 1
1. Bu ilke belirli kullanıcılara, cihazlara veya gruplara atanabilir. [Microsoft Intune, Kullanıcı ve cihaz profilleri atama](https://docs.microsoft.com/intune/device-profile-assign)makalesinde daha fazla bilgi bulunabilir.

![Intune özel cihaz yapılandırma ilkesi oluşturma](./media/howto-authentication-passwordless-security-key/intune-custom-profile.png)

### <a name="enable-with-a-provisioning-package"></a>Sağlama paketiyle etkinleştir

Intune tarafından yönetilmeyen cihazlar için, işlevselliği etkinleştirmek üzere bir sağlama paketi yükleyebilirsiniz. Windows yapılandırma Tasarımcısı uygulaması [Microsoft Store](https://www.microsoft.com/store/apps/9nblggh4tx22)yüklenebilir.

1. Windows yapılandırma Tasarımcısı 'nı başlatın.
1. **Dosya** > **Yeni proje**' yi seçin.
1. Projenize bir ad verin ve projenizin oluşturulduğu yolu bir yere göz atın.
1. **İleri**’yi seçin.
1. **Seçili proje iş akışı** olarak **sağlama paketini** seçili bırakın ve **İleri ' yi**seçin.
1. Görüntülenecek **ve yapılandırılacak ayarları seçin** altında **tüm Windows masaüstü sürümlerini** seçin ve **İleri ' yi**seçin.
1. **Son**’u seçin.
1. Yeni oluşturduğunuz projenizde, **Windowshelloforbusiness** > **SecurityKeys** > **Usesecuritykeyforsign > ** **çalışma zamanı ayarları** ' na gidin.
1. **Usesecuritykeyforsignın** öğesini **etkin**olarak ayarlayın.
1.  > **sağlama paketini** **dışarı aktar** 'ı seçin
1. **Sağlama paketini açıkla** altında **derleme** penceresinde varsayılan değerleri bırakın ve **İleri ' yi**seçin.
1. **Sağlama paketinin güvenlik ayrıntılarını Seç** altında **derleme** penceresinde varsayılan değerleri bırakın ve **İleri ' yi**seçin.
1. **Sağlama paketinin kaydedileceği yeri seçin** ' ın altında **derleme** pencereleri ' ne göz atın veya yolu değiştirin ve **İleri ' yi**seçin.
1. **Sağlama paketini oluşturma** sayfasında **Oluştur** ' u seçin.
1. Oluşturulan iki dosyayı (ppkg ve Cat), daha sonra makinelere uygulayabileceğiniz bir konuma kaydedin.
1. Oluşturduğunuz sağlama paketini uygulamak için [bir sağlama paketi uygulama](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-apply-package)makalesindeki yönergeleri izleyin.

> [!NOTE]
> Windows 10 sürüm 1809 çalıştıran cihazların Ayrıca paylaşılan bılgısayar modunu (EnableSharedPCMode) etkinleştirmesi gerekir. Bu özellikleri etkinleştirme hakkında bilgi için, [Windows 10 ile paylaşılan veya Konuk BIR bilgisayar ayarlama](https://docs.microsoft.com/windows/configuration/set-up-shared-or-guest-pc)makalesinde bulabilirsiniz.

### <a name="enable-with-group-policy"></a>grup ilkesi ile etkinleştir

**Karma Azure AD 'ye katılmış cihazlar** için kuruluşlar, Fido güvenlik anahtarı oturum açma özelliğini etkinleştirmek üzere aşağıdaki Grup İlkesi ayarını yapılandırabilir.

Ayar, **Yönetim Şablonları** > **bilgisayar yapılandırması** altında bulunabilir > **sistem** > **oturum açma** > **güvenlik anahtarı oturum açma özelliğini açın**.

- Bu ilkenin **etkin** olarak ayarlanması, kullanıcıların güvenlik anahtarlarıyla oturum açmalarına olanak tanır.
- Bu ilkenin **devre dışı** veya **yapılandırılmamış** olarak ayarlanması, kullanıcıların güvenlik anahtarlarıyla oturum açmasını durdurur.

Bu grup ilkesi ayarı, `credentialprovider.admx` grup ilkesi şablonunun güncelleştirilmiş bir sürümünü gerektirir. Bu yeni şablon, Windows Server 'ın bir sonraki sürümüyle ve Windows 10 20 H1 ile kullanılabilir. Bu ayar, Windows 'un bu yeni sürümlerinden birini çalıştıran bir cihazla veya destek konusundaki kılavuzu izleyerek, [Windows 'ta grup ilkesi Yönetim Şablonları Için merkezi mağaza oluşturma ve yönetme](https://support.microsoft.com/help/3087759/how-to-create-and-manage-the-central-store-for-group-policy-administra)ile yönetilebilir.

## <a name="sign-in-with-fido2-security-key"></a>FIDO2 güvenlik anahtarıyla oturum açın

Aşağıdaki örnekte, Bala Sandhu bir Kullanıcı önceki makaledeki adımları kullanarak FIDO2 güvenlik anahtarını sağladı, [passwordless güvenlik anahtarı oturum açma özelliğini etkinleştirin](howto-authentication-passwordless-security-key.md#user-registration-and-management-of-fido2-security-keys). Bala, Windows 10 kilit ekranından güvenlik anahtarı kimlik bilgileri sağlayıcısını seçebilir ve Windows 'da oturum açmak için güvenlik anahtarını ekleyebilir.

![Windows 10 kilit ekranında güvenlik anahtarı oturum açma](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Güvenlik anahtarını biyometrik, PIN veya sıfırlama güvenlik anahtarını yönetme

* Windows 10 sürüm 1903 veya üzeri
   * Kullanıcılar, **Windows ayarlarını** cihaz > **hesaplarında** açabilir > **güvenlik anahtarı**
   * Kullanıcılar PIN kodlarını değiştirebilir, Biyometri güncelleştirebilir veya güvenlik anahtarını sıfırlayabilir

## <a name="troubleshooting-and-feedback"></a>Sorun giderme ve geri bildirim

Bu özelliğin önizlemesini yaparken geri bildirimde bulunmak veya sorun yaşarsanız, lütfen Windows Geri Bildirim Hub 'ı uygulaması aracılığıyla paylaşabilirsiniz.

1. **Geri Bildirim Hub 'ını** başlatın ve oturum açtığınızdan emin olun.
1. Aşağıdaki kategoriye göre geri bildirim gönderin:
   1. Kategori: güvenlik ve Gizlilik
   1. Alt Kategori: FıDO
1. Günlükleri yakalamak için, şu seçeneği kullanın: **sorunum yeniden oluştur**

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD ve karma Azure AD 'ye katılmış cihazlar için şirket içi kaynaklara erişimi etkinleştirme](howto-authentication-passwordless-security-key-on-premises.md)

[Cihaz kaydı hakkında daha fazla bilgi edinin](../devices/overview.md)

[Azure Multi-Factor Authentication hakkında daha fazla bilgi](../authentication/howto-mfa-getstarted.md)
