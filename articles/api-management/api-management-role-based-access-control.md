---
title: Azure API Yönetiminde Rol Tabanlı Erişim Denetimi nasıl kullanılır| Microsoft Dokümanlar
description: Azure API Yönetimi'nde yerleşik rolleri nasıl kullanacağınızı ve özel roller oluşturmayı öğrenin
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: ed0cd51fc686735f2d9c110ce46d5904107cafc2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75430608"
---
# <a name="how-to-use-role-based-access-control-in-azure-api-management"></a>Azure API Management'te Rol Tabanlı Erişim Denetimini kullanma

Azure API Yönetimi, API Yönetimi hizmetleri ve varlıkları (örneğin, API'ler ve ilkeler) için ince ayarlı erişim yönetimini etkinleştirmek için Azure Role Tabanlı Erişim Denetimi'ne (RBAC) güvenir. Bu makalede, API Yönetimi'nde yerleşik ve özel rollere genel bir bakış sağlar. Azure portalında erişim yönetimi hakkında daha fazla bilgi için [bkz.](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="built-in-roles"></a>Yerleşik roller

API Yönetimi şu anda üç yerleşik rol sağlar ve yakın gelecekte iki rol daha ekler. Bu roller abonelik, kaynak grubu ve tek tek API Yönetimi örneği dahil olmak üzere farklı kapsamlarda atanabilir. Örneğin, kaynak grubu düzeyindeki bir kullanıcıya "API Yönetim Hizmeti Okuyucusu" rolünü atarsanız, kullanıcı kaynak grubu içindeki tüm API Yönetimi örneklerine erişimi okumuştur. 

Aşağıdaki tablo, yerleşik rollerin kısa açıklamalarını sağlar. Bu rolleri Azure portalını veya Azure [PowerShell,](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell) [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli)ve REST [API](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest)gibi diğer araçları kullanarak atayabilirsiniz. Yerleşik rollerin nasıl atayılabildiğini hakkında ayrıntılı bilgi için azure [abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullan'a](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)bakın.

| Rol          | Erişimi oku<sup>[1]</sup> | Erişim yazma<sup>[2]</sup> | Hizmet oluşturma, silme, ölçekleme, VPN ve özel etki alanı yapılandırması | Eski yayımcı portalına erişim | Açıklama
| ------------- | ---- | ---- | ---- | ---- | ---- 
| API Yönetim Hizmeti Katılımcısı | ✓ | ✓ | ✓ | ✓ | Süper kullanıcı. API Yönetimi hizmetlerine ve varlıklarına (örneğin, API'ler ve ilkeler) tam CRUD erişimivardır. Eski yayımcı portalına erişimi vardır. |
| API Yönetim Hizmet Okuyucu | ✓ | | || API Yönetimi hizmetlerine ve kuruluşlarına salt okunur erişime sahiptir. |
| API Yönetim Servis Operatörü | ✓ | | ✓ | | API Yönetimi hizmetlerini yönetebilir, ancak varlıkları yönetemez.|
| API Yönetim Hizmet Editörü<sup>*</sup> | ✓ | ✓ | |  | API Yönetimi varlıklarını yönetebilir, ancak hizmetleri yönetemez.|
| API Yönetimi İçerik Yöneticisi<sup>*</sup> | ✓ | | | ✓ | Geliştirici portalını yönetebilirsiniz. Yalnızca okunabilir hizmetlere ve varlıklara erişim.|

<sup>[1] API Yönetimi hizmetlerine ve varlıklarına (örneğin, API'ler ve ilkeler) erişimi okuyun.</sup>

<sup>[2] Aşağıdaki işlemler dışında API Yönetimi hizmetlerine ve varlıklarına erişim yazın: örnek oluşturma, silme ve ölçekleme; VPN yapılandırması; ve özel etki alanı kurulumu.</sup>

<sup>\*Hizmet Düzenleyicisi rolü, tüm yönetici UI'yi mevcut yayımcı portalından Azure portalına geçtikten sonra kullanılabilir olacaktır. İçerik Yöneticisi rolü, yayımcı portalı yalnızca geliştirici portalının yönetimiyle ilgili işlevselliği içerecek şekilde yeniden düzenlemeden sonra kullanılabilir.</sup>  

## <a name="custom-roles"></a>Özel roller

Yerleşik rollerin hiçbiri özel gereksinimlerinizi karşılamazsa, API Yönetimi varlıkları için daha ayrıntılı erişim yönetimi sağlamak için özel roller oluşturulabilir. Örneğin, bir API Yönetimi hizmetine salt okunur erişimi olan, ancak yalnızca belirli bir API'ye yazma erişimi olan özel bir rol oluşturabilirsiniz. Özel roller hakkında daha fazla bilgi edinmek için [Azure RBAC'daki Özel rollere](https://docs.microsoft.com/azure/role-based-access-control/custom-roles)bakın. 

> [!NOTE]
> Azure portalında bir API Yönetimi örneğini görebilmek için özel ```Microsoft.ApiManagement/service/read``` bir rolün eylemi içermesi gerekir.

Özel bir rol oluşturduğunuzda, yerleşik rollerden biriyle başlamak daha kolaydır. **Eylemler,** **NotActions**veya **AtamaSkoplar**eklemek için özniteliklerini ve ardından değişiklikleri yeni bir rol olarak kaydedin. Aşağıdaki örnek "API Yönetim Hizmeti Okuyucu" rolüyle başlar ve "Hesap Makinesi API Düzenleyicisi" adlı özel bir rol oluşturur. Özel rolü belirli bir API'ye atayabilirsiniz. Sonuç olarak, bu rolün yalnızca bu API'ye erişimi vardır. 

```powershell
$role = Get-AzRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access to Contoso APIM instance and write access to the Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.Actions.Add('Microsoft.ApiManagement/service/apis/*/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzRoleDefinition -Role $role
New-AzRoleAssignment -ObjectId <object ID of the user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

[Azure Kaynak Yöneticisi kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftapimanagement) makalesi, API Yönetimi düzeyinde verilebilen izinlerin listesini içerir.

## <a name="video"></a>Video


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
>
>

## <a name="next-steps"></a>Sonraki adımlar

Azure'da Role Tabanlı Erişim Denetimi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  * [Azure portalında erişim yönetimi ile çalışmaya başlama](../role-based-access-control/overview.md)
  * [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](../role-based-access-control/role-assignments-portal.md)
  * [Azure RBAC'da özel roller](../role-based-access-control/custom-roles.md)
  * [Azure Kaynak Yöneticisi kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftapimanagement)
