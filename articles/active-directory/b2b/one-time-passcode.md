---
title: B2B Konuk kullanıcıları için bir kerelik geçiş kodu kimlik doğrulaması-Azure AD
description: Bir Microsoft hesabı ihtiyaç duymadan B2B Konuk kullanıcılarının kimliğini doğrulamak için bir kerelik e-posta geçiş kodu kullanma.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: ac743a82405524efc16e16be015b61b9390bd05d
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83199479"
---
# <a name="email-one-time-passcode-authentication-preview"></a>E-posta bir kerelik geçiş kodu kimlik doğrulaması (Önizleme)

|     |
| --- |
| E-posta bir kerelik geçiş kodu, Azure Active Directory genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Bu makalede, B2B Konuk kullanıcıları için bir kerelik geçiş kodu kimlik doğrulamasının nasıl etkinleştirileceği açıklanır. E-posta bir kerelik geçiş kodu özelliği, Azure AD, Microsoft hesabı (MSA) veya Google Federasyonu gibi diğer yollarla kimlik doğrulamasından geçiyorlarsa B2B Konuk kullanıcılarının kimliğini doğrular. Tek seferlik geçiş kodu kimlik doğrulamasıyla Microsoft hesabı oluşturmanız gerekmez. Konuk Kullanıcı bir davetiyeyi bir davet edebilir veya paylaşılan bir kaynağa eriştiğinde, kendi e-posta adreslerine gönderilen geçici bir kod isteyebilir. Sonra oturum açmaya devam etmek için bu kodu girer.

Bu özellik şu anda önizleme için kullanılabilir (bkz. aşağıdaki [önizlemede bulunan](#opting-in-to-the-preview) ). Önizleme sonrasında, bu özellik tüm kiracılar için varsayılan olarak açılır.

> [!NOTE]
> Bir kerelik geçiş kodu kullanıcılarının kiracı bağlamını içeren bir bağlantı kullanarak oturum açması gerekir (örneğin, `https://myapps.microsoft.com/?tenantid=<tenant id>` veya `https://portal.azure.com/<tenant id>` doğrulanmış bir etki alanı söz konusu olduğunda `https://myapps.microsoft.com/<verified domain>.onmicrosoft.com` ). Uygulama ve kaynakların doğrudan bağlantıları, kiracı bağlamını dahil ettikleri sürece da çalışır. Konuk kullanıcılar şu anda kiracı bağlamı olmayan uç noktaları kullanarak oturum açamıyor. Örneğin,, `https://myapps.microsoft.com` `https://portal.azure.com` , veya ekipleri ortak uç nokta kullanımı bir hataya neden olur. 

## <a name="user-experience-for-one-time-passcode-guest-users"></a>Bir kerelik geçiş kodu Konuk kullanıcıları için Kullanıcı deneyimi
Bir kerelik geçiş kodu kimlik doğrulamasıyla, Konuk Kullanıcı doğrudan bağlantıya tıklayarak veya davet e-postasını kullanarak davetinizi kullanabilir. Her iki durumda da tarayıcıda bir ileti, Konuk kullanıcının e-posta adresine bir kod gönderileceğini belirtir. Konuk Kullanıcı, **kodu gönder**' i seçer:
 
   ![Kod Gönder düğmesini gösteren ekran görüntüsü](media/one-time-passcode/otp-send-code.png)
 
Kullanıcının e-posta adresine bir geçiş kodu gönderilir. Kullanıcı geçiş kodunu e-postayla alır ve tarayıcı penceresine girer:
 
   ![Kod gir sayfasını gösteren ekran görüntüsü](media/one-time-passcode/otp-enter-code.png)
 
Artık Konuk kullanıcının kimliği doğrulanır ve paylaşılan kaynağı görebilir veya oturum açmaya devam edebilir. 

> [!NOTE]
> Bir kerelik geçiş kodları 30 dakika için geçerlidir. 30 dakika sonra, belirli bir kerelik geçiş kodu artık geçerli değildir ve kullanıcının yeni bir tane istemesi gerekir. Kullanıcı oturumlarının süresi 24 saat sonra dolacak. Bu süreden sonra, Konuk Kullanıcı kaynağa erişirken yeni bir geçiş kodu alır. Oturum süre sonu, özellikle bir ziyaretçi Kullanıcı şirketten ayrıldığında veya artık erişime ihtiyaç duymuyorsa ek güvenlik sağlar.

## <a name="when-does-a-guest-user-get-a-one-time-passcode"></a>Konuk Kullanıcı bir kerelik geçiş kodu ne zaman alır?

Konuk Kullanıcı bir daveti bir kez kullanır veya bununla paylaşılan bir kaynağın bağlantısını kullandığında, şu durumlarda bir kerelik geçiş kodu alırlar:
- Azure AD hesabı yoktur 
- Microsoft hesabı yoktur 
- Davet eden kiracı, Google Federation @gmail.com 'i ve @googlemail.com kullanıcılarını ayarladı 

Davet sırasında, davet ettiğiniz kullanıcının bir kerelik geçiş kodu kimlik doğrulamasını kullanacağı belirtilecektir. Ancak Konuk Kullanıcı oturum açtığında, başka bir kimlik doğrulama yöntemi kullanılmıyorsa, tek seferlik geçiş kodu kimlik doğrulaması geri dönüş yöntemi olur. 

**Azure Active Directory**kullanıcılara giderek Azure Portal, tek seferlik Geçiş kodlarıyla kimlik doğrulayan Konuk kullanıcıları görüntüleyebilirsiniz  >  **Users**.

![Kaynak değeri OTP olan bir kerelik geçiş kodu kullanıcısını gösteren ekran görüntüsü](media/one-time-passcode/otp-users.png)

> [!NOTE]
> Bir Kullanıcı bir kerelik geçiş kodunu ve daha sonra bir MSA, Azure AD hesabını ya da başka bir Federasyon hesabını aldığında, bir kerelik geçiş kodu kullanarak kimlik doğrulamasından devam eder. Kimlik doğrulama yöntemini güncelleştirmek istiyorsanız, Konuk Kullanıcı hesabını silip yeniden davet edebilirsiniz.

### <a name="example"></a>Örnek
Konuk Kullanıcı alexdoe@gmail.com , Google Federasyonu ayarlanmamış olan Fabrikam 'a davet edilir. Alex Microsoft hesabı yok. Kimlik doğrulaması için bir kerelik geçiş kodu alırlar.

## <a name="opting-in-to-the-preview"></a>Önizlemede düzenleme 
Kabul etme eyleminin etkili olması birkaç dakika sürebilir. Bundan sonra, yalnızca yukarıdaki koşullara uyan yeni davet edilen kullanıcılar, tek seferlik geçiş kodu kimlik doğrulamasını kullanır. Daha önce bir davetiyeyi kullanan Konuk kullanıcılar aynı kimlik doğrulama yöntemini kullanmaya devam eder.

### <a name="to-opt-in-using-the-azure-ad-portal"></a>Azure AD portalını kullanmayı kabul etmek için
1.  [Azure Portal](https://portal.azure.com/) Azure AD Genel Yöneticisi olarak oturum açın.
2.  Gezinti bölmesinde **Azure Active Directory**' yi seçin.
3.  **Kurumsal ilişki**  >  **ayarları** ' nı seçin (veya **dış kimlikler**  >  **dış işbirliği ayarları**' nı seçin).
5.  **Konuklar Için tek seferlik geçiş kodunu etkinleştir (Önizleme)** altında **Evet**' i seçin.
 
### <a name="to-opt-in-using-powershell"></a>PowerShell 'i kullanmayı kabul etmek için

İlk olarak, Graph modülünün (AzureADPreview) Azure AD PowerShell 'in en son sürümünü yüklemeniz gerekir. Daha sonra, B2B ilkelerinin zaten mevcut olup olmadığını ve uygun komutları çalıştırıp çalıştırmayacağını belirlersiniz.

#### <a name="prerequisite-install-the-latest-azureadpreview-module"></a>Önkoşul: en son AzureADPreview modülünü yükler
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

#### <a name="check-for-existing-policies-and-opt-in"></a>Mevcut ilkeleri denetleme ve kabul etme

Sonra, B2BManagementPolicy Şu anda mevcut olup olmadığını kontrol ederek aşağıdakileri çalıştırın:

```powershell 
$currentpolicy =  Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy' -and $_.IsOrganizationDefault -eq $true} | select -First 1
$currentpolicy -ne $null
```
- Çıkış yanlışsa, ilke şu anda mevcut değildir. Yeni bir B2BManagementPolicy oluşturun ve aşağıdakileri çalıştırarak önizlemeyi kabul edin:

   ```powershell 
   $policyValue=@("{`"B2BManagementPolicy`":{`"PreviewPolicy`":{`"Features`":[`"OneTimePasscode`"]}}}")
   New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true
   ```

- Çıkış doğruysa B2BManagementPolicy ilkesi Şu anda mevcuttur. İlkeyi güncelleştirmek ve önizlemeye kabul etmek için aşağıdakileri çalıştırın:
  
   ```powershell 
   $policy = $currentpolicy.Definition | ConvertFrom-Json
   $features=[PSCustomObject]@{'Features'=@('OneTimePasscode')}; $policy.B2BManagementPolicy | Add-Member 'PreviewPolicy' $features -Force; $policy.B2BManagementPolicy
   $updatedPolicy = $policy | ConvertTo-Json -Depth 3
   Set-AzureADPolicy -Definition $updatedPolicy -Id $currentpolicy.Id
   ```

## <a name="opting-out-of-the-preview-after-opting-in"></a>Geri alındıktan sonra önizlemeyi kapatma
Geri çevirme eyleminin etkili olması birkaç dakika sürebilir. Önizlemeyi kapatırsanız, bir kerelik geçiş kodu kullanan tüm konuk kullanıcılar oturum açamaz. Konuk kullanıcıyı silebilir ve kullanıcıyı, başka bir kimlik doğrulama yöntemi kullanarak tekrar oturum açmasını sağlamak üzere yeniden davet edebilirsiniz.

### <a name="to-turn-off-the-preview-using-the-azure-ad-portal"></a>Azure AD portalını kullanarak önizlemeyi devre dışı bırakmak için
1.  [Azure Portal](https://portal.azure.com/) Azure AD Genel Yöneticisi olarak oturum açın.
2.  Gezinti bölmesinde **Azure Active Directory**' yi seçin.
3.  **Kurumsal ilişki**  >  **ayarları** ' nı seçin (veya **dış kimlikler**  >  **dış işbirliği ayarları**' nı seçin).
5.  **Konuklar Için tek seferlik geçiş kodunu etkinleştir (Önizleme)** altında **Hayır**' ı seçin.

### <a name="to-turn-off-the-preview-using-powershell"></a>PowerShell 'i kullanarak önizlemeyi devre dışı bırakmak için
Zaten yoksa en son AzureADPreview modülünü yükleyebilirsiniz (bkz. [Önkoşul: Yukarıdaki en son AzureADPreview modülünü Install](#prerequisite-install-the-latest-azureadpreview-module) ). Ardından, aşağıdakileri çalıştırarak tek seferlik geçiş kodu önizleme ilkesinin mevcut olduğunu doğrulayın:

```powershell 
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy' -and $_.IsOrganizationDefault -eq $true} | select -First 1
($currentPolicy -ne $null) -and ($currentPolicy.Definition -like "*OneTimePasscode*")
```

Çıkış doğruysa, aşağıdakileri çalıştırarak önizlemeyi geri çevirin:

```powershell 
$policy = $currentpolicy.Definition | ConvertFrom-Json
$policy.B2BManagementPolicy.PreviewPolicy.Features = $policy.B2BManagementPolicy.PreviewPolicy.Features.Where({$_ -ne "OneTimePasscode"})
$updatedPolicy = $policy | ConvertTo-Json -Depth 3
Set-AzureADPolicy -Definition $updatedPolicy -Id $currentpolicy.Id
```

