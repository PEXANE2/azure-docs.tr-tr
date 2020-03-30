---
title: Dağıtım sırası sırasını anlama
description: Plan ataması sırasında plan yapılarının dağıtılan varsayılan sırası ve dağıtım sırasını nasıl özelleştirişleri hakkında bilgi edinin.
ms.date: 08/22/2019
ms.topic: conceptual
ms.openlocfilehash: 51026862c989f15acf6d3e21702cfcfc8b2b27b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74128828"
---
# <a name="understand-the-deployment-sequence-in-azure-blueprints"></a>Azure Planları'ndaki dağıtım sırasını anlama

Azure Blueprints, plan tanımının atamasını işlerken kaynak oluşturma sırasını belirlemek için bir **sıralama sırası** kullanır. Bu makalede aşağıdaki kavramlar açıklanmaktadır:

- Kullanılan varsayılan sıralama sırası
- Siparişi özelleştirme
- Özelleştirilmiş sipariş nasıl işlenir?

JSON örneklerinde kendi değerlerinizle değiştirmeniz gereken değişkenler vardır:

- `{YourMG}` - Yönetim grubunuzun adıyla değiştirin

## <a name="default-sequencing-order"></a>Varsayılan sıralama sırası

Plan tanımı, yapıları dağıtma sırası için yönerge içermezse veya yönerge null ise, aşağıdaki sipariş kullanılır:

- Yapı adına göre sıralanmış abonelik düzeyi **rol atama** yapıları
- Yapı adına göre sıralanmış abonelik düzeyi **ilkesi atama** yapıları
- Yapı adına göre sıralanmış abonelik düzeyi **Azure Kaynak Yöneticisi şablonu** yapıları
- Yer tutucu adına göre sıralanmış **kaynak grubu** yapıları (alt yapılar dahil)

Her **kaynak grubu** yapı içinde, bu kaynak grubu içinde oluşturulacak yapılar için aşağıdaki sıra sırası kullanılır:

- Yapı adına göre sıralanmış kaynak grubu alt **rol atama** yapıları
- Yapı adına göre sıralanmış kaynak grubu alt **ilke atama** yapıları
- Kaynak grubu alt **Azure Kaynak Yöneticisi şablonu** yapı adına göre sıralanmış yapılar

> [!NOTE]
> [Yapıtların kullanımı()](../reference/blueprint-functions.md#artifacts) atıfta bulunulan yapıya üstü kapalı bir bağımlılık oluşturur.

## <a name="customizing-the-sequencing-order"></a>Sıralama sırasını özelleştirme

Büyük plan tanımları oluştururken, kaynakların belirli bir sırada oluşturulması gerekebilir. Bu senaryonun en yaygın kullanım deseni, bir plan tanımının birkaç Azure Kaynak Yöneticisi şablonu içerdiği dir. Planlar, sıralama sırasının tanımlanmasına izin vererek bu deseni işler.

Sıralama JSON bir `dependsOn` özellik tanımlayarak gerçekleştirilir. Kaynak grupları ve yapı nesneleri için plan tanımı bu özelliği destekler. `dependsOn`belirli bir yapının oluşturulmadan önce oluşturulması gereken yapı adlarının bir dizidir.

> [!NOTE]
> Plan nesneleri oluştururken, her yapı kaynağı, [PowerShell](/powershell/module/az.blueprint/new-azblueprintartifact)veya URL bitiş noktası kullanıyorsa, [REST API](/rest/api/blueprints/artifacts/createorupdate)kullanıyorsa, dosya adından adını alır.
> _kaynakGrup_ başvurularının planlar da tanımlananlarla eşleşmesi gerekir.

### <a name="example---ordered-resource-group"></a>Örnek - sıralı kaynak grubu

Bu örnek plan tanımı, standart bir kaynak grubuyla birlikte bir değer `dependsOn`beyan ederek özel sıralama sırası tanımlayan bir kaynak grubu içerir. Bu durumda, atanan **PolitikaEtiketler** adlı **yapı, sipariş edilen rg** kaynak grubundan önce işlenecektir.
**standart-rg** varsayılan sıralama sırasına göre işlenecektir.

```json
{
    "properties": {
        "description": "Example blueprint with custom sequencing order",
        "resourceGroups": {
            "ordered-rg": {
                "dependsOn": [
                    "assignPolicyTags"
                ],
                "metadata": {
                    "description": "Resource Group that waits for 'assignPolicyTags' creation"
                }
            },
            "standard-rg": {
                "metadata": {
                    "description": "Resource Group that follows the standard sequence ordering"
                }
            }
        },
        "targetScope": "subscription"
    },
    "type": "Microsoft.Blueprint/blueprints"
}
```

### <a name="example---artifact-with-custom-order"></a>Örnek - özel sırayla yapı

Bu örnek, Azure Kaynak Yöneticisi şablonuna bağlı bir ilke yapıdır. Varsayılan sıralama da, Azure Kaynak Yöneticisi şablonundan önce bir ilke yapısı oluşturulur. Bu sıralama, ilke yapının Azure Kaynak Yöneticisi şablonu oluşturulmasını beklemesini sağlar.

```json
{
    "properties": {
        "displayName": "Assigns an identifying tag",
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
        "resourceGroup": "standard-rg",
        "dependsOn": [
            "customTemplate"
        ]
    },
    "kind": "policyAssignment",
    "type": "Microsoft.Blueprint/artifacts"
}
```

### <a name="example---subscription-level-template-artifact-depending-on-a-resource-group"></a>Örnek - kaynak grubuna bağlı olarak abonelik düzeyi şablonu yapı

Bu örnek, bir kaynak grubuna bağlı kalmak üzere abonelik düzeyinde dağıtılan bir Kaynak Yöneticisi şablonu içindir. Varsayılan sıralamada, abonelik düzeyi yapıları bu kaynak gruplarındaki kaynak gruplarından ve alt yapılarından önce oluşturulur. Kaynak grubu aşağıdaki gibi plan tanımında tanımlanır:

```json
"resourceGroups": {
    "wait-for-me": {
        "metadata": {
            "description": "Resource Group that is deployed prior to the subscription level template artifact"
        }
    }
}
```

**Beni bekle** kaynak grubuna bağlı olarak abonelik düzeyi şablonu yapı alanı aşağıdaki gibi tanımlanır:

```json
{
    "properties": {
        "template": {
            ...
        },
        "parameters": {
            ...
        },
        "dependsOn": ["wait-for-me"],
        "displayName": "SubLevelTemplate",
        "description": ""
    },
    "kind": "template",
    "type": "Microsoft.Blueprint/blueprints/artifacts"
}
```

## <a name="processing-the-customized-sequence"></a>Özelleştirilmiş sırayı işleme

Oluşturma işlemi sırasında, bir topolojik sıralama planları eserler bağımlılık grafiği oluşturmak için kullanılır. Denetim, kaynak grupları ve yapılar arasındaki her bağımlılık düzeyinin desteklenmesini sağlar.

Varsayılan sırayı değiştirmeyecek bir yapı bağımlılığı bildirilirse, hiçbir değişiklik yapılmaz. Örnek, abonelik düzeyi ilkesine bağlı bir kaynak grubudur. Başka bir örnek, kaynak grubu 'standart-rg' alt rol atamaya bağlı bir kaynak grubu 'standart-rg' alt ilke atamasidır. Her iki durumda `dependsOn` da, varsayılan sıralama sırasını değiştirmez ve hiçbir değişiklik yapılmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Plan yaşam döngüsü](lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](parameters.md) kullanımını anlayın.
- [Şema kaynak kilitleme](resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../how-to/update-existing-assignments.md)öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.