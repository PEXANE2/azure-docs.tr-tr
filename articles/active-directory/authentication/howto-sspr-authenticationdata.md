---
title: Self servis parola sıfırlama için iletişim bilgilerini önceden doldur-Azure Active Directory
description: Azure Active Directory self servis parola sıfırlama (SSPR) kullanıcılarına yönelik iletişim bilgilerini, bir kayıt işlemini tamamlamadan özelliği kullanabilmesi için nasıl önceden doldureceğinizi öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/17/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 22037468c47de45513351e1d6126d3f3e5fccadc
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88717804"
---
# <a name="pre-populate-user-authentication-contact-information-for-azure-active-directory-self-service-password-reset-sspr"></a>Azure Active Directory self servis parola sıfırlama için Kullanıcı kimlik doğrulaması iletişim bilgilerini önceden doldur (SSPR)

Azure Active Directory (Azure AD) self servis parola sıfırlama (SSPR) kullanmak için, bir kullanıcının kimlik doğrulama iletişim bilgileri mevcut olmalıdır. Bazı kuruluşların kullanıcıları kimlik doğrulama verilerini kendileri kaydeder. Diğer kuruluşlar Active Directory Domain Services (AD DS) zaten mevcut olan kimlik doğrulama verilerinden eşitlemeyi tercih eder. Bu eşitlenmiş veriler, Kullanıcı etkileşimi gerektirmeden Azure AD ve SSPR için kullanılabilir hale getirilir. Kullanıcıların parolalarını değiştirmesi veya sıfırlaması gerektiğinde, daha önce iletişim bilgilerini kaydetmemiş olsalar bile bunu yapabilir.

Aşağıdaki gereksinimleri karşıladıysanız kimlik doğrulama iletişim bilgilerini önceden doldurabilirsiniz:

* Şirket içi dizininizde verileri doğru biçimlendirmiş olursunuz.
* Azure AD kiracınız için [Azure AD Connect](../hybrid/how-to-connect-install-express.md) yapılandırdınız.

Telefon numaraları + *1 4251234567*gibi + *CountryCode PhoneNumber*biçiminde olmalıdır.

> [!NOTE]
> Ülke kodu ve telefon numarası arasında bir boşluk olması gerekir.
>
> Parola sıfırlama, telefon uzantılarını desteklemez. *+ 1 4251234567X12345* biçiminde bile, çağrı yerleştirilmadan önce uzantılar kaldırılır.

## <a name="fields-populated"></a>Doldurulan alanlar

Azure AD Connect varsayılan ayarları kullanıyorsanız, SSPR için kimlik doğrulama iletişim bilgilerini doldurmak üzere aşağıdaki eşlemeler yapılır:

| Şirket içi Active Directory | Azure AD     |
|------------------------------|--------------|
| telephoneNumber 'dır              | Ofis telefonu |
| mobil                       | Cep telefonu |

Kullanıcı cep telefonu numarasını doğruladıktan sonra, Azure AD 'de **kimlik doğrulaması iletişim bilgileri** altındaki *Telefon* alanı da bu sayıyla doldurulur.

## <a name="authentication-contact-info"></a>Kimlik doğrulaması iletişim bilgileri

Azure portal bir Azure AD kullanıcısının **kimlik doğrulama yöntemleri** sayfasında, bir genel yönetici, aşağıdaki örnekte gösterildiği gibi, kimlik doğrulama iletişim bilgilerini el ile ayarlayabilir:

![Azure AD 'de bir kullanıcının kimlik doğrulaması iletişim bilgileri][Contact]

Bu kimlik doğrulaması iletişim bilgileri için aşağıdaki noktalar geçerlidir:

* *Telefon* alanı doldurulmuşsa ve SSPR ilkesinde *cep telefonu* etkinleştirilmişse, Kullanıcı bu numarayı parola sıfırlama kaydı sayfasında ve parola sıfırlama iş akışı sırasında görür.
* *Alternatif telefon* alanı parola sıfırlama için kullanılmaz.
* *E* -posta alanı doldurulmuşsa ve SSPR Ilkesinde *e-posta* etkinse, Kullanıcı bu e-postayı parola sıfırlama kaydı sayfasında ve parola sıfırlama iş akışı sırasında görür.
* *Diğer e-posta* alanı doldurulmuşsa ve SSPR Ilkesinde *e-posta* etkinse, Kullanıcı bu e-postayı parola sıfırlama kaydı sayfasında görmez, ancak parola sıfırlama iş akışı sırasında görürler.

## <a name="security-questions-and-answers"></a>Güvenlik soruları ve yanıtları

Güvenlik soruları ve yanıtları, Azure AD kiracınızda güvenli bir şekilde depolanır ve yalnızca [SSPR kayıt portalı](https://aka.ms/ssprsetup)aracılığıyla kullanıcılara erişilebilir. Yöneticiler başka kullanıcıların sorularının ve yanıtlarının içeriğini göremez, ayarlayabilir veya değiştiremezler.

## <a name="what-happens-when-a-user-registers"></a>Kullanıcı kaydettiğinde ne olur?

Bir Kullanıcı kaydettiğinde, kayıt sayfası aşağıdaki alanları ayarlar:

* **Kimlik doğrulama telefonu**
* **Kimlik doğrulama e-postası**
* **Güvenlik soruları ve yanıtları**

*Cep telefonu* veya *Alternatif e-posta*için bir değer sağladıysanız, kullanıcılar bu değerleri hizmet için kayıtlı olmasalar bile parolalarını sıfırlamak için hemen kullanabilir.

Kullanıcılar, ilk kez kaydolduklarında bu değerleri de görür ve isterseniz bunları değiştirebilir. Başarıyla kaydedildikten sonra bu değerler, sırasıyla *kimlik doğrulama telefon* ve *kimlik doğrulama e-posta* alanlarında kalıcı hale getirilir.

## <a name="set-and-read-the-authentication-data-through-powershell"></a>PowerShell ile kimlik doğrulama verilerini ayarlama ve okuma

Aşağıdaki alanlar PowerShell aracılığıyla ayarlanabilir:

* *Alternatif e-posta*
* *Cep telefonu*
* *Ofis telefonu*
    * Yalnızca şirket içi bir dizinle eşitleme yapsanız ayarlanabilir.

> [!IMPORTANT]
> PowerShell v1 ve PowerShell V2 arasındaki komut özelliklerinde bilinen bir eşlik eksikliği vardır. [Kimlik doğrulama yöntemleri için Microsoft Graph REST API (Beta)](/graph/api/resources/authenticationmethods-overview) , modern etkileşim sağlamak için geçerli mühendislik odadır.

### <a name="use-powershell-version-1"></a>PowerShell sürüm 1 kullan

Başlamak için [Azure AD PowerShell modülünü indirip yükleyin](/previous-versions/azure/jj151815(v=azure.100)#bkmk_installmodule). Yüklendikten sonra, her alanı yapılandırmak için aşağıdaki adımları kullanın.

#### <a name="set-the-authentication-data-with-powershell-version-1"></a>PowerShell sürüm 1 ile kimlik doğrulama verilerini ayarlama

```PowerShell
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 4251234567"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 4252345678"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 4251234567" -PhoneNumber "+1 4252345678"
```

#### <a name="read-the-authentication-data-with-powershell-version-1"></a>PowerShell sürüm 1 ile kimlik doğrulama verilerini okuma

```PowerShell
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="read-the-authentication-phone-and-authentication-email-options"></a>Kimlik doğrulama telefon ve kimlik doğrulama e-posta seçeneklerini okuyun

PowerShell sürüm 1 ' i kullanırken **kimlik doğrulama telefonunu** ve **kimlik doğrulama e-postasını** okumak için aşağıdaki komutları kullanın:

```PowerShell
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="use-powershell-version-2"></a>PowerShell sürüm 2 kullan

Başlamak için [Azure AD sürüm 2 PowerShell modülünü indirip yükleyin](/powershell/module/azuread/?view=azureadps-2.0).

Tarafından desteklenen son PowerShell sürümlerinden hızlıca yüklemek için `Install-Module` aşağıdaki komutları çalıştırın. İlk satır modülün zaten yüklü olup olmadığını denetler:

```PowerShell
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

Modül yüklendikten sonra, her alanı yapılandırmak için aşağıdaki adımları kullanın.

#### <a name="set-the-authentication-data-with-powershell-version-2"></a>PowerShell sürüm 2 ile kimlik doğrulama verilerini ayarlama

```PowerShell
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 4251234567"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 4252345678"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 4251234567" -TelephoneNumber "+1 4252345678"
```

#### <a name="read-the-authentication-data-with-powershell-version-2"></a>PowerShell sürüm 2 ile kimlik doğrulama verilerini okuma

```PowerShell
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Sonraki adımlar

Kimlik doğrulama iletişim bilgileri kullanıcılar için önceden doldurulduktan sonra, self servis parola sıfırlama özelliğini etkinleştirmek için aşağıdaki öğreticiyi doldurun:

> [!div class="nextstepaction"]
> [Azure AD self servis parola sıfırlamayı etkinleştirme](tutorial-enable-sspr.md)

[Contact]: ./media/howto-sspr-authenticationdata/user-authentication-contact-info.png "Genel Yöneticiler, kullanıcının kimlik doğrulama iletişim bilgilerini değiştirebilir"