---
title: Genel IP adreslerini yükseltme
titleSuffix: Azure Virtual Network
description: Genel IP adreslerini temel sunucudan standart sürümüne yükseltin.
services: virtual-network
documentationcenter: na
author: blehr
editor: ''
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.subservice: ip-services
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/08/2020
ms.author: blehr
ms.custom: references_regions , devx-track-azurecli
ms.openlocfilehash: 9ea29c47349fd7ccee469188f8929a864cf7bbef
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96905800"
---
# <a name="upgrade-public-ip-addresses"></a>Genel IP adreslerini yükseltme

Azure genel IP adresleri, işlevlerinin yönlerini (ayırma yöntemi, kullanılabilirlik alanları genelinde kullanım ve bunlarla ilişkilendirilebilen kaynakları dahil) belirleyen bir SKU 'SU (temel veya standart) ile oluşturulur. 

Bu makalede aşağıdaki senaryolar incelenmelidir:
* Temel SKU genel IP 'sini standart SKU genel IP 'ye yükseltme (portal, PowerShell veya CLı kullanarak)
* Klasik bir Azure Ayrılmış IP Azure Resource Manager temel SKU genel IP 'ye geçirme

## <a name="upgrade-public-ip-address-from-basic-to-standard-sku"></a>Genel IP adresini temel sunucudan standart SKU 'ya yükseltme

>[!NOTE]
>Genel IP 'Leri temel ile standart arasında yükseltme yeteneği, tüm bölgelerde kullanılamaz.  Daha fazla ayrıntı için lütfen [**sınırlamalara**](#limitations) bakın.

Genel bir IP 'yi yükseltmek için, bu, herhangi bir kaynakla ilişkilendirilmemelidir (genel IP 'Lerin ilişkisini kaldırma hakkında daha fazla bilgi için [Bu sayfaya](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address#view-modify-settings-for-or-delete-a-public-ip-address) bakın).

>[!IMPORTANT]
>Temel bilgisayarlardan standart SKU 'ya yükseltilen genel IP 'Ler, hiçbir [kullanılabilirlik](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones)alanına sahip olmaya devam eder.  Bu, bölgesel olarak yedekli olan veya bu, sunulan bölgelerde önceden belirtilmiş bir bölgeye bağlı bir Azure kaynağıyla ilişkilendirilemeyeceği anlamına gelir.

---
# <a name="basic-to-standard---powershell"></a>[**Temel-standart-PowerShell**](#tab/option-upgrade-powershell)

Aşağıdaki örnek, [Bu sayfada](https://docs.microsoft.com/azure/virtual-network/create-public-ip-powershell?tabs=option-create-public-ip-basic) , **myresourcegroup** Içinde temel genel IP **mybasicpublicıp** 'Si Ile verilen ÖRNEĞI kullanarak, temel SKU genel IP 'sini önceden oluşturmayı varsayar.

IP 'yi yükseltmek için PowerShell kullanarak aşağıdaki komutları yürütün.  Unutmayın IP adresi zaten statik olarak ayrılmışsa, bu bölüm atlanabilir.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroup'
$name = 'myBasicPublicIP'
$newsku = 'Standard'
$pubIP = Get-AzPublicIpAddress -name $name -ResourceGroupName $rg

## This section is only needed if the Basic IP is not already set to Static ##
$pubIP.PublicIpAllocationMethod = 'Static'
Set-AzPublicIpAddress -PublicIpAddress $pubIP

## This section is for conversion to Standard ##
$pubIP.Sku.Name = $newsku
Set-AzPublicIpAddress -PublicIpAddress $pubIP
```

# <a name="basic-to-standard---cli"></a>[**Temel-standart-CLı**](#tab/option-upgrade-cli)

Aşağıdaki örnek, [Bu sayfada](https://docs.microsoft.com/azure/virtual-network/create-public-ip-cli?tabs=option-create-public-ip-basic) , **myresourcegroup** Içinde temel genel IP **mybasicpublicıp** 'Si Ile verilen ÖRNEĞI kullanarak, temel SKU genel IP 'sini önceden oluşturmayı varsayar.

IP 'yi yükseltmek için, yalnızca Azure CLı kullanarak aşağıdaki komutları yürütün.  Unutmayın IP adresi zaten statik olarak ayrılmışsa, bu bölüm atlanabilir.

```azurecli-interactive
## Variables for the command ##
$rg = 'myResourceGroup'
$name = 'myBasicPublicIP'

## This section is only needed if the Basic IP is not already set to Static ##
az network public-ip update \
--resource-group $rg \
--name $name \
--allocation-method Static 

## This section is for conversion to Standard ##
az network public-ip update \
--resource-group $rg \
--name $name \
--sku Standard
```
---

## <a name="upgrade-migrate-a-classic-reserved-ip-to-a-static-public-ip"></a>Klasik bir Ayrılmış IP statik bir genel IP 'ye yükseltme (geçiş)

Azure Resource Manager yeni becerilerinin avantajlarından yararlanmak için, ayrılmış IP 'Ler adlı mevcut genel statik IP adresini (klasik modelden modern Azure Resource Manager modeline geçirebilirsiniz) geçirebilirsiniz.  Geçirilen genel IP, temel bir SKU türü olacaktır.


---

# <a name="reserved-to-basic---powershell"></a>[**Temel-PowerShell 'e ayrılmıştır**](#tab/option-migrate-powershell)

Aşağıdaki örnekte, **Myresourcegroup** içinde klasik bir Azure ayrılmış IP **myreservedip** 'nin önceki bir sürümünün oluşturulması varsayılmaktadır. Geçiş için başka bir önkoşul de Azure Resource Manager aboneliğin geçiş için kaydolmasında emin olunması sağlamaktır. Bu, [bu sayfanın](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-ps)3. ve 4. adımlarında ayrıntılı olarak ele alınmıştır.

Ayrılmış IP geçirmek için PowerShell kullanarak aşağıdaki komutları yürütün.  Not IP adresi herhangi bir hizmetle ilişkili değilse (aşağıda **hizmetim** adlı bir hizmet varsa), bu adım atlanabilir.

```azurepowershell-interactive
## Variables for the command ##
$name = 'myReservedIP'

## This section is only needed if the Reserved IP is not already disassociated from any Cloud Services ##
$serviceName = 'myService'
Remove-AzureReservedIPAssociation -ReservedIPName $name -ServiceName $service

$validate = Move-AzureReservedIP -ReservedIPName $name -Validate
$validate.ValidationMessages
```
Önceki komut, geçişi engelleyen tüm uyarıları ve hataları görüntüler. Doğrulama başarılı olursa, geçişi hazırlamak ve yürütmek için aşağıdaki adımlarla devam edebilirsiniz:
```azurepowershell-interactive
Move-AzureReservedIP -ReservedIPName $name -Prepare
Move-AzureReservedIP -ReservedIPName $name -Commit
```
Azure Resource Manager ' deki yeni bir kaynak grubu, geçirilen Ayrılmış IP adı kullanılarak oluşturulur (Yukarıdaki örnekte, kaynak grubu **Myreservedip-geçirilmiş** olur).

# <a name="reserved-to-basic---cli"></a>[**Temel CLı 'ye ayrılmıştır**](#tab/option-migrate-cli)

Aşağıdaki örnekte, **Myresourcegroup** içinde klasik bir Azure ayrılmış IP **myreservedip** 'nin önceki bir sürümünün oluşturulması varsayılmaktadır. Geçiş için başka bir önkoşul de Azure Resource Manager aboneliğin geçiş için kaydolmasında emin olunması sağlamaktır. Bu, [bu sayfanın](https://docs.microsoft.com/azure/virtual-machines/linux/migration-classic-resource-manager-cli)3. ve 4. adımlarında ayrıntılı olarak ele alınmıştır.

Ayrılmış IP geçirmek için, Azure CLı kullanarak aşağıdaki komutları yürütün.  Not IP adresi herhangi bir hizmetle ilişkili değilse (aşağıda **hizmetim** ve dağıtım **mydeployment** adlı bir hizmet varsa), bu adım atlanabilir.

```azurecli-interactive
## Variables for the command ##
$name = 'myReservedIP'

## This section is only needed if the Reserved IP is not already disassociated from any Cloud Services ##
$service = 'myService'
$deployment = 'myDeployment'
azure network reserved-ip disassociate $name $service $deployment

azure network reserved-ip validate-migration $name
```
Önceki komut, geçişi engelleyen tüm uyarıları ve hataları görüntüler. Doğrulama başarılı olursa, geçişi hazırlamak ve yürütmek için aşağıdaki adımlarla devam edebilirsiniz:
```azurecli-interactive
azure network reserved-ip prepare-migration $name
azure network reserved-ip commit-migration $name
```
Azure Resource Manager ' deki yeni bir kaynak grubu, geçirilen Ayrılmış IP adı kullanılarak oluşturulur (Yukarıdaki örnekte, kaynak grubu **Myreservedip-geçirilmiş** olur).

---

## <a name="limitations"></a>Sınırlamalar

* Bu özellik şu anda şu bölgelerde kullanılamıyor:<br>
US Gov Virginia<br>
Doğu US DoD<br>
Orta US DoD<br>
Doğu Çin<br>
Çin Doğu 2<br>
Kuzey Çin<br>
Çin Kuzey 2

* Temel bir genel IP 'yi yükseltmek için, herhangi bir Azure kaynağıyla ilişkilendirilemez.  Genel IP 'Lerin ilişkisini kaldırma hakkında daha fazla bilgi için lütfen [Bu sayfayı](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address#view-modify-settings-for-or-delete-a-public-ip-address) gözden geçirin.  Benzer şekilde, bir Ayrılmış IP geçirmek için herhangi bir bulut hizmeti ile ilişkilendirilemez.  Ayrılmış IP 'Lerin ilişkisini kaldırma hakkında daha fazla bilgi için lütfen [Bu sayfayı](https://docs.microsoft.com/azure/virtual-network/remove-public-ip-address-vm) gözden geçirin.  
* Temel ve standart SKU 'ya yükseltilen genel IP 'Ler, hiçbir [kullanılabilirlik](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones) alanına sahip olmaya devam eder ve bu nedenle bölge yedekli veya zonal olan bir Azure kaynağıyla ilişkilendirilemez.  Bu, yalnızca kullanılabilirlik alanları sunan bölgeler için geçerlidir.
* Standart 'dan temel 'e indirgeyemezsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

- Azure 'daki [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) hakkında daha fazla bilgi edinmek IÇIN, SKU türleri arasındaki farkı ve [genel IP adresi ayarlarını](virtual-network-public-ip-address.md#create-a-public-ip-address)öğrenin.
- [Azure genel yük dengeleyicilerini temel 'Ten standart sürümüne yükseltmeyi](https://docs.microsoft.com/azure/load-balancer/upgrade-basic-standard)öğrenin.
- [Klasik Azure ayrılmış IP 'leri](https://docs.microsoft.com/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip) ve [Klasik kaynakların Azure Resource Manager geçişini](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview)anlayın.
