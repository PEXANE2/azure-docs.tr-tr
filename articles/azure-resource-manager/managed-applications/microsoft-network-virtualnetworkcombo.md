---
title: VirtualNetworkCombo UI öğesi
description: Azure portal için Microsoft. Network. VirtualNetworkCombo UI öğesini açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: 53c9653b44a6c9d26d49d37b351cf6000676e2d4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75651975"
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a>Microsoft. Network. VirtualNetworkCombo UI öğesi

Yeni veya var olan bir sanal ağı seçmek için bir denetim grubu.

## <a name="ui-sample"></a>UI örneği

Kullanıcı yeni bir sanal ağ seçtiğinde, Kullanıcı her bir alt ağın adını ve adres önekini özelleştirebilir. Alt ağları yapılandırma isteğe bağlıdır.

![Microsoft. Network. VirtualNetworkCombo yeni](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-new.png)

Kullanıcı var olan bir sanal ağı seçtiğinde, dağıtım şablonu için gereken her bir alt ağı eşleştirmelidir. Bu durumda alt ağların yapılandırılması gerekir.

![Microsoft. Network. VirtualNetworkCombo var](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-existing.png)

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

- Belirtilmişse, boyutun ilk çakışmayan adres ön eki, `defaultValue.addressPrefixSize` kullanıcının aboneliğindeki var olan sanal ağlara göre otomatik olarak belirlenir.
- Ve için varsayılan değer `defaultValue.name` `defaultValue.addressPrefixSize` **null**.
- `constraints.minAddressPrefixSize`belirtilmelidir. Belirtilen değerden daha küçük bir adres alanına sahip var olan sanal ağlar seçim için kullanılamaz.
- `subnets`belirtilmelidir ve `constraints.minAddressPrefixSize` her alt ağ için belirtilmelidir.
- Yeni bir sanal ağ oluştururken, her alt ağın adres öneki, sanal ağın adres ön eki ve ilgili olarak otomatik olarak hesaplanır `addressPrefixSize` .
- Var olan bir sanal ağ kullanılırken, karşılık gelen alt ağlar `constraints.minAddressPrefixSize` seçim için kullanılamaz. Ayrıca, belirtilmişse, en az kullanılabilir adreslere sahip olmayan alt ağlar `minAddressCount` seçim için kullanılamaz. Varsayılan değer **0**' dır. Kullanılabilir adreslerin bitişik olduğundan emin olmak için, için **true değerini** belirtin `requireContiguousAddresses` . Varsayılan değer **true**'dur.
- Mevcut bir sanal ağda alt ağların oluşturulması desteklenmez.
- `options.hideExisting` **Doğru**ise, Kullanıcı var olan bir sanal ağı seçemiyorum. Varsayılan değer **false** şeklindedir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
