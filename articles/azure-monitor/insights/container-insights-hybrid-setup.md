---
title: Kapsayıcılar için Azure Monitörile Karma Kubernetes kümelerini yapılandırın | Microsoft Dokümanlar
description: Bu makalede, Azure Yığını veya diğer ortamda barındırılan Kubernetes kümelerini izlemek için kapsayıcılar için Azure Monitor'u nasıl yapılandırabileceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 01/24/2020
ms.openlocfilehash: 5a973e7e500906ebe833ec4cc6fd2fa8ee79c19e
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81255439"
---
# <a name="configure-hybrid-kubernetes-clusters-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Monitörü ile karma Kubernetes kümelerini yapılandırma

Kapsayıcılar için Azure Monitörü, Azure'da barındırılan kendi kendini yöneten bir Kubernetes kümesi olan [Azure'daki](https://github.com/Azure/aks-engine)Azure Kubernetes Hizmeti (AKS) ve AKS Engine için zengin bir izleme deneyimi sağlar. Bu makalede, Azure dışında barındırılan Kubernetes kümelerinin izlenmesinin nasıl etkinleştirilen ve benzer bir izleme deneyimi elde etmek için nasıl açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakileri yaptığınızdan emin olun:

* Log Analytics çalışma alanı.

    Kapsayıcılar için Azure Monitörü, bölgeye göre Azure [Ürünleri'nde](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor)listelenen bölgelerde ki Günlük Analizi çalışma alanını destekler. Kendi çalışma alanınızı oluşturmak için, [Azure Kaynak Yöneticisi](../platform/template-workspace-configuration.md)aracılığıyla , [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json)aracılığıyla veya [Azure portalında](../learn/quick-create-workspace.md)oluşturulabilir.

    >[!NOTE]
    >Aynı küme adı ile birden çok kümenin izlenmesini etkinleştirerek aynı Log Analytics çalışma alanına desteklenmez. Küme adları benzersiz olmalıdır.
    >

* Konteyner izlemeyi etkinleştirmek için **Log Analytics katılımcısı rolünün** bir üyesisiniz. Log Analytics çalışma alanına erişimi nasıl kontrol edebilirsiniz hakkında daha fazla bilgi için [bkz.](../platform/manage-access.md)

* [Belirtilen](https://helm.sh/docs/using_helm/) Kubernetes kümesi için kapsayıcılar grafiği için Azure Monitörü'ne binecek HELM istemcisi.

* Linux için Log Analytics aracısının Azure Monitor ile iletişim kurması için aşağıdaki proxy ve güvenlik duvarı yapılandırma bilgileri gereklidir:

    |Aracı Kaynağı|Bağlantı Noktaları |
    |------|---------|   
    |*.ods.opinsights.azure.com |Bağlantı Noktası 443 |  
    |*.oms.opinsights.azure.com |Bağlantı Noktası 443 |  
    |*.blob.core.windows.net |Bağlantı Noktası 443 |  
    |*.dc.services.visualstudio.com |Bağlantı Noktası 443 |

* Kapsayıcı aracı, performans ölçümlerini `cAdvisor secure port: 10250` toplamak `unsecure port :10255` için Kubelet'in veya kümedeki tüm düğümlerin açılmasını gerektirir. Kubelet'in `secure port: 10250` cAdvisor'ında daha önce yapılandırılmamışsa yapılandırmanızı öneririz.

* Kapsayıcı aracı, stok verilerini toplamak için küme içindeki Kubernetes API hizmetiyle iletişim kurabilmek için kapsayıcıda `KUBERNETES_SERVICE_HOST` `KUBERNETES_PORT_443_TCP_PORT`aşağıdaki çevresel değişkenlerin belirtilmesi gerekir ve .

>[!IMPORTANT]
>Hibrid Kubernetes kümelerinin izlenmesi için desteklenen minimum aracı sürümü ciprod10182019 veya sonraki sürümdür.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

Aşağıdakiler kapsayıcılar için Azure Monitor ile resmi olarak desteklenir.

- Ortamlar: Kubernetes şirket içi, Aks Engine on Azure ve Azure Yığını. Daha fazla bilgi için [Azure Yığını'ndaki AKS Engine'e](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908)bakın.
- Kubernetes sürümleri ve destek ilkesi [AKS desteklenen](../../aks/supported-kubernetes-versions.md)sürümleri ile aynıdır.
- Konteyner Çalışma Süresi: Docker ve Moby
- Master ve çalıştı düğümleri için Linux OS sürümü: Ubuntu (18,04 LTS ve 16,04 LTS)
- Erişim kontrolü desteklenir: Kubernetes RBAC ve RBAC olmayan

## <a name="enable-monitoring"></a>İzlemeyi etkinleştirme

Karma Kubernetes kümesi için kapsayıcılar için Azure Monitor'u etkinleştirmek, sırayla aşağıdaki adımları gerçekleştirmekten oluşur.

1. Log Analytics çalışma alanınızı Container Insights çözümüyle yapılandırın.

2. Log Analytics çalışma alanı ile kapsayıcıLAR HELM grafiği için Azure Monitörünü etkinleştirin.

### <a name="how-to-add-the-azure-monitor-containers-solution"></a>Azure Monitör Kapsayıcıları çözümü ekleme

Azure PowerShell cmdlet'ini `New-AzResourceGroupDeployment` kullanarak veya Azure CLI ile çözümü sağlanan Azure Kaynak Yöneticisi şablonuyla dağıtabilirsiniz.

Bir şablon kullanarak kaynakları dağıtma kavramına aşina değilseniz, bkz:

* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/templates/deploy-powershell.md)

* [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

Azure CLI'yi kullanmayı seçerseniz, öncelikle CLI'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLI sürümünü 2.0.59 veya sonraki sürümlerden çalışıyor olmalısınız. Sürümünüzü tanımlamak için `az --version`çalıştırın. Azure CLI'yi yüklemeniz veya yükseltmeniz gerekiyorsa, [bkz.](https://docs.microsoft.com/cli/azure/install-azure-cli)

Bu yöntem iki JSON şablonu içerir. Şablonlardan biri izlemeyi etkinleştirmek için yapılandırmayı belirtir ve diğeri aşağıdakileri belirtmek üzere yapılandırdığınız parametre değerlerini içerir:

- **workspaceResourceId** - Log Analytics çalışma alanınızın tam kaynak kimliği.
- **workspaceRegion** - Çalışma alanının oluşturulduğu bölge, Azure portalından görüntülenirken çalışma alanı özelliklerinde **Konum** olarak da adlandırılır.

Önce `workspaceResourceId` **ContainerSolutionParams.json** dosyasındaki parametre değeri için gerekli Log Analytics çalışma alanınızın tam kaynak kimliğini belirlemek için aşağıdaki adımları gerçekleştirin ve ardından çözümü eklemek için PowerShell cmdlet veya Azure CLI komutunu çalıştırın.

1. Aşağıdaki komutu kullanarak erişebildiğiniz tüm abonelikleri listele:

    ```azurecli
    az account list --all -o table
    ```

    Çıktı aşağıdakilere benzeyecektir:

    ```azurecli
    Name                                  CloudName    SubscriptionId                        State    IsDefault
    ------------------------------------  -----------  ------------------------------------  -------  -----------
    Microsoft Azure                       AzureCloud   68627f8c-91fO-4905-z48q-b032a81f8vy0  Enabled  True
    ```

    **SubscriptionId**değerini kopyalayın.

2. Aşağıdaki komutu kullanarak Log Analytics çalışma alanını barındıran aboneye geçin:

    ```azurecli
    az account set -s <subscriptionId of the workspace>
    ```

3. Aşağıdaki örnekte, aboneliklerinizdeki çalışma alanlarının listesi varsayılan JSON biçiminde görüntülenir.

    ```
    az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json
    ```

    Çıktıda, çalışma alanı adını bulun ve ardından alan **kimliği**altında bu Log Analytics çalışma alanının tam kaynak kimliğini kopyalayın.

4. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceResourceId": {
            "type": "string",
            "metadata": {
                "description": "Azure Monitor Log Analytics Workspace Resource ID"
            }
        },
        "workspaceRegion": {
            "type": "string",
            "metadata": {
                "description": "Azure Monitor Log Analytics Workspace region"
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "name": "[Concat('ContainerInsights', '-',  uniqueString(parameters('workspaceResourceId')))]",
            "apiVersion": "2017-05-10",
            "subscriptionId": "[split(parameters('workspaceResourceId'),'/')[2]]",
            "resourceGroup": "[split(parameters('workspaceResourceId'),'/')[4]]",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "apiVersion": "2015-11-01-preview",
                            "type": "Microsoft.OperationsManagement/solutions",
                            "location": "[parameters('workspaceRegion')]",
                            "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                            "properties": {
                                "workspaceResourceId": "[parameters('workspaceResourceId')]"
                            },
                            "plan": {
                                "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                                "product": "[Concat('OMSGallery/', 'ContainerInsights')]",
                                "promotionCode": "",
                                "publisher": "Microsoft"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
         }
      ]
   }
    ```

5. Bu dosyayı containerSolution.json olarak yerel bir klasöre kaydedin.

6. Aşağıdaki JSON sözdizimini dosyanıza yapıştırın:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "workspaceResourceId": {
          "value": "<workspaceResourceId>"
      },
      "workspaceRegion": {
        "value": "<workspaceRegion>"
      }
     }
    }
    ```

7. Adım 3'te kopyaladığınız değeri kullanarak **workspaceResourceId** değerlerini ve Azure CLI komutaz monitör günlük [analizi çalışma alanı gösterisini](https://docs.microsoft.com/cli/azure/monitor/log-analytics/workspace?view=azure-cli-latest#az-monitor-log-analytics-workspace-list)çalıştırdıktan sonra **çalışma alanıBölge** **değerini** kopyalayın.

8. Bu dosyayı containerSolutionParams.json olarak yerel bir klasöre kaydedin.

9. Bu şablonu dağıtmaya hazırsınız.

   * Azure PowerShell ile dağıtmak için şablonu içeren klasörde aşağıdaki komutları kullanın:

       ```powershell
       # configure and login to the cloud of log analytics workspace.Specify the corresponding cloud environment of your workspace to below command.
       Connect-AzureRmAccount -Environment <AzureCloud | AzureChinaCloud | AzureUSGovernment>
       ```

       ```powershell
       # set the context of the subscription of Log Analytics workspace
       Set-AzureRmContext -SubscriptionId <subscription Id of log analytics workspace>
       ```

       ```powershell
       # execute deployment command to add container insights solution to the specified Log Analytics workspace
       New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <resource group of log analytics workspace> -TemplateFile .\containerSolution.json -TemplateParameterFile .\containerSolutionParams.json
       ```

       Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakilere benzer ve sonucu içeren bir ileti görüntülenir:

       ```powershell
       provisioningState       : Succeeded
       ```

   * Azure CLI ile dağıtmak için aşağıdaki komutları çalıştırın:

       ```azurecli
       az login
       az account set --name <AzureCloud | AzureChinaCloud | AzureUSGovernment>
       az login
       az account set --subscription "Subscription Name"
       # execute deployment command to add container insights solution to the specified Log Analytics workspace
       az deployment group create --resource-group <resource group of log analytics workspace> --name <deployment name> --template-file  ./containerSolution.json --parameters @./containerSolutionParams.json
       ```

       Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakilere benzer ve sonucu içeren bir ileti görüntülenir:

       ```azurecli
       provisioningState       : Succeeded
       ```

       İzlemeyi etkinleştirdikten sonra, kümenin sistem durumu ölçümlerini görüntülemeniz yaklaşık 15 dakika sürebilir.

## <a name="install-the-chart"></a>Grafiği yükleme

HELM grafiğini etkinleştirmek için aşağıdakileri yapın:

1. Aşağıdaki komutu çalıştırarak Azure grafik deposunu yerel listenize ekleyin:

    ```
    helm repo add incubator https://kubernetes-charts-incubator.storage.googleapis.com/
    ````

2. Aşağıdaki komutu çalıştırarak grafiği yükleyin:

    ```
    $ helm install --name myrelease-1 \
    --set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<my_prod_cluster> incubator/azuremonitor-containers
    ```

    Log Analytics çalışma alanı Azure Çin'deyse aşağıdaki komutu çalıştırın:

    ```
    $ helm install --name myrelease-1 \
     --set omsagent.domain=opinsights.azure.cn,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
    ```

    Log Analytics çalışma alanı Azure ABD Hükümeti'ndeyse aşağıdaki komutu çalıştırın:

    ```
    $ helm install --name myrelease-1 \
    --set omsagent.domain=opinsights.azure.us,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
    ```

## <a name="configure-agent-data-collection"></a>Aracı veri toplamayı yapılandırma

Grafik sürüm 1.0.0 ile bakarken, aracı veri toplama ayarları ConfigMap'ten denetlenir. Aracı veri toplama [ayarları](container-insights-agent-config.md)yla ilgili belgelere buradan bakın.

Grafiği başarıyla dağıttıktan sonra, Azure portalındaki kapsayıcılar için Azure Monitor'daki karma Kubernetes kümenize ait verileri gözden geçirebilirsiniz.  

>[!NOTE]
>Yutma gecikmesi, Azure Log Analytics çalışma alanında işlemek için aracıdan yaklaşık beş ila on dakika dır. Kümenin durumu, gerekli tüm izleme verileri Azure Monitor'da kullanılabilir olana kadar **Veri Yok** veya **Bilinmiyor** değerini gösterir.

## <a name="troubleshooting"></a>Sorun giderme

Karma Kubernetes kümenizin izlemeyi etkinleştirmeye çalışırken bir hatayla karşılaşırsanız, PowerShell komut dosyasını [TroubleshootError_nonAzureK8s.ps1](https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/Troubleshoot/TroubleshootError_nonAzureK8s.ps1) kopyalayın ve bilgisayarınızdaki bir klasöre kaydedin. Bu komut dosyası, karşılaşılan sorunları algılamaya ve düzeltmeye yardımcı olmak için sağlanır. Düzeltmeyi algılamak ve düzeltmeye çalışmak için tasarladığı sorunlar şunlardır:

* Belirtilen Log Analytics çalışma alanı geçerlidir
* Log Analytics çalışma alanı, Kapsayıcılar için Azure Monitörü çözümüyle yapılandırılır. Değilse, çalışma alanını yapılandırın.
* OmsAgent çoğaltma kümesi bölmeleri çalışıyor
* OmsAgent daemonset pods çalışıyor
* OmsAgent Sağlık hizmeti çalışıyor
* Kapsayıcıaracı üzerinde yapılandırılan Log Analytics çalışma alanı kimliği ve anahtarı, Insight'ın yapılandırıldığı çalışma alanıyla eşleşir.
* Tüm Linux işçi düğümleri `kubernetes.io/role=agent` rs pod zamanlama etiketi var doğrulayın. Eğer yoksa, ekleyin.
* Kümedeki tüm düğümleri doğrulayın `cAdvisor secure port:10250` veya `unsecure port: 10255` açılır.

Azure PowerShell ile yürütmek için komut dosyasını içeren klasörde aşağıdaki komutları kullanın:

```powershell
.\TroubleshootError_nonAzureK8s.ps1 - azureLogAnalyticsWorkspaceResourceId </subscriptions/<subscriptionId>/resourceGroups/<resourcegroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName> -kubeConfig <kubeConfigFile> -clusterContextInKubeconfig <clusterContext>
```

## <a name="next-steps"></a>Sonraki adımlar

Hibrit Kubernetes kümenizin sistem durumu ve kaynak kullanımını ve üzerlerinde çalışan iş yüklerini toplamak için etkinleştirilen izleme sayesinde, kapsayıcılar için Azure [Monitörü'nü nasıl kullanacağınızı](container-insights-analyze.md) öğrenin.
