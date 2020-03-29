---
title: Sorun giderme bağlantıları - Azure CLI
titleSuffix: Azure Network Watcher
description: Azure CLI'yi kullanarak Azure Ağ İzleyicisi'nin bağlantı sorun giderme özelliğini nasıl kullanacağınızı öğrenin.
services: network-watcher
documentationcenter: na
author: damendo
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: damendo
ms.openlocfilehash: 842e58de8dbc06d3f045b0e9d0dc6b99e6b1e2fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76842895"
---
# <a name="troubleshoot-connections-with-azure-network-watcher-using-the-azure-cli"></a>Azure CLI'yi kullanarak Azure Ağ İzleyicisi ile bağlantıları giderme

> [!div class="op_single_selector"]
> - [Powershell](network-watcher-connectivity-powershell.md)
> - [Azure CLI](network-watcher-connectivity-cli.md)
> - [Azure REST API](network-watcher-connectivity-rest.md)

Sanal bir makineden belirli bir bitiş noktasına doğrudan TCP bağlantısının kurulup kurulamayacağını doğrulamak için bağlantı sorun giderini nasıl kullanacağınızı öğrenin.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, aşağıdaki kaynaklara sahip varsayar:

* Bağlantıyla sorun gidermek istediğiniz bölgedeki Ağ İzleyicisi örneği.
* Bağlantıları gidermek için sanal makineler.

> [!IMPORTANT]
> Bağlantı sorun giderme, sorun giderdiğiniz VM'nin `AzureNetworkWatcherExtension` VM uzantısıyüklü olması gerekir. Uzantıyı Windows VM'de yüklemek [için Windows için Azure Network Watcher Agent sanal makine uzantısını](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ziyaret edin ve Linux VM için Linux için Azure Network [Watcher Agent sanal makine uzantısını](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)ziyaret edin. Uzantı, hedef bitiş noktasında gerekli değildir.

## <a name="check-connectivity-to-a-virtual-machine"></a>Sanal makineye bağlantıyı kontrol edin

Bu örnek, bağlantı noktası 80 üzerinden bir hedef sanal makineye bağlantı denetler.

### <a name="example"></a>Örnek

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-resource Database0 --dest-port 80
```

### <a name="response"></a>Yanıt

Aşağıdaki yanıt önceki örnektendir.  Bu yanıtta, `ConnectionStatus` **Erişilemez.** Gönderilen tüm sondaların başarısız olduğunu görebilirsiniz. `NetworkSecurityRule` **UserRule_Port80**Bağlantı, 80. Bu bilgiler bağlantı sorunlarını araştırmak için kullanılabilir.

```json
{
  "avgLatencyInMs": null,
  "connectionStatus": "Unreachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "bb01d336-d881-4808-9fbc-72f091974d68",
      "issues": [],
      "nextHopIds": [
        "f8b074e9-9980-496b-a35e-619f9bcbf648"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "10.1.2.4",
      "id": "f8b074e9-9980-496b-a35e-619f9bcbf648",
      "issues": [],
      "nextHopIds": [
        "8a5857f3-6ab8-4b11-b9bf-a046d66b8696"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/fw
Nic/ipConfigurations/ipconfig1",
      "type": "VirtualAppliance"
    },
    {
      "address": "10.1.3.4",
      "id": "8a5857f3-6ab8-4b11-b9bf-a046d66b8696",
      "issues": [
        {
          "context": [
            {
              "key": "RuleName",
              "value": "UserRule_Port80"
            }
          ],
          "origin": "Outbound",
          "severity": "Error",
          "type": "NetworkSecurityRule"
        }
      ],
      "nextHopIds": [
        "6ce2f7a2-ceb4-4145-80e8-5d9f661655d6"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/au
Nic/ipConfigurations/ipconfig1",
      "type": "VirtualAppliance"
    },
    {
      "address": "10.1.4.4",
      "id": "6ce2f7a2-ceb4-4145-80e8-5d9f661655d6",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/db
Nic0/ipConfigurations/ipconfig1",
      "type": "VnetLocal"
    }
  ],
  "maxLatencyInMs": null,
  "minLatencyInMs": null,
  "probesFailed": 100,
  "probesSent": 100
}
```

## <a name="validate-routing-issues"></a>Yönlendirme sorunlarını doğrulama

Bu örnek, sanal makine ile uzak bir bitiş noktası arasındaki bağlantıyı denetler.

### <a name="example"></a>Örnek

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address 13.107.21.200 --dest-port 80
```

### <a name="response"></a>Yanıt

Aşağıdaki örnekte, `connectionStatus` **Erişilemez**olarak gösterilir. Ayrıntılarda, `hops` `issues` trafiğin bir `UserDefinedRoute`.

```json
{
  "avgLatencyInMs": null,
  "connectionStatus": "Unreachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "f2cb1868-2049-4839-b8ed-57a480d06f95",
      "issues": [
        {
          "context": [
            {
              "key": "RouteType",
              "value": "User"
            }
          ],
          "origin": "Outbound",
          "severity": "Error",
          "type": "UserDefinedRoute"
        }
      ],
      "nextHopIds": [
        "da4022db-0ab0-48c4-a507-dd4c03561ca5"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "13.107.21.200",
      "id": "da4022db-0ab0-48c4-a507-dd4c03561ca5",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Unknown",
      "type": "Destination"
    }
  ],
  "maxLatencyInMs": null,
  "minLatencyInMs": null,
  "probesFailed": 100,
  "probesSent": 100
}
```

## <a name="check-website-latency"></a>Web sitesi gecikmesi kontrol edin

Aşağıdaki örnek, bir web sitesine bağlantıdenetler.

### <a name="example"></a>Örnek

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address https://bing.com --dest-port 80
```

### <a name="response"></a>Yanıt

Aşağıdaki yanıtta, gösteriyi `connectionStatus` **Erişilebilir**olarak görebilirsiniz. Bir bağlantı başarılı olduğunda, gecikme değerleri sağlanır.

```json
{
  "avgLatencyInMs": 2,
  "connectionStatus": "Reachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "639c2d19-e163-4dfd-8737-5018dd1168ae",
      "issues": [],
      "nextHopIds": [
        "fd43a6e7-c758-4f48-90aa-8db99105a4a3"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "204.79.197.200",
      "id": "fd43a6e7-c758-4f48-90aa-8db99105a4a3",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Internet",
      "type": "Internet"
    }
  ],
  "maxLatencyInMs": 7,
  "minLatencyInMs": 0,
  "probesFailed": 0,
  "probesSent": 100
}
```

## <a name="check-connectivity-to-a-storage-endpoint"></a>Depolama bitiş noktasına bağlantı denetimi

Aşağıdaki örnek, sanal bir makineden bir blog depolama hesabına bağlantı yı denetler.

### <a name="example"></a>Örnek

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address https://contosoexamplesa.blob.core.windows.net/
```

### <a name="response"></a>Yanıt

Aşağıdaki json önceki cmdlet çalışan örnek yanıttır. Denetim başarılı olduğundan, `connectionStatus` özellik **Erişilebilir**olarak gösterir.  Depolama blob ve gecikme ulaşmak için gerekli atlama sayısı ile ilgili ayrıntıları sağlanır.

```json
{
  "avgLatencyInMs": 1,
  "connectionStatus": "Reachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "5136acff-bf26-4c93-9966-4edb7dd40353",
      "issues": [],
      "nextHopIds": [
        "f8d958b7-3636-4d63-9441-602c1eb2fd56"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "1.2.3.4",
      "id": "f8d958b7-3636-4d63-9441-602c1eb2fd56",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Internet",
      "type": "Internet"
    }
  ],
  "maxLatencyInMs": 7,
  "minLatencyInMs": 0,
  "probesFailed": 0,
  "probesSent": 100
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Uyarı tetiklenen paket yakalamayı](network-watcher-alert-triggered-packet-capture.md) görüntüleyerek Sanal makine uyarılarıyla paket yakalamaları nasıl otomatikleştirin

[KONTROL IP akışını doğrulayarak](diagnose-vm-network-traffic-filtering-problem.md) VM'nize belirli bir trafiğe izin verilip verilmediğini bulun
