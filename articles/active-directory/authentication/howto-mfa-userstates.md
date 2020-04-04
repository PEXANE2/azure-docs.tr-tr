---
title: Kullanıcı Başına Çok Faktörlü Kimlik Doğrulama - Azure Etkin Dizin
description: Azure Çok Faktörlü Kimlik Doğrulama'da kullanıcı durumlarını değiştirerek MFA'yı etkinleştirin.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: e955a3d10eea0a23154396a44fe6e1fc552a9ccb
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80653487"
---
# <a name="how-to-require-two-step-verification-for-a-user"></a>Bir kullanıcı için iki aşamalı doğrulama nasıl gerekir?

Her ikisi de genel bir yönetici hesabı kullanmayı gerektiren iki aşamalı doğrulama gerektiren iki yaklaşımdan birini alabilirsiniz. İlk seçenek, Azure Çok Faktörlü Kimlik Doğrulaması (MFA) için her kullanıcıyı etkinleştirmektir. Kullanıcılar tek tek etkinleştirildiğinde, her oturum açtıklarında (güvenilen IP adreslerinden oturum açtıklarında veya _hatırlanan aygıtlar_ özelliğinin açık olması gibi bazı istisnalar dışında) iki aşamalı doğrulama gerçekleştirirler. İkinci seçenek, belirli koşullar altında iki aşamalı doğrulama gerektiren bir Koşullu Erişim ilkesi ayarlamaktır.

> [!TIP]
> Koşullu Erişim ilkelerini kullanarak Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirmek önerilir yaklaşımdır. Lisanslarınız Koşullu Erişim içermediği sürece kullanıcı durumlarını değiştirmeniz artık önerilmez, çünkü kullanıcıların her oturum açmalarında MFA gerçekleştirmelerini gerektirecektir.

## <a name="choose-how-to-enable"></a>Nasıl etkinleştireceğinizi seçin

**Kullanıcı durumunu değiştirerek etkinleştirildi** - Bu, iki aşamalı doğrulama gerektiren geleneksel yöntemdir ve bu makalede tartışılır. Hem bulutta hem de Azure MFA Server'da Azure MFA ile çalışır. Bu yöntemi kullanmak, kullanıcıların her oturum **açtıklarında** iki aşamalı doğrulama gerçekleştirmelerini gerektirir ve Koşullu Erişim ilkelerini geçersiz kılar.

**Koşullu Erişim ilkesine göre etkinleştirilir** - Bu, kullanıcılarınız için iki aşamalı doğrulamayı etkinleştirmek için en esnek araçtır. Koşullu Erişim ilkesini etkinleştirmek yalnızca bulutta Azure MFA için çalışır ve Azure AD'nin üstün bir özelliğidir. Bu yöntem le ilgili daha fazla bilgiyi [Bulut Tabanlı Azure Çok Faktörlü Kimlik Doğrulamayı Dağıt'ta](howto-mfa-getstarted.md)bulabilirsiniz.

**Azure AD Kimlik Koruması tarafından etkinleştirildi** - Bu yöntem, yalnızca tüm bulut uygulamaları için oturum açma riskine dayalı olarak iki aşamalı doğrulama gerektiren Azure AD Kimlik Koruması risk ilkesini kullanır. Bu yöntem, Azure Active Directory P2 lisanslama gerektirir. Bu yöntem hakkında daha fazla bilgiyi [Azure Active Directory Identity Protection'da](../identity-protection/howto-sign-in-risk-policy.md) bulabilirsiniz

> [!Note]
> Lisanslar ve fiyatlandırma hakkında daha fazla bilgiyi [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) ve [Çok Faktörlü Kimlik Doğrulama](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) fiyatlandırma sayfalarında bulabilirsiniz.

## <a name="enable-azure-mfa-by-changing-user-state"></a>Kullanıcı durumunu değiştirerek Azure MFA'yı etkinleştirme

Azure Çok Faktörlü Kimlik Doğrulama'daki kullanıcı hesaplarının aşağıdaki üç farklı durumu vardır:

> [!IMPORTANT]
> Koşullu Erişim ilkesi yle Azure MFA'sını etkinleştirmek kullanıcının durumunu değiştirmez. Alarma neden olmayın kullanıcılar devre dışı görünüyor. Koşullu Erişim durumu değiştirmez. **Kuruluşlar, Koşullu Erişim ilkelerini kullanıyorsa kullanıcıları etkinleştirmemeli veya zorlamamalıdır.**

| Durum | Açıklama | Tarayıcı dışı uygulamalar etkilendi | Etkilenen tarayıcı uygulamaları | Etkilenen modern kimlik doğrulama |
|:---:| --- |:---:|:--:|:--:|
| Devre dışı | Azure MFA'ya kayıtlı olmayan yeni bir kullanıcı için varsayılan durum. | Hayır | Hayır | Hayır |
| Etkin | Kullanıcı Azure MFA'ya kaydoldu, ancak kaydolmadı. Bir sonraki oturum açtıklarında kaydolmak için bir istem alırlar. | Hayır.  Kayıt işlemi tamamlanana kadar çalışmaya devam ederler. | Evet. Oturum sona erdikten sonra Azure MFA kaydı gereklidir.| Evet. Erişim belirteci süresi dolduktan sonra Azure MFA kaydı gereklidir. |
| Uygulandı | Kullanıcı kaydoldu ve Azure MFA için kayıt işlemini tamamladı. | Evet. Uygulamalar uygulama parolaları gerektirir. | Evet. Oturum açarken Azure MFA gereklidir. | Evet. Oturum açarken Azure MFA gereklidir. |

Bir kullanıcının durumu, bir yöneticinin bunları Azure MFA'ya kaydedip kaydetmediğini ve kayıt işlemini tamamlayıp tamamlamadığını yansıtır.

Tüm kullanıcılar *Devre Dışı*başlatın. Kullanıcıları Azure MFA'ya kaydettiğinizde, durumları *Etkin*olarak değişir. Etkinleştirilmiş kullanıcılar oturum açın ve kayıt işlemini tamamladığında, durumları *Zorunlu*olarak değişir.

> [!NOTE]
> MFA, telefon veya e-posta gibi kayıt ayrıntıları zaten olan bir kullanıcı nesnesi üzerinde yeniden etkinleştirilirse, yöneticilerin bu kullanıcının Azure portalı veya PowerShell üzerinden MFA'yı yeniden kaydettirmeleri gerekir. Kullanıcı yeniden kaydolmuyorsa, MFA durumu MFA yönetim ui'sinde *Etkin'den* *Zorunlu'ya* geçiş yapmaz.

### <a name="view-the-status-for-a-user"></a>Kullanıcının durumunu görüntüleme

Kullanıcı durumlarını görüntüleyebileceğiniz ve yönetebileceğiniz sayfaya erişmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. *Azure Active Directory*'yi bulun ve seçin. **Kullanıcıları** > Seçin**Tüm kullanıcılar**.
3. **Çok Faktörlü Kimlik Doğrulama'yı**seçin. Bu menü seçeneğini görmek için sağa kaydırmanız gerekebilir. Azure portal penceresinin tamamını ve menü konumunu görmek için aşağıdaki örnek ekran görüntüsünü seçin:[![](media/howto-mfa-userstates/selectmfa-cropped.png "Azure AD'deki Kullanıcılar penceresinden Çok Faktörlü Kimlik Doğrulaması'nı seçin")](media/howto-mfa-userstates/selectmfa.png#lightbox)
4. Kullanıcı durumlarını görüntüleyen yeni bir sayfa açılır.
   ![çok faktörlü kimlik doğrulama kullanıcı durumu - ekran görüntüsü](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Kullanıcının durumunu değiştirme

1. Azure Çok Faktörlü Kimlik Doğrulama **kullanıcıları** sayfasına ulaşmak için önceki adımları kullanın.
2. Azure MFA için etkinleştirmek istediğiniz kullanıcıyı bulun. En üstteki görünümü değiştirmeniz gerekebilir.
   ![Kullanıcı sekmesinden durumu değiştirmek için kullanıcıyı seçin](./media/howto-mfa-userstates/enable1.png)
3. Adlarının yanındaki kutuyu işaretleyin.
4. Sağda, hızlı **adımlar**altında **Etkinleştir'i** veya **Devre Dışı At'ı**seçin.
   ![Hızlı adımlar menüsünde Etkinleştir'e tıklayarak seçili kullanıcıyı etkinleştirme](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > *Etkin* leştirilmiş kullanıcılar Azure MFA'ya kaydolduklarında otomatik olarak *Zorla'ya* geçer. Kullanıcı durumunu el ile *Zorla'ya değiştirmeyin.*

5. Açılan pencerede seçiminizi onaylayın.

Kullanıcıları etkinleştirdikten sonra, e-posta yoluyla bildirin. Bir dahaki oturum açsınlarda kayıt yaptırmaları isteneceklerini söyle. Ayrıca, kuruluşunuz modern kimlik doğrulamasını desteklemeyen tarayıcı dışı uygulamalar kullanıyorsa, uygulama parolaları oluşturmaları gerekir. Ayrıca, başlamalarına yardımcı olmak için [Azure MFA son kullanım kılavuzuna](../user-help/multi-factor-authentication-end-user.md) bir bağlantı da ekleyebilirsiniz.

### <a name="use-powershell"></a>PowerShell kullanma

[Azure AD PowerShell](/powershell/azure/overview)kullanarak kullanıcı durumunu `$st.State`değiştirmek için değiştirin. Üç olası durum vardır:

* Etkin
* Uygulandı
* Devre dışı  

Kullanıcıları doğrudan *Zorlanan* duruma taşımayın. Bunu yaparsanız, kullanıcı Azure MFA kaydından geçmediği ve bir [uygulama parolası](howto-mfa-mfasettings.md#app-passwords)elde etmediği için tarayıcı tabanlı olmayan uygulamalar çalışmayı durdurur.

Modülü ilk olarak şu kullanarak yükleyin:

   ```PowerShell
   Install-Module MSOnline
   ```

> [!TIP]
> **Önce Connect-MsolService** kullanarak bağlanmayı unutmayın

   ```PowerShell
   Connect-MsolService
   ```

Bu örnek PowerShell komut dosyası, tek bir kullanıcı için MFA sağlar:

   ```PowerShell
   Import-Module MSOnline
   Connect-MsolService
   $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
   $st.RelyingParty = "*"
   $st.State = "Enabled"
   $sta = @($st)
   Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta
   ```

PowerShell'i kullanmak, kullanıcıları toplu olarak etkinleştirmeniz gerektiğinde iyi bir seçenektir. Örnek olarak, aşağıdaki komut dosyası bir kullanıcı listesi aracılığıyla döngüler ve kendi hesaplarında MFA sağlar:

   ```PowerShell
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

MFA'yı devre dışı kalmak için şu komut dosyasını kullanın:

   ```PowerShell
   Get-MsolUser -UserPrincipalName user@domain.com | Set-MsolUser -StrongAuthenticationRequirements @()
   ```

hangi de kısaltılabilir:

   ```PowerShell
   Set-MsolUser -UserPrincipalName user@domain.com -StrongAuthenticationRequirements @()
   ```

### <a name="convert-users-from-per-user-mfa-to-conditional-access-based-mfa"></a>Kullanıcıları kullanıcı başına MFA'dan Koşullu Erişim tabanlı MFA'ya dönüştürme

Aşağıdaki PowerShell, Koşullu Erişim tabanlı Azure Çok Faktörlü Kimlik Doğrulama'ya dönüştürme konusunda size yardımcı olabilir.

Bu PowerShell'i Bir İmKB penceresinde çalıştırın veya bir . PS1 dosyası yerel olarak çalışacak.

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
> Biz son zamanlarda buna göre yukarıdaki davranış ve PowerShell komut değişti. Daha önce, komut dosyası MFA yöntemlerini kaydedin, MFA'yı devre dışı bıraktı ve yöntemleri geri yükledi. Devre dışı kalma için varsayılan davranış yöntemleri temizlemediği için artık bu artık gerekli değildir.
>
> MFA, telefon veya e-posta gibi kayıt ayrıntıları zaten olan bir kullanıcı nesnesi üzerinde yeniden etkinleştirilirse, yöneticilerin bu kullanıcının Azure portalı veya PowerShell üzerinden MFA'yı yeniden kaydettirmeleri gerekir. Kullanıcı yeniden kaydolmuyorsa, MFA durumu MFA yönetim ui'sinde *Etkin'den* *Zorunlu'ya* geçiş yapmaz.

## <a name="next-steps"></a>Sonraki adımlar

* Neden bir kullanıcıdan MFA gerçekleştirmesi istendi veya istenmedi? [Azure Çok Faktörlü Kimlik Doğrulama belgesindeki Raporlar'daki Azure AD oturum açma raporuna](howto-mfa-reporting.md#azure-ad-sign-ins-report)bakın.
* Güvenilir IP'ler, özel sesli mesajlar ve dolandırıcılık uyarıları gibi ek ayarları yapılandırmak için Azure [Çok Faktörlü Kimlik Doğrulama ayarlarını yapılandırma](howto-mfa-mfasettings.md) makalesine bakın
* Azure Çok Faktörlü Kimlik Doğrulama için kullanıcı ayarlarını yönetme hakkındaki bilgileri [bulutta Azure Çok Faktörlü Kimlik Doğrulaması ile kullanıcı ayarlarını yönet makalesinde](howto-mfa-userdevicesettings.md) bulabilirsiniz
