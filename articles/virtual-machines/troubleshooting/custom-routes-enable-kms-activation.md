---
title: Zorunlu tünelleme ile KMS etkinleştirmesini etkinleştirmek için Azure özel rotalarını kullanın | Microsoft Dokümanlar
description: Azure'da zorunlu tünel leme kullanırken KMS etkinleştirmesini etkinleştirmek için Azure özel yollarının nasıl kullanılacağını gösterir.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 12/20/2018
ms.author: genli
ms.openlocfilehash: 90034a56fcf5211059d37270e12391249f7a16b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920170"
---
# <a name="windows-activation-fails-in-forced-tunneling-scenario"></a>Windows etkinleştirme zorunlu tünel senaryosunda başarısız olur

Bu makalede, siteden siteye VPN bağlantısında veya ExpressRoute senaryolarında zorunlu tünel yapılmasını etkinleştirdiğinizde karşılaşabileceğiniz KMS etkinleştirme sorunu nasıl çözüleceğiniz açıklanmaktadır.

## <a name="symptom"></a>Belirti

Internet'e bağlı tüm trafiği şirket içi ağınıza yönlendirmek için Azure sanal ağ alt ağlarında [zorunlu tünel ler](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) yapılmasını sağlarsınız. Bu senaryoda, Windows çalıştıran Azure sanal makineleri (VM'ler) Windows'u etkinleştirmez.

## <a name="cause"></a>Nedeni

Azure Windows VM'lerinin Windows etkinleştirme için Azure KMS sunucusuna bağlanması gerekir. Etkinleştirme, etkinleştirme isteğinin Azure genel IP adresinden gelmesini gerektirir. Zorunlu tünelleme senaryosunda, etkinleştirme isteği Azure genel IP adresi yerine şirket içi ağınızdan geldiği için etkinleştirme başarısız olur.

## <a name="solution"></a>Çözüm

Bu sorunu gidermek için etkinleştirme trafiğini Azure KMS sunucusuna yönlendirmek için Azure özel rotasını kullanın.

Azure Global bulutu için KMS sunucusunun IP adresi 23.102.135.246'dır. DNS adı kms.core.windows.net. Azure Almanya gibi diğer Azure platformlarını kullanıyorsanız, ilgili KMS sunucusunun IP adresini kullanmanız gerekir. Daha fazla bilgi için aşağıdaki tabloya bakın:

|Platform| KMS DNS|KMS IP|
|------|-------|-------|
|Azure Global|kms.core.windows.net|23.102.135.246|
|Azure Almanya|kms.core.cloudapi.de|51.4.143.248|
|Azure US Government|kms.core.usgovcloudapi.net|23.97.0.13|
|Azure Çin 21Vianet|kms.core.chinacloudapi.cn|42.159.7.249|


Özel rotaeklemek için aşağıdaki adımları izleyin:

### <a name="for-resource-manager-vms"></a>Kaynak Yöneticisi VM'leri için

 

> [!NOTE] 
> Etkinleştirme ortak IP adreslerini kullanır ve Standart SKU Yük Dengeleyici yapılandırması tarafından etkilenir. Gereksinimler hakkında bilgi edinmek için [Azure'daki Giden bağlantıları](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) dikkatle inceleyin.

1. Azure PowerShell'i açın ve ardından [Azure aboneliğinizde oturum açın.](https://docs.microsoft.com/powershell/azure/authenticate-azureps)
2. Aşağıdaki komutları çalıştırın:

    ```powershell
    # First, get the virtual network that hosts the VMs that have activation problems. In this case, we get virtual network ArmVNet-DM in Resource Group ArmVNet-DM:

    $vnet = Get-AzVirtualNetwork -ResourceGroupName "ArmVNet-DM" -Name "ArmVNet-DM"

    # Next, create a route table and specify that traffic bound to the KMS IP (23.102.135.246) will go directly out:

    $RouteTable = New-AzRouteTable -Name "ArmVNet-DM-KmsDirectRoute" -ResourceGroupName "ArmVNet-DM" -Location "centralus"

    Add-AzRouteConfig -Name "DirectRouteToKMS" -AddressPrefix 23.102.135.246/32 -NextHopType Internet -RouteTable $RouteTable

    Set-AzRouteTable -RouteTable $RouteTable

    # Next, attach the route table to the subnet that hosts the VMs

    Set-AzVirtualNetworkSubnetConfig -Name "Subnet01" -VirtualNetwork $vnet -AddressPrefix "10.0.0.0/24" -RouteTable $RouteTable

    Set-AzVirtualNetwork -VirtualNetwork $vnet
    ```
3. Etkinleştirme sorunları olan VM'ye gidin. KMS sunucusuna erişip erişemeyebileceğini test etmek için [PsPing'i](https://docs.microsoft.com/sysinternals/downloads/psping) kullanın:

        psping kms.core.windows.net:1688

4. Windows'u etkinleştirmeye çalışın ve sorunun çözülüp çözülmediğini görün.

### <a name="for-classic-vms"></a>Klasik VM'ler için

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

1. Azure PowerShell'i açın ve ardından [Azure aboneliğinizde oturum açın.](https://docs.microsoft.com/powershell/azure/authenticate-azureps)
2. Aşağıdaki komutları çalıştırın:

    ```powershell
    # First, create a new route table:
    New-AzureRouteTable -Name "VNet-DM-KmsRouteGroup" -Label "Route table for KMS" -Location "Central US"

    # Next, get the route table that was created:
    $rt = Get-AzureRouteTable -Name "VNet-DM-KmsRouteTable"

    # Next, create a route:
    Set-AzureRoute -RouteTable $rt -RouteName "AzureKMS" -AddressPrefix "23.102.135.246/32" -NextHopType Internet

    # Apply the KMS route table to the subnet that hosts the problem VMs (in this case, we apply it to the subnet that's named Subnet-1):
    Set-AzureSubnetRouteTable -VirtualNetworkName "VNet-DM" -SubnetName "Subnet-1" 
    -RouteTableName "VNet-DM-KmsRouteTable"
    ```

3. Etkinleştirme sorunları olan VM'ye gidin. KMS sunucusuna erişip erişemeyebileceğini test etmek için [PsPing'i](https://docs.microsoft.com/sysinternals/downloads/psping) kullanın:

        psping kms.core.windows.net:1688

4. Windows'u etkinleştirmeye çalışın ve sorunun çözülüp çözülmediğini görün.

## <a name="next-steps"></a>Sonraki adımlar

- [KMS İstemcisi Kurulum Anahtarları](https://docs.microsoft.com/windows-server/get-started/kmsclientkeys
)
- [Etkinleştirme Yöntemlerini Gözden Geçirin ve Seçin](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11)
)
