---
title: Azure AD SSPR veri gereksinimleri-Azure Active Directory
description: Azure AD self servis parola sıfırlama için veri gereksinimleri ve bunların nasıl karşılaacağı
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 12/09/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42f7e120745357d3bd5735cca568bdd6971ea061
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80652350"
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Son Kullanıcı kaydı gerekmeden parola sıfırlamayı dağıtma

Azure Active Directory (Azure AD) self servis parola sıfırlama (SSPR) dağıtmak için, kimlik doğrulama verilerinin mevcut olması gerekir. Bazı kuruluşların kullanıcılarına kendi kimlik doğrulama verilerini girmesi gerekir. Diğer kuruluşlar Active Directory zaten mevcut olan verilerle eşitlemeyi tercih eder. Bu eşitlenmiş veriler, aşağıdaki gereksinimleri karşıladıysanız Kullanıcı etkileşimi gerektirmeden Azure AD ve SSPR için kullanılabilir hale getirilir:

* Şirket içi dizininizdeki verileri düzgün biçimde biçimlendirin.
* [Azure AD Connect Express ayarlarını kullanarak](../hybrid/how-to-connect-install-express.md)yapılandırın.

Doğru şekilde çalışmak için telefon numaralarının *+ CountryCode PhoneNumber*biçiminde olması gerekir, örneğin + 1 4255551234.

> [!NOTE]
> Ülke kodu ile telefon numarası arasında bir boşluk olması gerekir.
>
> Parola sıfırlama, telefon uzantılarını desteklemez. + 1 4255551234X12345 biçiminde bile, çağrı yerleştirilmadan önce uzantılar kaldırılır.

## <a name="fields-populated"></a>Doldurulan alanlar

Azure AD Connect varsayılan ayarları kullanıyorsanız aşağıdaki eşlemeler yapılır:

| Şirket içi Active Directory | Azure AD |
| --- | --- |
| telephoneNumber 'dır | Ofis telefonu |
| mobil | Cep telefonu |

Kullanıcı cep telefonu numarasını doğruladıktan sonra, Azure AD 'de **kimlik doğrulaması iletişim bilgileri** altındaki *Telefon* alanı da bu sayıyla doldurulur.

## <a name="authentication-contact-info"></a>Kimlik doğrulaması iletişim bilgileri

Azure portal bir Azure AD kullanıcısının **kimlik doğrulama yöntemleri** sayfasında, bir genel yönetici, aşağıdaki örnekte gösterildiği gibi, kimlik doğrulama iletişim bilgilerini el ile ayarlayabilir:

![Azure AD 'de bir kullanıcının kimlik doğrulaması iletişim bilgileri][Contact]

* **Telefon** alanı doldurulmuşsa ve SSPR ilkesinde **cep telefonu** etkinleştirilmişse, Kullanıcı bu numarayı parola sıfırlama kaydı sayfasında ve parola sıfırlama iş akışı sırasında görür.
* **Alternatif telefon** alanı parola sıfırlama için kullanılmaz.
* **E** -posta alanı doldurulmuşsa ve SSPR Ilkesinde **e-posta** etkinse, Kullanıcı bu e-postayı parola sıfırlama kaydı sayfasında ve parola sıfırlama iş akışı sırasında görür.
* **Diğer e-posta** alanı doldurulmuşsa ve SSPR Ilkesinde **e-posta** etkinse, Kullanıcı bu e-postayı parola sıfırlama kaydı **sayfasında görmez,** ancak parola sıfırlama iş akışı sırasında görürler.

## <a name="security-questions-and-answers"></a>Güvenlik soruları ve yanıtları

Güvenlik soruları ve yanıtları, Azure AD kiracınızda güvenli bir şekilde depolanır ve yalnızca [SSPR kayıt portalı](https://aka.ms/ssprsetup)aracılığıyla kullanıcılara erişilebilir. Yöneticiler başka kullanıcıların sorularının ve yanıtlarının içeriğini göremez, ayarlayabilir veya değiştiremezler.

## <a name="what-happens-when-a-user-registers"></a>Kullanıcı kaydettiğinde ne olur?

Bir Kullanıcı kaydettiğinde, kayıt sayfası aşağıdaki alanları ayarlar:

* **Kimlik doğrulama telefonu**
* **Kimlik doğrulama e-postası**
* **Güvenlik soruları ve yanıtları**

**Cep telefonu** veya **Alternatif e-posta**için bir değer sağladıysanız, kullanıcılar bu değerleri hizmet için kayıtlı olmasalar bile parolalarını sıfırlamak için hemen kullanabilir. Ayrıca, kullanıcılar ilk kez kaydolduklarında bu değerleri görür ve isterseniz bunları değiştirebilir. Başarıyla kaydolduktan sonra bu değerler, sırasıyla **kimlik doğrulama telefon** ve **kimlik doğrulama e-posta** alanlarında kalıcı hale getirilir.

## <a name="set-and-read-the-authentication-data-through-powershell"></a>PowerShell ile kimlik doğrulama verilerini ayarlama ve okuma

Aşağıdaki alanlar PowerShell aracılığıyla ayarlanabilir:

* **Alternatif e-posta**
* **Cep telefonu**
* **Ofis telefonu**: yalnızca şirket içi bir dizinle eşitleme yapmadıysanız ayarlanabilir

### <a name="use-powershell-version-1"></a>PowerShell sürüm 1 kullan

Başlamak için [Azure AD PowerShell modülünü indirip yüklemeniz](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)gerekir. Yükledikten sonra, her bir alanı yapılandırmak için aşağıdaki adımları kullanabilirsiniz.

#### <a name="set-the-authentication-data-with-powershell-version-1"></a>PowerShell sürüm 1 ile kimlik doğrulama verilerini ayarlama

```PowerShell
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
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

Başlamak için [Azure AD sürüm 2 PowerShell modülünü indirip yüklemeniz](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0)gerekir. Yükledikten sonra, her bir alanı yapılandırmak için aşağıdaki adımları kullanabilirsiniz.

Install-Module ' i destekleyen son PowerShell sürümlerinden hızlıca yüklemek için aşağıdaki komutları çalıştırın. (İlk satır modülün zaten yüklü olup olmadığını denetler.)

```PowerShell
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-the-authentication-data-with-powershell-version-2"></a>PowerShell sürüm 2 ile kimlik doğrulama verilerini ayarlama

```PowerShell
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
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

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir şeyin bozuk olduğunu düşünüyorum. SSPR sorunlarını gidermek Nasıl yaparım??](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Contact]: ./media/howto-sspr-authenticationdata/user-authentication-contact-info.png "Genel Yöneticiler, kullanıcının kimlik doğrulama iletişim bilgilerini değiştirebilir"
