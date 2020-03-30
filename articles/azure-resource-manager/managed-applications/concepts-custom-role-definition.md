---
title: Özel rol tanımlarına genel bakış
description: Yönetilen uygulamalar için özel rol tanımları oluşturma kavramını açıklar.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 09/16/2019
ms.openlocfilehash: 88e42fd9626276f6c77b46b33c138407f91d06ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75650766"
---
# <a name="custom-role-definition-artifact-in-azure-managed-applications"></a>Azure Yönetilen Uygulamalarda özel rol tanımı yapı

Özel rol tanımı, yönetilen uygulamalarda isteğe bağlı bir yapıdır. Yönetilen uygulamanın işlevlerini gerçekleştirmek için hangi izinlere ihtiyaç olduğunu belirlemek için kullanılır.

Bu makalede, özel rol tanımı artifakı ve yeteneklerine genel bir bakış sağlar.

## <a name="custom-role-definition-artifact"></a>Özel rol tanımı artifakı

Özel rol tanımı artifakı özelRoleDefinition.json adlandırmanız gerekir. Yönetilen bir uygulama tanımı oluşturan .zip paketinde createUiDefinition.json ve mainTemplate.json ile aynı düzeyde yerleştirin. .zip paketini nasıl oluşturup yönetilen bir uygulama tanımıyayımla öğrenmek için [bkz.](publish-managed-app-definition-quickstart.md)

## <a name="custom-role-definition-schema"></a>Özel rol tanımı şeması

customRoleDefinition.json dosyası, bir dizi `roles` rol içeren üst düzey bir özelliğe sahiptir. Bu roller, yönetilen uygulamanın çalışması için gereken izinlerdir. Şu anda yalnızca yerleşik rollere izin verilir, ancak birden çok rol belirtebilirsiniz. Bir rol, rol tanımının kimliğiyle veya rol adı ile başvurulabilir.

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

Bir rol a `$.properties.roleName` veya bir `id`oluşur:

```json
{
    "id": null,
    "properties": {
        "roleName": "Contributor"
    }
}
```

> [!NOTE]
> Alanı `id` veya alanı `roleName` kullanabilirsiniz. Sadece bir tane gerekiyor. Bu alanlar, uygulanması gereken rol tanımını aramak için kullanılır. Her ikisi de sağlanırsa, `id` alan kullanılır.

|Özellik|Gerekli mi?|Açıklama|
|---------|---------|---------|
|id|Evet|Yerleşik rolün kimliği. Tam kimliği veya sadece GUID'i kullanabilirsiniz.|
|roleName|Evet|Yerleşik rolün adı.|