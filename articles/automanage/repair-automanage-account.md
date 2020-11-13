---
title: Bozuk bir Azure oto Yönet hesabını onarma
description: Bozuk bir oto Yönet hesabını nasıl düzelteceğinizi öğrenin
author: asinn826
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 11/05/2020
ms.author: alsin
ms.openlocfilehash: ad54b37da8a4945162b507232f33083890ec1fff
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94558014"
---
# <a name="repair-a-broken-automanage-account"></a>Bozuk bir oto Yönet hesabını onarın
[Otomatikmanage hesabı](./automanage-virtual-machines.md#automanage-account) , güvenlik bağlamına veya otomatik işlemlerin oluştuğu kimliğe sahiptir. Kısa bir süre önce yeni bir kiracıya bir oto Yönet hesabı içeren bir abonelik taşıdıysanız, bu hesabınızı yeniden yapılandırmanız gerekir. Oto Yönet hesabınızı yeniden yapılandırmak için kimlik türünü sıfırlamanız ve hesap için uygun rolleri atamanız gerekir.

## <a name="step-1-reset-automanage-account-identity-type"></a>Adım 1: sıfırlama hesabı kimlik türünü Sıfırla
Aşağıdaki Azure Resource Manager (ARM) şablonuyla, hesap kimliği türünü oto Yönet ' i sıfırlayın. Dosyayı yerel olarak `armdeploy.json` veya benzer şekilde kaydedin. Hesap adı ve konumunuzu, ARM şablonunda gerekli parametreler olduğundan, bu adı göz önüne alın.

1. Aşağıdaki şablonla yeni bir ARM dağıtımı oluşturun ve bunu kullanın `identityType = None`
    * Bunu kullanarak Azure CLı ile yapabilirsiniz `az deployment sub create` . Buradaki komutu hakkında daha fazla bilgi edinin `az deployment sub` . [here](https://docs.microsoft.com/cli/azure/deployment/sub)
    * Bunu ayrıca modülünü kullanarak PowerShell ile de yapabilirsiniz `New-AzDeployment` . Modül hakkında buradan daha fazla bilgi edinin `New AzDeployment` . [here](https://docs.microsoft.com/powershell/module/az.resources/new-azdeployment)

1. Aynı ARM şablonunu bir kez daha çalıştırın `identityType = SystemAssigned`

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "identityType": {
            "type": "string",
            "allowedValues": [ "None", "SystemAssigned" ]
        }
    },
    "resources": [
        {
            "apiVersion": "2020-06-30-preview",
            "name": "[parameters('accountName')]",
            "location": "[parameters('location')]",
            "type": "Microsoft.Automanage/accounts",
            "identity": {
                "type": "[parameters('identityType')]"
            }
        }
    ]
}

```

## <a name="step-2-assign-appropriate-roles-for-the-automanage-account"></a>2. Adım: oto Yönet hesabı için uygun rolleri atama
Oto Yönet hesabı, sizin için, oto tarafından yönetilecek VM 'Leri içeren abonelikte katkıda bulunan ve kaynak Ilkesi katılımcısı rollerinin yapılmasını gerektirir. Azure portal, ARM şablonları veya Azure CLı kullanarak bu rolleri atayabilirsiniz.

ARM şablonu veya Azure CLı kullanıyorsanız, oto Yönet hesabınızın asıl KIMLIĞI (nesne KIMLIĞI olarak da bilinir) gerekir (Azure portal kullanıyorsanız bu gerekli değildir). Aşağıdaki yöntemleri kullanarak, sizin için, sizin, sizin için bir sonraki

- [Azure CLI](https://docs.microsoft.com/cli/azure/ad/sp): komutunu kullanın `az ad sp list --display-name <name of your Automanage Account>` .

- Azure portal: **Azure Active Directory** gidin ve hesap adına göre oto Yönet hesabınızı arayın. **Kurumsal uygulamalar** altında, görüntülendiğinde, hesap adını oto Yönet ' i seçin.

### <a name="azure-portal"></a>Azure portal
1. **Abonelikler** ' in altında, oto tarafından yönetilen sanal makinelerinizi içeren aboneliğe gidin.
1. **Access Control (IAM)** sayfasına gidin.
1. **Rol atamaları Ekle** ' ye tıklayın.
1. **Katkıda bulunan** rolünü seçin ve oto Yönet hesabınızın adını yazın.
1. **Kaydet** ’e basın.
1. Bu kez **kaynak Ilkesi katılımcısı** rolüyle 3-5 arasındaki adımları yineleyin.

### <a name="arm-template"></a>ARM şablonu
Aşağıdaki ARM şablonunu çalıştırın. Ana KIMLIĞI almak için, kendi hesabınızın asıl KIMLIĞINE sahip olmanız gerekir. İstendiğinde girin.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        }
    },
    "variables": {
        "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Resource Policy Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '36243c78-bf99-498c-9df9-86d9f8d28608')]"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[guid(uniqueString(variables('Contributor')))]",
            "properties": {
                "roleDefinitionId": "[variables('Contributor')]",
                "principalId": "[parameters('principalId')]"
            }
        },
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[guid(uniqueString(variables('Resource Policy Contributor')))]",
            "properties": {
                "roleDefinitionId": "[variables('Resource Policy Contributor')]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

### <a name="azure-cli"></a>Azure CLI
Aşağıdaki komutları çalıştırın:

```azurecli
az role assignment create --assignee-object-id <your Automanage Account's object id> --role "Contributor" --scope /subscriptions/<your subscription id>

az role assignment create --assignee-object-id <your Automanage Account's object id> --role "Resource Policy Contributor" --scope /subscriptions/<your subscription id>
```

## <a name="next-steps"></a>Sonraki adımlar
Azure oto yönetimi hakkında [buradan](./automanage-virtual-machines.md)daha fazla bilgi edinin.
