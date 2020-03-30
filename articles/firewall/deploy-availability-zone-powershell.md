---
title: PowerShell'i kullanarak Kullanılabilirlik Bölgeleri yle Azure Güvenlik Duvarı'nı dağıtma
description: Bu makalede, Azure PowerShell'i kullanarak Kullanılabilirlik Bölgeleri ile bir Azure Güvenlik Duvarı'nı nasıl dağıtabileceğinizi öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: 33dcebf14f4d534962783a30ec94f7ff6529ae0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74195919"
---
# <a name="deploy-an-azure-firewall-with-availability-zones-using-azure-powershell"></a>Azure PowerShell kullanarak Kullanılabilirlik Alanlarıyla Azure Firewall'u dağıtma

Azure Güvenlik Duvarı, daha fazla kullanılabilirlik için birden çok Kullanılabilirlik Bölgesine yayılacak şekilde dağıtım sırasında yapılandırılabilir.

Bu özellik aşağıdaki senaryoları sağlar:

- Kullanılabilirliği %99,99 çalışma süresine çıkarabilirsiniz. Daha fazla bilgi için Azure Güvenlik Duvarı [Hizmet Düzeyi Sözleşmesi'ne (SLA)](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/)bakın. İki veya daha fazla Kullanılabilirlik Bölgesi seçildiğinde %99,99 çalışma süresi SLA sunulur.
- Ayrıca, hizmet standardını %99,95 SLA kullanarak Azure Güvenlik Duvarını sadece yakınlık nedenleriyle belirli bir bölgeyle ilişkilendirebilirsiniz.

Azure Güvenlik Duvarı Kullanılabilirlik Bölgeleri hakkında daha fazla bilgi için Azure [Güvenlik Duvarı nedir?](overview.md)

Aşağıdaki Azure PowerShell örneği, Kullanılabilirlik Bölgeleri ile bir Azure Güvenlik Duvarı'nı nasıl dağıtabileceğinizi gösterir.

## <a name="create-a-firewall-with-availability-zones"></a>Kullanılabilirlik Bölgeleri ile bir güvenlik duvarı oluşturma

Bu örnek, 1, 2 ve 3 bölgelerinde bir güvenlik duvarı oluşturur.

Standart genel IP adresi oluşturulduğunda, belirli bir bölge belirtilmemiştir. Bu varsayılan olarak bölge gereksiz BIR IP adresi oluşturur. Standart genel IP adresleri tüm bölgelerde veya tek bir bölgede yapılandırılabilir.

Bilmeniz önemlidir, çünkü bölge 1'de bir güvenlik duvarınız ve bölge 2'de bir IP adresiniz olamaz. Ancak, tüm bölgelerde bölge 1 ve IP adresinde bir güvenlik duvarı veya yakınlık amacıyla aynı tek bölgede bir güvenlik duvarı ve IP adresi olabilir.

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

New-AzFirewall `
  -Name "azFw" `
  -ResourceGroupName $rgName `
  -Location centralus `
  -VirtualNetwork $vnet `
  -PublicIpAddress @($pip1)
  -Zone 1,2,3
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)
