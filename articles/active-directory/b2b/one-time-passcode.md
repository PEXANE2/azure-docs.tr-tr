---
title: B2B konuk kullanıcıları için tek seferlik parola kimlik doğrulaması - Azure AD
description: Microsoft hesabına gerek kalmadan B2B konuk kullanıcılarının kimliğini doğrulamak için E-posta parolasını tek seferlik parola kullanma.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6d897bb983eb06baa4f1573f1f875eea8bb8afc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263380"
---
# <a name="email-one-time-passcode-authentication-preview"></a>E-posta bir kerelik şifre kimlik doğrulaması (önizleme)

|     |
| --- |
| E-posta tek seferlik parola, Azure Active Directory'nin genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.|
|     |

Bu makalede, B2B konuk kullanıcıları için E-posta bir kerelik parola kimlik doğrulaması etkinleştirmek için nasıl açıklanmaktadır. E-posta tek seferlik parola özelliği, B2B konuk kullanıcılarının Azure AD, Microsoft hesabı (MSA) veya Google federasyonu gibi diğer yollarla kimlik doğrulaması olanınamadığında kimlik doğrulaması verir. Tek seferlik parola kimlik doğrulaması ile bir Microsoft hesabı oluşturmaya gerek yoktur. Konuk kullanıcı bir daveti kullandığınızda veya paylaşılan bir kaynağa eriştiğinde, e-posta adresine gönderilen geçici bir kod isteyebilir. Sonra oturum açmaya devam etmek için bu kodu girerler.

Bu özellik şu anda önizleme için kullanılabilir (bkz. aşağıdaki [önizlemeye katılma).](#opting-in-to-the-preview) Önizlemeden sonra, bu özellik varsayılan olarak tüm kiracılar için açık olur.

> [!NOTE]
> Tek seferlik parola kullanıcıları, kiracı bağlamını içeren bir bağlantıyı `https://myapps.microsoft.com/?tenantid=<tenant id>` kullanarak `https://portal.azure.com/<tenant id>`oturum açmalı (örneğin, veya `https://myapps.microsoft.com/<verified domain>.onmicrosoft.com`doğrulanmış bir etki alanı durumunda). Uygulamalara ve kaynaklara doğrudan bağlantılar, kiracı bağlamını içerdiği sürece de çalışır. Konuk kullanıcılar şu anda kiracı bağlamı olmayan uç noktaları kullanarak oturum açamıyor. Örneğin, , `https://myapps.microsoft.com` `https://portal.azure.com`, veya Takımlar ortak bitiş noktası kullanarak bir hataya neden olur. 

## <a name="user-experience-for-one-time-passcode-guest-users"></a>Tek seferlik parola konuk kullanıcıları için kullanıcı deneyimi
Tek seferlik parola kimlik doğrulaması ile konuk kullanıcı doğrudan bir bağlantıyı tıklatarak veya davet e-postasını kullanarak davetinizi kullanabilir. Her iki durumda da, tarayıcıdaki bir ileti, bir kodun konuk kullanıcının e-posta adresine gönderileceğini gösterir. Konuk kullanıcı **Kodu Gönder'i**seçer:
 
   ![Kod Gönder düğmesini gösteren ekran görüntüsü](media/one-time-passcode/otp-send-code.png)
 
Kullanıcının e-posta adresine bir parola gönderilir. Kullanıcı parolayı e-postadan alır ve tarayıcı penceresine girer:
 
   ![Kodu Gir sayfasını gösteren ekran görüntüsü](media/one-time-passcode/otp-enter-code.png)
 
Konuk kullanıcının kimliği artık doğrulanmış durumda ve paylaşılan kaynağı görebilir veya oturum açmaya devam edebilir. 

> [!NOTE]
> Tek seferlik geçiş kodları 30 dakika boyunca geçerlidir. 30 dakika sonra, belirli bir kerelik geçiş kodu artık geçerli değildir ve kullanıcı yeni bir parola talep etmelidir. Kullanıcı oturumları 24 saat sonra sona erer. Bu süre sonunda, konuk kullanıcı kaynağa erişirken yeni bir parola alır. Oturum sona ermesi, özellikle konuk kullanıcı şirketinden ayrıldığında veya artık erişime ihtiyaç dolmadığında ek güvenlik sağlar.

## <a name="when-does-a-guest-user-get-a-one-time-passcode"></a>Konuk kullanıcı ne zaman tek seferlik bir parola alır?

Konuk kullanıcı bir daveti kullandığında veya kendileriyle paylaşılan bir kaynağa bağlantı kullandığında, aşağıdakiler aşağıdakiler için tek seferlik bir parola alır:
- Azure REKLAM hesapları yok 
- Microsoft hesapları yok 
- Davet eden kiracı, Google federasyonunu @googlemail.com ve kullanıcılar için @gmail.com ayarlamadı 

Davet sırasında, davet ettiğiniz kullanıcının tek seferlik parola kimlik doğrulaması kullanacağına dair bir belirti yoktur. Ancak konuk kullanıcı giriş yaptığında, başka kimlik doğrulama yöntemi kullanılamazsa, tek seferlik parola kimlik doğrulaması geri dönüş yöntemi olacaktır. 

Azure portalında tek seferlik parolalarla kimlik doğrulayan konuk kullanıcıları, **Azure Active Directory** > **Organizational ilişkilerine** > giderek diğer**kuruluşlardan gelen kullanıcılar**olarak görüntüleyebilirsiniz.

![OTP Kaynak değerine sahip tek seferlik bir parola kullanıcısını gösteren ekran görüntüsü](media/one-time-passcode/otp-users.png)

> [!NOTE]
> Bir kullanıcı bir kerelik parolayı kullanır ve daha sonra bir MSA, Azure AD hesabı veya başka bir federe hesap aldığında, tek seferlik bir parola yla kimlik doğrulaması yapmaya devam eder. Kimlik doğrulama yöntemini güncelleştirmek istiyorsanız, konuk kullanıcı hesaplarını silebilir ve yeniden davet edebilirsiniz.

### <a name="example"></a>Örnek
Konuk alexdoe@gmail.com kullanıcı, Google federasyonu nun kurulmadı fabrikam'a davet edilir. Alex'in Microsoft hesabı yok. Kimlik doğrulama için tek seferlik bir parola alırlar.

## <a name="opting-in-to-the-preview"></a>Önizlemeye katılma 
Kabul eyleminin etkili olması birkaç dakika sürebilir. Bundan sonra, yalnızca yukarıdaki koşulları karşılayan yeni davet edilen kullanıcılar tek seferlik parola kimlik doğrulaması kullanır. Daha önce bir daveti kullanan konuk kullanıcılar aynı kimlik doğrulama yöntemini kullanmaya devam ederler.

### <a name="to-opt-in-using-the-azure-ad-portal"></a>Azure AD portalını kullanmayı tercih etmek için
1.  [Azure portalında](https://portal.azure.com/) Azure AD global yöneticisi olarak oturum açın.
2.  Gezinti bölmesinde Azure **Etkin Dizin'i**seçin.
3.  **Yönet**altında, **Kuruluş İlişkileri'ni**seçin.
4.  **Ayarlar'ı**seçin.
5.  **Misafirler için E-postaYı Etkinleştir tek seferlik şifre (Önizleme)** altında **Evet'i**seçin.
 
### <a name="to-opt-in-using-powershell"></a>PowerShell'i kullanmayı tercih etmek için

İlk olarak, Azure AD PowerShell for Graph modülünün (AzureADPreview) en son sürümünü yüklemeniz gerekir. Ardından B2B ilkelerinin zaten var olup olmadığını belirleyecek ve uygun komutları çalıştırırsınız.

#### <a name="prerequisite-install-the-latest-azureadpreview-module"></a>Ön koşul: En son AzureADPreview modüllerini yükleyin
İlk olarak, hangi modülleri yüklediğinizi denetleyin. Windows PowerShell’i yükseltilmiş yönetici olarak açın (Yönetici olarak çalıştırın) ve aşağıdaki komutu çalıştırın:
 
```powershell  
Get-Module -ListAvailable AzureAD*
```

AzureADPreview modülü, sonraki bir sürüm olduğunu belirten bir ileti olmadan görüntülenirse hazırsınız demektir. Aksi takdirde, çıktıya bağlı olarak aşağıdakilerden birini yapın:

- Bir sonuç döndürülmezse, AzureADPreview modülünü yüklemek için aşağıdaki komutu çalıştırın:
  
   ```powershell  
   Install-Module AzureADPreview
   ```
- Yalnızca sonuçlarda AzureAD modülü gösteriliyorsa, AzureADPreview modülünü yüklemek için aşağıdaki komutları çalıştırın: 

   ```powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ```
- Yalnızca sonuçlarda AzureADPreview modülü gösteriliyorsa, ancak sonraki bir sürümün olduğunu belirten bir ileti alırsanız modülü güncelleştirmek için aşağıdaki komutları çalıştırın: 

   ```powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
  ```

Modülü güvenilir olmayan bir depodan yüklediğinizi belirten bir istem alabilirsiniz. Önceden PSGallery deposunu güvenilir depo olarak ayarlamadıysanız bu oluşur. Modülü yüklemek için **Y** tuşuna basın.

#### <a name="check-for-existing-policies-and-opt-in"></a>Varolan ilkeleri kontrol edin ve

Ardından, aşağıdakileri çalıştırarak bir B2BManagementPolicy'nin şu anda var olup olmadığını denetleyin:

```powershell 
$currentpolicy =  Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy' -and $_.IsOrganizationDefault -eq $true} | select -First 1
$currentpolicy -ne $null
```
- Çıktı False ise, ilke şu anda yok. Yeni bir B2BManagementPolicy oluşturun ve aşağıdakileri çalıştırarak önizlemeyi tercih edin:

   ```powershell 
   $policyValue=@("{`"B2BManagementPolicy`":{`"PreviewPolicy`":{`"Features`":[`"OneTimePasscode`"]}}}")
   New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true
   ```

- Çıktı True ise, B2BManagementPolicy ilkesi şu anda var. İlkeyi güncelleştirmek ve önizlemeyi tercih etmek için aşağıdakileri çalıştırın:
  
   ```powershell 
   $policy = $currentpolicy.Definition | ConvertFrom-Json
   $features=[PSCustomObject]@{'Features'=@('OneTimePasscode')}; $policy.B2BManagementPolicy | Add-Member 'PreviewPolicy' $features -Force; $policy.B2BManagementPolicy
   $updatedPolicy = $policy | ConvertTo-Json -Depth 3
   Set-AzureADPolicy -Definition $updatedPolicy -Id $currentpolicy.Id
   ```

## <a name="opting-out-of-the-preview-after-opting-in"></a>Kabul ettikten sonra önizlemeyi devre dışı bırakma
Devre dışı bırakma eyleminin etkili olması birkaç dakika sürebilir. Önizlemeyi kapatırsanız, tek seferlik bir parola kullanan konuk kullanıcılar oturum açamaz. Konuk kullanıcıyı silebilir ve başka bir kimlik doğrulama yöntemi kullanarak yeniden oturum açmalarını sağlamak için kullanıcıyı yeniden davet edebilirsiniz.

### <a name="to-turn-off-the-preview-using-the-azure-ad-portal"></a>Azure AD portalını kullanarak önizlemeyi kapatmak için
1.  [Azure portalında](https://portal.azure.com/) Azure AD global yöneticisi olarak oturum açın.
2.  Gezinti bölmesinde Azure **Etkin Dizin'i**seçin.
3.  **Yönet**altında, **Kuruluş İlişkileri'ni**seçin.
4.  **Ayarlar'ı**seçin.
5.  **Misafirler için E-postaYı Etkinleştir Tek Seferlik Şifreyi Etkinleştir (Önizleme)** **altında, No'u**seçin.

### <a name="to-turn-off-the-preview-using-powershell"></a>PowerShell'i kullanarak önizlemeyi kapatmak için
En son AzureADPreview modüllerini yüklemeniz durumunda zaten (bkz. Ön koşul: Yukarıdaki [en son AzureADPreview modüllerini yükleyin).](#prerequisite-install-the-latest-azureadpreview-module) Ardından, tek seferlik parola önizleme ilkesinin şu anda aşağıdakileri çalıştırarak var olduğunu doğrulayın:

```powershell 
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy' -and $_.IsOrganizationDefault -eq $true} | select -First 1
($currentPolicy -ne $null) -and ($currentPolicy.Definition -like "*OneTimePasscode*")
```

Çıktı True ise, aşağıdakileri çalıştırarak önizlemeyi devre dışı bırakma:

```powershell 
$policy = $currentpolicy.Definition | ConvertFrom-Json
$policy.B2BManagementPolicy.PreviewPolicy.Features = $policy.B2BManagementPolicy.PreviewPolicy.Features.Where({$_ -ne "OneTimePasscode"})
$updatedPolicy = $policy | ConvertTo-Json -Depth 3
Set-AzureADPolicy -Definition $updatedPolicy -Id $currentpolicy.Id
```

