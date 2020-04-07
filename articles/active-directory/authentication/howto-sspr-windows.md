---
title: Windows için self servis parola sıfırlama - Azure Active Directory
description: Windows giriş ekranında unuttum parolasını kullanarak self servis parola sıfırlama nasıl etkinleştirilir?
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: d4f08161daf1d9c1a4431d9e3fba3ca741d88b16
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80743353"
---
# <a name="how-to-enable-password-reset-from-the-windows-login-screen"></a>Nasıl yapılır: Windows oturum açma ekranından parola sıfırlamayı etkinleştirme

Windows 7, 8, 8.1 ve 10 çalıştıran makinelerde kullanıcıların Windows giriş ekranında parolalarını sıfırlamalarını sağlayabilirsiniz. Kullanıcılar artık [SSPR portalına](https://aka.ms/sspr)erişmek için web tarayıcısı olan bir cihaz bulmak zorunda değil.

![Örnek Windows 7 ve 10 giriş ekranları gösterilen SSPR bağlantısı ile](./media/howto-sspr-windows/windows-reset-password.png)

## <a name="general-limitations"></a>Genel sınırlamalar

- Parola sıfırlama şu anda Uzak Masaüstü'nden veya Hyper-V gelişmiş oturumlardan desteklenmez.
- Bazı üçüncü taraf kimlik bilgileri sağlayıcılarının bu özellikile ilgili sorunlara neden olduğu bilinmektedir.
- [EnableLUA kayıt defteri anahtarının](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpsb/958053ae-5397-4f96-977f-b7700ee461ec) değiştirilmesi yle UAC'nin devre dışı bırakılmasının sorunlara neden olduğu bilinmektedir.
- Bu özellik, 802.1x ağ kimlik doğrulaması dağıtılan ağlarda ve "Kullanıcı oturum açmadan hemen önce gerçekleştirin" seçeneğinde çalışmaz. 802.1x ağ kimlik doğrulaması dağıtılan ağlar için bu özelliği etkinleştirmek için makine kimlik doğrulaması kullanılması önerilir.
- Karma Azure AD birleştirilmiş makinelerin, yeni parolayı kullanmak ve önbelleğe alınmış kimlik bilgilerini güncelleştirmek için bir etki alanı denetleyicisine ağ bağlantı hattı olmalıdır.
- Bir görüntü kullanıyorsanız, sysprep'i çalıştırmadan önce CopyProfile adımını gerçekleştirmeden önce yerleşik Yönetici için web önbelleğinin temizlendiğini sağlayın. Bu adım hakkında daha fazla bilgi, [özel varsayılan kullanıcı profilini kullanırken performans kötü](https://support.microsoft.com/help/4056823/performance-issue-with-custom-default-user-profile)destek makalesinde bulunabilir.
- Aşağıdaki ayarların Windows 10 cihazlarında parola kullanma ve sıfırlama olanağını engellebildiği bilinmektedir
    - Ctrl+Alt+Del, v1809'dan önce Windows 10 sürümlerinde ilke tarafından gerekliyse, **parolayı sıfırla** işe yaramaz.
    - Kilit ekranı bildirimleri kapatılırsa, **parolayı sıfırla** çalışmaz.
    - HideFastUserSwitching etkin olarak ayarlanır veya 1
    - DontDisplayLastUserName etkin olarak ayarlanır veya 1
    - NoLockScreen etkin olarak ayarlanır veya 1
    - EnableLostMode cihazda ayarlandı
    - Explorer.exe özel bir kabuk ile değiştirilir
- Aşağıdaki belirli üç ayaranın birleşimi bu özelliğin çalışmamasını sağlayabilir.
    - Etkileşimli oturum açma: CTRL+ALT+DEL = Devre dışı bırakma yı gerektirmez
    - Devre Dışı AtılabilirLockScreenAppNotifications = 1 veya Etkin
    - Windows SKU Ev veya Profesyonel sürümü değildir

## <a name="windows-10-password-reset"></a>Windows 10 parola sıfırlama

### <a name="windows-10-prerequisites"></a>Windows 10 ön koşullar

- Bir yöneticinin Azure portalından Azure AD self servis parola sıfırlamayı etkinleştirmesi gerekir.
- **Kullanıcılar bu özelliği kullanmadan önce SSPR'ye kaydolmalıdır**
- Ağ proxy gereksinimleri
   - Windows 10 cihazları 
       - Port 443 `passwordreset.microsoftonline.com` için ve`ajax.aspnetcdn.com`
       - Windows 10 aygıtları yalnızca makine düzeyinde proxy yapılandırması destekler
- En az Windows 10 çalıştırın, sürüm Nisan 2018 Güncelleştirmesi (v1803) ve aygıtlar aşağıdakilerden biri olmalıdır:
    - Azure AD'ye katılanlar
    - Hibrit Azure AD'ye katılmış

### <a name="enable-for-windows-10-using-intune"></a>Intune'u kullanarak Windows 10 için etkinleştirme

Oturum açma ekranından parola sıfırlama yapılmasını sağlayan yapılandırma değişikliğinin Intune'dan dağıtılması en esnek yöntemdir. Intune, yapılandırma değişikliğini tanımladığınız belirli bir makine grubuna dağıtmanızı sağlar. Bu yöntem, cihazın Intune kaydını gerektirir.

#### <a name="create-a-device-configuration-policy-in-intune"></a>Intune'da cihaz yapılandırma ilkesi oluşturma

1. [Azure Portal](https://portal.azure.com)'da oturum açın ve **Intune**'a tıklayın.
1. **Aygıt yapılandırma** > **Profilleri** > **Oluşturma Profiline** giderek yeni bir cihaz yapılandırma profili oluşturun
   - Profil için anlamlı bir ad girin
   - İsteğe bağlı olarak, profil için anlamlı bir açıklama girin
   - Platform **Windows 10 ve üstü**
   - Profil türü **Özel**
1. **Ayarları** Yapılandır
   - Parolayı sıfırla bağlantısını etkinleştirmek için şu OMA-URI Ayarını **ekleyin**
      - Ayarın ne yaptığını açıklayan anlamlı bir ad girin
      - İsteğe bağlı olarak, ayar için anlamlı bir açıklama girin
      - **OMA-URI** olarak `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset` ayarlayın
      - **Veri türü** olarak **Tamsayı** ayarlayın
      - **Değer** olarak **1** ayarlayın
      - **Tamam'ı** tıklatın
   - **Tamam'ı** tıklatın
1. **Oluştur'u** tıklatın
1. Bu ilke belirli kullanıcılara, aygıtlara veya gruplara atanabilir. Daha fazla bilgi Microsoft [Intune kullanıcı ve aygıt profilleri atalabilir](https://docs.microsoft.com/intune/device-profile-assign)makalede bulunabilir.

### <a name="enable-for-windows-10-using-the-registry"></a>Kayıt Defteri'ni kullanarak Windows 10 için etkinleştirme

1. Yönetim kimlik bilgilerini kullanarak Windows PC'de oturum açma
1. Yönetici olarak **regedit** komutunu çalıştırın
1. Aşağıdaki kayıt defteri anahtarını ayarlayın
   - `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      - `"AllowPasswordReset"=dword:00000001`

#### <a name="troubleshooting-windows-10-password-reset"></a>Windows 10 parola sıfırlama sorunu giderme

Azure AD denetim günlüğü parola sıfırlamanın oluştuğu yerin IP adresi ve ClientType'ı hakkında bilgi içerir.

![Örnek Azure AD Denetimi günlüğünde Windows 7 parola sıfırlama](media/howto-sspr-windows/windows-7-sspr-azure-ad-audit-log.png)

Kullanıcılar parolalarını bir Windows 10 aygıtının giriş ekranından sıyrettiklerinde, `defaultuser1` adı verilen düşük ayrıcalıklı geçici bir hesap oluşturulur. Bu hesap, parola sıfırlama işlemini güvenli tutmak için kullanılır. Hesabın kendisi rasgele oluşturulmuş bir parolaya sahiptir, aygıt oturum açma için görünmüyor ve kullanıcı parolasını sıkar sonra otomatik olarak kaldırılır. Birden `defaultuser` çok profil bulunabilir, ancak güvenli bir şekilde yoksayılabilir.

## <a name="windows-7-8-and-81-password-reset"></a>Windows 7, 8 ve 8.1 parola sıfırlama

### <a name="windows-7-8-and-81-prerequisites"></a>Windows 7, 8 ve 8.1 ön koşullar

- Bir yöneticinin Azure portalından Azure AD self servis parola sıfırlamayı etkinleştirmesi gerekir.
- **Kullanıcılar bu özelliği kullanmadan önce SSPR'ye kaydolmalıdır**
- Ağ proxy gereksinimleri
   - Windows 7, 8 ve 8.1 aygıtları
       - Bağlantı noktası 443 için`passwordreset.microsoftonline.com`
- Yamalı Windows 7 veya Windows 8.1 İşletim Sistemi.
- [TLS 1.2, Aktarım Katmanı Güvenliği (TLS) kayıt defteri ayarlarında](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12)bulunan kılavuz kullanılarak etkinleştirildi.
- Makinenizde birden fazla 3.

> [!WARNING]
> TLS 1.2 etkin olmalı, sadece otomatik pazarlık için ayarlanmamalıdır

### <a name="install"></a>Yükleme

1. Etkinleştirmek istediğiniz Windows sürümü için uygun yükleyiciyi indirin.
   - Yazılım, Microsoft indirme merkezinde[https://aka.ms/sspraddin](https://aka.ms/sspraddin)
1. Yüklemek istediğiniz makinede oturum açın ve yükleyiciyi çalıştırın.
1. Yüklemeden sonra yeniden başlatma önerilir.
1. Yeniden başlatıldıktan sonra, giriş ekranında bir kullanıcı seçin ve "Şifremi unuttum?" parola sıfırlama iş akışını başlatmak için.
1. Parolanızı sıfırlamak için ekrandaki adımları izleyerek iş akışını tamamlayın.

![Örnek Windows 7 "Şifremi unuttum" seçeneğini tıklattı. SSPR akışı](media/howto-sspr-windows/windows-7-sspr.png)

#### <a name="silent-installation"></a>Sessiz yükleme

- Sessiz yüklemek için "msiexec /i SsprWindowsLogon.PROD.msi /qn" komutunu kullanın.
- Sessiz kaldırma için "msiexec /x SsprWindowsLogon.PROD.msi /qn" komutunu kullanın.

#### <a name="troubleshooting-windows-7-8-and-81-password-reset"></a>Windows 7, 8 ve 8.1 parola sıfırlama sorunu giderme

Etkinlikler hem makinede hem de Azure AD'de günlüğe kaydedilir. Azure AD Etkinlikleri, parola sıfırlamanın gerçekleştiği IP adresi ve ClientType hakkındaki bilgileri içerir.

![Örnek Azure AD Denetimi günlüğünde Windows 7 parola sıfırlama](media/howto-sspr-windows/windows-7-sspr-azure-ad-audit-log.png)

Ek günlüğe kaydetme gerekiyorsa, makinedeki bir kayıt defteri anahtarı, ayrıntılı günlüğe kaydetmeyi etkinleştirmek için değiştirilebilir. Yalnızca sorun giderme amacıyla ayrıntılı günlüğe kaydetmeyi etkinleştirin.

`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\Credential Providers\{86D2F0AC-2171-46CF-9998-4E33B3D7FD4F}`

- Ayrıntılı günlüğe kaydetmeyi etkinleştirmek için bir `REG_DWORD: "EnableLogging"`,
- Ayrıntılı günlüğe kaydetmeyi devre dışı `REG_DWORD: "EnableLogging"` kılabilir, 0 olarak değiştirin.

## <a name="what-do-users-see"></a>Kullanıcıların ne görecek

Windows aygıtlarınız için parola sıfırlama yı yapılandırdığınıza göre, kullanıcı için ne gibi değişiklikler var? Oturum açma ekranında parolalarını sıfırlayabileceklerini nasıl anlayacaklar?

![Örnek Windows 7 ve 10 giriş ekranları gösterilen SSPR bağlantısı ile](./media/howto-sspr-windows/windows-reset-password.png)

Kullanıcılar oturum açmaya çalıştıklarında, artık oturum açma ekranında self servis parola sıfırlama deneyimini açan bir **Sıfırlama parolası** veya **unuttum parola** bağlantısını görürler. Bu işlev kullanıcıların web tarayıcısına erişmek için başka bir cihaz kullanmalarına gerek kalmadan parolalarını sıfırlamalarına olanak tanır.

Kullanıcılarınız bu özelliği kullanma yönergelerini [İş veya okul parolanızı sıfırlama](../user-help/active-directory-passwords-update-your-own-password.md) konusunda bulabilirler

## <a name="next-steps"></a>Sonraki adımlar

[İzin vermek için kimlik doğrulama yöntemlerini planlayın](concept-authentication-methods.md)

[Windows 10'u yapılandırma](https://docs.microsoft.com/windows/configuration/)
