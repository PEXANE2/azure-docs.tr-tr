---
title: Publicıpaddresscombo UI öğesi
description: Azure portal için Microsoft. Network. Publicıpaddresscombo UI öğesini açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: 0393673663df8f3ca580ff34e16bee910b955f8d
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75651923"
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Microsoft. Network. Publicıpaddresscombo UI öğesi

Yeni veya var olan bir genel IP adresini seçmek için bir denetim grubu.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Kullanıcı genel IP adresi için ' none ' seçerse, etki alanı adı etiketi metin kutusu gizlidir.
- Kullanıcı var olan bir genel IP adresini seçerse, etki alanı adı etiketi metin kutusu devre dışı bırakılır. Değeri, seçilen IP adresinin etki alanı adı etikettir.
- Etki alanı adı soneki (örneğin, westus.cloudapp.azure.com), seçilen konuma göre otomatik olarak güncelleştirilir.

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

Kullanıcı hiçbir ortak IP adresi seçerse, Denetim aşağıdaki çıktıyı döndürür:

```json
{
  "newOrExistingOrNone": "none"
}
```

Kullanıcı yeni veya mevcut bir IP adresi seçerse, Denetim aşağıdaki çıktıyı döndürür:

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

- `options.hideNone` **true**olarak belirtildiğinde, `newOrExistingOrNone` yalnızca **Yeni** veya **varolan**bir değere sahip olur.
- `options.hideDomainNameLabel` **true**olarak belirtildiğinde `domainNameLabel` bildirilmemiş olur.

## <a name="remarks"></a>Açıklamalar

- `constraints.required.domainNameLabel` **true**olarak ayarlanırsa, yeni BIR genel IP adresi oluştururken kullanıcının bir etki alanı adı etiketi sağlaması gerekir. Etiketi olmayan mevcut genel IP adresleri seçim için kullanılamaz.
- `options.hideNone` **true**olarak ayarlanırsa, genel IP adresi için **hiçbirini** seçme seçeneği gizlenir. Varsayılan değer **false**.
- `options.hideDomainNameLabel` **true**olarak ayarlanırsa, etki alanı adı etiketinin metin kutusu gizlenir. Varsayılan değer **false**.
- `options.hideExisting` true ise, Kullanıcı var olan bir genel IP adresini seçemeyebilir. Varsayılan değer **false**.
- `zone`için, yalnızca belirtilen bölge veya bölge dayanıklı genel IP adresleri için genel IP adresleri kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
