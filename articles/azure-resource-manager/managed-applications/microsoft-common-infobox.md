---
title: InfoBox UI öğesi
description: Azure portalı için Microsoft.Common.InfoBox UI öğesini açıklar. Yönetilen uygulamayı dağıtırken metin veya uyarı eklemek için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: tomfitz
ms.openlocfilehash: 6d1e4a84904ef7022d9ce85803bf10285bf0b8ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75652482"
---
# <a name="microsoftcommoninfobox-ui-element"></a>Microsoft.Common.InfoBox UI öğesi

Bilgi kutusu ekleyen bir denetim. Kutu, kullanıcıların sağladıkları değerleri anlamalarına yardımcı olan önemli metin veya uyarılar içerir. Ayrıca daha fazla bilgi için bir URI bağlantı olabilir.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.InfoBox](./media/managed-application-elements/microsoft.common.infobox.png)


## <a name="schema"></a>Şema

```json
{
  "name": "text1",
  "type": "Microsoft.Common.InfoBox",
  "visible": true,
  "options": {
    "icon": "None",
    "text": "Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo.",
    "uri": "https://www.microsoft.com"
  }
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
"Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo."
```

## <a name="remarks"></a>Açıklamalar

* Için `icon`, **Yok**, **Bilgi**, **Uyarı**, veya **Hata**kullanın .
* Özellik `uri` isteğe bağlıdır.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
