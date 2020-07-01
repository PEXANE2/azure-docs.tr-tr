---
title: Azure Resource Manager şablonu kullanarak Log Analytics çalışma alanına Azure etkinlik günlüğü gönderme
description: Bir Log Analytics çalışma alanı ve Azure Izleyici günlüklerine etkinlik günlüğü göndermek için bir tanılama ayarı oluşturmak için ARM şablonları kullanın.
ms.subservice: logs
ms.topic: quickstart
ms.custom: subject-armqs
author: bwren
ms.author: bwren
ms.date: 06/25/2020
ms.openlocfilehash: ce7c8df0fcea66d21ba2640ba26213a49efcb1c0
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85601658"
---
# <a name="quickstart-send-azure-activity-log-to-log-analytics-workspace-using-an-arm-template"></a>Hızlı başlangıç: ARM şablonunu kullanarak Log Analytics çalışma alanına Azure etkinlik günlüğü gönderme
Etkinlik günlüğü, Azure 'da abonelik düzeyindeki olaylara ilişkin Öngörüler sağlayan bir platform Günlüğliğidir. Bu, bir kaynağın değiştirildiği veya bir sanal makinenin başlatıldığı zaman gibi bilgileri içerir. Etkinlik günlüğünü Azure portal görüntüleyebilir veya PowerShell ve CLı ile girdileri alabilirsiniz. Bu hızlı başlangıçta, [günlük sorguları](../log-query/log-query-overview.md) ve [günlük uyarıları](../platform/alerts-log-query.md) ve [çalışma kitapları](../platform/workbooks-overview.md)gibi diğer özellikleri etkinleştirmek üzere bir Log Analytics çalışma alanı ve etkinlik günlüğünü Azure izleyici GÜNLÜKLERINE göndermek için Azure Resource Manager şablonlarının (ARM şablonları) nasıl kullanılacağı gösterilmektedir.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme
Aşağıdaki şablon boş bir Log Analytics çalışma alanı oluşturur. Bu şablonu *CreateWorkspace.js*olarak kaydedin.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "workspaceName": {
      "type": "string",
        "metadata": {
          "description": "Name of the workspace."
        }
    },
    "sku": {
      "type": "string",
      "allowedValues": [
        "pergb2018",
        "Free",
        "Standalone",
        "PerNode",
        "Standard",
        "Premium"
      ],
      "defaultValue": "pergb2018",
      "metadata": {
        "description": "Pricing tier: PerGB2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium) which are not available to all customers."
      }
    },
    "location": {
      "type": "string",
      "allowedValues": [
        "australiacentral",
        "australiaeast",
        "australiasoutheast",
        "brazilsouth",
        "canadacentral",
        "centralindia",
        "centralus",
        "eastasia",
        "eastus",
        "eastus2",
        "francecentral",
        "japaneast",
        "koreacentral",
        "northcentralus",
        "northeurope",
        "southafricanorth",
        "southcentralus",
        "southeastasia",
        "switzerlandnorth",
        "switzerlandwest",
        "uksouth",
        "ukwest",
        "westcentralus",
        "westeurope",
        "westus",
        "westus2"
      ],
      "metadata": {
        "description": "Specifies the location for the workspace."
      }
    },
    "retentionInDays": {
      "type": "int",
      "defaultValue": 120,
      "metadata": {
        "description": "Number of days to retain data."
      }
    },
    "resourcePermissions": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "true to use resource or workspace permissions. false to require workspace permissions."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.OperationalInsights/workspaces",
      "apiVersion": "2020-03-01-preview",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "properties": {
        "sku": {
          "name": "[parameters('sku')]"
        },
        "retentionInDays": "[parameters('retentionInDays')]",
        "features": {
          "searchVersion": 1,
          "legacy": 0,
          "enableLogAccessUsingOnlyResourcePermissions": "[parameters('resourcePermissions')]"
        }
      }
    }
  ]
}
```

Bu şablon bir kaynağı tanımlar:

- [Microsoft. Operationalınsights/çalışma alanları](/azure/templates/microsoft.operationalinsights/workspaces)

### <a name="deploy-the-template"></a>Şablonu dağıtma
CLı ve PowerShell kullanarak aşağıdaki örnekler gibi [BIR ARM şablonunu dağıtmak](../../azure-resource-manager/templates/deploy-portal.md) için herhangi bir standart yöntemi kullanarak şablonu dağıtın. **Kaynak grubu**, **çalışmaalanıadı**ve **konum** için örnek değerleri, ortamınız için uygun değerlerle değiştirin. Çalışma alanı adı tüm Azure abonelikleri arasında benzersiz olmalıdır.

# <a name="cli"></a>[CLI](#tab/CLI1)

```azurecli
az login
az deployment group create \
    --name CreateWorkspace \
    --resource-group my-resource-group \
    --template-file CreateWorkspace.json \
    --parameters workspaceName='my-workspace-01' location='eastus'

```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell1)

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName my-subscription
New-AzResourceGroupDeployment -Name AzureMonitorDeployment -ResourceGroupName my-resource-group -TemplateFile CreateWorkspace.json -workspaceName my-workspace-01 -location eastus
```

---

### <a name="validate-the-deployment"></a>Dağıtımı doğrulama
Çalışma alanının, aşağıdaki komutlardan birini kullanarak oluşturulduğunu doğrulayın. **Kaynak grubu** ve **çalışmaalanıadı** için örnek değerleri yukarıda kullandığınız değerlerle değiştirin.

# <a name="cli"></a>[CLI](#tab/CLI2)

```azurecli
az monitor log-analytics workspace show --resource-group my-workspace-01 --workspace-name my-resource-group
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell2)

```powershell
Get-AzOperationalInsightsWorkspace -Name my-workspace-01 -ResourceGroupName my-resource-group
```

---

## <a name="create-diagnostic-setting"></a>Tanılama ayarı oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme
Aşağıdaki şablon, bir Log Analytics çalışma alanına etkinlik günlüğü gönderen bir tanılama ayarı oluşturur. Bu şablonu *CreateDiagnosticSetting.js*olarak kaydedin.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "settingName": {
        "type": "String"
    },
    "workspaceId": {
        "type": "String"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/diagnosticSettings",
      "apiVersion": "2017-05-01-preview",
      "name": "[parameters('settingName')]",
      "dependsOn": [],
      "properties": {
        "workspaceId": "[parameters('workspaceId')]",
        "logs": [
          {
          "category": "Administrative",
          "enabled": true
          },
          {
          "category": "Alert",
          "enabled": true
          },
          {
          "category": "Autoscale",
          "enabled": true
          },
          {
          "category": "Policy",
          "enabled": true
          },
          {
          "category": "Recommendation",
          "enabled": true
          },
          {
          "category": "ResourceHealth",
          "enabled": true
          },
          {
          "category": "Security",
          "enabled": true
          },
          {
          "category": "ServiceHealth",
          "enabled": true
          }
        ]
      }
    }
  ]
}
```

Bu şablon bir kaynağı tanımlar:

- [Microsoft. Insights/diagnosticSettings](/azure/templates/microsoft.insights/diagnosticsettings)

### <a name="deploy-the-template"></a>Şablonu dağıtma
CLı ve PowerShell kullanarak aşağıdaki örnekler gibi [BIR ARM şablonunu dağıtmak](/azure-resource-manager/templates/deploy-portal) için herhangi bir standart yöntemi kullanarak şablonu dağıtın. **Kaynak grubu**, **çalışmaalanıadı**ve **konum** için örnek değerleri, ortamınız için uygun değerlerle değiştirin. Çalışma alanı adı tüm Azure abonelikleri arasında benzersiz olmalıdır.

# <a name="cli"></a>[CLI](#tab/CLI3)

```azurecli
az deployment sub create --name CreateDiagnosticSetting --location eastus --template-file CreateDiagnosticSetting.json --parameters settingName='Send Activity log to workspace' workspaceId='/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace-01'

```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell3)

```powershell
New-AzSubscriptionDeployment -Name CreateDiagnosticSetting -location eastus -TemplateFile CreateDiagnosticSetting.json -settingName="Send Activity log to workspace" -workspaceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace-01"
```
---

### <a name="validate-the-deployment"></a>Dağıtımı doğrulama
Aşağıdaki komutlardan birini kullanarak tanılama ayarının oluşturulduğunu doğrulayın. Abonelik ve ayar adı için örnek değerleri yukarıda kullandığınız değerlerle değiştirin.

> [!NOTE]
> Şu anda PowerShell kullanarak abonelik düzeyi tanılama ayarlarını alamazsınız.

```azurecli
az monitor diagnostic-settings show --resource '/subscriptions/00000000-0000-0000-0000-000000000000' --name 'Send Activity log to workspace'
```



## <a name="generate-log-data"></a>Günlük verileri oluştur
Log Analytics çalışma alanına yalnızca yeni etkinlik günlüğü girdileri gönderilir, bu nedenle aboneliğinizde bir sanal makineyi başlatma veya durdurma ya da başka bir kaynağı oluşturma veya değiştirme gibi günlüğe kaydedilecek bazı eylemler gerçekleştirin. Tanılama ayarının oluşturulması ve verilerin başlangıçta çalışma alanına yazılması için birkaç dakika beklemeniz gerekebilir. Bu gecikmeden sonra, etkinlik günlüğüne yazılan tüm olaylar çalışma alanına birkaç saniye içinde gönderilir.

## <a name="retrieve-data-with-a-log-query"></a>Günlük sorgusuyla veri alma
Çalışma alanından veri almak için Log Analytics kullanmak üzere Azure portal kullanın. Azure portal için arama yapın ve ardından **izleyici**' yi seçin. 

![Azure portal](media/quick-collect-activity-log/azure-portal-monitor.png)

**Azure izleyici** menüsünde **Günlükler** ' i seçin. **Örnek sorgular** sayfasını kapatın. Kapsam, oluşturduğunuz çalışma alanına ayarlanmamışsa **kapsam Seç** ' e tıklayın ve bulun.

![Log Analytics kapsamı](media/quick-collect-activity-log/log-analytics-scope.png)

Sorgu penceresinde, yazın `AzureActivity` ve **Çalıştır**' a tıklayın. Bu, etkinlik günlüğünden gönderilen tüm kayıtları içeren *AzureActivity* tablosundaki tüm kayıtları döndüren basit bir sorgudur.

![Basit sorgu](media/quick-collect-activity-log/query-01.png)

Ayrıntılı özelliklerini görüntülemek için kayıtlardan birini genişletin.

![Özellikleri Genişlet](media/quick-collect-activity-log/expand-properties.png)

`AzureActivity | summarize count() by CategoryValue`Kategoriye göre özetlenen olay sayısını veren gibi daha karmaşık bir sorgu deneyin.

![Karmaşık sorgu](media/quick-collect-activity-log/query-02.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmak isteyebilirsiniz. Artık gerekli değilse, uyarı kuralını ve ilgili kaynakları silen kaynak grubunu silin. Azure CLı veya Azure PowerShell kullanarak kaynak grubunu silmek için


 
# <a name="cli"></a>[CLI](#tab/CLI3)

```azurecli
az group delete --name my-resource-group
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell3)

```powershell
Remove-AzResourceGroup -Name my-resource-group
```

---

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, etkinlik günlüğünü bir Log Analytics çalışma alanına gönderilmek üzere yapılandırdınız. Artık diğer verileri, Azure Izleyici 'deki [günlük sorgularını](../log-query/log-query-overview.md) kullanarak analiz oluşturabileceğiniz ve [günlük uyarıları](../platform/alerts-log-query.md) ve [çalışma kitapları](../platform/workbooks-overview.md)gibi özelliklerden yararlanarak, çalışma alanına toplanacak şekilde yapılandırabilirsiniz. Daha sonra, her kaynakta gerçekleştirilen işlemlere ilişkin Öngörüler sağlayan etkinlik günlüğündeki verileri karmaşıklama Azure kaynaklarınızdan [kaynak günlüklerini](../platform/resource-logs.md) toplamanız gerekir.


> [!div class="nextstepaction"]
> [Azure Izleyici ile kaynak günlüklerini toplayın ve çözümleyin](tutorial-resource-logs.md)