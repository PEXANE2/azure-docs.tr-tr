---
title: Azure bilgi kutusu kullanıcı arabirimi öğesi | Microsoft Docs
description: Azure portal için Microsoft. Common. Infobox Kullanıcı arabirimi öğesini açıklar. Yönetilen uygulamayı dağıttığınızda metin veya uyarı eklemek için kullanın.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2018
ms.author: tomfitz
ms.openlocfilehash: 67ae05c2ec7b36d76e49f26d5765bbc68b952292
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72331681"
---
# <a name="microsoftcommoninfobox-ui-element"></a>Microsoft. Common. Infobox Kullanıcı arabirimi öğesi
Bilgi kutusu ekleyen bir denetim. Bu kutu, kullanıcıların sağladıkları değerleri anlamasına yardımcı olan önemli metin veya uyarılar içerir. Daha fazla bilgi için bir URI 'ye de bağlanabilir.

## <a name="ui-sample"></a>UI örneği
![Microsoft. Common. Infobox](./media/managed-application-elements/microsoft.common.infobox.png)


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

## <a name="remarks"></a>Açıklamalar

* @No__t-0 için **none**, **Info**, **Warning**veya **Error**kullanın.
* @No__t-0 özelliği isteğe bağlıdır.

## <a name="sample-output"></a>Örnek çıktı

```json
"Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo."
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
