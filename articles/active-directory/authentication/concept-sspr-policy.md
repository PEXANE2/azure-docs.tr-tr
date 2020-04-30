---
title: Self servis parola sıfırlama ilkeleri-Azure Active Directory
description: Farklı Azure Active Directory self servis parola sıfırlama ilkesi seçenekleri hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: e8b6d08dd2073de80ac0f7fd08f510d9cda80545
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82143230"
---
# <a name="self-service-password-reset-policies-and-restrictions-in-azure-active-directory"></a>Azure Active Directory içindeki self servis parola sıfırlama ilkeleri ve kısıtlamaları

Bu makalede, Azure Active Directory (Azure AD) kiracınızdaki Kullanıcı hesaplarıyla ilişkili parola ilkeleri ve karmaşıklık gereksinimleri açıklanmaktadır.

## <a name="administrator-reset-policy-differences"></a>Yönetici sıfırlama ilkesi farklılıkları

**Microsoft, herhangi bir Azure yönetici rolü için güçlü bir varsayılan *iki ağ geçidi* parola sıfırlama ilkesi uygular**. Bu ilke, kullanıcılarınız için tanımladığınız sunucudan farklı olabilir ve bu ilke değiştirilemez. Parola sıfırlama işlevini her zaman, hiçbir Azure yönetici rolü atanmadan Kullanıcı olarak sınamalısınız.

İki ağ geçidi ilkesiyle, **Yöneticilerin güvenlik sorularını kullanma yeteneği yoktur**.

İki kapı ilkesi, *e-posta adresi*, *kimlik doğrulayıcı uygulaması*veya *telefon numarası*gibi iki kimlik doğrulama verisi gerektirir. İki kapı ilkesi aşağıdaki koşullarda geçerlidir:

* Aşağıdaki Azure Yönetici rollerinin tümü etkilenir:
  * Yardım Masası Yöneticisi
  * Hizmet desteği yöneticisi
  * Faturalama yöneticisi
  * Partner Katman1 desteği
  * Partner Katman2 desteği
  * Exchange yöneticisi
  * Skype Kurumsal yöneticisi
  * Kullanıcı Yöneticisi
  * Dizin yazarları
  * Genel yönetici veya Şirket Yöneticisi
  * SharePoint yöneticisi
  * Uyumluluk Yöneticisi
  * Uygulama Yöneticisi
  * Güvenlik yöneticisi
  * Ayrıcalıklı rol yöneticisi
  * Intune yöneticisi
  * Uygulama Proxy Hizmeti Yöneticisi
  * Dynamics 365 Yöneticisi
  * Power BI hizmet yöneticisi
  * Kimlik doğrulama Yöneticisi
  * Ayrıcalıklı kimlik doğrulama Yöneticisi

* Deneme aboneliğinde 30 gün geçtikten sonra veya
* Azure AD kiracınız için *contoso.com*gibi özel bir etki alanı yapılandırılmıştır. veya
* Azure AD Connect, şirket içi dizininizdeki kimlikler eşitleniyor

### <a name="exceptions"></a>Özel Durumlar

Tek bir geçit ilkesi, bir e-posta adresi veya telefon numarası gibi bir dizi kimlik doğrulama verisi gerektirir. Tek bir geçit ilkesi aşağıdaki koşullarda geçerlidir:

* Deneme aboneliğinin ilk 30 günü içinde olur; veya
* Azure AD kiracınız için özel bir etki alanı yapılandırılmamış, bu nedenle varsayılan **. onmicrosoft.com*kullanılıyor. Varsayılan **. onmicrosoft.com* etki alanı, üretim kullanımı için önerilmez; '
* Azure AD Connect kimlikleri eşitlemiyor

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>Tüm Kullanıcı hesaplarına uygulanan UserPrincipalName ilkeleri

Azure AD 'de oturum açması gereken her kullanıcı hesabının, hesabıyla ilişkilendirilmiş benzersiz bir Kullanıcı asıl adı (UPN) özniteliği değeri olmalıdır. Aşağıdaki tabloda, hem bulut hem de yalnızca bulut Kullanıcı hesapları ile eşitlenen şirket içi Active Directory Domain Services Kullanıcı hesapları için uygulanan ilkeler özetlenmektedir:

| Özellik | UserPrincipalName gereksinimleri |
| --- | --- |
| İzin verilen karakterler |<ul> <li>A – Z</li> <li>a-z</li><li>0 – 9</li> <li> ' \. - \_ ! \# ^ \~</li></ul> |
| Karakterlere izin verilmiyor |<ul> <li>Herhangi bir\@ \" "Kullanıcı adını etki alanından ayıran karakter.</li> <li>Nokta karakteri içeremez "." hemen önceki "\@ \" sembol</li></ul> |
| Uzunluk kısıtlamaları |<ul> <li>Toplam uzunluk 113 karakterden uzun olmamalıdır</li><li>"\@ \" Simgesinden önce en fazla 64 karakter olabilir</li><li>"\@ \" Simgesinden sonra en fazla 48 karakter olabilir</li></ul> |

## <a name="password-policies-that-only-apply-to-cloud-user-accounts"></a>Yalnızca bulut kullanıcısı hesaplarına uygulanan parola ilkeleri

Aşağıdaki tabloda, Azure AD 'de oluşturulup yönetilen Kullanıcı hesaplarına uygulanan parola ilkesi ayarları açıklanmaktadır:

| Özellik | Gereksinimler |
| --- | --- |
| İzin verilen karakterler |<ul><li>A – Z</li><li>a-z</li><li>0 – 9</li> <li>@ # $% ^ & *-_! + = [] {} &#124; \: ',. ? / \`~ " ( ) ;</li> <li>boş alan</li></ul> |
| Karakterlere izin verilmiyor | Unicode karakterler. |
| Parola kısıtlamaları |<ul><li>En az 8 karakter ve en fazla 256 karakter.</li><li>Aşağıdakilerden üç tanesi olmalıdır:<ul><li>Küçük harfli karakterler.</li><li>Büyük harfli karakterler.</li><li>Sayılar (0-9).</li><li>Semboller (önceki parola kısıtlamalarına bakın).</li></ul></li></ul> |
| Parola süre sonu süresi (en fazla parola yaşı) |<ul><li>Varsayılan değer: **90** gün.</li><li>Değer, Windows PowerShell için Azure Active Directory modülünden `Set-MsolPasswordPolicy` cmdlet kullanılarak yapılandırılabilir.</li></ul> |
| Parola süre sonu bildirimi (kullanıcılara parola süre sonu bildirildiğinde) |<ul><li>Varsayılan değer: **14** gün (parolanın süresi dolmadan önce).</li><li>Değer, `Set-MsolPasswordPolicy` cmdlet 'i kullanılarak yapılandırılabilir.</li></ul> |
| Parola süre sonu (parolaların süresi dolmasın) |<ul><li>Varsayılan değer: **false** (parolanın bir sona erme tarihi olduğunu gösterir).</li><li>Değer, `Set-MsolUser` cmdlet 'i kullanılarak bireysel kullanıcı hesapları için yapılandırılabilir.</li></ul> |
| Parola değiştirme geçmişi | Kullanıcı bir parolayı değiştirdiğinde *son parola yeniden kullanılamaz.* |
| Parola sıfırlama geçmişi | Son parola, Kullanıcı unutulan bir parolayı sıfırladığında *yeniden kullanılabilir.* |
| Hesap kilitleme | 10 başarısız oturum açma denemesi yanlış parola ile çalıştıktan sonra, Kullanıcı bir dakika boyunca kilitlenir. Daha fazla hatalı oturum açma denemesi, Kullanıcı süresini artırmak için kullanıcıyı kilitler. [Akıllı kilitleme](howto-password-smart-lockout.md) , aynı parolanın kilitleme sayacını arttırmaktan kaçınmak için son üç hatalı parola karmalarını izler. Birisi aynı hatalı parolayı birden çok kez girerse, bu davranış hesabın kilitlenmesine neden olmaz. |

## <a name="set-password-expiration-policies-in-azure-ad"></a>Azure AD 'de parola süre sonu ilkelerini ayarlama

Bir Microsoft bulut hizmeti için *genel yönetici* veya *Kullanıcı Yöneticisi* , kullanıcı parolalarının kullanım süresini sona ermeyecek şekilde ayarlamak için *Windows PowerShell için Microsoft Azure ad modülü* kullanabilir. Windows PowerShell cmdlet 'lerini, hiçbir zaman sona erme yapılandırmasını kaldırmak veya hangi kullanıcı parolalarının süresi dolmayacak şekilde ayarlandığını görmek için de kullanabilirsiniz.

Bu kılavuz, Intune ve Office 365 gibi diğer sağlayıcılar için geçerlidir; bu da kimlik ve Dizin Hizmetleri için Azure AD 'ye bağımlıdır. Parola süre sonu, ilkenin değiştirilebilecek tek bölümüdür.

> [!NOTE]
> Yalnızca dizin eşitleme üzerinden eşitlenmemiş Kullanıcı hesapları için parolalar sona ermeyecek şekilde yapılandırılabilir. Dizin eşitleme hakkında daha fazla bilgi için bkz. [Azure AD Ile ad bağlama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="set-or-check-the-password-policies-by-using-powershell"></a>PowerShell kullanarak parola ilkelerini ayarlama ve denetleme

Başlamak için [Azure AD PowerShell modülünü indirip yükleyin](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0) ve [Azure AD kiracınıza bağlayın](https://docs.microsoft.com/powershell/module/azuread/connect-azuread?view=azureadps-2.0#examples). Modül yüklendikten sonra, her alanı yapılandırmak için aşağıdaki adımları kullanın.

### <a name="check-the-expiration-policy-for-a-password"></a>Parola için süre sonu ilkesini denetleme

1. Kullanıcı yöneticinize veya Şirket Yöneticisi kimlik bilgilerinizi kullanarak Windows PowerShell 'e bağlanın.
1. Aşağıdaki komutlardan birini çalıştırın:

   * Tek bir kullanıcının parolasının süresiz olarak ayarlanmış olup olmadığını görmek için, UPN 'yi (örneğin, *aprilr\@contoso.onmicrosoft.com*) veya denetlemek istediğiniz kullanıcının kullanıcı kimliğini kullanarak aşağıdaki cmdlet 'i çalıştırın:

   ```powershell
   Get-AzureADUser -ObjectId <user ID> | Select-Object @{N="PasswordNeverExpires";E={$_.PasswordPolicies -contains "DisablePasswordExpiration"}}
   ```

   * Tüm kullanıcılar için **parola süresi dolmasın** ayarını görmek için aşağıdaki cmdlet 'i çalıştırın:

   ```powershell
   Get-AzureADUser -All $true | Select-Object UserPrincipalName, @{N="PasswordNeverExpires";E={$_.PasswordPolicies -contains "DisablePasswordExpiration"}}
   ```

### <a name="set-a-password-to-expire"></a>Bir parolayı sona ermek üzere ayarla

1. Kullanıcı yöneticinize veya Şirket Yöneticisi kimlik bilgilerinizi kullanarak Windows PowerShell 'e bağlanın.
1. Aşağıdaki komutlardan birini yürütün:

   * Parolanın süresi dolmadan bir kullanıcının parolasını ayarlamak için, UPN veya kullanıcının kullanıcı KIMLIĞINI kullanarak aşağıdaki cmdlet 'i çalıştırın:

   ```powershell
   Set-AzureADUser -ObjectId <user ID> -PasswordPolicies None
   ```

   * Kuruluştaki tüm kullanıcıların parolalarını süreleri dolacak şekilde ayarlamak için aşağıdaki cmdlet 'i kullanın:

   ```powershell
   Get-AzureADUser -All $true | Set-AzureADUser -PasswordPolicies None
   ```

### <a name="set-a-password-to-never-expire"></a>Süresiz olarak bir parola ayarlayın

1. Kullanıcı yöneticinize veya Şirket Yöneticisi kimlik bilgilerinizi kullanarak Windows PowerShell 'e bağlanın.
1. Aşağıdaki komutlardan birini yürütün:

   * Bir kullanıcının parolasını hiçbir zaman dolmayacak şekilde ayarlamak için, UPN 'yi veya kullanıcının kullanıcı KIMLIĞINI kullanarak aşağıdaki cmdlet 'i çalıştırın:

   ```powershell
   Set-AzureADUser -ObjectId <user ID> -PasswordPolicies DisablePasswordExpiration
   ```

   * Bir kuruluştaki tüm kullanıcıların parolalarını hiçbir zaman sona ermeyecek şekilde ayarlamak için aşağıdaki cmdlet 'i çalıştırın:

   ```powershell
   Get-AzureADUser -All $true | Set-AzureADUser -PasswordPolicies DisablePasswordExpiration
   ```

   > [!WARNING]
   > Parolalar, `pwdLastSet` özniteliğe `-PasswordPolicies DisablePasswordExpiration` göre hala yaş olarak ayarlanmıştır. `pwdLastSet` Özniteliğini temel alarak, süre sonunu olarak `-PasswordPolicies None`değiştirirseniz, 90 günden `pwdLastSet` eski olan tüm parolalar kullanıcının bir sonraki oturum açışlarında değiştirmesini gerektirir. Bu değişiklik, çok sayıda kullanıcıyı etkileyebilir.

## <a name="next-steps"></a>Sonraki adımlar

SSPR 'yi kullanmaya başlamak için bkz. [öğretici: kullanıcıların hesaplarının kilidini açma veya Azure Active Directory self servis parola sıfırlama kullanarak parolaları sıfırlamalarını sağlama](tutorial-enable-sspr.md).

Eğer veya kullanıcılarınızın SSPR ile ilgili sorunları varsa bkz. [self servis parola sıfırlama sorunlarını giderme](active-directory-passwords-troubleshoot.md)
