---
title: Azure Red Hat OpenShift v3 kümenizi izlemeyi durdurma | Microsoft Docs
description: Bu makalede, Azure Red Hat OpenShift kümenizi kapsayıcılar için Azure Izleyici ile izlemeyi nasıl durdurulabileceğinizi açıklanmaktadır.
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: e1d3200af62ad185fa942fa2c8f7f3b4e6bfd89b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82196198"
---
# <a name="how-to-stop-monitoring-your-azure-red-hat-openshift-v3-cluster"></a>Azure Red Hat OpenShift v3 kümenizi izlemeyi durdurma

Azure Red Hat OpenShift sürüm 3. x kümenizi izlemeyi etkinleştirdikten sonra, artık bunu izlemek istediğinize karar verirseniz, kapsayıcıyı kapsayıcılar için Azure Izleyici ile izlemeyi durdurabilirsiniz. Bu makalede, belirtilen Azure Resource Manager şablonu kullanılarak bunun nasıl yapılacağı gösterilmektedir.  

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

, Çözüm kaynaklarını kaynak grubunuzda sürekli olarak ve sürekli olarak kaldırmayı desteklemek için iki Azure Resource Manager şablonudur. Bunlardan biri, izlemeyi durduracak yapılandırmayı belirten bir JSON şablonudur ve diğeri, OpenShift kümesi kaynak KIMLIĞINI ve kümenin dağıtıldığı Azure bölgesini belirtmek için yapılandırdığınız parametre değerlerini içerir.

Bir şablon kullanarak kaynak dağıtma kavramı hakkında bilgi sahibi değilseniz, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/templates/deploy-powershell.md)
* [Kaynak Yöneticisi şablonları ve Azure CLı ile kaynak dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

Azure CLı 'yı kullanmayı seçerseniz, önce CLı 'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLı sürüm 2.0.65 veya üstünü çalıştırıyor olmanız gerekir. Sürümünüzü tanımlamak için öğesini çalıştırın `az --version`. Azure CLı 'yi yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 'Yı yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="create-template"></a>Şablon oluşturma

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aroResourceId": {
           "type": "string",
           "metadata": {
             "description": "ARO Cluster Resource ID"
          }
        },
        "aroResourceLocation": {
          "type": "string",
          "metadata": {
            "description": "Location of the aro cluster resource e.g. westcentralus"
          }
        }
      },
      "resources": [
        {
           "name": "[split(parameters('aroResourceId'),'/')[8]]",
           "type": "Microsoft.ContainerService/openShiftManagedClusters",
           "location": "[parameters('aroResourceLocation')]",
           "apiVersion": "2019-09-30-preview",
           "properties": {
             "mode": "Incremental",
             "id": "[parameters('aroResourceId')]",
             "monitorProfile": {
               "workspaceResourceID": null,
               "enabled": false
            }
          }
        }
      ]
    }
    ```

2. Bu dosyayı bir yerel klasöre **Optouttemplate. JSON** olarak kaydedin.

3. Aşağıdaki JSON sözdizimini dosyanıza yapıştırın:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aroResourceId": {
          "value": "/subscriptions/<subscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.ContainerService/openShiftManagedClusters/<clusterName>"
        },
        "aroResourceLocation": {
          "value": "<azure region of the cluster e.g. westcentralus>"
        }
      }
    }
    ```

4. **Aroresourceıd** ve **Aroresourcelocation** değerlerini, seçili kümenin **Özellikler** sayfasında bulabileceğiniz OpenShift kümesinin değerlerini kullanarak düzenleyin.

    ![Kapsayıcı özellikleri sayfası](media/container-insights-optout-openshift/cluster-properties-page.png)

5. Bu dosyayı bir yerel klasöre **Optoutparam. JSON** olarak kaydedin.

6. Bu şablonu dağıtmaya hazırsınız.

### <a name="remove-the-solution-using-azure-cli"></a>Azure CLı kullanarak çözümü kaldırma

Çözümü kaldırmak ve kümenizdeki yapılandırmayı temizlemek için Linux 'ta Azure CLı ile aşağıdaki komutu yürütün.

```azurecli
az login   
az account set --subscription "Subscription Name"
az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren aşağıdakine benzer bir ileti döndürülür:

```output
ProvisioningState       : Succeeded
```

### <a name="remove-the-solution-using-powershell"></a>PowerShell 'i kullanarak çözümü kaldırma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Çözümü kaldırmak ve yapılandırmayı kümenizdeki temizlemek için şablonu içeren klasörde aşağıdaki PowerShell komutlarını yürütün.    

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName <yourSubscriptionName>
New-AzResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren aşağıdakine benzer bir ileti döndürülür:

```output
ProvisioningState       : Succeeded
```

## <a name="next-steps"></a>Sonraki adımlar

Çalışma alanı yalnızca kümeyi izlemeyi desteklemek için oluşturulduysa ve artık gerekmiyorsa, el ile silmeniz gerekir. Bir çalışma alanının nasıl silineceği konusunda bilgi sahibi değilseniz bkz. [Azure Log Analytics çalışma alanını silme](../../log-analytics/log-analytics-manage-del-workspace.md).
