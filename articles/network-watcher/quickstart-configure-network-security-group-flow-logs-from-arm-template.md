---
title: 'Hızlı başlangıç: Azure Resource Manager şablonu kullanarak NSG akış günlüklerini yapılandırma'
description: Azure Resource Manager şablonu (ARM şablonu) ve Azure PowerShell kullanarak NSG akış günlüklerinin nasıl etkinleştirileceğini öğrenin.
services: network-watcher
author: damendo
Customer intent: I need to enable the NSG flow logs using Azure Resource Manager Template
ms.service: network-watcher
ms.topic: quickstart
ms.date: 07/22/2020
ms.author: damendo
ms.custom: subject-armqs
ms.openlocfilehash: 96f30c05527754cbce3b7593c8d62fb56844d41e
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93042753"
---
# <a name="quickstart-configure-nsg-flow-logs-using-an-arm-template"></a>Hızlı başlangıç: bir ARM şablonu kullanarak NSG akış günlüklerini yapılandırma

Bu hızlı başlangıçta, bir [Azure Resource Manager](../azure-resource-manager/management/overview.md) şablonu (ARM şablonu) ve Azure PowerShell kullanarak [NSG akış günlüklerini](network-watcher-nsg-flow-logging-overview.md) etkinleştirirsiniz.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

NSG akış günlüğü nesnesinin özelliklerine genel bir bakış sunarak ve ardından birkaç örnek şablon tarafından başlayacağız. Ardından, şablonu yerel bir PowerShell örneği kullanarak dağıtın.

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-networkwatcher-flowLogs-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-networkwatcher-flowlogs-create) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-networkwatcher-flowlogs-create/azuredeploy.json":::

Şablonda birden çok kaynak tanımlanmıştır:

- [Microsoft. Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts)
- [Microsoft. resources/dağıtımlar](/azure/templates/microsoft.resources/deployments)

## <a name="nsg-flow-logs-object"></a>NSG akış günlükleri nesnesi

NSG akış günlüğü nesnesi tüm parametreleri aşağıda gösterilmiştir. Özelliklere yönelik kapsamlı bir genel bakış için bkz. [Microsoft. Network networkWatchers/flowLogs](/azure/templates/microsoft.network/networkwatchers/flowlogs).

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

Bir kaynak oluşturmak için `Microsoft.Network/networkWatchers/flowLogs` , YUKARıDAKI JSON 'ı şablonunuzun kaynaklar bölümüne ekleyin.

## <a name="creating-your-template"></a>Şablonunuz oluşturma

ARM şablonlarını ilk kez kullanıyorsanız, aşağıdaki bağlantıları kullanarak bunlarla ilgili daha fazla bilgi edinebilirsiniz.

- [ARM şablonları ve Azure PowerShell kaynak dağıtma](../azure-resource-manager/templates/deploy-powershell.md#deploy-local-template)
- [Öğretici: ilk ARM şablonunuzu oluşturma ve dağıtma](../azure-resource-manager/templates/template-tutorial-create-first-template.md)

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-networkwatcher-flowlogs-create) alınmıştır.

Tüm şablon örneği aşağıda, NSG akış günlüklerini ayarlamak için en az parametrelerin geçirildiği en basit sürümdür. Daha fazla örnek için bu [nasıl yapılır kılavuzuna](network-watcher-nsg-flow-logging-azure-resource-manager.md)bakın.

**Örnek** : aşağıdaki şablon, hedef NSG 'de NSG akış günlüklerini etkinleştirirler ve bunları belirli bir depolama hesabında depolar.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "apiProfile": "2019-09-01",
  "resources": [
    {
      "name": "NetworkWatcher_centraluseuap/Microsoft.NetworkDalanDemoPerimeterNSG",
      "type": "Microsoft.Network/networkWatchers/FlowLogs/",
      "location": "centraluseuap",
      "apiVersion": "2019-09-01",
      "properties": {
        "targetResourceId": "/subscriptions/<subscription Id>/resourceGroups/DalanDemo/providers/Microsoft.Network/networkSecurityGroups/PerimeterNSG",
        "storageId": "/subscriptions/<subscription Id>/resourceGroups/MyCanaryFlowLog/providers/Microsoft.Storage/storageAccounts/storagev2ira",
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
> - Kaynak adı, _üst Resource_Child_ biçimindeki bir kaynağa sahiptir. Burada, üst kaynak bölgesel ağ Izleyicisi örneğidir (Biçim: NetworkWatcher_RegionName. Örnek: NetworkWatcher_centraluseuap)
> - `targetResourceId` hedef NSG 'nin kaynak KIMLIĞIDIR.
> - `storageId` , hedef depolama hesabının kaynak KIMLIĞIDIR.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Bu öğreticide, akış günlüğünü etkinleştirmek için mevcut bir kaynak grubunuz ve bir NSG olduğunu varsaymaktadır.
Yukarıdaki örnek şablonlardan herhangi birini olarak yerel olarak kaydedebilirsiniz `azuredeploy.json` . Özellik değerlerini aboneliğinizdeki geçerli kaynaklara işaret eden bir şekilde güncelleştirin.

Şablonu dağıtmak için PowerShell 'de aşağıdaki komutu çalıştırın.

```azurepowershell-interactive
$context = Get-AzSubscription -SubscriptionId <subscription Id>
Set-AzContext $context
New-AzResourceGroupDeployment -Name EnableFlowLog -ResourceGroupName NetworkWatcherRG `
    -TemplateFile "C:\MyTemplates\azuredeploy.json"
```

> [!NOTE]
> Yukarıdaki komutlar, NSG 'yi içeren kaynak grubuna değil NetworkWatcherRG kaynak grubuna bir kaynak dağıtmakta

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Dağıtımınızın başarılı olup olmadığını denetlemek için birkaç yol vardır. PowerShell konsolunuzun olarak gösterilmesi `ProvisioningState` gerekir `Succeeded` . Ayrıca, değişikliklerinizi onaylamak için [NSG akış günlükleri portalı sayfasını](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs) ziyaret edebilirsiniz. Dağıtım ile ilgili sorunlar varsa bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarıyla Ilgili sorunları giderme](../azure-resource-manager/templates/common-deployment-errors.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure, dağıtım modu aracılığıyla kaynak silmeyi mümkün `Complete` . Bir akış günlükleri kaynağını silmek için, `Complete` silmek istediğiniz kaynağı dahil etmeden bir dağıtımı modda belirtin. [Dağıtım modundan tamamen](../azure-resource-manager/templates/deployment-modes.md#complete-mode)ilgili daha fazla bilgi edinin.

Alternatif olarak, aşağıdaki adımlar uyarınca Azure portal NSG akış günlüğünü devre dışı bırakabilirsiniz:

1. Azure portalda oturum açın
1. Portalın sol üst köşesinde **Tüm hizmetler** ’i seçin. **Filtre** kutusuna _Ağ İzleyicisi_ yazın. **Ağ İzleyicisi** , arama sonuçlarında görüntülendiğinde seçin.
1. **Günlükler** altında **NSG akış günlükleri** ' ni seçin.
1. NSG 'ler listesinden, akış günlüklerini devre dışı bırakmak istediğiniz NSG 'yi seçin.
1. **Akış günlükleri ayarları** altında akışlar günlük durumunu **kapalı** olarak ayarlayın.
1. Aşağı kaydırın ve **Kaydet** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta NSG akış günlüklerini etkinleştirdiniz. Şimdi, kullanarak NSG akış verilerinizi görselleştirmeyi öğrenmeniz gerekir:

- [Microsoft Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)
- [Açık kaynak araçları](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
- [Azure Trafik Analizi](traffic-analytics.md)
