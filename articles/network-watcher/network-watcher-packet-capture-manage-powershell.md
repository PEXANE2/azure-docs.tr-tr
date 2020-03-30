---
title: Paket yakalamaları yönetme - Azure PowerShell
titleSuffix: Azure Network Watcher
description: Bu sayfa, PowerShell kullanarak Network Watcher'ın paket yakalama özelliğini nasıl yönetilir açıklar
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
ms.openlocfilehash: 06263f85f7d6ad6cc80724baab01124833498739
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79129644"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-powershell"></a>PowerShell'i kullanarak Azure Ağ İzleyicisi ile paket yakalamaları yönetme

> [!div class="op_single_selector"]
> - [Azure portalında](network-watcher-packet-capture-manage-portal.md)
> - [Powershell](network-watcher-packet-capture-manage-powershell.md)
> - [Azure CLI](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST API](network-watcher-packet-capture-manage-rest.md)

Network Watcher paket yakalama ve sanal bir makineden gelen trafiği izlemek için yakalama oturumları oluşturmanıza olanak sağlar. Yalnızca istediğiniz trafiği yakalamanızı sağlamak için yakalama oturumu için filtreler sağlanır. Paket yakalama, ağ anomalilerinin hem reaktif hem de proaktif olarak tanılenmesine yardımcı olur. Diğer kullanım alanları arasında ağ istatistiklerinin toplanması, ağ ihlalleri hakkında bilgi edinme, istemci-sunucu iletişimini hata ayıklama ve çok daha fazlası yer almaktadır. Paket yakalamalarını uzaktan tetikleyebilen bu özellik, paket yakalamayı el ile ve istenilen makinede çalıştırma nın yükünü hafifleterek değerli zamandan tasarruf sağlar.

Bu makalede, şu anda paket yakalama için kullanılabilir olan farklı yönetim görevleri aracılığıyla alır.

- [**Paket yakalama başlatma**](#start-a-packet-capture)
- [**Paket yakalamayı durdurma**](#stop-a-packet-capture)
- [**Paket yakalamayı silme**](#delete-a-packet-capture)
- [**Paket yakalama indirme**](#download-a-packet-capture)


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, aşağıdaki kaynaklara sahip varsayar:

* Paket yakalama oluşturmak istediğiniz bölgedeki Ağ İzleyicisi örneği

* Paket yakalama uzantısı etkin olan sanal bir makine.

> [!IMPORTANT]
> Paket yakalama sanal bir `AzureNetworkWatcherExtension`makine uzantısı gerektirir. Uzantıyı Windows VM'de yüklemek [için Windows için Azure Network Watcher Agent sanal makine uzantısını](../virtual-machines/windows/extensions-nwa.md) ziyaret edin ve Linux VM için Linux için Azure Network [Watcher Agent sanal makine uzantısını](../virtual-machines/linux/extensions-nwa.md)ziyaret edin.

## <a name="install-vm-extension"></a>VM uzantıları yükleme

### <a name="step-1"></a>1. Adım

```powershell
$VM = Get-AzVM -ResourceGroupName testrg -Name VM1
```

### <a name="step-2"></a>2. Adım

Aşağıdaki örnek, `Set-AzVMExtension` cmdlet çalıştırmak için gerekli uzantı bilgilerini alır. Bu cmdlet, paket yakalama aracısını konuk sanal makineye yükler.

> [!NOTE]
> Cmdlet'in `Set-AzVMExtension` tamamlanması birkaç dakika sürebilir.

Windows sanal makineleri için:

```powershell
$AzureNetworkWatcherExtension = Get-AzVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentWindows -Version 1.4.585.2
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

Linux sanal makineleri için:

```powershell
$AzureNetworkWatcherExtension = Get-AzVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentLinux -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

Aşağıdaki örnek `Set-AzVMExtension` cmdlet çalıştırdıktan sonra başarılı bir yanıttır.

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
```

### <a name="step-3"></a>3. Adım

Aracının yüklü olduğundan emin olmak `Get-AzVMExtension` için cmdlet'i çalıştırın ve sanal makine adını ve uzantı adını geçirin.

```powershell
Get-AzVMExtension -ResourceGroupName $VM.ResourceGroupName  -VMName $VM.Name -Name $ExtensionName
```

Aşağıdaki örnek, çalışan yanıtbir örnektir`Get-AzVMExtension`

```
ResourceGroupName       : testrg
VMName                  : testvm1
Name                    : AzureNetworkWatcherExtension
Location                : westcentralus
Etag                    : null
Publisher               : Microsoft.Azure.NetworkWatcher
ExtensionType           : NetworkWatcherAgentWindows
TypeHandlerVersion      : 1.4
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1/
                          extensions/AzureNetworkWatcherExtension
PublicSettings          : 
ProtectedSettings       : 
ProvisioningState       : Succeeded
Statuses                : 
SubStatuses             : 
AutoUpgradeMinorVersion : True
ForceUpdateTag          : 
```

## <a name="start-a-packet-capture"></a>Paket yakalama başlatma

Önceki adımlar tamamlandıktan sonra, paket yakalama aracısı sanal makineye yüklenir.

### <a name="step-1"></a>1. Adım

Bir sonraki adım, Ağ İzleyicisi örneğini almaktır. Bu değişken adım `New-AzNetworkWatcherPacketCapture` 4'teki cmdlete geçirilir.

```powershell
$networkWatcher = Get-AzResource -ResourceType "Microsoft.Network/networkWatchers" | Where {$_.Location -eq "WestCentralUS" }
```

### <a name="step-2"></a>2. Adım

Bir depolama hesabı alın. Bu depolama hesabı paket yakalama dosyasını depolamak için kullanılır.

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName testrg -Name testrgsa123
```

### <a name="step-3"></a>3. Adım

Filtreler, paket yakalama tarafından depolanan verileri sınırlamak için kullanılabilir. Aşağıdaki örnekte iki filtre vardır.  Bir filtre giden TCP trafiğini yalnızca yerel IP 10.0.0.3'ten 20, 80 ve 443 numaralı hedef bağlantı noktalarına kadar toplar.  İkinci filtre yalnızca UDP trafiğini toplar.

```powershell
$filter1 = New-AzPacketCaptureFilterConfig -Protocol TCP -RemoteIPAddress "1.1.1.1-255.255.255.255" -LocalIPAddress "10.0.0.3" -LocalPort "1-65535" -RemotePort "20;80;443"
$filter2 = New-AzPacketCaptureFilterConfig -Protocol UDP
```

> [!NOTE]
> Paket yakalama için birden çok filtre tanımlanabilir.

### <a name="step-4"></a>4. Adım

Önceki `New-AzNetworkWatcherPacketCapture` adımlarda alınan gerekli değerleri geçerek paket yakalama işlemini başlatmak için cmdlet çalıştırın.
```powershell

New-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $vm.Id -PacketCaptureName "PacketCaptureTest" -StorageAccountId $storageAccount.id -TimeLimitInSeconds 60 -Filter $filter1, $filter2
```

Aşağıdaki `New-AzNetworkWatcherPacketCapture` örnek, cmdlet'in çalıştırılmasına beklenen çıktıdır.

```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"3bf27278-8251-4651-9546-c7f369855e4e"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]


```

## <a name="get-a-packet-capture"></a>Paket yakalama alma

Cmdlet'i `Get-AzNetworkWatcherPacketCapture` çalıştırarak, şu anda çalışan veya tamamlanmış paket yakalama durumunu alır.

```powershell
Get-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

Aşağıdaki örnek `Get-AzNetworkWatcherPacketCapture` cmdlet çıkış. Aşağıdaki örnek, yakalama tamamlandıktan sonradır. PacketCaptureStatus değeri Durdurulur ve TimeExceeded'In Durdurulması Sonucu Durdu. Bu değer, paket yakalamanın başarılı olduğunu ve zamanını çalıştırdığını gösterir.
```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"4b9a81ed-dc63-472e-869e-96d7166ccb9b"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]
CaptureStartTime        : 2/1/2017 10:43:01 PM
PacketCaptureStatus     : Stopped
StopReason              : TimeExceeded
PacketCaptureError      : []
```

## <a name="stop-a-packet-capture"></a>Paket yakalamayı durdurma

`Stop-AzNetworkWatcherPacketCapture` Cmdlet çalıştırılarak, bir yakalama oturumu devam ediyorsa durdurulur.

```powershell
Stop-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> Cmdlet, şu anda çalışan bir yakalama oturumunda veya zaten durmuş olan varolan bir oturumda çalıştırıldığında yanıt vermez.

## <a name="delete-a-packet-capture"></a>Paket yakalamayı silme

```powershell
Remove-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
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














