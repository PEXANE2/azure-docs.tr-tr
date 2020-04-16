---
title: Ortak parametreler ve üst bilgiler
description: Anahtar Kasa kaynaklarıyla ilgili yapabileceğiniz tüm işlemlerde ortak parametreler ve üstbilgiler.
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
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430883"
---
# <a name="common-parameters-and-headers"></a>Ortak parametreler ve üst bilgiler

Aşağıdaki bilgiler, Key Vault kaynaklarıyla ilgili yapabileceğiniz tüm işlemlerde yaygındır:

- URI'deki api sürümüyle değiştirin. `{api-version}`
- URI'deki abonelik tanımlayıcınızla değiştirin `{subscription-id}`
- Kaynak `{resource-group-name}` grubuyla değiştirin. Daha fazla bilgi için Azure kaynaklarınızı yönetmek için Kaynak gruplarını kullanma'ya bakın.
- URI'deki anahtar kasa adınız ile değiştirin. `{vault-name}`
- İçerik Türü üstbilgisini uygulama/json olarak ayarlayın.
- Yetkilendirme üstbilgisini Azure Etkin Dizini'nden (AAD) aldığınız bir JSON Web Belirteci olarak ayarlayın. Daha fazla bilgi için Azure [Kaynak Yöneticisi isteklerini doğrulama'ya](authentication-requests-and-responses.md) bakın.

## <a name="common-error-response"></a>Yaygın hata yanıtı
Hizmet, başarı veya başarısızlığı belirtmek için HTTP durum kodlarını kullanır. Ayrıca, hatalar aşağıdaki biçimde bir yanıt içerir:

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
| message | string | Hataya neyin neden olduğunu açıklaması. |



## <a name="see-also"></a>Ayrıca Bkz.
 [Azure Key Vault REST API Başvurusu](/rest/api/keyvault/)
