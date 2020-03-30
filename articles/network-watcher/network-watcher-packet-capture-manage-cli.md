---
title: Azure Ağ İzleyicisi ile paket yakalamaları yönetme - Azure CLI | Microsoft Dokümanlar
description: Bu sayfa, Azure CLI'yi kullanarak Ağ İzleyicisi'nin paket yakalama özelliğini nasıl yönetişleyini açıklar
services: network-watcher
documentationcenter: na
author: damendo
ms.assetid: cb0c1d10-f7f2-4c34-b08c-f73452430be8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 7a69610d1ac176354a9d7e388a12ccc7f064d848
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382724"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-azure-cli"></a>Azure CLI'yi kullanarak Azure Ağ İzleyicisi ile paket yakalamaları yönetme

> [!div class="op_single_selector"]
> - [Azure portalında](network-watcher-packet-capture-manage-portal.md)
> - [Powershell](network-watcher-packet-capture-manage-powershell.md)
> - [Azure CLI](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST API](network-watcher-packet-capture-manage-rest.md)

Network Watcher paket yakalama ve sanal bir makineden gelen trafiği izlemek için yakalama oturumları oluşturmanıza olanak sağlar. Yalnızca istediğiniz trafiği yakalamanızı sağlamak için yakalama oturumu için filtreler sağlanır. Paket yakalama, ağ anomalilerinin hem reaktif hem de proaktif olarak tanılenmesine yardımcı olur. Diğer kullanım alanları arasında ağ istatistiklerinin toplanması, ağ ihlalleri hakkında bilgi edinme, istemci-sunucu iletişimini hata ayıklama ve çok daha fazlası yer almaktadır. Paket yakalamalarını uzaktan tetikleyebilen bu özellik, paket yakalamayı el ile ve istenilen makinede çalıştırma nın yükünü hafifleterek değerli zamandan tasarruf sağlar.

Bu makaledeki adımları gerçekleştirmek için [Mac, Linux ve Windows için Azure Komut Satırı Arabirimini (Azure CLI) yüklemeniz](/cli/azure/install-azure-cli)gerekir.

Bu makalede, şu anda paket yakalama için kullanılabilir olan farklı yönetim görevleri aracılığıyla alır.

- [**Paket yakalama başlatma**](#start-a-packet-capture)
- [**Paket yakalamayı durdurma**](#stop-a-packet-capture)
- [**Paket yakalamayı silme**](#delete-a-packet-capture)
- [**Paket yakalama indirme**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, aşağıdaki kaynaklara sahip varsayar:

- Paket yakalama oluşturmak istediğiniz bölgedeki Ağ İzleyicisi örneği
- Paket yakalama uzantısı etkin olan sanal bir makine.

> [!IMPORTANT]
> Paket yakalama, sanal makinede çalışan bir aracı gerektirir. Aracı uzantı olarak yüklenir. VM uzantıları yla ilgili talimatlar için [Sanal Makine uzantılarını ve özelliklerini](../virtual-machines/windows/extensions-features.md)ziyaret edin.

## <a name="install-vm-extension"></a>VM uzantıları yükleme

### <a name="step-1"></a>1. Adım

Paket `az vm extension set` yakalama aracısını konuk sanal makineye yüklemek için komutu çalıştırın.

Windows sanal makineleri için:

```azurecli-interactive
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

Linux sanal makineleri için:

```azurecli-interactive
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux --version 1.4
```

### <a name="step-2"></a>2. Adım

Aracının yüklü olduğundan emin olmak `vm extension show` için komutu çalıştırın ve kaynak grubunu ve sanal makine adını geçirin. Aracının yüklü olduğundan emin olmak için ortaya çıkan listeyi kontrol edin.

Windows sanal makineleri için:

```azurecli-interactive
az vm extension show --resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

Linux sanal makineleri için:

```azurecli-interactive
az vm extension show --resource-group resourceGroupName --vm-name virtualMachineName --name AzureNetworkWatcherExtension
```

Aşağıdaki örnek, çalışan yanıtbir örnektir`az vm extension show`

```json
{
  "autoUpgradeMinorVersion": true,
  "forceUpdateTag": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/extensions/NetworkWatcherAgentWindows",
  "instanceView": null,
  "location": "westcentralus",
  "name": "NetworkWatcherAgentWindows",
  "protectedSettings": null,
  "provisioningState": "Succeeded",
  "publisher": "Microsoft.Azure.NetworkWatcher",
  "resourceGroup": "{resourceGroupName}",
  "settings": null,
  "tags": null,
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "typeHandlerVersion": "1.4",
  "virtualMachineExtensionType": "NetworkWatcherAgentWindows"
}
```

## <a name="start-a-packet-capture"></a>Paket yakalama başlatma

Önceki adımlar tamamlandıktan sonra, paket yakalama aracısı sanal makineye yüklenir.

### <a name="step-1"></a>1. Adım

Bir depolama hesabı alın. Bu depolama hesabı paket yakalama dosyasını depolamak için kullanılır.

```azurecli-interactive
az storage account list
```

### <a name="step-2"></a>2. Adım

Bu noktada, bir paket yakalama oluşturmak için hazırsınız.  Öncelikle, yapılandırmak isteyebileceğiniz parametreleri inceleyelim. Filtreler, paket yakalama tarafından depolanan verileri sınırlamak için kullanılabilecek türlerden biridir. Aşağıdaki örnek, birkaç filtreyle bir paket yakalama ayarlar.  İlk üç filtre, giden TCP trafiğini yalnızca yerel IP 10.0.0.3'ten 20, 80 ve 443 hedef bağlantı noktalarına kadar toplar.  Son filtre yalnızca UDP trafiğini toplar.

```azurecli-interactive
az network watcher packet-capture create --resource-group {resourceGroupName} --vm {vmName} --name packetCaptureName --storage-account {storageAccountName} --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

Aşağıdaki örnek, `az network watcher packet-capture create` komutu çalıştıran beklenen çıktıdır.

```json
{
  "bytesToCapturePerPacket": 0,
  "etag": "W/\"b8cf3528-2e14-45cb-a7f3-5712ffb687ac\"",
  "filters": [
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "20"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "80"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "443"
    },
    {
      "localIpAddress": "",
      "localPort": "",
      "protocol": "UDP",
      "remoteIpAddress": "",
      "remotePort": ""
    }
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/pa
cketCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/p
roviders/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapture_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="get-a-packet-capture"></a>Paket yakalama alma

Komutu `az network watcher packet-capture show-status` çalıştırıyor, şu anda çalışan veya tamamlanmış paket yakalama durumunu alır.

```azurecli-interactive
az network watcher packet-capture show-status --name packetCaptureName --location {networkWatcherLocation}
```

Aşağıdaki `az network watcher packet-capture show-status` örnek, komuttan çıktıdır. Aşağıdaki örnek, yakalamanın Durdurulduğu ve TimeExceeded'ın Durdurulmasıdır.

```
{
  "additionalProperties": {
    "status": "Succeeded"
  },
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/pa
cketCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "packetCaptureError": [],
  "packetCaptureStatus": "Stopped",
  "stopReason": "TimeExceeded"
}
```

## <a name="stop-a-packet-capture"></a>Paket yakalamayı durdurma

Komutu `az network watcher packet-capture stop` çalıştırarak, bir yakalama oturumu devam ediyorsa durdurulur.

```azurecli-interactive
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> Komut, şu anda çalışan bir yakalama oturumunda veya zaten durmuş olan varolan bir oturumda çalıştırıldığında yanıt döndürür.

## <a name="delete-a-packet-capture"></a>Paket yakalamayı silme

```azurecli-interactive
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> Paket yakalamayı silmek, depolama hesabındaki dosyayı silmez.

## <a name="download-a-packet-capture"></a>Paket yakalama indirme

Paket yakalama oturumunuz tamamlandıktan sonra, yakalama dosyası blob depolama alanına veya VM'deki yerel bir dosyaya yüklenebilir. Paket yakalamanın depolama konumu oturumun oluşturulmasında tanımlanır. Bir depolama hesabına kaydedilen bu yakalama dosyalarına erişmek için kullanışlı bir araç, buradan indirilebilen Microsoft Azure Depolama Gezgini'dir:https://storageexplorer.com/

Bir depolama hesabı belirtilirse, paket yakalama dosyaları aşağıdaki konumdaki bir depolama hesabına kaydedilir:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Sonraki adımlar

[Uyarı tetiklenen paket yakalamayı](network-watcher-alert-triggered-packet-capture.md) görüntüleyerek Sanal makine uyarılarıyla paket yakalamaları nasıl otomatikleştirin

[KONTROL IP akışını doğrulayarak](diagnose-vm-network-traffic-filtering-problem.md) VM'nize belirli bir trafiğe izin verilip verilmediğini bulun

<!-- Image references -->
