---
title: PıM-Azure AD 'de Azure AD rolleri için PowerShell | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içindeki PowerShell cmdlet 'lerini kullanarak Azure AD rollerini yönetin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 284e05e372ef18877f02d11525fd70b0ecf977b1
ms.sourcegitcommit: 24f31287b6a526e23ff5b5469113522d1ccd4467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84743652"
---
# <a name="powershell-for-azure-ad-roles-in-privileged-identity-management"></a>Privileged Identity Management 'de Azure AD rolleri için PowerShell

Bu makale, Azure AD rollerini Privileged Identity Management (PıM) içinde yönetmek için Azure Active Directory (Azure AD) PowerShell cmdlet 'lerini kullanmaya yönelik yönergeler içerir. Ayrıca, Azure AD PowerShell modülü ile nasıl ayarlanalınacağını da söyler.

> [!Note]
> Resmi PowerShell, yalnızca yeni Azure AD Privileged Identity Management sürümlerimizde desteklenir. Lütfen Privileged Identity Management gidin ve hızlı başlangıç dikey penceresinde aşağıdaki başlık bulunduğundan emin olun.
> [![sahip olduğunuz Privileged Identity Management sürümünü denetleyin](media/pim-how-to-add-role-to-user/pim-new-version.png "Azure AD > seçin Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox) Bu başlığa sahip değilseniz, şu anda bir sonraki birkaç hafta içinde bu güncelleştirilmiş deneyimi verme sürecinde olduğundan lütfen bekleyin.
> Privileged Identity Management PowerShell cmdlet 'leri Azure AD önizleme modülü aracılığıyla desteklenir. Farklı bir modül kullanıyorsanız ve bu modül şimdi bir hata mesajı döndürüyorsa, lütfen bu yeni modülü kullanmaya başlayın. Farklı bir modülün üzerine inşa ettiğiniz üretim sistemleriniz varsa, lütfen bu duruma ulaşınpim_preview@microsoft.com

## <a name="installation-and-setup"></a>Yükleme ve Kurulum

1. Azure AD önizleme modülünü yükler

        Install-module AzureADPreview

1. Devam etmeden önce gerekli rol izinlerine sahip olduğunuzdan emin olun. Rol ataması verme veya rol ayarı güncelleştirme gibi yönetim görevleri gerçekleştirmeye çalışıyorsanız, genel yönetici veya ayrıcalıklı rol yöneticisi rolüne sahip olduğunuzdan emin olun. Yalnızca kendi atamasını etkinleştirmeye çalışıyorsanız, varsayılan Kullanıcı izinlerinin ötesinde izin istenmez.

1. Azure AD 'ye bağlanın.

        $AzureAdCred = Get-Credential  
        Connect-AzureAD -Credential $AzureAdCred

1. **Azure Active Directory**  >  **Özellikler**  >  **dizin kimliği**' ne giderek Azure AD kuruluşunuzun Kiracı kimliğini bulun. Cmdlet 'ler bölümünde, RESOURCEID sağlamanız gerektiğinde bu KIMLIĞI kullanın.

    ![Azure AD kuruluşunun özelliklerinde kuruluş KIMLIĞINI bulma](./media/powershell-for-azure-ad-roles/tenant-id-for-Azure-ad-org.png)

> [!Note]
> Aşağıdaki bölümler, çalışmaya başlamanıza yardımcı olabilecek basit örneklerdir. Aşağıdaki cmdlet 'lerle ilgili daha ayrıntılı belgeler bulabilirsiniz https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#privileged_role_management . Ancak, ProviderID parametresindeki "azureResources" öğesini "aadRoles" ile değiştirmeniz gerekir. Ayrıca RESOURCEID parametresi olarak Azure AD kuruluşunuz için kuruluş KIMLIĞINI de kullanmayı unutmayın.

## <a name="retrieving-role-definitions"></a>Rol tanımları alınıyor

Azure AD kuruluşunuzda tüm yerleşik ve özel Azure AD rollerini almak için aşağıdaki cmdlet 'i kullanın. Bu önemli adım, rol adı ve Roledefinitionıd arasındaki eşlemeyi sağlar. Roledefinitionıd, belirli bir role başvurmak için bu cmdlet 'ler genelinde kullanılır.

Roledefinitionıd, Azure AD kuruluşunuza özgüdür ve rol yönetim API 'SI tarafından döndürülen Roledefinitionıd 'den farklıdır.

    Get-AzureADMSPrivilegedRoleDefinition -ProviderId aadRoles -ResourceId 926d99e7-117c-4a6a-8031-0cc481e9da26

Sonuç:

![Azure AD kuruluşu için tüm rolleri edinme](./media/powershell-for-azure-ad-roles/get-all-roles-result.png)

## <a name="retrieving-role-assignments"></a>Rol atamaları alınıyor

Azure AD kuruluşunuzda tüm rol atamalarını almak için aşağıdaki cmdlet 'i kullanın.

    Get-AzureADMSPrivilegedRoleAssignment -ProviderId "aadRoles" -ResourceId "926d99e7-117c-4a6a-8031-0cc481e9da26"

Belirli bir kullanıcı için tüm rol atamalarını almak üzere aşağıdaki cmdlet 'i kullanın. Bu liste, Azure AD portalındaki "rollerim" olarak da bilinir. Buradaki tek fark, konu KIMLIĞI için bir filtre ekledik. Bu bağlamdaki konu KIMLIĞI, Kullanıcı KIMLIĞI veya grup KIMLIĞIDIR.

    Get-AzureADMSPrivilegedRoleAssignment -ProviderId "aadRoles" -ResourceId "926d99e7-117c-4a6a-8031-0cc481e9da26" -Filter "subjectId eq 'f7d1887c-7777-4ba3-ba3d-974488524a9d'" 

Belirli bir rolün tüm rol atamalarını almak için aşağıdaki cmdlet 'i kullanın. Burada Roledefinitionıd, önceki cmdlet tarafından döndürülen KIMLIĞIDIR.

    Get-AzureADMSPrivilegedRoleAssignment -ProviderId "aadRoles" -ResourceId "926d99e7-117c-4a6a-8031-0cc481e9da26" -Filter "roleDefinitionId eq '0bb54a22-a3df-4592-9dc7-9e1418f0f61c'"

Cmdlet 'ler aşağıda gösterilen rol atama nesnelerinin bir listesi ile sonuçlanır. Konu KIMLIĞI, rolün atandığı kullanıcının Kullanıcı KIMLIĞIDIR. Atama durumu etkin ya da uygun olabilir. Kullanıcı etkinse ve LinkedEligibleRoleAssignmentId alanında bir KIMLIK varsa bu, rolün etkin olduğu anlamına gelir.

Sonuç:

![Azure AD kuruluşu için tüm rol atamalarını alma](./media/powershell-for-azure-ad-roles/get-all-role-assignments-result.png)

## <a name="assign-a-role"></a>Rol atama

Uygun bir atama oluşturmak için aşağıdaki cmdlet 'i kullanın.

    Open-AzureADMSPrivilegedRoleAssignmentRequest -ProviderId 'aadRoles' -ResourceId '926d99e7-117c-4a6a-8031-0cc481e9da26' -RoleDefinitionId 'ff690580-d1c6-42b1-8272-c029ded94dec' -SubjectId 'f7d1887c-7777-4ba3-ba3d-974488524a9d' -Type 'adminAdd' -AssignmentState 'Eligible' -schedule $schedule -reason "dsasdsas" 

Atamanın başlangıç ve bitiş saatini tanımlayan zamanlama, aşağıdaki örnekte olduğu gibi oluşturulabilen bir nesnedir:

    $schedule = New-Object Microsoft.Open.MSGraph.Model.AzureADMSPrivilegedSchedule
    $schedule.Type = "Once"
    $schedule.StartDateTime = (Get-Date).ToUniversalTime().ToString("yyyy-MM-ddTHH:mm:ss.fffZ")
    $schedule.endDateTime = "2020-07-25T20:49:11.770Z"
> [!Note]
> EndDateTime değeri null olarak ayarlandıysa, kalıcı bir atamayı gösterir.

## <a name="activate-a-role-assignment"></a>Rol atamasını etkinleştirme

Uygun bir atamayı etkinleştirmek için aşağıdaki cmdlet 'i kullanın.

    Open-AzureADMSPrivilegedRoleAssignmentRequest -ProviderId 'aadRoles' -ResourceId '926d99e7-117c-4a6a-8031-0cc481e9da26' -RoleDefinitionId 'f55a9a68-f424-41b7-8bee-cee6a442d418' -SubjectId 'f7d1887c-7777-4ba3-ba3d-974488524a9d' -Type 'UserAdd' -AssignmentState 'Active' -schedule $schedule -reason "dsasdsas" 

Bu cmdlet, rol ataması oluşturmaya yönelik cmdlet ile neredeyse aynıdır. Cmdlet 'ler arasındaki temel fark, – Type parametresi için etkinleştirmenin "adminAdd" yerine "userAdd" olması gerektiğidir. Diğer fark, – atamadurumu parametresinin "uygun" yerine "etkin" olması gerektiğidir.

> [!Note]
> PowerShell aracılığıyla rol etkinleştirmeye yönelik iki sınırlandırma senaryosu vardır.
> 1. Rol ayarınız içinde bilet sistemi/bilet numarası gerekliyse, bu parametreleri parametre olarak sağlamanın bir yolu yoktur. Bu nedenle, rolü Azure portal ötesinde etkinleştirmek mümkün değildir. Bu özellik, sonraki birkaç ay içinde PowerShell 'e kullanıma sunulacaktır.
> 1. Rol etkinleştirme için çok faktörlü kimlik doğrulamasına ihtiyacınız varsa, bu durumda PowerShell 'in rolünü etkinleştirdiklerinde kullanıcıyı sınama yolu yoktur. Bunun yerine, mühendislerimizden birinden [Bu blog gönderisini](http://www.anujchaudhary.com/2020/02/connect-to-azure-ad-powershell-with-mfa.html) Izleyerek KULLANıCıLARıN Azure AD 'ye bağlandıklarında MFA sınamasını tetiklemesi gerekecektir. PıM için bir uygulama geliştiriyorsanız, olası bir uygulama, kullanıcıları güçlük ve "MfaRule" hatası aldıktan sonra modüle yeniden bağlamayı sağlar.

## <a name="retrieving-and-updating-role-settings"></a>Rol ayarları alınıyor ve güncelleştiriliyor

Azure AD kuruluşunuzda tüm rol ayarlarını almak için aşağıdaki cmdlet 'i kullanın.

    Get-AzureADMSPrivilegedRoleSetting -ProviderId 'aadRoles' -Filter "ResourceId eq '926d99e7-117c-4a6a-8031-0cc481e9da26'" 

Ayarında dört ana nesne vardır. Şu anda bu nesnelerden yalnızca üçü PıM tarafından kullanılmaktadır. UserMemberSettings etkinleştirme ayarlarından, AdminEligibleSettings uygun atamaların atama ayarları, AdminmemberSettings ise etkin atamaların atama ayarlardır.

[![](media/powershell-for-azure-ad-roles/get-update-role-settings-result.png "Get and update role settings")](media/powershell-for-azure-ad-roles/get-update-role-settings-result.png#lightbox)

Rol ayarını güncelleştirmek için, belirli bir rol için var olan ayar nesnesini almanız ve üzerinde değişiklikler yapmanız gerekir:

    $setting = Get-AzureADMSPrivilegedRoleSetting -ProviderId 'aadRoles' -Filter "roleDefinitionId eq 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'"
    $setting.UserMemberSetting.justificationRule = '{"required":false}'

Daha sonra devam edebilir ve ayarı aşağıda gösterildiği gibi belirli bir rol için nesnelerden birine uygulayabilirsiniz. Buradaki KIMLIK, rol ayarları cmdlet 'inin sonuçlarından alınabilecek rol ayarı KIMLIĞIDIR.

    Set-AzureADMSPrivilegedRoleSetting -ProviderId 'aadRoles' -Id 'ff518d09-47f5-45a9-bb32-71916d9aeadf' -ResourceId '3f5887ed-dd6e-4821-8bde-c813ec508cf9' -RoleDefinitionId '2387ced3-4e95-4c36-a915-73d803f93702' -UserMemberSettings $setting 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD özel rolü atama](azure-ad-custom-roles-assign.md)
- [Azure AD özel rol atamasını kaldırma veya güncelleştirme](azure-ad-custom-roles-update-remove.md)
- [Azure AD özel rol ataması yapılandırma](azure-ad-custom-roles-configure.md)
- [Azure AD 'de rol tanımları](../users-groups-roles/directory-assign-admin-roles.md)
