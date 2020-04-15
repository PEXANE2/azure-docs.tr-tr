---
title: Kullanıcı başına Çok Faktörlü Kimlik Doğrulamayı Etkinleştirme - Azure Etkin Dizini
description: Kullanıcı durumunu değiştirerek kullanıcı başına Azure Çok Faktörlü Kimlik Doğrulaması'nı nasıl etkinleştirmenizi öğrenin
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
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309776"
---
# <a name="enable-per-user-azure-multi-factor-authentication-to-secure-sign-in-events"></a>Oturum açma etkinliklerini güvence altına almak için kullanıcı başına Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirme

Azure AD'de çok faktörlü kimlik doğrulama sıdatarak kullanıcı oturum açma olaylarını güvence altına almanın iki yolu vardır. İlk ve tercih edilen seçenek, belirli koşullar altında çok faktörlü kimlik doğrulaması gerektiren bir Koşullu Erişim ilkesi ayarlamaktır. İkinci seçenek, Azure Çok Faktörlü Kimlik Doğrulaması için her kullanıcıyı etkinleştirmektir. Kullanıcılar tek tek etkinleştirildiğinde, her oturum açtıklarında (güvenilen IP adreslerinden oturum açtıklarında veya _hatırlanan aygıtlar_ özelliğinin açık olması gibi bazı istisnalar dışında) çok faktörlü kimlik doğrulaması gerçekleştirirler.

> [!NOTE]
> Koşullu Erişim ilkelerini kullanarak Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirmek önerilir yaklaşımdır. Lisanslarınız Koşullu Erişim içermediği sürece, kullanıcıların her oturum açmalarında MFA gerçekleştirmelerini gerektirmediği sürece kullanıcı durumlarını değiştirmeniz artık önerilmez.
>
> Koşullu Erişim'i kullanmaya başlamak için [Bkz. Öğretici: Azure Çok Faktörlü Kimlik Doğrulama ile kullanıcı oturum açma olaylarını güvenli hale](tutorial-enable-azure-mfa.md)getirin.

## <a name="azure-multi-factor-authentication-user-states"></a>Azure Çok Faktörlü Kimlik Doğrulama kullanıcı durumları

Azure Çok Faktörlü Kimlik Doğrulama'daki kullanıcı hesaplarının aşağıdaki üç farklı durumu vardır:

> [!IMPORTANT]
> Koşullu Erişim ilkesi yle Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirmek kullanıcının durumunu değiştirmez. Kullanıcılar devre dışı görünüyorsa paniğe kapılmayın. Koşullu Erişim durumu değiştirmez.
>
> **Koşullu Erişim ilkelerini kullanıyorsanız kullanıcıları etkinleştirmemememelidir veya zorlamamalısınız.**

| Durum | Açıklama | Tarayıcı dışı uygulamalar etkilendi | Etkilenen tarayıcı uygulamaları | Etkilenen modern kimlik doğrulama |
|:---:| --- |:---:|:--:|:--:|
| Devre dışı | Azure Çok Faktörlü Kimlik Doğrulama'ya kaydolmayan yeni bir kullanıcı için varsayılan durum. | Hayır | Hayır | Hayır |
| Etkin | Kullanıcı Azure Çok Faktörlü Kimlik Doğrulama'ya kaydolmuş, ancak kaydolmamıştır. Bir sonraki oturum açtıklarında kaydolmak için bir istem alırlar. | Hayır.  Kayıt işlemi tamamlanana kadar çalışmaya devam ederler. | Evet. Oturum sona erdikten sonra Azure Çok Faktörlü Kimlik Doğrulama kaydı gereklidir.| Evet. Erişim belirteci süresi dolduktan sonra, Azure Çok Faktörlü Kimlik Doğrulama kaydı gereklidir. |
| Uygulandı | Kullanıcı kaydoldu ve Azure Çok Faktörlü Kimlik Doğrulama için kayıt işlemini tamamladı. | Evet. Uygulamalar uygulama parolaları gerektirir. | Evet. Oturum açarken Azure Çok Faktörlü Kimlik Doğrulama gereklidir. | Evet. Oturum açarken Azure Çok Faktörlü Kimlik Doğrulama gereklidir. |

Bir kullanıcının durumu, bir yöneticinin bunları Azure Çok Faktörlü Kimlik Doğrulama'ya kaydedip kaydetmediğini ve kayıt işlemini tamamlayıp tamamlamadığını yansıtır.

Tüm kullanıcılar *Devre Dışı*başlatın. Kullanıcıları Azure Çok Faktörlü Kimlik Doğrulama'ya kaydettiğinizde, durumları *Etkin*olarak değişir. Etkinleştirilmiş kullanıcılar oturum açın ve kayıt işlemini tamamladığında, durumları *Zorunlu*olarak değişir.

> [!NOTE]
> MFA, telefon veya e-posta gibi kayıt ayrıntıları zaten olan bir kullanıcı nesnesi üzerinde yeniden etkinleştirilirse, yöneticilerin bu kullanıcının Azure portalı veya PowerShell üzerinden MFA'yı yeniden kaydettirmeleri gerekir. Kullanıcı yeniden kaydolmuyorsa, MFA durumu MFA yönetim ui'sinde *Etkin'den* *Zorunlu'ya* geçiş yapmaz.

## <a name="view-the-status-for-a-user"></a>Kullanıcının durumunu görüntüleme

Kullanıcı durumlarını görüntüleyebileceğiniz ve yönetebileceğiniz Azure portal sayfasına erişmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
1. Azure Active *Directory'yi*arayın ve seçin, ardından **Tüm** > **Kullanıcıları**Seçin.
1. **Çok Faktörlü Kimlik Doğrulama'yı**seçin. Bu menü seçeneğini görmek için sağa kaydırmanız gerekebilir. Azure portal penceresinin tamamını ve menü konumunu görmek için aşağıdaki örnek ekran görüntüsünü seçin:[![](media/howto-mfa-userstates/selectmfa-cropped.png "Azure AD'deki Kullanıcılar penceresinden Çok Faktörlü Kimlik Doğrulaması'nı seçin")](media/howto-mfa-userstates/selectmfa.png#lightbox)
1. Aşağıdaki örnekte gösterildiği gibi kullanıcı durumunu görüntüleyen yeni bir sayfa açılır.
   ![Azure Çok Faktörlü Kimlik Doğrulaması için örnek kullanıcı durumu bilgilerini gösteren ekran görüntüsü](./media/howto-mfa-userstates/userstate1.png)

## <a name="change-the-status-for-a-user"></a>Kullanıcının durumunu değiştirme

Bir kullanıcı için Azure Çok Faktörlü Kimlik Doğrulama durumunu değiştirmek için aşağıdaki adımları tamamlayın:

1. Azure Çok Faktörlü Kimlik Doğrulama **kullanıcıları** sayfasına ulaşmak için önceki adımları kullanın.
1. Azure Çok Faktörlü Kimlik Doğrulaması için etkinleştirmek istediğiniz kullanıcıyı bulun. En üstteki görünümü **kullanıcılara**değiştirmeniz gerekebilir.
   ![Kullanıcı sekmesinden durumu değiştirmek için kullanıcıyı seçin](./media/howto-mfa-userstates/enable1.png)
1. Durumu değiştirmek için kullanıcının(lar)ın adının yanındaki kutuyu işaretleyin.
1. Sağ tarafta, **hızlı adımlar**altında **Etkinleştir'i** veya **Devre Dışı Atla'yı**seçin. Aşağıdaki örnekte, kullanıcı *John Smith'in* adının yanında bir denetimi vardır ![ve kullanım için etkinleştirilir: Hızlı adımlar menüsünde Etkinleştir'i tıklatarak seçili kullanıcıyı etkinleştirin](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > *Etkin* leştirilmiş kullanıcılar Azure Çok Faktörlü Kimlik Doğrulaması'na kaydolduklarında otomatik olarak *Zorla'ya* geçer. Kullanıcı durumunu el ile *Zorla'ya değiştirmeyin.*

1. Açılan pencerede seçiminizi onaylayın.

Kullanıcıları etkinleştirdikten sonra, e-posta yoluyla bildirin. Kullanıcılara bir sonraki oturum açtıklarında kaydolmalarını istemek için bir istem görüntülendiğini söyleyin. Ayrıca, kuruluşunuz modern kimlik doğrulamasını desteklemeyen tarayıcı dışı uygulamalar kullanıyorsa, uygulama parolaları oluşturmaları gerekir. Daha fazla bilgi için, başlamalarına yardımcı olmak için [Azure Çok Faktörlü Kimlik Doğrulama son kullanım kılavuzuna](../user-help/multi-factor-authentication-end-user.md) bakın.

## <a name="change-state-using-powershell"></a>PowerShell kullanarak durumu değiştirme

[Azure AD PowerShell'i](/powershell/azure/overview)kullanarak kullanıcı durumunu `$st.State` değiştirmek için bir kullanıcı hesabının parametresini değiştirirsiniz. Bir kullanıcı hesabı için üç olası durum vardır:

* *Etkin*
* *Uygulandı*
* *Devre dışı*  

Kullanıcıları doğrudan *Zorlanan* duruma taşımayın. Bunu yaparsanız, kullanıcı Azure Çok Faktörlü Kimlik Doğrulama kaydından geçmediği ve bir uygulama [parolası](howto-mfa-mfasettings.md#app-passwords)elde etmediği için tarayıcı tabanlı olmayan uygulamalar çalışmayı durdurur.

Başlamak için [Install-Module'i](/powershell/module/powershellget/install-module) kullanarak *MSOnline* modüllerini aşağıdaki gibi yükleyin:

```PowerShell
Install-Module MSOnline
```

Sonra, [Connect-MsolService](/powershell/module/msonline/connect-msolservice)kullanarak bağlanın:

```PowerShell
Connect-MsolService
```

Aşağıdaki örnek PowerShell komut dosyası, mfa *bsimon@contoso.com*adlı tek bir kullanıcı için sağlar:

```PowerShell
$st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
$st.RelyingParty = "*"
$st.State = "Enabled"
$sta = @($st)

# Change the following UserPrincipalName to the user you wish to change state
Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta
```

PowerShell'i kullanmak, kullanıcıları toplu olarak etkinleştirmeniz gerektiğinde iyi bir seçenektir. Aşağıdaki komut dosyası, kullanıcıların bir listesi üzerinden döngüler ve kendi hesaplarında MFA sağlar. Kullanıcı hesaplarını aşağıdaki `$users` gibi ilk satırda ayarlanın:

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

MFA'yı devre dışı kalmak için, aşağıdaki örnek [Get-MsolUser](/powershell/module/msonline/get-msoluser)ile bir kullanıcıyı alır, ardından [Set-MsolUser](/powershell/module/msonline/set-msoluser)kullanarak tanımlanan kullanıcı için ayarlanmış Herhangi bir *StrongAuthenticationRequirements'ı* kaldırır:

```PowerShell
Get-MsolUser -UserPrincipalName bsimon@contoso.com | Set-MsolUser -StrongAuthenticationRequirements @()
```

[Ayrıca, Set-MsolUser'i](/powershell/module/msonline/set-msoluser) kullanan bir kullanıcı için MFA'yı aşağıdaki gibi doğrudan devre dışı bırakabilirsiniz:

```PowerShell
Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements @()
```

## <a name="convert-users-from-per-user-mfa-to-conditional-access-based-mfa"></a>Kullanıcıları kullanıcı başına MFA'dan Koşullu Erişim tabanlı MFA'ya dönüştürme

Aşağıdaki PowerShell, Koşullu Erişim tabanlı Azure Çok Faktörlü Kimlik Doğrulama'ya dönüştürme konusunda size yardımcı olabilir.

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
> Yakın zamanda davranış ve bu PowerShell komut değişti. Daha önce, komut dosyası MFA yöntemlerini kaydedin, MFA'yı devre dışı bıraktı ve yöntemleri geri yükledi. Devre dışı kalma için varsayılan davranış yöntemleri temizlemediği için artık bu artık gerekli değildir.
>
> MFA, telefon veya e-posta gibi kayıt ayrıntıları zaten olan bir kullanıcı nesnesi üzerinde yeniden etkinleştirilirse, yöneticilerin bu kullanıcının Azure portalı veya PowerShell üzerinden MFA'yı yeniden kaydettirmeleri gerekir. Kullanıcı yeniden kaydolmuyorsa, MFA durumu MFA yönetim ui'sinde *Etkin'den* *Zorunlu'ya* geçiş yapmaz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Çok Faktörlü Kimlik Doğrulama ayarlarını güvenilir IP'ler, özel sesli mesajlar ve dolandırıcılık uyarıları gibi yapılandırmak için [bkz.](howto-mfa-mfasettings.md) Azure Çok Faktörlü Kimlik Doğrulama için kullanıcı ayarlarını yönetmek için [bkz.](howto-mfa-userdevicesettings.md)

Bir kullanıcıdan neden MFA gerçekleştirmesi istendiğini veya istenmediğini anlamak için [Azure Çok Faktörlü Kimlik Doğrulama raporlarına](howto-mfa-reporting.md#azure-ad-sign-ins-report)bakın.
