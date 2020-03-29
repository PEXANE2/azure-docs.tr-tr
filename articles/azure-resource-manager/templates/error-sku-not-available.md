---
title: SKU kullanılabilir hatalar değil
description: Azure Kaynak Yöneticisi ile kaynak dağıtırken SKU kullanılabilir olmayan hatayı nasıl gidermeyeceğiniaçıklar.
ms.topic: troubleshooting
ms.date: 02/18/2020
ms.openlocfilehash: 3dcc26f2d74799a6d282ee4bd733d36bec7b05e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942723"
---
# <a name="resolve-errors-for-sku-not-available"></a>Kullanılamayan SKU’larla ilgili hataları giderme

Bu makalede, **SkuNotAvailable** hatasınasıl çözüleceği açıklanmaktadır. O bölgede/bölgede veya iş gereksinimlerinizi karşılayan alternatif bir bölge/bölgede uygun bir SKU bulamıyorsanız, Azure Desteği'ne bir [SKU isteği](https://aka.ms/skurestriction) gönderin.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="symptom"></a>Belirti

Bir kaynak (genellikle sanal makine) dağıtırken, aşağıdaki hata kodu ve hata iletisini alırsınız:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>'
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>Nedeni

Seçtiğiniz kaynak SKU (VM boyutu gibi) seçtiğiniz konum için kullanılamadığında bu hatayı alırsınız.

Bir Azure Spot VM veya Spot ölçeği seti örneği dağıtıyorsanız, bu konumda Azure Spot için kapasite yoktur. Daha fazla bilgi için [Bkz. Spot hata iletileri.](../../virtual-machines/error-codes-spot.md)

## <a name="solution-1---powershell"></a>Çözüm 1 - PowerShell

Bir bölgede/bölgede hangi SKU'ların kullanılabilip kullanılabilip bulunabilmek için [Get-AzComputeResourceSku](/powershell/module/az.compute/get-azcomputeresourcesku) komutunu kullanın. Sonuçları konuma göre filtreleyin. Bu komut için PowerShell'in en son sürümüne sahip olmalısınız.

```azurepowershell-interactive
Get-AzComputeResourceSku | where {$_.Locations -icontains "centralus"}
```

Sonuçlar, konum için SKU'ların bir listesini ve bu SKU için herhangi bir kısıtlamayı içerir. Bir SKU'nun `NotAvailableForSubscription`.

```output
ResourceType          Name           Locations   Zone      Restriction                      Capability           Value
------------          ----           ---------   ----      -----------                      ----------           -----
virtualMachines       Standard_A0    centralus             NotAvailableForSubscription      MaxResourceVolumeMB   20480
virtualMachines       Standard_A1    centralus             NotAvailableForSubscription      MaxResourceVolumeMB   71680
virtualMachines       Standard_A2    centralus             NotAvailableForSubscription      MaxResourceVolumeMB  138240
virtualMachines       Standard_D1_v2 centralus   {2, 1, 3}                                  MaxResourceVolumeMB
```

Bazı ek örnekler:

```azurepowershell-interactive
Get-AzComputeResourceSku | where {$_.Locations.Contains("centralus") -and $_.ResourceType.Contains("virtualMachines") -and $_.Name.Contains("Standard_DS14_v2")}
Get-AzComputeResourceSku | where {$_.Locations.Contains("centralus") -and $_.ResourceType.Contains("virtualMachines") -and $_.Name.Contains("v3")} | fc
```

Sonunda "fc" ekledaha fazla ayrıntı döndürür.

## <a name="solution-2---azure-cli"></a>Çözüm 2 - Azure CLI

Bir bölgede hangi SNU'ların kullanılabilerini belirlemek için komutu `az vm list-skus` kullanın. Çıktıyı `--location` kullandığınız konuma filtrelemek için parametreyi kullanın. Kısmi `--size` boyut adına göre arama yapmak için parametreyi kullanın.

```azurecli-interactive
az vm list-skus --location southcentralus --size Standard_F --output table
```

Komut sonuçları döndürür:

```output
ResourceType     Locations       Name              Zones    Capabilities    Restrictions
---------------  --------------  ----------------  -------  --------------  --------------
virtualMachines  southcentralus  Standard_F1                ...             None
virtualMachines  southcentralus  Standard_F2                ...             None
virtualMachines  southcentralus  Standard_F4                ...             None
...
```


## <a name="solution-3---azure-portal"></a>Çözüm 3 - Azure portalı

Bir bölgede hangi SNU'ların mevcut olduğunu belirlemek için [portalı](https://portal.azure.com)kullanın. Portalda oturum açın ve arayüz üzerinden bir kaynak ekleyin. Değerleri ayarlarken, bu kaynak için kullanılabilir SNU'ları görürsünüz. Dağıtımı tamamlamanız gerekmez.

Örneğin, sanal bir makine oluşturma işlemini başlatın. Kullanılabilir diğer boyutu görmek **için, boyutu değiştir'i**seçin.

![VM oluşturma](./media/error-sku-not-available/create-vm.png)

Kullanılabilir boyutlarda filtre uygulayabilir ve kaydırabilirsiniz.

![Kullanılabilir SKU'lar](./media/error-sku-not-available/available-sizes.png)

## <a name="solution-4---rest"></a>Çözüm 4 - REST

Bir bölgede hangi SDO'ların kullanılabileni belirlemek için [Kaynak Skus - Liste](/rest/api/compute/resourceskus/list) işlemini kullanın.

Kullanılabilir SNU'ları ve bölgeleri aşağıdaki biçimde döndürür:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    ...
  ]
}
```

