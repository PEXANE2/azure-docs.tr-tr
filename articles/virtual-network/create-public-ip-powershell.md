---
title: Genel IP Azure PowerShell oluşturma
description: Azure PowerShell kullanarak genel IP oluşturmayı öğrenin
services: virtual-network
documentationcenter: na
author: blehr
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2020
ms.author: blehr
ms.openlocfilehash: 6174d108fd80df9725ca5ef0fb9296dfffaf4a64
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89303520"
---
# <a name="quickstart-create-a-public-ip-address-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell kullanarak genel IP adresi oluşturma

Bu makalede, Azure PowerShell kullanarak genel IP adresi kaynağı oluşturma gösterilmektedir. Bu kaynakların hangi kaynaklarla ilişkilendiribileceği hakkında daha fazla bilgi için, temel ve standart SKU arasındaki fark ve diğer ilgili bilgiler için bkz. [genel IP adresleri](https://docs.microsoft.com/azure/virtual-network/public-ip-addresses).  Bu örnekte yalnızca IPv4 adreslerine odaklanacağız; IPv6 adresleri hakkında daha fazla bilgi için bkz. [Azure VNET Için IPv6](https://docs.microsoft.com/azure/virtual-network/ipv6-overview).

## <a name="prerequisites"></a>Ön koşullar

- Azure PowerShell yerel olarak veya Azure Cloud Shell yüklendi

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell 'i yerel olarak çalıştırıyorsanız `Connect-AzAccount` Azure ile bir bağlantı oluşturmak için öğesini de çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

**Eastus2** konumunda **Myresourcegroup** adlı [New-azresourcegroup](/powershell/module/az.resources/new-azresourcegroup) adlı bir kaynak grubu oluşturun.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroup'
$loc = 'eastus2'

New-AzResourceGroup -Name $rg -Location $loc
```
---
# <a name="standard-sku---using-zones"></a>[**Standart SKU-bölgeleri kullanma**](#tab/option-create-public-ip-standard-zones)

>[!NOTE]
>Aşağıdaki komut API sürüm 2020-08-01 veya üzeri için geçerlidir.  Kullanılmakta olan API sürümü hakkında daha fazla bilgi için lütfen [kaynak sağlayıcıları ve türleri](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-providers-and-types)inceleyin.

**Myresourcegroup**Içinde **Mystandardzrpublicıp** adlı standart bölge-yedekli genel IP adresi oluşturmak Için [New-azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) komutunu kullanın.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroup'
$loc = 'eastus2'
$pubIP = 'myStandardZRPublicIP'
$sku = 'Standard'
$alloc = 'Static'
$zone = 1,2,3

New-AzPublicIpAddress -ResourceGroupName $rg -Name $pubIP -Location $loc -AllocationMethod $alloc -SKU $sku -zone $zone
```
> [!IMPORTANT]
> API 'nin 2020-08-01 'den eski sürümleri için, bölgesel olarak yedekli bir IP adresi oluşturmak üzere bölge parametresi belirtmeden yukarıdaki komutu çalıştırın. 
>

**Myresourcegroup**Içinde **Mystandardzonalpublicıp** adlı bölge 2 standart bir genel IP adresi oluşturmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroup'
$loc = 'eastus2'
$pubIP = 'myStandardZonalPublicIP'
$sku = 'Standard'
$alloc = 'Static'
$zone = 2

New-AzPublicIpAddress -ResourceGroupName $rg -Name $pubIP -Location $loc -AllocationMethod $alloc -SKU $sku -zone $zone
```

Bölgeler için yukarıdaki seçeneklerin yalnızca [kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones)bölgelerinde geçerli seçimler olduğunu unutmayın.

# <a name="standard-sku---no-zones"></a>[**Standart SKU-bölge yok**](#tab/option-create-public-ip-standard)

>[!NOTE]
>Aşağıdaki komut API sürüm 2020-08-01 veya üzeri için geçerlidir.  Kullanılmakta olan API sürümü hakkında daha fazla bilgi için lütfen [kaynak sağlayıcıları ve türleri](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-providers-and-types)inceleyin.

**Myresourcegroup**Içinde **mystandardpublicıp** ADıNDA bir standart genel IP adresi oluşturmak Için [New-azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) komutunu kullanın.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroup'
$loc = 'eastus2'
$pubIP = 'myStandardPublicIP'
$sku = 'Standard'
$alloc = 'Static'

New-AzPublicIpAddress -ResourceGroupName $rg -Name $pubIP -Location $loc -AllocationMethod $alloc -SKU $sku
```

Bu seçim tüm bölgelerde geçerlidir ve [kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones)olmayan bölgelerde standart genel IP adresleri için varsayılan seçimdir.

# <a name="basic-sku"></a>[**Temel SKU**](#tab/option-create-public-ip-basic)

**Myresourcegroup**Içinde **mystandardpublicıp** adlı temel bir statik genel IP adresi oluşturmak Için [New-azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) komutunu kullanın.  Temel genel IP 'Lerde kullanılabilirlik alanları kavramı yoktur.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroup'
$loc = 'eastus2'
$pubIP = 'myBasicPublicIP'
$sku = 'Basic'
$alloc = 'Static'

New-AzPublicIpAddress -ResourceGroupName $rg -Name $pubIP -Location $loc -AllocationMethod $alloc -SKU $sku
```
IP adresinin zaman içinde değiştirilmesi kabul edilebilir ise, AllocationMethod yöntemi ' Dynamic ' olarak değiştirilerek **dinamik** IP ataması seçilebilir.

---

## <a name="additional-information"></a>Ek bilgiler 

Yukarıda listelenen bağımsız değişkenlerle ilgili daha fazla ayrıntı için lütfen bkz. [genel IP adreslerini yönetme](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address#create-a-public-ip-address).

## <a name="next-steps"></a>Sonraki adımlar
- [Genel IP adresini bir sanal makineyle](https://docs.microsoft.com/azure/virtual-network/associate-public-ip-address-vm#azure-portal) ilişkilendir
- Azure 'da [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) hakkında daha fazla bilgi edinin.
- Tüm [genel IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address)hakkında daha fazla bilgi edinin.