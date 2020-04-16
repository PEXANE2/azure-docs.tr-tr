---
title: Self servis parola sıfırlama ilkeleri - Azure Etkin Dizini
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
ms.openlocfilehash: 5f5987bee82dc22d3742cb5d87040930e5d2c52d
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393045"
---
# <a name="self-service-password-reset-policies-and-restrictions-in-azure-active-directory"></a>Azure Active Directory'de self servis parola sıfırlama ilkeleri ve kısıtlamaları

Bu makalede, Azure Etkin Dizin (Azure AD) kiracınızda kullanıcı hesaplarıyla ilişkili parola ilkeleri ve karmaşıklık gereksinimleri açıklanmaktadır.

## <a name="administrator-reset-policy-differences"></a>Yönetici sıfırlama ilkesi farklılıkları

**Microsoft, herhangi bir Azure yönetici rolü için güçlü bir varsayılan *iki geçitli* parola sıfırlama ilkesi ni zorlar.** Bu ilke, kullanıcılarınız için tanımladığınızdan farklı olabilir ve bu ilke değiştirilemez. Herhangi bir Azure yönetici rolü atanmadan parola sıfırlama işlevini her zaman bir kullanıcı olarak test etmeniz gerekir.

İki geçitli bir ilke yle, **yöneticilerin güvenlik sorularını kullanma olanağı yoktur.**

İki kapılı ilke, *e-posta adresi,* *kimlik doğrulayıcı uygulaması*veya *telefon numarası*gibi iki adet kimlik doğrulama verisi gerektirir. İki kapılı bir ilke aşağıdaki durumlarda geçerlidir:

* Aşağıdaki tüm Azure yönetici rolleri etkilenir:
  * Yardım Masası yöneticisi
  * Hizmet desteği yöneticisi
  * Faturalama yöneticisi
  * Ortak Tier1 Desteği
  * Ortak Tier2 Desteği
  * Exchange yöneticisi
  * Skype Kurumsal yöneticisi
  * Kullanıcı yöneticisi
  * Dizin yazarları
  * Genel yönetici veya şirket yöneticisi
  * SharePoint yöneticisi
  * Uyumluluk yöneticisi
  * Uygulama yöneticisi
  * Güvenlik yöneticisi
  * Ayrıcalıklı rol yöneticisi
  * Intune yöneticisi
  * Uygulama proxy servis yöneticisi
  * Dynamics 365 yöneticisi
  * Power BI hizmet yöneticisi
  * Kimlik doğrulama yöneticisi
  * Ayrıcalıklı Kimlik Doğrulama yöneticisi

* Deneme aboneliğinde 30 gün geçmişse; Veya
* Azure AD kiracınız için *contoso.com*gibi özel bir etki alanı yapılandırıldı; Veya
* Azure AD Connect, şirket içi dizininizdeki kimlikleri eşitler

### <a name="exceptions"></a>Özel durumlar

Tek kapılı ilke, e-posta adresi veya telefon numarası gibi tek bir kimlik doğrulama verisi gerektirir. Tek kapılı bir ilke aşağıdaki durumlarda geçerlidir:

* Deneme aboneliğinin ilk 30 günü içinde; Veya
* Azure AD kiracınız için özel bir etki alanı yapılandırılmamış, bu nedenle varsayılan **.onmicrosoft.com*. Varsayılan **.onmicrosoft.com* etki alanı üretim kullanımı için önerilmez; Ve
* Azure AD Connect kimlikleri eşitlemiyor

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>Tüm kullanıcı hesapları için geçerli olan UserPrincipalName ilkeleri

Azure AD'de oturum açması gereken her kullanıcı hesabının, hesabıyla ilişkili benzersiz bir kullanıcı anaadı (UPN) özniteliği değeri olmalıdır. Aşağıdaki tablo, hem buluta eşitlenen şirket içi Active Directory Domain Services kullanıcı hesapları hem de yalnızca bulut kullanıcı hesapları için geçerli olan ilkeleri özetlemektedir:

| Özellik | UserPrincipalName gereksinimleri |
| --- | --- |
| İzin verilen karakterler |<ul> <li>A – Z</li> <li>a - z</li><li>0 – 9</li> <li> ' \. - \_ ! \# ^ \~</li></ul> |
| Karakterlere izin verilmiyor |<ul> <li>Kullanıcı\@ \" adını etki alanından ayırmayan herhangi bir " karakter.</li> <li>"\@ \" sembolünden hemen önce "." bir nokta karakteri içeremez</li></ul> |
| Uzunluk kısıtlamaları |<ul> <li>Toplam uzunluk 113 karakteri geçmemelidir</li><li>"\@ \" sembolünden önce en fazla 64 karakter olabilir</li><li>"\@ \" sembolünden sonra en fazla 48 karakter olabilir</li></ul> |

## <a name="password-policies-that-only-apply-to-cloud-user-accounts"></a>Yalnızca bulut kullanıcısı hesaplarına uygulanan parola ilkeleri

Aşağıdaki tabloda, Azure AD'de oluşturulan ve yönetilen kullanıcı hesaplarına uygulanan parola ilkesi ayarları açıklanmaktadır:

| Özellik | Gereksinimler |
| --- | --- |
| İzin verilen karakterler |<ul><li>A – Z</li><li>a - z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = ] { } &#124; \ : ' , . ? / \`~ " ( ) ;</li> <li>Boşluk</li></ul> |
| Karakterlere izin verilmiyor | Unicode karakterleri. |
| Parola kısıtlamaları |<ul><li>En az 8 karakter ve en fazla 256 karakter.</li><li>Aşağıdakidört kişiden üçünü gerektirir:<ul><li>Küçük karakterler.</li><li>Büyük harf karakterleri.</li><li>Sayılar (0-9).</li><li>Semboller (önceki parola kısıtlamalarına bakın).</li></ul></li></ul> |
| Parola son kullanma süresi (Maksimum parola yaşı) |<ul><li>Varsayılan değer: **90** gün.</li><li>Değer, Windows PowerShell `Set-MsolPasswordPolicy` için Azure Active Directory Modülü'ndeki cmdlet kullanılarak yapılandırılabilir.</li></ul> |
| Parola son kullanma bildirimi (Kullanıcılara parola süresi nin sona erdiği bildirilirse) |<ul><li>Varsayılan değer: **14** gün (parolanın süresi dolmadan önce).</li><li>Değeri `Set-MsolPasswordPolicy` cmdlet kullanılarak yapılandırılabilir.</li></ul> |
| Parola süresi nin dolması (Parolaların süresinin dolmasına izin verin) |<ul><li>Varsayılan değer: **false** (parolanın son kullanma tarihi olduğunu gösterir).</li><li>Değer `Set-MsolUser` cmdlet kullanılarak bireysel kullanıcı hesapları için yapılandırılabilir.</li></ul> |
| Parola değiştirme geçmişi | Kullanıcı parolayı değiştirdiğinde son *parola* tekrar kullanılamaz. |
| Parola sıfırlama geçmişi | Kullanıcı unutulan bir parolayı sıfırladığında son parola yeniden *kullanılabilir.* |
| Hesap kilitleme | Yanlış parolayla yapılan 10 başarısız oturum açma denemesinden sonra, kullanıcı bir dakika lığına kilitlenir. Daha fazla yanlış oturum açma girişimi, kullanıcıyı daha uzun süreler için kilitler. [Akıllı kilitleme,](howto-password-smart-lockout.md) aynı parola için kilitleme sayacının artışını önlemek için son üç kötü parola çözümünü izler. Birisi aynı kötü parolayı birden çok kez girerse, bu davranış hesabın kilitlenmesine neden olmaz. |

## <a name="set-password-expiration-policies-in-azure-ad"></a>Azure AD'de parola son kullanma ilkeleri ayarlama

Bir Microsoft bulut hizmeti için küresel bir *yönetici* veya *kullanıcı yöneticisi,* kullanıcı parolalarının süresinin dolmasına göre ayarlanması için Windows *PowerShell için Microsoft Azure REKLAM Modülü'ni* kullanabilir. Windows PowerShell cmdlets'i, süresi dolmayan yapılandırmayı kaldırmak veya hangi kullanıcı parolalarının süresinin asla dolup çıkamayacağını görmek için de kullanabilirsiniz.

Bu kılavuz, kimlik ve dizin hizmetleri için Azure AD'ye de güvenen Intune ve Office 365 gibi diğer sağlayıcılar için geçerlidir. Parola sona erdirme, ilkenin değiştirilebilen tek bölümüdür.

> [!NOTE]
> Yalnızca dizin eşitlemesi yoluyla eşitlenmemiş kullanıcı hesaplarının parolaları süresi dolacak şekilde yapılandırılabilir. Dizin eşitleme hakkında daha fazla bilgi için [Bkz.](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect)

## <a name="set-or-check-the-password-policies-by-using-powershell"></a>PowerShell kullanarak parola ilkelerini ayarlama ve denetleme

Başlamak için [Azure AD PowerShell modüllerini indirin ve yükleyin.](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0) Modül yüklendikten sonra, her alanı yapılandırmak için aşağıdaki adımları kullanın.

### <a name="check-the-expiration-policy-for-a-password"></a>Parola için son kullanma ilkesini denetleme

1. Kullanıcı yöneticinizi veya şirket yöneticisi kimlik bilgilerinizi kullanarak Windows PowerShell'e bağlanın.
1. Aşağıdaki komutlardan birini çalıştırın:

   * Tek bir kullanıcının parolasının süresinin dolup dolmadığını görmek için, upn (örneğin, *\@aprilr contoso.onmicrosoft.com)* veya kontrol etmek istediğiniz kullanıcının kullanıcı kimliğini kullanarak aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Get-AzureADUser -ObjectId <user ID> | Select-Object @{N="PasswordNeverExpires";E={$_.PasswordPolicies -contains "DisablePasswordExpiration"}}
   ```

   * Parola'nın tüm kullanıcılar için **hiçbir zaman süresinin dolduğunu** görmek için aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Get-AzureADUser -All $true | Select-Object UserPrincipalName, @{N="PasswordNeverExpires";E={$_.PasswordPolicies -contains "DisablePasswordExpiration"}}
   ```

### <a name="set-a-password-to-expire"></a>Süresi dolacak şekilde parola ayarlama

1. Kullanıcı yöneticinizi veya şirket yöneticisi kimlik bilgilerinizi kullanarak Windows PowerShell'e bağlanın.
1. Aşağıdaki komutlardan birini uygulayın:

   * Parolanın süresidile bir kullanıcının parolasını ayarlamak için, UPN veya kullanıcı kimliğini kullanarak aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Set-AzureADUser -ObjectId <user ID> -PasswordPolicies None
   ```

   * Kuruluştaki tüm kullanıcıların parolalarını, süresi dolacak şekilde ayarlamak için aşağıdaki cmdlet'i kullanın:

   ```powershell
   Get-AzureADUser -All $true | Set-AzureADUser -PasswordPolicies None
   ```

### <a name="set-a-password-to-never-expire"></a>Parolayı asla süresi dolmayacak şekilde ayarlama

1. Kullanıcı yöneticinizi veya şirket yöneticisi kimlik bilgilerinizi kullanarak Windows PowerShell'e bağlanın.
1. Aşağıdaki komutlardan birini uygulayın:

   * Bir kullanıcının parolasını asla sona ermeyecek şekilde ayarlamak için, UPN'yi veya kullanıcı kimliğini kullanarak aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Set-AzureADUser -ObjectId <user ID> -PasswordPolicies DisablePasswordExpiration
   ```

   * Bir kuruluştaki tüm kullanıcıların parolalarının asla süresinin dolmasına izin vermek için aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Get-AzureADUser -All $true | Set-AzureADUser -PasswordPolicies DisablePasswordExpiration
   ```

   > [!WARNING]
   > Parolalar özniteliğe göre `-PasswordPolicies DisablePasswordExpiration` `pwdLastSet` hala yaş olarak ayarlanır. Özniteliğe `pwdLastSet` bağlı olarak, son kullanma `-PasswordPolicies None`tarihini 90 günden `pwdLastSet` eski tüm parolalar olarak değiştirirseniz, kullanıcının bir sonraki oturum açamalarında bunları değiştirmesini gerektirir. Bu değişiklik çok sayıda kullanıcıyı etkileyebilir.

## <a name="next-steps"></a>Sonraki adımlar

SSPR'ye başlamak için [Bkz. Öğretici: Kullanıcıların Azure Active Directory self servis parola sıfırlamayı kullanarak hesaplarının kilidini açmalarını veya parolalarını sıfırlamalarını etkinleştirin.](tutorial-enable-sspr.md)

Siz veya kullanıcılar SSPR ile ilgili sorunlar yaşıyorsanız, [Sorun Giderme self servis parola sıfırlama](active-directory-passwords-troubleshoot.md)
