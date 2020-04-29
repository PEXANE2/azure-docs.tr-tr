---
title: Özel rol tanımlarına genel bakış
description: Yönetilen uygulamalar için özel rol tanımları oluşturma kavramını açıklar.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 09/16/2019
ms.openlocfilehash: 7b7fff913c177703f959bfa103c8e310d01059e2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81391837"
---
# <a name="custom-role-definition-artifact-in-azure-managed-applications"></a>Azure yönetilen uygulamalarında özel rol tanımı yapıtı

Özel rol tanımı yönetilen uygulamalarda isteğe bağlı bir yapıdır. Yönetilen uygulamanın işlevlerini gerçekleştirmesi için gereken izinleri belirlemek için kullanılır.

Bu makalede özel rol tanımı yapıtı ve özelliklerine genel bir bakış sunulmaktadır.

## <a name="custom-role-definition-artifact"></a>Özel rol tanımı yapıtı

CustomRoleDefinition. JSON özel rol tanımı yapıtı adını yazmanız gerekir. Yönetilen bir uygulama tanımı oluşturan. zip paketindeki Createuıdefinition. JSON ve mainTemplate. JSON ile aynı düzeyde yer koyun. . Zip paketini oluşturma ve yönetilen uygulama tanımını yayımlama hakkında bilgi edinmek için bkz [. yönetilen uygulama tanımını yayımlama.](publish-service-catalog-app.md)

## <a name="custom-role-definition-schema"></a>Özel rol tanımı şeması

CustomRoleDefinition. JSON dosyası, rol dizisi olan bir en `roles` üst düzey özelliğe sahiptir. Bu roller, yönetilen uygulamanın çalışması için gereken izinlerdir. Şu anda yalnızca yerleşik rollere izin veriliyor, ancak birden çok rol belirtebilirsiniz. Role rol tanımının KIMLIĞI veya rol adı tarafından başvurulabilir.

Özel rol tanımı için örnek JSON:

```json
{
    "contentVersion": "0.0.0.1",
    "roles": [
        {
            "properties": {
                "roleName": "Contributor"
            }
        },
        {
            "id": "acdd72a7-3385-48ef-bd42-f606fba81ae7"
        },
        {
            "id": "/providers/Microsoft.Authorization/roledefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
        }
    ]
}
```

## <a name="roles"></a>Roller

Bir rol ya bir `$.properties.roleName` veya ' den oluşur: `id`

```json
{
    "id": null,
    "properties": {
        "roleName": "Contributor"
    }
}
```

> [!NOTE]
> `id` Ya `roleName` da alanını kullanabilirsiniz. Yalnızca bir tane gereklidir. Bu alanlar, uygulanması gereken rol tanımını aramak için kullanılır. Her ikisi de sağlanırsa, `id` alanı kullanılacaktır.

|Özellik|Gerekli mi?|Açıklama|
|---------|---------|---------|
|id|Yes|Yerleşik rolün KIMLIĞI. Tam KIMLIĞI veya yalnızca GUID 'YI kullanabilirsiniz.|
|roleName|Yes|Yerleşik rolün adı.|