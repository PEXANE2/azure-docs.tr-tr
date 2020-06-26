---
title: Azure Resource Manager şablonu kullanarak Azure özel rolü oluşturma-Azure RBAC
description: Azure Resource Manager şablonları ve Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak Azure özel rolü oluşturmayı öğrenin.
services: role-based-access-control,azure-resource-manager
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.custom: subject-armqs
ms.workload: identity
ms.date: 06/25/2020
ms.author: rolyon
ms.openlocfilehash: bcf1966ffc326291448cb611d99390fe0d652151
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85392982"
---
# <a name="create-an-azure-custom-role-using-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak Azure özel rolü oluşturma

[Azure yerleşik rolleri](built-in-roles.md) , kuruluşunuzun belirli ihtiyaçlarını karşılamıyorsa, kendi [özel rollerinizi](custom-roles.md)de oluşturabilirsiniz. Bu makalede, bir Azure Resource Manager şablonu kullanarak nasıl özel rol oluşturulacağı açıklanır.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

Özel bir rol oluşturmak için şunları yapmanız gerekir:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel bir rol oluşturmak için, rol adı, izinler ve rolün kullanılabileceği yerleri belirtirsiniz. Bu makalede, bir abonelik kapsamında veya daha düşük bir şekilde atanabilecek kaynak izinleriyle "özel rol-RG Reader" adlı bir rol oluşturacaksınız.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu makalede kullanılan şablon, [Azure hızlı başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-deployments/create-role-def). Şablonda dört parametre ve bir kaynak bölümü vardır. Dört parametre şunlardır:

- Varsayılan değeri ["Microsoft. resources/abonelikler/resourceGroups/Read"] olan eylem dizisi
- Boş bir varsayılan değere sahip notActions dizisi
- Varsayılan "özel rol-RG Reader" değerine sahip rol adı
- "Rol tanımının abonelik düzeyi dağıtımı" varsayılan değerine sahip rol açıklaması

Şablonda tanımlanan kaynak:

- [Microsoft. Authorization/roleDefinitions](/azure/templates/Microsoft.Authorization/roleDefinitions)

Bu özel rolün atanabileceği kapsam geçerli aboneliğe ayarlanır.

:::code language="json" source="~/quickstart-templates/subscription-deployments/create-role-def/azuredeploy.json":::

### <a name="deploy-the-template"></a>Şablonu dağıtma

Önceki şablonu dağıtmak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. PowerShell için Azure Cloud Shell açın.

1. Aşağıdaki betiği kopyalayıp Cloud Shell yapıştırın.

    ```azurepowershell
    $location = Read-Host -Prompt "Enter a location (i.e. centralus)"
    [string[]]$actions = Read-Host -Prompt "Enter actions as a comma-separated list (i.e. action1,action2)"
    $actions = $actions.Split(',')

    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-deployments/create-role-def/azuredeploy.json"

    New-AzDeployment -Location $location -TemplateUri $templateUri -actions $actions
    ```

1. Dağıtım için tek *merkezde ABD*gibi bir konum girin.

1. Özel rol için *Microsoft. resources/Resources/Read, Microsoft. resources/abonelikler/resourceGroups/Read*gibi bir virgülle ayrılmış liste olarak eylemlerin bir listesini girin.

1. Gerekirse, New-AzDeployment komutunu çalıştırmak için ENTER tuşuna basın.

    [New-AzDeployment](/powershell/module/az.resources/new-azdeployment) komutu şablonu özel rol oluşturmak için dağıtır.

    Aşağıdakine benzer bir çıktı görmeniz gerekir:

    ```azurepowershell
    PS> New-AzDeployment -Location $location -TemplateUri $templateUri -actions $actions
    
    Id                      : /subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/azuredeploy
    DeploymentName          : azuredeploy
    Location                : centralus
    ProvisioningState       : Succeeded
    Timestamp               : 6/25/2020 8:08:32 PM
    Mode                    : Incremental
    TemplateLink            :
                              Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-deployments/create-role-def/azuredeploy.json
                              ContentVersion : 1.0.0.0
    
    Parameters              :
                              Name               Type                       Value
                              =================  =========================  ==========
                              actions            Array                      [
                                "Microsoft.Resources/resources/read",
                                "Microsoft.Resources/subscriptions/resourceGroups/read"
                              ]
                              notActions         Array                      []
                              roleName           String                     Custom Role - RG Reader
                              roleDescription    String                     Subscription Level Deployment of a Role Definition
    
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Özel rolün oluşturulduğunu doğrulamak için aşağıdaki adımları izleyin.

1. Özel rolü listelemek için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) komutunu çalıştırın.

    ```azurepowershell
    Get-AzRoleDefinition "Custom Role - RG Reader" | ConvertTo-Json
    ```

    Aşağıdakine benzer bir çıktı görmeniz gerekir:

    ```azurepowershell
    {
      "Name": "Custom Role - RG Reader",
      "Id": "11111111-1111-1111-1111-111111111111",
      "IsCustom": true,
      "Description": "Subscription Level Deployment of a Role Definition",
      "Actions": [
        "Microsoft.Resources/resources/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/{subscriptionId}"
      ]
    }
    ```

1. Azure portal aboneliğinizi açın.

1. Sol menüde **erişim denetimi (IAM)** öğesine tıklayın.

1. **Roller** sekmesine tıklayın.

1. **Tür** listesini **customrole**olarak ayarlayın.

1. **Özel rol-RG okuyucu** rolünün listelendiğini doğrulayın.

   ![Azure portal yeni özel rol](./media/custom-roles-template/custom-role-template-portal.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel rolü kaldırmak için aşağıdaki adımları izleyin.

1. Özel rolü kaldırmak için aşağıdaki komutu çalıştırın.

    ```azurepowershell
    Get-AzRoleDefinition -Name "Custom Role - RG Reader" | Remove-AzRoleDefinition
    ```

1. Özel rolü kaldırmak istediğinizi onaylamak için **Y** yazın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure rol tanımlarını anlama](role-definitions.md)
- [Hızlı başlangıç: Azure Resource Manager şablonu kullanarak Azure rol ataması ekleme](quickstart-role-assignments-template.md)
- [ARM şablonu belgeleri](../azure-resource-manager/templates/index.yml)
