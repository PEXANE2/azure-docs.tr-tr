---
title: Görünüm tanımına genel bakış
description: Azure Yönetilen Uygulamalar için görünüm tanımı oluşturma kavramını açıklar.
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 06/12/2019
ms.openlocfilehash: d0c60f5738bf634f9d43d6d4f0d78c1239b7ff3c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75650701"
---
# <a name="view-definition-artifact-in-azure-managed-applications"></a>Azure Yönetilen Uygulamalarda tanım artifakı görüntüleme

Görünüm tanımı, Azure Yönetilen Uygulamalar'da isteğe bağlı bir yapıdır. Genel bakış sayfasını özelleştirmeye ve Ölçümler ve Özel kaynaklar gibi daha fazla görünüm eklemeye olanak tanır.

Bu makalede, görünüm tanımı artifakı ve yeteneklerine genel bir bakış sağlar.

## <a name="view-definition-artifact"></a>Tanım yapıtını görüntüleme

Görünüm tanımı artifakı **viewDefinition.json** olarak adlandırılmalı ve yönetilen bir uygulama tanımı oluşturan .zip paketinde **createUiDefinition.json** ve **mainTemplate.json** ile aynı düzeyde yerleştirilmelidir. .zip paketini nasıl oluşturup yönetilen bir uygulama tanımıyayımla öğrenmek için [bkz.](publish-managed-app-definition-quickstart.md)

## <a name="view-definition-schema"></a>Tanım şea'yı görüntüle

**viewDefinition.json** dosyası, bir dizi `views` görünüme sahip yalnızca bir üst düzey özelliğe sahiptir. Her görünüm, yönetilen uygulama kullanıcı arabiriminde içindekiler tablosunda ayrı bir menü öğesi olarak gösterilir. Her görünüm, `kind` görünümün türünü ayarlayan bir özelliğe sahiptir. Aşağıdaki değerlerden birine ayarlanmalıdır: [Genel Bakış](#overview), [Ölçümler](#metrics), [Özel Kaynaklar](#custom-resources), [Dernekler](#associations). Daha fazla bilgi [için viewDefinition.json için geçerli JSON şemasına](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#)bakın.

Görünüm tanımı için örnek JSON:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#",
    "contentVersion": "0.0.0.1",
    "views": [
        {
            "kind": "Overview",
            "properties": {
                "header": "Welcome to your Azure Managed Application",
                "description": "This managed application is for demo purposes only.",
                "commands": [
                    {
                        "displayName": "Test Action",
                        "path": "testAction"
                    }
                ]
            }
        },
        {
            "kind": "Metrics",
            "properties": {
                "displayName": "This is my metrics view",
                "version": "1.0.0",
                "charts": [
                    {
                        "displayName": "Sample chart",
                        "chartType": "Bar",
                        "metrics": [
                            {
                                "name": "Availability",
                                "aggregationType": "avg",
                                "resourceTagFilter": [ "tag1" ],
                                "resourceType": "Microsoft.Storage/storageAccounts",
                                "namespace": "Microsoft.Storage/storageAccounts"
                            }
                        ]
                    }
                ]
            }
        },
        {
            "kind": "CustomResources",
            "properties": {
                "displayName": "Test custom resource type",
                "version": "1.0.0",
                "resourceType": "testCustomResource",
                "createUIDefinition": { },
                "commands": [
                    {
                        "displayName": "Custom Context Action",
                        "path": "testCustomResource/testContextAction",
                        "icon": "Stop",
                        "createUIDefinition": { }
                    }
                ],
                "columns": [
                    {"key": "name", "displayName": "Name"},
                    {"key": "properties.myProperty1", "displayName": "Property 1"},
                    {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
                ]
            }
        },
        {
            "kind": "Associations",
            "properties": {
                "displayName": "Test association resource type",
                "version": "1.0.0",
                "targetResourceType": "Microsoft.Compute/virtualMachines",
                "createUIDefinition": { }
            }
        }
    ]
}
```

## <a name="overview"></a>Genel Bakış

`"kind": "Overview"`

Bu görünümü **viewDefinition.json'da**sağladığınızda, yönetilen uygulamanızdaki varsayılan Genel Bakış sayfasını geçersiz kılar.

```json
{
    "kind": "Overview",
    "properties": {
        "header": "Welcome to your Azure Managed Application",
        "description": "This managed application is for demo purposes only.",
        "commands": [
            {
                "displayName": "Test Action",
                "path": "testAction"
            }
        ]
    }
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|üst bilgi|Hayır|Genel bakış sayfasının üstbilgi.|
|açıklama|Hayır|Yönetilen uygulamanızın açıklaması.|
|komutlar|Hayır|Genel bakış sayfasının ek araç çubuğu düğmeleri dizisi, [bkz.](#commands)|

![Genel Bakış](./media/view-definition/overview.png)

## <a name="metrics"></a>Ölçümler

`"kind": "Metrics"`

Metrik görünümü, Azure Monitör Ölçümleri'nde yönetilen uygulama [Azure Monitor Metrics](../../azure-monitor/platform/data-platform-metrics.md)kaynaklarınızdan veri toplamanızı ve toplamanızı sağlar.

```json
{
    "kind": "Metrics",
    "properties": {
        "displayName": "This is my metrics view",
        "version": "1.0.0",
        "charts": [
            {
                "displayName": "Sample chart",
                "chartType": "Bar",
                "metrics": [
                    {
                        "name": "Availability",
                        "aggregationType": "avg",
                        "resourceTagFilter": [ "tag1" ],
                        "resourceType": "Microsoft.Storage/storageAccounts",
                        "namespace": "Microsoft.Storage/storageAccounts"
                    }
                ]
            }
        ]
    }
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Hayır|Görünümün görüntülenen başlığı.|
|version|Hayır|Görünümü işlemek için kullanılan platformun sürümü.|
|Grafik|Evet|Ölçümler sayfasının grafik dizisi.|

### <a name="chart"></a>Grafik

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Evet|Grafiğin görüntülenen başlığı.|
|chartType|Hayır|Bu grafik için kullanılacak görselleştirme. Varsayılan olarak, bir çizgi grafiği kullanır. Desteklenen grafik türleri: `Bar, Line, Area, Scatter`.|
|metrics|Evet|Bu grafikte çizilen ölçümler dizisi. Azure portalında desteklenen ölçümler hakkında daha fazla bilgi edinmek için Azure [Monitor ile Desteklenen ölçümlere](../../azure-monitor/platform/metrics-supported.md) bakın|

### <a name="metric"></a>Ölçüm

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|ad|Evet|Metnin adı.|
|Aggregationtype|Evet|Bu metrik için kullanılacak toplama türü. Desteklenen toplama türleri:`none, sum, min, max, avg, unique, percentile, count`|
|ad alanı|Hayır|Doğru ölçüm sağlayıcısını belirlerken kullanılacak ek bilgiler.|
|kaynakTagFilter|Hayır|Kaynak etiketleri dizisi (word `or` ile ayrılacak) hangi ölçümleriçin görüntülenir. Kaynak türü filtresinin üstüne uygulanır.|
|resourceType|Evet|Ölçümlerin görüntüleneceği kaynak türü.|

![Ölçümler](./media/view-definition/metrics.png)

## <a name="custom-resources"></a>Özel kaynaklar

`"kind": "CustomResources"`

Bu tür birden çok görünüm tanımlayabilirsiniz. Her **görünüm, mainTemplate.json'da**tanımladığınız özel sağlayıcıdan **benzersiz** bir özel kaynak türünü temsil eder. Özel sağlayıcılara giriş için Azure [Özel Sağlayıcıları Önizleme genel görünümüne](../custom-providers/overview.md)bakın.

Bu görünümde, özel kaynak türünüz için GET, PUT, DELETE ve POST işlemlerini gerçekleştirebilirsiniz. POST işlemleri, özel kaynak türünüz bağlamında genel özel eylemler veya özel eylemler olabilir.

```json
{
    "kind": "CustomResources",
    "properties": {
        "displayName": "Test custom resource type",
        "version": "1.0.0",
        "resourceType": "testCustomResource",
        "icon": "Polychromatic.ResourceList",
        "createUIDefinition": { },
        "commands": [
            {
                "displayName": "Custom Context Action",
                "path": "testCustomResource/testContextAction",
                "icon": "Stop",
                "createUIDefinition": { },
            }
        ],
        "columns": [
            {"key": "name", "displayName": "Name"},
            {"key": "properties.myProperty1", "displayName": "Property 1"},
            {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
        ]
    }
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Evet|Görünümün görüntülenen başlığı. Başlık **viewDefinition.json**her CustomResources görünümü için **benzersiz** olmalıdır.|
|version|Hayır|Görünümü işlemek için kullanılan platformun sürümü.|
|resourceType|Evet|Özel kaynak türü. Özel sağlayıcınızın **benzersiz** bir özel kaynak türü olmalıdır.|
|Simge|Hayır|Görünümün simgesi. Örnek simgelerin listesi [JSON Schema](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#)tanımlanır.|
|createUIDefinition|Hayır|Özel kaynak komutu oluşturmak için Kullanıcı Arabirimi Tanımı şeması oluşturun. UI tanımlarını oluşturmaya giriş için [createUiDefinition ile başlarken](create-uidefinition-overview.md)|
|komutlar|Hayır|CustomResources görünümünün ek araç çubuğu düğmeleri dizisi, [bkz.](#commands)|
|sütunlar|Hayır|Özel kaynağın sütun dizisi. Tanımlanmamışsa `name` sütun varsayılan olarak gösterilir. Sütun ve `"key"` `"displayName"`. Anahtar için, özelliğin anahtarını bir görünümde görüntülemek için sağlayın. İç içe yse, noktayı sınırlayıcı olarak `"key": "name"` kullanın, örneğin, veya `"key": "properties.property1"`. Görüntü adı için, görünümde görüntülenecek özelliğin görüntü adını sağlayın. Ayrıca bir `"optional"` özellik de sağlayabilirsiniz. Doğru olarak ayarlandığında, sütun varsayılan olarak bir görünümde gizlenir.|

![Özel Kaynaklar](./media/view-definition/customresources.png)

## <a name="commands"></a>Komutlar

Komutlar, sayfada görüntülenen bir dizi ek araç çubuğu düğmesidir. Her **komut, mainTemplate.json'da**tanımlanan Azure Özel Sağlayıcınızdan gelen bir POST eylemini temsil eder. Özel sağlayıcılara giriş için Azure [Özel Sağlayıcılarına genel bakış'a](../custom-providers/overview.md)bakın.

```json
{
    "commands": [
        {
            "displayName": "Start Test Action",
            "path": "testAction",
            "icon": "Start",
            "createUIDefinition": { }
        },
    ]
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Evet|Komut düğmesinin görüntülenen adı.|
|yol|Evet|Özel sağlayıcı eylem adı. Eylem **mainTemplate.json'da**tanımlanmalıdır.|
|Simge|Hayır|Komut düğmesinin simgesi. Örnek simgelerin listesi [JSON Schema](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#)tanımlanır.|
|createUIDefinition|Hayır|Komut için UI Tanımı şeması oluşturun. UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.|

## <a name="associations"></a>Dernek

`"kind": "Associations"`

Bu tür birden çok görünüm tanımlayabilirsiniz. Bu görünüm, **mainTemplate.json'da**tanımladığınız özel sağlayıcı aracılığıyla varolan kaynakları yönetilen uygulamaya bağlamanızı sağlar. Özel sağlayıcılara giriş için Azure [Özel Sağlayıcıları Önizleme genel görünümüne](../custom-providers/overview.md)bakın.

Bu görünümde, varolan Azure kaynaklarını `targetResourceType`. Bir kaynak seçildiğinde, **genel** özel sağlayıcıya bir onboarding isteği oluşturur ve bu istek kaynağa bir yan etki uygulayabilir. 

```json
{
    "kind": "Associations",
    "properties": {
        "displayName": "Test association resource type",
        "version": "1.0.0",
        "targetResourceType": "Microsoft.Compute/virtualMachines",
        "createUIDefinition": { }
    }
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Evet|Görünümün görüntülenen başlığı. Başlık **viewDefinition.json**her Dernekler görünümü için **benzersiz** olmalıdır.|
|version|Hayır|Görünümü işlemek için kullanılan platformun sürümü.|
|targetResourceType|Evet|Hedef kaynak türü. Bu, kaynak onboarding için görüntülenecek kaynak türüdür.|
|createUIDefinition|Hayır|İlişkilendirme kaynak komutu oluşturmak için UI Tanımı şeması oluşturun. UI tanımlarını oluşturmaya giriş için [createUiDefinition ile başlarken](create-uidefinition-overview.md)|

## <a name="looking-for-help"></a>Yardım arıyorum

Azure Yönetilen Uygulamalar hakkında sorularınız varsa, [Yığın Taşma'da](https://stackoverflow.com/questions/tagged/azure-managedapps)soru sormayı deneyin. Benzer bir soru zaten sorulmuş ve yanıtlanmış olabilir, bu nedenle göndermeden önce ilk kontrol edin. Hızlı yanıt `azure-managedapps` almak için etiketi ekleyin!

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](overview.md) konusunu inceleyin.
- Özel sağlayıcılara giriş için Azure [Özel Sağlayıcılarına genel bakış'a](../custom-providers/overview.md)bakın.
- Azure Özel Sağlayıcıları yla Azure Yönetilen Uygulama oluşturmak için [Bkz. Öğretici: Özel sağlayıcı eylemleri ve kaynak türleri ile yönetilen uygulama oluşturma](tutorial-create-managed-app-with-custom-provider.md)
