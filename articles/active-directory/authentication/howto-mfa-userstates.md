---
title: Kullanıcı başına Multi-Factor Authentication etkinleştir-Azure Active Directory
description: Kullanıcı durumunu değiştirerek Kullanıcı başına Azure Multi-Factor Authentication etkinleştirmeyi öğrenin
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 08/17/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 433cfa3789aa37f4145982da97719526c0abfc47
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719504"
---
# <a name="enable-per-user-azure-multi-factor-authentication-to-secure-sign-in-events"></a>Oturum açma olaylarının güvenliğini sağlamak için kullanıcı başına Azure Multi-Factor Authentication’ı etkinleştirme

Azure AD 'de Kullanıcı oturum açma olaylarını güvenli hale getirmek için çok faktörlü kimlik doğrulamasının (MFA) gerekli olmasını sağlayabilirsiniz. Koşullu erişim ilkelerini kullanarak Azure Multi-Factor Authentication etkinleştirme, kullanıcıları korumak için önerilen yaklaşımdır. Koşullu erişim bir Azure AD Premium P1 veya P2 özelliğidir. Bu özellik, belirli senaryolarda gereken MFA gerektirirken kurallar uygulamanıza olanak tanır. Koşullu erişimi kullanmaya başlamak için bkz. [öğretici: Azure Multi-Factor Authentication Ile güvenli Kullanıcı oturum açma olayları](tutorial-enable-azure-mfa.md).

Koşullu erişim olmadan Azure AD ücretsiz kiracılarında, [kullanıcıları korumak için güvenlik varsayılanlarını kullanabilirsiniz](../fundamentals/concept-fundamentals-security-defaults.md). Kullanıcılara gerektiğinde MFA sorulur, ancak davranışı denetlemek için kendi kurallarınızı tanımlayamazsınız.

Gerekirse, her bir hesabı kullanıcı başına Azure Multi-Factor Authentication için etkinleştirebilirsiniz. Kullanıcılar ayrı ayrı etkinleştirildiğinde, her oturum açtıklarında çok faktörlü kimlik doğrulaması gerçekleştirir (güvenilen IP adreslerinden oturum açtıklarında veya _güvenilir CIHAZLARDA MFA 'yı anımsa_ özelliği açık olduğunda olduğu gibi bazı özel durumlar ile).

Azure AD lisanslarınız koşullu erişim içermediği ve güvenlik varsayılanlarını kullanmak istemediğinizde, Kullanıcı durumlarının değiştirilmesi önerilmez. MFA 'yı etkinleştirmenin farklı yolları hakkında daha fazla bilgi için bkz. [Azure Multi-Factor Authentication Için Özellikler ve lisanslar](concept-mfa-licensing.md).

> [!IMPORTANT]
>
> Bu makalede, Kullanıcı başına Azure Multi-Factor Authentication durumu görüntüleme ve değiştirme ayrıntıları anlatılmaktadır. Koşullu erişim veya güvenlik Varsayılanları kullanırsanız, bu adımları kullanarak Kullanıcı hesaplarını gözden geçirmezsiniz veya etkinleştiremezsiniz.
>
> Koşullu erişim ilkesi aracılığıyla Azure Multi-Factor Authentication etkinleştirilmesi, kullanıcının durumunu değiştirmez. Kullanıcılar devre dışı görünüyorsa uyarıda yok. Koşullu erişim durumu değiştirmez.
>
> **Koşullu erişim ilkeleri kullanıyorsanız, Kullanıcı başına Azure Multi-Factor Authentication etkinleştirmeyin veya zorlamaz.**

## <a name="azure-multi-factor-authentication-user-states"></a>Azure Multi-Factor Authentication Kullanıcı durumları

Bir kullanıcının durumu, yönetici tarafından Kullanıcı başına Azure Multi-Factor Authentication kayıtlı olup olmadığını yansıtır. Azure Multi-Factor Authentication Kullanıcı hesapları aşağıdaki üç farklı duruma sahiptir:

| Durum | Açıklama | Eski kimlik doğrulaması etkilendi | Etkilenen tarayıcı uygulamaları | Modern kimlik doğrulaması etkilendi |
|:---:| --- |:---:|:--:|:--:|
| Devre dışı | Kullanıcı başına Azure Multi-Factor Authentication kayıtlı olmayan bir kullanıcının varsayılan durumu. | Hayır | Hayır | Hayır |
| Etkin | Kullanıcı, Kullanıcı başına Azure Multi-Factor Authentication kaydedilir, ancak eski kimlik doğrulaması için parolalarını kullanmaya devam edebilir. Kullanıcı, MFA kimlik doğrulama yöntemlerini henüz kaydetmediyse, modern kimlik doğrulaması (örneğin, bir Web tarayıcısı aracılığıyla) kullanarak bir sonraki oturum açtıklarında kaydolmak üzere bir istem alırlar. | Hayır. Eski kimlik doğrulaması, kayıt işlemi tamamlanana kadar çalışmaya devam eder. | Evet. Oturumun süresi dolduktan sonra Azure Multi-Factor Authentication kaydı gereklidir.| Evet. Erişim belirtecinin süresi dolduktan sonra Azure Multi-Factor Authentication kaydı gereklidir. |
| Uygulandı | Kullanıcı Kullanıcı başına Azure Multi-Factor Authentication kaydedilir. Kullanıcı, kimlik doğrulama yöntemlerini henüz kaydetmediyse, modern kimlik doğrulaması (örneğin, bir Web tarayıcısı aracılığıyla) kullanarak bir sonraki oturum açtıklarında kaydolmak üzere bir istem alırlar. *Etkin* durumda kaydı gerçekleştiren kullanıcılar otomatik olarak *Zorlanmış* duruma taşınır. | Evet. Uygulamalar uygulama parolaları gerektirir. | Evet. Azure Multi-Factor Authentication oturum açma sırasında gereklidir. | Evet. Azure Multi-Factor Authentication oturum açma sırasında gereklidir. |

Tüm kullanıcılar *devre dışı bırakıldı*. Kullanıcıları Kullanıcı başına Azure Multi-Factor Authentication kaydettiğinizde, durumları *etkin*olarak değişir. Etkin kullanıcılar oturum açıp kayıt işlemini tamamladıktan sonra, durumu *Zorlanmış*olarak değişir. Yöneticiler, *Zorlanmış* durumdan *etkin* veya *devre dışı*olarak da dahil olmak üzere kullanıcıları durumlar arasında taşıyabiliriz.

> [!NOTE]
> Kullanıcı başına MFA bir kullanıcı için yeniden etkinleştirilmişse ve Kullanıcı yeniden kaydolmazsa, MFA durumu, MFA yönetimi Kullanıcı arabirimindeki *etkin* *' e geçiş* yapmaz. Yöneticinin kullanıcıyı doğrudan *Zorlanmış*olarak taşıması gerekir.

## <a name="view-the-status-for-a-user"></a>Kullanıcının durumunu görüntüleme

Kullanıcı durumlarını görüntülemek ve yönetmek için aşağıdaki adımları tamamlayarak Azure portal sayfasına erişin:

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
1. *Azure Active Directory*arayıp seçin, sonra **Kullanıcılar**  >  **tüm kullanıcılar**' ı seçin.
1. **Multi-Factor Authentication**'ı seçin. Bu menü seçeneğini görmek için sağa kaydırmanız gerekebilir. Tam Azure portal pencere ve menü konumunu görmek için aşağıdan örnek ekran görüntüsünü seçin: [ ![ Azure AD 'de kullanıcılar penceresinden Multi-Factor Authentication seçin.](media/howto-mfa-userstates/selectmfa-cropped.png)](media/howto-mfa-userstates/selectmfa.png#lightbox)
1. Aşağıdaki örnekte gösterildiği gibi, Kullanıcı durumunu görüntüleyen yeni bir sayfa açılır.
   ![Azure Multi-Factor Authentication için örnek Kullanıcı durumu bilgilerini gösteren ekran görüntüsü](./media/howto-mfa-userstates/userstate1.png)

## <a name="change-the-status-for-a-user"></a>Kullanıcının durumunu değiştirme

Bir kullanıcı için Kullanıcı başına Azure Multi-Factor Authentication durumunu değiştirmek için aşağıdaki adımları izleyin:

1. Bir kullanıcının Azure Multi-Factor Authentication **kullanıcıları** sayfasına ulaşmak üzere [durumunu görüntülemek](#view-the-status-for-a-user) için önceki adımları kullanın.
1. Kullanıcı başına Azure Multi-Factor Authentication için etkinleştirmek istediğiniz kullanıcıyı bulun. En üstteki görünümü **kullanıcılara**değiştirmeniz gerekebilir.
   ![Durumu Kullanıcı sekmesinden değiştirmek için kullanıcıyı seçin](./media/howto-mfa-userstates/enable1.png)
1. Durumu değiştirmek için Kullanıcı (ler) in yanındaki kutuyu işaretleyin.
1. Sağ tarafta **hızlı adımlar**altında **Etkinleştir** veya **devre dışı bırak**' ı seçin. Aşağıdaki örnekte *John Smith* kullanıcısı adının yanında bir denetim sahibi olur ve kullanım için Etkinleştiriliyor: ![ hızlı adımlar menüsünde Etkinleştir ' e tıklayarak Seçili kullanıcıyı etkinleştir](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > *Etkin* kullanıcılar, Azure Multi-Factor Authentication kaydolduklarında otomatik olarak *zorlandı* . Kullanıcı zaten kayıtlı olmadığı veya kullanıcının eski kimlik doğrulama protokollerine yönelik bağlantılarda kesintiye *uğratılmasını* kabul etmediği takdirde Kullanıcı durumunu zorunlu olarak değiştirmeyin.

1. Açılan pencere penceresinde seçiminizi onaylayın.

Kullanıcıları etkinleştirdikten sonra, e-posta ile bildirim alın. Kullanıcılara bir dahaki sefer oturum açtıklarında kaydolmalarını istemek için bir istem görüntülendiğini söyleyin. Ayrıca, kuruluşunuz modern kimlik doğrulamayı desteklemeyen tarayıcı olmayan uygulamalar kullanıyorsa, uygulama parolaları oluşturmaları gerekir. Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication Son Kullanıcı Kılavuzu](../user-help/multi-factor-authentication-end-user-first-time.md) , kullanmaya başlamanıza yardımcı olur.

## <a name="change-state-using-powershell"></a>PowerShell kullanarak durumu değiştirme

Kullanıcı durumunu [Azure AD PowerShell](/powershell/azure/)kullanarak değiştirmek için, `$st.State` bir kullanıcı hesabının parametresini değiştirirsiniz. Bir kullanıcı hesabının üç olası durumu vardır:

* *Etkin*
* *Uygulandı*
* *Devre dışı*  

Genel olarak, kullanıcıları zaten MFA için kayıtlı olmadıkları takdirde doğrudan *Zorlanmış* duruma taşımayın. Bunu yaparsanız, Kullanıcı Azure Multi-Factor Authentication kaydı ve bir [uygulama parolası](howto-mfa-app-passwords.md)edinmediği için eski kimlik doğrulama uygulamaları çalışmayı durdurur. Bazı durumlarda bu davranış istenebilir, ancak Kullanıcı kaydoluncaya kadar Kullanıcı deneyimini etkiler.

Başlamak için, [Install-Module](/powershell/module/powershellget/install-module) kullanarak *MSOnline* modülünü aşağıdaki gibi çalıştırın:

```PowerShell
Install-Module MSOnline
```

Sonra [Connect-MsolService](/powershell/module/msonline/connect-msolservice)kullanarak bağlanın:

```PowerShell
Connect-MsolService
```

Aşağıdaki örnek PowerShell betiği, adlı tek bir kullanıcı için MFA 'yı sunar *bsimon@contoso.com* :

```PowerShell
$st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
$st.RelyingParty = "*"
$st.State = "Enabled"
$sta = @($st)

# Change the following UserPrincipalName to the user you wish to change state
Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta
```

Kullanıcıları toplu olarak etkinleştirmeniz gerektiğinde PowerShell kullanmak iyi bir seçenektir. Aşağıdaki betik, bir kullanıcı listesi ile döngülerinde MFA 'yı sunar. Aşağıdaki gibi ilk satırda ayarlanan Kullanıcı hesaplarını tanımlayın `$users` :

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

## <a name="convert-users-from-per-user-mfa-to-conditional-access"></a>Kullanıcıları Kullanıcı başına MFA 'dan koşullu erişime dönüştürme

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
> MFA bir kullanıcı üzerinde yeniden etkinleştirilmişse ve Kullanıcı yeniden kaydolmazsa, MFA durumu, MFA yönetimi Kullanıcı arabiriminde *etkin* durumuna geçmez. *Enforced* Bu durumda, yöneticinin kullanıcıyı doğrudan *Zorlanmış*olarak taşıması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Multi-Factor Authentication ayarlarını yapılandırmak için bkz.  [azure Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md).

Azure Multi-Factor Authentication Kullanıcı ayarlarını yönetmek için bkz. [azure Multi-Factor Authentication ile Kullanıcı ayarlarını yönetme](howto-mfa-userdevicesettings.md).

Bir kullanıcının neden MFA yapması istenmediğini anlamak için, bkz. [Azure Multi-Factor Authentication raporları](howto-mfa-reporting.md).