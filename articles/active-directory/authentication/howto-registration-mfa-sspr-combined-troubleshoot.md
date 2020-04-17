---
title: Sorun giderme birleşik kayıt - Azure Active Directory
description: Sorun Giderme Azure AD Çok Faktörlü Kimlik Doğrulaması ve self servis parola sıfırlama kombine kaydı
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: troubleshooting
ms.date: 04/15/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c840df2c53554519f62a3d1d7a7d8b305187ffb
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81450947"
---
# <a name="troubleshooting-combined-security-information-registration"></a>Sorun giderme birleştirilmiş güvenlik bilgi kaydı

Bu makaledeki bilgiler, birleştirilmiş kayıt deneyimi kullanıcıları tarafından bildirilen sorun giderme sorunları olan yöneticilere rehberlik etmek içindir.

## <a name="audit-logs"></a>Denetim günlükleri

Birleşik kayıt için günlüğe kaydedilen olaylar, Azure REKLAM denetim günlüklerinde Kimlik Doğrulama Yöntemleri kategorisinde yer alır.

![Azure AD Denetimi, kayıt olaylarını gösteren arabirimi kaydeder](media/howto-registration-mfa-sspr-combined-troubleshoot/combined-security-info-audit-log.png)

Aşağıdaki tablo, birleşik kayıt tarafından oluşturulan tüm denetim olaylarını listeler:

| Etkinlik | Durum | Neden | Açıklama |
| --- | --- | --- | --- |
| Kullanıcı gerekli tüm güvenlik bilgilerini kaydetti | Başarılı | Kullanıcı gerekli tüm güvenlik bilgilerini kaydetti. | Bu olay, bir kullanıcı kaydı başarıyla tamamladığında oluşur.|
| Kullanıcı gerekli tüm güvenlik bilgilerini kaydetti | Hata | Kullanıcı güvenlik bilgileri kaydını iptal etti. | Bu olay, bir kullanıcı kesme modundan kaydı iptal ettiğinde oluşur.|
| Kullanıcı kayıtlı güvenlik bilgileri | Başarılı | Kullanıcı kayıtlı *yöntem*. | Bu olay, bir kullanıcı tek bir yöntem kaydettiğinde oluşur. *Yöntem* Authenticator uygulaması, Telefon, E-posta, Güvenlik soruları, Uygulama şifresi, Alternatif telefon ve benzeri olabilir.| 
| Kullanıcı gözden güvenlik bilgileri | Başarılı | Kullanıcı güvenlik bilgilerini başarıyla gözden geçirdi. | Bu olay, bir kullanıcı güvenlik bilgileri inceleme sayfasında **iyi görünüyor** seçtiğinde oluşur.|
| Kullanıcı gözden güvenlik bilgileri | Hata | Kullanıcı güvenlik bilgilerini gözden geçiremedi. | Bu olay, bir kullanıcı güvenlik bilgileri inceleme sayfasında **iyi görünüyor** seçer ancak bir şey arka uçta başarısız olur oluşur.|
| Kullanıcı silinen güvenlik bilgileri | Başarılı | Kullanıcı silinen *yöntem*. | Bu olay, bir kullanıcı tek bir yöntemi sildiğinde oluşur. *Yöntem* Authenticator uygulaması, Telefon, E-posta, Güvenlik soruları, Uygulama şifresi, Alternatif telefon ve benzeri olabilir.|
| Kullanıcı silinen güvenlik bilgileri | Hata | Kullanıcı *yöntemi*silmek için başarısız oldu. | Bu olay, bir kullanıcı bir yöntemi silmeye çalıştığında ancak deneme bazı nedenlerden dolayı başarısız olduğunda oluşur. *Yöntem* Authenticator uygulaması, Telefon, E-posta, Güvenlik soruları, Uygulama şifresi, Alternatif telefon ve benzeri olabilir.|
| Kullanıcı varsayılan güvenlik bilgilerini değiştirdi | Başarılı | Kullanıcı *yöntem*için varsayılan güvenlik bilgilerini değiştirdi. | Bu olay, bir kullanıcı varsayılan yöntemi değiştirdiğinde oluşur. *Yöntem* Authenticator uygulaması bildirimi, benim authenticator uygulaması veya belirteç bir kod olabilir, Çağrı +X XXXXXXXxxXXX, Metin bir kod +X XXXXXXXXX, ve benzeri.|
| Kullanıcı varsayılan güvenlik bilgilerini değiştirdi | Hata | Kullanıcı *yöntem*için varsayılan güvenlik bilgilerini değiştirmek için başarısız oldu. | Bu olay, bir kullanıcı varsayılan yöntemi değiştirmeye çalıştığında ancak deneme nin bazı nedenlerden dolayı başarısız olması durumunda oluşur. *Yöntem* Authenticator uygulaması bildirimi, benim authenticator uygulaması veya belirteç bir kod olabilir, Çağrı +X XXXXXXXxxXXX, Metin bir kod +X XXXXXXXXX, ve benzeri.|

## <a name="troubleshooting-interrupt-mode"></a>Sorun giderme kesme modu

| Belirti | Sorun giderme adımları |
| --- | --- |
| Görmeyi umduğum yöntemleri göremiyorum. | 1. Kullanıcının Azure AD yöneticisi rolü olup olmadığını kontrol edin. Evet ise, SSPR yönetici ilkesi farklılıklarını görüntüleyin. <br> 2. Kullanıcının Çok Faktörlü Kimlik Doğrulama kaydı nın uygulanması veya SSPR kayıt uygulaması nedeniyle kesintiye uğrayıp uğramadığını belirleyin. Hangi yöntemlerin gösterilmesi gerektiğini belirlemek için "Birleşik kayıt modları" altındaki [akış şemasına](../../active-directory/authentication/concept-registration-mfa-sspr-combined.md#combined-registration-modes) bakın. <br> 3. Çok Faktörlü Kimlik Doğrulama veya SSPR ilkesinin ne kadar yakın zamanda değiştirilmediğini belirleyin. Değişiklik son zamanlarda yapıldıysa, güncelleştirilmiş ilkeğin yayılması biraz zaman alabilir.|

## <a name="troubleshooting-manage-mode"></a>Sorun giderme yönetimi modu

| Belirti | Sorun giderme adımları |
| --- | --- |
| Belirli bir yöntem ekleme seçeneğim yok. | 1. Yöntemin Çok Faktörlü Kimlik Doğrulama veya SSPR için etkinleştirilip etkinleştirilemeyeceğini belirleyin. <br> 2. Yöntem etkinleştirilirse, ilkeleri yeniden kaydedin ve yeniden test etmeden önce 1-2 saat bekleyin. <br> 3. Yöntem etkinse, kullanıcının ayarlamasına izin verilen yöntemin en yüksek sayısını zaten ayarlamadığından emin olun.|

## <a name="disable-combined-registration"></a>Kombine kaydı devre dışı

Bir kullanıcı yeni birleştirilmiş deneyimde bir telefon numarası ve/veya mobil uygulama kaydettirdiğinde, hizmetimiz bu kullanıcıdaki yöntemler için bir bayrak kümesi (StrongAuthentication Methods) damgalar. Bu işlevsellik, çok faktörlü kimlik doğrulama gerektiğinde kullanıcının bu yöntemlerle Çok Faktörlü Kimlik Doğrulama gerçekleştirmesine olanak tanır.

Bir yönetici önizlemeyi etkinleştirilerse, kullanıcılar yeni deneyim üzerinden kaydolursa ve yönetici önizlemeyi devre dışı bırakırsa, kullanıcılar bilmeden Çoklu Faktörlü Kimlik Doğrulama'ya da kaydolabilir.

Birleşik kaydı tamamlamış bir kullanıcı geçerli self servis parola sıfırlama (SSPR) [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)kayıt sayfasına giderse, kullanıcıdan bu sayfaya erişebilmeleri için Çok Faktörlü Kimlik Doğrulama'yı gerçekleştirmesi istenir. Bu adım teknik açıdan beklenmektedir, ancak daha önce yalnızca SSPR için kayıtlı olan kullanıcılar için yenidir. Bu ek adım, başka bir güvenlik düzeyi sağlayarak kullanıcının güvenlik duruşunu iyileştirse de, yöneticiler kullanıcılarını artık Çok Faktörlü Kimlik Doğrulama'yı gerçekleştirememek için geri almak isteyebilir.  

### <a name="how-to-roll-back-users"></a>Kullanıcıları geri alma

Yönetici olarak, bir kullanıcının Çok Faktörlü Kimlik Doğrulama ayarlarını sıfırlamak istiyorsanız, bir sonraki bölümde sağlanan PowerShell komut dosyasını kullanabilirsiniz. Komut dosyası, bir kullanıcının mobil uygulaması ve/veya telefon numarası için StrongAuthenticationMethods özelliğini temizler. Bu komut dosyasını kullanıcılarınız için çalıştırdıysanız, gerekirse Çok Faktörlü Kimlik Doğrulama için yeniden kaydolmaları gerekir. Etkilenen tüm kullanıcıları geri almadan önce bir veya iki kullanıcıyla geri alma test etmenizi öneririz.

İzleyen adımlar, bir kullanıcıyı veya kullanıcı grubunu geri almanıza yardımcı olur.

#### <a name="prerequisites"></a>Ön koşullar

1. Uygun Azure AD PowerShell modüllerini yükleyin. PowerShell penceresinde, modülleri yüklemek için aşağıdaki komutları çalıştırın:

   ```powershell
   Install-Module -Name MSOnline
   Import-Module MSOnline
   ```

1. Etkilenen kullanıcı nesnesi kimliklerinin listesini satır başına bir kimliği olan bir metin dosyası olarak bilgisayarınıza kaydedin. Dosyanın konumuna dikkat edin.
1. Aşağıdaki komut dosyasını bilgisayarınıza kaydedin ve komut dosyasının konumuna dikkat edin:

   ```powershell
   <# 
   //********************************************************
   //*                                                      *
   //*   Copyright (C) Microsoft. All rights reserved.      *
   //*                                                      *
   //********************************************************
   #>

   param($path)

   # Define Remediation Fn
   function RemediateUser {

       param  
       (
           $ObjectId
       )

       $user = Get-MsolUser -ObjectId $ObjectId

       Write-Host "Checking if user is eligible for rollback: UPN: "  $user.UserPrincipalName  " ObjectId: "  $user.ObjectId -ForegroundColor Yellow

       $hasMfaRelyingParty = $false
       foreach($p in $user.StrongAuthenticationRequirements)
       {
           if ($p.RelyingParty -eq "*")
           {
               $hasMfaRelyingParty = $true
               Write-Host "User was enabled for per-user MFA." -ForegroundColor Yellow
           }
       }

       if ($user.StrongAuthenticationMethods.Count -gt 0 -and -not $hasMfaRelyingParty)
       {
           Write-Host $user.UserPrincipalName " is eligible for rollback" -ForegroundColor Yellow
           Write-Host "Rolling back user ..." -ForegroundColor Yellow
           Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName $user.UserPrincipalName
           Write-Host "Successfully rolled back user " $user.UserPrincipalName -ForegroundColor Green
       }
       else
       {
           Write-Host $user.UserPrincipalName " is not eligible for rollback. No action required."
       }

       Write-Host ""
       Start-Sleep -Milliseconds 750
   }

   # Connect
   Import-Module MSOnline
   Connect-MsolService

   foreach($line in Get-Content $path)
   {
       RemediateUser -ObjectId $line
   }
   ```

#### <a name="rollback"></a>Geri alma

PowerShell penceresinde, komut dosyası ve kullanıcı dosyası konumlarını sağlayarak aşağıdaki komutu çalıştırın. İstendiğinde genel yönetici kimlik bilgilerini girin. Komut dosyası, her kullanıcı güncelleştirme işleminin sonucunu çıktıracaktır.

`<script location> -path <user file location>`

### <a name="disable-the-updated-experience"></a>Güncelleştirilmiş deneyimi devre dışı atma

Kullanıcılarınız için güncelleştirilmiş deneyimi devre dışı katmak için aşağıdaki adımları tamamlayın:

1. Azure portalında kullanıcı yöneticisi olarak oturum açın.
2. Azure **Active Directory** > **Kullanıcı ayarlarına** > gidin**Erişim paneli önizleme özellikleri için ayarları yönetin.**
3. Kullanıcılar altında **güvenlik bilgilerini kaydetmek ve yönetmek için önizleme özelliklerini kullanabilir,** seçiciyi **Yok**olarak ayarlayabilir ve ardından **Kaydet'i**seçebilir.

Kullanıcılardan artık güncelleştirilmiş deneyimi kullanarak kaydolması istenmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Self servis parola sıfırlama ve Azure Çok Faktörlü Kimlik Doğrulama için birleşik kayıt hakkında daha fazla bilgi edinin](concept-registration-mfa-sspr-combined.md)
