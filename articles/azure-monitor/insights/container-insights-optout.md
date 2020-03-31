---
title: Azure Kubernetes Hizmet kümenizi İzlemeyi Nasıl Durdurun | Microsoft Dokümanlar
description: Bu makalede, kapsayıcılar için Azure Monitor ile Azure AKS kümenizin izlemeyi nasıl durdurabileceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 08/19/2019
ms.openlocfilehash: 618a4d7e10212dd2b042724b1ea11c97920dad57
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275262"
---
# <a name="how-to-stop-monitoring-your-azure-kubernetes-service-aks-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici ile Azure Kubernetes Service (AKS) ortamınızı izlemeyi durdurma

AKS kümenizin izlenmesini etkinleştirdikten sonra, artık izlemek istemediğinize karar verirseniz kümeyi izlemeyi durdurabilirsiniz. Bu makalede, Azure CLI'yi kullanarak veya sağlanan Azure Kaynak Yöneticisi şablonlarıyla bunu nasıl başarılalıştırılacak gösterilmektedir.  


## <a name="azure-cli"></a>Azure CLI

Kapsayıcılar için Azure Monitor'u devre dışı katmak için [az aks devre dışı-eklentiler](https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-disable-addons) komutunu kullanın. Komut aracıyı küme düğümlerinden kaldırır, çözümü veya Azure Monitor kaynağınızda toplanıp depolanmış verileri kaldırmaz.  

```azurecli
az aks disable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG
```

Kümenizin izlemeyi yeniden etkinleştirmek için [bkz.](container-insights-enable-new-cluster.md#enable-using-azure-cli)

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Sağlanan, çözüm kaynaklarının kaynak grubunuzda tutarlı ve sürekli olarak kaldırılmasını destekleyen iki Azure Kaynak Yöneticisi şablonu sunulur. Bunlardan biri, izlemeyi durdurmak için yapılandırmayı belirten bir JSON şablonudur ve diğeri de kümenin dağıtıldığı AKS küme kaynak kimliğini ve kaynak grubunu belirtmek için yapılandırdığınız parametre değerlerini içerir.

Şablon kullanarak kaynakları dağıtma kavramına aşina değilseniz, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/templates/deploy-powershell.md)
* [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

>[!NOTE]
>Şablonun kümenin aynı kaynak grubunda dağıtılması gerekir. Bu şablonu kullanırken başka özellikleri veya eklentileri atlarsanız, kümeden kaldırılmasına neden olabilir. Örneğin, kümenizde uygulanan RBAC ilkeleri için *etkinleştiriciRBAC* veya AKS kümesi için etiketler belirtilmişse *aksResourceTagValues.*  
>

Azure CLI'yi kullanmayı seçerseniz, öncelikle CLI'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLI sürümünü 2.0.27 veya sonraki sürümde çalışıyor olmalısınız. Sürümünüzü tanımlamak için `az --version`çalıştırın. Azure CLI'yi yüklemeniz veya yükseltmeniz gerekiyorsa, [bkz.](https://docs.microsoft.com/cli/azure/install-azure-cli)

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

2. Bu dosyayı **OptOutTemplate.json** olarak yerel bir klasöre kaydedin.

3. Aşağıdaki JSON sözdizimini dosyanıza yapıştırın:

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

4. Seçili kümenin **Özellikleri** sayfasında bulabileceğiniz AKS kümesinin değerlerini kullanarak **aksResourceId** ve **aksResourceLocation** değerlerini düzenleyin.

    ![Kapsayıcı özellikleri sayfası](media/container-insights-optout/container-properties-page.png)

    **Özellikler** sayfasındayken, **Çalışma Alanı Kaynak Kimliğini**de kopyalayın. Günlük Analizi çalışma alanını daha sonra silmeye karar verirseniz, bu değer gereklidir. Log Analytics çalışma alanı bu işlemin bir parçası olarak gerçekleştirilmez.

    AKS kümesi için belirtilen varolan etiket değerleriyle eşleşecek **şekilde aksResourceTagValues** değerlerini düzenle.

5. Bu dosyayı **OptOutParam.json** olarak yerel bir klasöre kaydedin.

6. Bu şablonu dağıtmaya hazırsınız.

### <a name="remove-the-solution-using-azure-cli"></a>Azure CLI'yi kullanarak çözümü kaldırma

Çözümü kaldırmak ve AKS kümenizdeki yapılandırmayı temizlemek için Linux'ta Azure CLI ile aşağıdaki komutu uygulayın.

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

Çözümü kaldırmak ve yapılandırmayı AKS kümenizden temizlemek için şablonu içeren klasörde aşağıdaki PowerShell komutlarını uygulayın.    

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

Çalışma alanı yalnızca kümenin izlenmesini desteklemek için oluşturulduysa ve artık gerek yoksa, el ile silmeniz gerekir. Çalışma alanını nasıl sileceksiniz hakkında bilginiz yoksa, [bkz.](../../log-analytics/log-analytics-manage-del-workspace.md) 4. adımda daha önce kopyalanan **Çalışma Alanı Kaynak Kimliğini** unutmayın, buna ihtiyacınız olacak.
