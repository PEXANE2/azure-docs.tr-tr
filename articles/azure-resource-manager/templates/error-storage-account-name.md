---
title: Depolama hesabı adı hataları
description: Bir depolama hesabı adı belirtirken karşılaşabileceğiniz hataları açıklar.
ms.topic: troubleshooting
ms.date: 03/09/2018
ms.openlocfilehash: 5b2706d8540ea38ef08bf7ca0f804e6811a93085
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76153981"
---
# <a name="resolve-errors-for-storage-account-names"></a>Depolama hesabı adları için hataları çözme

Bu makalede, bir depolama hesabı dağıtırken karşılaşabileceğiniz adlandırma hataları açıklanmaktadır.

## <a name="symptom"></a>Belirti

Depolama hesabı adınız yasaklı karakterler içeriyorsa, şu gibi bir hata alırsınız:

```
Code=AccountNameInvalid
Message=S!torageckrexph7isnoc is not a valid storage account name. Storage account name must be
between 3 and 24 characters in length and use numbers and lower-case letters only.
```

Depolama hesapları için, Azure'da benzersiz olan kaynak için bir ad sağlamanız gerekir. Benzersiz bir ad sağlamazsanız şöyle bir hata alırsınız:

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

Aboneliğinizde varolan bir depolama hesabıyla aynı ada sahip bir depolama hesabı dağıtıyor, ancak farklı bir konum sağlıyorsanız, depolama hesabının zaten farklı bir konumda olduğunu belirten bir hata alırsınız. Varolan depolama hesabını silin veya varolan depolama hesabıyla aynı konumu sağlayın.

## <a name="cause"></a>Nedeni

Depolama hesabı adları uzunluk ve kullanım numaraları ve küçük harfler sadece 3 ve 24 karakter arasında olmalıdır. İsim benzersiz olmalı.

## <a name="solution"></a>Çözüm

Depolama hesabı adının benzersiz olduğundan emin olun. ÖzgünString işlevinin sonucuyla adlandırma kuralınızı bir araya getirerek [uniqueString](template-functions-string.md#uniquestring) benzersiz bir ad oluşturabilirsiniz.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Depolama hesabı adınızın 24 karakteri geçmediğinden emin olun. [uniqueString](template-functions-string.md#uniquestring) işlevi 13 karakter döndürür. **BenzersizString** sonucuna bir önek veya postfix eklerseniz, 11 karakter veya daha az bir değer sağlayın.

```json
"parameters": {
  "storageNamePrefix": {
    "type": "string",
    "maxLength": 11,
    "defaultValue": "storage",
    "metadata": {
    "description": "The value to use for starting the storage account name."
    }
  }
}
```

Depolama hesabı adınızın büyük harf veya özel karakter içermediğinden emin olun.