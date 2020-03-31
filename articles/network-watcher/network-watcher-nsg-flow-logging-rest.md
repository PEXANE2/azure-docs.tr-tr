---
title: NSG akış günlüklerini yönetme - Azure REST API
titleSuffix: Azure Network Watcher
description: Bu sayfa, REST API ile Azure Ağ İzleyicisi'ndeki Ağ Güvenliği Grubu akış günlüklerinin nasıl yönetilenini açıklar
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 7cc47414dc985f6fc2fff3c57d809f307b142e30
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840936"
---
# <a name="configuring-network-security-group-flow-logs-using-rest-api"></a>REST API kullanarak Ağ Güvenlik Grubu akış günlüklerini yapılandırma

> [!div class="op_single_selector"]
> - [Azure portalında](network-watcher-nsg-flow-logging-portal.md)
> - [Powershell](network-watcher-nsg-flow-logging-powershell.md)
> - [Azure CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Ağ Güvenliği Grubu akış günlükleri, Ağ Güvenlik Grubu aracılığıyla giriş ve çıkış IP trafiği hakkındaki bilgileri görüntülemenize olanak tanıyan Ağ İzleyicisi'nin bir özelliğidir. Bu akış günlükleri json biçiminde yazılır ve kural bazında giden ve gelen akışları gösterir, akış için geçerli nic, akış hakkında 5-tuple bilgi (Kaynak/Hedef IP, Kaynak/Hedef Bağlantı Noktası, Protokol) ve trafiğe izin verilip verilmediğini veya reddedilmişse.

## <a name="before-you-begin"></a>Başlamadan önce

ARMclient PowerShell kullanarak REST API aramak için kullanılır. ARMClient [Chocolatey armclient](https://chocolatey.org/packages/ARMClient) çikolataüzerinde bulunur

Bu senaryo, Ağ İzleyicisi oluşturmak için [Ağ İzleyicisi Oluştur'daki](network-watcher-create.md) adımları zaten izlediğinizi varsayar.

> [!Important]
> Network Watcher REST API için istek URI kaynak grubu adı çağırır ağ watcher değil, üzerinde tanılama eylemleri gerçekleştiren kaynakları içeren kaynak grubudur.

## <a name="scenario"></a>Senaryo

Bu makalede kapsanan senaryo, REST API'sini kullanarak akış günlüklerini nasıl etkinleştirdiğinizi, devre dışı bırakabileceğinizi ve sorgu akışı günlüklerini nasıl etkinleştirdiğinizi gösterir. Ağ Güvenliği Grubu akış günlüğe kaydetmeleri hakkında daha fazla bilgi edinmek için [Ağ Güvenliği Grubu akış günlüğe kaydetme - Genel Bakış](network-watcher-nsg-flow-logging-overview.md)' ı ziyaret edin.

Bu senaryoda şunları yapacaksınız:

* Akış günlüklerini etkinleştirme (Sürüm 2)
* Akış günlüklerini devre dışı bırak
* Sorgu akışı günlükleri durumu

## <a name="log-in-with-armclient"></a>ARMClient ile giriş yapın

Azure kimlik bilgilerinizle birlikte silah istemcisi için oturum açın.

```powershell
armclient login
```

## <a name="register-insights-provider"></a>Insights sağlayıcısını kaydetme

Akış günlüğe kaydetmenin başarılı bir şekilde çalışabilmesi için **Microsoft.Insights** sağlayıcısının kaydedilmesi gerekir. **Microsoft.Insights** sağlayıcısının kayıtlı olup olmadığından emin değilseniz, aşağıdaki komut dosyasını çalıştırın.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
armclient post "https://management.azure.com//subscriptions/${subscriptionId}/providers/Microsoft.Insights/register?api-version=2016-09-01"
```

## <a name="enable-network-security-group-flow-logs"></a>Ağ Güvenlik Grubu akış günlüklerini etkinleştirme

Akış günlükleri sürüm 2 etkinleştirmek için komutu aşağıdaki örnekte gösterilir. Sürüm 1 için 'sürüm' alanını '1' ile değiştirin:

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'true',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        },
    'format': {
        'type': 'JSON',
        'version': 2
    }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

Önceki örnekten döndürülen yanıt aşağıdaki gibidir:

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
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
```

## <a name="disable-network-security-group-flow-logs"></a>Ağ Güvenlik Grubu akış günlüklerini devre dışı bırak

Akış günlüklerini devre dışı kakmak için aşağıdaki örneği kullanın. Arama, etkin özellik için **yanlış** ayarlanması dışında akış günlüklerini etkinleştirme yle aynıdır.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'false',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        },
    'format': {
        'type': 'JSON',
        'version': 2
    }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

Önceki örnekten döndürülen yanıt aşağıdaki gibidir:

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": false,
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
```

## <a name="query-flow-logs"></a>Sorgu akış günlükleri

Aşağıdaki REST çağrısı, Ağ Güvenlik Grubu'ndaki akış günlüklerinin durumunu sorgular.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/queryFlowLogStatus?api-version=2016-12-01" $requestBody
```

Döndürülen yanıtın bir örneği aşağıda verilmiştir:

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
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
```

## <a name="download-a-flow-log"></a>Akış günlüğü indirin

Akış günlüğünün depolama konumu oluşturmada tanımlanır. Bir depolama hesabına kaydedilen bu akış günlüklerine erişmek için kullanışlı bir araç, buradan indirilebilen Microsoft Azure Depolama Gezgini'dir:https://storageexplorer.com/

Bir depolama hesabı belirtilirse, paket yakalama dosyaları aşağıdaki konumdaki bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

## <a name="next-steps"></a>Sonraki adımlar

[PowerBI ile NSG akış günlüklerinizi nasıl görselleştirebilirsiniz](network-watcher-visualize-nsg-flow-logs-power-bi.md) öğrenin

[NSG akış günlüklerinizi açık kaynak araçlarıyla nasıl görselleştirebilirsiniz](network-watcher-visualize-nsg-flow-logs-open-source-tools.md) öğrenin
