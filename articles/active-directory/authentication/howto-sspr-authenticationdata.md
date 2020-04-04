---
title: Azure AD SSPR veri gereksinimleri - Azure Active Directory
description: Azure AD self servis parola sıfırlama ve bunları nasıl karşılarsınız için veri gereksinimleri
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
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652350"
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Son kullanıcı kaydı gerektirmeden parola sıfırlama dağıtma

Azure Etkin Dizini (Azure AD) self servis parola sıfırlama (SSPR) dağıtmak için kimlik doğrulama verilerinin bulunması gerekir. Bazı kuruluşlar, kullanıcılarının kimlik doğrulama verilerini kendileri girmesini sağlar. Diğer kuruluşlar Active Directory'de zaten var olan verilerle eşitlemeyi tercih ediyor. Bu eşitlenmiş veriler, aşağıdaki gereksinimleri karşılıyorsanız kullanıcı etkileşimi gerektirmeden Azure AD ve SSPR tarafından kullanılabilir hale getirilir:

* Şirket içi dizininizdeki verileri düzgün bir şekilde biçimlendirin.
* Azure [AD Connect'i ekspres ayarları kullanarak](../hybrid/how-to-connect-install-express.md)yapılandırın.

Düzgün çalışabilmek için telefon numaraları *+CountryCode Telefon Numarası +1*4255551234 biçiminde olmalıdır.

> [!NOTE]
> Ülke kodu ile telefon numarası arasında bir boşluk olması gerekiyor.
>
> Parola sıfırlama telefon uzantılarını desteklemez. +1 4255551234X12345 biçiminde bile, arama yerleştirilmeden önce uzantılar kaldırılır.

## <a name="fields-populated"></a>Doldurulan alanlar

Azure AD Connect'te varsayılan ayarları kullanıyorsanız, aşağıdaki eşlemeler yapılır:

| Şirket içi Active Directory | Azure AD |
| --- | --- |
| Telephonenumber | Ofis telefonu |
| mobil | Cep telefonu |

Bir kullanıcı cep telefonu numarasını doğruladıktan sonra, Azure AD'deki **Kimlik Doğrulama kişi bilgileri** altındaki *Telefon* alanı da bu numarayla doldurulur.

## <a name="authentication-contact-info"></a>Kimlik doğrulama iletişim bilgileri

Azure portalındaki bir Azure REKLAM kullanıcısının **Kimlik Doğrulama yöntemleri** sayfasında, Bir Global Yönetici aşağıdaki örnek ekran görüntüsünde gösterildiği gibi kimlik doğrulama iletişim bilgilerini el ile ayarlayabilir:

![Azure AD'deki bir kullanıcının kimlik doğrulama kişi bilgileri][Contact]

* **Telefon** alanı doldurulursa ve **Cep telefonu** SSPR ilkesinde etkinse, kullanıcı bu numarayı parola sıfırlama kayıt sayfasında ve parola sıfırlama iş akışı sırasında görür.
* **Alternatif telefon** alanı parola sıfırlama için kullanılmaz.
* **E-posta** alanı doldurulursa ve SSPR ilkesinde **E-posta** etkinse, kullanıcı bu e-postayı parola sıfırlama kayıt sayfasında ve parola sıfırlama iş akışı sırasında görür.
* Alternatif **e-posta** alanı doldurulursa ve SSPR ilkesinde **E-posta** etkinleştirilmişse, **kullanıcı** bu e-postayı parola sıfırlama kayıt sayfasında görmez, ancak parola sıfırlama iş akışı sırasında görür.

## <a name="security-questions-and-answers"></a>Güvenlik soruları ve cevapları

Güvenlik soruları ve yanıtları Azure AD kiracınızda güvenli bir şekilde saklanır ve kullanıcılara yalnızca [SSPR kayıt portalı](https://aka.ms/ssprsetup)üzerinden erişilebilir. Yöneticiler başka kullanıcıların soru ve yanıtlarının içeriğini göreemez, ayarlayabilir veya değiştiremez.

## <a name="what-happens-when-a-user-registers"></a>Bir kullanıcı kaydolduğunda ne olur?

Bir kullanıcı kaydolduğunda, kayıt sayfası aşağıdaki alanları ayarlar:

* **Kimlik Doğrulama Telefonu**
* **Kimlik Doğrulama E-postası**
* **Güvenlik Soruları ve Cevapları**

**Cep telefonu** veya **Alternatif e-posta**için bir değer sağladıysanız, kullanıcılar hizmete kaydedilmemiş olsalar bile parolalarını sıfırlamak için bu değerleri hemen kullanabilirler. Ayrıca, kullanıcılar bu değerleri ilk kez kaydettiklerinde görür ler ve isterlerse değiştirebilirler. Başarılı bir şekilde kaydedildikten sonra, bu değerler sırasıyla **Kimlik Doğrulama Telefonu** ve Kimlik Doğrulama **E-postası** alanlarında kalıcıdır.

## <a name="set-and-read-the-authentication-data-through-powershell"></a>PowerShell aracılığıyla kimlik doğrulama verilerini ayarlayın ve okuyun

Aşağıdaki alanlar PowerShell üzerinden ayarlanabilir:

* **Alternatif e-posta**
* **Cep telefonu**
* **Ofis telefonu**: Yalnızca şirket içi bir dizini yle eşitleme bilmiyorsanız ayarlanabilir

### <a name="use-powershell-version-1"></a>PowerShell sürüm 1'i kullanma

Başlamak için [Azure AD PowerShell modüllerini indirmeniz ve yüklemeniz](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)gerekir. Yükledikten sonra, her alanı yapılandırmak için izleyen adımları kullanabilirsiniz.

#### <a name="set-the-authentication-data-with-powershell-version-1"></a>PowerShell sürüm 1 ile kimlik doğrulama verilerini ayarlama

```PowerShell
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-1"></a>PowerShell sürüm 1 ile kimlik doğrulama verilerini okuyun

```PowerShell
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="read-the-authentication-phone-and-authentication-email-options"></a>Kimlik Doğrulama Telefonu ve Kimlik Doğrulama E-posta seçeneklerini okuyun

PowerShell sürüm 1'i kullandığınızda **Kimlik Doğrulama Telefonu** ve Kimlik Doğrulama **E-postasını** okumak için aşağıdaki komutları kullanın:

```PowerShell
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="use-powershell-version-2"></a>PowerShell sürüm 2'yi kullanma

Başlamak için [Azure AD sürüm 2 PowerShell modüllerini indirmeniz ve yüklemeniz](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0)gerekir. Yükledikten sonra, her alanı yapılandırmak için izleyen adımları kullanabilirsiniz.

PowerShell'in Install-Module'i destekleyen son sürümlerinden hızlı bir şekilde yüklemek için aşağıdaki komutları çalıştırın. (İlk satır, modülün zaten yüklü olup olmadığını denetler.)

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

#### <a name="read-the-authentication-data-with-powershell-version-2"></a>PowerShell sürüm 2 ile kimlik doğrulama verilerini okuyun

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
* [Sanırım bir şey kırıldı. SSPR'yi nasıl giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Contact]: ./media/howto-sspr-authenticationdata/user-authentication-contact-info.png "Global yöneticiler, kullanıcının kimlik doğrulama kişi bilgilerini değiştirebilir"
