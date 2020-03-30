---
title: PowerShell - Azure AD | Microsoft Dokümanlar
description: Azure Active Directory cmdlets kullanarak grupların ayarlarını nasıl yönetin?
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/20/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d56bb7c30a8289fe7f261979dca6a4ffe2bfe99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80048148"
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri

Bu makalede, gruplar oluşturmak ve güncelleştirmek için Azure Active Directory (Azure AD) PowerShell cmdlets kullanma yönergeleri içerir. Bu içerik yalnızca Office 365 grupları (bazen birleşik gruplar olarak adlandırılır) için geçerlidir.

> [!IMPORTANT]
> Bazı ayarlar için Azure Active Directory Premium P1 lisansı gerekir. Daha fazla bilgi için [Şablon ayarları](#template-settings) tablosuna bakın.

Yönetici olmayan kullanıcıların [Set-MSOLCompanySettings'te](https://docs.microsoft.com/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)açıklandığı `Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False` şekilde ayarlanmış güvenlik grupları oluşturmasını nasıl önleyeceğimize ilişkin daha fazla bilgi için.

Office 365 Grupları ayarları, Ayarlar nesnesi ve AyarlarŞablon nesnesi kullanılarak yapılandırılır. Dizinvarsayılan ayarlarla yapılandırıldığından, başlangıçta dizininizde hiç Ayarlar nesnesi göremezsiniz. Varsayılan ayarları değiştirmek için, ayarlar şablonu kullanarak yeni bir ayarlar nesnesi oluşturmanız gerekir. Ayarlar şablonları Microsoft tarafından tanımlanır. Birkaç farklı ayar şablonu vardır. Dizininiz için Office 365 grup ayarlarını yapılandırmak için "Group.Unified" adlı şablonu kullanırsınız. Tek bir grupta Office 365 grup ayarlarını yapılandırmak için "Group.Unified.Guest" adlı şablonu kullanın. Bu şablon, bir Office 365 grubuna konuk erişimini yönetmek için kullanılır. 

Cmdletler Azure Active Directory PowerShell V2 modülünün bir parçasıdır. Modülü bilgisayarınıza nasıl indirip yükleyebilirsiniz yönergeleri için [Azure Active Directory PowerShell Sürüm 2](https://docs.microsoft.com/powershell/azuread/)makalesine bakın. Modülün sürüm 2 sürümünü [PowerShell galerisinden](https://www.powershellgallery.com/packages/AzureAD/)yükleyebilirsiniz.

## <a name="install-powershell-cmdlets"></a>PowerShell cmdlet'lerini yükleme

Windows PowerShell için Graph Modülü için Azure Active Directory PowerShell'in eski bir sürümünü kaldırdığınızdan ve PowerShell komutlarını çalıştırmadan önce [Azure Active Directory PowerShell for Graph - Public Preview Release (daha sonra 2.0.0.137)](https://www.powershellgallery.com/packages/AzureADPreview) yüklediğinizden emin olun.

1. Windows PowerShell uygulamasını yönetici olarak açın.
2. Eski AzureADPreview sürümlerini kaldırın.
  
   ``` PowerShell
   Uninstall-Module AzureADPreview
   Uninstall-Module azuread
   ```

3. En son AzureADPreview sürümünü yükleyin.
  
   ``` PowerShell
   Install-Module AzureADPreview
   ```
   
## <a name="create-settings-at-the-directory-level"></a>Dizin düzeyinde ayarlar oluşturma
Bu adımlar, dizindeki tüm Office 365 grupları için geçerli olan dizin düzeyinde ayarlar oluşturur. AzureADDirectorySettingTemplate cmdlet yalnızca Graph için [Azure AD PowerShell Önizleme modülünde](https://www.powershellgallery.com/packages/AzureADPreview)kullanılabilir.

1. Dizin Ayarları cmdlets'te kullanmak istediğiniz Ayarlar Şablonu'nun kimliğini belirtmeniz gerekir. Bu kimliği bilmiyorsanız, bu cmdlet tüm ayarlar şablonlarının listesini döndürür:
  
   ```powershell
   Get-AzureADDirectorySettingTemplate

   ```
   Bu cmdlet çağrısı kullanılabilir tüm şablonları döndürür:
  
   ``` PowerShell
   Id                                   DisplayName         Description
   --                                   -----------         -----------
   62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
   08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Office 365 group
   16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define the different settings that can be used for the associ...
   4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
   898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
   5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
   ```
2. Kullanım kılavuzu URL'si eklemek için öncelikle kullanım kılavuzu URL değerini tanımlayan SettingsTemplate nesnesini almanız gerekir; diğer bir şey, Group.Unified şablonu:
  
   ```powershell
   $TemplateId = (Get-AzureADDirectorySettingTemplate | where { $_.DisplayName -eq "Group.Unified" }).Id
   $Template = Get-AzureADDirectorySettingTemplate | where -Property Id -Value $TemplateId -EQ
   ```
3. Ardından, bu şablonu temel alan yeni bir ayarlar nesnesi oluşturun:
  
   ```powershell
   $Setting = $Template.CreateDirectorySetting()
   ```  
4. Ardından kullanım kılavuzu değerini güncelleştirin:
  
   ```powershell
   $Setting["UsageGuidelinesUrl"] = "https://guideline.example.com"
   ```  
5. Ardından ayarı uygulayın:
  
   ```powershell
   New-AzureADDirectorySetting -DirectorySetting $Setting
   ```
6. Değerleri aşağıdakileri kullanarak okuyabilirsiniz:

   ```powershell
   $Setting.Values
   ```
   
## <a name="update-settings-at-the-directory-level"></a>Dizin düzeyindeki ayarları güncelleştirme
Ayar şablonundaki KullanımKılavuzuLinesUrl değerini güncelleştirmek için Azure AD'deki geçerli ayarları okuyun, aksi takdirde UsageGuideLinesUrl dışındaki varolan ayarların üzerine yazabiliriz.

1. Group.Unified SettingsTemplate'ten geçerli ayarları alın:
   
   ```powershell
   $Setting = Get-AzureADDirectorySetting | ? { $_.DisplayName -eq "Group.Unified"}
   ```  
2. Geçerli ayarları kontrol edin:
   
   ```powershell
   $Setting.Values
   ```
   
   Çıktı:
   ```powershell
    Name                          Value
    ----                          -----
    EnableMIPLabels               false
    CustomBlockedWordsList
    EnableMSStandardBlockedWords  False
    ClassificationDescriptions
    DefaultClassification
    PrefixSuffixNamingRequirement
    AllowGuestsToBeGroupOwner     False
    AllowGuestsToAccessGroups     True
    GuestUsageGuidelinesUrl
    GroupCreationAllowedGroupId
    AllowToAddGuests              True
    UsageGuidelinesUrl            https://guideline.example.com
    ClassificationList
    EnableGroupCreation           True
    ```
3. UsageGuideLinesUrl değerini kaldırmak için URL'yi boş bir dize olarak edin:
   
   ```powershell
   $Setting["UsageGuidelinesUrl"] = ""
   ```  
4. Güncelleştirmeyi dizine kaydet:
   
   ```powershell
   Set-AzureADDirectorySetting -Id $Setting.Id -DirectorySetting $Setting
   ```  

## <a name="template-settings"></a>Şablon ayarları
Group.Unified SettingsTemplate'te tanımlanan ayarlar aşağıda verilmiştir. Aksi belirtilmedikçe, bu özellikler azure Active Directory Premium P1 lisansı gerektirir. 

| **Ayar** | **Açıklama** |
| --- | --- |
|  <ul><li>Grup Oluşturmayı Etkinleştirme<li>Türü: Boolean<li>Varsayılan: True |Office 365 grup oluşturmanın yönetici olmayan kullanıcılar tarafından dizine izin verilip verilmediğini belirten bayrak. Bu ayar, Azure Active Directory Premium P1 lisansı gerektirmez.|
|  <ul><li>GroupCreationAllowedGroupId<li>Türü: Dize<li>Varsayılan: "" |EnableGroupCreation == yanlış olsa bile, üyelerin Office 365 gruplarını oluşturmasına izin verilen güvenlik grubunun GUID'i. |
|  <ul><li>Kullanım KılavuzuUrl<li>Türü: Dize<li>Varsayılan: "" |Grup Kullanım Yönergeleri'ne bağlantı. |
|  <ul><li>Sınıflandırma Açıklamalar<li>Türü: Dize<li>Varsayılan: "" | Sınıflandırma açıklamalarının virgülle sınırlandırıladaki listesi. Sınıflandırma Açıklamaları'nın değeri yalnızca bu biçimde geçerlidir:<br>$setting["SınıflandırmaTanımları"] ="Sınıflandırma:Açıklama,Sınıflandırma:Açıklama"<br>Sınıflandırma Listesi'ndeki bir girişle eşleşir.<br>EnableMIPLabels == True olduğunda bu ayar geçerli değildir.|
|  <ul><li>Varsayılan Sınıflandırma<li>Türü: Dize<li>Varsayılan: "" | Hiçbiri belirtilmemişse, bir grup için varsayılan sınıflandırma olarak kullanılacak sınıflandırma.<br>EnableMIPLabels == True olduğunda bu ayar geçerli değildir.|
|  <ul><li>ÖnekSuffixNamingRequirement<li>Türü: Dize<li>Varsayılan: "" | Office 365 grupları için yapılandırılan adlandırma kuralını tanımlayan en fazla 64 karakter uzunluğundaki dize. Daha fazla bilgi için [bkz.](groups-naming-policy.md) |
| <ul><li>CustomBlockedWordsList<li>Türü: Dize<li>Varsayılan: "" | Kullanıcıların grup adlarında veya takma adlarda kullanmasına izin verilmeyecek virgülle ayrılmış tümcecikler dizisi. Daha fazla bilgi için [bkz.](groups-naming-policy.md) |
| <ul><li>EnableMSStandardBlockedWords<li>Türü: Boolean<li>Varsayılan: "False" | Kullanmayın
|  <ul><li>MisafirTobeGroupOwner izin<li>Türü: Boolean<li>Varsayılan: False | Konuk kullanıcının grupların sahibi olup olmadığını belirten Boolean. |
|  <ul><li>Misafirlerin Erişilen Gruplara İzin Verme<li>Türü: Boolean<li>Varsayılan: True | Konuk kullanıcının Office 365 grupları içeriğine erişip erişemeyeceğini belirten boolean.  Bu ayar, Azure Active Directory Premium P1 lisansı gerektirmez.|
|  <ul><li>GuestUsageGuidelinesUrl<li>Türü: Dize<li>Varsayılan: "" | Konuk kullanım yönergelerine bir bağlantının url'si. |
|  <ul><li>İzin veren misafirler<li>Türü: Boolean<li>Varsayılan: True | Bu dizine misafir ekleyip eklememe izin verilip verilmediğini belirten bir boolean. <br>*EnableMIPLabels* *True* olarak ayarlanmışsa ve bir konuk ilkesi gruba atanan duyarlılık etiketiyle ilişkilendirilirse, bu ayar geçersiz kılınabilir ve salt okunur hale gelebilir.<br>AllowToAddGuests ayarı kiracı düzeyinde False olarak ayarlanırsa, grup düzeyindeki AllowToAddGuests ayarı yoksayılır. Yalnızca birkaç grup için konuk erişimini etkinleştirmek istiyorsanız, AllowToAddGuests'ı kiracı düzeyinde doğru olması için ayarlamanız ve ardından belirli gruplar için seçici olarak devre dışı bırakmalısınız. |
|  <ul><li>Sınıflandırma Listesi<li>Türü: Dize<li>Varsayılan: "" | Office 365 Gruplarına uygulanabilecek geçerli sınıflandırma değerlerinin virgülle sınırlandırılma listesi. <br>EnableMIPLabels == True olduğunda bu ayar geçerli değildir.|
|  <ul><li>EnableMIPLabels<li>Türü: Boolean<li>Varsayılan: "False" |Microsoft 365 Uyumluluk Merkezi'nde yayınlanan duyarlılık etiketlerinin Office 365 Gruplarına uygulanıp uygulanamayacağını belirten bayrak. Daha fazla bilgi için Bkz. [Office 365 grupları için Duyarlılık Etiketleri atay.](groups-assign-sensitivity-labels.md) |

## <a name="example-configure-guest-policy-for-groups-at-the-directory-level"></a>Örnek: Dizin düzeyindeki gruplar için Konuk ilkesini yapılandır
1. Tüm ayar şablonlarını alın:
   ```powershell
   Get-AzureADDirectorySettingTemplate
   ```
2. Dizin düzeyindeki gruplar için konuk ilkesi ayarlamak için Group.Unified şablonuna ihtiyacınız vardır
   ```powershell
   $Template = Get-AzureADDirectorySettingTemplate | where -Property Id -Value "62375ab9-6b52-47ed-826b-58e47e0e304b" -EQ
   ```
3. Ardından, bu şablonu temel alan yeni bir ayarlar nesnesi oluşturun:
  
   ```powershell
   $Setting = $template.CreateDirectorySetting()
   ```  
4. Ardından AllowToAddGuests ayarını güncelleştirin
   ```powershell
   $Setting["AllowToAddGuests"] = $False
   ```  
5. Ardından ayarı uygulayın:
  
   ```powershell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```
6. Değerleri aşağıdakileri kullanarak okuyabilirsiniz:

   ```powershell
   $Setting.Values
   ```   

## <a name="read-settings-at-the-directory-level"></a>Ayarları dizin düzeyinde okuma

Almak istediğiniz ayarın adını biliyorsanız, geçerli ayarlar değerini almak için aşağıdaki cmdlet'i kullanabilirsiniz. Bu örnekte, "Kullanım Yönergeleri Url" adlı bir ayarın değerini geri alıyoruz. 

   ```powershell
   (Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
   ```
Bu adımlar, dizindeki tüm Office grupları için geçerli olan dizin düzeyindeki ayarları okur.

1. Varolan tüm dizin ayarlarını okuyun:
   ```powershell
   Get-AzureADDirectorySetting -All $True
   ```
   Bu cmdlet tüm dizin ayarlarının bir listesini döndürür:
   ```powershell
   Id                                   DisplayName   TemplateId                           Values
   --                                   -----------   ----------                           ------
   c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
   ```

2. Belirli bir grubun tüm ayarlarını okuyun:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
   ```

3. Ayarlar Kimliği GUID'i kullanarak belirli bir dizin ayarları nesnesinin tüm dizin ayarları değerlerini okuyun:
   ```powershell
   (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
   ```
   Bu cmdlet, bu belirli grup için bu ayarlar nesnesi adları ve değerleri döndürür:
   ```powershell
   Name                          Value
   ----                          -----
   ClassificationDescriptions
   DefaultClassification
   PrefixSuffixNamingRequirement
   CustomBlockedWordsList        
   AllowGuestsToBeGroupOwner     False 
   AllowGuestsToAccessGroups     True
   GuestUsageGuidelinesUrl
   GroupCreationAllowedGroupId
   AllowToAddGuests              True
   UsageGuidelinesUrl            https://guideline.example.com
   ClassificationList
   EnableGroupCreation           True
   ```

## <a name="remove-settings-at-the-directory-level"></a>Dizin düzeyindeki ayarları kaldırma
Bu adım, dizin düzeyindeki ayarları kaldırır ve bu da dizindeki tüm Office grupları için geçerlidir.
   ```powershell
   Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
   ```

## <a name="create-settings-for-a-specific-group"></a>Belirli bir grup için ayarlar oluşturma

1. "Groups.Unified.Guest" adlı ayarlar şablonunu arayın
   ```powershell
   Get-AzureADDirectorySettingTemplate
  
   Id                                   DisplayName            Description
   --                                   -----------            -----------
   62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
   08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Office 365 group
   4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
   898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
   5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
   ```
2. Groups.Unified.Guest şablonu için şablon nesnesini alın:
   ```powershell
   $Template1 = Get-AzureADDirectorySettingTemplate | where -Property Id -Value "08d542b9-071f-4e16-94b0-74abb372e3d9" -EQ
   ```
3. Şablondan yeni bir ayarlar nesnesi oluşturun:
   ```powershell
   $SettingCopy = $Template1.CreateDirectorySetting()
   ```

4. Ayarı gerekli değere ayarlama:
   ```powershell
   $SettingCopy["AllowToAddGuests"]=$False
   ```
5. Bu ayarı uygulamak istediğiniz grubun kimliğini alın:
   ```powershell
   $groupID= (Get-AzureADGroup -SearchString "YourGroupName").ObjectId
   ```
6. Dizinde gerekli grup için yeni ayar oluşturun:
   ```powershell
   New-AzureADObjectSetting -TargetType Groups -TargetObjectId $groupID -DirectorySetting $SettingCopy
   ```
7. Ayarları doğrulamak için şu komutu çalıştırın:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId $groupID -TargetType Groups | fl Values
   ```

## <a name="update-settings-for-a-specific-group"></a>Belirli bir grubun ayarlarını güncelleştirme
1. Ayarını güncelleştirmek istediğiniz grubun kimliğini alın:
   ```powershell
   $groupID= (Get-AzureADGroup -SearchString "YourGroupName").ObjectId
   ```
2. Grubun ayarını alın:
   ```powershell
   $Setting = Get-AzureADObjectSetting -TargetObjectId $groupID -TargetType Groups
   ```
3. Grubun ayarını istediğiniz gibi güncelleştirin, örneğin.
   ```powershell
   $Setting["AllowToAddGuests"] = $True
   ```
4. Ardından bu özel grubun ayar kimliğini alın:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId $groupID -TargetType Groups
   ```
   Buna benzer bir yanıt alırsınız:
   ```powershell
   Id                                   DisplayName            TemplateId                             Values
   --                                   -----------            -----------                            ----------
   2dbee4ca-c3b6-4f0d-9610-d15569639e1a Group.Unified.Guest    08d542b9-071f-4e16-94b0-74abb372e3d9   {class SettingValue {...
   ```
5. Ardından bu ayar için yeni değeri ayarlayabilirsiniz:
   ```powershell
   Set-AzureADObjectSetting -TargetType Groups -TargetObjectId $groupID -Id 2dbee4ca-c3b6-4f0d-9610-d15569639e1a -DirectorySetting $Setting
   ```
6. Doğru güncelleştirdiğinden emin olmak için ayarın değerini okuyabilirsiniz:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId $groupID -TargetType Groups | fl Values
   ```

## <a name="cmdlet-syntax-reference"></a>Cmdlet sözdizimi başvurusu
[Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0)adresinde daha fazla Azure Active Directory PowerShell dokümantasyonu bulabilirsiniz.

## <a name="additional-reading"></a>Ek okuma

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)
