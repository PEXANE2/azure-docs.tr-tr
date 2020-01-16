---
title: Azure Red Hat OpenShift kümenizi izlemeyi durdurma | Microsoft Docs
description: Bu makalede, Azure Red Hat OpenShift kümenizi kapsayıcılar için Azure Izleyici ile izlemeyi nasıl durdurulabileceğinizi açıklanmaktadır.
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: 97ca333f724dc4914dabda2912c4512a40520253
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75977778"
---
# <a name="how-to-stop-monitoring-your-azure-red-hat-openshift-cluster-with-azure-monitor-for-containers"></a>Azure Red Hat OpenShift kümenizi kapsayıcılar için Azure Izleyici ile izlemeyi durdurma

Azure Red Hat OpenShift kümenizi izlemeyi etkinleştirdikten sonra, artık bunu izlemek istememeye karar verirseniz kümeyi izlemeyi durdurabilirsiniz. Bu makalede, bunun nasıl yapılacağı, belirtilen Azure Resource Manager şablonları kullanılarak nasıl gerçekleştirileceğini gösterir.  

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Sağlanan olan iki çözüm kaynakları tutarlı ve sürekli kaynak grubunuzda kaldırma desteklemek için Azure Resource Manager şablonu. Bunlardan biri, izlemeyi durduracak yapılandırmayı belirten bir JSON şablonudur ve diğeri, OpenShift kümesi kaynak KIMLIĞINI ve kümenin dağıtıldığı Azure bölgesini belirtmek için yapılandırdığınız parametre değerlerini içerir.

Bir şablon kullanarak kaynakları dağıtma kavramıyla bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/templates/deploy-powershell.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLı sürüm 2.0.65 veya üstünü çalıştırıyor olmanız gerekir. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

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

2. Bu dosyayı farklı Kaydet **OptOutTemplate.json** yerel bir klasöre.

3. Aşağıdaki JSON söz dizimi dosyanıza yapıştırın:

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

    ![Kapsayıcı Özellikleri Sayfası](media/container-insights-optout-openshift/cluster-properties-page.png)

5. Bu dosyayı farklı Kaydet **OptOutParam.json** yerel bir klasöre.

6. Bu şablonu dağıtmaya hazırsınız.

### <a name="remove-the-solution-using-azure-cli"></a>Azure CLI kullanarak çözümünü Kaldır

Çözümü kaldırmak ve kümenizdeki yapılandırmayı temizlemek için Linux 'ta Azure CLı ile aşağıdaki komutu yürütün.

```azurecli
az login   
az account set --subscription "Subscription Name"
az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuçları içeren aşağıdakine benzer bir ileti döndürülür:

```azurecli
ProvisioningState       : Succeeded
```

### <a name="remove-the-solution-using-powershell"></a>PowerShell kullanarak çözümünü Kaldır

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Çözümü kaldırmak ve yapılandırmayı kümenizdeki temizlemek için şablonu içeren klasörde aşağıdaki PowerShell komutlarını yürütün.    

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName <yourSubscriptionName>
New-AzResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuçları içeren aşağıdakine benzer bir ileti döndürülür:

```powershell
ProvisioningState       : Succeeded
```

## <a name="next-steps"></a>Sonraki adımlar

Yalnızca küme İzleme'yi desteklemek için çalışma alanı oluşturuldu ve artık gerekli olmadığında, el ile silmeniz gerekir. Bir çalışma alanının nasıl silineceği konusunda bilgi sahibi değilseniz bkz. [Azure Log Analytics çalışma alanını silme](../../log-analytics/log-analytics-manage-del-workspace.md).
