---
title: Azure Powershell'i kullanarak Bir Burç ana bilgisayar oluşturma | Microsoft Dokümanlar
description: Bu makalede, Azure Kalesi ana bilgisayar nasıl oluşturulacak öğrenin
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: cherylmc
ms.openlocfilehash: 43d834f0c834696cd4a836466c9663fe7c31a392
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80520510"
---
# <a name="create-an-azure-bastion-host-using-azure-powershell"></a>Azure PowerShell'i kullanarak Azure Bastion ana bilgisayar oluşturma

Bu makalede, PowerShell kullanarak bir Azure Bastion ana bilgisayar oluşturmak için nasıl gösterir. Azure Bastion hizmetini sanal ağınızda kullanıma saðlamaniz sonra, sorunsuz RDP/SSH deneyimi ayný sanal ağdaki tüm Sanal M'ler için kullanılabilir. Azure Bastion dağıtımı, abonelik/hesap veya sanal makine başına değil, sanal ağ başınadır.

İsteğe bağlı olarak, [Azure portalını](bastion-create-host-portal.md)kullanarak bir Azure Bastion ana bilgisayarı oluşturabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="create-a-bastion-host"></a><a name="createhost"></a>Bastion konağı oluşturma

Bu bölüm, Azure PowerShell'i kullanarak yeni bir Azure Bastion kaynağı oluşturmanıza yardımcı olur.

1. Sanal ağ ve Azure Bastion alt ağı oluşturun. **AzureBastionSubnet**ad değerini kullanarak Azure Bastion alt netini oluşturmanız gerekir. Bu değer, Azure'un Bastion kaynaklarını hangi alt ağa dağıtacağını bilmesini sağlar. Bu, Ağ Geçidi alt ağından farklıdır. En az /27 veya daha büyük bir alt ağ (/27, /26 vb.) alt ağı kullanmalısınız. **AzureBastionSubnet'i** herhangi bir rota tabloları veya delegasyonları olmadan oluşturun. **AzureBastionSubnet'te**Ağ Güvenlik Grupları kullanıyorsanız, [NSG'lerle Çalışma](bastion-nsg.md) makalesine bakın.

   ```azurepowershell-interactive
   $subnetName = "AzureBastionSubnet"
   $subnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   $vnet = New-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myBastionRG" -Location "westeurope" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

2. Azure Bastion için genel bir IP adresi oluşturun. Genel IP, RDP/SSH'ye erişilen Bastion kaynağının ortak IP adresidir (bağlantı noktası 443'ten fazladır). Ortak IP adresi, oluşturduğunuz Bastion kaynağıyla aynı bölgede olmalıdır.

   ```azurepowershell-interactive
   $publicip = New-AzPublicIpAddress -ResourceGroupName "myBastionRG" -name "myPublicIP" -location "westeurope" -AllocationMethod Static -Sku Standard
   ```

3. Sanal ağınızın AzureBastionSubnet'inde yeni bir Azure Bastion kaynağı oluşturun. Bastion kaynağının oluşturması ve dağıtması yaklaşık 5 dakika sürer.

   ```azurepowershell-interactive
   $bastion = New-AzBastion -ResourceGroupName "myBastionRG" -Name "myBastion" -PublicIpAddress $publicip -VirtualNetwork $vnet
   ```

## <a name="next-steps"></a>Sonraki adımlar

* Ek bilgi için [Bastion SSS'yi](bastion-faq.md) okuyun.

* Azure Bastion alt ağıyla Ağ Güvenlik Grupları'nı kullanmak için Bkz. [NSG'lerle Çalışma.](bastion-nsg.md)
