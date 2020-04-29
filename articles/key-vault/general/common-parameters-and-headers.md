---
title: Ortak parametreler ve üst bilgiler
description: Key Vault kaynaklarıyla ilgili yapabileceğiniz tüm işlemlerde ortak olan parametreler ve üstbilgiler.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: d0ada9c1e6b45b1be17b15b67f67fc64fc266203
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81430883"
---
# <a name="common-parameters-and-headers"></a>Ortak parametreler ve üst bilgiler

Aşağıdaki bilgiler Key Vault kaynaklarıyla ilgili yapabileceğiniz tüm işlemler için ortaktır:

- URI `{api-version}` 'deki api sürümü ile değiştirin.
- URI `{subscription-id}` 'deki abonelik tanımlayıcın ile değiştirin
- Kaynak `{resource-group-name}` grubuyla değiştirin. Daha fazla bilgi için bkz. Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma.
- URI `{vault-name}` 'deki Anahtar Kasası adınızla değiştirin.
- Content-Type üst bilgisini Application/JSON olarak ayarlayın.
- Yetkilendirme üst bilgisini Azure Active Directory (AAD) tarafından aldığınız JSON Web Token ayarlayın. Daha fazla bilgi için bkz. [kimlik doğrulama Azure Resource Manager](authentication-requests-and-responses.md) istekleri.

## <a name="common-error-response"></a>Ortak hata yanıtı
Hizmet, başarılı veya başarısız olduğunu göstermek için HTTP durum kodlarını kullanacaktır. Ayrıca, başarısızlıklar aşağıdaki biçimde bir yanıt içerir:

```
   {  
     "error": {  
     "code": "BadRequest",  
     "message": "The key vault sku is invalid."  
     }  
   }  
```

|Öğe adı | Tür | Açıklama |
|---|---|---|
| kod | string | Oluşan hata türü.|
| message | string | Hatanın neden olduğuna ilişkin bir açıklama. |



## <a name="see-also"></a>Ayrıca Bkz.
 [Azure Key Vault REST API başvurusu](/rest/api/keyvault/)
