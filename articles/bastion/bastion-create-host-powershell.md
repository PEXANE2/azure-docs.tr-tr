---
title: Azure PowerShell kullanarak bir savunma ana bilgisayarı oluşturun | Microsoft Docs
description: Bu makalede, Azure savunma Konağı oluşturmayı öğrenin
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 10/13/2020
ms.author: cherylmc
ms.openlocfilehash: f0e7a66ef7d6947306f1b2ffec54b8e9d12737d7
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92018602"
---
# <a name="create-an-azure-bastion-host-using-azure-powershell"></a>Azure PowerShell kullanarak bir Azure savunma ana bilgisayarı oluşturma

Bu makalede, PowerShell kullanarak bir Azure savunma ana bilgisayarı oluşturma işlemi gösterilmektedir. Savunma 'yı dağıttıktan sonra, Azure portal kullanarak sanal makineye kendi özel IP adresi aracılığıyla bağlanabilirsiniz. VM 'nizin genel bir IP adresi, ek bir istemci veya özel bir yazılım olması gerekmez. Azure savunma dağıtımı, abonelik/hesap veya sanal makine başına değil, sanal ağ başına değildir. Sorunsuz RDP/SSH deneyimi, aynı sanal ağdaki tüm VM 'Ler tarafından kullanılabilir.

Bu yönergeler PowerShell dağıtımı içindir. Ayrıca, [Azure Portal](tutorial-create-host-portal.md) veya [Azure CLI](create-host-cli.md)kullanarak bir Azure savunma ana bilgisayarı da oluşturabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

[!INCLUDE [PowerShell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="create-a-bastion-host"></a><a name="createhost"></a>Savunma Konağı oluşturma

Bu bölüm, Azure PowerShell kullanarak sanal ağınız için yeni bir Azure savunma kaynağı oluşturmanıza yardımcı olur.

1. Bir sanal ağ ve bir Azure savunma alt ağı oluşturun. **AzureBastionSubnet**ad değerini kullanarak Azure savunma alt ağını oluşturmanız gerekir. Bu değer, Azure 'un savunma kaynaklarını hangi alt ağa dağıtacağınızı bilmesini sağlar. Bu, bir ağ geçidi alt ağından farklıdır. En az/27 veya daha büyük alt ağın (/27,/26, vb.) bir alt ağını kullanmanız gerekir. Rota tabloları veya temsilcileri olmadan **AzureBastionSubnet** oluşturun. **AzureBastionSubnet**üzerinde ağ güvenlik grupları kullanıyorsanız [Nsgs ile çalışma](bastion-nsg.md) makalesine başvurun.

   ```azurepowershell-interactive
   $subnetName = "AzureBastionSubnet"
   $subnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   $vnet = New-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myBastionRG" -Location "westeurope" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

2. Azure savunma için genel bir IP adresi oluşturun. Genel IP, RDP/SSH 'ye erişilecek savunma kaynağına genel IP adresidir (443 numaralı bağlantı noktası üzerinden). Genel IP adresi, oluşturmakta olduğunuz savunma kaynağıyla aynı bölgede olmalıdır.

   ```azurepowershell-interactive
   $publicip = New-AzPublicIpAddress -ResourceGroupName "myBastionRG" -name "myPublicIP" -location "westeurope" -AllocationMethod Static -Sku Standard
   ```

3. Sanal ağınızın AzureBastionSubnet yeni bir Azure savunma kaynağı oluşturun. Savunma kaynağının oluşturulması ve dağıtılması yaklaşık 5 dakika sürer.

   ```azurepowershell-interactive
   $bastion = New-AzBastion -ResourceGroupName "myBastionRG" -Name "myBastion" -PublicIpAddress $publicip -VirtualNetwork $vnet
   ```

## <a name="next-steps"></a>Sonraki adımlar

* Bir sanal makineye bağlanın.
   * [Linux VM](bastion-connect-vm-ssh.md)
   * [Windows VM](bastion-connect-vm-rdp.md)
