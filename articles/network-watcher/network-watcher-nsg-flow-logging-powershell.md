---
title: NSG Akış günlüklerini yönetme - Azure PowerShell
titleSuffix: Azure Network Watcher
description: Bu sayfa, PowerShell ile Azure Ağ İzleyicisi'ndeki Ağ Güvenliği Grubu Akışı günlüklerinin nasıl yönetilenini açıklar
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
ms.openlocfilehash: 9612afdb63c6988c0027f003caeacd456b5e50e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840953"
---
# <a name="configuring-network-security-group-flow-logs-with-powershell"></a>PowerShell ile Ağ Güvenlik Grubu Akış günlüklerini yapılandırma

> [!div class="op_single_selector"]
> - [Azure portalında](network-watcher-nsg-flow-logging-portal.md)
> - [Powershell](network-watcher-nsg-flow-logging-powershell.md)
> - [Azure CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Ağ Güvenliği Grubu akış günlükleri, Ağ Güvenlik Grubu aracılığıyla giriş ve çıkış IP trafiği hakkındaki bilgileri görüntülemenize olanak tanıyan Ağ İzleyicisi'nin bir özelliğidir. Bu akış günlükleri json biçiminde yazılır ve kural bazında giden ve gelen akışları gösterir, akış için geçerli nic, akış hakkında 5-tuple bilgi (Kaynak/Hedef IP, Kaynak/Hedef Bağlantı Noktası, Protokol) ve trafiğe izin verilip verilmediğini veya reddedilmişse.

## <a name="register-insights-provider"></a>Insights sağlayıcısını kaydetme

Akış günlüğe kaydetmenin başarılı bir şekilde çalışabilmesi için **Microsoft.Insights** sağlayıcısının kaydedilmesi gerekir. **Microsoft.Insights** sağlayıcısının kayıtlı olup olmadığından emin değilseniz, aşağıdaki komut dosyasını çalıştırın.

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs-and-traffic-analytics"></a>Ağ Güvenliği Grup Akış günlüklerini ve Trafik Analizini etkinleştirme

Akış günlüklerini etkinleştirme komutu aşağıdaki örnekte gösterilmiştir:

```powershell
$NW = Get-AzNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzNetworkSecurityGroup -ResourceGroupName nsgRG -Name nsgName
$storageAccount = Get-AzStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id

#Traffic Analytics Parameters
$workspaceResourceId = "/subscriptions/bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb/resourcegroups/trafficanalyticsrg/providers/microsoft.operationalinsights/workspaces/taworkspace"
$workspaceGUID = "cccccccc-cccc-cccc-cccc-cccccccccccc"
$workspaceLocation = "westeurope"

#Configure Version 1 Flow Logs
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 1

#Configure Version 2 Flow Logs, and configure Traffic Analytics
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2

#Configure Version 2 FLow Logs with Traffic Analytics Configured
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2 -EnableTrafficAnalytics -WorkspaceResourceId $workspaceResourceId -WorkspaceGUID $workspaceGUID -WorkspaceLocation $workspaceLocation

#Query Flow Log Status
Get-AzNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
```

Belirttiğiniz depolama hesabı, ağ erişimini yalnızca Microsoft hizmetleri veya belirli sanal ağlarla sınırlandıran ağ kurallarının onun için yapılandırılamıyor. Depolama hesabı, akış günlüğünü etkinleştirdiğiniz NSG ile aynı veya farklı bir Azure aboneliğinde olabilir. Farklı abonelikler kullanıyorsanız, her ikisinin de aynı Azure Etkin Dizin kiracıyla ilişkilendirilmesi gerekir. Her abonelik için kullandığınız hesap [gerekli izinlere](required-rbac-permissions.md)sahip olmalıdır.

## <a name="disable-traffic-analytics-and-network-security-group-flow-logs"></a>Trafik Analizi ve Ağ Güvenliği Grup Akışı günlüklerini devre dışı bırak

Trafik analitiğini ve akış günlüklerini devre dışı kalmak için aşağıdaki örneği kullanın:

```powershell
#Disable Traffic Analaytics by removing -EnableTrafficAnalytics property
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2 -WorkspaceResourceId $workspaceResourceId -WorkspaceGUID $workspaceGUID -WorkspaceLocation $workspaceLocation

#Disable Flow Logging
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a>Akış günlüğü indirin

Akış günlüğünün depolama konumu oluşturmada tanımlanır. Bir depolama hesabına kaydedilen bu akış günlüklerine erişmek için kullanışlı bir araç, buradan indirilebilen Microsoft Azure Depolama Gezgini'dir:https://storageexplorer.com/

Bir depolama hesabı belirtilirse, akış günlüğü dosyaları aşağıdaki konumdaki bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Günlük ziyaret [Ağ Güvenlik Grubu Akış günlüğü Genel Bakış](network-watcher-nsg-flow-logging-overview.md) yapısı hakkında bilgi için

## <a name="next-steps"></a>Sonraki Adımlar

[PowerBI ile NSG akış günlüklerinizi nasıl görselleştirebilirsiniz](network-watcher-visualize-nsg-flow-logs-power-bi.md) öğrenin

[NSG akış günlüklerinizi açık kaynak araçlarıyla nasıl görselleştirebilirsiniz](network-watcher-visualize-nsg-flow-logs-open-source-tools.md) öğrenin
