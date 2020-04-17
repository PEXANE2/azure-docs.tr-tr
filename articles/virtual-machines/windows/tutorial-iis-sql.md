---
title: Öğretici - Azure'da SQL, IIS, .NET yığını çalıştıran VM'ler oluşturun
description: Bu öğreticide, Azure’daki bir Windows sanal makinesinde Azure SQL, IIS, .NET yığını yüklemeyi öğrenirsiniz.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/05/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: e67f4dfebc00897b7a4ed4d675159a848e4b2fdf
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81455691"
---
# <a name="tutorial-install-the-sql-iis-net-stack-in-a-windows-vm-with-azure-powershell"></a>Öğretici: Azure PowerShell ile Windows VM'de SQL, IIS, .NET yığınını yükleme

Bu öğreticide, Azure PowerShell kullanarak bir SQL, IIS, .NET yığını yüklüyoruz. Bu yığın, Windows Server 2016’da çalışan, biri IIS ve .NET, diğeri SQL Server’a sahip iki sanal makineden oluşur.

> [!div class="checklist"]
> * VM oluşturma 
> * Sanal makineye IIS ve .NET Core SDK’sı yükleme
> * SQL Server çalıştıran bir sanal makine oluşturma
> * SQL Server uzantısı yükleme

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca bulut shell'i ayrı bir tarayıcı [https://shell.azure.com/powershell](https://shell.azure.com/powershell)sekmesinde başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="create-an-iis-vm"></a>IIS VM oluşturma 

Bu örnekte, PowerShell Cloud Shell'de Hızlı bir Windows Server 2016 VM oluşturmak ve ardından IIS ve .NET Framework'ü yüklemek için [Yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet kullanıyoruz. IIS ve SQL sanal makineleri bir kaynak grubu ile sanal ağ paylaşır, dolayısıyla bu adlara yönelik değişkenler oluştururuz.


```azurepowershell-interactive
$vmName = "IISVM"
$vNetName = "myIISSQLvNet"
$resourceGroup = "myIISSQLGroup"
New-AzVm `
    -ResourceGroupName $resourceGroup `
    -Name $vmName `
    -Location "East US" `
    -VirtualNetworkName $vNetName `
    -SubnetName "myIISSubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -AddressPrefix 192.168.0.0/16 `
    -PublicIpAddressName "myIISPublicIpAddress" `
    -OpenPorts 80,3389 
```

[Set-AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) cmdlet ile özel komut dosyası uzantısını kullanarak IIS ve .NET çerçevesini yükleyin.

```azurepowershell-interactive
Set-AzVMExtension `
    -ResourceGroupName $resourceGroup `
    -ExtensionName IIS `
    -VMName $vmName `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features"}' `
    -Location EastUS
```

## <a name="create-another-subnet"></a>Başka bir alt ağ oluşturma

SQL sanal makinesi için ikincil alt ağ oluşturun. [Get-AzVirtualNetwork]{/powershell/module/az.network/get-azvirtualnetwork} kullanarak vNet'i alın.

```azurepowershell-interactive
$vNet = Get-AzVirtualNetwork `
   -Name $vNetName `
   -ResourceGroupName $resourceGroup
```

[Add-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/add-azvirtualnetworksubnetconfig)kullanarak alt ağ için bir yapılandırma oluşturun.


```azurepowershell-interactive
Add-AzVirtualNetworkSubnetConfig `
   -AddressPrefix 192.168.0.0/24 `
   -Name mySQLSubnet `
   -VirtualNetwork $vNet `
   -ServiceEndpoint Microsoft.Sql
```

[Set-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetwork) kullanarak vNet'i yeni alt ağ bilgileriyle güncelleştirin
   
```azurepowershell-interactive   
$vNet | Set-AzVirtualNetwork
```

## <a name="azure-sql-vm"></a>Azure SQL VM

SQL VM oluşturmak için bir SQL Server'ın önceden yapılandırılmış Azure Market görüntüsünü kullanın. İlk olarak VM’i oluştururuz, ardından bu VM’e SQL Server Uzantısı’nı yükleriz. 


```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName $resourceGroup `
    -Name "mySQLVM" `
    -ImageName "MicrosoftSQLServer:SQL2016SP1-WS2016:Enterprise:latest" `
    -Location eastus `
    -VirtualNetworkName $vNetName `
    -SubnetName "mySQLSubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "mySQLPublicIpAddress" `
    -OpenPorts 3389,1401 
```

[SQL Server uzantısını](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) SQL VM'ye eklemek için [Set-AzVMSqlServerExtension'ı](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsqlserverextension) kullanın.

```azurepowershell-interactive
Set-AzVMSqlServerExtension `
   -ResourceGroupName $resourceGroup  `
   -VMName mySQLVM `
   -Name "SQLExtension" `
   -Location "EastUS"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure PowerShell kullanarak SQL&#92;IIS&#92;.NET yığını yüklediniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * VM oluşturma 
> * Sanal makineye IIS ve .NET Core SDK’sı yükleme
> * SQL Server çalıştıran bir sanal makine oluşturma
> * SQL Server uzantısı yükleme

TLS/SSL sertifikaları ile IIS web sunucusunun nasıl güvenli hale getirilebildiğini öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [TLS/SSL sertifikalarına sahip güvenli IIS web sunucusu](tutorial-secure-web-server.md)

