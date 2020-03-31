---
title: VirtualNetworkCombo UI öğesi
description: Azure portalı için Microsoft.Network.VirtualNetworkCombo UI öğesini açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: 53c9653b44a6c9d26d49d37b351cf6000676e2d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75651975"
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a>Microsoft.Network.VirtualNetworkCombo UI öğesi

Yeni veya varolan bir sanal ağ seçmek için bir denetim grubu.

## <a name="ui-sample"></a>UI örneği

Kullanıcı yeni bir sanal ağ seçtiğinde, kullanıcı her alt netin adını ve adres önekini özelleştirebilir. Alt ağları yapılandırmak isteğe bağlıdır.

![Microsoft.Network.VirtualNetworkCombo yeni](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-new.png)

Kullanıcı varolan bir sanal ağı seçtiğinde, kullanıcıdağıtım şablonununun varolan bir alt ağa gerektirdiği her alt ağı eşlemelidir. Bu durumda alt ağları yapılandırmak gereklidir.

![Microsoft.Network.VirtualNetworkCombo mevcut](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-existing.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Network.VirtualNetworkCombo",
  "label": {
    "virtualNetwork": "Virtual network",
    "subnets": "Subnets"
  },
  "toolTip": {
    "virtualNetwork": "",
    "subnets": ""
  },
  "defaultValue": {
    "name": "vnet01",
    "addressPrefixSize": "/16"
  },
  "constraints": {
    "minAddressPrefixSize": "/16"
  },
  "options": {
    "hideExisting": false
  },
  "subnets": {
    "subnet1": {
      "label": "First subnet",
      "defaultValue": {
        "name": "subnet-1",
        "addressPrefixSize": "/24"
      },
      "constraints": {
        "minAddressPrefixSize": "/24",
        "minAddressCount": 12,
        "requireContiguousAddresses": true
      }
    },
    "subnet2": {
      "label": "Second subnet",
      "defaultValue": {
        "name": "subnet-2",
        "addressPrefixSize": "/26"
      },
      "constraints": {
        "minAddressPrefixSize": "/26",
        "minAddressCount": 8,
        "requireContiguousAddresses": true
      }
    }
  },
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
{
  "name": "vnet01",
  "resourceGroup": "rg01",
  "addressPrefixes": ["10.0.0.0/16"],
  "newOrExisting": "new",
  "subnets": {
    "subnet1": {
      "name": "subnet-1",
      "addressPrefix": "10.0.0.0/24",
      "startAddress": "10.0.0.1"
    },
    "subnet2": {
      "name": "subnet-2",
      "addressPrefix": "10.0.1.0/26",
      "startAddress": "10.0.1.1"
    }
  }
}
```

## <a name="remarks"></a>Açıklamalar

- Belirtilirse, ilk çakışmayan adres öneki, `defaultValue.addressPrefixSize` kullanıcının aboneliğindeki varolan sanal ağlara göre otomatik olarak belirlenir.
- Varsayılan değer `defaultValue.name` için `defaultValue.addressPrefixSize` ve **null**.
- `constraints.minAddressPrefixSize`belirtilmelidir. Belirtilen değerden daha küçük bir adres alanı olan varolan sanal ağlar seçim için kullanılamaz.
- `subnets`belirtilmelidir ve `constraints.minAddressPrefixSize` her alt ağ için belirtilmelidir.
- Yeni bir sanal ağ oluşturulurken, her alt netin adres öneki, sanal ağın adres `addressPrefixSize`öneki ve ilgili .
- Varolan bir sanal ağ kullanırken, ilgiliden `constraints.minAddressPrefixSize` daha küçük alt ağlar seçim için kullanılamaz. Ayrıca, belirtilmişse, en az `minAddressCount` kullanılabilir adresleri olmayan alt ağlar seçim için kullanılamaz. Varsayılan değer **0'dır.** Kullanılabilir adreslerin bitişik olduğundan emin olmak **true** için, `requireContiguousAddresses`'' için geçerli olduğunu belirtin. Varsayılan değer **doğrudur.**
- Varolan bir sanal ağda alt ağlar oluşturma desteklenmez.
- `options.hideExisting` **Doğruysa,** kullanıcı varolan bir sanal ağı seçemez. Varsayılan değer **false** şeklindedir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
