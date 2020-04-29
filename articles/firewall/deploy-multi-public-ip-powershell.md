---
title: PowerShell kullanarak birden çok genel IP adresi ile Azure Güvenlik Duvarı dağıtma
description: Bu makalede, Azure PowerShell kullanarak birden çok genel IP adresi ile Azure Güvenlik Duvarı dağıtmayı öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 04/14/2020
ms.author: victorh
ms.openlocfilehash: 21f645e64c9944ed102f538710ea6facc26c7e83
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81314025"
---
# <a name="deploy-an-azure-firewall-with-multiple-public-ip-addresses-using-azure-powershell"></a>Azure PowerShell'i kullanarak birden çok IP adresiyle Azure Firewall dağıtma

Bu özellik aşağıdaki senaryolara izin vermez:

- **DNAT** -birden çok standart bağlantı noktası örneğini arka uç sunucularınıza çevirebilirsiniz. Örneğin, iki genel IP adresiniz varsa, TCP bağlantı noktası 3389 ' ü (RDP) her iki IP adresi için de çevirebilirsiniz.
- **SNAT** -giden SNAT bağlantıları için ek bağlantı noktaları kullanılabilir ve bu, SNAT bağlantı noktası tükenmesi potansiyelini azaltır. Azure Güvenlik Duvarı şu anda bir bağlantı için kullanılacak kaynak genel IP adresini rastgele seçer. Ağınızda herhangi bir aşağı akış filtresi varsa, güvenlik duvarınızdan ilişkili tüm genel IP adreslerine izin vermeniz gerekir.
 
Birden çok genel IP adresine sahip Azure Güvenlik Duvarı Azure portal, Azure PowerShell, Azure CLı, REST ve şablonlar aracılığıyla kullanılabilir. Azure Güvenlik Duvarı 'Nı en fazla 100 genel IP adresi ile dağıtabilirsiniz.

Aşağıdaki Azure PowerShell örneklerde, Azure Güvenlik Duvarı için genel IP adreslerini nasıl yapılandırabileceğiniz, ekleyebileceğiniz ve kaldırabilmeniz gösterilmektedir.

> [!NOTE]
> İlk IP adresini Azure Güvenlik Duvarı genel IP adresi yapılandırması sayfasından kaldıramazsınız. IP adresini değiştirmek istiyorsanız Azure PowerShell kullanabilirsiniz.

## <a name="create-a-firewall-with-two-or-more-public-ip-addresses"></a>İki veya daha fazla genel IP adresi ile bir güvenlik duvarı oluşturun

Bu örnek, iki genel IP adresi ile sanal ağ *VNET* 'e eklenmiş bir güvenlik duvarı oluşturur.

```azurepowershell
$rgName = "resourceGroupName"

$vnet = Get-AzVirtualNetwork `
  -Name "vnet" `
  -ResourceGroupName $rgName

$pip1 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

$pip2 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp2" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

New-AzFirewall `
  -Name "azFw" `
  -ResourceGroupName $rgName `
  -Location centralus `
  -VirtualNetwork $vnet `
  -PublicIpAddress @($pip1, $pip2)
```

## <a name="add-a-public-ip-address-to-an-existing-firewall"></a>Mevcut bir güvenlik duvarına genel IP adresi ekleme

Bu örnekte, *azFwPublicIp1* genel IP adresi güvenlik duvarına iliştirilir.

```azurepowershell
$pip = New-AzPublicIpAddress `
  -Name "azFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

$azFw = Get-AzFirewall `
  -Name "AzureFirewall" `
  -ResourceGroupName "rg"

$azFw.AddPublicIpAddress($pip)

$azFw | Set-AzFirewall
```

## <a name="remove-a-public-ip-address-from-an-existing-firewall"></a>Mevcut bir güvenlik duvarındaki genel IP adresini kaldırma

Bu örnekte, *azFwPublicIp1* genel IP adresi güvenlik duvarından ayrılır.

```azurepowershell
$pip = Get-AzPublicIpAddress `
  -Name "azFwPublicIp1" `
  -ResourceGroupName "rg"

$azFw = Get-AzFirewall `
  -Name "AzureFirewall" `
  -ResourceGroupName "rg"

$azFw.RemovePublicIpAddress($pip)

$azFw | Set-AzFirewall
```

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: birden çok genel IP adresi ile Azure Güvenlik Duvarı oluşturma-Kaynak Yöneticisi şablonu](quick-create-multiple-ip-template.md)
