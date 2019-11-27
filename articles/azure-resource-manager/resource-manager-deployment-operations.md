---
title: Dağıtım geçmişi
description: Portal, PowerShell, Azure CLı ve REST API Azure Resource Manager dağıtım işlemlerinin nasıl görüntüleneceğini açıklar.
tags: top-support-issue
ms.topic: conceptual
ms.date: 11/26/2019
ms.openlocfilehash: 895704e5c4cb8acc60067809bdd7e7baa6f05142
ms.sourcegitcommit: 36eb583994af0f25a04df29573ee44fbe13bd06e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74538874"
---
# <a name="view-deployment-history-with-azure-resource-manager"></a>Azure Resource Manager ile dağıtım geçmişini görüntüle

Azure Resource Manager, dağıtım geçmişinizi görüntülemenize ve geçmiş dağıtımlarda belirli işlemleri incelemenize olanak sağlar. Dağıtılan kaynakları görebilir ve hatalar hakkında bilgi alabilirsiniz.

Belirli dağıtım hatalarını çözümlemede yardım için bkz. [Azure 'a kaynak dağıtılırken sık karşılaşılan hataları çözümleme Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="get-deployments-and-correlation-id"></a>Dağıtımları ve bağıntı KIMLIĞINI al

Azure portal, PowerShell, Azure CLı veya REST API bir dağıtım hakkındaki ayrıntıları görüntüleyebilirsiniz. Her dağıtımda ilgili olayları izlemek için kullanılan bir bağıntı KIMLIĞI vardır. Bir dağıtımda sorun gidermeye yönelik teknik destekle çalışırken yararlı olabilir.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

1. İncelemek istediğiniz kaynak grubunu seçin.

1. **Dağıtımlar**altındaki bağlantıyı seçin.

   ![Dağıtım geçmişini seçin](./media/resource-manager-deployment-operations/select-deployment-history.png)

1. Dağıtım geçmişinden dağıtımlardan birini seçin.

   ![Dağıtım seçin](./media/resource-manager-deployment-operations/select-details.png)

1. Bağıntı KIMLIĞI de dahil olmak üzere dağıtımın bir özeti görüntülenir. 

    ![Dağıtım Özeti](./media/resource-manager-deployment-operations/show-correlation-id.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Bir kaynak grubunun tüm dağıtımlarını listelemek için [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment) komutunu kullanın.

```azurepowershell-interactive
Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup
```

Bir kaynak grubundan belirli bir dağıtım almak için **DeploymentName** parametresini ekleyin.

```azurepowershell-interactive
Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName ExampleDeployment
```

Bağıntı KIMLIĞINI almak için şunu kullanın:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName ExampleDeployment).CorrelationId
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Bir kaynak grubunun dağıtımını listelemek için [az Group Deployment List](/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-list)kullanın.

```azurecli-interactive
az group deployment list --resource-group ExampleGroup
```

Belirli bir dağıtımı almak için [az Group Deployment Show](/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-show)' ı kullanın.

```azurecli-interactive
az group deployment show --resource-group ExampleGroup --name ExampleDeployment
```
  
Bağıntı KIMLIĞINI almak için şunu kullanın:

```azurecli-interactive
az group deployment show --resource-group ExampleGroup --name ExampleDeployment --query properties.correlationId
```

# <a name="httptabhttp"></a>[HTTP](#tab/http)

Bir kaynak grubunun dağıtımlarını listelemek için aşağıdaki işlemi kullanın. İstekte kullanılacak en son API sürüm numarası için bkz. [kaynak grubuna göre dağıtımlar-Listele](/rest/api/resources/deployments/listbyresourcegroup). 

```
GET https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/?api-version={api-version}
```

Belirli bir dağıtımı almak için. aşağıdaki işlemi kullanın. İstekte kullanılacak en son API sürüm numarası için bkz. [dağıtımlar-al](/rest/api/resources/deployments/get).

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
```

Yanıt bağıntı KIMLIĞINI içerir.

```json
{
 ...
 "properties": {
   "mode": "Incremental",
   "provisioningState": "Failed",
   "timestamp": "2019-11-26T14:18:36.4518358Z",
   "duration": "PT26.2091817S",
   "correlationId": "47ff4228-bf2e-4ee5-a008-0b07da681230",
   ...
 }
}
```

---

## <a name="get-deployment-operations-and-error-message"></a>Dağıtım işlemlerini ve hata iletisini Al

Her dağıtım birden çok işlem içerebilir. Bir dağıtım hakkında daha fazla ayrıntı görmek için dağıtım işlemlerini görüntüleyin. Dağıtım başarısız olduğunda, dağıtım işlemleri bir hata iletisi içerir.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

1. Bir dağıtımın özetinde **işlem ayrıntıları**' nı seçin.

    ![Dağıtım işlemlerini seçin](./media/resource-manager-deployment-operations/get-operation-details.png)

1. Dağıtım adımının ayrıntılarını görürsünüz. Bir hata oluştuğunda, Ayrıntılar hata mesajını içerir.

    ![İşlem ayrıntılarını göster](./media/resource-manager-deployment-operations/see-operation-details.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Bir kaynak grubuna dağıtıma yönelik dağıtım işlemlerini görüntülemek için [Get-AzResourceGroupDeploymentOperation](/powershell/module/az.resources/get-azdeploymentoperation) komutunu kullanın.

```azurepowershell-interactive
Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName ExampleDeploy
```

Başarısız işlemleri görüntülemek için, **başarısız** durumundaki işlemleri filtreleyin.

```azurepowershell-interactive
(Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName ExampleDeploy).Properties | Where-Object ProvisioningState -eq Failed
```

Başarısız işlemlerin durum iletisini almak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
((Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName ExampleDeploy ).Properties | Where-Object ProvisioningState -eq Failed).StatusMessage.error
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Bir kaynak grubuna dağıtıma yönelik dağıtım işlemlerini görüntülemek için [az Group Deployment Operation List](/cli/azure/group/deployment/operation?view=azure-cli-latest#az-group-deployment-operation-list) komutunu kullanın.

```azurecli-interactive
az group deployment operation list --resource-group ExampleGroup --name ExampleDeployment
```

Başarısız işlemleri görüntülemek için, **başarısız** durumundaki işlemleri filtreleyin.

```azurecli-interactive
az group deployment operation list --resource-group ExampleGroup --name ExampleDeploy --query "[?properties.provisioningState=='Failed']"
```

Başarısız işlemlerin durum iletisini almak için aşağıdaki komutu kullanın:

```azurecli-interactive
az group deployment operation list --resource-group ExampleGroup --name ExampleDeploy --query "[?properties.provisioningState=='Failed'].properties.statusMessage.error"
```

# <a name="httptabhttp"></a>[HTTP](#tab/http)

Dağıtım işlemlerini almak için aşağıdaki işlemi kullanın. İstekte kullanılacak en son API sürüm numarası için bkz. [dağıtım işlemleri-listesi](/rest/api/resources/deploymentoperations/list).

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
```

Yanıt bir hata iletisi içerir.

```json
{
  "value": [
    {
      "id": "/subscriptions/xxxx/resourceGroups/examplegroup/providers/Microsoft.Resources/deployments/exampledeploy/operations/13EFD9907103D640",
      "operationId": "13EFD9907103D640",
      "properties": {
        "provisioningOperation": "Create",
        "provisioningState": "Failed",
        "timestamp": "2019-11-26T14:18:36.3177613Z",
        "duration": "PT21.0580179S",
        "trackingId": "9d3cdac4-54f8-486c-94bd-10c20867b8bc",
        "serviceRequestId": "01a9d0fe-896b-4c94-a30f-60b70a8f1ad9",
        "statusCode": "BadRequest",
        "statusMessage": {
          "error": {
            "code": "InvalidAccountType",
            "message": "The AccountType Standard_LRS1 is invalid. For more information, see - https://aka.ms/storageaccountskus"
          }
        },
        "targetResource": {
          "id": "/subscriptions/xxxx/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/storageq2czadzfgizc2",
          "resourceType": "Microsoft.Storage/storageAccounts",
          "resourceName": "storageq2czadzfgizc2"
        }
      }
    },
    ...
  ]
}
```

---

## <a name="next-steps"></a>Sonraki adımlar

* Belirli dağıtım hatalarını çözümlemede yardım için bkz. [Azure 'a kaynak dağıtılırken sık karşılaşılan hataları çözümleme Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Diğer eylem türlerini izlemek için etkinlik günlüklerini kullanma hakkında bilgi edinmek için bkz. [Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüleme](resource-group-audit.md).
* Yürütmeden önce dağıtımınızı doğrulamak için, bkz. [Azure Resource Manager şablonuyla kaynak grubu dağıtma](resource-group-template-deploy.md).

