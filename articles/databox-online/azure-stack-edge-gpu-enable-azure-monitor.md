---
title: Azure Stack Edge Pro GPU cihazında Azure Izleyicisini etkinleştirme
description: Azure Stack Edge Pro GPU cihazında Azure Izleyici 'nin nasıl etkinleştirileceğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/02/2021
ms.author: alkohli
ms.openlocfilehash: 171b4dbfb2a5852e270c483a28cad31f97dcb42b
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99493921"
---
# <a name="enable-azure-monitor-on-your-azure-stack-edge-pro-gpu-device"></a>Azure Stack Edge Pro GPU cihazınızda Azure Izleyicisini etkinleştirme

Azure Stack Edge Pro GPU cihazındaki kapsayıcıları izlemek, özellikle birden çok işlem uygulaması çalıştırırken özel olarak kritik öneme sahiptir. Azure Izleyici, cihazınızda çalışan Kubernetes kümesinden kapsayıcı günlüklerini ve bellek ve işlemci ölçümlerini toplamanıza olanak tanır.

Bu makalede, cihazınızda Azure Izleyicisini etkinleştirmek ve Log Analytics çalışma alanında kapsayıcı günlüklerini toplamak için gereken adımlar açıklanmaktadır. Azure Izleyici ölçüm deposu, Azure Stack Edge Pro GPU cihazınız ile Şu anda desteklenmiyor. 


## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapmanız gerekir:

- Azure Stack Edge Pro cihazı. Öğreticideki adımları izleyerek cihazın etkinleştirildiğinden emin olun [: cihazınızı etkinleştirme](azure-stack-edge-gpu-deploy-activate.md).
- Öğreticiye göre **işlem adımını yapılandırmayı** tamamladınız: cihazınızda [Azure Stack Edge Pro cihazınızda işlem yapılandırma](azure-stack-edge-gpu-deploy-configure-compute.md) . Cihazınızın ilişkili bir IoT Hub kaynağına, IoT cihazına ve bir IoT Edge cihazına sahip olması gerekir.


## <a name="create-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

Bir Log Analytics çalışma alanı oluşturmak için aşağıdaki adımları uygulayın. Log Analytics çalışma alanı, günlük verilerinin toplandığı ve depolandığı bir mantıksal depolama birimidir.

1. Azure portal **+ kaynak oluştur** ' u seçin ve **Log Analytics çalışma alanı** araması yapın ve ardından **Oluştur**' u seçin. 
1. **Log Analytics oluştur çalışma** alanında aşağıdaki ayarları yapılandırın. Kalanı varsayılan olarak kabul edin.

    1. **Temel bilgiler** sekmesinde, çalışma alanı için abonelik, kaynak grubu, ad ve bölge bilgilerini girin. 

        ![Log Analytics çalışma alanı için temel bilgiler sekmesi](media/azure-stack-edge-gpu-enable-azure-monitor/create-log-analytics-workspace-basics-1.png)  

    1. **Fiyatlandırma katmanı** sekmesinde varsayılan **Kullandıkça Öde planını** kabul edin.

        ![Log Analytics çalışma alanı için fiyatlandırma sekmesi](media/azure-stack-edge-gpu-enable-azure-monitor/create-log-analytics-workspace-pricing-1.png) 

    1. **Gözden geçir + oluştur** sekmesinde çalışma alanınızın bilgilerini gözden geçirin ve **Oluştur**' u seçin.

        ![Log Analytics çalışma alanı için + oluşturmayı gözden geçirin](media/azure-stack-edge-gpu-enable-azure-monitor/create-log-analytics-workspace-review-create-1.png)

Daha fazla bilgi için [Azure Portal aracılığıyla Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-create-workspace.md)bölümündeki ayrıntılı adımlara bakın.



## <a name="enable-container-insights"></a>Kapsayıcı öngörülerini etkinleştir

Çalışma alanınızda kapsayıcı öngörülerini etkinleştirmek için aşağıdaki adımları uygulayın. 

1. [Azure Izleyici kapsayıcıları çözümünü ekleme](../azure-monitor/insights/container-insights-hybrid-setup.md#how-to-add-the-azure-monitor-containers-solution)bölümündeki ayrıntılı adımları izleyin. Aşağıdaki şablon dosyasını kullanın `containerSolution.json` :

    ```yml
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

1. Kaynak KIMLIĞINI ve konumunu alın. `Your Log Analytics workspace > General > Properties` öğesine gidin. Aşağıdaki bilgileri kopyalayın:

    - Azure Log Analytics çalışma alanının tam Azure Kaynak KIMLIĞI olan **kaynak kimliği**. 
    - Azure bölgesi olan **konum**.

    ![Log Analytics çalışma alanının özellikleri](media/azure-stack-edge-gpu-enable-azure-monitor/log-analytics-workspace-properties-1.png) 

1. Aşağıdaki parametreler dosyasını kullanın `containerSolutionParams.json` . `workspaceResourceId`Kaynak kimliğiyle ve `workspaceRegion` önceki adımda kopyalandığı konum ile değiştirin.

    ```yaml
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        "workspaceResourceId": {
            "value": "/subscriptions/fa68082f-8ff7-4a25-95c7-ce9da541242f/resourcegroups/myaserg/providers/microsoft.operationalinsights/workspaces/myaseloganalyticsws"
        },
        "workspaceRegion": {
        "value": "westus"
        }
        }
    }
    ```
    
    Kapsayıcı öngörüleri etkin olan bir Log Analytics çalışma alanının örnek çıktısı aşağıda verilmiştir:
    
    ```powershell
    Requesting a Cloud Shell.Succeeded.
    Connecting terminal...
    MOTD: Switch to Bash from PowerShell: bash
    VERBOSE: Authenticating to Azure ...
    VERBOSE: Building your Azure drive ...
    
    PS /home/alpa> az account set -s fa68082f-8ff7-4a25-95c7-ce9da541242f
    PS /home/alpa> ls
    clouddrive  containerSolution.json
    PS /home/alpa> ls
    clouddrive  containerSolution.json  containerSolutionParams.json
    PS /home/alpa> az deployment group create --resource-group myaserg --name Testdeployment1 --template-file containerSolution.json --parameters containerSolutionParams.json
    {- Finished ..
        "id": "/subscriptions/fa68082f-8ff7-4a25-95c7-ce9da541242f/resourceGroups/myaserg/providers/Microsoft.Resources/deployments/Testdeployment1",
        "location": null,
        "name": "Testdeployment1",
        "properties": {
        "correlationId": "3a9045fe-2de0-428c-b17b-057508a8c575",
        "debugSetting": null,
        "dependencies": [],
        "duration": "PT11.1588316S",
        "error": null,
        "mode": "Incremental",
        "onErrorDeployment": null,
        "outputResources": [
            {
            "id": "/subscriptions/fa68082f-8ff7-4a25-95c7-ce9da541242f/resourceGroups/myaserg/providers/Microsoft.OperationsManagement/solutions/ContainerInsights(myaseloganalyticsws)",
            "resourceGroup": "myaserg"
            }
        ],
        "outputs": null,
        "parameters": {
            "workspaceRegion": {
            "type": "String",
            "value": "westus"
            },
            "workspaceResourceId": {
            "type": "String",
            "value": "/subscriptions/fa68082f-8ff7-4a25-95c7-ce9da541242f/resourcegroups/myaserg/providers/microsoft.operationalinsights/workspaces/myaseloganalyticsws"
            }
        },
        "parametersLink": null,
        "providers": [
            {
            "id": null,
            "namespace": "Microsoft.Resources",
            "registrationPolicy": null,
            "registrationState": null,
            "resourceTypes": [
                {
                "aliases": null,
                "apiProfiles": null,
                "apiVersions": null,
                "capabilities": null,
                "defaultApiVersion": null,
                "locations": [
                    null
                ],
                "properties": null,
                "resourceType": "deployments"
                }
            ]
            }
        ],
        "provisioningState": "Succeeded",
        "templateHash": "10500027184662969395",
        "templateLink": null,
        "timestamp": "2020-11-06T22:09:56.908983+00:00",
        "validatedResources": null
        },
        "resourceGroup": "myaserg",
        "tags": null,
        "type": "Microsoft.Resources/deployments"
    }
    PS /home/alpa>
    ```

## <a name="configure-azure-monitor-on-your-device"></a>Cihazınızda Azure Izleyicisini yapılandırma

1. Yeni oluşturulan Log Analytics kaynağına gidin ve **çalışma alanı kimliğini** ve **birincil anahtarı** (çalışma alanı anahtarı) kopyalayın.

    ![Log Analytics çalışma alanında aracılar yönetimi](media/azure-stack-edge-gpu-enable-azure-monitor/log-analytics-workspace-agents-management-1.png)

    Daha sonraki bir adımda kullanacağınız gibi bu bilgileri kaydedin.

1. [Cihazın PowerShell arabirimine bağlanın](azure-stack-edge-gpu-connect-powershell-interface.md#connect-to-the-powershell-interface). 
    
1. Log Analytics çalışma alanı KIMLIĞI ve çalışma alanı anahtarını aşağıdaki cmdlet ile kullanın:

    `Set-HcsKubernetesAzureMonitorConfiguration -WorkspaceId <> -WorkspaceKey <>`

1. Azure Izleyici etkinleştirildikten sonra, Log Analytics çalışma alanında Günlükler görmeniz gerekir. Cihazınızda dağıtılan Kubernetes kümesinin durumunu görüntülemek için **Azure izleyici > öngörüler > kapsayıcıları**' na gidin. Ortam seçeneği için **Tümü**' nü seçin. 

    ![Log Analytics çalışma alanındaki ölçümler](media/azure-stack-edge-gpu-enable-azure-monitor/log-analytics-workspace-metrics-1.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Kubernetes çalışma yüklerini Kubernetes panosu aracılığıyla nasıl izleyeceğinizi](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md)öğrenin.
- [Cihaz olay uyarı bildirimlerini yönetmeyi](azure-stack-edge-gpu-manage-device-event-alert-notifications.md)öğrenin. 