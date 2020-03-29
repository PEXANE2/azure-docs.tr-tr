---
title: PublicIpAddressCombo UI öğesi
description: Azure portalı için Microsoft.Network.PublicIpAddressCombo UI öğesini açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: 0393673663df8f3ca580ff34e16bee910b955f8d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75651923"
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Microsoft.Network.PublicIpAddressCombo UI öğesi

Yeni veya varolan genel IP adresini seçmek için bir denetim grubu.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Kullanıcı genel IP adresi için 'Yok' seçerse, alan adı etiketi metin kutusu gizlenir.
- Kullanıcı varolan genel bir IP adresi seçerse, etki alanı adı etiketi metin kutusu devre dışı bırakılır. Değeri, seçili IP adresinin alan adı etiketidir.
- Etki alanı adı soneki (örneğin, westus.cloudapp.azure.com) seçili konuma göre otomatik olarak güncellenir.

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Network.PublicIpAddressCombo",
  "label": {
    "publicIpAddress": "Public IP address",
    "domainNameLabel": "Domain name label"
  },
  "toolTip": {
    "publicIpAddress": "",
    "domainNameLabel": ""
  },
  "defaultValue": {
    "publicIpAddressName": "ip01",
    "domainNameLabel": "mydomain"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false,
    "zone": 3
  },
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

Kullanıcı ortak IP adresi seçmezse, denetim aşağıdaki çıktıyı döndürür:

```json
{
  "newOrExistingOrNone": "none"
}
```

Kullanıcı yeni veya varolan bir IP adresi seçerse, denetim aşağıdaki çıktıyı döndürür:

```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "mydomain",
  "publicIPAllocationMethod": "Dynamic",
  "sku": "Basic",
  "newOrExistingOrNone": "new"
}
```

- Doğru `options.hideNone` olarak **true** `newOrExistingOrNone` belirtildiğinde, yalnızca **yeni** veya **varolan**bir değere sahip olacaktır.
- Doğru `options.hideDomainNameLabel` olarak **true**belirtildiğinde, `domainNameLabel` bildirilmemiştir.

## <a name="remarks"></a>Açıklamalar

- `constraints.required.domainNameLabel` **Doğru**ayarlanmışsa, kullanıcı yeni bir genel IP adresi oluştururken bir etki alanı adı etiketi sağlamalıdır. Etiketi olmayan varolan genel IP adresleri seçim için kullanılamaz.
- Doğru `options.hideNone` olarak **true**ayarlanmışsa, genel IP adresi için **Yok** seçeneğini seçme seçeneği gizlenir. Varsayılan değer **false** şeklindedir.
- Doğru `options.hideDomainNameLabel` olarak **true**ayarlanmışsa, etki alanı adı etiketi için metin kutusu gizlenir. Varsayılan değer **false** şeklindedir.
- Doğruysa, `options.hideExisting` kullanıcı varolan genel bir IP adresini seçemez. Varsayılan değer **false** şeklindedir.
- Çünkü, `zone`yalnızca belirtilen bölge veya bölge esnek genel IP adresleri için genel IP adresleri kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
