---
title: PowerShell ile bir abonelikteki tüm VM’lerle ilgili ayrıntıları toplama
description: PowerShell ile bir abonelikteki tüm VM’lerle ilgili ayrıntıları toplama
services: virtual-machines-windows
documentationcenter: virtual-machines
author: v-miegge
manager: dcscontentpm
editor: v-miegge
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/01/2019
ms.author: v-miegge
ms.custom: mvc
ms.openlocfilehash: 8d61da9c3a3d575fcbce0b77e1f8b2684df58106
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87171685"
---
# <a name="collect-details-about-all-vms-in-a-subscription-with-powershell"></a>PowerShell ile bir abonelikteki tüm VM’lerle ilgili ayrıntıları toplama

Bu betik VM adı, kaynak grubu adı, bölge, VM boyutu, sanal ağ, alt ağ, özel IP adresi, işletim sistemi türü ve belirtilen abonelikteki VM 'Lerin genel IP adresini içeren bir CSV oluşturur.

[Azure aboneliğiniz](../../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell’i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca, ' a giderek ayrı bir tarayıcı sekmesinde Cloud Shell de başlatabilirsiniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) . **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="sample-script"></a>Örnek betik

```azurepowershell-interactive
#Provide the subscription Id where the VMs reside
$subscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

#Provide the name of the csv file to be exported
$reportName = "myReport.csv"

Select-AzSubscription $subscriptionId
$report = @()
$vms = Get-AzVM
$publicIps = Get-AzPublicIpAddress 
$nics = Get-AzNetworkInterface | ?{ $_.VirtualMachine -NE $null} 
foreach ($nic in $nics) { 
    $info = "" | Select VmName, ResourceGroupName, Region, VmSize, VirtualNetwork, Subnet, PrivateIpAddress, OsType, PublicIPAddress, NicName, ApplicationSecurityGroup 
    $vm = $vms | ? -Property Id -eq $nic.VirtualMachine.id 
    foreach($publicIp in $publicIps) { 
        if($nic.IpConfigurations.id -eq $publicIp.ipconfiguration.Id) {
            $info.PublicIPAddress = $publicIp.ipaddress
            } 
        } 
        $info.OsType = $vm.StorageProfile.OsDisk.OsType 
        $info.VMName = $vm.Name 
        $info.ResourceGroupName = $vm.ResourceGroupName 
        $info.Region = $vm.Location 
        $info.VmSize = $vm.HardwareProfile.VmSize
        $info.VirtualNetwork = $nic.IpConfigurations.subnet.Id.Split("/")[-3] 
        $info.Subnet = $nic.IpConfigurations.subnet.Id.Split("/")[-1] 
        $info.PrivateIpAddress = $nic.IpConfigurations.PrivateIpAddress 
        $info.NicName = $nic.Name 
        $info.ApplicationSecurityGroup = $nic.IpConfigurations.ApplicationSecurityGroups.Id 
        $report+=$info 
    } 
$report | ft VmName, ResourceGroupName, Region, VmSize, VirtualNetwork, Subnet, PrivateIpAddress, OsType, PublicIPAddress, NicName, ApplicationSecurityGroup 
$report | Export-CSV "$home/$reportName"
```

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, bir abonelikteki VM 'lerin ayrıntılarının CSV dışa aktarılmasını oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

|Komut|Notlar|
|-|-|
|[Select-AzSubscription](/powershell/module/az.accounts/set-azcontext)|Geçerli oturumda kullanılacak cmdlet 'lerin kiracı, abonelik ve ortamını ayarlar.|
|[Get-AzVM](/powershell/module/az.compute/get-azvm)|Sanal makinenin özelliklerini alır.|
|[Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress)|Genel bir IP adresi alır.|
|[Get-Aznetworkınterface](/powershell/module/az.network/get-aznetworkinterface)|Bir ağ arabirimini alır.|

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=/azure/virtual-machines/windows/toc.json) içinde bulunabilir.
