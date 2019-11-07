---
title: Azure yönetilen uygulamalarında özel rol tanımlarına genel bakış | Microsoft Docs
description: Yönetilen uygulamalar için özel rol tanımları oluşturma kavramını açıklar.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 09/16/2019
ms.openlocfilehash: d645eebefde473e404f7760d2bc8a67c7e3e9087
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73609040"
---
# <a name="custom-role-definition-artifact-in-azure-managed-applications"></a>Azure yönetilen uygulamalarında özel rol tanımı yapıtı

Özel rol tanımı yönetilen uygulamalarda isteğe bağlı bir yapıdır. Yönetilen uygulamanın işlevlerini gerçekleştirmesi için gereken izinleri belirlemek için kullanılır.

Bu makalede özel rol tanımı yapıtı ve özelliklerine genel bir bakış sunulmaktadır.

## <a name="custom-role-definition-artifact"></a>Özel rol tanımı yapıtı

Özel rol tanımı yapıtı, **Customroledefinition. JSON** olarak adlandırılmalıdır ve yönetilen uygulama tanımı oluşturan. zip paketindeki **createuıdefinition. JSON** ve **maintemplate. JSON** ile aynı düzeye yerleştirilmelidir. . Zip paketini oluşturma ve yönetilen uygulama tanımını yayımlama hakkında bilgi edinmek için bkz [. yönetilen uygulama tanımını yayımlama.](publish-managed-app-definition-quickstart.md)

## <a name="custom-role-definition-schema"></a>Özel rol tanımı şeması

**Customroledefinition. JSON** dosyası bir rol dizisi olan bir üst düzey `roles` özelliğine sahiptir. Bu rollerin her biri, yönetilen uygulamanın çalışması için gereken izinlerdir. Şu anda yalnızca yerleşik rollere izin veriliyor, ancak birden çok rol belirtilebilir. Role rol tanımının KIMLIĞI veya rol adı tarafından başvurulabilir.

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

## <a name="role"></a>Rol

Rol, bir `$.properties.roleName` veya `id`oluşur.

```json
{
    "id": null,
    "properties": {
        "roleName": "Contributor"
    }
}
```

> [!Note]
> `id` ya da `roleName` alanlarından yalnızca biri gereklidir. Bu alanlar, uygulanacak rol tanımını aramak için kullanılır. Her ikisi de sağlanırsa `id` alanı kullanılacaktır.

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|Kimlik|*Yes*|Yerleşik rolün KIMLIĞI. Bu özellik tam KIMLIK veya yalnızca GUID olabilir.|
|RoleName|*Yes*|Yerleşik rolün adı.|