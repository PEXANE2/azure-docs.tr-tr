---
title: Tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişimi yükselt | Microsoft Docs
description: Azure portal veya REST API kullanarak Azure Active Directory tüm abonelikleri ve Yönetim gruplarını yönetmek üzere genel yönetici için erişimin nasıl yükseltilceğini açıklar.
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
ms.date: 02/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 984284fa185d4d8454b1689a62ca9e08c342e33b
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195109"
---
# <a name="elevate-access-to-manage-all-azure-subscriptions-and-management-groups"></a>Tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişimi yükseltme

Azure Active Directory (Azure AD) içinde genel yönetici olarak, dizininizdeki tüm aboneliklere ve yönetim gruplarına erişiminiz olmayabilir. Bu makalede, erişiminizi tüm aboneliklere ve yönetim gruplarına yükseltebilmeniz gereken yollar açıklanmaktadır.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="why-would-you-need-to-elevate-your-access"></a>Erişiminizi neden yükseltmek gerekir?

Genel yöneticiyseniz, aşağıdakileri yapmak istediğiniz zamanlar olabilir:

- Bir kullanıcı erişimi kaybettiği zaman bir Azure aboneliğine veya yönetim grubuna yeniden erişim elde edin
- Başka bir kullanıcıya veya kendinize Azure aboneliğine veya yönetim grubuna erişim izni verme
- Bir kuruluştaki tüm Azure aboneliklerine veya yönetim gruplarına bakın
- Otomasyon uygulamasının (faturalama veya denetim uygulaması gibi) tüm Azure aboneliklerine veya yönetim gruplarına erişmesine izin ver

## <a name="how-does-elevate-access-work"></a>Erişim nasıl çalışır?

Azure AD ve Azure kaynakları birbirinden bağımsız olarak güvenli hale getirilir. Diğer bir deyişle, Azure AD rol atamaları Azure kaynaklarına erişim vermez ve Azure rol atamaları Azure AD 'ye erişim vermez. Ancak, Azure AD 'de [genel yöneticiyseniz](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator-permissions) , kendi dizininizdeki tüm Azure aboneliklerine ve yönetim gruplarına erişim izni atayabilirsiniz. Sanal makineler veya depolama hesapları gibi Azure abonelik kaynaklarına erişiminiz yoksa ve bu kaynaklara erişim kazanmak için genel yönetici ayrıcalıklarınızı kullanmak istiyorsanız bu özelliği kullanın.

Erişiminizi yükseltirse, Azure 'da kök kapsamında (`/`) [Kullanıcı erişimi yönetici](built-in-roles.md#user-access-administrator) rolü atamış olursunuz. Bu, tüm kaynakları görüntülemenize ve dizindeki herhangi bir aboneliğe veya yönetim grubuna erişim atamanıza olanak tanır. Kullanıcı erişimi yönetici rolü atamaları, PowerShell kullanılarak kaldırılabilir.

Kök kapsamda yapmanız gereken değişiklikleri yaptıktan sonra bu yükseltilmiş erişimi kaldırmalısınız.

![Erişimi yükselt](./media/elevate-access-global-admin/elevate-access.png)

## <a name="azure-portal"></a>Azure portal

Azure portal kullanarak genel yöneticiye erişimi yükseltmek için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com) veya [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com) genel yönetici olarak oturum açın.

1. Gezinti listesinde **Azure Active Directory** ' a ve ardından **Özellikler**' e tıklayın.

   ![Azure AD özellikleri-ekran görüntüsü](./media/elevate-access-global-admin/aad-properties.png)

1. **Azure kaynakları Için erişim yönetimi**altında, geçiş seçeneğini **Evet**olarak ayarlayın.

   ![Azure kaynakları için erişim yönetimi-ekran görüntüsü](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Geçiş seçeneğini **Evet**olarak belirlediğinizde, kök kapsamda (/) Azure RBAC 'de Kullanıcı erişimi Yöneticisi rolü atanır. Bu, tüm Azure aboneliklerine ve bu Azure AD diziniyle ilişkili yönetim gruplarına rol atama izni verir. Bu geçiş yalnızca Azure AD 'de Genel yönetici rolü atanmış olan kullanıcılar tarafından kullanılabilir.

   **Hayır**olarak ayarlandığında, Azure RBAC 'Deki Kullanıcı erişimi yönetici rolü kullanıcı hesabınızdan kaldırılır. Artık bu Azure AD diziniyle ilişkili tüm Azure aboneliklerinde ve yönetim gruplarında rol atayamazsınız. Yalnızca erişim izni verilen Azure aboneliklerini ve Yönetim gruplarını görüntüleyebilir ve yönetebilirsiniz.

1. Ayarınızı kaydetmek için **Kaydet** ' e tıklayın.

   Bu ayar genel bir özellik değildir ve yalnızca şu anda oturum açmış olan kullanıcı için geçerlidir. Genel yönetici rolünün tüm üyeleri için erişimi yükselmez.

1. Oturumunuzu yenilemek için oturumunuzu kapatın ve yeniden oturum açın.

    Artık dizininizdeki tüm aboneliklere ve yönetim gruplarına erişiminizin olması gerekir. Kök kapsamda Kullanıcı erişimi yönetici rolü atandığını fark edeceksiniz.

   ![Kök kapsamına sahip abonelik rolü atamaları-ekran görüntüsü](./media/elevate-access-global-admin/iam-root.png)

1. Yükseltilmiş erişimle yapmanız gereken değişiklikleri yapın.

    Rol atama hakkında daha fazla bilgi için bkz. [RBAC kullanarak erişimi yönetme ve Azure Portal](role-assignments-portal.md). Azure AD Privileged Identity Management (PıM) kullanıyorsanız, bkz. [PIM 'de yönetmek Için Azure kaynaklarını bulma](../active-directory/privileged-identity-management/pim-resource-roles-discover-resources.md) veya [PIM 'de Azure Kaynak rolleri atama](../active-directory/privileged-identity-management/pim-resource-roles-assign-roles.md).

1. İşiniz bittiğinde, **Azure kaynakları Için erişim yönetimini** **Hayır**olarak ayarlayın. Bu, Kullanıcı başına bir ayar olduğundan, erişimi yükseltmek için aynı kullanıcı olarak oturum açmış olmanız gerekir.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

### <a name="list-role-assignment-at-the-root-scope-"></a>Kök kapsamda rol atamasını listeleyin (/)

Kök kapsamdaki (`/`) bir kullanıcının Kullanıcı erişimi Yöneticisi rolü atamasını listelemek için [Get-azroleatama](/powershell/module/az.resources/get-azroleassignment) komutunu kullanın.

```azurepowershell
Get-AzRoleAssignment | where {$_.RoleDefinitionName -eq "User Access Administrator" `
  -and $_.SignInName -eq "<username@example.com>" -and $_.Scope -eq "/"}
```

```Example
RoleAssignmentId   : /providers/Microsoft.Authorization/roleAssignments/098d572e-c1e5-43ee-84ce-8dc459c7e1f0
Scope              : /
DisplayName        : username
SignInName         : username@example.com
RoleDefinitionName : User Access Administrator
RoleDefinitionId   : 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9
ObjectId           : d65fd0e9-c185-472c-8f26-1dafa01f72cc
ObjectType         : User
CanDelegate        : False
```

### <a name="remove-a-role-assignment-at-the-root-scope-"></a>Kök kapsamda rol atamasını kaldırma (/)

Kök kapsamdaki (`/`) bir kullanıcı için Kullanıcı erişimi yönetici rolü atamasını kaldırmak için aşağıdaki adımları izleyin.

1. Yükseltilmiş erişimi kaldırabilirler bir kullanıcı olarak oturum açın. Bu, erişimi veya başka bir genel yöneticiyi kök kapsamda yükseltilmiş erişimle yükseltmek için kullanılan Kullanıcı ile aynı olabilir.


1. Kullanıcı erişimi yönetici rolü atamasını kaldırmak için [Remove-Azroleatama](/powershell/module/az.resources/remove-azroleassignment) komutunu kullanın.

    ```azurepowershell
    Remove-AzRoleAssignment -SignInName <username@example.com> `
      -RoleDefinitionName "User Access Administrator" -Scope "/"
    ```

## <a name="rest-api"></a>REST API

### <a name="elevate-access-for-a-global-administrator"></a>Genel yönetici için erişimi yükseltme

REST API kullanarak genel yöneticiye erişimi yükseltmek için aşağıdaki temel adımları kullanın.

1. , Size kök kapsamda `elevateAccess`(`/`) Kullanıcı erişimi yönetici rolü veren Rest, çağırın.

   ```http
   POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
   ```

1. Herhangi bir kapsamda herhangi bir rol atamak için bir [rol ataması](/rest/api/authorization/roleassignments) oluşturun. Aşağıdaki örnek, kök kapsamda {Roledefinitionıd} rolünü atamaya yönelik özellikleri gösterir (`/`):

   ```json
   { 
     "properties": {
       "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionID}",
       "principalId": "{objectID}",
       "scope": "/"
     },
     "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
     "type": "Microsoft.Authorization/roleAssignments",
     "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
   }
   ```

1. Kullanıcı yönetici erişimi sağlarken, rol atamalarını kök kapsamda da kaldırabilirsiniz (`/`).

1. Kullanıcı erişim yönetici ayrıcalıklarınızı, tekrar gerekene kadar kaldırın.

### <a name="list-role-assignments-at-the-root-scope-"></a>Rol atamalarını kök kapsamda listeleyin (/)

Bir kullanıcının tüm rol atamalarını kök kapsamda (`/`) listeleyebilirsiniz.

- Rol atamalarını almak istediğiniz kullanıcının `{objectIdOfUser}` nesne kimliği olan [Get roleatamaları](/rest/api/authorization/roleassignments/listforscope) çağırın.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectIdOfUser}'
   ```

### <a name="list-deny-assignments-at-the-root-scope-"></a>Kök kapsamda (/) atamaları Reddet listesi

Bir kullanıcı için tüm reddetme atamalarını kök kapsamda (`/`) listeleyebilirsiniz.

- Reddetme atamalarını almak istediğiniz kullanıcının nesne `{objectIdOfUser}` kimliği olan get denyaslarından çağırın.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter=gdprExportPrincipalId+eq+'{objectIdOfUser}'
   ```

### <a name="remove-elevated-access"></a>Yükseltilmiş erişimi kaldır

' İ çağırdığınızda `elevateAccess`, kendiniz için bir rol ataması oluşturursunuz, bu nedenle bu ayrıcalıkları iptal etmek için atamayı kaldırmanız gerekir.

1. Kullanıcı erişimi Yöneticisi rolünün ad `roleName` kimliğini belirleyebilmek için Kullanıcı erişimi Yöneticisi 'ne eşit olan [roledefinitions](/rest/api/authorization/roledefinitions/get) ' i çağırın.

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

    Bu durumda`18d7d88d-d35e-4fb5-a5c3-7773c20a72d9`, parametreyi parametresinden kaydedin. `name`

2. Dizin kapsamındaki Dizin yöneticisinin rol atamasını de listeetmeniz gerekir. Yükseltme erişimi çağrısını yapan Dizin yöneticisinin Dizin kapsamındaki `principalId` tüm atamaları listeleyin. Bu işlem, ObjectID için dizindeki tüm atamaları listeler.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >Bir dizin yöneticisinin birçok ataması olmaması gerekir. önceki sorgu çok fazla atama döndürürse, yalnızca dizin kapsamı düzeyindeki tüm atamaları sorgulayabilir ve sonra sonuçları filtreleyebilirsiniz:`GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
        
   1. Önceki çağrılar, rol atamalarının bir listesini döndürür. Kapsamın `"/"` bulunduğu rol atamasını `roleDefinitionId` ve 1. adımda bulduğunuz rol adı kimliği ile biter ve `principalId` Dizin yöneticisinin ObjectID ile eşleşen rolü bulun. 
    
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
               "createdBy": "93ce6722-3638-4222-b582-78b75c5c6d65",
               "updatedBy": "93ce6722-3638-4222-b582-78b75c5c6d65"
             },
             "id": "/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
             "type": "Microsoft.Authorization/roleAssignments",
             "name": "e7dd75bc-06f6-4e71-9014-ee96a929d099"
           }
         ],
         "nextLink": null
       }
       ```
        
      Yeniden, bu durumda e7dd75bc-06f6-4e71-9014-ee96a929d099 `name` parametresini parametresinden kaydedin.

   1. Son olarak, tarafından `elevateAccess`eklenen atamayı kaldırmak IÇIN rol atama kimliğini kullanın:

      ```http
      DELETE https://management.azure.com/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01
      ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'daki farklı rolleri anlama](rbac-and-directory-admin-roles.md)
- [RBAC ve REST API kullanarak Azure kaynaklarına erişimi yönetme](role-assignments-rest.md)
