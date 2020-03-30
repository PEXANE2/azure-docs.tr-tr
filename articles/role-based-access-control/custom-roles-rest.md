---
title: REST API ile Azure kaynakları için özel roller oluşturma veya güncelleştirme
description: REST API'sini kullanarak Azure kaynakları için rol tabanlı erişim denetimi (RBAC) ile özel rolleri nasıl listeleyeceğimiz, oluşturarak, güncelleştirecek veya silebilirsiniz.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: fda0400310f46da64322654c42af75521746d679
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062182"
---
# <a name="create-or-update-custom-roles-for-azure-resources-using-the-rest-api"></a>REST API'sini kullanarak Azure kaynakları için özel roller oluşturma veya güncelleştirme

> [!IMPORTANT]
> Bir yönetim grubu `AssignableScopes` eklemek şu anda önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Azure [kaynakları için yerleşik roller](built-in-roles.md) kuruluşunuzun özel gereksinimlerini karşılamazsa, kendi özel rollerinizi oluşturabilirsiniz. Bu makalede, REST API'yi kullanarak özel rollerin nasıl listelenildiği, oluşturulan, güncelleştirilen veya silinilen açıklanmaktadır.

## <a name="list-custom-roles"></a>Özel rolleri listeleme

Bir dizindeki tüm özel rolleri listelemek için [Rol Tanımları - Liste](/rest/api/authorization/roledefinitions/list) REST API'sını kullanın.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. *{filter}* rolünün türüyle değiştirin.

    > [!div class="mx-tableFixed"]
    > | Filtre | Açıklama |
    > | --- | --- |
    > | `$filter=type+eq+'CustomRole'` | CustomRole türüne göre filtreleme |

## <a name="list-custom-roles-at-a-scope"></a>Özel rolleri kapsamda listelama

Bir kapsamdaki özel rolleri listelemek için [Rol Tanımları - Liste](/rest/api/authorization/roledefinitions/list) REST API'sını kullanın.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. URI *içinde{scope}* yerine rolleri listelemek istediğiniz kapsam değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/{resourceGroup1}` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/{resourceGroup1}/providers/Microsoft.Web/sites/{site1}` | Kaynak |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |

1. *{filter}* rolünün türüyle değiştirin.

    > [!div class="mx-tableFixed"]
    > | Filtre | Açıklama |
    > | --- | --- |
    > | `$filter=type+eq+'CustomRole'` | CustomRole türüne göre filtreleme |

## <a name="list-a-custom-role-definition-by-name"></a>Özel bir rol tanımını ada göre listele

Görüntü adıyla özel bir rol hakkında bilgi almak için [Rol Tanımları - REST](/rest/api/authorization/roledefinitions/get) API'yi alın' ı kullanın.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. URI *içinde{scope}* yerine rolleri listelemek istediğiniz kapsam değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/{resourceGroup1}` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/{resourceGroup1}/providers/Microsoft.Web/sites/{site1}` | Kaynak |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |

1. *{filter}* rolünün görüntü adı ile değiştirin.

    > [!div class="mx-tableFixed"]
    > | Filtre | Açıklama |
    > | --- | --- |
    > | `$filter=roleName+eq+'{roleDisplayName}'` | Rolün tam görüntü adının URL kodlanmış biçimini kullanın. Örneğin,`$filter=roleName+eq+'Virtual%20Machine%20Contributor'` |

## <a name="list-a-custom-role-definition-by-id"></a>Özel rol tanımını ID'ye göre listele

Benzersiz tanımlayıcısı ile özel bir rol hakkında bilgi almak için [Rol Tanımları - REST API'yi alın.](/rest/api/authorization/roledefinitions/get)

1. Rol [tanımlarını](/rest/api/authorization/roledefinitions/list) kullanın - Rol için GUID tanımlayıcısını almak için REST API'yi listele.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI *içinde{scope}* yerine rolleri listelemek istediğiniz kapsam değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/{resourceGroup1}` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/{resourceGroup1}/providers/Microsoft.Web/sites/{site1}` | Kaynak |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |

1. *{roleDefinitionId}* ile rol tanımının GUID tanımlayıcısını değiştirin.

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel bir rol oluşturmak için [Rol Tanımları - REST API'yi Oluştur veya Güncelleştir'i](/rest/api/authorization/roledefinitions/createorupdate) kullanın. Bu API'yi aramak için, tüm 'de `Microsoft.Authorization/roleDefinitions/write` izin verilen bir rol atanmış `assignableScopes`bir kullanıcıyla oturum açmış olmanız gerekir. Yerleşik rollerarasında yalnızca [Sahibi](built-in-roles.md#owner) ve [Kullanıcı Erişim Yöneticisi](built-in-roles.md#user-access-administrator) bu izni içerir.

1. Özel rolünüz için izinler oluşturmak için kullanılabilen [kaynak sağlayıcı işlemleri](resource-provider-operations.md) listesini gözden geçirin.

1. Özel rol tanımlayıcısı için kullanılacak benzersiz bir tanımlayıcı oluşturmak için bir GUID aracı kullanın. Tanımlayıcının biçimi vardır:`00000000-0000-0000-0000-000000000000`

1. Aşağıdaki istek ve gövde ile başlayın:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId1}",
          "/subscriptions/{subscriptionId2}",
          "/subscriptions/{subscriptionId1}/resourceGroups/{resourceGroup1}",
          "/subscriptions/{subscriptionId2}/resourceGroups/{resourceGroup2}",
          "/providers/Microsoft.Management/managementGroups/{groupId1}"
        ]
      }
    }
    ```

1. URI *içinde{scope}* yerine özel `assignableScopes` rolün ilkini değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/{resourceGroup1}` | Kaynak grubu |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |

1. *{roleDefinitionId}* ile özel rolün GUID tanımlayıcısını değiştirin.

1. İstek gövdesi *içinde,{roleDefinitionId}* ile GUID tanımlayıcısını değiştirin.

1. Abonelik `assignableScopes` veya kaynak grubuysa, *{subscriptionId} veya {resourceGroup}* örneklerini tanımlayıcılarınızla değiştirin. *{resourceGroup}*

1. Bir `assignableScopes` yönetim grubuysa, *{groupId}* örneğini yönetim grubu tanımlayıcınızla değiştirin. Bir yönetim grubu `assignableScopes` eklemek şu anda önizlemededir.

1. Özellikte, `actions` rolün gerçekleştirilmesine izin verdiği işlemleri ekleyin.

1. Özellik, `notActions` izin verilen `actions`dışında olan işlemleri ekleyin.

1. `roleName` Ve `description` özelliklerinde, benzersiz bir rol adı ve açıklama belirtin. Özellikler hakkında daha fazla bilgi için [Özel rolleri](custom-roles.md)görün.

    Aşağıdaki bir istek gövdesi örneği gösterilmektedir:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
              "Microsoft.Storage/*/read",
              "Microsoft.Network/*/read",
              "Microsoft.Compute/*/read",
              "Microsoft.Compute/virtualMachines/start/action",
              "Microsoft.Compute/virtualMachines/restart/action",
              "Microsoft.Authorization/*/read",
              "Microsoft.ResourceHealth/availabilityStatuses/read",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000",
          "/providers/Microsoft.Management/managementGroups/marketing-group"
        ]
      }
    }
    ```

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

Özel bir rolü güncelleştirmek için [Rol Tanımları - REST API'yi Oluştur veya Güncelleştir'i](/rest/api/authorization/roledefinitions/createorupdate) kullanın. Bu API'yi aramak için, tüm 'de `Microsoft.Authorization/roleDefinitions/write` izin verilen bir rol atanmış `assignableScopes`bir kullanıcıyla oturum açmış olmanız gerekir. Yerleşik rollerarasında yalnızca [Sahibi](built-in-roles.md#owner) ve [Kullanıcı Erişim Yöneticisi](built-in-roles.md#user-access-administrator) bu izni içerir.

1. Özel rol hakkında bilgi almak için [Rol Tanımları - Liste](/rest/api/authorization/roledefinitions/list) veya Rol Tanımları - REST API'sini [alın.](/rest/api/authorization/roledefinitions/get) Daha fazla bilgi için önceki [Özel Rolleri Listele](#list-custom-roles) bölümüne bakın.

1. Aşağıdaki istekle başlayın:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI *içinde{scope}* yerine özel `assignableScopes` rolün ilkini değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/{resourceGroup1}` | Kaynak grubu |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |

1. *{roleDefinitionId}* ile özel rolün GUID tanımlayıcısını değiştirin.

1. Özel rol hakkındaki bilgilere dayanarak, aşağıdaki biçime sahip bir istek gövdesi oluşturun:

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId1}",
          "/subscriptions/{subscriptionId2}",
          "/subscriptions/{subscriptionId1}/resourceGroups/{resourceGroup1}",
          "/subscriptions/{subscriptionId2}/resourceGroups/{resourceGroup2}",
          "/providers/Microsoft.Management/managementGroups/{groupId1}"
        ]
      }
    }
    ```

1. İstek gövdesini özel rolde yapmak istediğiniz değişikliklerle güncelleştirin.

    Aşağıdaki yeni bir tanıayarları eylem eklenen bir istek gövdesi örneği gösterir:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
              "Microsoft.Storage/*/read",
              "Microsoft.Network/*/read",
              "Microsoft.Compute/*/read",
              "Microsoft.Compute/virtualMachines/start/action",
              "Microsoft.Compute/virtualMachines/restart/action",
              "Microsoft.Authorization/*/read",
              "Microsoft.ResourceHealth/availabilityStatuses/read",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Insights/diagnosticSettings/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000",
          "/providers/Microsoft.Management/managementGroups/marketing-group"
        ]
      }
    }
    ```

## <a name="delete-a-custom-role"></a>Özel rolü silme

Özel bir rolü silmek için [Rol Tanımları - REST](/rest/api/authorization/roledefinitions/delete) API'sini silin' i kullanın. Bu API'yi aramak için, tüm 'de `Microsoft.Authorization/roleDefinitions/delete` izin verilen bir rol atanmış `assignableScopes`bir kullanıcıyla oturum açmış olmanız gerekir. Yerleşik rollerarasında yalnızca [Sahibi](built-in-roles.md#owner) ve [Kullanıcı Erişim Yöneticisi](built-in-roles.md#user-access-administrator) bu izni içerir.

1. Özel [rolün](/rest/api/authorization/roledefinitions/list) GUID tanımlayıcısını almak için Rol Tanımlarını - Liste veya Rol [Tanımlarını -](/rest/api/authorization/roledefinitions/get) REST API'sini alın. Daha fazla bilgi için önceki [Özel Rolleri Listele](#list-custom-roles) bölümüne bakın.

1. Aşağıdaki istekle başlayın:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI *içinde{scope}* yerine özel rolü silmek istediğiniz kapsam değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/{resourceGroup1}` | Kaynak grubu |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |

1. *{roleDefinitionId}* ile özel rolün GUID tanımlayıcısını değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için özel roller](custom-roles.md)
- [RBAC ve REST API'sini kullanarak Azure kaynaklarına erişimi yönetme](role-assignments-rest.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
