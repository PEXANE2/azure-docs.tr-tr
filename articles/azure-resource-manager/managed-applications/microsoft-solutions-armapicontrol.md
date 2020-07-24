---
title: ArmApiControl UI öğesi
description: Azure portal için Microsoft. Solutions. ArmApiControl Kullanıcı arabirimi öğesini açıklar. API işlemlerini çağırmak için kullanılır.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: tomfitz
ms.openlocfilehash: bbe36e072d10b81c421331b2212d8b161afd2693
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099910"
---
# <a name="microsoftcommonarmapicontrol-ui-element"></a>Microsoft. Common. ArmApiControl UI öğesi

ArmApiControl, Azure Resource Manager API işleminin sonuçlarını almanızı sağlar. Diğer denetimlerde dinamik içeriği doldurmak için sonuçları kullanın.

## <a name="ui-sample"></a>UI örneği

Bu denetim için Kullanıcı arabirimi yok.

## <a name="schema"></a>Şema

Aşağıdaki örnekte bu denetimin şeması gösterilmektedir:

```json
{
    "name": "testApi",
    "type": "Microsoft.Solutions.ArmApiControl",
    "request": {
        "method": "{HTTP-method}",
        "path": "{path-for-the-URL}",
        "body": {
            "key1": "val1",
            "key2": "val2"
        }
    }
}
```

## <a name="sample-output"></a>Örnek çıktı

Denetimin çıktısı kullanıcıya gösterilmez. Bunun yerine, işlemin sonucu diğer denetimlerde kullanılır.

## <a name="remarks"></a>Açıklamalar

- `request.method`ÖZELLIĞI http yöntemini belirtir. Yalnızca `GET` veya `POST` buna izin verilir.
- `request.path`Özelliği, URL 'nin göreli yolunu belirtir. Bu bir statik yol olabilir veya diğer denetimlerin çıkış değerleriyle dinamik olarak oluşturulabilir.
- `request.body`Özelliği isteğe bağlıdır. İstekle birlikte gönderilen bir JSON gövdesi belirtmek için bunu kullanın. Gövde statik içerik olabilir veya diğer denetimlerden çıkış değerlerine başvurarak dinamik olarak oluşturulabilir.

## <a name="example"></a>Örnek

Aşağıdaki örnekte, `providersApi` öğesi, sağlayıcı nesnelerinin bir dizisini almak için BIR API çağırır.

`allowedValues`Öğesinin özelliği, `providersDropDown` sağlayıcılarının adlarını almak için yapılandırılır. Onları açılan listede görüntüler.

```json
{
    "name": "providersApi",
    "type": "Microsoft.Solutions.ArmApiControl",
    "request": {
        "method": "GET",
        "path": "[concat(subscription().id, '/providers/Microsoft.Network/expressRouteServiceProviders?api-version=2019-02-01')]"
    }
},
{
    "name": "providerDropDown",
    "type": "Microsoft.Common.DropDown",
    "label": "Provider",
    "toolTip": "The provider that offers the express route connection.",
    "constraints": {
        "allowedValues": "[map(steps('settings').providersApi.value, (item) => parse(concat('{\"label\":\"', item.name, '\",\"value\":\"', item.name, '\"}')))]",
        "required": true
    },
    "visible": true
}
```

Bir kaynak adının kullanılabilirliğini denetlemek için ArmApiControl kullanma örneği için bkz. [Microsoft. Common. TextBox](microsoft-common-textbox.md).

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
