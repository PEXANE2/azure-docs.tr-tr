---
title: Dağıtım geçmişi
description: Portal, PowerShell, Azure CLI ve REST API ile Azure Kaynak Yöneticisi dağıtım işlemlerinin nasıl görüntüleneceğimiz açıklanır.
tags: top-support-issue
ms.topic: conceptual
ms.date: 11/26/2019
ms.openlocfilehash: b0f196f86bed05094b04bfc20c7cef2248a91c65
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460305"
---
# <a name="view-deployment-history-with-azure-resource-manager"></a>Azure Kaynak Yöneticisi ile dağıtım geçmişini görüntüleme

Azure Kaynak Yöneticisi, dağıtım geçmişinizi görüntülemenize ve geçmiş dağıtımlarda belirli işlemleri incelemenize olanak tanır. Dağıtılan kaynakları görebilir ve hatalar hakkında bilgi alabilirsiniz.

Belirli dağıtım hatalarını çözme konusunda yardım için, [Azure Kaynak Yöneticisi ile kaynakları Azure'a dağıtırken sık karşılaşılan hataları giderme](common-deployment-errors.md)bölümüne bakın.

## <a name="get-deployments-and-correlation-id"></a>Dağıtımlar ve korelasyon kimliği alın

Azure portalı, PowerShell, Azure CLI veya REST API'den dağıtımla ilgili ayrıntıları görüntüleyebilirsiniz. Her dağıtım, ilgili olayları izlemek için kullanılan bir korelasyon kimliğine sahiptir. Bir dağıtımı gidermek için teknik destekle çalışırken yararlı olabilir.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. İncelemek istediğiniz kaynak grubunu seçin.

1. **Dağıtımlar**altındaki bağlantıyı seçin.

   ![Dağıtım geçmişini seçin](./media/deployment-history/select-deployment-history.png)

1. Dağıtım geçmişinden dağıtımlardan birini seçin.

   ![Dağıtımı seçin](./media/deployment-history/select-details.png)

1. Dağıtımın bir özeti, korelasyon kimliği de dahil olmak üzere görüntülenir.

    ![Dağıtım özeti](./media/deployment-history/show-correlation-id.png)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Bir kaynak grubu için tüm dağıtımları listelemek için [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment) komutunu kullanın.

```azurepowershell-interactive
Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup
```

Kaynak grubundan belirli bir dağıtım almak için **DeploymentName** parametresini ekleyin.

```azurepowershell-interactive
Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName ExampleDeployment
```

Korelasyon kimliğini almak için şunları kullanın:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName ExampleDeployment).CorrelationId
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Kaynak grubu için dağıtımı listelemek için [az dağıtım grup listesini](/cli/azure/group/deployment?view=azure-cli-latest#az-deployment-group-list)kullanın.

```azurecli-interactive
az deployment group list --resource-group ExampleGroup
```

Belirli bir dağıtım almak için [az dağıtım grubu gösterisini](/cli/azure/group/deployment?view=azure-cli-latest#az-deployment-group-show)kullanın.

```azurecli-interactive
az deployment group show --resource-group ExampleGroup --name ExampleDeployment
```

Korelasyon kimliğini almak için şunları kullanın:

```azurecli-interactive
az deployment group show --resource-group ExampleGroup --name ExampleDeployment --query properties.correlationId
```

# <a name="http"></a>[HTTP Kı.,](#tab/http)

Bir kaynak grubunun dağıtımlarını listelemek için aşağıdaki işlemi kullanın. İstekte kullanılacak en son API sürüm numarası için [Dağıtımlar - Kaynak Grubuna Göre Liste](/rest/api/resources/deployments/listbyresourcegroup)'ye bakın.

```
GET https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/?api-version={api-version}
```

Belirli bir dağıtım almak için. aşağıdaki işlemi kullanın. İstekte kullanılacak en son API sürüm numarası için [Dağıtımlar - Get](/rest/api/resources/deployments/get)' e bakın.

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
```

Yanıt, korelasyon kimliğini içerir.

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

## <a name="get-deployment-operations-and-error-message"></a>Dağıtım işlemleri ve hata iletisi alın

Her dağıtım birden çok işlem içerebilir. Dağıtım hakkında daha fazla ayrıntı görmek için dağıtım işlemlerini görüntüleyin. Bir dağıtım başarısız olduğunda, dağıtım işlemleri bir hata iletisi içerir.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Dağıtım özetinde, İşlem **ayrıntılarını**seçin.

    ![Dağıtım işlemlerini seçin](./media/deployment-history/get-operation-details.png)

1. Dağıtımın o adımının ayrıntılarını görüyorsunuz. Bir hata oluştuğunda, ayrıntılar hata iletisini içerir.

    ![İşlem ayrıntılarını göster](./media/deployment-history/see-operation-details.png)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Bir kaynak grubuna dağıtım için dağıtım işlemlerini görüntülemek için [Get-AzResourceGroupDeploymentOperation](/powershell/module/az.resources/get-azdeploymentoperation) komutunu kullanın.

```azurepowershell-interactive
Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName ExampleDeploy
```

Başarısız işlemleri görüntülemek için, **Başarısız** durumla işlemleri filtreleyin.

```azurepowershell-interactive
(Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName ExampleDeploy).Properties | Where-Object ProvisioningState -eq Failed
```

Başarısız işlemlerin durum iletisini almak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
((Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName ExampleDeploy ).Properties | Where-Object ProvisioningState -eq Failed).StatusMessage.error
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Kaynak grubuna dağıtım için dağıtım işlemlerini görüntülemek için [az dağıtım grubu işlem listesi](/cli/azure/group/deployment/operation?view=azure-cli-latest#az-deployment-group-operation-list) komutunu kullanın.

```azurecli-interactive
az deployment group operation list --resource-group ExampleGroup --name ExampleDeployment
```

Başarısız işlemleri görüntülemek için, **Başarısız** durumla işlemleri filtreleyin.

```azurecli-interactive
az deployment group operation list --resource-group ExampleGroup --name ExampleDeploy --query "[?properties.provisioningState=='Failed']"
```

Başarısız işlemlerin durum iletisini almak için aşağıdaki komutu kullanın:

```azurecli-interactive
az deployment group operation list --resource-group ExampleGroup --name ExampleDeploy --query "[?properties.provisioningState=='Failed'].properties.statusMessage.error"
```

# <a name="http"></a>[HTTP Kı.,](#tab/http)

Dağıtım işlemlerini almak için aşağıdaki işlemi kullanın. İstekte kullanılacak en son API sürüm numarası için [Dağıtım İşlemleri - Liste'ye](/rest/api/resources/deploymentoperations/list)bakın.

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

* Belirli dağıtım hatalarını çözme konusunda yardım için, [Azure Kaynak Yöneticisi ile kaynakları Azure'a dağıtırken sık karşılaşılan hataları giderme](common-deployment-errors.md)bölümüne bakın.
* Diğer eylem türlerini izlemek için etkinlik günlüklerini kullanma hakkında bilgi edinmek [için Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüle'ye](../management/view-activity-logs.md)bakın.
* Dağıtımınızı yürütmeden önce doğrulamak için Azure [Kaynak Yöneticisi şablonuna sahip bir kaynak grubu dağıt'a](deploy-powershell.md)bakın.

