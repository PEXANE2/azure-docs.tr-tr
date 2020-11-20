---
title: Ağ Izleyicisi uzantısını en son sürüme güncelleştirin
description: Azure ağ Izleyicisi uzantısını en son sürüme güncelleştirmeyi öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: damendo
manager: balar
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.subservice: extensions
ms.topic: article
ms.workload: infrastructure-services
ms.date: 09/23/2020
ms.author: damendo
ms.openlocfilehash: c427a206e0422e66cb526a29a462d8b6bdf6818e
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94965944"
---
# <a name="update-the-network-watcher-extension-to-the-latest-version"></a>Ağ Izleyicisi uzantısını en son sürüme güncelleştirin

## <a name="overview"></a>Genel Bakış

[Azure Ağ İzleyicisi](../../network-watcher/network-watcher-monitoring-overview.md) , Azure ağlarını izleyen bir ağ performansı izleme, tanılama ve analiz hizmetidir. Ağ Izleyicisi Aracısı sanal makinesi (VM) uzantısı, ağ trafiğini talep üzerine yakalama ve Azure VM 'lerinde diğer gelişmiş işlevleri kullanma gereksinimidir. Ağ Izleyicisi uzantısı, bağlantı Izleyicisi, bağlantı Izleyicisi (Önizleme), bağlantı sorunlarını giderme ve paket yakalama gibi özellikler tarafından kullanılır.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, ağ Izleyicisi uzantısının sanal makinenizde yüklü olduğu varsayılır.

## <a name="latest-version"></a>En son sürüm

Ağ Izleyicisi uzantısının en son sürümü şu anda `1.4.1654.1` .

## <a name="update-your-extension"></a>Uzantınızı güncelleştirme

Uzantınızı güncelleştirmek için uzantı sürümünüzü bilmeniz gerekir.

### <a name="check-your-extension-version"></a>Uzantı sürümünüzü denetleyin

Azure portal, Azure CLı veya PowerShell 'i kullanarak uzantı sürümünüzü kontrol edebilirsiniz.

#### <a name="usetheazureportal"></a>Azure portal kullanın

1. Azure portal sanal makinenizin **Uzantılar** bölmesine gidin.
1. Ayrıntılar bölmesini görmek için **Azurenetworkizleyici** uzantısını seçin.
1. **Sürüm alanındaki sürüm** numarasını bulun.  

#### <a name="use-the-azure-cli"></a>Azure CLI kullanma

Bir Azure CLı isteminde aşağıdaki komutu çalıştırın:

```azurecli
az vm get-instance-view --resource-group  "SampleRG" --name "Sample-VM"
```
Çıktıda **"AzureNetworkWatcherExtension"** konumunu bulun ve çıktıda *"typehandlerversion"* alanından sürüm numarasını belirleyin.  Lütfen unutmayın: uzantı hakkındaki bilgiler JSON çıktısında birden çok kez görünür. Lütfen "Uzantılar" bloğunun altına bakın ve uzantının tam sürüm numarasını görmeniz gerekir. 

Aşağıdaki gibi bir şey görmeniz gerekir: ![ Azure CLI ekran görüntüsü](./media/network-watcher/azure-cli-screenshot.png)

#### <a name="usepowershell"></a>PowerShell 'i kullanma

Bir PowerShell isteminden aşağıdaki komutları çalıştırın:

```powershell
Get-AzVM -ResourceGroupName "SampleRG" -Name "Sample-VM" -Status
```
Çıktıda Azure ağ Izleyicisi uzantısını bulun ve çıktıda *"Typehandlerversion"* alanından sürüm numarasını belirleyin.   

Aşağıdakine benzer bir şey görmeniz gerekir: ![ PowerShell ekran görüntüsü](./media/network-watcher/powershell-screenshot.png)

### <a name="update-your-extension"></a>Uzantınızı güncelleştirme

Sürümünüz `1.4.1654.1` , geçerli en son sürümü olan sürümünden daha eski ise, aşağıdaki seçeneklerden herhangi birini kullanarak uzantınızı güncelleştirin.

#### <a name="option-1-use-powershell"></a>Seçenek 1: PowerShell kullanma

Aşağıdaki komutları çalıştırın:

```powershell
#Linux command
Set-AzVMExtension -ResourceGroupName "myResourceGroup1" -Location "WestUS" -VMName "myVM1" -Name "AzureNetworkWatcherExtension" -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentLinux"

#Windows command
Set-AzVMExtension -ResourceGroupName "myResourceGroup1" -Location "WestUS" -VMName "myVM1" -Name "NetworkWatcherAgentWindows" -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentWindows" -ForceRerun "True"

```

Bu işe yaramazsa. Aşağıdaki adımları kullanarak uzantıyı kaldırın ve yeniden yüklemeyi deneyin. Bu, en son sürümü otomatik olarak ekler.

Uzantı kaldırılıyor 

```powershell
#Same command for Linux and Windows
Remove-AzVMExtension -ResourceGroupName "SampleRG" -VMName "Sample-VM" -Name "AzureNetworkWatcherExtension"
``` 

Uzantıyı yeniden yükleme

```powershell
#Linux command
Set-AzVMExtension -ResourceGroupName "SampleRG" -Location "centralus" -VMName "Sample-VM" -Name "AzureNetworkWatcherExtension" -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentLinux" -typeHandlerVersion "1.4"

#Windows command
Set-AzVMExtension -ResourceGroupName "SampleRG" -Location "centralus" -VMName "Sample-VM" -Name "AzureNetworkWatcherExtension" -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentWindows" -typeHandlerVersion "1.4"
```

#### <a name="option-2-use-the-azure-cli"></a>2. seçenek: Azure CLı 'yi kullanma

Yükseltmeye zorlayın.

```azurecli
#Linux command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher" --force-update

#Windows command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher" --force-update
```

Bu işe yaramazsa, uzantıyı yeniden kaldırın ve en son sürümü otomatik olarak eklemek için aşağıdaki adımları izleyin.

Uzantıyı kaldırın.

```azurecli
#Same for Linux and Windows
az vm extension delete --resource-group "myResourceGroup1" --vm-name "myVM1" -n "AzureNetworkWatcherExtension"

```

Uzantıyı yeniden yükler.

```azurecli
#Linux command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher"

#Windows command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher"

```

#### <a name="option-3-reboot-your-vms"></a>Seçenek 3: VM 'lerinizi yeniden başlatın

Ağ Izleyicisi uzantısı için otomatik yükseltmeyi doğru olarak ayarlarsanız, VM yüklemenizi en son uzantıya yeniden başlatın.

## <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa [Linux](./network-watcher-linux.md) veya [Windows](./network-watcher-windows.md)için ağ izleyicisi uzantısı belgelerine bakın. Ayrıca [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla da iletişime geçin. Alternatif olarak, bir Azure destek olayı dosyası. [Azure destek sitesine](https://azure.microsoft.com/support/options/)gidin ve **Destek Al**' ı seçin. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.
