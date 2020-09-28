---
title: Ağ Izleyicisi uzantısını en son sürüme güncelleştirin
description: Ağ Izleyicisi uzantısını en son sürüme güncelleştirmeyi öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: damendo
manager: balar
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.workload: infrastructure-services
ms.date: 09/23/2020
ms.author: damendo
ms.openlocfilehash: c386685d63894472623ffc4392a529541a91391c
ms.sourcegitcommit: b48e8a62a63a6ea99812e0a2279b83102e082b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2020
ms.locfileid: "91410363"
---
# <a name="how-to-update-the-network-watcher-extension-to-the-latest-the-version"></a>Ağ Izleyicisi uzantısını en son sürüme güncelleştirme 

## <a name="overview"></a>Genel Bakış

[Azure Ağ İzleyicisi](../../network-watcher/network-watcher-monitoring-overview.md) , Azure ağlarını izlemeye izin veren bir ağ performansı izleme, tanılama ve analiz hizmetidir. Ağ Izleyicisi Aracısı sanal makine uzantısı, istek üzerine ağ trafiği yakalama ve Azure sanal makinelerinde diğer gelişmiş işlevler için bir gereksinimdir. Ağ Izleyicisi uzantısı bağlantı Izleyici, bağlantı Izleyicisi (Önizleme), bağlantı sorunlarını giderme ve paket yakalama gibi özellikler tarafından kullanılır.   

## <a name="prerequisites"></a>Önkoşullar
Bu belge, sanal makinenizde ağ Izleyicisi uzantısının yüklü olduğunu varsayar ve en son sürüme güncelleştirmek için yönergeler sağlar. 

## <a name="latest-version"></a>En son sürüm
Ağ Izleyicisi uzantısının en son sürümü şu anda `1.4.1654.1` .

## <a name="updating-your-extension"></a>Uzantınızı güncelleştirme 

### <a name="check-your-extension-version"></a>Uzantı sürümünüzü denetleyin  

**Azure portal kullanma**

1. Azure portal sanal makinenizin ' Uzantılar ' dikey penceresine gidin.   
2. Ayrıntılar bölmesini görmek için ' Azurenetworkizleyici ' uzantısına tıklayın.  
3. ' Sürüm ' alanındaki sürüm numarasını bulun.  

**Azure CLI 'Yi kullanma** Bir Azure CLı isteminde aşağıdaki komutu çalıştırın.   

```azurecli
az vm extension list --resource-group  <ResourceGroupName> --vm-name <VMName>
```

Çıktıda Azurenetworkizleyici uzantısını bulun ve çıktıda "TypeHandlerVersion" alanından sürüm numarasını belirleyin.  


**PowerShell 'ı kullanma** Bir PowerShell isteminden aşağıdaki komutları çalıştırın:   

```powershell
Get-AzVMExtension -ResourceGroupName <ResourceGroupName> -VMName <VMName>  
```

Çıktıda Azurenetworkizleyici uzantısını bulun ve çıktıda "TypeHandlerVersion" alanından sürüm numarasını belirleyin.   


### <a name="update-your-extension"></a>Uzantınızı güncelleştirme

Büyük bir deyişle sürümünüz, `1.4.1654.1` aşağıdaki seçeneklerden herhangi birini kullanarak uzantınızı güncelleştirin (geçerli en son sürüm). 

**Seçenek 1: PowerShell kullanma**

```powershell
#Linux command
Set-AzVMExtension `  -ResourceGroupName "myResourceGroup1" `  -Location "WestUS" `  -VMName "myVM1" `  -Name "AzureNetworkWatcherExtension" `  -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentLinux"   

#Windows command
Set-AzVMExtension `  -ResourceGroupName "myResourceGroup1" `  -Location "WestUS" `  -VMName "myVM1" `  -Name "AzureNetworkWatcherExtension" `  -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentWindows"   
```


**2. seçenek: Azure CLı kullanma**  

Yükseltmeyi zorla 

```azurecli
#Linux command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher" --force-update

#Windows command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher" --force-update
```

Bu işe yaramazsa. Aşağıdaki adımları kullanarak uzantıyı kaldırın ve yeniden yüklemeyi deneyin. Bu, en son sürümü otomatik olarak ekler. 

Uzantı kaldırılıyor 

```azurecli
#Same for Linux and Windows
az vm extension delete --resource-group "myResourceGroup1" --vm-name "myVM1" -n "AzureNetworkWatcherExtension"

```

Uzantıyı yeniden yükleme

```azurecli
#Linux command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher"  

#Windows command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher" 

```

**Seçenek 3: VM 'lerinizi yeniden başlatın**

Networkizleyici uzantısı için otomatik yükseltmeniz true olarak ayarlandıysa. SANAL makinenizin yeniden başlatılması en son uzantıyı yükler.


## <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, ağ Izleyicisi uzantısı belgelerine ([Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-linux), [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-windows)) başvurabilir veya [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek Al ' ı seçin. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.
