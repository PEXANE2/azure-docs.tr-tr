---
title: Ağ İzleyicisi - Azure Kaynak Yöneticisi şablonu kullanarak NSG akış günlükleri oluşturma
description: NSG Akış Günlüklerini kolayca ayarlamak için Azure Kaynak Yöneticisi şablonu ve PowerShell kullanın.
services: network-watcher
documentationcenter: na
author: damendo
manager: twooley
editor: ''
tags: azure-resource-manager
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/26/2020
ms.author: damendo
ms.openlocfilehash: 35d185a625a81a259c366a45999769ecf76c6a7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77538167"
---
# <a name="configure-nsg-flow-logs-from-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonundan NSG Akış Günlüklerini yapılandırma

> [!div class="op_single_selector"]
> - [Azure portalında](network-watcher-nsg-flow-logging-portal.md)
> - [Powershell](network-watcher-nsg-flow-logging-powershell.md)
> - [Azure CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)
> - [Azure Resource Manager](network-watcher-nsg-flow-logging-azure-resource-manager.md)


[Azure Kaynak Yöneticisi,](https://azure.microsoft.com/features/resource-manager/) [Azure'un altyapınızı kod olarak](https://docs.microsoft.com/azure/devops/learn/what-is-infrastructure-as-code)yönetmenin yerel ve güçlü yoludur.

Bu makalede, Azure Kaynak Yöneticisi şablonu ve Azure PowerShell kullanarak [NSG Akış Günlüklerini](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) programlı olarak nasıl etkinleştirdiğinizgösterilmektedir. NSG Akış Günlüğü nesnesinin özelliklerine genel bir bakış sunarak başlıyoruz ve ardından birkaç örnek şablon uyguluyoruz. Daha sonra yerel bir PowerShell örneğini kullanarak dağıtma şablonu kullanırız.


## <a name="nsg-flow-logs-object"></a>NSG Akış Günlükleri nesnesi

Tüm parametreleri ile NSG Akış Günlükleri nesne aşağıda gösterilmiştir.
Özellikleri tam bir bakış için, [NSG Akış Günlükleri şablon başvuru](https://docs.microsoft.com/azure/templates/microsoft.network/2019-11-01/networkwatchers/flowlogs#RetentionPolicyParameters)okuyabilirsiniz.

```json
{
  "name": "string",
  "type": "Microsoft.Network/networkWatchers/flowLogs",
  "location": "string",
  "apiVersion": "2019-09-01",
  "properties": {
    "targetResourceId": "string",
    "storageId": "string",
    "enabled": "boolean",
    "flowAnalyticsConfiguration": {
      "networkWatcherFlowAnalyticsConfiguration": {
         "enabled": "boolean",
         "workspaceResourceId": "string",
          "trafficAnalyticsInterval": "integer"
        },
        "retentionPolicy": {
           "days": "integer",
           "enabled": "boolean"
         },
        "format": {
           "type": "string",
           "version": "integer"
         }
      }
    }
  }
```
Bir Microsoft.Network/networkWatchers/flowLogs kaynağı oluşturmak için yukarıdaki JSON'u şablonunuzun kaynaklar bölümüne ekleyin.


## <a name="creating-your-template"></a>Şablonunuzu oluşturma

Azure Kaynak Yöneticisi şablonlarını ilk kez kullanıyorsanız, aşağıdaki bağlantıları kullanarak bunlar hakkında daha fazla bilgi edinebilirsiniz.

* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-powershell#deploy-local-template)
* [Öğretici: İlk Azure Kaynak Yöneticisi şablonunuzu oluşturun ve dağıtın](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)


Aşağıda NSG Akış Günlükleri kurmak için tam şablonlar iki örnek verilmiştir.

**Örnek 1**: Yukarıdaki en basit versiyonu ile minimum parametreler geçilir. Aşağıdaki şablon, hedef NSG'deki NSG Akış Günlüklerini sağlar ve bunları belirli bir depolama hesabında saklar.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "apiProfile": "2019-09-01",
  "resources": [
 {
    "name": "NetworkWatcher_centraluseuap/Microsoft.NetworkDalanDemoPerimeterNSG",
    "type": "Microsoft.Network/networkWatchers/FlowLogs/",
    "location": "centraluseuap",
    "apiVersion": "2019-09-01",
    "properties": {
      "targetResourceId": "/subscriptions/56abfbd6-ec72-4ce9-831f-bc2b6f2c5505/resourceGroups/DalanDemo/providers/Microsoft.Network/networkSecurityGroups/PerimeterNSG",
      "storageId": "/subscriptions/56abfbd6-ec72-4ce9-831f-bc2b6f2c5505/resourceGroups/MyCanaryFlowLog/providers/Microsoft.Storage/storageAccounts/storagev2ira",
      "enabled": true,
      "flowAnalyticsConfiguration": {},
      "retentionPolicy": {},
      "format": {}
    }

  }
  ]
}
```

> [!NOTE]
> * Kaynak adı "Üst Kaynak>/Alt kaynak" biçimine sahiptir. Burada, ana kaynak bölgesel Ağ İzleyicisi<RegionName>örneğidir (Biçim: NetworkWatcher_. Örnek: NetworkWatcher_centraluseuap)
> * targetResourceId hedef NSG kaynak kimliğidir
> * storageId, hedef depolama hesabının kaynak kimliğidir

**Örnek 2**: NSG Akış Günlüklerini (sürüm 2) 5 gün bekletme ile etkinleştiren aşağıdaki şablonlar. Trafik Analitiğini 10 dakikalık bir işlem aralığıyla etkinleştirme.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "apiProfile": "2019-09-01",
  "resources": [
 {
    "name": "NetworkWatcher_centraluseuap/Microsoft.NetworkDalanDemoPerimeterNSG",
    "type": "Microsoft.Network/networkWatchers/FlowLogs/",
    "location": "centraluseuap",
    "apiVersion": "2019-09-01",
    "properties": {
      "targetResourceId": "/subscriptions/56abfbd6-ec72-4ce9-831f-bc2b6f2c5505/resourceGroups/DalanDemo/providers/Microsoft.Network/networkSecurityGroups/PerimeterNSG",
      "storageId": "/subscriptions/56abfbd6-ec72-4ce9-831f-bc2b6f2c5505/resourceGroups/MyCanaryFlowLog/providers/Microsoft.Storage/storageAccounts/storagev2ira",
      "enabled": true,
      "flowAnalyticsConfiguration": {
        "networkWatcherFlowAnalyticsConfiguration": {
            "enabled": true,
            "workspaceResourceId": "/subscriptions/56abfbd6-ec72-4ce9-831f-bc2b6f2c5505/resourceGroups/defaultresourcegroup-wcus/providers/Microsoft.OperationalInsights/workspaces/1c4f42e5-3a02-4146-ac9b-3051d8501db0",
            "trafficAnalyticsInterval": 10
                }
      },
      "retentionPolicy": {
        "days": 5,
        "enabled": true
      },
      "format": {
        "type": "JSON",
        "version": 2            
      }
    }

  }
  ]
}
```

## <a name="deploying-your-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonunuzu dağıtma

Bu öğretici, varolan bir Kaynak grubunuz ve Akış oturum açmayı etkinleştirebileceğiniz bir NSG'niz olduğunu varsayar.
Yukarıdaki örnek şablonlardan herhangi birini yerel `azuredeploy.json`olarak kaydedebilirsiniz. Özellik değerlerini, aboneliğinizdeki geçerli kaynakları işaret edebilecekleri şekilde güncelleştirin.

Şablonu dağıtmak için PowerShell'de aşağıdaki komutu çalıştırın.
```azurepowershell
New-AzResourceGroupDeployment -Name EnableFlowLog -ResourceGroupName NetworkWatcherRG `
    -TemplateFile "C:\MyTemplates\azuredeploy.json"
```


## <a name="verifying-your-deployment"></a>Dağıtımınızı doğrulama

Dağıtımınızın Başarılı olup olmadığını denetlemenin birkaç yolu vardır. PowerShell konsolunuz "ProvisioningState"i "Başarılı" olarak göstermelidir. Ayrıca, değişikliklerinizi onaylamak için [NSG Akış Günlükleri portal sayfasını](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs) ziyaret edebilirsiniz. Dağıtımla ilgili sorunlar varsa, Azure [Kaynak Yöneticisi ile sık karşılaşılan Azure dağıtım hatalarının giderilmesine](https://docs.microsoft.com/azure/azure-resource-manager/templates/common-deployment-errors)bir göz atın.


## <a name="next-steps"></a>Sonraki adımlar

NSG Akış verilerinizi nasıl görselleştirerek görüntüleyin:
* [Microsoft Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)
* [Açık kaynak araçları](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
* [Azure Trafik Analizi](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)
