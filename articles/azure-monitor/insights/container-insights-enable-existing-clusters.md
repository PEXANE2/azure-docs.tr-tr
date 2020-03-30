---
title: Dağıtılan bir Azure Kubernetes Hizmetini (AKS) kümesini izleme | Microsoft Dokümanlar
description: Aboneliğinizde zaten dağıtılan kapsayıcılar için Azure Monitor ile Azure Kubernetes Hizmeti (AKS) kümesinin izlenmesini nasıl etkinleştirin.
ms.topic: conceptual
ms.date: 09/12/2019
ms.openlocfilehash: 8589ea71b5c7affadc61d5e4543f734a660ab543
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275457"
---
# <a name="enable-monitoring-of-azure-kubernetes-service-aks-cluster-already-deployed"></a>Zaten dağıtılmış olan Azure Kubernetes Hizmeti (AKS) kümesinin izlenmesini etkinleştirin

Bu makalede, aboneliğinizde zaten dağıtılmış olan [Azure Kubernetes Hizmetinde](https://docs.microsoft.com/azure/aks/) barındırılan yönetilen Kubernetes kümesini izlemek için kapsayıcılar için Azure Monitörü nasıl ayarlanış ları açıklanmaktadır.

Desteklenen yöntemlerden birini kullanarak zaten dağıtılan bir AKS kümesinin izlenmesini etkinleştirebilirsiniz:

* Azure CLI
* Terraform
* [Azure Monitor'dan](#enable-from-azure-monitor-in-the-portal) veya doğrudan Azure [portalındaki AKS kümesinden](#enable-directly-from-aks-cluster-in-the-portal)
* Azure PowerShell cmdlet'i `New-AzResourceGroupDeployment` kullanarak veya Azure CLI ile sağlanan Azure Kaynak Yöneticisi [şablonu](#enable-using-an-azure-resource-manager-template) ile.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="enable-using-azure-cli"></a>Azure CLI'yi kullanarak etkinleştirme

Aşağıdaki adım, Azure CLI kullanarak AKS kümenizin izlenmesini sağlar. Bu örnekte, varolan bir çalışma alanını oluşturmanız veya belirtmeniz gerekmez. Bu komut, aks küme aboneliğinin varsayılan kaynak grubunda varsayılan bir çalışma alanı oluşturarak işlemi basitleştirir.  Oluşturulan varsayılan çalışma alanı Varsayılan *Çalışma alanı\<biçimini benzer- GUID>-\<Bölge>. *  

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG  
```

Çıktı aşağıdakilere benzeyecektir:

```output
provisioningState       : Succeeded
```

### <a name="integrate-with-an-existing-workspace"></a>Varolan bir çalışma alanıyla tümleştirme

Varolan bir çalışma alanıyla tümleştirmek isterseniz, önce `--workspace-resource-id` log analytics çalışma alanınızın parametre için gereken tam kaynak kimliğini belirlemek için aşağıdaki adımları gerçekleştirin ve ardından belirtilen çalışma alanına karşı izleme eklentisini etkinleştirmek için komutu çalıştırın.  

1. Aşağıdaki komutu kullanarak erişebildiğiniz tüm abonelikleri listele:

    ```azurecli
    az account list --all -o table
    ```

    Çıktı aşağıdakilere benzeyecektir:

    ```output
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

    ```azurecli
    az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json
    ```

    Çıktıda, çalışma alanı adını bulun ve ardından bu Log Analytics çalışma alanının tam kaynak kimliğini alan **kimliği**altında kopyalayın.

4. Parametre nin değerini değiştirerek izleme eklentisini etkinleştirmek için `--workspace-resource-id` aşağıdaki komutu çalıştırın. Dize değeri çift tırnak içinde olmalıdır:

    ```azurecli
    az aks enable-addons -a monitoring -n ExistingManagedCluster -g ExistingManagedClusterRG --workspace-resource-id "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>"
    ```

    Çıktı aşağıdakilere benzeyecektir:

    ```output
    provisioningState       : Succeeded
    ```

## <a name="enable-using-terraform"></a>Terraform kullanarak etkinleştirme

1. Oms_agent **eklenti** profilini varolan [azurerm_kubernetes_cluster kaynağına](https://www.terraform.io/docs/providers/azurerm/d/kubernetes_cluster.html#addon_profile) ekleme

   ```
   addon_profile {
    oms_agent {
      enabled                    = true
      log_analytics_workspace_id = "${azurerm_log_analytics_workspace.test.id}"
     }
   }
   ```

2. Terraform belgelerindeki adımları izleyerek [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) ekleyin.

## <a name="enable-from-azure-monitor-in-the-portal"></a>Portaldaki Azure Monitör'den etkinleştirme

Azure Portalı'ndaki AKS kümenizin Azure Monitörü'nden izlenmesini etkinleştirmek için aşağıdakileri yapın:

1. Azure portalında **Monitör'ü**seçin.

2. Listeden **Kapsayıcılar'ı** seçin.

3. **Monitörde - kapsayıcılar** sayfasında, **izlenmeyen kümeleri**seçin.

4. İzlenmeyen kümeler listesinden, listedeki kapsayıcıyı bulun ve **Etkinleştir'i**tıklatın.   

5. Kapsayıcılar için **Onboarding** to Azure Monitor sayfasında, kümeyle aynı abonelikte mevcut bir Log Analytics çalışma alanınız varsa, açılan listeden seçin.  
    Liste, AKS kapsayıcısının abonelikte dağıtılan varsayılan çalışma alanını ve konumunu önceden seçer.

    ![AKS Konteyner öngörülerini izleme yi etkinleştirin](./media/container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >İzleme verilerini kümeden depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız, [Log Analytics çalışma alanı oluştur'daki](../../azure-monitor/learn/quick-create-workspace.md)yönergeleri izleyin. AKS kapsayıcısının dağıtıldığı aynı abonelikte çalışma alanını oluşturduğunuzdan emin olun.

İzlemeyi etkinleştirdikten sonra, kümenin sistem durumu ölçümlerini görüntülemeniz yaklaşık 15 dakika sürebilir.

## <a name="enable-directly-from-aks-cluster-in-the-portal"></a>Portaldaki AKS kümesinden doğrudan etkinleştirme

Azure portalındaki AKS kümelerinizden doğrudan izleme yi etkinleştirmek için aşağıdakileri yapın:

1. Azure portalında **Tüm hizmetler'i**seçin.

2. Kaynaklar listesinde, **Kapsayıcılar**yazmaya başlayın.  Liste, girişinize göre filtreler.

3. **Kubernetes hizmetlerini**seçin.  

    ![Kubernetes hizmetleri bağlantısı](./media/container-insights-onboard/portal-search-containers-01.png)

4. Kapsayıcılar listesinde bir kapsayıcı seçin.

5. Kapsayıcıya genel bakış sayfasında, **Denetim Lisi'ni**seçin.  

6. Kapsayıcılar için **Onboarding** to Azure Monitor sayfasında, kümeyle aynı abonelikte mevcut bir Log Analytics çalışma alanınız varsa, açılan listede seçin.  
    Liste, AKS kapsayıcısının abonelikte dağıtılan varsayılan çalışma alanını ve konumunu önceden seçer.

    ![AKS konteyner sistem durumu izlemesini etkinleştirin](./media/container-insights-onboard/kubernetes-onboard-brownfield-02.png)

    >[!NOTE]
    >İzleme verilerini kümeden depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız, [Log Analytics çalışma alanı oluştur'daki](../../azure-monitor/learn/quick-create-workspace.md)yönergeleri izleyin. AKS kapsayıcısının dağıtıldığı aynı abonelikte çalışma alanını oluşturduğunuzdan emin olun.

İzlemeyi etkinleştirdikten sonra, kümenin operasyonel verilerini görüntüleyebiliyor olmak yaklaşık 15 dakika sürebilir.

## <a name="enable-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak etkinleştirme

Bu yöntem iki JSON şablonu içerir. Şablonlardan biri izlemeyi etkinleştirmek için yapılandırmayı belirtir ve diğeri aşağıdakileri belirtmek üzere yapılandırdığınız parametre değerlerini içerir:

* AKS kapsayıcı kaynak kimliği.
* Kümenin dağıtılan kaynak grubu.

>[!NOTE]
>Şablonun kümeyle aynı kaynak grubunda dağıtılması gerekir.
>

Azure PowerShell veya CLI kullanarak izlemeyi etkinleştirmeden önce Log Analytics çalışma alanının oluşturulması gerekir. Çalışma alanını oluşturmak için, azure [kaynak yöneticisi](../../azure-monitor/platform/template-workspace-configuration.md)aracılığıyla , [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json)üzerinden veya [Azure portalında](../../azure-monitor/learn/quick-create-workspace.md)ayarlayabilirsiniz.

Bir şablon kullanarak kaynakları dağıtma kavramına aşina değilseniz, bkz:

* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/templates/deploy-powershell.md)

* [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

Azure CLI'yi kullanmayı seçerseniz, öncelikle CLI'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLI sürümünü 2.0.59 veya sonraki sürümlerden çalışıyor olmalısınız. Sürümünüzü tanımlamak için `az --version`çalıştırın. Azure CLI'yi yüklemeniz veya yükseltmeniz gerekiyorsa, [bkz.](https://docs.microsoft.com/cli/azure/install-azure-cli)

### <a name="create-and-execute-a-template"></a>Şablon oluşturma ve yürütme

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
        },
        "workspaceResourceId": {
          "type": "string",
          "metadata": {
            "description": "Azure Monitor Log Analytics Resource ID"
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
                "enabled": true,
                "config": {
                  "logAnalyticsWorkspaceResourceID": "[parameters('workspaceResourceId')]"
                }
              }
            }
          }
        }
      ]
    }
    ```

2. Bu dosyayı **varolan ClusterOnboarding.json** olarak yerel bir klasöre kaydedin.

3. Aşağıdaki JSON sözdizimini dosyanıza yapıştırın:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
          "value": "/subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
        },
        "aksResourceLocation": {
          "value": "<aksClusterLocation>"
        },
        "workspaceResourceId": {
          "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
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

4. AKS kümesi için **AKS Genel Bakış** sayfasındaki değerleri kullanarak **aksResourceId** ve **aksResourceLocation** değerlerini düzenleyin. **WorkspaceResourceId** için değer, çalışma alanı adını içeren Log Analytics çalışma alanınızın tam kaynak kimliğidir.

    AKS kümesi için belirtilen varolan etiket değerleriyle eşleşecek **şekilde aksResourceTagValues** değerlerini düzenle.

5. Bu dosyayı **varolan ClusterParam.json** olarak yerel bir klasöre kaydedin.

6. Bu şablonu dağıtmaya hazırsınız.

   * Azure PowerShell ile dağıtmak için şablonu içeren klasörde aşağıdaki komutları kullanın:

       ```powershell
       New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
       ```

       Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakilere benzer ve sonucu içeren bir ileti görüntülenir:

       ```output
       provisioningState       : Succeeded
       ```

   * Azure CLI ile dağıtmak için aşağıdaki komutları çalıştırın:

       ```azurecli
       az login
       az account set --subscription "Subscription Name"
       az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
       ```

       Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakilere benzer ve sonucu içeren bir ileti görüntülenir:

       ```output
       provisioningState       : Succeeded
       ```

       İzlemeyi etkinleştirdikten sonra, kümenin sistem durumu ölçümlerini görüntülemeniz yaklaşık 15 dakika sürebilir.

## <a name="verify-agent-and-solution-deployment"></a>Aracıyı ve çözüm dağıtımını doğrula

Aracı sürümü *06072018* veya daha sonra, hem aracının hem de çözümün başarıyla dağıtıldığını doğrulayabilirsiniz. Aracının önceki sürümlerinde yalnızca aracı dağıtımını doğrulayabilirsiniz.

### <a name="agent-version-06072018-or-later"></a>Aracı sürümü 06072018 veya sonrası

Aracının başarıyla dağıtılmış olduğunu doğrulamak için aşağıdaki komutu çalıştırın.

```
kubectl get ds omsagent --namespace=kube-system
```

Çıktı, düzgün dağıtıldığını gösteren aşağıdakilere benzemelidir:

```output
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Çözümün dağıtımını doğrulamak için aşağıdaki komutu çalıştırın:

```
kubectl get deployment omsagent-rs -n=kube-system
```

Çıktı, düzgün dağıtıldığını gösteren aşağıdakilere benzemelidir:

```output
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>06072018'den önceki aracı sürümü

*06072018'den* önce yayımlanan Log Analytics aracısı sürümünün düzgün bir şekilde dağıtılmış olduğunu doğrulamak için aşağıdaki komutu çalıştırın:  

```
kubectl get ds omsagent --namespace=kube-system
```

Çıktı, düzgün dağıtıldığını gösteren aşağıdakilere benzemelidir:  

```output
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>CLI ile yapılandırmayı görüntüleme

Çözüm `aks show` etkin olup olmadığı, Log Analytics çalışma alanı kaynağı kimliği ve küme yle ilgili özet ayrıntıları gibi ayrıntıları almak için komutu kullanın.  

```azurecli
az aks show -g <resourceGroupofAKSCluster> -n <nameofAksCluster>
```

Birkaç dakika sonra komut tamamlar ve çözüm hakkında JSON biçimlendirilmiş bilgileri döndürür.  Komutun sonuçları izleme eklentiprofilini göstermeli ve aşağıdaki örnek çıktıya benzer:

```output
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```

## <a name="next-steps"></a>Sonraki adımlar

* Çözüme binmeye çalışırken sorunlarla karşılaşırsanız, [sorun giderme kılavuzunu](container-insights-troubleshoot.md) gözden geçirin

* AKS kümenizin sistem durumu ve kaynak kullanımını ve üzerlerinde çalışan iş yüklerini toplamak için etkinleştirilen izleme sayesinde, kapsayıcılar için Azure Monitor'u [nasıl kullanacağınızı](container-insights-analyze.md) öğrenin.
