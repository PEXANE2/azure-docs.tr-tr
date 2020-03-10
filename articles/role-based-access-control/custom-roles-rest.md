---
title: REST API ile Azure kaynakları için özel roller oluşturun veya güncelleştirin
description: REST API kullanarak Azure kaynakları için rol tabanlı erişim denetimi (RBAC) ile özel rolleri listeleme, oluşturma, güncelleştirme veya silme hakkında bilgi edinin.
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
ms.date: 04/18/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 145bc45e1b7faeddc23cf5f0662337e15ab51c29
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78392480"
---
# <a name="create-or-update-custom-roles-for-azure-resources-using-the-rest-api"></a>REST API kullanarak Azure kaynakları için özel roller oluşturma veya güncelleştirme

[Azure kaynaklarına yönelik yerleşik roller](built-in-roles.md) , kuruluşunuzun belirli ihtiyaçlarını karşılamıyorsa, kendi özel rollerinizi oluşturabilirsiniz. Bu makalede, REST API kullanarak özel rolleri listeleme, oluşturma, güncelleştirme veya silme işlemlerinin nasıl yapılacağı açıklanır.

## <a name="list-custom-roles"></a>Özel rolleri listeleme

Bir dizindeki tüm özel rolleri listelemek için, [rol tanımları-liste](/rest/api/authorization/roledefinitions/list) REST API kullanın.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. *{Filter}* öğesini rol türüyle değiştirin.

    | Filtre | Açıklama |
    | --- | --- |
    | `$filter=type%20eq%20'CustomRole'` | CustomRole türünü temel alan filtre |

## <a name="list-custom-roles-at-a-scope"></a>Bir kapsamdaki özel rolleri listeleme

Özel rolleri bir kapsamda listelemek için, [rol tanımları-liste](/rest/api/authorization/roledefinitions/list) REST API kullanın.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. URI içinde, *{scope}* değerini rolleri listelemek istediğiniz kapsamla değiştirin.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. *{Filter}* öğesini rol türüyle değiştirin.

    | Filtre | Açıklama |
    | --- | --- |
    | `$filter=type%20eq%20'CustomRole'` | CustomRole türünü temel alan filtre |

## <a name="list-a-custom-role-definition-by-name"></a>Özel bir rol tanımını ada göre Listele

Görünen adına göre özel bir rol hakkında bilgi almak için, [get REST API rol tanımlarını](/rest/api/authorization/roledefinitions/get) kullanın.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. URI içinde, *{scope}* değerini rolleri listelemek istediğiniz kapsamla değiştirin.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. *{Filter}* değerini rolün görünen adıyla değiştirin.

    | Filtre | Açıklama |
    | --- | --- |
    | `$filter=roleName%20eq%20'{roleDisplayName}'` | Rolün tam ekran adının URL kodlamalı formunu kullanın. Örneğin, `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

## <a name="list-a-custom-role-definition-by-id"></a>KIMLIĞE göre özel bir rol tanımı listeleyin

Özel bir rol hakkında benzersiz tanımlayıcı ile ilgili bilgi almak için, Get REST API [rol tanımlarını](/rest/api/authorization/roledefinitions/get) kullanın.

1. Rolün GUID tanımlayıcısını almak için [rol tanımları-liste](/rest/api/authorization/roledefinitions/list) REST API kullanın.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI içinde, *{scope}* değerini rolleri listelemek istediğiniz kapsamla değiştirin.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. *{Roledefinitionıd}* değerini rol tanımının GUID tanımlayıcısı ile değiştirin.

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel bir rol oluşturmak için, rol tanımlarını kullanın; REST API [oluşturun veya güncelleştirin](/rest/api/authorization/roledefinitions/createorupdate) . Bu API 'yi çağırmak için, tüm `assignableScopes``Microsoft.Authorization/roleDefinitions/write` iznine sahip bir rol atanmış bir kullanıcıyla oturum açmalısınız. Yerleşik rollerde yalnızca [sahip](built-in-roles.md#owner) ve [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu izni içerir.

1. Özel rolünüzün izinlerini oluşturmak için kullanılabilir [kaynak sağlayıcısı işlemlerinin](resource-provider-operations.md) listesini gözden geçirin.

1. Özel rol tanımlayıcısı için kullanılacak benzersiz bir tanımlayıcı oluşturmak için bir GUID aracı kullanın. Tanımlayıcı şu biçimdedir: `00000000-0000-0000-0000-000000000000`

1. Aşağıdaki istek ve gövdeyi başlatın:

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
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. URI içinde, *{scope}* öğesini özel rolün birinci `assignableScopes` ile değiştirin.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. *{Roledefinitionıd}* öğesini özel rolün GUID tanımlayıcısı ile değiştirin.

1. İstek gövdesi içinde, `assignableScopes` özelliğindeki *{Roledefinitionıd}* değerini GUID tanımlayıcısı ile değiştirin.

1. *{SubscriptionID}* öğesini abonelik tanımlayıcın ile değiştirin.

1. `actions` özelliğinde, rolün izin verdiği işlemleri ekleyin.

1. `notActions` özelliğinde, izin verilen `actions`hariç tutulan işlemleri ekleyin.

1. `roleName` ve `description` Özellikler ' de benzersiz bir rol adı ve açıklama belirtin. Özellikler hakkında daha fazla bilgi için bkz. [özel roller](custom-roles.md).

    Aşağıda bir istek gövdesi örneği gösterilmektedir:

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
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

Özel bir rolü güncelleştirmek için, rol tanımlarını kullanın; REST API [oluşturun veya güncelleştirin](/rest/api/authorization/roledefinitions/createorupdate) . Bu API 'yi çağırmak için, tüm `assignableScopes``Microsoft.Authorization/roleDefinitions/write` iznine sahip bir rol atanmış bir kullanıcıyla oturum açmalısınız. Yerleşik rollerde yalnızca [sahip](built-in-roles.md#owner) ve [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu izni içerir.

1. Özel rol hakkında bilgi almak için [rol tanımları-liste](/rest/api/authorization/roledefinitions/list) veya [rol tanımları-al](/rest/api/authorization/roledefinitions/get) REST API kullanın. Daha fazla bilgi için, önceki [özel roller listesi](#list-custom-roles) bölümüne bakın.

1. Aşağıdaki istekle başlayın:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI içinde, *{scope}* öğesini özel rolün birinci `assignableScopes` ile değiştirin.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. *{Roledefinitionıd}* öğesini özel rolün GUID tanımlayıcısı ile değiştirin.

1. Özel rolle ilgili bilgilere bağlı olarak, aşağıdaki biçimde bir istek gövdesi oluşturun:

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
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. İstek gövdesini, özel rolde yapmak istediğiniz değişikliklerle güncelleştirin.

    Aşağıda yeni bir tanılama ayarları eylemi eklenen bir istek gövdesi örneği gösterilmektedir:

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
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="delete-a-custom-role"></a>Özel rolü silme

Özel bir rolü silmek için, [rol tanımlarını kullanın-sil](/rest/api/authorization/roledefinitions/delete) REST API. Bu API 'yi çağırmak için, tüm `assignableScopes``Microsoft.Authorization/roleDefinitions/delete` iznine sahip bir rol atanmış bir kullanıcıyla oturum açmalısınız. Yerleşik rollerde yalnızca [sahip](built-in-roles.md#owner) ve [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu izni içerir.

1. [Rol tanımlarını-liste](/rest/api/authorization/roledefinitions/list) veya [rol tanımlarını kullanın-](/rest/api/authorization/roledefinitions/get) özel rolün guıd tanımlayıcısını almak için REST API alın. Daha fazla bilgi için, önceki [özel roller listesi](#list-custom-roles) bölümüne bakın.

1. Aşağıdaki istekle başlayın:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI içinde, *{scope}* öğesini özel rolü silmek istediğiniz kapsamla değiştirin.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. *{Roledefinitionıd}* öğesini özel rolün GUID tanımlayıcısı ile değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için özel roller](custom-roles.md)
- [RBAC ve REST API kullanarak Azure kaynaklarına erişimi yönetme](role-assignments-rest.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
