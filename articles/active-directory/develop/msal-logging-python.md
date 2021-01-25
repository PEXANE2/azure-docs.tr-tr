---
title: Python için MSAL 'te hatalar ve özel durumlar günlüğe kaydediliyor
titleSuffix: Microsoft identity platform
description: Python için MSAL 'te hataların ve özel durumların nasıl günlüğe alınacağını öğrenin
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/25/2021
ms.author: marsma
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: f660bd17954afb4ae02da77e93d4d9cc36f3d355
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98763582"
---
# <a name="logging-in-msal-for-python"></a>Python için MSAL’de oturum açma

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="msal-for-python-logging"></a>Python günlüğü için MSAL

MSAL Python 'da oturum açmak standart Python günlüğü mekanizmasını kullanır, örneğin, `logging.info("msg")` msal günlüğünü aşağıdaki gibi yapılandırabilir ve [username_password_sample](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/1.0.0/sample/username_password_sample.py#L31L32)):

### <a name="enable-debug-logging-for-all-modules"></a>Tüm modüller için hata ayıklama günlüğünü etkinleştir

Varsayılan olarak, herhangi bir Python betiğinde günlüğe kaydetme kapalıdır. Tüm Python betikinizdeki tüm modüller için hata ayıklama günlüğünü etkinleştirmek istiyorsanız şunu kullanın:

```python
logging.basicConfig(level=logging.DEBUG)
```

### <a name="silence-only-msal-logging"></a>Yalnızca sessizlik MSAL günlüğü

Yalnızca MSAL kitaplık günlüğünü sessizlik için, Python betiğinizdeki diğer tüm modüllerde hata ayıklama günlüğünü etkinleştirerek, MSAL Python tarafından kullanılan günlükçü 'yi kapatın:

```Python
logging.getLogger("msal").setLevel(logging.WARN)
```

### <a name="personal-and-organizational-data-in-python"></a>Python 'da kişisel ve Kurumsal veriler

Python için MSAL kişisel verileri veya kurumsal verileri günlüğe eklemez. Kişisel veya kuruluş verilerinin oturum açmasını açma veya kapatma özelliği yoktur.

İstediğiniz her şeyi günlüğe kaydetmek için standart Python günlüğünü kullanabilirsiniz, ancak hassas verileri güvenle ve yasal gereksinimleri izleyerek siz de sorumlusunuz.

Python 'da günlüğe kaydetme hakkında daha fazla bilgi için lütfen Python 'un  [günlüğe kaydetme: nasıl yapılır](https://docs.python.org/3/howto/logging.html#logging-basic-tutorial).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla kod örneği için [Microsoft Identity platform kodu örneklerine](sample-v2-code.md)bakın.

---