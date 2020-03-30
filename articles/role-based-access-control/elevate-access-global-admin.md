---
title: Tüm Azure aboneliklerini ve yönetim gruplarını yönetmek için erişimi yükseltme
description: Azure portalı veya REST API'yi kullanarak Azure Etkin Dizin'deki tüm abonelikleri ve yönetim gruplarını yönetmek için Global Administrator'a erişimi nasıl yükselteceğimiz açıklanır.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: bagovind
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/03/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 1731c630cb98ac542ebcdc7aedf07f7bb63eaec0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77137453"
---
# <a name="elevate-access-to-manage-all-azure-subscriptions-and-management-groups"></a>Tüm Azure aboneliklerini ve yönetim Gruplarını yönetmeye erişimi artırın

Azure Etkin Dizini'nde (Azure AD) Genel Yönetici olarak, dizininizdeki tüm aboneliklere ve yönetim gruplarına erişiminiz olmayabilir. Bu makalede, tüm aboneliklere ve yönetim gruplarına erişiminizi yükseltme yolları açıklanmaktadır.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="why-would-you-need-to-elevate-your-access"></a>Neden erişiminizi yükseltmeniz gerekiyor?

Genel Yöneticiyseniz, aşağıdakileri yapmak istediğiniz zamanlar olabilir:

- Bir kullanıcı erişimini kaybettiğinde Azure aboneliğine veya yönetim grubuna yeniden erişin
- Başka bir kullanıcıya veya kendisine Azure aboneliği veya yönetim grubu üzerinde erişim verme
- Kuruluştaki tüm Azure aboneliklerini veya yönetim gruplarını görün
- Bir otomasyon uygulamasının (faturalama veya denetleme uygulaması gibi) tüm Azure aboneliklerine veya yönetim gruplarına erişmesine izin ver

## <a name="how-does-elevated-access-work"></a>Yükseltilmiş erişim nasıl çalışır?

Azure AD ve Azure kaynakları birbirinden bağımsız olarak güvenlidir. Diğer bir deyişle, Azure REKLAM rol atamaları Azure kaynaklarına erişim vermez ve Azure rol atamaları Azure AD'ye erişim vermez. Ancak, Azure AD'de [Global Administrator](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator-permissions) iseniz, dizininizdeki tüm Azure aboneliklerine ve yönetim gruplarına erişim atayabilirsiniz. Sanal makineler veya depolama hesapları gibi Azure abonelik kaynaklarına erişiminiz yoksa ve bu kaynaklara erişmek için Global Administrator ayrıcalığınızı kullanmak istiyorsanız bu özelliği kullanın.

Erişiminizi yükselttiğinde, kök kapsamda Azure'da [Kullanıcı Erişim Yöneticisi](built-in-roles.md#user-access-administrator) rolü`/`atanır ( ).Bu, tüm kaynakları görüntülemenize ve dizindeki herhangi bir abonelik veya yönetim grubuna erişim atamanıza olanak tanır. Kullanıcı Erişim Yöneticisi rol atamaları Azure PowerShell, Azure CLI veya REST API kullanılarak kaldırılabilir.

Kök kapsamda yapmanız gereken değişiklikleri yaptıktan sonra bu yükseltilmiş erişimi kaldırmanız gerekir.

![Erişimi yükseltme](./media/elevate-access-global-admin/elevate-access.png)

## <a name="azure-portal"></a>Azure portalında

### <a name="elevate-access-for-a-global-administrator"></a>Global Yönetici için erişimi yükseltme

Azure portalını kullanarak Bir Global Yönetici için erişimi yükseltmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com) veya Azure [Etkin Dizin yöneticisi merkezinde](https://aad.portal.azure.com) Küresel Yönetici olarak oturum açın.

1. **Azure Active Directory**'yi bulun ve seçin.

   ![Azure Active Directory'yi seçin - ekran görüntüsü](./media/elevate-access-global-admin/search-for-azure-active-directory.png)

1. **Yönet**altında, **Özellikleri**seçin.

   ![Azure Etkin Dizin özellikleri için Özellikler'i seçin - ekran görüntüsü](./media/elevate-access-global-admin/azure-active-directory-properties.png)

1. **Azure kaynakları için Access yönetimi**altında, geçişi **Evet**olarak ayarlayın.

   ![Azure kaynakları için erişim yönetimi - ekran görüntüsü](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Geçişi **Evet**olarak ayarladığınızda, Azure RBAC'da kullanıcı erişim yöneticisi rolünü kök kapsamda (/) atanırsınız. Bu, bu Azure AD diziniyle ilişkili tüm Azure aboneliklerinde ve yönetim gruplarında rol atama izni verir. Bu geçiş yalnızca Azure AD'de Genel Yönetici rolü atanan kullanıcılar tarafından kullanılabilir.

   Geçişi **Hayır**olarak ayarladığınızda, Azure RBAC'daki Kullanıcı Erişim Yöneticisi rolü kullanıcı hesabınızdan kaldırılır. Bu Azure AD diziniyle ilişkili tüm Azure aboneliklerinde ve yönetim gruplarında artık rol atayamaz. Yalnızca size erişim izni verilen Azure aboneliklerini ve yönetim gruplarını görüntüleyebilir ve yönetebilirsiniz.

    > [!NOTE]
    > [Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM)](../active-directory/privileged-identity-management/pim-configure.md)kullanıyorsanız, rol atamanızı devre dışı kakmak bu geçişi **Hayır**olarak değiştirmez. En az ayrıcalıklı erişimi korumak için, rol atamanızı devre dışı bırakmadan önce bu geçişini **Hayır** olarak ayarlamanızı öneririz.
    
1. Ayarınızı kaydetmek için **Kaydet'i** tıklatın.

   Bu ayar genel bir özellik değildir ve yalnızca şu anda oturum açmış olan kullanıcı için geçerlidir. Global Administrator rolünün tüm üyeleri için erişimi yükseltemezsiniz.

1. Erişiminizi yenilemek için oturum unuzu tamamlayın ve tekrar oturum açın.

    Artık dizininizdeki tüm aboneliklere ve yönetim gruplarına erişebilmeniz gerekir. Access denetimi (IAM) bölmesini görüntülediğinizde, kök kapsamda Kullanıcı Erişim Yöneticisi rolüne atandığınızı fark esiniz.

   ![Kök kapsamı ile abonelik rol atamaları - ekran görüntüsü](./media/elevate-access-global-admin/iam-root.png)

1. Yükseltilmiş erişimde yapmanız gereken değişiklikleri yapın.

    Rolleratama hakkında daha fazla bilgi için [RBAC ve Azure portalını kullanarak erişimi yönet'e](role-assignments-portal.md)bakın. Azure AD Ayrıcalıklı Kimlik Yönetimi 'ni (PIM) kullanıyorsanız, [PIM'de yönetmek için Azure kaynaklarını keşfedin](../active-directory/privileged-identity-management/pim-resource-roles-discover-resources.md) veya [PIM'de Azure kaynak rollerini atayın'a](../active-directory/privileged-identity-management/pim-resource-roles-assign-roles.md)bakın.

### <a name="remove-elevated-access"></a>Yükseltilmiş erişimi kaldırma

Root kapsamında Ki Kullanıcı Erişim Yöneticisi`/`rol atamasını kaldırmak için aşağıdaki adımları izleyin.

1. Erişimi yükseltmek için kullanılan kullanıcı olarak oturum açın.

1. Gezinti listesinde Azure **Etkin Dizin'i** tıklatın ve ardından **Özellikler'i**tıklatın.

1. Azure **kaynaklarıiçin Access yönetimini** **No'ya**geri ayarlamayı ayarlayın. Bu, kullanıcı başına bir ayar olduğundan, erişimi yükseltmek için kullanılan kullanıcıyla aynı kullanıcı olarak oturum açmış olmalısınız.

    Access denetimi (IAM) bölmesindeki Kullanıcı Erişim Yöneticisi rol atamasını kaldırmaya çalışırsanız, aşağıdaki iletiyi görürsünüz. Rol atamasını kaldırmak için, geçiş ini **Hayır** olarak ayarlamanız veya Azure PowerShell, Azure CLI veya REST API'sini kullanmanız gerekir.

    ![Kök kapsamı yla rol atamalarını kaldırma](./media/elevate-access-global-admin/iam-root-remove.png)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

### <a name="list-role-assignment-at-root-scope-"></a>Kök kapsamda ki rol atamayı listele (/)

Kök kapsamdaki bir kullanıcı için Kullanıcı Erişim`/`Yöneticisi rol atamasını listelemek için [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) komutunu kullanın.

```azurepowershell
Get-AzRoleAssignment | where {$_.RoleDefinitionName -eq "User Access Administrator" `
  -and $_.SignInName -eq "<username@example.com>" -and $_.Scope -eq "/"}
```

```Example
RoleAssignmentId   : /providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111
Scope              : /
DisplayName        : username
SignInName         : username@example.com
RoleDefinitionName : User Access Administrator
RoleDefinitionId   : 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9
ObjectId           : 22222222-2222-2222-2222-222222222222
ObjectType         : User
CanDelegate        : False
```

### <a name="remove-elevated-access"></a>Yükseltilmiş erişimi kaldırma

Kullanıcı Erişim Yöneticisi rol atamasını kendiniz veya kök`/`kapsamdaki başka bir kullanıcı için kaldırmak için aşağıdaki adımları izleyin.

1. Yüksek erişimi kaldırabilen bir kullanıcı olarak oturum açın. Bu, erişimi yükseltmek için kullanılan kullanıcı veya kök kapsamda yüksek erişime sahip başka bir Global Yönetici olabilir.

1. Kullanıcı Erişim Yöneticisi rol atamasını kaldırmak için [Kaldır-AzRoleAssignment](/powershell/module/az.resources/remove-azroleassignment) komutunu kullanın.

    ```azurepowershell
    Remove-AzRoleAssignment -SignInName <username@example.com> `
      -RoleDefinitionName "User Access Administrator" -Scope "/"
    ```

## <a name="azure-cli"></a>Azure CLI

### <a name="list-role-assignment-at-root-scope-"></a>Kök kapsamda ki rol atamayı listele (/)

Kök kapsamdaki bir kullanıcı için Kullanıcı Erişim`/`Yöneticisi rol atamasını listelemek için ( ), [az rol atama listesi](/cli/azure/role/assignment#az-role-assignment-list) komutunu kullanın.

```azurecli
az role assignment list --role "User Access Administrator" --scope "/"
```

```Example
[
  {
    "canDelegate": null,
    "id": "/providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111",
    "name": "11111111-1111-1111-1111-111111111111",
    "principalId": "22222222-2222-2222-2222-222222222222",
    "principalName": "username@example.com",
    "principalType": "User",
    "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "roleDefinitionName": "User Access Administrator",
    "scope": "/",
    "type": "Microsoft.Authorization/roleAssignments"
  }
]

```

### <a name="remove-elevated-access"></a>Yükseltilmiş erişimi kaldırma

Kullanıcı Erişim Yöneticisi rol atamasını kendiniz veya kök`/`kapsamdaki başka bir kullanıcı için kaldırmak için aşağıdaki adımları izleyin.

1. Yüksek erişimi kaldırabilen bir kullanıcı olarak oturum açın. Bu, erişimi yükseltmek için kullanılan kullanıcı veya kök kapsamda yüksek erişime sahip başka bir Global Yönetici olabilir.

1. Kullanıcı Erişim Yöneticisi rol atamasını kaldırmak için [az rol atamasilme](/cli/azure/role/assignment#az-role-assignment-delete) komutunu kullanın.

    ```azurecli
    az role assignment delete --assignee username@example.com --role "User Access Administrator" --scope "/"
    ```

## <a name="rest-api"></a>REST API

### <a name="elevate-access-for-a-global-administrator"></a>Global Yönetici için erişimi yükseltme

REST API'sini kullanarak Global Administrator'a erişimi yükseltmek için aşağıdaki temel adımları kullanın.

1. REST'i `elevateAccess`kullanarak, root kapsamında Kullanıcı Erişim Yöneticisi`/`rolünü size veren çağrıyı ( ).

   ```http
   POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
   ```

1. Herhangi bir kapsamda herhangi bir rol atamak için bir [rol ataması](/rest/api/authorization/roleassignments) oluşturun. Aşağıdaki örnekte kök kapsamda {roleDefinitionID} rolünü atamak`/`için özellikler gösterilmektedir ():

   ```json
   { 
     "properties": {
       "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionID}",
       "principalId": "{objectID}",
       "scope": "/"
     },
     "id": "providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111",
     "type": "Microsoft.Authorization/roleAssignments",
     "name": "11111111-1111-1111-1111-111111111111"
   }
   ```

1. Kullanıcı Erişim Yöneticisi iken, rol atamaları kök kapsamda`/`kaldırabilirsiniz ( ).

1. Kullanıcı Erişim Yöneticisi ayrıcalıklarınızı yeniden ihtiyaç duyulana kadar kaldırın.

### <a name="list-role-assignments-at-root-scope-"></a>Temel kapsamdaki rol atamalarını listele (/)

Bir kullanıcıiçin tüm rol atamalarını kök kapsamda`/`listeleyebilirsiniz ( ).

- [GET roleAssignments'ı](/rest/api/authorization/roleassignments/listforscope) arayın Rol atamaları almak istediğiniz kullanıcının nesne kimliği nerede. `{objectIdOfUser}`

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectIdOfUser}'
   ```

### <a name="list-deny-assignments-at-root-scope-"></a>Kök kapsamdaki atamaları listele (/)

Bir kullanıcı için reddedilen atamaların tümünü kök`/`kapsamda listeleyebilirsiniz ( ).

- Get denyAssignments'ı arayın, almak istediğiniz atamaları reddeden kullanıcının nesne kimliği nerededir. `{objectIdOfUser}`

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter=gdprExportPrincipalId+eq+'{objectIdOfUser}'
   ```

### <a name="remove-elevated-access"></a>Yükseltilmiş erişimi kaldırma

Aradiğinizde, `elevateAccess`kendiniz için bir rol ataması oluşturursunuz, bu nedenle kullanıcı erişim yöneticisi rol atamasını`/`kök kapsamda kendiniz için kaldırmanız gerekir ( )

1. Kullanıcı Erişim Yöneticisi `roleName` rolünün ad kimliğini belirlemek için Kullanıcı Erişim Yöneticisi'ne eşit olan [GET roleDefinitions'ı](/rest/api/authorization/roledefinitions/get) arayın.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter=roleName+eq+'User Access Administrator'
    ```

    ```json
    {
      "value": [
        {
          "properties": {
      "roleName": "User Access Administrator",
      "type": "BuiltInRole",
      "description": "Lets you manage user access to Azure resources.",
      "assignableScopes": [
        "/"
      ],
      "permissions": [
        {
          "actions": [
            "*/read",
            "Microsoft.Authorization/*",
            "Microsoft.Support/*"
          ],
          "notActions": []
        }
      ],
      "createdOn": "0001-01-01T08:00:00.0000000Z",
      "updatedOn": "2016-05-31T23:14:04.6964687Z",
      "createdBy": null,
      "updatedBy": null
          },
          "id": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
          "type": "Microsoft.Authorization/roleDefinitions",
          "name": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"
        }
      ],
      "nextLink": null
    }
    ```

    Bu durumda `18d7d88d-d35e-4fb5-a5c3-7773c20a72d9`kimliği `name` parametreden kaydedin.

1. Dizin kapsamındaki dizin yöneticisinin rol atamasını da listamanız gerekir. Yükseltme erişim çağrısını yapan dizin yöneticisinin `principalId` dizin kapsamındaki tüm atamaları listele. Bu, objectid için dizindeki tüm atamaları listeler.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
        
    >[!NOTE] 
    >Bir dizin yöneticisi nin çok fazla ataması olmamalıdır, önceki sorgu çok fazla atama döndürürse, dizin kapsamı düzeyindeki tüm atamalar için sorgu yapabilir ve sonuçları filtreleyebilirsiniz:`GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
            
1. Önceki aramalar, rol atamalarının listesini döndürer. Kapsam `"/"` ve `roleDefinitionId` adım 1'de bulduğunuz rol adı kimliği ile sona `principalId` erer ve dizin yöneticisinin objectId'i ile eşleşen rol atamasını bulun. 
    
    Örnek rol ataması:
    
    ```json
    {
      "value": [
        {
          "properties": {
            "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
            "principalId": "{objectID}",
            "scope": "/",
            "createdOn": "2016-08-17T19:21:16.3422480Z",
            "updatedOn": "2016-08-17T19:21:16.3422480Z",
            "createdBy": "22222222-2222-2222-2222-222222222222",
            "updatedBy": "22222222-2222-2222-2222-222222222222"
          },
          "id": "/providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111",
          "type": "Microsoft.Authorization/roleAssignments",
          "name": "11111111-1111-1111-1111-111111111111"
        }
      ],
      "nextLink": null
    }
    ```
    
    Yine, `name` bu durumda 11111111-1111-11111-111111111111111111111111111111111111111111111111111111111111111111111111111 11111111111111111111111111111111111111111 111111111111

1. Son olarak, aşağıdakiler tarafından `elevateAccess`eklenen atamayı kaldırmak için rol atama kimliğini kullanın:

    ```http
    DELETE https://management.azure.com/providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111?api-version=2015-07-01
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'daki farklı rolleri anlama](rbac-and-directory-admin-roles.md)
- [RBAC ve REST API'sini kullanarak Azure kaynaklarına erişimi yönetme](role-assignments-rest.md)
