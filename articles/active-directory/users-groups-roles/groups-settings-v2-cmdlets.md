---
title: Grupları yönetmek için PowerShell V2 örnekleri - Azure AD | Microsoft Dokümanlar
description: Bu sayfa, Gruplarınızı Azure Etkin Dizini'nde yönetmenize yardımcı olacak PowerShell örnekleri sağlar
keywords: Azure AD, Azure Etkin Dizini, PowerShell, Gruplar, Grup yönetimi
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a218e956c72f8005e533db7b8800e98ee72ce223
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74233106"
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a>Grup yönetimi için Azure Active Directory sürüm 2 cmdlets

> [!div class="op_single_selector"]
> - [Azure portalında](../fundamentals/active-directory-groups-create-azure-portal.md?context=azure/active-directory/users-groups-roles/context/ugr-context)
> - [Powershell](groups-settings-v2-cmdlets.md)
>
>

Bu makalede, Gruplarınızı Azure Etkin Dizini'nde (Azure AD) yönetmek için PowerShell'in nasıl kullanılacağına ait örnekler yer alıyor.  Ayrıca Azure AD PowerShell modülüyle nasıl kurulum alabildiğinizi de söyler. İlk olarak, [Azure AD PowerShell modüllerini indirmeniz](https://www.powershellgallery.com/packages/AzureAD/)gerekir.

## <a name="install-the-azure-ad-powershell-module"></a>Azure AD PowerShell modüllerini yükleme

Azure AD PowerShell modüllerini yüklemek için aşağıdaki komutları kullanın:

```powershell
    PS C:\Windows\system32> install-module azuread
    PS C:\Windows\system32> import-module azuread
```

Modülün kullanıma hazır olup olmadığını doğrulamak için aşağıdaki komutu kullanın:

```powershell
    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
```

Şimdi modüldeki cmdlets kullanmaya başlayabilirsiniz. Azure AD modülündeki cmdletlerin tam açıklaması için lütfen [Azure Active Directory PowerShell Sürüm 2](/powershell/azure/install-adv2?view=azureadps-2.0)için çevrimiçi başvuru belgelerine bakın.

## <a name="connect-to-the-directory"></a>Dizine bağlanma

Azure AD PowerShell cmdlets kullanarak grupları yönetmeye başlamadan önce PowerShell oturumunuzu yönetmek istediğiniz dizine bağlamanız gerekir. Aşağıdaki komutu kullanın:

```powershell
    PS C:\Windows\system32> Connect-AzureAD
```

Cmdlet, dizininize erişmek için kullanmak istediğiniz kimlik bilgilerini ister. Bu örnekte, gösteri karen@drumkit.onmicrosoft.com dizinine erişmek için kullanıyoruz. Cmdlet, oturumun dizininize başarıyla bağlandığını gösteren bir onay döndürür:

```powershell
    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…
```

Artık dizininizdeki grupları yönetmek için AzureAD cmdletlerini kullanmaya başlayabilirsiniz.

## <a name="retrieve-groups"></a>Grupları al

Varolan grupları dizininizden almak için AzureADGroups cmdlet'ini kullanın. 

Dizindeki tüm grupları almak için, parametreler olmadan cmdlet kullanın:

```powershell
    PS C:\Windows\system32> get-azureadgroup
```

Cmdlet bağlı dizindeki tüm grupları döndürür.

-objectID parametresini, grubun objectID'sini belirttiğiniz belirli bir grubu almak için kullanabilirsiniz:

```powershell
    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b
```

Cmdlet şimdi objectID girdiğiniz parametrenin değeriyle eşleşen grubu döndürür:

```powershell
    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True
```

-filtre parametresini kullanarak belirli bir grubu arayabilirsiniz. Bu parametre bir ODATA filtre yan tümcesi alır ve aşağıdaki örnekte olduğu gibi filtreyle eşleşen tüm grupları döndürür:

```powershell
    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True
```

> [!NOTE]
> Azure AD PowerShell cmdlets OData sorgu standardını uygular. Daha fazla bilgi için, [OData bitiş noktasını kullanarak OData sistem sorgu seçeneklerindeki](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter) **$filter** bakın.

## <a name="create-groups"></a>Grup oluşturma

Dizininizde yeni bir grup oluşturmak için Yeni AzureADGroup cmdlet'ini kullanın. Bu cmdlet "Pazarlama" adlı yeni bir güvenlik grubu oluşturur:

```powershell
    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"
```

## <a name="update-groups"></a>Grupları güncelleştir

Varolan bir grubu güncelleştirmek için Set-AzureADGroup cmdlet'ini kullanın. Bu örnekte, "Intune Administrators" grubunun DisplayName özelliğini değiştiriyoruz. İlk olarak, Grubu AzureADGroup cmdlet ve filtresini kullanarak DisplayName özniteliğini kullanarak buluyoruz:

```powershell
    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True
```

Ardından, Açıklama özelliğini yeni değer "Intune Device Administrators" olarak değiştiriyoruz:

```powershell
    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"
```

Şimdi, grubu yeniden bulursak, Açıklama özelliğinin yeni değeri yansıtacak şekilde güncellendiğini görürüz:

```powershell
    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"

    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True
```

## <a name="delete-groups"></a>Grupları silme

Grupları dizininizden silmek için Azure'u KaldırADGroup cmdlet'ini aşağıdaki gibi kullanın:

```powershell
    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b
```

## <a name="manage-group-membership"></a>Grup üyeliğini yönetme

### <a name="add-members"></a>Üye ekle

Bir gruba yeni üyeler eklemek için Ekle-AzureADGroupMember cmdlet'ini kullanın. Bu komut, önceki örnekte kullandığımız Intune Yöneticileri grubuna bir üye ekler:

```powershell
    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea
```

-ObjectId parametresi, üye eklemek istediğimiz grubun ObjectID'sidir ve -RefObjectId gruba üye olarak eklemek istediğimiz kullanıcının ObjectID'sidir.

### <a name="get-members"></a>Üye alın

Bir grubun varolan üyelerini almak için, aşağıdaki örnekte olduğu gibi AzureADGroupMember cmdlet'ini kullanın:

```powershell
    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User
```

### <a name="remove-members"></a>Üyeleri kaldırma

Gruba daha önce eklediğimiz üyeyi kaldırmak için, burada gösterildiği gibi Kaldır-AzureADGroupMember cmdlet'ini kullanın:

```powershell
    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea
```

### <a name="verify-members"></a>Üyeleri doğrula

Bir kullanıcının grup üyeliklerini doğrulamak için Select-AzureADGroupSUserIsMemberOf cmdlet'i kullanın. Bu cmdlet, grup üyeliklerini kontrol etmek için kullanıcının ObjectId'ini ve üyelikleri kontrol edilebilen grupların listesini parametreleri olarak alır. Grupların listesi "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck" türünde karmaşık bir değişken şeklinde sağlanmalıdır, bu nedenle önce bu türde bir değişken oluşturmalıyız:

```powershell
    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck
```

Daha sonra, bu karmaşık değişkenin "GroupIds" özniteliğini kontrol etmek için groupId'ler için değerler sağlarız:

```powershell
    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"
```

Şimdi, objectid 72cd4bbd-2594-40a2-935c-016f3cfeeeea ile bir kullanıcının grup üyeliklerini $g gruplara karşı kontrol etmek istiyorsak, aşağıdakileri kullanmalıyız:

```powershell
    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}
```

Döndürülen değer, bu kullanıcının üyesi olduğu grupların listesidir. Ayrıca bu yöntemi, Select-AzureADGroupSContactIsMemberOf, Select-AzureADGroupIdsGroupIsMemberOf veya belirli bir grup listesi için Kişiler, Gruplar veya Hizmet Müdürleri üyeliğini denetlemek için de uygulayabilirsiniz. Select-AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="disable-group-creation-by-your-users"></a>Kullanıcılarınızın grup oluşturmayı devre dışı arası

Yönetici olmayan kullanıcıların güvenlik grupları oluşturmasını engelleyebilirsiniz. Microsoft Çevrimiçi Dizin Hizmetleri'ndeki (MSODS) varsayılan davranış, yönetici olmayan kullanıcıların self servis grup yönetimi (SSGM) etkin olsun veya olmasın gruplar oluşturmasına izin vermektir. SSGM ayarı yalnızca Uygulamalarım erişim panelindeki davranışları denetler.

Yönetici olmayan kullanıcılar için grup oluşturmayı devre dışı kalmak için:

1. Yönetici olmayan kullanıcıların grup oluşturmasına izin verildiğini doğrulayın:
   
   ```powershell
   PS C:\> Get-MsolCompanyInformation | fl UsersPermissionToCreateGroupsEnabled
   ```
  
2. Dönerse, `UsersPermissionToCreateGroupsEnabled : True`yönetici olmayan kullanıcılar gruplar oluşturabilir. Bu özelliği devre dışı kalmak için:
  
   ```powershell 
   Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False
   ```
  
## <a name="manage-owners-of-groups"></a>Grup sahiplerini yönetme

Bir gruba sahip eklemek için Ekle-AzureADGroupOwner cmdlet'ini kullanın:

```powershell
    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea
```

-ObjectId parametresi, bir sahip eklemek istediğimiz grubun ObjectID'sidir ve -RefObjectId, grubun sahibi olarak eklemek istediğimiz kullanıcı veya hizmet sorumlusunun ObjectID'sidir.

Bir grubun sahiplerini almak için Get-AzureADGroupOwner cmdlet'ini kullanın:

```powershell
    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
```

Cmdlet, belirtilen grup için sahiplerinin (kullanıcılar ve hizmet müdürleri) listesini döndürür:

```powershell
    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User
```

Bir sahibini gruptan kaldırmak istiyorsanız, Kaldır-AzureADGroupOwner cmdlet'ini kullanın:

```powershell
    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67
```

## <a name="reserved-aliases"></a>Ayrılmış takma adlar

Bir grup oluşturulduğunda, belirli uç noktalar son kullanıcının grubun e-posta adresinin bir parçası olarak kullanılmak üzere bir posta Takma adı veya takma ad belirtmesine olanak sağlar.Aşağıdaki yüksek ayrıcalıklı e-posta takma adlarını içeren gruplar yalnızca bir Azure AD genel yöneticisi tarafından oluşturulabilir. 
  
* Kötüye
* yönetici
* yönetici
* hostmaster
* majordomo
* Postmaster
* kök
* Güvenli
* güvenlik
* ssl-admin
* Webmaster

## <a name="group-writeback-to-on-premises-preview"></a>Şirket içi yazmayı gruplandırma (önizleme)

Bugün, birçok grup hala şirket içi Active Directory yönetilir. Bulut gruplarını şirket içinde eşitleme isteklerini yanıtlamak için, Azure AD için Office 365 grupları yazma özelliği artık önizleme için kullanılabilir.

Office 365 grupları bulutta oluşturulur ve yönetilir. Geri yazma özelliği, Exchange yüklü bir Active Directory ormanına dağıtım grupları olarak Office 365 gruplarını geri yazmanızı sağlar. Şirket içi Exchange posta kutularına sahip kullanıcılar bu gruplardan e-posta gönderip alabilirler. Grup geri yazma özelliği Azure AD güvenlik gruplarını veya dağıtım gruplarını desteklemez.

Daha fazla bilgi için lütfen [Azure AD Connect eşitleme hizmetiiçin](../hybrid/how-to-connect-syncservice-features.md)belgelere bakın.

Office 365 grup yazma, Azure Etkin Dizinin (Azure AD) genel önizleme özelliğidir ve herhangi bir ücretli Azure AD lisans planında kullanılabilir. Önizlemeler hakkında bazı yasal bilgiler [için, Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0)adresinde daha fazla Azure Active Directory PowerShell dokümantasyonu bulabilirsiniz.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md?context=azure/active-directory/users-groups-roles/context/ugr-context)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md?context=azure/active-directory/users-groups-roles/context/ugr-context)
