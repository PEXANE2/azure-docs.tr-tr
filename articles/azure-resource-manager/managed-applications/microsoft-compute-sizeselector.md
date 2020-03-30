---
title: SizeSelector UI öğesi
description: Azure portalı için Microsoft.Compute.SizeSelector UI öğesini açıklar. Sanal makinenin boyutunu seçmek için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: d98ddbb09ac4f1f933237b3288db7a0cb78380b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75652053"
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Microsoft.Compute.SizeSelector UI öğesi

Bir veya daha fazla sanal makine örnekleri için boyut seçmek için bir denetim.

## <a name="ui-sample"></a>UI örneği

Kullanıcı, öğe tanımından varsayılan değerleri olan bir seçici görür.

![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

Denetimi seçtikten sonra, kullanıcı kullanılabilir boyutların genişletilmiş görünümünü görür.

![Microsoft.Compute.SizeSelector genişletildi](./media/managed-application-elements/microsoft.compute.sizeselector-expanded.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": [],
    "numAvailabilityZonesRequired": 3,
    "zone": "3"
  },
  "options": {
    "hideDiskTypeFilter": false
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
"Standard_D1"
```

## <a name="remarks"></a>Açıklamalar

- `recommendedSizes`en az bir boyutu olmalıdır. Önerilen ilk boyut varsayılan olarak kullanılır. Kullanılabilir boyutlar listesi önerilen duruma göre sıralanmıyor. Kullanıcı önerilen duruma göre sıralamak için bu sütunu seçebilirsiniz.
- Önerilen boyut seçili konumda kullanılamıyorsa, boyut otomatik olarak atlanır. Bunun yerine, sonraki önerilen boyut kullanılır.
- `constraints.allowedSizes`ve `constraints.excludedSizes` her ikisi de isteğe bağlıdır, ancak aynı anda kullanılamaz. Kullanılabilir boyutların [listesi, bir abonelik için kullanılabilir sanal makine boyutları listesi'ni](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)arayarak belirlenebilir. Belirtilen herhangi bir `constraints.allowedSizes` boyut gizlidir ve belirtilmeyen `constraints.excludedSizes` herhangi bir boyut gösterilir.
- `osPlatform`belirtilmelidir ve **Windows** veya **Linux**olabilir. Sanal makinelerin donanım maliyetlerini belirlemek için kullanılır.
- `imageReference`birinci taraf görüntüler için atlanır, ancak üçüncü taraf görüntüler için sağlanır. Sanal makinelerin yazılım maliyetlerini belirlemek için kullanılır.
- `count`öğe için uygun çarpanı ayarlamak için kullanılır. **2**gibi statik bir değeri veya başka bir öğeden `[steps('step1').vmCount]`dinamik bir değeri destekler. Varsayılan değer **1'dir.**
- 1, `numAvailabilityZonesRequired` 2 veya 3 olabilir.
- Varsayılan olarak, `hideDiskTypeFilter` **yanlıştır.** Disk türü filtresi, kullanıcının tüm disk türlerini veya yalnızca SSD'yi görmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
