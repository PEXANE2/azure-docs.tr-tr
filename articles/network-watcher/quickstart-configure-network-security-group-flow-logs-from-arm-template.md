---
title: 'Hızlı başlangıç: Azure Resource Manager şablonu kullanarak NSG akış günlüklerini yapılandırma'
description: Azure Resource Manager şablonu ve Azure PowerShell kullanarak el ile NSG akış günlüklerini etkinleştirmeyi öğrenin.
services: network-watcher
author: damendo
Customer intent: I need to enable the NSG Flow Logs using Azure Resource Manager Template
ms.service: network-watcher
ms.topic: quickstart
ms.date: 07/22/2020
ms.author: damendo
ms.custom: subject-armqs
ms.openlocfilehash: fd0f295fbc47a8e461885916dbe48f7604e182c3
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87854661"
---
# <a name="quickstart-configure-nsg-flow-logs-from-arm-template"></a>Hızlı başlangıç: NSG akış günlüklerini ARM şablonundan yapılandırma

Bu hızlı başlangıçta [NSG akış günlüklerini](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) bir [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) şablonu (ARM şablonu) ve Azure PowerShell kullanarak programlı bir şekilde etkinleştirirsiniz. 

NSG akış günlüğü nesnesinin özelliklerine genel bir bakış sunarak ve ardından birkaç örnek şablon tarafından başlayacağız. Ardından, şablonu yerel bir PowerShell örneği kullanarak dağıtın.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="nsg-flow-logs-object"></a>NSG akış günlükleri nesnesi

NSG akış günlüğü nesnesi tüm parametreleri aşağıda gösterilmiştir.
Özelliklere yönelik kapsamlı bir genel bakış için [NSG akış günlükleri şablon başvurusunu](https://docs.microsoft.com/azure/templates/microsoft.network/networkwatchers/flowlogs)okuyabilirsiniz.

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
Bir Microsoft. Network/networkWatchers/flowLogs kaynağı oluşturmak için, yukarıdaki JSON 'ı şablonunuzun kaynaklar bölümüne ekleyin.


## <a name="creating-your-template"></a>Şablonunuz oluşturma

Azure Resource Manager şablonlarını ilk kez kullanıyorsanız aşağıdaki bağlantıları kullanarak bunlarla ilgili daha fazla bilgi edinebilirsiniz.

* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-powershell#deploy-local-template)
* [Öğretici: ilk Azure Resource Manager şablonunuzu oluşturma ve dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)


Tüm şablon örneği aşağıda, NSG akış günlüklerini ayarlamak için en az parametrelerin geçirildiği en basit sürümdür. Daha fazla örnek için bu [bağlantıya](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-azure-resource-manager)gidin.

**Örnek**: aşağıdaki şablon, hedef NSG 'de NSG akış günlüklerini etkinleştirirler ve bunları belirli bir depolama hesabında depolar.

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
> * Kaynağın adı "üst Resource_Child kaynağı" biçimindedir. Burada, üst kaynak bölgesel ağ Izleyicisi örneğidir (Biçim: NetworkWatcher_RegionName. Örnek: NetworkWatcher_centraluseuap)
> * Targetresourceıd, hedef NSG 'nin kaynak KIMLIĞIDIR
> * Storageıd, hedef depolama hesabının kaynak KIMLIĞIDIR


## <a name="deploying-your-azure-resource-manager-template"></a>Azure Resource Manager şablonunuzu dağıtma

Bu öğreticide, akış günlüğünü etkinleştirmek için mevcut bir kaynak grubunuz ve bir NSG olduğunu varsaymaktadır.
Yukarıdaki örnek şablonlardan herhangi birini olarak yerel olarak kaydedebilirsiniz `azuredeploy.json` . Özellik değerlerini aboneliğinizdeki geçerli kaynaklara işaret eden bir şekilde güncelleştirin.

Şablonu dağıtmak için PowerShell 'de aşağıdaki komutu çalıştırın.
```azurepowershell-interactive
$context = Get-AzSubscription -SubscriptionId 56acfbd6-vc72-43e9-831f-bcdb6f2c5505
Set-AzContext $context
New-AzResourceGroupDeployment -Name EnableFlowLog -ResourceGroupName NetworkWatcherRG `
    -TemplateFile "C:\MyTemplates\azuredeploy.json"
```

> [!NOTE]
> Yukarıdaki komutlar, NSG 'yi içeren kaynak grubuna değil NetworkWatcherRG kaynak grubuna bir kaynak dağıtmakta


## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Dağıtımınızın başarılı olup olmadığını denetlemek için birkaç yol vardır. PowerShell konsolunuzun "başarılı" olarak "ProvisioningState" gösterilmesi gerekir. Ayrıca, değişikliklerinizi onaylamak için [NSG akış günlükleri portalı sayfasını](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs) ziyaret edebilirsiniz. Dağıtım ile ilgili sorunlar varsa, [Azure Resource Manager ile yaygın Azure dağıtım hatalarıyla Ilgili sorunları gidermeye](https://docs.microsoft.com/azure/azure-resource-manager/templates/common-deployment-errors)göz atın.

## <a name="deleting-your-resource"></a>Kaynağınız siliniyor
Azure, "tamamlanmış" dağıtım modundan kaynak silmeye izin vermez. Bir akış günlükleri kaynağını silmek için, silmek istediğiniz kaynağı dahil etmeden bir dağıtımı tamamlanmış modda belirtin. [Tüm dağıtım modu](https://docs.microsoft.com/azure/azure-resource-manager/templates/deployment-modes#complete-mode) hakkında daha fazla bilgi edinin

Alternatif olarak, aşağıdaki adımlar uyarınca Azure portal NSG akış günlüğünü devre dışı bırakabilirsiniz:
1. Azure portalda oturum açın
2. Portalın sol üst köşesinde **Tüm hizmetler**’i seçin. **Filtre** kutusuna *Ağ İzleyicisi* yazın. **Ağ İzleyicisi**, arama sonuçlarında görüntülendiğinde seçin.
3. **Günlükler**altında **NSG akış günlükleri** ' ni seçin.
4. NSG 'ler listesinden, akış günlüklerini devre dışı bırakmak istediğiniz NSG 'yi seçin
5. **Akış günlükleri ayarları**altında akışlar günlük durumunu **kapalı** olarak ayarlayın
6. Aşağı kaydırın ve **Kaydet** ' i seçin

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta NSG akış günlüklerini etkinleştirdiniz. Şimdi, kullanarak NSG akış verilerinizi görselleştirmeyi öğrenmeniz gerekir: 

* [Microsoft Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)
* [Açık kaynak araçları](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
* [Azure Trafik Analizi](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)
