---
title: Azure Red Hat OpenShift kümenizi izlemeyi nasıl durdurun | Microsoft Dokümanlar
description: Bu makalede, kapsayıcılar için Azure Monitor ile Azure Kırmızı Şapka OpenShift kümenizi izlemeyi nasıl durdurabileceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: eff5203aeedd3c7ad283b55ba12f0e45a556460d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78250728"
---
# <a name="how-to-stop-monitoring-your-azure-red-hat-openshift-cluster-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Monitor ile Azure Red Hat OpenShift kümenizi izlemeyi durdurma

Azure Red Hat OpenShift kümenizin izlenmesini etkinleştirdikten sonra, artık izlemek istemediğinize karar verirseniz kümeyi izlemeyi durdurabilirsiniz. Bu makalede, sağlanan Azure Kaynak Yöneticisi şablonlarını kullanarak bunu nasıl başarılacağI gösterilmektedir.  

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Sağlanan, çözüm kaynaklarının kaynak grubunuzda tutarlı ve sürekli olarak kaldırılmasını destekleyen iki Azure Kaynak Yöneticisi şablonu sunulur. Bunlardan biri, izlemeyi durdurmak için yapılandırmayı belirten bir JSON şablonu, diğeri ise kümenin dağıtıldığı OpenShift küme kaynak kimliğini ve Azure bölgesini belirtmek için yapılandırdığınız parametre değerlerini içerir.

Şablon kullanarak kaynakları dağıtma kavramına aşina değilseniz, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/templates/deploy-powershell.md)
* [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

Azure CLI'yi kullanmayı seçerseniz, öncelikle CLI'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLI sürümünü 2.0.65 veya sonraki sürümde çalışıyor olmalısınız. Sürümünüzü tanımlamak için `az --version`çalıştırın. Azure CLI'yi yüklemeniz veya yükseltmeniz gerekiyorsa, [bkz.](https://docs.microsoft.com/cli/azure/install-azure-cli)

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

2. Bu dosyayı **OptOutTemplate.json** olarak yerel bir klasöre kaydedin.

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

4. Seçili kümenin **Özellikleri** sayfasında bulabileceğiniz OpenShift kümesinin değerlerini kullanarak **aroResourceId** ve **aroResourceLocation** değerlerini düzenleme.

    ![Kapsayıcı özellikleri sayfası](media/container-insights-optout-openshift/cluster-properties-page.png)

5. Bu dosyayı **OptOutParam.json** olarak yerel bir klasöre kaydedin.

6. Bu şablonu dağıtmaya hazırsınız.

### <a name="remove-the-solution-using-azure-cli"></a>Azure CLI'yi kullanarak çözümü kaldırma

Çözümü kaldırmak ve kümenizdeki yapılandırmayı temizlemek için Linux'ta Azure CLI ile aşağıdaki komutu uygulayın.

```azurecli
az login   
az account set --subscription "Subscription Name"
az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren aşağıdakine benzer bir ileti döndürülür:

```output
ProvisioningState       : Succeeded
```

### <a name="remove-the-solution-using-powershell"></a>PowerShell kullanarak çözümü kaldırın

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Çözümü kaldırmak ve yapılandırmayı kümenizden temizlemek için şablonu içeren klasörde aşağıdaki PowerShell komutlarını uygulayın.    

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

Çalışma alanı yalnızca kümenin izlenmesini desteklemek için oluşturulduysa ve artık gerek yoksa, el ile silmeniz gerekir. Çalışma alanını nasıl sildiğinizi bilmiyorsanız, [bkz.](../../log-analytics/log-analytics-manage-del-workspace.md)
