---
title: PowerShell'i kullanarak birden çok genel IP adresiyle Azure Güvenlik Duvarı'nı dağıtma
description: Bu makalede, Azure PowerShell'i kullanarak birden çok genel IP adresine sahip bir Azure Güvenlik Duvarı'nı nasıl dağıtabileceğinizi öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 04/14/2020
ms.author: victorh
ms.openlocfilehash: 21f645e64c9944ed102f538710ea6facc26c7e83
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81314025"
---
# <a name="deploy-an-azure-firewall-with-multiple-public-ip-addresses-using-azure-powershell"></a>Azure PowerShell'i kullanarak birden çok IP adresiyle Azure Firewall dağıtma

Bu özellik aşağıdaki senaryoları sağlar:

- **DNAT** - Birden çok standart bağlantı noktası örneğini arka uç sunucularınıza çevirebilirsiniz. Örneğin, iki genel IP adresiniz varsa, her iki IP adresi için de TCP bağlantı noktası 3389'u (RDP) çevirebilirsiniz.
- **SNAT** - Giden SNAT bağlantıları için ek bağlantı noktaları kullanılabilir ve bu da SNAT bağlantı noktası tükenmesi potansiyelini azaltır. Şu anda, Azure Güvenlik Duvarı bağlantı için kullanılacak kaynak genel IP adresini rasgele seçer. Ağınızda herhangi bir alt akış filtresi varsa, güvenlik duvarınızla ilişkili tüm genel IP adreslerine izin verebilirsiniz.
 
Birden çok genel IP adresine sahip Azure Güvenlik Duvarı, Azure portalı, Azure PowerShell, Azure CLI, REST ve şablonlar aracılığıyla kullanılabilir. En fazla 100 genel IP adresine sahip bir Azure Güvenlik Duvarı dağıtabilirsiniz.

Aşağıdaki Azure PowerShell örnekleri, Azure Güvenlik Duvarı için genel IP adreslerini nasıl yapılandırabileceğinizi, ekleyebileceğiniz ve kaldırabileceğinizi gösterir.

> [!NOTE]
> Azure Güvenlik Duvarı genel IP adresi yapılandırma sayfasından ilk ipConfiguration'ı kaldıramazsınız. IP adresini değiştirmek istiyorsanız Azure PowerShell'i kullanabilirsiniz.

## <a name="create-a-firewall-with-two-or-more-public-ip-addresses"></a>İki veya daha fazla genel IP adresiyle bir güvenlik duvarı oluşturma

Bu örnek, iki genel IP adresine sahip sanal ağ *vnet'ine* bağlı bir güvenlik duvarı oluşturur.

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

## <a name="add-a-public-ip-address-to-an-existing-firewall"></a>Varolan bir güvenlik duvarına genel IP adresi ekleme

Bu örnekte, genel IP adresi *azFwPublicIp1* güvenlik duvarına eklenir.

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

## <a name="remove-a-public-ip-address-from-an-existing-firewall"></a>Genel BIR IP adresini varolan bir güvenlik duvarından kaldırma

Bu örnekte, genel IP adresi *azFwPublicIp1* güvenlik duvarından ayrılmıştır.

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

* [Hızlı başlangıç: Birden çok genel IP adresiyle bir Azure Güvenlik Duvarı oluşturma - Kaynak Yöneticisi şablonu](quick-create-multiple-ip-template.md)
