---
title: Kullanıcı başına Multi-Factor Authentication etkinleştir-Azure Active Directory
description: Kullanıcı durumunu değiştirerek Kullanıcı başına Azure Multi-Factor Authentication etkinleştirmeyi öğrenin
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 04/13/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e8ceaf13324864c7ec3df731c3e710815b0eba9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81309776"
---
# <a name="enable-per-user-azure-multi-factor-authentication-to-secure-sign-in-events"></a>Oturum açma olaylarını güvenli hale getirmek için Kullanıcı başına Azure Multi-Factor Authentication etkinleştirme

Azure AD 'de Multi-Factor Authentication 'ı zorunlu kılarak Kullanıcı oturum açma olaylarını güvenli hale getirmek için iki yol vardır. İlk ve tercih edilen seçeneği, belirli koşullarda çok faktörlü kimlik doğrulaması gerektiren bir koşullu erişim ilkesi ayarlamaya yönelik bir seçenektir. İkinci seçenek, her kullanıcıyı Azure Multi-Factor Authentication için etkinleştirmektir. Kullanıcılar ayrı ayrı etkinleştirildiğinde, her oturum açtıklarında çok faktörlü kimlik doğrulaması gerçekleştirir (güvenilir IP adreslerinden oturum açtıklarında veya _anımsanan cihazlar_ özelliği açık olduğunda olduğu gibi bazı özel durumlar ile).

> [!NOTE]
> Koşullu erişim ilkelerini kullanarak Azure Multi-Factor Authentication etkinleştirme önerilen yaklaşımdır. Lisanslarınız, kullanıcıların her oturum açtıklarında MFA gerçekleştirmesini gerektirdiği için koşullu erişim içermediği sürece Kullanıcı durumlarının değiştirilmesi artık önerilmez.
>
> Koşullu erişimi kullanmaya başlamak için bkz. [öğretici: Azure Multi-Factor Authentication Ile güvenli Kullanıcı oturum açma olayları](tutorial-enable-azure-mfa.md).

## <a name="azure-multi-factor-authentication-user-states"></a>Azure Multi-Factor Authentication Kullanıcı durumları

Azure Multi-Factor Authentication Kullanıcı hesapları aşağıdaki üç farklı duruma sahiptir:

> [!IMPORTANT]
> Koşullu erişim ilkesi aracılığıyla Azure Multi-Factor Authentication etkinleştirilmesi, kullanıcının durumunu değiştirmez. Kullanıcılar devre dışı görünüyorsa uyarıda yok. Koşullu erişim durumu değiştirmez.
>
> **Koşullu erişim ilkeleri kullanıyorsanız, kullanıcıları etkinleştirmezseniz veya zorlayamazsınız.**

| Durum | Açıklama | Etkilenen tarayıcı olmayan uygulamalar | Etkilenen tarayıcı uygulamaları | Modern kimlik doğrulaması etkilendi |
|:---:| --- |:---:|:--:|:--:|
| Devre dışı | Azure Multi-Factor Authentication 'de kayıtlı olan yeni bir kullanıcının varsayılan durumu. | Hayır | Hayır | Hayır |
| Etkin | Kullanıcı Azure Multi-Factor Authentication kaydettirildi, ancak kaydolmadı. Bir sonraki oturum açışlarında kaydolmak için bir istem alırlar. | Hayır.  Kayıt işlemi tamamlanana kadar çalışmaya devam eder. | Evet. Oturumun süresi dolduktan sonra Azure Multi-Factor Authentication kaydı gereklidir.| Evet. Erişim belirtecinin süresi dolduktan sonra Azure Multi-Factor Authentication kaydı gereklidir. |
| Uygulandı | Kullanıcı kaydedildi ve Azure Multi-Factor Authentication kayıt işlemini tamamladı. | Evet. Uygulamalar uygulama parolaları gerektirir. | Evet. Oturum açmada Azure Multi-Factor Authentication gereklidir. | Evet. Oturum açmada Azure Multi-Factor Authentication gereklidir. |

Bir kullanıcının durumu, yöneticinin Azure Multi-Factor Authentication 'e kaydolduğunu ve kayıt işleminin tamamlanıp tamamlanmadığını gösterir.

Tüm kullanıcılar *devre dışı bırakıldı*. Kullanıcıları Azure Multi-Factor Authentication kaydettiğinizde, durumları *etkin*olarak değişir. Etkin kullanıcılar oturum açıp kayıt işlemini tamamladıktan sonra, durumu *Zorlanmış*olarak değişir.

> [!NOTE]
> MFA, telefon veya e-posta gibi kayıt ayrıntılarına sahip olan bir kullanıcı nesnesi üzerinde yeniden etkinleştirilmişse, yöneticilerin bu kullanıcının MFA 'yı Azure portal veya PowerShell aracılığıyla yeniden kaydetmesi gerekir. Kullanıcı yeniden kaydolmazsa, MFA durumu, MFA yönetimi Kullanıcı arabiriminde *etkin* durumuna geçmez. *Enforced*

## <a name="view-the-status-for-a-user"></a>Kullanıcının durumunu görüntüleme

Kullanıcı durumlarını görüntüleyebileceğiniz ve yönetebileceğiniz Azure portal sayfasına erişmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
1. *Azure Active Directory*arayıp seçin, sonra **Kullanıcılar** > **tüm kullanıcılar**' ı seçin.
1. **Multi-Factor Authentication**seçin. Bu menü seçeneğini görmek için sağa kaydırmanız gerekebilir. Tam Azure portal pencere ve menü konumunu görmek için aşağıdan örnek ekran görüntüsünü seçin:[![](media/howto-mfa-userstates/selectmfa-cropped.png "Azure AD 'de kullanıcılar penceresinden Multi-Factor Authentication seçin")](media/howto-mfa-userstates/selectmfa.png#lightbox)
1. Aşağıdaki örnekte gösterildiği gibi, Kullanıcı durumunu görüntüleyen yeni bir sayfa açılır.
   ![Azure Multi-Factor Authentication için örnek Kullanıcı durumu bilgilerini gösteren ekran görüntüsü](./media/howto-mfa-userstates/userstate1.png)

## <a name="change-the-status-for-a-user"></a>Kullanıcının durumunu değiştirme

Bir kullanıcının Azure Multi-Factor Authentication durumunu değiştirmek için aşağıdaki adımları izleyin:

1. Azure Multi-Factor Authentication **kullanıcıları** sayfasına ulaşmak için yukarıdaki adımları kullanın.
1. Azure Multi-Factor Authentication için etkinleştirmek istediğiniz kullanıcıyı bulun. En üstteki görünümü **kullanıcılara**değiştirmeniz gerekebilir.
   ![Durumu Kullanıcı sekmesinden değiştirmek için kullanıcıyı seçin](./media/howto-mfa-userstates/enable1.png)
1. Durumu değiştirmek için Kullanıcı (ler) in yanındaki kutuyu işaretleyin.
1. Sağ tarafta **hızlı adımlar**altında **Etkinleştir** veya **devre dışı bırak**' ı seçin. Aşağıdaki örnekte *John Smith* kullanıcısı adının yanında bir denetim sahibi olur ve kullanım için Etkinleştiriliyor: ![hızlı adımlar menüsünde Etkinleştir ' e tıklayarak Seçili kullanıcıyı etkinleştir](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > *Etkin* kullanıcılar, Azure Multi-Factor Authentication kaydolduklarında otomatik olarak *zorlandı* . Kullanıcı durumunu *Zorlanmış*olarak el ile değiştirmeyin.

1. Açılan pencere penceresinde seçiminizi onaylayın.

Kullanıcıları etkinleştirdikten sonra, e-posta ile bildirim alın. Kullanıcılara bir dahaki sefer oturum açtıklarında kaydolmalarını istemek için bir istem görüntülendiğini söyleyin. Ayrıca, kuruluşunuz modern kimlik doğrulamayı desteklemeyen tarayıcı olmayan uygulamalar kullanıyorsa, uygulama parolaları oluşturmaları gerekir. Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication Son Kullanıcı Kılavuzu](../user-help/multi-factor-authentication-end-user.md) , kullanmaya başlamanıza yardımcı olur.

## <a name="change-state-using-powershell"></a>PowerShell kullanarak durumu değiştirme

Kullanıcı durumunu [Azure AD PowerShell](/powershell/azure/overview)kullanarak değiştirmek için, bir kullanıcı hesabının `$st.State` parametresini değiştirirsiniz. Bir kullanıcı hesabının üç olası durumu vardır:

* *Etkin*
* *Uygulandı*
* *Devre dışı*  

Kullanıcıları doğrudan *Zorlanmış* duruma taşımayın. Bunu yaparsanız, tarayıcı tabanlı olmayan uygulamalar çalışmayı durdurur çünkü Kullanıcı Azure Multi-Factor Authentication kaydı ve bir [uygulama parolası](howto-mfa-mfasettings.md#app-passwords)elde eder.

Başlamak için, [Install-Module](/powershell/module/powershellget/install-module) kullanarak *MSOnline* modülünü aşağıdaki gibi çalıştırın:

```PowerShell
Install-Module MSOnline
```

Sonra [Connect-MsolService](/powershell/module/msonline/connect-msolservice)kullanarak bağlanın:

```PowerShell
Connect-MsolService
```

Aşağıdaki örnek PowerShell betiği, adlı *bsimon@contoso.com*tek bir kullanıcı için MFA 'yı sunar:

```PowerShell
$st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
$st.RelyingParty = "*"
$st.State = "Enabled"
$sta = @($st)

# Change the following UserPrincipalName to the user you wish to change state
Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta
```

Kullanıcıları toplu olarak etkinleştirmeniz gerektiğinde PowerShell kullanmak iyi bir seçenektir. Aşağıdaki betik, bir kullanıcı listesi ile döngülerinde MFA 'yı sunar. Aşağıdaki `$users` gibi ilk satırda ayarlanan Kullanıcı hesaplarını tanımlayın:

   ```PowerShell
   # Define your list of users to update state in bulk
   $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"

   foreach ($user in $users)
   {
       $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
       $st.RelyingParty = "*"
       $st.State = "Enabled"
       $sta = @($st)
       Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
   }
   ```

MFA 'yı devre dışı bırakmak için aşağıdaki örnek, [Get-MsolUser](/powershell/module/msonline/get-msoluser)ile bir kullanıcı alır ve [set-MsolUser](/powershell/module/msonline/set-msoluser)kullanarak tanımlı kullanıcı Için ayarlanmış olan *strongauthenticationrequirements* öğesini kaldırır:

```PowerShell
Get-MsolUser -UserPrincipalName bsimon@contoso.com | Set-MsolUser -StrongAuthenticationRequirements @()
```

[Set-MsolUser](/powershell/module/msonline/set-msoluser) kullanarak bir kullanıcı için MFA 'yı da doğrudan devre dışı bırakabilirsiniz:

```PowerShell
Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements @()
```

## <a name="convert-users-from-per-user-mfa-to-conditional-access-based-mfa"></a>Kullanıcı başına MFA 'dan koşullu erişim tabanlı MFA 'ya Kullanıcı dönüştürme

Aşağıdaki PowerShell, koşullu erişim tabanlı Azure Multi-Factor Authentication dönüştürmeyi yaparken size yardımcı olabilir.

```PowerShell
# Sets the MFA requirement state
function Set-MfaState {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $ObjectId,
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $UserPrincipalName,
        [ValidateSet("Disabled","Enabled","Enforced")]
        $State
    )

    Process {
        Write-Verbose ("Setting MFA state for user '{0}' to '{1}'." -f $ObjectId, $State)
        $Requirements = @()
        if ($State -ne "Disabled") {
            $Requirement =
                [Microsoft.Online.Administration.StrongAuthenticationRequirement]::new()
            $Requirement.RelyingParty = "*"
            $Requirement.State = $State
            $Requirements += $Requirement
        }

        Set-MsolUser -ObjectId $ObjectId -UserPrincipalName $UserPrincipalName `
                     -StrongAuthenticationRequirements $Requirements
    }
}

# Disable MFA for all users
Get-MsolUser -All | Set-MfaState -State Disabled
```

> [!NOTE]
> Kısa süre önce davranışı ve bu PowerShell betiğini değiştirdik. Daha önce, komut dosyası MFA yöntemlerine kaydedilir, MFA 'yı devre dışı bırakmış ve yöntemleri geri yükledi. Artık devre dışı bırakma için varsayılan davranış metotları temizlemez.
>
> MFA, telefon veya e-posta gibi kayıt ayrıntılarına sahip olan bir kullanıcı nesnesi üzerinde yeniden etkinleştirilmişse, yöneticilerin bu kullanıcının MFA 'yı Azure portal veya PowerShell aracılığıyla yeniden kaydetmesi gerekir. Kullanıcı yeniden kaydolmazsa, MFA durumu, MFA yönetimi Kullanıcı arabiriminde *etkin* durumuna geçmez. *Enforced*

## <a name="next-steps"></a>Sonraki adımlar

Güvenilen IP 'Ler, özel ses iletileri ve sahtekarlık uyarıları gibi Azure Multi-Factor Authentication ayarlarını yapılandırmak için bkz. [azure Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md). Azure Multi-Factor Authentication Kullanıcı ayarlarını yönetmek için bkz. [azure Multi-Factor Authentication ile Kullanıcı ayarlarını yönetme](howto-mfa-userdevicesettings.md).

Bir kullanıcının neden MFA yapması istenmediğini anlamak için, bkz. [Azure Multi-Factor Authentication raporları](howto-mfa-reporting.md#azure-ad-sign-ins-report).
