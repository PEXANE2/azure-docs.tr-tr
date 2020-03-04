---
title: İzleme bilgisayarınızı Azure Kubernetes Hizmeti Durdur kümesine nasıl | Microsoft Docs
description: Bu makalede, Azure AKS kümenizi kapsayıcılar için Azure İzleyici ile izleme nasıl kesmek açıklanır.
ms.topic: conceptual
ms.date: 08/19/2019
ms.openlocfilehash: 618a4d7e10212dd2b042724b1ea11c97920dad57
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78250739"
---
# <a name="how-to-stop-monitoring-your-azure-kubernetes-service-aks-with-azure-monitor-for-containers"></a>Azure Kubernetes Service (AKS) kapsayıcıları için Azure İzleyici ile izleme durdurma

AKS kümenizi izleme etkinleştirdikten sonra artık bunu izlemek istediğiniz karar verirseniz küme izleme durdurabilirsiniz. Bu makalede, Azure CLI kullanarak bunu sağlamak gösterilmektedir veya sağlanan Azure Resource Manager şablonları ile.  


## <a name="azure-cli"></a>Azure CLI

Kapsayıcılar için Azure Izleyicisini devre dışı bırakmak için [az aks Disable-addons](https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-disable-addons) komutunu kullanın. Komut, aracıyı küme düğümlerinden kaldırır, çözümü veya daha önce toplanan ve Azure Izleyici kaynağınız içinde depolanan verileri kaldırmaz.  

```azurecli
az aks disable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG
```

Kümeniz için izlemeyi yeniden etkinleştirmek üzere bkz. [Azure CLI kullanarak Izlemeyi etkinleştirme](container-insights-enable-new-cluster.md#enable-using-azure-cli).

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Sağlanan olan iki çözüm kaynakları tutarlı ve sürekli kaynak grubunuzda kaldırma desteklemek için Azure Resource Manager şablonu. Bunlardan biri, izlemeyi durduracak yapılandırmayı belirten bir JSON şablonudur ve diğeri, kümenin dağıtıldığı AKS kümesi kaynak KIMLIĞINI ve kaynak grubunu belirtmek için yapılandırdığınız parametre değerlerini içerir.

Bir şablon kullanarak kaynakları dağıtma kavramıyla bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/templates/deploy-powershell.md)
* [Kaynak Yöneticisi şablonları ve Azure CLı ile kaynak dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

>[!NOTE]
>Şablonun, kümenin aynı kaynak grubunda dağıtılması gerekir. Bu şablonu kullanırken başka özellikleri veya eklentileri atlarsanız, kümeden kaldırılmasına neden olabilir. Örneğin, kümenizde uygulanan RBAC ilkeleri için *Enablertzya* ya da aks kümesi için Etiketler belirtilmişse, *Aksresourcetagvalues* .  
>

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için `az --version`çalıştırın. Azure CLı 'yi yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 'Yı yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="create-template"></a>Şablon oluşturma

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
           "type": "string",
           "metadata": {
             "description": "AKS Cluster Resource ID"
           }
       },
      "aksResourceLocation": {
        "type": "string",
        "metadata": {
           "description": "Location of the AKS resource e.g. \"East US\""
         }
       },
    "aksResourceTagValues": {
      "type": "object",
      "metadata": {
        "description": "Existing all tags on AKS Cluster Resource"
        }
      }
     },
    "resources": [
      {
        "name": "[split(parameters('aksResourceId'),'/')[8]]",
        "type": "Microsoft.ContainerService/managedClusters",
        "location": "[parameters('aksResourceLocation')]",
        "tags": "[parameters('aksResourceTagValues')]",
        "apiVersion": "2018-03-31",
        "properties": {
          "mode": "Incremental",
          "id": "[parameters('aksResourceId')]",
          "addonProfiles": {
            "omsagent": {
              "enabled": false,
              "config": null
            }
           }
         }
       }
      ]
    }
    ```

2. Bu dosyayı bir yerel klasöre **Optouttemplate. JSON** olarak kaydedin.

3. Aşağıdaki JSON söz dizimi dosyanıza yapıştırın:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
          "value": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
        },
        "aksResourceLocation": {
          "value": "<aksClusterRegion>"
        },
        "aksResourceTagValues": {
          "value": {
            "<existing-tag-name1>": "<existing-tag-value1>",
            "<existing-tag-name2>": "<existing-tag-value2>",
            "<existing-tag-nameN>": "<existing-tag-valueN>"
          }
        }
      }
    }
    ```

4. **Aksresourceıd** ve **Aksresourcelocation** değerlerini, seçilen kümenin **Özellikler** sayfasında bulabileceğiniz aks kümesinin değerlerini kullanarak düzenleyin.

    ![Kapsayıcı Özellikleri Sayfası](media/container-insights-optout/container-properties-page.png)

    **Özellikler** sayfasında, **çalışma alanı kaynak kimliğini**de kopyalayın. Daha sonra Log Analytics çalışma alanını silmek istediğinize karar verirseniz, bu değer gereklidir. Log Analytics çalışma alanı siliniyor, bu işlemin bir parçası olarak yapılmaz.

    **Aksresourcetagvalues** değerlerini, aks kümesi için belirtilen varolan etiket değerleriyle eşleşecek şekilde düzenleyin.

5. Bu dosyayı bir yerel klasöre **Optoutparam. JSON** olarak kaydedin.

6. Bu şablonu dağıtmaya hazırsınız.

### <a name="remove-the-solution-using-azure-cli"></a>Azure CLI kullanarak çözümünü Kaldır

Çözüm kaldırın ve AKS kümenizde yapılandırma temizlemek için Linux'ta Azure CLI ile aşağıdaki komutu yürütün.

```azurecli
az login   
az account set --subscription "Subscription Name"
az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuçları içeren aşağıdakine benzer bir ileti döndürülür:

```output
ProvisioningState       : Succeeded
```

### <a name="remove-the-solution-using-powershell"></a>PowerShell kullanarak çözümünü Kaldır

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Çözüm kaldırın ve AKS kümenizi yapılandırmasından temizlemek için şablonu içeren klasörde şu PowerShell komutlarını yürütün.    

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName <yourSubscriptionName>
New-AzResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuçları içeren aşağıdakine benzer bir ileti döndürülür:

```output
ProvisioningState       : Succeeded
```


## <a name="next-steps"></a>Sonraki adımlar

Yalnızca küme İzleme'yi desteklemek için çalışma alanı oluşturuldu ve artık gerekli olmadığında, el ile silmeniz gerekir. Bir çalışma alanının nasıl silineceği konusunda bilgi sahibi değilseniz, bkz. [Azure Portal bir Azure Log Analytics çalışma alanını silme](../../log-analytics/log-analytics-manage-del-workspace.md). 4\. adımda daha önce kopyalanmış **çalışma alanı kaynak kimliği** hakkında unutmayın, bunun için gerekli olacaktır.
