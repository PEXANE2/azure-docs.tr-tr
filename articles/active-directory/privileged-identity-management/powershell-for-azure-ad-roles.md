---
title: PIM'de Azure REKLAM rolleri için PowerShell - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) PowerShell cmdlets'i kullanarak Azure REKLAM rollerini yönetin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/26/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5fa241a261b8dcb21dd39b5dacacac9aa4889304
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80519660"
---
# <a name="powershell-for-azure-ad-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetiminde Azure REKLAM rolleri için PowerShell

Bu makalede, Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure AD rollerini yönetmek için Azure Active Directory (Azure AD) PowerShell cmdlets'i kullanma yönergeleri bulunur. Ayrıca Azure AD PowerShell modülüyle nasıl kurulum alabildiğinizi de söyler.

> [!Note]
> Resmi PowerShell'imiz yalnızca Azure AD Ayrıcalıklı Kimlik Yönetimi'nin yeni sürümündeyseniz desteklenir. Lütfen Ayrıcalıklı Kimlik Yönetimi'ne gidin ve hızlı başlangıç bıçağında aşağıdaki banner'ın olduğundan emin olun.
> [![sahip olduğunuz Ayrıcalıklı Kimlik Yönetimi sürümünü kontrol edin](media/pim-how-to-add-role-to-user/pim-new-version.png "Azure AD > Ayrıcalıklı Kimlik Yönetimi'ni seçin")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox) Bu afiş yoksa, şu anda önümüzdeki birkaç hafta içinde bu güncelleştirilmiş deneyim yayılıyor sürecinde olduğu gibi bekleyin.
> Ayrıcalıklı Kimlik Yönetimi PowerShell cmdlets Azure AD Önizleme modülü üzerinden desteklenir. Farklı bir modül kullanıyorsanız ve bu modül artık bir hata iletisi döndürürse, lütfen bu yeni modülü kullanmaya başlayın. Farklı bir modülün üzerine inşa edilmiş herhangi bir üretim sistemi varsa, lütfenpim_preview@microsoft.com

## <a name="installation-and-setup"></a>Yükleme ve Kurulum

1. Azure AD Önizleme modüllerini yükleme

        Install-module AzureADPreview

1. Devam etmeden önce gerekli rol izinlerine sahip olduğundan emin olun. Rol ataması verme veya rol ayarı güncelleme gibi yönetim görevlerini gerçekleştirmeye çalışıyorsanız, Global yönetici veya Ayrıcalıklı rol yöneticisi rolüne sahip olduğundan emin olun. Yalnızca kendi atamanızı etkinleştirmeye çalışıyorsanız, varsayılan kullanıcı izinlerinin ötesinde izin gerekmez.

1. Azure AD'ye bağlanın.

        $AzureAdCred = Get-Credential  
        Connect-AzureAD -Credential $AzureAdCred

1. **Azure Active Directory** > **Properties** > **Directory ID'ye**giderek kiracı kimliğinizi bulun. cmdlets bölümünde, resourceId'yi sağlamanız gerektiğinde bu kimliği kullanın.

    ![Azure AD kuruluşunun özelliklerinde kiracı kimliğini bulma](./media/powershell-for-azure-ad-roles/tenant-id-for-Azure-ad-org.png)

> [!Note]
> Aşağıdaki bölümler, çalışmaya ve çalıştırmanıza yardımcı olabilecek basit örneklerdir. Aşağıdaki cmdlets ile ilgili daha detaylı https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#privileged_role_managementdokümantasyon alabilirsiniz. Ancak, providerID parametresindeki "azureResources"ı "aadRoles" ile değiştirmeniz gerekir. Ayrıca, Azure REKLAM kuruluşunuz için kiracı kimliğini kaynak Kimliği parametresi olarak kullanmayı da unutmamalısınız.

## <a name="retrieving-role-definitions"></a>Rol tanımlarını alma

Azure REKLAM kuruluşunuzdaki (kiracı) tüm yerleşik ve özel Azure REKLAM rollerini almak için aşağıdaki cmdlet'i kullanın. Bu önemli adım, rol adı ve roleDefinitionId arasındaki eşleme verir. RoleDefinitionId belirli bir role başvurmak için bu cmdlets boyunca kullanılır.

RoleDefinitionId Azure REKLAM kuruluşunuza özgüdür ve rol yönetimi API'sı tarafından döndürülen roleDefinitionId'den farklıdır.

    Get-AzureADMSPrivilegedRoleDefinition -ProviderId aadRoles -ResourceId 926d99e7-117c-4a6a-8031-0cc481e9da26

Sonuç:

![Azure AD organizasyonu için tüm rolleri alın](./media/powershell-for-azure-ad-roles/get-all-roles-result.png)

## <a name="retrieving-role-assignments"></a>Rol atamaları alma

Azure REKLAM kuruluşunuzdaki tüm rol atamalarını almak için aşağıdaki cmdlet'i kullanın.

    Get-AzureADMSPrivilegedRoleAssignment -ProviderId "aadRoles" -ResourceId "926d99e7-117c-4a6a-8031-0cc481e9da26"

Belirli bir kullanıcı için tüm rol atamaları almak için aşağıdaki cmdlet kullanın. Bu liste, Azure AD portalında "Rollerim" olarak da bilinir. Buradaki tek fark, konu kimliği için bir filtre eklemenizdir. Bu bağlamda ki konu kimliği kullanıcı kimliği veya grup kimliğidir.

    Get-AzureADMSPrivilegedRoleAssignment -ProviderId "aadRoles" -ResourceId "926d99e7-117c-4a6a-8031-0cc481e9da26" -Filter "subjectId eq 'f7d1887c-7777-4ba3-ba3d-974488524a9d'" 

Belirli bir rol için tüm rol atamaları almak için aşağıdaki cmdlet kullanın. RoleDefinitionId burada önceki cmdlet tarafından döndürülen kimliktir.

    Get-AzureADMSPrivilegedRoleAssignment -ProviderId "aadRoles" -ResourceId "926d99e7-117c-4a6a-8031-0cc481e9da26" -Filter "roleDefinitionId eq '0bb54a22-a3df-4592-9dc7-9e1418f0f61c'"

Cmdlets aşağıda gösterilen rol atama nesnelerin bir listesini sonuçlanır. Özne kimliği, rolün atandığı kullanıcının kullanıcı kimliğidir. Atama durumu etkin veya uygun olabilir. Kullanıcı etkinse ve LinkedEligibleRoleAssignmentId alanında bir kimlik varsa, bu rolün şu anda etkinleştirildiğini anlamına gelir.

Sonuç:

![Azure AD organizasyonu için tüm rol atamalarını alma](./media/powershell-for-azure-ad-roles/get-all-role-assignments-result.png)

## <a name="assign-a-role"></a>Rol atama

Uygun bir atama oluşturmak için aşağıdaki cmdlet'i kullanın.

    Open-AzureADMSPrivilegedRoleAssignmentRequest -ProviderId 'aadRoles' -ResourceId '926d99e7-117c-4a6a-8031-0cc481e9da26' -RoleDefinitionId 'ff690580-d1c6-42b1-8272-c029ded94dec' -SubjectId 'f7d1887c-7777-4ba3-ba3d-974488524a9d' -Type 'adminAdd' -AssignmentState 'Eligible' -schedule $schedule -reason "dsasdsas" 

Atamanın başlangıç ve bitiş saatini tanımlayan zamanlama, aşağıdaki örnek gibi oluşturulabilecek bir nesnedir:

    $schedule = New-Object Microsoft.Open.MSGraph.Model.AzureADMSPrivilegedSchedule
    $schedule.Type = "Once"
    $schedule.StartDateTime = (Get-Date).ToUniversalTime().ToString("yyyy-MM-ddTHH:mm:ss.fffZ")
    $schedule.endDateTime = "2020-07-25T20:49:11.770Z"
> [!Note]
> EndDateTime değeri null olarak ayarlanmışsa, kalıcı bir atama gösterir.

## <a name="activate-a-role-assignment"></a>Rol atamayı etkinleştirme

Uygun bir atamayı etkinleştirmek için aşağıdaki cmdlet'i kullanın.

    Open-AzureADMSPrivilegedRoleAssignmentRequest -ProviderId 'aadRoles' -ResourceId '926d99e7-117c-4a6a-8031-0cc481e9da26' -RoleDefinitionId 'f55a9a68-f424-41b7-8bee-cee6a442d418' -SubjectId 'f7d1887c-7777-4ba3-ba3d-974488524a9d' -Type 'UserAdd' -AssignmentState 'Active' -schedule $schedule -reason "dsasdsas" 

Bu cmdlet bir rol atama oluşturmak için cmdlet hemen hemen aynıdır. Cmdlets arasındaki temel fark , -Tip parametresi için aktivasyon "adminAdd" yerine "userAdd" olmasıdır. Diğer fark ise -AssignmentState parametresi "Uygun" yerine "Etkin" olmasıdır.

> [!Note]
> PowerShell aracılığıyla rol etkinleştirme için iki sınırlayıcı senaryo vardır.
> 1. Eğer rol ayarı bilet sistemi / bilet numarası gerekiyorsa, bir parametre olarak bu tedarik etmek için bir yol yoktur. Bu nedenle, rolü Azure portalının ötesinde etkinleştirmek mümkün olmaz. Bu özellik önümüzdeki birkaç ay içinde PowerShell'e sunuluyor.
> 1. Rol etkinleştirme için çok faktörlü kimlik doğrulamasına ihtiyaç duyuyorsanız, Şu anda PowerShell'in rolünü etkinleştirdiğinde kullanıcıya meydan okumasının bir yolu yoktur. Bunun yerine, kullanıcıların Mühendislerimizden birinin [bu blog gönderisini](http://www.anujchaudhary.com/2020/02/connect-to-azure-ad-powershell-with-mfa.html) izleyerek Azure AD'ye bağlandıklarında MFA meydan okumasını tetiklemeleri gerekir. PIM için bir uygulama geliştiriyorsanız, olası bir uygulama kullanıcılara meydan okumak ve bir "MfaRule" hatası aldıktan sonra onları modüle yeniden bağlamaktır.

## <a name="retrieving-and-updating-role-settings"></a>Rol ayarlarını alma ve güncelleştirme

Azure REKLAM kuruluşunuzdaki tüm rol ayarlarını almak için aşağıdaki cmdlet'i kullanın.

    Get-AzureADMSPrivilegedRoleSetting -ProviderId 'aadRoles' -Filter "ResourceId eq '926d99e7-117c-4a6a-8031-0cc481e9da26'" 

Ayarda dört ana nesne vardır. Bu nesnelerden yalnızca üçü şu anda PIM tarafından kullanılmaktadır. UserMemberSettings etkinleştirme ayarları, AdminUygun Ayarlar uygun atamalar için atama ayarları ve YöneticiÜye Ayarları etkin atamalar için atama ayarlarıdır.

[![](media/powershell-for-azure-ad-roles/get-update-role-settings-result.png "Get and update role settings")](media/powershell-for-azure-ad-roles/get-update-role-settings-result.png#lightbox)

Rol ayarını güncelleştirmek için önce bir ayar nesnesini aşağıdaki gibi tanımlamanız gerekir:

    $setting = New-Object Microsoft.Open.MSGraph.Model.AzureADMSPrivilegedRuleSetting 
    $setting.RuleIdentifier = "JustificationRule"
    $setting.Setting = "{'required':false}"

Daha sonra devam edebilir ve aşağıda gösterildiği gibi belirli bir rol için nesnelerden birine ayarı uygulayabilirsiniz. Buradaki kimlik, liste rol ayarları cmdlet sonucu alınabilen rol belirleme kimliğidir.

    Set-AzureADMSPrivilegedRoleSetting -ProviderId 'aadRoles' -Id 'ff518d09-47f5-45a9-bb32-71916d9aeadf' -ResourceId '3f5887ed-dd6e-4821-8bde-c813ec508cf9' -RoleDefinitionId '2387ced3-4e95-4c36-a915-73d803f93702' -UserMemberSettings $setting 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD özel rolü atama](azure-ad-custom-roles-assign.md)
- [Azure AD özel rol ataması kaldırma veya güncelleştirme](azure-ad-custom-roles-update-remove.md)
- [Azure AD özel rol ataması yapılandırma](azure-ad-custom-roles-configure.md)
- [Azure AD'de rol tanımları](../users-groups-roles/directory-assign-admin-roles.md)
