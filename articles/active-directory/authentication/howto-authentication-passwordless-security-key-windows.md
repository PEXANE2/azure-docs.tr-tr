---
title: Parolasız güvenlik anahtarı oturum açma Windows - Azure Active Directory
description: FIDO2 güvenlik anahtarlarını kullanarak Azure Active Directory'de parolasız güvenlik anahtarı oturum açmayı nasıl etkinleştirin (önizleme) öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 01/30/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b8f5d6aaa96c24eb37eb78d237a489f1d25293c
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80653994"
---
# <a name="enable-passwordless-security-key-sign-in-to-windows-10-devices-with-azure-active-directory-preview"></a>Azure Active Directory (önizleme) ile Windows 10 cihazlarında parolasız güvenlik anahtarı oturum açmayı etkinleştirme

Bu belge, Windows 10 aygıtlarıyla FIDO2 güvenlik anahtarı tabanlı parolasız kimlik doğrulamasını etkinleştirme üzerinde odaklanır. Bu makalenin sonunda, bir FIDO2 güvenlik anahtarı kullanarak Azure AD hesabınızla birlikte Azure AD'ınıza ve karma Azure AD'ınızda oturum açabilirsiniz.

|     |
| --- |
| FIDO2 güvenlik anahtarları, Azure Etkin Dizini'nin genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bakın|
|     |

## <a name="requirements"></a>Gereksinimler

| Cihaz Türü | Azure AD'ye katılanlar | Hibrit Azure AD'ye katılmış |
| --- | --- | --- |
| [Azure Çok Faktörlü Kimlik Doğrulama](howto-mfa-getstarted.md) | X | X |
| [Birleşik güvenlik bilgileri kaydı önizlemesi](concept-registration-mfa-sspr-combined.md) | X | X |
| Uyumlu [FIDO2 güvenlik tuşları](concept-authentication-passwordless.md#fido2-security-keys) | X | X |
| WebAuthN Windows 10 sürüm 1809 veya daha yüksek gerektirir | X | X |
| [Azure AD'ye katılan aygıtlar](../devices/concept-azure-ad-join.md) Windows 10 sürüm 1903 veya daha yüksek gerektirir | X |   |
| [Karma Azure AD birleştirilmiş aygıtlar](../devices/concept-azure-ad-join-hybrid.md) Windows 10 Insider Build 18945 veya daha yüksek gerektirir |   | X |
| Tamamen yamalı Windows Server 2016/2019 Etki Alanı Denetleyicileri. |   | X |
| [Azure AD Connect](../hybrid/how-to-connect-install-roadmap.md#install-azure-ad-connect) sürümü 1.4.32.0 veya sonrası |   | X |
| [Microsoft Intune](https://docs.microsoft.com/intune/fundamentals/what-is-intune) (İsteğe Bağlı) | X | X |
| Sağlama paketi (İsteğe Bağlı) | X | X |
| Grup İlkesi (İsteğe Bağlı) |   | X |

### <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Aşağıdaki senaryolar desteklenmez:

- Windows Server Active Directory Domain Services (AD DS) etki alanına katıldı (yalnızca şirket içi aygıtlar) dağıtımı.
- Güvenlik anahtarı kullanarak RDP, VDI ve Citrix senaryoları.
- Güvenlik anahtarı nı kullanarak S/MIME.
- Güvenlik anahtarı nı kullanarak "Çalıştır" olarak çalıştırın.
- Güvenlik anahtarı nı kullanarak sunucuda oturum açın.
- Çevrimiçi yken cihazınızda oturum açmak için güvenlik anahtarınızı kullanmadıysanız, oturum açmak veya çevrimdışı kilidini açmak için kullanamazsınız.
- Birden çok Azure AD hesabı içeren bir güvenlik anahtarıyla bir Windows 10 aygıtında oturum açma veya kilidini açma. Bu senaryo, güvenlik anahtarına eklenen son hesabı kullanır. WebAuthN, kullanıcıların kullanmak istedikleri hesabı seçmelerine olanak tanır.
- Windows 10 sürüm 1809 çalıştıran bir aygıtın kilidini açın. En iyi deneyimi yaşamak için Windows 10 sürüm 1903 veya daha yüksek kullanın.

## <a name="prepare-devices-for-preview"></a>Cihazları önizlemeiçin hazırlama

Azure AD, özellik önizlemesi sırasında pilot luk yaptığınız aygıtları birleştirerek Windows 10 sürüm 1809 veya daha yüksek bir sürümünü çalıştırmalıdır. En iyi deneyim Windows 10 sürüm 1903 veya daha yüksektir.

Karma Azure AD birleştirilmiş aygıtlar Windows 10 Insider Build 18945 veya daha yeni çalıştırmalıdır.

## <a name="enable-security-keys-for-windows-sign-in"></a>Windows oturum açma için güvenlik anahtarlarını etkinleştirme

Kuruluşlar, kuruluşlarının gereksinimlerine göre Windows oturum açma için güvenlik anahtarlarını etkinleştirmek için aşağıdaki yöntemlerden birini veya birkaçını kullanmayı seçebilir:

- [Intune ile etkinleştirme](#enable-with-intune)
- [Hedeflenen Intune dağıtımı](#targeted-intune-deployment)
- [Bir sağlama paketi yle etkinleştirme](#enable-with-a-provisioning-package)
- [Grup İlkesi ile etkinleştirme (Yalnızca Karma Azure AD birleştirilmiş aygıtlar)](#enable-with-group-policy)

> [!IMPORTANT]
> Karma **Azure AD'ye katılan aygıtlara** sahip **kuruluşların,** Windows 10 FIDO2 güvenlik anahtarı kimlik doğrulaması çalışmadan önce [FIDO2 kimlik doğrulamasını şirket içi kaynaklara etkinleştirme](howto-authentication-passwordless-security-key-on-premises.md) başlıklı makaledeki adımları da tamamlaması gerekir.
>
> Azure **AD'ye katılan aygıtlara** sahip kuruluşların, aygıtlarının FIDO2 güvenlik anahtarlarıyla şirket içi kaynaklara kimlik doğrulaması yapabilmesi için bunu yapması gerekir.

### <a name="enable-with-intune"></a>Intune ile etkinleştirme

Intune'u kullanarak güvenlik anahtarlarının kullanımını etkinleştirmek için aşağıdaki adımları tamamlayın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. **Microsoft Intune** > **Aygıt kaydı** > **Windows kayıt** > **Windows Hello İş** > **Özellikleri**için göz atın.
1. **Ayarlar**altında, Oturum **Açma için Güvenlik Anahtarlarını** **Etkin'e**ayarlayın.

Oturum açma için güvenlik anahtarlarının yapılandırması, İşletmeler için Windows Hello yapılandırmaya bağlı değildir.

### <a name="targeted-intune-deployment"></a>Hedeflenen Intune dağıtımı

Kimlik bilgisi sağlayıcısını etkinleştirmek için belirli aygıt gruplarını hedeflemek için Intune üzerinden aşağıdaki özel ayarları kullanın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. **Microsoft Intune** > **Device yapılandırma** > **Profilleri** > **profili oluşturun**göz atın.
1. Yeni profili aşağıdaki ayarlarla yapılandırın:
   - Ad: Windows Oturum Açma için Güvenlik Tuşları
   - Açıklama: FIDO Güvenlik Anahtarlarının Windows Oturum Açma sırasında kullanılmasını sağlar
   - Platform: Windows 10 ve sonrası
   - Profil türü: Özel
   - Özel OMA-URI Ayarları:
      - Ad: Windows Oturum Açma için FIDO Güvenlik Anahtarlarını açın
      - OMA-URI: ./Cihaz/Satıcı/MSFT/PassportForWork/SecurityKey/UseSecurityKeyForSignin
      - Veri Türü: Karşıcı
      - Değer: 1
1. Bu ilke belirli kullanıcılara, aygıtlara veya gruplara atanabilir. Daha fazla bilgi için [Microsoft Intune'da kullanıcı ve aygıt profillerini atay'a](https://docs.microsoft.com/intune/device-profile-assign)bakın.

![Intune özel aygıt yapılandırma ilkesi oluşturma](./media/howto-authentication-passwordless-security-key/intune-custom-profile.png)

### <a name="enable-with-a-provisioning-package"></a>Bir sağlama paketi yle etkinleştirme

Intune tarafından yönetilmeyen aygıtlar için işlevselliği etkinleştirmek için bir sağlama paketi yüklenebilir. Windows Configuration Designer uygulaması [Microsoft Mağazası'ndan](https://www.microsoft.com/p/windows-configuration-designer/9nblggh4tx22)yüklenebilir. Bir sağlama paketi oluşturmak için aşağıdaki adımları tamamlayın:

1. Windows Yapılandırma Tasarımcısı'nı başlatın.
1. **Dosya** > **Yeni projeyi**seçin.
1. Projenize bir ad verin ve projenizin oluşturulduğu yolu not alın, ardından **İleri'yi**seçin.
1. **Seçili proje iş akışı** olarak seçilen Sağlama *paketini* bırakın ve **İleri'yi**seçin.
1. **Hangi ayarların görüntüleneve yapılandırışla**değiştirilen tüm *Windows masaüstü sürümlerini* seçin, ardından **İleri'yi**seçin.
1. **Bitiş'i**seçin.
1. Yeni oluşturduğunuz projede, **Runtime ayarlarına** > göz atın**WindowsHelloForBusiness** > **SecurityKeys** > **UseSecurityKeyForSignIn**.
1. **UseSecurityKeyForSignIn'i** *Etkin*olarak ayarlayın.
1. **İhracat** > **Sağlama paketini** seçin
1. Varsayılanları Sağlama **Paketini Açıkla'nın**altındaki **Yapı** penceresinde bırakın, ardından **İleri'yi**seçin.
1. Varsayılanları **Oluşturma** **penceresinde, sağlama paketi için güvenlik ayrıntılarını seç'in** altında bırakın ve **İleri'yi**seçin.
1. **Sağlama paketini kaydedin** ve **İleri'yi**seç'in altındaki **Yapı** pencerelerinde yolu not alın veya değiştirin.
1. Sağlama paketi sayfasını **Oluştur'da Oluştur'u** seçin. **Build the provisioning package**
1. Oluşturulan iki dosyayı *(ppkg* ve *cat)* makinelere daha sonra uygulayabileceğiniz bir konuma kaydedin.
1. Oluşturduğunuz sağlama paketini uygulamak için [bkz.](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-apply-package)

> [!NOTE]
> Windows 10 Sürüm 1809 çalıştıran aygıtlar da paylaşılan PC modunu etkinleştirmelidir *(EnableSharedPCMode).* Bu işlevselliği etkinleştirme hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/windows/configuration/set-up-shared-or-guest-pc)

### <a name="enable-with-group-policy"></a>Grup İlkesi ile Etkinleştir

**Karma Azure AD'ye katılan aygıtlar**için kuruluşlar, FIDO güvenlik anahtarı oturum açma işlemlerini etkinleştirmek için aşağıdaki Grup İlkesi ayarını yapılandırabilir. Ayar **Bilgisayar Yapılandırma** > **İdari Şablonlar** > **Sistem** > **Oturum Açma** > **güvenlik anahtarı oturum açma**altında bulunabilir:

- Bu ilkeyi **Etkin** olarak ayarlamak, kullanıcıların güvenlik anahtarlarıyla oturum açmalarına olanak tanır.
- Bu ilkeyi **Devre Dışı Bırakılmış** veya **Yapılandırılmamış** olarak ayarlamak, kullanıcıların güvenlik anahtarlarıyla oturum açmalarını engeller.

Bu Grup İlkesi ayarı, Grup İlkesi şablonunun `credentialprovider.admx` güncelleştirilmiş bir sürümünü gerektirir. Bu yeni şablon, Windows Server'ın bir sonraki sürümünde ve Windows 10 20H1 ile kullanılabilir. Bu ayar, Windows'un bu yeni sürümlerinden birini çalıştıran bir aygıtla veya destek konusundaki kılavuzu izleyerek merkezi olarak yönetilebilir, [Windows'ta Grup İlkesi Yönetim Şablonları için Merkezi Mağaza nasıl oluşturulur ve yönetilir.](https://support.microsoft.com/help/3087759/how-to-create-and-manage-the-central-store-for-group-policy-administra)

## <a name="sign-in-with-fido2-security-key"></a>FIDO2 güvenlik anahtarıyla oturum açın

Aşağıdaki örnekte, Bala Sandhu adlı bir kullanıcı, önceki makaledeki adımları kullanarak FIDO2 güvenlik anahtarını zaten sağlamış, [şifresiz güvenlik anahtarı oturum aç.'](howto-authentication-passwordless-security-key.md#user-registration-and-management-of-fido2-security-keys)oturum aç. Karma Azure AD'ye katılan aygıtlar için, [şirket içi kaynaklarda parolasız güvenlik anahtarı oturum açma](howto-authentication-passwordless-security-key-on-premises.md)özelliğini de etkinleştirdiğinizden emin olun. Bala, Windows 10 kilit ekranından güvenlik anahtarı kimlik bilgisi sağlayıcısını seçebilir ve Windows'da oturum açabilmek için güvenlik anahtarını ekleyebilir.

![Windows 10 kilit ekranında güvenlik anahtarı oturum açma](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Güvenlik anahtarı biyometrik, PIN veya sıfırlama güvenlik anahtarını yönetme

* Windows 10 sürüm 1903 veya üstü
   * Kullanıcılar Windows **Ayarlarını** kendi aygıtlarında > Hesaplar**Güvenlik Anahtarı'nda** **açabilirler** > 
   * Kullanıcılar PIN'lerini değiştirebilir, biyometrilerini güncelleyebilir veya güvenlik anahtarlarını sıfırlayabilir

## <a name="troubleshooting-and-feedback"></a>Sorun giderme ve geri bildirim

Bu özelliği niönizlerken geri bildirim paylaşmak veya sorunlarla karşılaşmak istiyorsanız, aşağıdaki adımları kullanarak Windows Geri Bildirim Hub uygulaması üzerinden paylaşın:

1. **Geri Bildirim Hub'ı** başlatın ve oturum açmış olduğunuzdan emin olun.
1. Aşağıdaki kategoriler altında geri bildirim gönderin:
   - Kategori: Güvenlik ve Gizlilik
   - Alt kategori: FIDO
1. Günlükleri yakalamak **için, Sorunumu Yeniden Oluşturma** seçeneğini kullanın

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD ve karma Azure AD birleştirilmiş aygıtlar için şirket içi kaynaklara erişimi etkinleştirme](howto-authentication-passwordless-security-key-on-premises.md)

[Cihaz kaydı hakkında daha fazla bilgi edinin](../devices/overview.md)

[Azure Çok Faktörlü Kimlik Doğrulama hakkında daha fazla bilgi edinin](../authentication/howto-mfa-getstarted.md)
