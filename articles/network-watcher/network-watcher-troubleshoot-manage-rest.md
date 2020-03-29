---
title: Sorun Giderme VNET Ağ Geçidi ve Bağlantıları - Azure REST API
titleSuffix: Azure Network Watcher
description: Bu sayfa, REST kullanarak Azure Ağ İzleyicisi ile Sanal Ağ Ağ Ağ Geçitleri ve Bağlantıları ile nasıl sorun giderilen açıklar
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: damendo
ms.openlocfilehash: ab9f7fd95d7081b66e05dfd3d6a5ef47eb3c4053
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840681"
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher"></a>Azure Ağ İzleyicisi kullanarak Sanal Ağ ağ geçidi ve Bağlantıları sorun giderme

> [!div class="op_single_selector"]
> - [Portal](diagnose-communication-problem-between-networks.md)
> - [Powershell](network-watcher-troubleshoot-manage-powershell.md)
> - [Azure CLI](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Ağ İzleyicisi, Azure'daki ağ kaynaklarınızı anlamayla ilgili olarak birçok özellik sağlar. Bu özelliklerden biri kaynak sorun gidermedir. Kaynak sorun giderme portalı, PowerShell, CLI veya REST API üzerinden çağrılabilir. Ağ İzleyicisi çağrıldığında, Sanal Ağ Ağ Ağ Geçidi'nin veya Bağlantının sistem durumunu inceler ve bulgularını döndürür.

Bu makalede, şu anda kaynak sorun giderme için kullanılabilir olan farklı yönetim görevleri aracılığıyla alır.

- [**Sanal Ağ ağ geçidini giderme**](#troubleshoot-a-virtual-network-gateway)
- [**Bağlantıyı Sorun Giderme**](#troubleshoot-connections)

## <a name="before-you-begin"></a>Başlamadan önce

ARMclient PowerShell kullanarak REST API aramak için kullanılır. ARMClient [Chocolatey armclient](https://chocolatey.org/packages/ARMClient) çikolataüzerinde bulunur

Bu senaryo, Ağ İzleyicisi oluşturmak için [Ağ İzleyicisi Oluştur'daki](network-watcher-create.md) adımları zaten izlediğinizi varsayar.

Desteklenen ağ geçidi türlerinin listesi ziyaret, [Desteklenen Ağ Geçidi türleri.](network-watcher-troubleshoot-overview.md#supported-gateway-types)

## <a name="overview"></a>Genel Bakış

Ağ İzleyicisorun giderme yeteneği Sanal Ağ ağ geçitleri ve Bağlantıları ile ortaya çıkan sorunları gidermek sağlar. Kaynak sorun giderme için bir istek yapıldığında, günlükler sorgulanır ve denetlenir. Denetim tamamlandığında, sonuçlar döndürülür. Sorun giderme API istekleri, bir sonucu döndürmek için birden çok dakika sürebilir uzun çalışan istekleri vardır. Günlükler bir depolama hesabındaki bir kapta depolanır.

## <a name="log-in-with-armclient"></a>ARMClient ile giriş yapın

```powershell
armclient login
```

## <a name="troubleshoot-a-virtual-network-gateway"></a>Sanal Ağ ağ geçidini giderme


### <a name="post-the-troubleshoot-request"></a>Sorun giderme isteğini POST

Aşağıdaki örnek, Sanal Ağ ağ geçidinin durumunu sorgular.

```powershell

$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "ContosoRG"
$NWresourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$vnetGatewayName = "ContosoVNETGateway"
$storageAccountName = "contososa"
$containerName = "gwlogs"
$requestBody = @"
{
'TargetResourceId': '/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/virtualNetworkGateways/${vnetGatewayName}',
'Properties': {
'StorageId': '/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Storage/storageAccounts/${storageAccountName}',
'StoragePath': 'https://${storageAccountName}.blob.core.windows.net/${containerName}'
}
}
"@


armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30" $requestBody -verbose
```

Bu işlem uzun süredir devam ettiği için, işlemi sorgulamak için URI ve sonuç için URI aşağıdaki yanıtta gösterildiği gibi yanıt başlığında döndürülür:

**Önemli Değerler**

* **Azure-Asyncİşlem** - Bu özellik, Async sorun giderme işlemini sorgulamak için URI içerir
* **Konum** - Bu özellik, işlem tamamlandığında sonuçların olduğu URI'yi içerir

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 8a1167b7-6768-4ac1-85dc-703c9c9b9247
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4364d88a-bd08-422c-a716-dbb0cdc99f7b
x-ms-routing-request-id: NORTHCENTRALUS:20170112T183202Z:4364d88a-bd08-422c-a716-dbb0cdc99f7b
Date: Thu, 12 Jan 2017 18:32:01 GMT

null
```

### <a name="query-the-async-operation-for-completion"></a>Tamamlanma kullanabilirsiniza async işlemini sorgula

Aşağıdaki örnekte görüldüğü gibi işlemin ilerlemesini sorgulamak için URI işlemlerini kullanın:

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30" -verbose
```

İşlem devam ederken, yanıt aşağıdaki örnekte görüldüğü gibi **Devam'ı** gösterir:

```json
{
  "status": "InProgress"
}
```

İşlem tamamlandığında, **durum**Başarılı olarak değişir.

```json
{
  "status": "Succeeded"
}
```

### <a name="retrieve-the-results"></a>Sonuçları alma

Döndürülen durum **Başarılı**olduktan sonra, sonuçları almak için result URI operasyonunda GET Metodu'nu arayın.

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30" -verbose
```

Aşağıdaki yanıtlar, bir ağ geçidini giderme sonuçlarını sorgularken döndürülen tipik bir yanıtörnekleridir. Bkz. [Yanıttaki özelliklerin](#understanding-the-results) ne anlama geldiğini açıklığa kavuşturmak için sonuçları anlama.

```json
{
  "startTime": "2017-01-12T10:31:41.562646-08:00",
  "endTime": "2017-01-12T18:31:48.677Z",
  "code": "Degraded",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time the gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This is a transient state while the Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If the condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting the VPN Gateway"
        },
        {
          "actionText": "If your VPN gateway isn't up and running by the expected resolution time, contact support",
          "actionUri": "https://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    },
    {
      "id": "NoFault",
      "summary": "This VPN gateway is running normally",
      "detail": "There aren't any known Azure platform problems affecting this VPN Connection",
      "recommendedActions": [
        {
          "actionText": "If you are still experience problems with the VPN gateway, please try resetting the VPN gateway.",
          "actionUri": "https://azure.microsoft.com/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "https://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```


## <a name="troubleshoot-connections"></a>Sorun Giderme Bağlantıları

Aşağıdaki örnekte Bağlantı durumunu sorgular.

```powershell

$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "ContosoRG"
$NWresourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$connectionName = "VNET2toVNET1Connection"
$storageAccountName = "contososa"
$containerName = "gwlogs"
$requestBody = @{
"TargetResourceId": "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/connections/${connectionName}",
"Properties": {
"StorageId": "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Storage/storageAccounts/${storageAccountName}",
"StoragePath": "https://${storageAccountName}.blob.core.windows.net/${containerName}"
}

}
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30 $requestBody"
```

> [!NOTE]
> Sorun giderme işlemi, Bağlantı ve ilgili ağ geçitlerinde paralel olarak çalıştırılamaz. İşlem, önceki kaynakta çalıştırmadan önce tamamlanması gerekir.

Bu uzun süren bir işlem olduğundan, yanıt üstbilgisinde, işlemi sorgulamak için URI ve sonuç için URI aşağıdaki yanıtta gösterildiği gibi döndürülür:

**Önemli Değerler**

* **Azure-Asyncİşlem** - Bu özellik, Async sorun giderme işlemini sorgulamak için URI içerir
* **Konum** - Bu özellik, işlem tamamlandığında sonuçların olduğu URI'yi içerir

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 8a1167b7-6768-4ac1-85dc-703c9c9b9247
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4364d88a-bd08-422c-a716-dbb0cdc99f7b
x-ms-routing-request-id: NORTHCENTRALUS:20170112T183202Z:4364d88a-bd08-422c-a716-dbb0cdc99f7b
Date: Thu, 12 Jan 2017 18:32:01 GMT

null
```

### <a name="query-the-async-operation-for-completion"></a>Tamamlanma kullanabilirsiniza async işlemini sorgula

Aşağıdaki örnekte görüldüğü gibi işlemin ilerlemesini sorgulamak için URI işlemlerini kullanın:

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

İşlem devam ederken, yanıt aşağıdaki örnekte görüldüğü gibi **Devam'ı** gösterir:

```json
{
  "status": "InProgress"
}
```

İşlem tamamlandığında, durum **Başarılı**olarak değişir.

```json
{
  "status": "Succeeded"
}
```

Aşağıdaki yanıtlar, bir Bağlantı sorun giderme sonuçlarını sorgularken döndürülen tipik bir yanıtörnekleridir.

### <a name="retrieve-the-results"></a>Sonuçları alma

Döndürülen durum **Başarılı**olduktan sonra, sonuçları almak için result URI operasyonunda GET Metodu'nu arayın.

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

Aşağıdaki yanıtlar, bir Bağlantı sorun giderme sonuçlarını sorgularken döndürülen tipik bir yanıtörnekleridir.

```json
{
  "startTime": "2017-01-12T14:09:19.1215346-08:00",
  "endTime": "2017-01-12T22:09:23.747Z",
  "code": "UnHealthy",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time the gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This 
is a transient state while the Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If the condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting the VPN gateway"
        },
        {
          "actionText": "If your VPN Connection isn't up and running by the expected resolution time, contact support",
          "actionUri": "https://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    },
    {
      "id": "NoFault",
      "summary": "This VPN Connection is running normally",
      "detail": "There aren't any known Azure platform problems affecting this VPN Connection",
      "recommendedActions": [
        {
          "actionText": "If you are still experience problems with the VPN gateway, please try resetting the VPN gateway.",
          "actionUri": "https://azure.microsoft.com/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "https://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```

## <a name="understanding-the-results"></a>Sonuçları anlama

Eylem metni, sorunun nasıl çözüleceği konusunda genel yönerge ler sağlar. Sorun için bir eylem yapılabilirse, ek yönergeyle birlikte bir bağlantı sağlanır. Ek bir kılavuz bulunmadığı durumlarda, yanıt bir destek örneği açmak için url sağlar.  Yanıtın özellikleri ve nelerin dahil olduğu hakkında daha fazla bilgi için [Network Watcher Sorun Giderme Sorunu Özeti'ne genel bakışı](network-watcher-troubleshoot-overview.md) ziyaret edin

Azure depolama hesaplarından dosya indirme yle ilgili talimatlar için [,.NET'i kullanarak Azure Blob depolama alanına başlayın'a](../storage/blobs/storage-dotnet-how-to-use-blobs.md)bakın. Kullanılabilecek bir diğer araç da Depolama Gezgini'dir. Depolama Gezgini hakkında daha fazla bilgiyi aşağıdaki bağlantıda bulabilirsiniz: [Depolama Gezgini](https://storageexplorer.com/)

## <a name="next-steps"></a>Sonraki adımlar

VPN bağlantısını durduran ayarlar değiştirildiyse, ağ güvenlik grubunu ve söz konusu olabilecek güvenlik kurallarını izlemek için [Ağ Güvenlik Gruplarını Yönet'e](../virtual-network/manage-network-security-group.md) bakın.
