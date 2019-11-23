---
title: Tutorial - Create a custom role for Azure resources using Azure CLI
description: Get started creating a custom role for Azure resources using Azure CLI in this tutorial.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 02/20/2019
ms.author: rolyon
ms.openlocfilehash: 29b0bea851466b5173711441a5a7927c796c9926
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74419587"
---
# <a name="tutorial-create-a-custom-role-for-azure-resources-using-azure-cli"></a>Tutorial: Create a custom role for Azure resources using Azure CLI

If the [built-in roles for Azure resources](built-in-roles.md) don't meet the specific needs of your organization, you can create your own custom roles. Bu öğretici için Azure CLI'yı kullanarak Reader Support Tickets adlı özel bir rol oluşturacaksınız. The custom role allows the user to view everything in the management plane of a subscription and also open support tickets.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Özel rol oluşturma
> * Özel rolleri listeleme
> * Özel rolü güncelleştirme
> * Özel rolü silme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni
- [Azure Cloud Shell](../cloud-shell/overview.md) or [Azure CLI](/cli/azure/install-azure-cli)

## <a name="sign-in-to-azure-cli"></a>Azure CLI'da oturum açma

[Azure CLI](/cli/azure/authenticate-azure-cli)'da oturum açın.

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel rol oluşturmanın en kolay yolu bir JSON şablonuyla başlayıp değişikliklerinizi ekledikten sonra yeni bir rol oluşturmaktır.

1. [Microsoft.Support kaynak sağlayıcısının](resource-provider-operations.md#microsoftsupport) işlem listesini gözden geçirin. İzinlerinizi oluşturmak için kullanabileceğiniz işlemleri bilmeniz yararlıdır.

    | İşlem | Açıklama |
    | --- | --- |
    | Microsoft.Support/register/action | Destek Kaynağı Sağlayıcısı'na kayıt yapar |
    | Microsoft.Support/supportTickets/read | Durum, önem derecesi, kişi ayrıntıları ve iletişimler gibi Destek Biletleri ayrıntılarını alır veya aboneliklerdeki Destek Biletleri listesini alır. |
    | Microsoft.Support/supportTickets/write | Destek Bileti oluşturur veya güncelleştirir. Teknik, Faturalama, Kotalar veya Abonelik Yönetimi konusunda bir Destek Bileti oluşturabilirsiniz. Var olan destek biletlerinin önem derecesini, iletişim bilgilerini ve iletişimlerini güncelleştirebilirsiniz. |
    
1. **ReaderSupportRole.json** adlı yeni bir dosya oluşturun.

1. ReaderSupportRole.json dosyasını bir düzenleyicide açıp aşağıdaki JSON kodunu ekleyin.

    For information about the different properties, see [Custom roles for Azure resources](custom-roles.md).

    ```json
    {
      "Name": "",
      "IsCustom": true,
      "Description": "",
      "Actions": [],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/{subscriptionId1}"
      ]
    }
    ```
    
1. Aşağıdaki işlemleri `Actions` özelliğine ekleyin. Bu eylemler, kullanıcının abonelikteki her şeyi görüntülemesini ve destek bileti oluşturmasını sağlar.

    ```
    "*/read",
    "Microsoft.Support/*"
    ```

1. [az account list](/cli/azure/account#az-account-list) komutunu kullanarak aboneliğinizin kimliğini alın.

    ```azurecli
    az account list --output table
    ```

1. `AssignableScopes` içinde `{subscriptionId1}` yerine abonelik kimliğinizi yazın.

    Açık abonelik kimliklerini girmeniz gerekir, aksi halde rolü aboneliğinize aktaramazsınız.

1. `Name` ve `Description` özelliklerini "Okuyucu Destek Biletleri" ve "Abonelikteki her şeyi görüntüleme ve destek bileti açma." olarak değiştirin.

    JSON dosyanız aşağıdaki gibi görünmelidir:

    ```json
    {
      "Name": "Reader Support Tickets",
      "IsCustom": true,
      "Description": "View everything in the subscription and also open support tickets.",
      "Actions": [
        "*/read",
        "Microsoft.Support/*"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
    
1. Yeni özel rolü oluşturmak için [az role definition create](/cli/azure/role/definition#az-role-definition-create) komutunu kullanın ve JSON rol tanımı dosyasını belirtin.

    ```azurecli
    az role definition create --role-definition "~/CustomRoles/ReaderSupportRole.json"
    ```

    ```Output
    {
      "additionalProperties": {},
      "assignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ],
      "description": "View everything in the subscription and also open support tickets.",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/22222222-2222-2222-2222-222222222222",
      "name": "22222222-2222-2222-2222-222222222222",
      "permissions": [
        {
          "actions": [
            "*/read",
            "Microsoft.Support/*"
          ],
          "additionalProperties": {},
          "dataActions": [],
          "notActions": [],
          "notDataActions": []
        }
      ],
      "roleName": "Reader Support Tickets",
      "roleType": "CustomRole",
      "type": "Microsoft.Authorization/roleDefinitions"
    }
    ```

    Yeni özel rol artık kullanılabilir ve yerleşik roller gibi kullanıcılara, gruplara veya hizmet sorumlularına atanabilir.

## <a name="list-custom-roles"></a>Özel rolleri listeleme

- Özel rollerinizin tümünü listelemek için [az role definition list](/cli/azure/role/definition#az-role-definition-list) komutunu `--custom-role-only` parametresiyle birlikte kullanın.

    ```azurecli
    az role definition list --custom-role-only true
    ```
    
    ```Output
    [
      {
        "additionalProperties": {},
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ],
        "description": "View everything in the subscription and also open support tickets.",
        "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/22222222-2222-2222-2222-222222222222",
        "name": "22222222-2222-2222-2222-222222222222",
        "permissions": [
          {
            "actions": [
              "*/read",
              "Microsoft.Support/*",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Insights/diagnosticSettings/*/read"
            ],
            "additionalProperties": {},
            "dataActions": [],
            "notActions": [],
            "notDataActions": []
          }
        ],
        "roleName": "Reader Support Tickets",
        "roleType": "CustomRole",
        "type": "Microsoft.Authorization/roleDefinitions"
      }
    ]
    ```
    
    Özel rolü Azure portalında da görebilirsiniz.

    ![Azure portalına aktarılmış olan özel rolün ekran görüntüsü](./media/tutorial-custom-role-cli/custom-role-reader-support-tickets.png)

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

Özel rolü güncelleştirmek için JSON dosyasını ve ardından özel rolü güncelleştirin.

1. ReaderSupportRole.json dosyasını açın.

1. `Actions` içine kaynak grubu dağıtımlarını oluşturma ve yönetme işlemini ekleyin: `"Microsoft.Resources/deployments/*"`. Önceki işlemden sonra virgül eklemeyi unutmayın.

    Güncelleştirilmiş JSON dosyanız aşağıdaki gibi görünmelidir:

    ```json
    {
      "Name": "Reader Support Tickets",
      "IsCustom": true,
      "Description": "View everything in the subscription and also open support tickets.",
      "Actions": [
        "*/read",
        "Microsoft.Support/*",
        "Microsoft.Resources/deployments/*"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
        
1. Özel rolü güncelleştirmek için [az role definition update](/cli/azure/role/definition#az-role-definition-update) komutunu kullanarak güncelleştirilmiş JSON dosyasını belirtin.

    ```azurecli
    az role definition update --role-definition "~/CustomRoles/ReaderSupportRole.json"
    ```

    ```Output
    {
      "additionalProperties": {},
      "assignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ],
      "description": "View everything in the subscription and also open support tickets.",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/22222222-2222-2222-2222-222222222222",
      "name": "22222222-2222-2222-2222-222222222222",
      "permissions": [
        {
          "actions": [
            "*/read",
            "Microsoft.Support/*",
            "Microsoft.Resources/deployments/*"
          ],
          "additionalProperties": {},
          "dataActions": [],
          "notActions": [],
          "notDataActions": []
        }
      ],
      "roleName": "Reader Support Tickets",
      "roleType": "CustomRole",
      "type": "Microsoft.Authorization/roleDefinitions"
    }
    ```
    
## <a name="delete-a-custom-role"></a>Özel rolü silme

- Özel rolü silmek için [az role definition delete](/cli/azure/role/definition#az-role-definition-delete) komutunu kullanın ve rol adını veya rol kimliğini belirtin.

    ```azurecli
    az role definition delete --name "Reader Support Tickets"
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak Azure kaynakları için özel roller oluşturma](custom-roles-cli.md)