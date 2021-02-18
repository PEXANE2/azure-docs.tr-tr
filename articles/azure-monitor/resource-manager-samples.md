---
title: Azure Izleyici için Kaynak Yöneticisi şablonu örnekleri
description: Kaynak Yöneticisi şablonları kullanarak Azure Izleyici özelliklerini dağıtma ve yapılandırma
author: bwren
ms.author: bwren
services: azure-monitor
ms.topic: sample
ms.date: 05/18/2020
ms.subservice: ''
ms.openlocfilehash: 0791ccf10c76f2a1781bf373c674f606ca365fff
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100628380"
---
# <a name="resource-manager-template-samples-for-azure-monitor"></a>Azure Izleyici için Kaynak Yöneticisi şablonu örnekleri

Azure Izleyici, [Azure Resource Manager şablonu](../azure-resource-manager/templates/template-syntax.md)kullanılarak ölçeklendirilebilir şekilde dağıtılabilir ve yapılandırılabilir. Aşağıdaki makalelerde, farklı Azure Izleyici özelliklerine yönelik örnek şablonlar sağlanmaktadır. Bu örnekler, belirli gereksinimleriniz için değiştirilebilir ve Kaynak Yöneticisi şablonları dağıtmak için herhangi bir standart yöntem kullanılarak dağıtılabilir. 

## <a name="deploying-the-sample-templates"></a>Örnek şablonları dağıtma
Örnekleri kullanmak için temel adımlar şunlardır:

1. Şablonu kopyalayın ve bir JSON dosyası olarak kaydedin.
2. Ortamınızın parametrelerini değiştirin ve bir JSON dosyası olarak kaydedin.
4. [Kaynak Yöneticisi şablonlar için herhangi bir dağıtım yöntemini](../azure-resource-manager/templates/deploy-powershell.md)kullanarak şablonu dağıtın. 

Örneğin, PowerShell veya Azure CLı kullanarak bir kaynak grubuna şablon ve parametre dosyasını dağıtmak için aşağıdaki komutları kullanın.


```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName my-subscription
New-AzResourceGroupDeployment -Name AzureMonitorDeployment -ResourceGroupName my-resource-group -TemplateFile azure-monitor-deploy.json -TemplateParameterFile azure-monitor-deploy.parameters.json
```

```azurecli
az login
az deployment group create \
    --name AzureMonitorDeployment \
    --resource-group ResourceGroupofTargetResource \
    --template-file azure-monitor-deploy.json \
    --parameters azure-monitor-deploy.parameters.json
```

## <a name="list-of-sample-templates"></a>Örnek şablonların listesi

- [Aracılar](agents/resource-manager-agent.md) -Log Analytics Aracısı ve tanılama uzantısı dağıtın ve yapılandırın.
- Uyarılar
  - [Uyarı kurallarını günlüğe kaydet](alerts/resource-manager-alerts-log.md) -günlük sorgularından ve Azure etkinlik günlüğünden uyarılar.
  - [Ölçüm uyarı kuralları](alerts/resource-manager-alerts-metric.md) -farklı mantık türleri kullanılarak ölçümlerin uyarıları.
- [Uygulama Bilgileri](app/resource-manager-app-resource.md)
- [Tanılama ayarları](essentials/resource-manager-diagnostic-settings.md) -farklı kaynak türlerindeki günlükleri ve ölçümleri iletmek için Tanılama ayarları oluşturun.
- [Günlük sorguları](logs/resource-manager-log-queries.md) -kaydedilmiş günlük sorgularını Log Analytics çalışma alanında oluşturun.
- [Log Analytics çalışma alanı](logs/resource-manager-workspace.md) -Log Analytics aracılarından Log Analytics çalışma alanı oluşturun ve farklı veri kaynakları koleksiyonunu yapılandırın.
- [Çalışma kitapları](visualize/resource-manager-workbooks.md) -çalışma kitapları oluşturma.
- [Kapsayıcılar Için Azure izleyici](containers/resource-manager-container-insights.md) -kapsayıcılar Için Azure izleyicisine kümeler ekleyin.
- [VM'ler için Azure izleyici](vm/resource-manager-vminsights.md) sanal makineler VM'ler için Azure izleyici.



## <a name="next-steps"></a>Sonraki adımlar

- [Kaynak Yöneticisi şablonları](../azure-resource-manager/templates/overview.md) hakkında daha fazla bilgi edinin